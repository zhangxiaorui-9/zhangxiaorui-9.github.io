---
title: jQuery中的事件委托
date: 2016-12-23 18:11:09
categories: jQuery
tags: [事件委托]
---
## 事件绑定的两种方式
以click事件为例：
<!--more--> 
1. 普通绑定事件：
```
$('.btn1').click(function(){
    
})
```
2. on绑定事件：
```
$(document).on('click','.btn2',function(){
    
})
```
## 两种绑定方式的区别
1. click事件是在页面加载后，获取的所有类名为btn1的元素，然后绑定了这个click事件，要是通过其他操作再生成一个btn1元素，它就没有click这个事件；
2. 而on()事件起到了监听的效果,可以实现动态html元素绑定，比如一开始只有一个btn2元素，通过某种方法又加了一个btn2元素，这个元素也可以点击，可以无限添加btn2。  

举个栗子：
```
<ul>
    <li>我是第一个</li>
    <li>我是第二个</li>
</ul>
<button>添加</button>
```

```
$('button').click(function(){
    $('ul').append($('<li>我是被添加的</li>'));
})
```
通过点击'添加'按钮，我们在ul里动态添加了一个li，此时，通过click方式，新增加的li是没有点击事件的：
```
$('li').click(function(){
    console.log($(this).text());
})
```
而通过on方式给ul添加，我们后来添加的li也是有点击事件的：
```
$('ul').on('click','li',function(){
    console.log($(this).text());
})
```
## 事件委托原理
那么怎么实现这个动态监听的过程呢？  
on()事件相当于是:
```
$('ul').click(function(){
    if(点击的是li){
        
    }
});
```
给ul添加了一个click事件，当点击的是li，事件冒泡原理，从里到外，就相当于点击了ul，那么就会执行后面的操作，本质上只给ul添加了一个事件。
## 事件委托的好处
1. 原来的事件绑定，相当于给所有的li都要绑定事件，现在只需要给ul一个元素绑定事件，大大提高了效率和页面性能
2. 解决了动态添加的元素不能触发事件的问题。

注意：jQuery1.7+开始支持
## 参考资料
[jQuery里面的普通绑定事件和on委托事件](https://www.cnblogs.com/wufangfang/p/5333007.html)