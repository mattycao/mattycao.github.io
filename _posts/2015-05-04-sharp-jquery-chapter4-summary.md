---
layout:     post
title:      Sharp JQuery Chapter 4 Summary
date:       2015-05-04 18:04:00
summary:    Chapter 4 about JQuery Event and Animation!
categories: jquery
---

## Chapter 4 JQuery中的事件和动画

### 4.1 JQuery中的事件
#### 4.1.1 加载DOM
`$(document).ready()`与`window.onload`的区别：

* 执行时机: JQuery的方法会在DOM树就绪就开始加载事件，但是JS原生的会在所有的资源下载完成后才开始加载事件。但是有时候原生的也许更有必要，比如在没有加载图片时我们不知道图片的高度宽度这一类的属性，那么用第一个JQuery的方法我们就无法操作相关的方法，所以JQuery提供了一个和原生等价的方法:

  ```javascript
  $(window).load(function(){
  }
  ```
* 多次使用：JQuery的可以追加更多的行为，而原生的却不可以，只能覆盖之前的行为。
* 简写方式：以下的三种方法是等价的:

  ```javascript
  $(document).ready(function(){});
  $().readu(function(){});
  $(function(){});
  ```

#### 4.1.2 事件绑定
为元素绑定相关的某些操作，可以使用`bind()`方法来匹配元素进行特定的事件绑定:

```javascript
    bind(type [, data], fn);
```
* `type`: 类型包括：`blur, focus, load, scroll, unload, click, dblclick, mousedown, mouseup, mouseover, mousemove, mouseout, mouseenter, mouseleave, change, select, submit, keydown, keypress, keyup`, 和`error`等，当然可以自己定义。
* 第二个参数作为`event.dat`a属性值传递给事件对象的额外数据对象。
* 第三个参数则是用来绑定的处理函数。

**Notice:**
JQuery提供了事件的简写，`$(element).click`

#### 4.1.3 合成事件
* `hover()`方法：`hover(enter, leave)`, 在这里模拟光标悬停的事件，enter事件和leave事件
**Notice**: `hover`方法准确的说是替代了JQuery中的`mouseenter`和`mouseleave`。而不是`mouseover`和`mouseout`, 注意这两对事件是有区别的。
* `toggle()`方法：`toggle(f1, f2, ...)`, 模拟鼠标连续点击事件，第一次事件，如此类推，然后循环往复, 注意这个方法已经在1.8中被抛弃，后面的这个作用还在。此外，`toggle()`方法还有另外一个作用，就是切换元素的可见状态。

```javascript
    $(function(){
        $('.head').toggle(function() {
            //$(this).next().show();
            $(this).next().toggle();
        }, function() {
            $(this).next().toggle();
        }
    )};
```
#### 4.1.4 事件冒泡
事件冒泡从内到外，会导致额外的事件发生，我们在解决之前先提出几个介绍：
* 事件对象：

```javascript
    $('element').bind('click', function(event) {});
    // event here 是指事件对象，点击element的时候才会有event事件，事件函数处理完毕这个event被销毁，实际上jquery对这个event做了封装，因为event有一些兼容在里面
```
* 停止事件冒泡：`event.stopPropagation()` 停止事件冒泡，也做了兼容性的处理。
* 阻止默认行为：`event.preventDefault()`阻止元素的默认行为，比如提交。
* 如果相同时对事件对象停止冒泡和默认行为，可以在事件处理函数中返回false。

JQuery不支持事件捕获，所以要想使用捕获，那么需要使用原生的JS。

#### 4.1.5 事件对象属性
这个都是JQuery对事件对象的常用属性的封装，所以要与原生的区分开：
* event.type: 获取事件的类型
* event.preventDefault()
* event.stopPropagation()
* event.target: 获取到出发事件的元素，注意这里是元素，而不是JQuery对象，所以获取后可以通过JS属性来获取相关的属性。
* event.relatedTarget: JQuery对IE做了兼容性的处理
* event.pageX/event.pageY: 也做了兼容性处理，获取的是相对与页面的x坐标和y坐标
* event.which: 获取鼠标点击中获取到的鼠标的左中右键(`e.which == 1, 2, 3`)
* event.mateKey: boolean value， 说明当时事件是否点击了ctrl键( 进行了封装)

#### 4.1.6 移除事件
* 移除按钮元素上以前注册的事件

```javascript
    $('a').unbind(); // delete all event
    $('a').unbind('click'); // delete the click event
    $('a').unbind('click', fn1); // delete the fn1 in the click event
```
* 对于只需要触发一次，随后就要立即解除绑定的情况，JQuery提供了一种简单的写法，one()方法。这个绑定在运行一次后会立即删除。用法和bind一样。

#### 4.1.7 模拟操作
* 常用模拟: `trigger(type, [data])`， data为传入的数据，必须为数组。

