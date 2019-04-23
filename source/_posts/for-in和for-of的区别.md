---
title: for in和for of的区别
date: 2017-08-29 16:17:15
categories: javaScript
tags: [for of, for in]
---
![image](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1555500528968&di=991e325c9d04c29d1d58a258272046f6&imgtype=0&src=http%3A%2F%2Fs3.lvjs.com.cn%2Fuploads%2Fpc%2Fplace2%2F2018-06-22%2F4a29a65b-245a-487d-b6c5-50ea8b82a725.jpg)
<!-- more -->
### 遍历数组
以前我们遍历数组通常使用for循环，例如：

```
var arr = ['apple', 'orange', 'banana']
for (var i = 0; i < arr.length; i++) {
    console.log(arr[i])
}
```

ES5的话也可以使用forEach，例如：

```
var arr = ['apple', 'orange', 'banana']
arr.forEach(function(val) {
    console.log(val)
})
```
但是使用forEach遍历数组的话，使用break不能中断循环，使用return也不能返回到外层函数。

ES5具有遍历数组功能的还有**map**、**filter**、**some**、**every**等，只不过他们的返回结果不一样。

使用for in也可以遍历数组，例如：

```
Array.prototype.method = function(){
　console.log(this.length)
}
var arr = ['apple', 'orange', 'banana']
arr.name = "数组"
for (var index in arr) {
  console.log(arr[index])
}
```
打印的结果：

```
apple
orange
banana
数组
ƒ (){
　console.log(this.length)
}
```
由打印的结果可以看出，使用for in会遍历数组所有的可枚举属性，包括原型。例如上面栗子中的原型方法method和name属性。   

除此之外，使用for in遍历数组还会存在以下问题：

1. index索引为字符串型数字，不能直接进行几何运算
2. 遍历顺序有可能不是按照实际数组的内部顺序

所以for in更适合遍历对象，不要使用for in遍历数组。

那么除了使用for循环，如何更简单的正确的遍历数组达到我们的期望呢（即不遍历method和name），ES6中的for of是个很好的选择。

```
Array.prototype.method = function(){
　console.log(this.length)
}
const arr = ['apple', 'orange', 'banana']
arr.name="数组";
for (var value of arr) {
  console.log(value);
}
// 'apple', 'orange', 'banana'
```
打印结果可以看出，for of遍历的只是数组内的元素，而不包括数组的原型属性method和索引name。

### 遍历对象
通常用for in来遍历对象的键名，例如：
```
Object.prototype.method = function() {
　console.log(this);
}
var myObject={
　a:1,
　b:2,
　c:3
}
for (var key in myObject) {
  console.log(key)
}
```
for in可以遍历到myObject的原型方法method。  
如果不想遍历原型方法和属性的话，可以在循环内部判断下,hasOwnPropery方法可以判断某属性是否是该对象的实例属性：
```
for (var key in myObject) {
　if（myObject.hasOwnProperty(key)){
　　console.log(key);
　}
}
```
或者可以通过ES5的Object.keys(myObject)获取对象的实例属性组成的数组，不包括原型方法和属性。

```
Object.keys(myObject).forEach(function(key) {
 console.log(myObject[key])
})
```
### 总结
1. for in遍历的是数组的索引（即键名），而for of遍历的是数组元素值。
2. for in更适合遍历对象，for of更适合遍历数组。