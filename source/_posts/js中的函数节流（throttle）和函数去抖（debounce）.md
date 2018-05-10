---
title: js中的函数节流（throttle）和函数去抖（debounce）
date: 2017-07-22 19:49:56
categories: javaScript
tags: [throttle,debounce]
---
## 前言
> 在js中，我们经常会遇到这种情况，需要监听页面的scroll事件或者鼠标的mousemove事件等。由于这些事件在鼠标移动的过程中会被浏览器频繁的触发，会导致对应的事件也会被频繁的触发，这样就会造成很大的浏览器资源开销，而且好多中间的处理是不必要的，这样就会造成浏览器卡顿的现象。我们无法做到让浏览器不去触发对应的事件，但是可以做到让处理事件的方法执行频率减少（throttle）或者在执行动作完成后执行一次（debounce），从而减少对应的处理开销。

<!--more-->

## throttle和debounce的区别

throttle指，在动作执行过程中，隔断时间触发一次事件，这样可以减少事件的方法执行的频率。debounce指只在动作结束后触发一次，把中间的处理函数全部过滤掉了，只执行规判定时间内的最后一个事件。

一般window的resize，input的keyup事件使用debounce；scroll、mousemove等事件使用throttle。

## throttle的实现

直接上代码  

{% codeblock lang:objc %}
//实现思路
//两个参数：一个对应的处理函数,一个执行的频率
//内部需要一个lastTime变量记录上一次执行的时间
function throttle (func, wait) {
   let lastTime = null　　　　// 为了避免每次调用lastTime都被清空，利用闭包返回一个function确保不声明全局变量也可以
   return function () {
    let now = new Date()
    // 如果上次执行的时间和这次触发的时间大于一个执行周期，则执行
    if (now - lastTime - wait > 0) {
     func()
     lastTime = now
    }
   }
  }
{% endcodeblock %}

接下来就是调用了

{% codeblock lang:objc %}
let throttleRun = throttle(() => {
  console.log(123)
}, 400);

window.addEventListener('scroll', throttleRun)
{% endcodeblock %}

这时候疯狂的滚动页面，会发现会400ms打印一个123，而没有节流的话会不断地打印。

但是到这里，我们的节流方法是不完善的，因为我们的方法没有获取事件发生时的this对象，而且由于我们的方法简单粗暴的通过判断这次触发的时间和上次执行时间的间隔来决定是否执行回调，这样就会造成最后一次触发无法执行，或者用户触发的间隔确实很短，也无法执行，造成了误杀，所以需要对方法进行完善。


{% codeblock lang:objc %}
 function throttle(func, wait) {
    let lastTime = null
    let timeout
    return function () {
        let context = this;
        let now = new Date();
        let arg = arguments;
        // 如果上次执行的时间和这次触发的时间大于一个执行周期，则执行
        if (now - lastTime - wait > 0) {
            // 如果之前有了定时任务则清除
            if (timeout) {
                clearTimeout(timeout)
                timeout = null
            }
            func.apply(context, arg)
            lastTime = now
        } else if (!timeout) {
            timeout = setTimeout(() => {
                // 改变执行上下文环境
                func.apply(context, arg)
            }, wait)
        }
    }
}
{% endcodeblock %}
这样我们的方法就相对完善了，调用方法和之前相同。

## debounce的实现

去抖的方法，和节流思路一致，但是只有在动作被判定结束后，方法才会得到执行。


{% codeblock lang:objc %}
function debounce(func, wait) {
    let lastTime = null;
    let timeout;
    return function () {
        let context = this;
        let now = new Date();
        let arg = arguments;
        // 判定不是一次抖动
        if (now - lastTime - wait > 0) {
            timeout = setTimeout(() => {
                func.apply(context, arg)
            }, wait)
        } else {
            if (timeout) {
                clearTimeout(timeout)
                timeout = null
            }
            timeout = setTimeout(() => {
                func.apply(context, arg)
            }, wait)
        }
        // 注意这里lastTime是上次的触发时间
        lastTime = now
    }
}
{% endcodeblock %}
调用方法和之前相同。

如有错误，欢迎指正！