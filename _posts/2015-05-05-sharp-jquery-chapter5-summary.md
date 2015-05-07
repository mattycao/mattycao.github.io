---
layout:     post
title:      Sharp JQuery Chapter 5 Summary
date:       2015-05-05 18:33:00
summary:    Chapter 5 about JQuery for form and table!
categories: jquery
---

## Chapter 4 JQuery对表单表格的操作以及更多的应用
### 5.1 表单应用
form has three parts:
* the form tag, it has the action and method about the form
* the input area, like text, password, email, textarea, multiselect, tab, files
* the submit button like submit, reset, and some thing else.

#### 5.1.1 单行文本框应用
简单的应用关于获取和失去焦点改变样式。
* 如果不是IE6， 我们可以根据css中的`:focus`伪类来进行操作，但是JQuery可以弥补这个不足。

```javascript
    $(':input').focus(function() {
        $(this).addClass('focus');
    }).blur(function() {
        $(this).removeClass('focus');
    });;
```
#### 5.1.2 多行文本框应用
* 可以通过改变行高来实现大小框的动画效果。

```javascript
    if($comment.is(':animated')) {
        $comment.animate({height: '-=50'}, 400);
    }
```
* 滚动条高度变化: 通过改变scrollTop来实现这个功能

```javascript
    if($comment.is(':animated')) {
        $comment.animate({scrollTop: '-=50'}, 400);
    }
```
#### 5.1.3 复选框的应用
对于checkbox的应用有以下几个注意的点：
* 复选框是否被选中需要检查`checked`这个属性，在JQuery中我们可以用`attr()`或者`prop()`两个方法，但是两个方法是有区别的：

```javascript
    $('[name=items]:checkbox').attr('checked');
    $('[name=items]:checkbox').prop('checked');
    // 虽然看上去是没有区别的，但是attr虽然是获取元素的属性，但是比如遇见了类似checked或者disabled属性，这些属性只需要写出来即可，并没有赋值，所以在这种条件下，最好用prop()方法，他会返回一个boolean值。，而不是像attr()方法一样返回一个disabled或者空字符串。总之，如果属性值就是boolean值或者可以不写属性值的时候，我们要用prop()方法。
```
* 复选框的选择符: ` $('[name=items]:checkbox')`
* 要获取复选框的值我们要用: `$(this).val()`
* 在这个情况下，也许用JQuery并不合适，原生的JS获取类似属性是非常方便的: `this.checked`

#### 5.1.3 下拉框的应用
关于select的应用有以下几点需要注意：
* appendTo()方法可以完成删除和追加的两个步骤

```javascript
    var $options = $('#select1 option:selected');
    var $remove = $options.remove();
    $remove.appendTo('#select2');
    // 等价于
    var $options = $('#select1 option:selected');
    $options.appendTo('#select2');
    $('option:selected', this); // notice this one
```
#### 5.1.4 表单的应用
这里例子非常好，review please

### 5.2 表格应用
#### 5.2.1 表格变色
* 普通的隔行变色

```javascript
    $('tbody>tr:odd').addClass('odd');
```
* 单选框控制表格行高亮
主要看例子，但是引入了两个新的东西介绍：
  * `end()`:  End the most recent filtering operation in the current chain and return the set of matched elements to its previous state.
  * `:has`: Selects elements which contain at least one element that matches the specified selector.
* 复选框控制表格行高亮
* 表格展开关闭
* 表格内容筛选，用到了filter()方法和:contains选择器

### 5.3 其他应用
例子非常典型，注意复习，有关于选项卡等。

