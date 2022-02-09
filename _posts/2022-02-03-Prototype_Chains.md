---
title: "Prototype Chains"
date: 2022-02-03
category: notes
tags: Notes JavaScript Prototypes Inheritance OOP
---

**Prototype Chains are a mechanism for making objects that resemble other objects.**

When we want to have two objects that have all the same properties, the naive approach would be to copy all properties of one object into the other. Using prototype chains, on the other hand, makes one object behave as though it has all the properties of another by delegating failed property lookups to the other object at lookup time.

# How to Create Prototype Chains

The [`Object.create(proto [, propertiesObject])`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/create) static method will make a prototype chain for us.

An important benefit of this method is that failed property lookups are delegated at lookup-time, whereas a one-time property copy utility function will only clone properties at copy-time. Any new properties added to the prototype object will be accessible via fall-through by objects created with `Object.create`.

<br>

# Prototypal Classes

Transform this:
```javascript
var Car = function (loc) {
  var obj = { loc: loc };
  _.extend(obj, Car.methods);
  return obj;
};
Car.methods = {
  move: function () { this.loc++; };
};
```

into this:
```javascript
var Car = function (loc) {
  var obj = Object.create(Car.prototype);
  obj.loc = loc;
  return obj;
};
Car.prototype.move = function () { this.loc++; };
```

This establishes a prototype chain (`.extend` replaced with `.create`) and assigns `loc` directly. Each instance of Car now has access to properties and methods of the parent class, Car, without having to copy them individually at instatiation.

# Note about using the term `prototype`

If someone says "`obj1`'s prototype is `obj2`," a reasonable interpretation would be to think that failed lookups on `obj1` would fall through to `obj2`. This is true if `obj1` is an *instance* created from a prototypal `obj2` class. This interpretation is **not true** of the relationship the class itself has to the prototype.

A class is declared using a function object, and failed property lookups on it will fall through to `Fuction.prototype`! In the example above, Car is a function object, and it's prototype is `Function.prototype`.

*And yet*, when `Car` runs, it will create objects that delegate failed lookups to `Car.prototype`. In that sense, you might say that Car's prototype is `Car.prototype`. The nomenclature is confusing and ambiguous.

Saying that an instance prototype is `obj.prototype` means something different from saying that a class prototype is `obj.prototype`, yet both can be true statements!
