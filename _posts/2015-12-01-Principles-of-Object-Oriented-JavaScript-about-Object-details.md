---
layout:     post
title:      Principles of Object Oriented JavaScript about Object details
date:       2015-12-01 11:02:00
summary:    Principles of Object Oriented JavaScript Part2
categories: Javascript
---

## Chapter 3 Understanding Objects
1. Define properties  
    When a property is firstly added to an object, JS uses an internal method called [[Put]] on the object. The [[Put]] method creates a spot in the object to store the property. You can compare this to adding a key to a hash table for the first time. The result of calling [[Put]] is the creation of an own property on the object. 
2. When a new value is assigned to an existing property, a separate operation called [[Set]] takes place. This operation replaces the current value of the property with the new one.
3. Detecting Properties: in, hasOwnProperty  

```js
'name' in person1
person1.hasOwnProperty('name')
```
4. Remove property: the delete operator works on a single object property and calls internal operation named [[delete]]. Think of thi operation as removing a key/value pair from a hash table. Return true if success.
5. Enumeration: By default, all properties that we add are enumerable, which means that you can iterate over them using a for-in loop. Enumerable properties have their internal [[Enumerable]] attributes set to true. 
6. ECMAScript5 introduce Object.keys(object) to return array of enumerable property names, as shown here.
7. Notice the differences: for-in loop also enumerates prototype properies, while Object.keys() only return the own instance properties.
8. Many native properties are not enumerable by default. [[Enumerable]] set to be false. object.propertyIsEnumerable('length') will tell us.
9. Type of properties: data properties, and access properties. Accessor properties don’t contain a value but instead define a function to call when the property is read (called a getter), and a function to call when the property is written to (called a setter). Accessor properties only require either a getter or a setter, though they can have both.

```js
var person = {
    _name: 'Ni',
    get name() {
        console.log('Reading Name');
        return this._name;
    } 
    set name(value) {
        console.log('Set name to' + value);
        this._name = value;
    }
    // The leading underscore means the variable is for private use.
}
```
**Note**:
You don’t need to define both a getter and a setter; you can choose one or both. If you define only a getter, then the property becomes read-only, and attempts to write to it will fail silently in nonstrict mode and throw an error in strict mode. If you define only a setter, then the property becomes write-only, and attempts to read the value will fail silently in both strict and nonstrict modes.
10. Property Attributes: In ECMAScript5, for the data property and access properties
    1. The common attributes: eumerable, and configurable(whether the property can be changed). You can remove a configurable property using delete and can change its attributes at any time. (This also means configurable properties can be changed from data to accessor properties and vice versa.)
    2. We can change property attributes, by using the `Object.defineProperty()` method. This method accespts three arguments: the object owns the property, the property name, and an object that contain the attribute.

```js
var person1 = {
    name: "Nick'
    };
Object.defineProperty(person1, 'name', {
    enumerable: false
    });
    person1.propertyIsEnumerable('name'); // false
    // if you already define the configurable to be false already, then you cannot change it back.
    // And notice the square brackets are missing here.
```
    3. **Note**: When JavaScript is running in strict mode, attempting to delete a nonconfigurable property results in an error. In nonstrict mode, the operation silently fails.
11. The data property attributes: data properties have two more properties that accessors do not.
    1. [[Value]], which hold the property value, even if it is a function, it is called automatically when you create a property on an object.
    2. [[Writable]], which is a boolean value.
12. With these two more values, we can define a data property by using the Object.defineProperty().

```js
var person1 = {};
Object.defineProperty(person1, "name", {
    value: "Nicholas",
    enumerable: true,
    configurable: true,
    writable: true
});
```
13. **Notice**: Nonwritable properties throw an error in strict mode when you try to change the value. In nonstrict mode, the operation silently fails.
14. Accessor Property Attributes: since no value is stored for accessor properties, there is no need for [[value]]. So access have [[Get]] and [[Set]]. 
15. The benefit of using Object.defineProperty() is that we can define a accessor properties for the existing objects.

```js
var person1 = {
_name: "Nicholas"
};
Object.defineProperty(person1, "name", {
get: function() {
console.log("Reading name");
return this._name;
},
set: function(value) {
console.log("Setting name to %s", value);
this._name = value;
},
enumerable: true,
configurable: true
});
```
16. **Note**: As with accessor properties defined via object literal notation, an accessor property without a setter throws an error in strict mode when you try to change the value. In nonstrict mode, the operation silently fails. Attempting to read an accessor property that has only a setter defined always returns undefined.
17. Define multiple properties: Object.defineProperties()
18. Retrieving property attribute: `Object.getOwnPropertyDescriptor(the object, the attribute)`,return the object contains the four property attributes.

```js
var person1 = {
name: "Nicholas"
};
var descriptor = Object.getOwnPropertyDescriptor(person1, "name");
console.log(descriptor.enumerable); // true
console.log(descriptor.configurable); // true
console.log(descriptor.writable); // true
console.log(descriptor.value); // "Nicholas"
```
19. Preventing Object Modification: the object has an attribute `[[Extensible]]` that is a boolean value indicating if the object itself can be modified.
20. Two method: 1. `Object.isExtensible()` 2. `Object.preventExtensions()`

```js
var person1 = {
name: "Nicholas"
};
    console.log(Object.isExtensible(person1)); // true
    Object.preventExtensions(person1);
    console.log(Object.isExtensible(person1)); // false
    person1.sayName = function() {
    console.log(this.name);
};
console.log("sayName" in person1);
```
**Note**: Attempting to add a property to a nonextensible object will throw an error in strict mode. In nonstrict mode, the operation fails silently. You should always use strict mode with nonextensible objects so that you are aware when a nonextensible object is being used incorrectly.
21. Sealing Objects: set it to noconfigurable and non-extensible. Not only you can not add the new properties to the object, but also you cannot remove properties or change the type(from data to accessor or vice versa). 
22. Using 2 methods: `Object.seal()` and `Object.isSealed()`.
23. Freezing Objects: seal and also read-only with the data property
24. Frozen objects cannot become unfrozen, so they remain in the state they were. `Object.freeze()`, `Object.isFrozen()`. 