---
title: 关于Vue中的计算属性computed
date: 2018-03-12 16:30:09
categories: Vue
tags: [Vue,computed]
---
#### 为什么要使用计算属性（computed）  
我们都知道，在Vue中，对于所有的数据绑定，Vue.js 都提供了完全的 JavaScript 表达式支持。  
看看代码:
<!--more-->
```
{{ number + 1 }}

{{ ok ? 'YES' : 'NO' }}

{{ message.split('').reverse().join('') }}

<div v-bind:id="'list-' + id"></div>
```
这些表达式会在所属 Vue 实例的数据作用域下作为 JavaScript 被解析。有个限制就是，每个绑定都只能包含单个表达式，所以下面的例子都不会生效。

```
<!-- 这是语句，不是表达式 -->

{{ var a = 1 }}

<!-- 流控制也不会生效，请使用三元表达式 -->

{{ if (ok) { return message } }}
```
模板内的表达式非常便利，但是设计它们的初衷是用于简单运算的。在模板中放入太多的逻辑会让模板过重且难以维护。例如：

```
<div id="example">
  {{ message.split('').reverse().join('') }}
</div>
```
在这个地方，模板不再是简单的声明式逻辑。你必须看一段时间才能意识到，这里是想要显示变量 message 的翻转字符串。当你想要在模板中多次引用此处的翻转字符串时，就会更加难以处理。  
所以，对于任何复杂逻辑，你都应当使用计算属性。一个简单的例子：
html：

```
<div id="example">
  <p>Original message: "{{ message }}"</p>
  <p>Computed reversed message: "{{ reversedMessage }}"</p>
</div>
```
js：

```
var vm = new Vue({
  el: '#example',
  data: {
    message: 'Hello'
  },
  computed: {
    // 计算属性的 getter
    reversedMessage: function () {
      // `this` 指向 vm 实例
      return this.message.split('').reverse().join('')
    }
  }
})
```
结果：
> Original message: "Hello"
> Computed reversed message: "olleH"

这里我们声明了一个计算属性 _reversedMessage_。我们提供的函数将用作属性 vm.reversedMessage的 _getter_ 函数：

```
console.log(vm.reversedMessage) // => 'olleH'
vm.message = 'Goodbye'
console.log(vm.reversedMessage) // => 'eybdooG'
```
你可以打开浏览器的控制台，自行修改例子中的 vm。vm.reversedMessage 的值始终取决于 vm.message 的值。 

你可以像绑定普通属性一样在模板中绑定计算属性。Vue 知道 vm.reversedMessage 依赖于 vm.message，因此当 vm.message 发生改变时，所有依赖 vm.reversedMessage 的绑定也会更新。而且最妙的是我们已经以声明的方式创建了这种依赖关系：计算属性的 _getter_ 函数是没有副作用 (side effect)的，这使它更易于测试和理解。
#### 计算属性(computed) 与方法（methods）的区别
你可能已经注意到我们可以通过在表达式中调用方法来达到同样的效果：

html：
```
<p>Reversed message: "{{ reversedMessage() }}"</p>
```

js：
```
// 在组件中
methods: {
  reversedMessage: function () {
    return this.message.split('').reverse().join('')
  }
}
```
我们可以将同一函数定义为一个方法（methods）而不是一个计算属性（computed）。两种方式的最终结果确实是完全相同的。然而， **不同的是计算属性是基于它们的依赖进行缓存的。计算属性只有在它的相关依赖发生改变时才会重新求值。** 这就意味着只要 message 还没有发生改变，多次访问 reversedMessage 计算属性会立即返回之前的计算结果，而**不必再次**执行函数。

这也同样意味着下面的计算属性将不再更新，因为 Date.now() 不是响应式依赖：
```
computed: {
  now: function () {
    return Date.now()
  }
}
```
相比之下，每当触发重新渲染时，调用方法（methods）将**总会**再次执行函数。

我们为什么需要缓存？假设我们有一个性能开销比较大的的计算属性 A，它需要遍历一个巨大的数组并做大量的计算。然后我们可能有其他的计算属性依赖于 A 。如果没有缓存，我们将不可避免的多次执行 A 的 _getter_！如果你不希望有缓存，请用方法（methods）来替代。  
#### 计算属性(computed)与侦听属性（watch）  
Vue 提供了一种更通用的方式来观察和响应 Vue 实例上的数据变动：侦听属性（watch）。当你有一些数据需要随着其它数据变动而变动时，你很容易滥用 watch——特别是如果你之前使用过 AngularJS。然而，通常更好的做法是使用计算属性（computed）而不是命令式的 watch 回调。细想一下这个例子：

html：
```
<div id="demo">{{ fullName }}</div>
```

js：
```
var vm = new Vue({
  el: '#demo',
  data: {
    firstName: 'Foo',
    lastName: 'Bar',
    fullName: 'Foo Bar'
  },
  watch: {
    firstName: function (val) {
      this.fullName = val + ' ' + this.lastName
    },
    lastName: function (val) {
      this.fullName = this.firstName + ' ' + val
    }
  }
})
```

上面代码是命令式且重复的。将它与计算属性（computed）的版本进行比较：
```
var vm = new Vue({
  el: '#demo',
  data: {
    firstName: 'Foo',
    lastName: 'Bar'
  },
  computed: {
    fullName: function () {
      return this.firstName + ' ' + this.lastName
    }
  }
})
```
是不是好了很多呢？
#### 计算属性的setter  
计算属性默认只有 _getter_ ，不过在需要时你也可以提供一个 _setter_ ：
```
// ...
computed: {
  fullName: {
    // getter
    get: function () {
      return this.firstName + ' ' + this.lastName
    },
    // setter
    set: function (newValue) {
      var names = newValue.split(' ')
      this.firstName = names[0]
      this.lastName = names[names.length - 1]
    }
  }
}
// ...
```
现在再运行 vm.fullName = 'John Doe' 时，_setter_ 会被调用，vm.firstName 和 vm.lastName 也会相应地被更新。


#### 参考资料  
[vue官网](https://cn.vuejs.org/)
