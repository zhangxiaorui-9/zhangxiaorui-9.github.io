---
title: flatten（扁平化）数组
date: 2018-11-15 9:32:15
categories: javaScript
tags: [flatten, 扁平化]
---
![image](http://ppvq158m9.bkt.clouddn.com/bg02.png)
<!-- more -->
### 前言
顾名思义，扁平化就是将嵌套的多维数组变成一维数组的过程。今天将通过几种方式来实现数组的扁平化。

先定义这几种方法公用的一个数组

```
const arr = [1, 2, [3, 4, [5, 6]]]
```

### 初级版
初级版也通过两种方式来实现
#### 第一种
利用数组的concat方法

```
function flatten(arr) {
  let res = []  
  arr.map(element => {
    if(Array.isArray(element)) {
      res = res.concat(flatten(element))
    }
    else{
      res.push(element)
    }
  })
  return res
}
```
#### 第二种
使用参数默认值的方法

```
function flatten(arr, res = []) {
  for (let item of arr) {
    if (Array.isArray(item)) {
      res = flatten03(item, res)      
    }
    else{
      res.push(item)        
    }
  }
  return res
}
```
### 进阶版
使用...三点运算符结合concat方法

```
function flatten(arr) {
  while(arr.some(item => Array.isArray(item))) {
    arr = [].concat(...arr)
  }
  return arr
}
```
### 终极版
Array.flat(n)是ES10扁平数组的api， n表示维度， n值为 Infinity时维度为无限大。  
我们的例子中是个三维数组，所以，写法为：
```
arr.flat(3)
```
