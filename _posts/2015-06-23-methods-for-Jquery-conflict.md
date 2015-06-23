---
layout:     post
title:      Methods to solve the conflict between the JQuery and other libraries
date:       2015-06-23 11:52:00
summary:    Two conditions to solving this problem
categories: JQuery
---

## Methods to solve the conflict between the JQuery and other libraries

### 1. JQuery is introduced after the other libraries
After this condition, we can use the
```js
JQuery.noConflict();
```
to release the `$` to the other libraries. Then we can use the JQuery instead of `$` for JQuery use.

On the other hand, we can assign it to a new variable
```js
var $j = JQuery.noConflict();
```
Then we can use the `$j` for JQuery use.

But if we don't want use the backup variable for use, and still want to use the `$` as the JQuery. Then we have two ways:

* Release the `$` and then pass it into the JQuery loading function like this:

```js
JQuery.noConflict();
JQuery.(function($) {
    $('p').click(function() {
        alert(1);
    });
});

$('pp').style.display = 'none'; // here, the prototype use the $
```
* Pass the $ to this anonymity function

```js
JQuery.noConflict();
(function($) {
    $(function() {
        $('p').click(function() {
            alert(1);
        });
    });
})(JQuery);
$('pp').style.display = 'none'; // here, the prototype use the $
```

### 2. JQuery is introduced before the other libraries
If this condition, then we can use `JQuery` directly without declaring the `JQuery.noConflict()`. Since at this time, the `$` is representing the other libraries.

```js
JQuery(function() {
    JQuery('p').click(function() {
        alert(1);
    });
});

$('pp').style.display = 'none'; // here, the prototype use the $
```

If we get all these, we use easily use the JQuery with other different libraries.