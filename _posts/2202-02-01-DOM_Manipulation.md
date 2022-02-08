---
layout: single
title: "DOM Manipulation"
date: 2022-02-01
categories: notes
---

Objectives:
 - Explain the relationship between the DOM and HTML
 - Explain what the DOM is
 - Explain what the document is
 - Explore and interact with the DOM using browser dev tools

Further reading on MDN: [Introduction to the Dom](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction).

<hr>

# What is the DOM?

The **Document Object Model** is an API (Application Programming Interface). An API is a means for programs to interact with each other.

The browser's DOM API allows other programs to interact with the **document**.

# What is a Document?

The most commonly used document is a web page.

The document can be modeled in a number of ways:
 - Textually via HTML (how a dev might see the page)
 - Visually via the browser window (how a user will see the page)
 - Digitally via the DOM (how a script sees the page)

# How the DOM models the document

As a tree. The data structure has a root node with branches leading to child nodes (which may, in turn, have branches).

# Fundamental DOM Data Types

| Data Type | Description |
| --- | --- |
| Document | The root `document` object, which serves as an entry point into the web page's content. |
| Node | Every object within a document is a node of some kind. `Node` is an abstract base class upon which other DOM objects are based. There is no such thing as a plain `Node` object, but every kind of DOM node is represented by an interface based on `Node`. See [nodeType](https://developer.mozilla.org/en-US/docs/Web/API/Node/nodeType). |
| Element | A type based on `Node`, which refers to an element or a node of type `element` returned by a member of the DOM. More specific classes (such as `HTMLElement`) extend the `Element` interface with methods specific to the element. |
| NodeList | A collection of elements. Items in a `nodeList` can be accessed in two ways: {::nomarkdown}<table><tr><td>list.item(index)</td><td>(returns `null` if index is out of bounds)</td></tr><tr><td>list\[index]</td><td>(returns `undefined` if index is out of bounds)</td></tr></table>{:/} NodeLists can be _live_ or _static_. Live nodelists are updated automatically with the DOM. Static nodelists are not modified automatically after creation. |
| Attr | An object reference that exposes a special interface for element attributes. Attributes are nodes in the DOM, just like Elements, but they are rarely used as such. |
| NamedNodeMap | Despite being named *node*map, `NamedNodeMap` is a live collection of `Attr` objects. Objects in a `NamedNodeMap` are not in any particular order (unlike `NodeList`), but the objects can be accessed by name or enumerated by index. |
|

# DOM Interfaces

## Interfaces and Object Inheritance

Many objects in the DOM borrow from several interfaces. For example, the table object implements the specialized `HTMLTableElement` which includes table specific methods like `insertRow`. But since it's also an HTML element, it implements the `Element` interface. Since it's an element on the DOM tree, it implements the `Node` interface.

## Core Interfaces in the DOM

These are some of the most commonly-used interfaces in the DOM.

The `document` and `window` objects are two objects whose interfaces are used very often in manipulating the DOM. `window` represents something like the browser, and `document` is the root of the document itself.

`Element` inherits from the generic `Node` interface, and together these two interfaces provide many of the methods and properties used on individual elements.

Common APIs in web scripting with the DOM include:
 - `document.querySelector(selector)`
 - `document.querySelectorAll(name)`
 - `document.createElement(name)`
 - `parentNode.appendChild(node)`
 - `element.innerHTML`
 - `element.style.left`
 - `element.setAttribute()`
 - `element.getAttribute()`
 - `element.addEventListener()`
 - `window.content`
 - `GlobalEventHandlers/onload`
 - `window.scrollTo()`