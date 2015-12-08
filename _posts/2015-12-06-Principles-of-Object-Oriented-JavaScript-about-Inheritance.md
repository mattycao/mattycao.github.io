---
layout:     post
title:      Principles of Object Oriented JavaScript about Inheritance
date:       2015-12-07 21:50:00
summary:    Principles of Object Oriented JavaScript Part4
categories: Javascript
---

## Chapter 5: inheritance
1. Prototype Chaining and Object.prototype
2. Methods inherited from Object.prototype: `hasOwnProperty()`, `propertyIsEnumerable()`, `isPrototypeOf()`, `valueOf()`, `toString()`.
3. `valueOf`, gets called whenever an operator is used on an object. By default, `valueOf()` simply returns the object instance.  But the primitive type will override the `valueOf()` method and it will return the string, boolean, and number. The Date Object, will return the epoch time in milliseconds.
4. `toString()`, is called as fallback whenever `valueOf()` returns a reference value instead of a primitive value. It is also implicitly called on primitive values whenever js is expecting a string. 
5. Modifying `Object.prototype`, totally forbidden
6. Object inheritance: We can specify `[[Prototype]]` with the Object.create() method.

```js
var book = {
    title: "The Principles of Object-Oriented JavaScript"
};
// is the same as
var book = Object.create(Object.prototype, {
title: {
configurable: true,
enumerable: true,
value: "The Principles of Object-Oriented JavaScript",
writable: true
}
});
```
We can also set the first parameters to be null.
7. Constructor Inheritance: based on that every function has a `prototype` property that can be modified or replaced. 

```js
function YourConstructor() {
// initialization
}
// JavaScript engine does this for you behind the scenes
YourConstructor.prototype = Object.create(Object.prototype, {
constructor: {
    configurable: true,
    enumerable: true,
    value: YourConstructor
    writable: true
}
});
```
8. An example of using the constructor inheritance:
```js
Square.prototype = new Rectangle();
Square.prototype.constructor = Square;
```
9. This the same as it show before:

```js
// inherits from Rectangle
function Square(size) {
    this.length = size;
    this.width = size;
}
Square.prototype = Object.create(Rectangle.prototype, {
constructor: {
    configurable: true,
    enumerable: true,
    value: Square,
    writable: true
}
});
Square.prototype.toString = function() {
    return "[Square " + this.length + "x" + this.width + "]";
};
```
10. constructor stealing: using the `call` and `apply`
11. Access super-type methods: Father.prototype.method to access the parent methods.