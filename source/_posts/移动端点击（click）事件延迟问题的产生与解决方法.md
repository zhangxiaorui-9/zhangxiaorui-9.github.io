---
title: 移动端点击（click）事件延迟问题的产生与解决方法
date: 2018-04-02 16:59:15
categories: javaScript
tags: [300ms延迟, fastclick]
---
### 为什么会存在300ms延迟？

移动浏览器为什么会设置300毫秒的等待时间呢？这与双击缩放的方案有关。平时我们可能已经注意到了，双击缩放，即用手指在屏幕上快速点击两次，可以看到内容或者图片放大，再次双击，浏览器会将网页缩放至原始比例。 
<!--more--> 

浏览器捕获第一次单击后，会先等待一段时间，如果在这段时间区间里用户未进行下一次点击，浏览器会做单击事件的处理。如果这段时间里用户进行了第二次单击操作，则浏览器会做缩放处理。这段时间就是移动端点击事件的300ms延迟。

### 如何避免延迟？
#### 方法一：禁止缩放

```
<meta name = "viewport" content="user-scalable=no" >
```

使用这个方法是通过完全禁用缩放来达到目的，虽然大部分移动端能解决这个延迟问题，但是部分苹果手机还是不行。
#### 方法二：fastclick.js
FastClick 是 FT Labs 专门为解决移动端浏览器 300 ms点击延迟问题所开发的一个轻量级的库。简而言之，FastClick 在检测到touchend事件的时候，会通过 DOM 自定义事件立即触发一个模拟click事件，并把浏览器在 300 ms之后真正触发的click事件阻止掉。使用方法如下：

第一步：在页面中引入fastclick.js文件；  

第二步：在js文件中添加以下代码：

```
// 原生js写法
if ('addEventListener' in document) {
    document.addEventListener('DOMContentLoaded', function() {
        // 参数可以是任意的dom元素，如果写document.body，说明会将document.body下面的所的元素都绑定fastclick
        FastClick.attach(documen.body)
    }, false);
}

// jQuery写法
$(function() {
    FastClick.attach(document.body);
});

// 在vue中的使用方法
// 安装
npm install fastclick -S

// 引入
import FastClick from 'fastclick'

// 使用
FastClick.attach(document.body);
```
