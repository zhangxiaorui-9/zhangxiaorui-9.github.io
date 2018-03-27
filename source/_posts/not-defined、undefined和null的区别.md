---
title: not defined、undefined和null的区别
date: 2017-02-24 14:11:40
categories: javaScript
tags: [null,undefined,not defined]
---
##### not defined  
意为没有定义的变量。  
例：
<!--more-->
```
<script type="text/javascript">
		console.log(m);
</script>
```

上述代码中m没有被定义，此时浏览器会报错
> Uncaught ReferenceError: m is not defined  

##### undefined  

undefined表示"缺少值"，就是此处应该有一个值，但是还没有定义。典型用法是：  
1. 变量被声明了，但没有赋值时，就等于undefined。
2. 调用函数时，应该提供的参数没有提供，该参数等于undefined。
3. 对象没有赋值的属性，该属性的值为undefined。
4. 函数没有返回值时，默认返回undefined。  

```
var i;
i // undefined

function f(x){console.log(x)}
f() // undefined

var  o = new Object();
o.p // undefined

var x = f();
x // undefined
```
上面四种情况执行结果一样：不会报错，会打印出undefined。  
##### null  
null表示 "没有对象"，即该处不应该有值。典型用法是：  
1. 作为函数的参数，表示该函数的参数不是对象。
2. 作为对象原型链的终点。  

```
Object.getPrototypeOf(Object.prototype)
// null
```
##### 需要注意  
这里需要注意的是：null == undefined结果是true的，null === undefined结果是false的。  

##### 参考资料
[阮一峰：undefined与null的区别](http://www.ruanyifeng.com/blog/2014/03/undefined-vs-null.html)
