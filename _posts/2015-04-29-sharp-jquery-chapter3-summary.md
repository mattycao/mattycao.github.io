---
layout:     post
title:      Sharp JQuery Chapter 3 Summary
date:       2015-04-29 21:18:00
summary:    Chapter 2 about JQuery DOM manipulation!
categories: jquery
---

## Chapter 3 JQuery中的DOM操作

### 3.1 DOM操作的分类

* DOM Core
DOM Core并不专属JS, 任何一种支持DOM的程序设计语言都可以使用它。它的用途并非仅限于处理网页，也可以用来处理任何一种使用标记语言编写的文档，例如XML。比如JS中getElementById，getAttrbute等都是Core的组成部分。
* HTML-DOM
比如`document.forms`, `element.src`, 通过上面的类似属性我们可以获取web文档中的一些对象和属性。
* CSS-DOM
针对CSS的操作。主要是设置和获取style对象的各种属性。`element.style.color = 'red'`。

### 3.2 JQuery中的DOM操作
注意，在这里所有的操作都是围绕DOM树展开。
#### 3.2.1 查找节点
我们通过在第二章的JQuery选择器完成。
  * 查找元素节点:
    ```javascript
    var #li = $('ul li:eq(1)')
    var li_txt = $li.text();
    ```
  * 查找属性节点:
    ```javascript
    var $para = $('p');
    var p_txt = $para.attr('title'); // 如果attr中只有一个参数那么就是读取
    ```
  * 创建节点:
    创建元素节点: 两个步骤，创建然后添加

    ```javascript
    $li = $('<li>apple</li>') // 创建一个DOM对象后然后包装成JQuery对象返回
    $('ul').append($li);
    ```
    我们在创建的时候可以把文本节点和属性节点都放进去一起创建。
  * 插入结点:
    动态创建HTML元素并没有实际用处，还需要将创建的元素插入文档中。将新创建的节点插入文档最简单的方法就是，让它成为这个文档的某个节点的子节点。前面使用了一个插入节点的方法`append()`, 他会在元素内部追加新创建的内容。
    插入节点的其他方法：
    ![插入结点的方法](http://mattycao.github.io/images/jquery-chapter3-1.png)
    ![插入结点的方法续表](http://mattycao.github.io/images/jquery-chapter3-2.png)
  * 删除节点

    ```javascript
    $('ul li:eq(1)').remove();
    ```
    **注意**：当用到`remove()`方法删除后，该节点所包含的所有后代节点将同时被删除。返回值是一个指向已经被删除的节点引用。所以我们可以使用它。
    ```javascript
    var $l1 = $('ul li:eq(1)').remove();
    $l1.appendTo('ul');
    $('ul li').remove('li[title!="apple"]'); // remove中可以传参数
    ```
    删除中还有`detach()`方法：但是这个方法不会把匹配的元素从JQuery对象中删除，因而可以在将来在使用这些匹配的元素。与`remove()`不同的是，所有绑定的时间和附件的数据等都会保留下来。
    ```javascript
    $('ul li').click(function() ){
        alert($(this).html());
    });
    var $li = $('ul li:eq(1)').detach();
    $li.appendTo('ul'); // the event is still there
    ```
    还有`empty()`方法：主要是清空节点，他能清空元素中的所有后代节点。
  * 复制节点
    `clone()`方法，注意被复制的新元素并不具有任何的行为。如果要新元素也具有复制功能，要传入true参数，`$(this).clone(true).appendTo('body');` 他会复制元素的同时复制元素中所绑定的事件。
  * 替换结点
    replaceWidth(), 和replaceAll(), 只有替换元素，而事件并没有被替换

    ```javascript
    $('p').replaceWith('<strong>item</strong?');
    $('<strong>item</strong?').replaceAll('p');
    ```
  * 包裹节点
    `wrap()`是将某个节点用其他的标记包裹起来。`wrapAll()`是将所有匹配的元素用一个元素包裹起来，和`wrap()`是有区别的，`wrap()`是单独包裹。而`wrapInner()`将每一个匹配的元素的字内容包括文本节点用其他结构化的标记包裹起来。

    ```javascript
    $('strong').wrap('<b></b>');
    $('strong').wrapAll('<b></b>'); // 如果被包裹的多个元素间有其他的元素，其他的元素会被放到包裹元素之后
    ```
  * 属性操作
    `attr()`方法来获取和设置元素属性，`removeAttr()`来删除元素属性

    ```javascript
    $para.attr('title');
    $.para.attr('title' : 'name', 'name': 'test'); // setting multiple attr
    ```
  * 删除属性
    `prop`，`removeProp()`
  * 样式操作
    * 获取样式和设置样式，` attr() `获取和设置
    * 追加样式 `addClass()`
    * 移除样式 `removeClass()`, 可以用空格方式移除多个class， `$li.removeClass("high another");`, 而当没有参数的时候，就是删除所有的样式
    * 切换样式 toggle(),控制行为上的重复切换， toggleClass(),方法控制样式上的重复切换。`$li.toggleClass('another')`添加和删除之间切换
    * 判断是否某个样式 `hasClass()`, 这个方法的提供只是为了增加可读性，其实内部的是现实使用到了is方法：`$('p').is('.another')`
    * 设置和获取HTML，文本和值
      * `html()`: innerHTML类似，读取或者设置摸个元素内的HTML内容
      * `text()`: 类似innerText，读取和设置某个元素内部的文本内容
      * `val()`: 类似value属性，用来设置和获取元素的值。无论是文本框，下拉列表还是单选框，都可以返回元素的值。如果为多选，那么返回有一个包含所有选择的值的数组。

      ```javascript
      $('#single').val('choose number 2');
      $('#multiple').val(['choose number 2'], ['choose number 3']);
      $(':checkbox').val(['check2', 'check3']);
      $(':radio').val(['radio2']); // notice: in the form, the checkbox and radio must have the value attribute.
      ```
#### 3.2.2 遍历节点
* `children()`方法: 取得匹配元素的子元素的集合。
* `next()`方法: 取得匹配元素后面紧邻的同辈元素。
* `prev()`方法: 取得匹配元素前面紧邻的同辈元素。
* `siblings()`方法: 取得匹配元素前后所有的同辈元素。
* `closest()`: 取得最近的匹配元素。检查当前元素是否匹配，如果没有匹配向上查找父元素直到找到位置，如果没有返回一个空JQuery对象。
* `parent, parents, closest`的方法们区别:
* 还有`find, filter，nextAll， prevAll`等

#### 3.2.3 CSS-DOM操作
* 方法css可以获取不管内联外联，内嵌的所有的样式。

  ```javascript
  $('p').css('color', 'red');
  $('p').css({'color': 'red', 'font-size':'14px'});  // 多重的css样式
  ```
  **Notice**：
  * 如果值是数字，那么自动转换成像素值。
  * css方法中，如果属性中有`-`符号，如果不带引号，我们要改成驼峰写法，例如`fontSize`。如果加了引号，两种方法都可以的。
  * 对于`opacity`的属性，JQuery已经做了兼容性的解决。
* height()返回获取元素的高度值px。如果用height设置的时候如果不是px值我们要传入字符串,例如`height('10em')`。**注意**：css方法获取的高度值和样式的设置有关，可能会得到`auto`等字符串，但是`height()`方法获取的高度值是元素在页面中的实际高度，与样式无关，并且不带单位，因为就是`px`。
* `width()`与`height()`的使用方法类似
* 关于元素定位的方法：
  * `offset()`: 获取元素当前视窗的相对偏移，返回两个属性，`top`和`left`，只是对可见元素有效果。
  * `position()`: 获取元素相对一个最近的`position`样式属性为`relative/absolute`的祖父节点的相对偏移，也是返回两个属性。
  * `scrollTop(), scrollLeft()`: 获取元素的滚动条距离顶端的距离和左测的距离。我们也可以写入数字来设置距离。

