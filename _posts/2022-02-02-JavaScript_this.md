---
layout: single
title: "JavaScript Keyword: this"
date: 2022-02-02
category: notes
tags: Notes JavaScript this
---

`this` is an identifier for values that is designed to support object oriented programming.

When a *method* or a *constructor* is invoked, the JavaScript interpreter binds `this` to a **likely-focal object**.

<hr>

**⚠ Warning ⚠**

Resources on the internet regarding `this` are often inconsistent, and frequently misrepresent the truth.

<hr>

# What `this` isn't

Consider the following snippet:

{% highlight javascript %}
var fn = function (a, b) {
  console.log(this);
}
{% endhighlight %}

`this` ***is not*** bound to:
- the function object in which `this` appears
- an instance of the function in which `this` appears
- an object that has the function in which `this` appears as a property (method)
- an object literal that has the function in which `this` appears
- an execution context or scope of the function call in which `this` appears

`this` ***is*** bound to:
- the object to the left of the dot where the containing function is called.

For example:

{% highlight javascript %}
var obj = {
  fn: function (a, b) {
    console.log(this);
  }
};

obj.fn(3, 4);
{% endhighlight %}

In the statement `obj.fn(3, 4)`, since `obj` is to the left of the dot, `this` is bound to `obj`.

The operative determiner of which object `this` refers to depends only on the function invocation, and not with that function's membership as a property of any other objects.

## Behavior of the `this` keyword

The rules for how `this` gets a binding mostly resemble the rules for how positional function parameters get bindings.

If we want to call a method with an object that does not have the function as a property, we can use `Function.prototype.call()` using the following syntax:

{% highlight javascript %}
fn.call(thisArg, ...args);
{% endhighlight %}

`.call()` can also be used to override `this` when the function is already being invoked as a method of another object.

{% highlight javascript %}
obj.fn.call(anotherObj, ...args); // `this` binds to anotherObj
{% endhighlight %}

## How to determine what `this` is bound to

| Binding Pattern | Binding Target | Use Case |
|-----------------|----------------|----------|
| Global Reference | The `global` object (`window` in a browser) | The default choice (when not `use strict`) |
| Free Function Invocation | The `global` object (`window` in a browser) | The default choice
| `.call` or `.apply` Invocation | First argument (`thisArg`) | Manually specifies a binding for `this` |
| Construction Mode (`new`) | A **new object** created for the invocation | So constructors will operate on the instance they create |
| Method Invocation | object on the left of the **call time** dot | So methods run in the context of the object calling them |

*Most* of the time, it's Method Invocation. Look to the left of the dot.