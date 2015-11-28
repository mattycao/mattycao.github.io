---
layout:     post
title:      Principles of Object Oriented JavaScript about types and functions
date:       2015-06-23 11:52:00
summary:    Principles_of_Object_Oriented_JavaScript Part1
categories: Javascript
---

## Chapter 1 Primitive and reference types
1. Object oriented programming has such characteristics:
    1. encapsulation
    2. aggregation
    3. inheritance
    4. polymorphism
2. Function are represented as objects in js, which makes them first-class functions.
3. Types: not just like other oop, the js doesn't has the stack and heap concept. It tracks variables for a perticular scope with variable object.
4. So the primitive type store directly on the variable object, while the reference values are placed as a pointer in the variable object.
5. five primitive types: boolean, number, string, null, undefined
6. null has only one value, that is the null
7. undefined, a type that only has one value, undefined is the value assigned to a vairable that is not initialized.
8. Assignment of the primitive value will result in a copy.
9. The best way to identify primitive types is with the typeof operator.
10. Trick part: typeof null -> object
11. Best way to determine the null is using the === strict mode
12. Some built in object types: Array, date, error, function, object, regExp
13. **Note**:  
Using an object literal doesn't actually call new Object(). Instead, the javascript engine follows the same steps it does when using the new Object() without actually calling the constructor. This is true for all reference literals.
14. Function call also be created by the constructor form.
15. Identify Arrays:  
JavaScript values can be passed back and forth between frames in the same web page. This becomes a problem only when you try to identify the type of a reference value, because each web page has its own global context—its own version of Object, Array, and all other builtin types. As a result, when you pass an array from one frame to another, instanceof doesn’t work because the array is actually an instance of Array from a different frame.

So to solve the problem, ECMAscirpt 5 introduce the Array.isArray(). Not for the ie8 or earlier.
16. Primitive wrapper types: The primitive wrapper types are reference types that are automatically created behind the scenes whenever strings, numbers,
or Booleans are read. After that it will be destroyed, which is also called autoboxing.
16. Because instanceof doesn’t actually read anything, no temporary objects are created, and it tells us the values aren’t instances of primitive wrapper types. so name instanceof String return false.

### Chapter 2 Function
1. Function is object also. What distinguishes it from other object is the property of *internal property* named [[call]]. typeof operator uses this to distinguish the function. 
2. Function declarations and function expressions, this is a function expressions.

```js
var add = function(a, b) {
    return a + b;
    };
```
Function declarations are hoisted to the top of the context.
3. Function hoisting happens only for function declarations because the function name is known ahead of time.
4. First value function which makes the function can be used as any other reference value.
5. The defined function has a property named length, which indicate the number of parameters it expects.
6. Overloading: doesn't like other language has the signature, so it we can check the named parameter against undefined is more common.
7. The keyword this is useful for multiplexing.
8. The ways to changing this: 
    1. the call() method, the first one is new this. Then give the parameters as one by one. The call method is the method of a function.
    2. apply, only two parameters.
    3. bind, which is introduced in ECMAScript5. Notice attaching a method to an object doesn't change 'this'.