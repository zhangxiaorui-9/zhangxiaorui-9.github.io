---
title: js中的深拷贝和浅拷贝
date: 2017-03-01 15:18:23
categories: javaScript
tags: [深拷贝]
---
## 两者的概念和区别
对于基本数据类型(Undefined、Null、String、Number、Boolean)，浅拷贝是对值的复制，改变其中一个的值另一个不会跟着改变。<!--more-->对于对象来说，浅拷贝是对地址的复制，并没有开辟新的栈，也就是复制的结果是两个对象指向同一个地址，修改其中一个对象的属性，则另一个对象的属性也会改变；而深拷贝则是开辟新的栈，两个对象对应两个不同的地址，修改一个对象的属性，不会改变另一个对象的属性。
## 深拷贝的实现方式
关于深拷贝的实现，网上也有很多方式了。在此我总结了几种我用的比较多的。
### 方式一：递归拷贝

```
function cloneDeep(obj) {
    if (typeof obj !== 'object' || Object.keys(obj).length === 0) {
        return obj
    }
    let resultData = {}
    return recursion(obj, resultData)
}

function recursion(obj, data = {}) {
    for (key in obj) {
        if (typeof obj[key] == 'object' && Object.keys(obj[key].length > 0)) {
            data[key] = recursion(obj[key])
        } else {
            data[key] = obj[key]
        }
    }
    return data
}
let obj = {
    name: '小明',
    age: {
        child: 20
    }
}
let obj2 = cloneDeep(obj)
obj2.name = '小红'
obj2.age.child = 24
console.log(obj); // {name:'小明',age:{child:20}}
console.log(obj2);// {name:'小红',age:{child:24}}
```
### 方式二：jquery中$.extent方法
```
var obj03 = {
    a: 1,
    b: {
        f: {
            g: 1
        }
    },
    c: [1, 2, 3]
};
var obj04 = $.extend(true, {}, obj03);
obj04.b.f = {
    k : 2
}
console.log(obj03.b.f === obj04.b.f); // false
```
### 方式三：JSON.parse方法
```
var obj = {
    body: {
        a: 10
    }
};
var shallowClone = obj;
var deepClone = JSON.parse(JSON.stringify(obj));
console.log(shallowClone.body === obj.body);//true
console.log(deepClone.body === obj.body);//false
```
以上三种方式都可以实现对对象的深拷贝。

### 对数组的深拷贝

对数组的深拷贝可以使用下面这种方式：
```
var ary = ['one','two','three'];
var ary02 = ary.slice(0);
ary02.push('four');
console.log(ary);//['one','two','three']
console.log(ary02);//['one','two','three','four']
```

上面是个人的总结，如果错误，欢迎留言交流！