---
layout: single
title: "Version Control: the Git Data Model"
date: 2022-02-11
category: notes
tags:
  - Version Control
  - VCS
  - Git
  - Data Model
excerpt:
tagline:
header:
  teaser: "/assets/images/Git-Logo-White.png"
  overlay_image: "/assets/images/Git-Logo-White.png"
  overlay_filter: 0.5
---

*[VCS]: Version Control System

Version control systems are tools used to track changes to files and folders under their purview.
A VCS will maintain a history of changes, and facilitate collaboration by tracking metadata like who created each snapshot of changes and why.

Version control is useful because it lets you examine old snapshots of a project, keep a log of why certain changes were made and when, and work on parallel branches of development.
It also helps to clarify what other people have changed, as well as providing a means to resolve conflicts in concurrent development.

There are a number of unique VCSs, but Git has emerged as the de facto standard.

# The Git Data Model

From the [Git Book](https://git-scm.com/book/en/v2/Git-Internals-Plumbing-and-Porcelain):
> "Git is fundamentally a content-addressable filesystem with a VCS user interface written on top of it."

Yikes!

Git's interface is a [leaky abstraction](https://en.wikipedia.org/wiki/Leaky_abstraction),
and learning a handful of commands to use like magic incantations can lead to confusion really quickly.
Indeed, Git has a reputation in the industry, captured by this [XKCD comic](https://xkcd.com/1597/).

![xkcd 1597](https://imgs.xkcd.com/comics/git.png "If that doesn't fix it, git.txt contains the phone number of a friend of mine who understands git. Just wait through a few minutes of 'It's really pretty simple, just think of branches as...' and eventually you'll learn the commands that will fix everything."){: .align-center}

It's true that Git's interface is a bit ugly, especially to a new user that may not be accustomed to CLI tools.
But the underlying design and principles of Git are beautiful.
An ugly interface has to be memorized, but a beautiful design can be understood.

## Snapshots

Git models the history of a collection of files and folders as a series of snapshots.
In Git parlance, a file is called a *blob*.

Try this out! <br>
You can have Git tell you the object type of any object in Git, given its SHA-1 key. <br>
`git cat-file -t [SHA-1 key]`
{: .notice}

Directories are called *trees*, and they map names to blobs or trees, since a directory can house both files and directories.

A snapshot is the state of the top-level tree that is being version controlled.
An example tree might look like:

```plaintext
<root> (tree)
|
+- dir (tree)
|  |
|  +- file1.txt (blob, contents := 'Hello, World!')
|
+- file2.txt (blob, contents := 'Git is neat!')
```

## Modeling a History: Relating Snapshots

Rather than implement a linear history of snapshots, Git uses a Directed Acyclic Graph (DAG).
Each snapshot in Git refers to a set of 'parents', the snapshots that preceded it.
A set of parents is used instead of a single parent (as it would be in a linear history) because a snapshot may descend from multiple parents, as would be the case when merging branches of parallel development.

Git calls these snapshots *commits*. Visualizing a commit history could look something like this:

```plaintext
o <-- o <-- o <-- o
            ^
             \
              --- o <-- o
```

Each `o` corresponds to individual commits in the history.
The arrows point back to parent commits (Git uses a **comes before** relation, not a **comes after** relation).
Note that after the third commit, there are two independent branches of history progressing in parallel.
In a future commit, these branches could be merged into a commit that incorporates both histories.
That would look something like this:

```plaintext
o <-- o <-- o <-- o <---- o
            ^            /
             \          v
              --- o <-- o
```

Git commits are immutable.
A history of commits can still be edited, but those edits take the form of new commits that replace the old ones.

## The Git Model in Pseudocode

Let's lay out the Git model, as we've discussed it so far, as pseudocode.

```plaintext
// a blob (file) is a bunch of bytes
type blob := array<byte>

// a tree (directory) contains named trees and blobs
type tree := map<string ~> tree | blob>

// a commit has parents, metadata, and the toplevel tree
type commit := struct {
  parents:  array<commit>
  author:   string
  message:  string
  snapshot: tree
}
```

Laid out like this, the Git model is a simple and straightforward VCS.
We'll refer back to this as we expand our understanding of Git in the next sections.

## Objects and Content Addressing

An *Object* can be a blob, a tree, or a commit.

```plaintext
type object := blob | tree | commit
```

In a Git data store, objects are content-addressed by their [SHA-1](https://en.wikipedia.org/wiki/SHA-1) hash.

```plaintext
objects := map<string ~> object>

fn store(object):
  id = SHA1(object)
  objects[id] = object

fn load(id):
  return objects[id]
```

So blobs, trees, and commits are all objects.
When they reference other objects, they don't need to contain the referenced object itself.
Rather, they can use the data store to refer to another object by it's SHA-1 hash.

## References

