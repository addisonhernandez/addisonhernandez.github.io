---
title: "Pseudoclassical Classes"
date: 2022-02-04
category: notes
tags: Notes JavaScript Prototypes Inheritance Classes OOP
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
```javascript
var Car = function (loc) {
  this    = Object.create(Car.prototype); // "created" by `new`

  var obj = Object.create(Car.prototype);
  obj.loc = loc;
  return obj;

  return this; // "created" by `new`
};
Car.prototype.move = function () { this.loc++; };

var pseudoCar = new Car(42)
```
Removing the redundant lines, the above code can therefore be pared down to:
```javascript
var Car = function(loc) {
  this.loc = loc;
};
Car.prototype.move = function () { this.loc++; };

var pseudoCar = new Car(42);
```
When invoked as `new Car(42)`, a new object will be created (and `this` will be bound to it) behind the scenes, and will be returned after modification.

The pseudoclassical class structure is syntactic sugar on top of the prototypal class structure. Hence 'pseudo'.