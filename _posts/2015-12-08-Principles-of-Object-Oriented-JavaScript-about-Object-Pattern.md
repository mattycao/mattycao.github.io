---
layout:     post
title:      Principles of Object Oriented JavaScript about Object Pattern
date:       2015-12-08 20:55:00
summary:    Principles of Object Oriented JavaScript Part5
categories: Javascript
---

### Chapter 6 Object patterns
1. Private and privilege members: prefix properties with an underscore, which means it is private.
2. the module pattern: object creation pattern designed to create singleton object with private data. using the Immediately invoked function expression. It is called privilieged methods:

```js
var yourObject = (function() {
// private data variables
return {
// public methods and properties
};
}());
var person = (function() {
    var age = 25;
return {
    name: "Nicholas",
    getAge: function() {
        return age;
    },
    growOlder: function() {
        age++;
    }
};
}());
var person = (function() {
var age = 25;
function getAge() {
    return age;
}
function growOlder() {
    age++;
}
return {
    name: "Nicholas",
    getAge: getAge,
    growOlder: growOlder
};
}());
```
The last one is preferred, it is also called revealing module pattern.
3. Private members for constructors: define a private property in the constructor:

```js
function Person(name) {
// define a variable only accessible inside of the Person constructor
var age = 25;
this.name = name;
this.getAge = function() {
    return age;
};
this.growOlder = function() {
    age++;
};
}
var person = new Person("Nicholas");
console.log(person.name); // "Nicholas"
console.log(person.getAge()); // 25
person.age = 100;
console.log(person.getAge()); // 25
person.growOlder();
console.log(person.getAge()); // 26
```
4. If we want share the variable in all instances, then we can define it like this way:

```js
var Person = (function() {
// everyone shares the same age
    var age = 25;
    function InnerPerson(name) {
    this.name = name;
}
InnerPerson.prototype.getAge = function() {
    return age;
};
InnerPerson.prototype.growOlder = function() {
    age++;
};
return InnerPerson;
}());
var person1 = new Person("Nicholas");
var person2 = new Person("Greg");
console.log(person1.name); // "Nicholas"
console.log(person1.getAge()); // 25
console.log(person2.name); // "Greg"
console.log(person2.getAge()); // 25
person1.growOlder();
console.log(person1.getAge()); // 26
console.log(person2.getAge()); // 26
```
5. mixins: One thing to keep in mind about using mixins in this way is that accessor properties on the supplier become data properties on the receiver, which means you can overwrite them if you’re not careful：

```js
function mixin(receiver, supplier) {
for (var property in supplier) {
if (supplier.hasOwnProperty(property)) {
receiver[property] = supplier[property]
}
}
return receiver;
}
// a better way to solve the problem, here, only work for the ECMAscript5
function mixin(receiver, supplier) {
     Object.keys(supplier).forEach(function(property) {
    var descriptor = Object.getOwnPropertyDescriptor(supplier, property);
     Object.defineProperty(receiver, property, descriptor);
});
return receiver;
}
```
**Notice**:   
Keep in mind that `Object.keys()` returns only enumerable properties. If you want to also copy over none-numerable properties, use `Object.getOwnPropertyNames()` instead.
6. Scope-safe constructors: Keep in mind that this code is running in non-strict mode, as leaving out new would throw an error in strict mode. Many built-in constructors, such as Array and RegExp, also work without new because they are written to be scope safe. A scope-safe constructor can be called with or without new and returns the same type of object in either case

```js
function Person(name) {
if (this instanceof Person) {
this.name = name;
} else {
return new Person(name);
}
}
```
Using this pattern and we will get a scope-safe. 