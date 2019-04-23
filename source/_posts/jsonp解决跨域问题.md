---
title: jsonp解决跨域问题
date: 2017-08-13 10:32:07
categories: javaScript
tags: [跨域,jsonp]
---
![image](http://static.runoob.com/images/mix/BFD206B6-07E8-41B5-AF88-924CECFCA256.png)
<!--more-->  
jsonp名字听上去挺高大上，实际只要理解了就是个矮穷矬。
#### jsonp原理
利用script标签没有跨域限制的“漏洞”，来达到与第三方通讯的目的。 

第三方产生的响应为json数据的包装（故称之为jsonp，即json padding）。  
 
它的基本思想是，网页通过添加一个script元素，向服务器请求JSON数据，这种做法不受同源政策限制；服务器收到请求后，将数据放在一个指定名字的回调函数里传回来。  

首先，网页动态插入script元素，由它向跨源网址发出请求。  

```
function addScriptTag(src) {
  var script = document.createElement('script');
  script.setAttribute("type","text/javascript");
  script.src = src;
  document.body.appendChild(script);
}

window.onload = function () {
  addScriptTag('http://example.com/ip?callback=foo');
}

function foo(data) {
  console.log('Your public IP address is: ' + data.ip);
};
```
上面代码通过动态添加script元素，向服务器example.com发出请求。注意，该请求的查询字符串有一个callback参数，用来指定回调函数的名字，这对于JSONP是必需的。  
服务器收到这个请求以后，会将数据放在回调函数的参数位置返回。
```
foo({
  "ip": "8.8.8.8"
});
```
由于script元素请求的脚本，直接作为代码运行。这时，只要浏览器定义了foo函数，该函数就会立即调用。作为参数的JSON数据被视为JavaScript对象，而不是字符串，因此避免了使用JSON.parse的步骤。

#### 面试中的问题
1. 知道 jsonp么？  
答：知道，可以实现跨域请求；  
答不知道：换别的话题。
2. 为什么 ajax 不可以，但是 jsonp 可以实现跨域请求呢？  
答：因为 jsonp 是通过插入一个 script 标签，利用 script 可以跨域请求来实现的。换问题3；  
答：面试官傻逼，ajax 现在也可以使用 cors 来做跨域请求；换问题 2.5。  
答不知道：换问题 2.5。  
2.5  jsonp实现原理？  
答：通过创建一个 script 标签，将 src 设置为目标请求，插入到 dom 中，服务器接受该请求并返回数据，数据通常被包裹在回调钩子中；  
回答不知道：我自己解释 jsonp 的实现。  
3. 可以用 jsonp 发送 post 请求么？  
答：显然不行，看过支持 post 请求的 script 么？  
答不知道：反问，看过支持 post 请求的 script 么？  
4. 参考 jsonp，还有那些发送跨域请求的途径？  
答：img link iframe 等元素都可以发送跨域请求呀！  
答不知道：反问img link iframe 等元素是不是也可以？

#### 参考资料
[浏览器同源政策及其规避方法](http://www.ruanyifeng.com/blog/2016/04/same-origin-policy.html)  
[jsonp为什么不支持post请求，寸志的回答](https://www.zhihu.com/question/28890257/answer/269738446)