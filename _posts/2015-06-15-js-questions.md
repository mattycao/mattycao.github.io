---
layout:     post
title:      A question about call in javascript
date:       2015-06-15 09:32:00
summary:    deep understanding about call method in javascript
categories: Javascript
---
## One Question about Call
### Question:

```js
function fn1() {
    console.log(1);
};
function fn2() {
    console.log(2);
};
fn3 = fn2.call;
fn2.call(fn1);
fn3.call(fn1);
```
Please determine what we get?

### Solution:
```js
2
1
```
Key Points:
* fn1.call is a function reference. which is:
    * `Function.prototype.call = fn3 = fn1.call;`
* When fn1.call() is running, what is this inside the call? Notice the `this` inside the `call` and `this` inside the `fn1` are totally different. The `this` inside `call` is `fn1`. The one who called `call` method, it is the `this` of `call` method. On the other hand, `this` inside `call` is the one who called `call` method.
* Call is also a method, it can use the `call` method also.
* The `this` in the class method, it is like `this` is calling the method.

Thus,

fn3.call(fn1) == fn2.call(fn1) = Function.prototype.call.call(fn1) = fn1.call() = fn1;
Thus, we get 1 as a result.