---
layout: single
title: "Decorators and Classes"
date: 2022-02-06
category: notes
tags:
  - JavaScript
  - Decorators
  - Classes
  - OOP
excerpt: "A first foray into object-oriented JavaScript"
tagline: "A first foray into object-oriented JavaScript"
header:
  teaser: "/assets/images/tania-miron-1t4ZtSfNtTc-unsplash-small.jpg"
  overlay_image: "/assets/images/tania-miron-1t4ZtSfNtTc-unsplash.jpg"
  overlay_filter: linear-gradient(90deg, rgba(0, 0, 0, 0.75) 33%, rgba(0, 0, 0, 0.5))
---

# Decorators

A *Decorator* is a function that accepts an object and augments it with some extra properties or functionality

This is nonstandard nomenclature!
To complicate the issue further, there is currently a Stage 2 [proposal](https://github.com/tc39/proposal-decorators) to add Class and Class Element Decorators to ECMAScript.
The decorator constructs in these notes refer to legacy JavaScript decorator function objects.
{: .notice--warning}

It's common to use adjectives as the names for decorators.
To decorate an instance of a `Car` object, we could name the decorator `carlike`.

```javascript
var carlike = function (obj, loc) {
  obj.loc = loc;
  obj.move = function () { obj.loc++; };
  return obj;
};
```

We can invoke this decorator function on any object to make it *carlike*, augmenting it with the properties that we expect a `Car` to have.

```javascript
var myCar = carlike({}, 6);
```

We instantiated a new `Car` object, `myCar`, by passing in an empty object literal to decorate.

# Classes

A *Class* is a construct capable of producing a fleet of similar objects that all conform to some interface.

It's convention to name classes with a capitalized noun.

Functions that produce these objects are called **`Constructors`**.
The object that gets returned from a constructor is called an **`instance`** of the class.
Calling a constructor to build an instance is usually referred to as instantiating.

A class *builds the object it's augmenting*, whereas the decorator patten above *accepts a target object to augment*.

We can rewrite the functional object decorator as a functional class like so:

{% highlight javascript linenos %}
var Car = function (loc) {
  var obj = { loc };
  obj.move = function () { obj.loc++; };
  return obj;
};
{% endhighlight %}

Instantiating a new instance of a `Car` object is now simpler as well:

```javascript
var myCar = Car(6);
```

There is some debate among the JavaScript community about whether Functional Class patterns are *real classes*, especially when compared to classes from other object-oriented languages.
For the purposes of these notes, we'll use our class definition above: any construct capable of producing a fleet of similar objects that all conform to some interface.
In this sense, the `Car` function object is a class!
{: .notice--info}

## Avoiding Duplicate Methods

The `Car` functional class object above will unfortunately result in duplicate `move` methods for each instance of a `Car` object.
If we know that the number of `Car` objects will be small, this might be an okay tradeoff, since the time to develop such a pattern is low.

For an arbitrary number of `Car` objects, or an arbitrarily complicated `move` method, this pattern is a waste of resources.
To avoid this, we can move the function definition outside of `Car`, and then assign it for every new instance rather than create it for every new instance.

{% highlight javascript linenos %}
var Car = function (loc) {
  var obj = { loc };
  obj.move = move;
  return obj;
};
var move = function () { this.loc++ };
{% endhighlight %}

The assignment on line 3 now references a function that is only created once.
The keyword `this` allows us flexible access to the properties of the individual instance at call-time, since we do not have local-scope access to the properties within `Car` once we move the function outside that scope.

For more information about JavaScript keyword `this`, check out my [notes on the subject](/path/to/this_notes).
{: notice--tip}
