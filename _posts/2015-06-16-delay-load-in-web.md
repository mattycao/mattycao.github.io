---
layout:     post
title:      Delay Loading used in web Design
date:       2015-06-16 11:04:00
summary:    The theory about the delay loading
categories: Javascript
---

## Delay Loading used in web Design
Usually, we can see a lot of websites implementing a trick that some sessions will be loaded after the you scroll down to the end. This can be implemented in pure javascript. The key points are: `clientHeight, offsetHeight, offsetTop, scrollWidth, onscroll event`. Now let us dig in.

### Basic Concept
In the element DOM, it has a lot of attribute to access, here are some related to our project.
* `clientHeight, clientWidth`  
    it refers to the distance about the content inside the element and the padding.
* `clientLeft, clientTop`  
    it refers to the left border width and top border width
* `offsetHeight, offsetWidth`  
    it refers to the distance of the content inside the element, the padding, and the border. 
* `scrollTop, scrollLeft`  
    contains the part which is overflowed. The upper side of the padding in the overflow part to the upper side of the padding in the content. This is a value can be read and write.
* `scrollWidth, scrollHeight`  
    the height which includes the overflow parts, whether the element overflow is hidden will have influence on the height. 
    * it looks like if the overflow is hidden, then the up and down padding will be added with extra 1 pixel.(in chrome)
    * if the overflow is not hidden, just visible, by default, the downside of the padding will not be added.
* `offsetParent`  
    the offset reference part, it is the offsetParent

### The help Functions
* Method to get the CSS value in an element

```js
function getCss(ele, attr) {
    if (window.getComputedStyle) {
        return window.getComputedStyle(ele, null)[attr];
    } else {
        return ele.currentStyle[attr];
    }
}
```

* Method that get the offset to the body

```js
// try to get any element offset data
// in ie8, we don't need add the clientLeft, means the border
function offset(ele) {
    var p = ele.offsetParent;
    var l = ele.offsetLeft;
    var t = ele.offsetTop;
    while (p) {
        if (window.navigator.userAgent.indexOf('MSIE 8.0') > -1) {
            l += p.offsetLeft;
            t += p.offsetTop;
        } else {
            l += p.offsetLeft + p.clientLeft;
            t += p.offsetTop + p.clientTop;
        }
        p = p.offsetParent;
    }
    return {
        top: t,
        left: l
    }
}
```

### The basic theory about Delay Loading

* The distance between the body to the bottom side of the browser -> browser clientHeight + browser scrollTop
* The distance betweent the img load element to the body -> img offset + img offsetHeight
* When you scroll the body, the first distance will increase, then when the second distance is no more large than the frist one, then it means the img element is now showing in the browser, now it is the time to do the loading.
* What we listen is the window.onscroll event

```js
window.onscroll = function () {
        var oDiv = document.getElementById('div1');
        var n = (document.documentElement.clientHeight || document.body.clientHeight) + (document.documentElement.scrollTop || document.body.scrollTop);
        var m = utils.offset(oDiv).top + oDiv.offsetHeight;
        console.log(m + ': m, ' + n + ': n');
        if (m < n) {
            oDiv.style.background = 'green';
        }
    }
```

Now we know the basic theory, the implementation will be easy if we know the whole idea in the backend. The waterfall layout is a great implementation about delay loading. Next time, I will share some details about waterfall layout.

    
    