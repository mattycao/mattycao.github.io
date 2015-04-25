---
layout:     post
title:      Sharp JQuery Chapter 1 Summary
date:       2015-04-24 11:02:20
summary:    Chapter 1 about what is JQuery and what it is used for.
categories: jquery
---

## Chapter1 认识JQuery
### JQuery和JS的认识
1. Javascript的弊端
  * 复杂的DOM
  * 不一致的浏览器实现和便捷的开发
  * 调试工具的缺乏
2. AJAX: asynchronous Javascript And XML
3. JQuery: 轻量级的库，拥有强的选择器，出色的DOM操作，可靠的时间处理，和完善的兼容性和链式操作等功能。
4. JQuery的优势：
  * 轻量级 采用uglifyJS压缩
  * 强大的选择器 支持CSS1到CSS3几乎所有的选择器， 以及JQuery自己的高级的选择器。
  * 出色的DOM封装
  * 可靠的事件处理机制，有graceful degradation, 循序渐进和非入侵式(unobtrusive)编程思想。
  * 完善的AJAX
  * 不污染顶级变量
  * 出色的浏览器兼容性
  * 链式操作
  * 隐式迭代
  * 行为层和结构层的分离
  * 丰富的插件支持
  * 完善的文档
  * 开源

### JQuery代码的编写
1. JQuery分为生产版和开发版
2. 在JQuery库中，$就是JQuery的一种简写形式
3. First Code:
{% highlight javascript %}
$(document).ready(function() {
    alert('JQuery');
});
{% endhighlight %}

|item | `window.onload` | `$(document).ready()`|
|:---|:-----:|:------:|
|执行时机| 必须等待网页中所有的内容加载完毕后(包括图片)才能执行 | 网页中所有的DOM结构绘制完毕后就执行 可能DOM元素相关联的东西并没有加载完|
|编写个数| 不能写多个 | 可以写多个|
|简化写法| 无 | `$(function(){})`|

### JQuery的代码风格：
  - 对于同一个对象不超过三个操作，可以直接写成一行。
  - 对于同一个对象的较多操作，建议每行写一个操作
  - 对于多个对象的少量操作，可以每个对象写一行，如果涉及子元素，可以考虑适当的缩进。
{% highlight javascript %}
$(this).addClass('a')
    .children('li').show().end()
.siblings.removeClass('high');
{% endhighlight %}
### JQuery对象和DOM对象
1. DOM对象:
  Document Object Model by using the document method.
2. JQuery对象:
  就是通过JQuery包装DOM对象后产生的对象。
  如果他是一个JQuery对象，那么就可以使用JQuery里面的方法。
  因此，在JQuery对象中，无法使用DOM对象的任何方法。

###JQuery对象和DOM对象之间的相互转换
1. 首先， 我们先约定好获取的对象如果为JQuery对象，那么我们要在变量前面加上$。
  {% highlight javascript %}
     var $v = $('#id');
  {% endhighlight %}
2. JQuery对象转成DOM对象
  - `[index]`
  - `get(index)`
JQuery对象是一个类似数组的对象，可以通过`index`的方法来获得相应的DOM对象：
  {% highlight javascript %}
    var c = $v[0]; // the dom element
  {% endhighlight %}
另一种方法是JQuery本身提供的，通过`get(index)`得到相应的DOM对象；
  {% highlight javascript %}
    var c = $v.get(0);
  {% endhighlight %}
3. DOM对象转成JQuery对象
  {% highlight javascript %}
    var $v = $(c);
  {% endhighlight %}
然后我们就可以随意使用JQuery所提供的相关方法了。
4. 解决JQuery和其他库的冲突
  {% highlight javascript %}
    JQuery.noConflict(); // 释放$的控制权给其他库，JQuery仍然可以用JQuery来使用JQuery的东西。
    //此外，我们可以自定义一个备用名称来access
    var $j = JQuery.noConflict();
    // it can be a, $a, awesome 等
  {% endhighlight %}
5. 如果不想给备用名，而同时使用$符号，我们可以：
  {% highlight javascript %}
    //method 1
    JQuery.noConflict();
    JQuery(function($) {
        //here we can still using the $ as the JQuery
    });
    // outside the scope, the $ will be released
    //method 2
    JQuery.noConflict();
    (function($) {
        $(function() {
        //here we can still using the $ as the JQuery
        });
    })(JQuery);
    // outside the scope, the $ will be released
  {% endhighlight %}
6. JQuery库在其他库之前导入
  如果JQuery库在其他库之前就导入，那么可以直接使用JQuery来做JQuery一些的工作，而无需调用`JQuery.noConflict()`.