---
title: "Pseudoclassical Classes"
date: 2022-02-04
category: notes
tags:
  - Notes
  - JavaScript
  - Prototypes
  - Inheritance
  - Classes
  - OOP
excerpt: "How to teach an old function object a 'new' trick"
tagline: "Refactoring function objects with a 'new' class pattern"
header:
  teaser: "/assets/images/stefany-andrade-GbSCAAsU2Fo-unsplash-small.jpg"
  overlay_image: "/assets/images/stefany-andrade-GbSCAAsU2Fo-unsplash.jpg"
  overlay_filter: linear-gradient(90deg, rgba(0, 0, 0, 0.75) 33%, rgba(0, 0, 0, 0.5))
---

Using this prototypal class structure:

```javascript
var Car = function (loc) {
  var obj = Object.create(Car.prototype);
  obj.loc = loc;
  return obj;
};
Car.prototype.move = function () { this.loc++; };

var protoCar = Car(42);
```

Let's refactor using a related pattern: **pseudoclassical**.

# The `new` keyword

Invoking a function with `new` handles the tasks of creating an object prototype chain and returning an object within the function automatically. It's effectively like doing this:

{% highlight javascript linenos %}
var Car = function (loc) {
  this    = Object.create(Car.prototype); // "created" by `new`

  var obj = Object.create(Car.prototype);
  obj.loc = loc;
  return obj;

  return this; // "created" by `new`
};
Car.prototype.move = function () { this.loc++; };

var pseudoCar = new Car(42); // note the inclusion of `new`
{% endhighlight %}

Removing the redundant statements on lines 4 and 7, the above code can be pared down to:

```javascript
var Car = function(loc) {
  this.loc = loc;
};
Car.prototype.move = function () { this.loc++; };

var pseudoCar = new Car(42);
```

When the `Car` function object is invoked with `new Car(42)`, a new object will be created (and `this` will be bound to it) behind the scenes, and will be returned after modification.

The pseudoclassical class structure is syntactic sugar on top of the prototypal class structure. Hence 'pseudo'.