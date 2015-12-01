---
layout:     post
title:      ES6 about let and const keyword
date:       2015-06-23 11:52:00
summary:    about the let and const keyword
categories: Javascript
---

## ES6 summary Chapter 1: about let and const keyword

### let
1. `let` keyword is used for declaring variable, but it has block scope. If it is outside, then it will suffer the ReferenceError. 
2. **Notice**: the `let` keyword doesn't allow to duplicatedly declare the variable, even if you use the `var` or `const`. Will throw an error.
3. The benefits of using let:
    1. we will no longer use the IIFE to build a block scope.
    2. we will no longer use the closure to build a block scope.
4. **Notice**:  
In ES6, the scope of a function is also inside its block scope.
  
```js
function f() { console.log('I am outside!');
;(function() {
    if(false) {
        //declare the function again
        function f() { console.log('I am inside!');
    }
f();
}());
```
For this situation, if under ES5, we will get 'I am outside!', if under es6, we will get 'I am inside!'.
5. The `let` will not hoist the variable like `var` keyword.

### const
1. If declared by using const, we will never change the value again.
2. If we declare the value again, we will not get an error, the system will just ignore it.
3. The const is the same with let, they both has the block scope.
4. The same with let, cannot duplicatedly declare the variable, will also throw an error.

