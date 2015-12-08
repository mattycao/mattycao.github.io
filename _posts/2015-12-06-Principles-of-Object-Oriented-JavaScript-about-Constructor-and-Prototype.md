---
layout:     post
title:      Principles of Object Oriented JavaScript about Constructor and Prototype
date:       2015-12-06 20:50:00
summary:    Principles of Object Oriented JavaScript Part3
categories: Javascript
---
## Chapter 4 Constructor and prototypes
1. A constructor is simply a function that is used with `new` to create an object. 
2. Check type of instance using the constructor property. Since every object instance is automatically created with a `constructor` property that contains a reference to the constructor function. But the instanceof method is recommended. Because the `constructor` might be overwritten and cause mistakes.
3. You can also explicitly call return inside of a constructor. If the returned value is an object, it will be returned instead of the newly created object instance. If the returned value is a primitive, the newly created object is used and the returned value is ignored.
4. In the constructor, it allows as to initialize an instance of a type in a consistent way, like by using the `Object.defineProperty()`.
5. The new keyword lets the function getting an return value.
6. **Notice**: An error occurs if you call the Person constructor in strict mode without using new. This is because strict mode doesn’t assign this to the global object. Instead, this remains undefined, and an error occurs whenever you attempt to create a property on undefined.

### Prototypes
1. Every function(with the exception of some built-in functions) has a `prototype` property that is used during the creation of new instances.
```js
// a way to identify a prototype property
function hasPrototypeProperty(object, name) {
    return name in Object && !object.hasOwnProperty(name);
}
```
2. `[[prototype]]` property: An instance keeps track of tis prototype through an internal property called `[[prototype]]`. This property is a pointer back to the prototype object that the instance is using. WHen you create a new object using new, the constructor's prototype property is assigned to the `[[Prototype]]` property of that new object. 
3. You can read the value of the `[[Prototype]]` property by using the `Object.getPrototypeOf()` method on an object. 
4. **Notice**: Some JavaScript engines also support a property called `_proto_`on all objects. This property allows you to both read from and write to the `[[Prototype]]` property. Firefox, Safari, Chrome, and Node.js all support this property, and `_proto_` is on the path for standardization in ECMAScript 6.
5. `isPrototypeOf`:
```js
var object = {};
console.log(Object.prototype.isPrototypeOf(object);
```
6. **Notice**:   
Keep in mind that you can’t delete a prototype property from an instance because the delete operator acts only on own properties. You cannot assign a value to a prototype property from an instance.
7. Using Prototypes with constructor: the constructor property exists on the prototype, so when we create an object, we can do it like this way: 
```js
function Person(name) {
    this.name = name;
}
Person.prototype = {
    constructor: Person,
    sayName: function() {
        console.log(this.name);
},
toString: function() {
    return "[Person " + this.name + "]";
}
};
var person1 = new Person("Nicholas");
var person2 = new Person("Greg");
console.log(person1 instanceof Person); // true
console.log(person1.constructor === Person); // true
console.log(person1.constructor === Object); // false
console.log(person2 instanceof Person); // true
console.log(person2.constructor === Person); // true
console.log(person2.constructor === Object); // false
```
8. The ability to modify the prototype at any time has some interesting repercussions for sealed and frozen objects. When you use `Object.seal()` or `Object.freeze()` on an object, you are acting solely on the object instance and the own properties. You can’t add new own properties or change existing own properties on frozen objects, but you can certainly still add properties on the prototype and continue extending those objects.