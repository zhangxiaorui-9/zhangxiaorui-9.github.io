---
title: slice、substr和substring的区别
date: 2016-12-08 11:35:49
categories: javaScript
tags: [slice, substr, substring]
---
![image](http://static.runoob.com/images/mix/rfwDB3L.png)
<!--more-->
### 前言
我们经常需要截取字符串，slice(),substr()和substring()都是截取字符串的方法，那这三个方法如何使用，以及三种用法有何不同，今天就来整理一下

### 参数都为正数时
这三种方法都接收两个参数，slice()和substring()接收的是起始位置和结束位置(不包括结束位置)，而substr接收的则是起始位置和所要返回的字符串长度。看个例子：
```
var str = 'hello world';
console.log(str.slice(4,7)); //o w
console.log(str.substring(4,7)); //o w
console.log(str.substr(4,7)); //o world
```
这里有个需要注意的地方：substring是以两个参数中较小一个作为起始位置，较大的参数作为结束位置。
```
console.log(str.substring(7,4)); //o w
```
而slice如果第一个参数大于第二个参数，则返回空字符串。
```
console.log(str.slice(7,4)); //''
```
### 参数为负数时

> slice()会将所有的负数于字符串的长度相加;  
substr()会将第一个负参数与字符串长度相加，第二个负参数转化为 0;  
substring() 将所有的负参数转化为 0.

```
var str = 'hello world';  // length = 11
console.log(str.slice(-3));  // rld    slice(8)
console.log(str.substring(-3));  //hello world  substring(0)
console.log(str.substr(-3));  // rld  substr(8)
console.log(str.slice(3, -4));  //lo w slice(3, 7)
console.log(str.substring(3, -4)); //hel   substring(3, 0)
console.log(str.substr(3, -4)); //  ''   substr(3, 0)

```
