---
title: vue中的修饰符sync
date: 2018-04-10 14:58:33
categories: Vue
tags: [sync修饰符]
---
## 为什么要使用sync
在有些情况下，我们可能需要对一个 prop 进行“双向绑定”。不幸的是，真正的双向绑定会带来维护上的问题，因为子组件可以修改父组件，且在父组件和子组件都没有明显的改动来源。 
<!--more-->
.sync 修饰符所提供的功能。当一个子组件改变了一个 prop 的值时，这个变化也会同步到父组件中所绑定。就是说我们可以直接在我们需要传的prop后面加上 .sync

## 使用方法和demo

比如我需要绑定 mes,然后我在他后面加上.sync。
{% codeblock lang:objc %}
<son :mes.sync="message"></son>
{% endcodeblock %}
他会扩展成：
{% codeblock lang:objc %}
 <son :mes="message" @update:mes="val => message= val"></son>
{% endcodeblock %}
举个栗子：

父组件代码：
{% codeblock lang:objc %}
<template>
 <div>
   <son :mes.sync='message'>
   </son>
</div>
</template>
<script>
import Son from './Son.vue'
export default {
    name: 'fathor',
    components: {
        Son
    },
    data() {
      return {
            message: '我是父组件的数据'
    }     
}
</script>
{% endcodeblock %}

子组件代码：

{% codeblock lang:objc %}
<template>
    <div>
      <input type="text" v-model="mes"/>
    </div>
</template>
<script>
    export default{
        name:'son',
        props:{
            mes: String
        },
        watch:{
            mes(newValue){
                this.$emit('update:mes',newValue)
            }
        }
    }
</script>
{% endcodeblock %}

##  与自定义事件的区别

看完上面的代码，你可能觉得这好像和“通过自定义事件（emit）从子组件向父组件中传递数据”是一样的。

其实并不一样， 两者有着父子组件关系上的不同， 下面我通过一行关键的代码证明它们的区别所在：

在我们使用.sync修饰符中， 自定义事件发生时候运行的响应表达式是：

{% codeblock lang:objc %}
 <son :mes="message" @update:mes="val => message= val"></son>
{% endcodeblock %}
在“通过自定义事件从子组件向父组件中传递数据” 里，自定义事件发生时候运行的响应表达式是:


{% codeblock lang:objc %}
<son @eventYouDefined = "arg => functionYours(arg)"></son>
{% endcodeblock %}
对前者， 表达式 val => message = val意味着强制让父组件的数据等于子组件传递过来的数据， 这个时候，我们发现父子组件的地位是平等的。 父可以改变子（数据）， 子也可以改变父（数据）。

对后者， 你的functionYours是在父组件中定义的， 在这个函数里， 你可以对从子组件接受来的arg数据做任意的操作或处理， 决定权完全落在父组件中， 也就是：  父可以改变子（数据）， 但子不能直接改变父（数据）。 父中数据的变动只能由它自己决定。

## 参考资料
[Vue使用.sync 实现父子组件的双向绑定数据](https://www.jianshu.com/p/bf3bc4a9cd0d)
[Vue中的父子组件通讯以及使用sync同步父子组件数据](https://www.cnblogs.com/penghuwan/p/7473375.html#_label0_2)