```javascript
    $('#btn').trigger('click'); // 触发click事件
    $('#btn').click(); // 简写形式，与上面的等同
    $('#btn').trigger('myClick', [1, 2]); // 触发自定义的事件，传入数据
    $('input').triggerHandler('focus'); // 触发这个元素的绑定的特定事件，并且取消浏览器对此事件的默认操作，就是说在这里，文本框只是出发绑定的focus事件，而不会得到焦点，所以注意trigger和triggerHanlder的区别
```
#### 4.1.8 其他用法
* 绑定多个事件

```javascript
    $('div').bind('mouseover mouseout', function() {
        $(this).toggleClass('over');
    });
```
* 添加事件命名空间，便于管理

```javascript
    $('div').bind('click.plugin', function() {
        $(this).toggleClass('over');
    });
    $('div').bind('mouseover.plugin', function() {
        $(this).toggleClass('over');
    });
    $('div').unbind('.plugin'); // plugin命名的事件将被删除，但是没有plugin的事件不会被删除
    $('div').unbind('click').unbind('mouseover'); // 删除可以进行连式操作
```
* 相同的事件名称，不同的命名空间执行方法

```javascript
    $('div').bind('click', function() {
        $(this).toggleClass('over');
    });
    $('div').bind('click.plugin', function() {
        $(this).toggleClass('over');
    });
    $('div').trigger('click'); // 触发所有的click事件
    $('div').trigger('click!'); // 出发所有不包含在命名空间中的click方法
```

### 4.2 JQuery中的动画
#### 4.2.1 show()和hide()方法
* hide(): 这个方法相当于把css中的display属性改成none，但是在修改之前，会把当前的display属性记住，类似block, inline等属性，等留作show()使用
* show(): 相当于修改成恢复hide之前的display属性值。
* 这两个函数可以传入字符串或者数字为参数，字符串的参数有`slow(0.6s), normal(0.4s), fast(0.2s)`， 还可以传入数字，表示多少微秒，这样子可以产生动画效果，这个效果是在元素的高度，宽度，不透明度同时变化，应该是用到了jquery中的animate函数。

#### 4.2.2 fadeIn()和fadeOut()方法
只改变不透明度，直到元素的display为none。

#### 4.2.3 slideUp()和slideDown()方法
只改变高度，直到元素的display为none。

#### 4.2.4 自定义动画aminate()
`animate(params, speed, callback);`
* 自定义的简单动画

```javascript
    $(this).animate({left: '500px'}, 1000);
```
* 累加累减动画

```javascript
    $(this).animate({left: '+=500px'}, 1000);
```
* 多重动画

```javascript
    $(this).animate({left: '500px'， height: '500px'}, 1000); // 同时
     $(this).animate({left: '500px'}, 1000).animate({top: '500px'}, 1000); // 顺序执行
```

#### 4.2.5 动画的回调函数
动画的队列只有动画的相关方法才会排列进去，所以我们要完成类似不能用动画相关函数完成的动画我们需要用回调完成。

```javascript
    $(this).animate({left: '500px'}, 1000, function() {
        $(this).css('background-color', 'red');
    });
```

#### 4.2.6 停止动画和判断是否处于动画的状态
* 停止动画: `stop([clearQueue], [gotoEnd]);`, 两个参数都是boolean值，第一个代表是否要清空未执行的动画队列， 第二个参数代表是否直接将正在执行的动画跳转到末状态。**Notice**： 正在执行的动画

```javascript
    $(this).stop(true).anmiate({height: '22'}, 100).anmiate({left: '23'}， 110);
```
* 判断是否处于动画状态:
使用animate()需要避免动画积累而导致动画与用户的行为不一致，所以要判断是否处于动画状态

```javascript
    if(!$(this).is(':animated')) {
      $(this).animate({left: '500px'}, 1000);
    }
```
* 延迟动画

```javascript
    if(!$(this).is(':animated')) {
      $(this).animate({left: '500px'}, 1000).delay(100); // 延迟动画用delay方法
    }
```

#### 4.2.7 其他动画方法
* `toggle( speed, [callback])；` 切换元素的可见状态
* `slideToggle( speed, [ easing], [callback]);` 通过高度来切换元素的可见状态
* `fadeTo( speed, opacity, [ callback]);` 把不透明度通过渐进的方式实现
* `fadeToggle( speed, [ easing], [ callback]);`  通过不透明的变化来切换匹配元素的可见性

![动画方法的总结 1 ](http://mattycao.github.io/images/jquery-chapter4-1.png)
![动画方法的总结 2 ](http://mattycao.github.io/images/jquery-chapter4-2.png)
![动画方法的总结 3 ](http://mattycao.github.io/images/jquery-chapter4-3.png)
![动画方法的总结 4 ](http://mattycao.github.io/images/jquery-chapter4-4.png)