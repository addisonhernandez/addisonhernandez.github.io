---
layout: single
title: "Git Incantations"
date: 2022-02-21
category: notes
tags:
  - Version Control
  - VCS
  - Git
  - Data Model
  - DAG
  - CLI
tagline: "Powerful magicks to improve your workflow and impress your peers"
excerpt: "Git recipes for success"
header:
  teaser: "/assets/images/yancy-min-842ofHC6MaI-unsplash-small.jpg"
  overlay_image: "/assets/images/yancy-min-842ofHC6MaI-unsplash.jpg"
  overlay_filter: linear-gradient(90deg, rgba(0, 0, 0, 0.75) 33%, rgba(0, 0, 0, 0.5))
---

*[DAG]: Directed Acyclic Graph

In my [last post](/notes/2022/02/11/Git_and_Github.html),
we took a look at the data model underpinning Git.
Understanding that all of Git's [many commands](https://ndpsoftware.com/git-cheatsheet.html)
(and there are [*many*](https://git-scm.com/docs/git#_git_commands))
are just an interface for manipulating that data model can help us to understand *why* Git works the way it does.

Remember, an ugly interface has to be memorized, but a beautiful design can be understood.

# Git Incantations

Learning a handful of commands to use like magic incantations is a short-term solution at best, and a dangerous habit at worst.

But maybe you just need a quick solution, or maybe you like to live dangerously. Who am I to judge?

I'm still learning Git, myself.
So if you've come here in search of the most esoteric, most arcane, most impressive Git one-liners,
you may be disappointed with my current spellbook.

## Basics

These are the cantrips, the small magics that all Git acolytes will learn.

- `git help <command>`: get help for a git command
- `git init`: creates a new git repo, with data stored in the `.git` directory
- `git status`: tells you what's going on
- `git add <filename>`: adds files to staging area
- `git commit`: creates a new commit

  Public Service Announcement:
  <br>
  Write [good commit messages](https://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html)!
  <br>
  Seriously, write [good commit messages](https://chris.beams.io/posts/git-commit/)!
  {: .notice}

- `git log`: shows a flattened log of history
- `git log --all --decorate --oneline --graph`: (a.k.a. log-a-dog) visualizes history as a DAG
- `git diff <filename>`: show changes you made relative to the staging area
- `git diff <revision> <filename>`: shows differences in a file between snapshots
- `git checkout <revision>`: updates HEAD and current branch

## Remotes

Managing your local file history is great,
but the ability to *distribute* your version control remotely imparts extra power.

- `git remote`: list remotes
- `git remote add <name> <url>`: add a remote
- `git push <remote> <local branch>:<remote branch>`: send objects to remote, and update remote reference
- `git branch --set-upstream-to=<remote>/<remote branch>`: set up correspondence between local and remote branch
- `git fetch`: retrieve objects/references from a remote
- `git pull`: effectively: `git fetch; git merge`
- `git clone`: download repository from remote

## Undo

>Git documentation has this chicken and egg problem where you can't search for how to get yourself out of a mess,
**unless you already know the name of the thing you need to know about** in order to fix your problem.<br>
-- Katie Sylor-Miller

These are for simple fixes.
If you've messed up harder, you're in good company.
Check out [Oh Shit, Git!?!](https://ohshitgit.com/) for some commands to recover from common Git mistakes.

- `git commit --amend`: edit a commit's contents/message
- `git reset HEAD <file>`: unstage a file
- `git checkout -- <file>`: discard changes

## More Advanced Git

- `git config`: Git is [highly customizable](https://git-scm.com/docs/git-config)
- `git clone --depth=1`: shallow clone, without entire version history
- `git add -p`: interactive staging
- `git rebase -i`: interactive rebasing
- `git blame`: show who last edited which line
- `git stash`: temporarily remove modifications to working directory
- `git bisect`: binary search history (e.g. for regressions)
- `.gitignore`: [specify](https://git-scm.com/docs/gitignore) files to ignore intentionally

<br>

# Resources

Here's my collection of extra reading and tangentially-related topic links.

## Augmentations

**GUIs**: try one of the [GUI Clients](https://git-scm.com/downloads/guis/)
recommended by the Git community.

**Integration**: bring your Git into your editor with extensions!
The [VSCode Marketplace](https://marketplace.visualstudio.com/search?term=git&target=VSCode&category=All%20categories&sortBy=Installs)
has many to choose from.
Tim Pope's [fugitive.vim](https://github.com/tpope/vim-fugitive) is the premier Git plugin for Vim.

**Workflows**: There are many different ways to use Git for your project.
The general rule is to follow the flow of your team or project.
If you're trying to decide which model works best for your team, consider these options:
 - [git-flow](https://nvie.com/posts/a-successful-git-branching-model/)
 - [GitHub flow](https://docs.github.com/en/get-started/quickstart/github-flow)
 - [OneFlow](https://www.endoflineblog.com/oneflow-a-git-branching-model-and-workflow)
 - [Compare Workflows](https://www.atlassian.com/git/tutorials/comparing-workflows)

## Links

 - [Pro Git](https://git-scm.com/book/en/v2) - A Creative Commons licensed book by Scott Chacon and Ben Straub.
 - [Learn Git Branching](https://learngitbranching.js.org/) - An interactive game to help learn git commands!