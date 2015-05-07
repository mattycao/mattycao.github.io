---
layout:     post
title:      Sharp JQuery Chapter 6 Summary
date:       2015-05-06 23:31:00
summary:    Chapter 6 about AJAX!
categories: jquery
---

## Chapter6 JQuery与AJAX的应用
### 6.1 AJAX的优势与不足
#### 6.1.1 优势
* 不需要插件支持
* 优秀的用户体验
* 提高web程序的性能
* 减轻服务器和带宽的负担

#### 6.1.2 不足
* 浏览器对于XMLHttpRequest对象的支持不足
* 破坏浏览器的前进，后腿按钮的正常工作
* 对SEO的支持不足
* 开发和调试的工具不足

### 6.2 JQuery中的AJAX
#### 6.2.1 load()方法

`load(url [, data] [, callback]);`

* `load()`方法是内部方法，所以JQuery对象可以操作
* `url`参数是`string`，里面的语法是 ` url selector`, 比如我们要载入`html`文件，可以加入`selector`来筛选`html`文档。
* 传递方式，如果我们没有传入`data Object`参数，那么我们用的就是`get`方法，如果添加了那么我们用的就是`post`方法。
* `callback(responseText, textStatus, XMLHttpRequest)`: 在加载完成后会调用，`textStatus`有`success, error, notmodifed, timeout`四种状态。注意，在这里`callback`无论`AJAX`请求是否成功，只要完成就会触发这个回调函数。对应的是`$.ajax()`里面的`complete()`函数。

#### 6.2.2 $.get()方法和$.post()方法
`load()`方法一般只是用来获取`web`中的静态数据文件，我们用的更多的是`get`和`post`这两个JQuery全局函数。

##### 6.2.2.1 $.get()
`$.get(url [, data] [, callback] [ , type]);`

![$.get()方法参数1](http://mattycao.github.io/images/jquery-chapter6-1.png)
![$.get()方法参数2](http://mattycao.github.io/images/jquery-chapter6-2.png)

* 其中的回调函数有两个参数，data和textStatus, textStatus和load中的一样，而且只有当数据成功返回success后才会被调用，这点与load()方法不一样。

##### 6.2.2.2 $.post()
Get请求和Post请求的区别：
* GET请求会把参数跟在URL后面进行传递，而POST请求则是作为HTTP消息的实体内容发送给服务器。
* GET方式对传输数据有大小限制，通常不能大于2KB，而POST理论上不受限制。
* GET方式请求的数据会被浏览器缓存起来，因此其他人就可以从浏览器的历史记录中读取到这些数据。所以GET的安全问题成问题。

在用法上两者差不多的。

#### 6.2.3 $.getScript(), getJson()
* `$.getScript()`: 加载js文件，可以用回调函数处理之后的使用。
* `jQuery.getJSON( url [, data ] [, success ] )`: `success`的定义: `Function( PlainObject data, String textStatus, jqXHR jqXHR ) `
    * 注意: 在JQuery1.5之后，`success`回调函数接受一个`jqXHR`，而不是`XMLHttpRequest object`。
    * 只有`textStatus`状态为`success`，才会调用这个函数
    * 注意接受的函数是`Object`，方便操作
    * `$.each()`: `A generic iterator function, which can be used to seamlessly iterate over both objects and arrays( includes array like object)`.
    * `jQuery.each( array, callback ), callback: Function( Integer indexInArray, Object value )`
    * `jQuery.each( object, callback ), callback:  Function( String propertyName, Object valueOfProperty )`
    * `.each(function):Iterate over a jQuery object, executing a function for each matched element. Function( Integer index, Element element )`
    * 注意以上两个`each`方法

#### 6.2.4 $.ajax()
为AJAX的底层函数，只有一个参数，包含了很多设置信息，都是可选的：
![$.ajax()中options参数1](http://mattycao.github.io/images/jquery-chapter6-3.png)
![$.ajax()中options参数2](http://mattycao.github.io/images/jquery-chapter6-4.png)

### 6.3 序列化元素
#### 6.3.1 serialize()方法
表单数据序列化，用法是:
`$('#form1').serialize();`
`serialize`方法会用到`encodeURIComponent()`去给字符编码(中文问题)。当然不光表单，类似这样子也可以: `$(':checkbox, :radio').serialize()`

#### 6.3.2 serializeArray()方法
将DOM元素序列化，返回JSON格式的数据。

#### 6.3.3 $.param()方法
是serialize的核心，用来对一个数组或者对象按照`key/value`进行序列化。

```javascript
    var k = $.param({a:1, b:2});
    alert(k); // a=1&b=2
```

#### 6.4 JQuery中的全局事件
* 有些方法是全局方法，不管创建他们的代码位于何处，只要AJAX请求发生时，就会触发他们。
* 可以通过设置`ajax`中的`option`中的`global`的`boolean`值来处理是否触发AJAX事件


![AJAX流程](http://mattycao.github.io/images/jquery-chapter6-5.png)