---
title: Vue.observable()使用方法
date: 2019-08-12 18:55:05
categories: Vue
tags: [observable]
---
### 前言
随着组件的细化，就会遇到多组件状态共享的情况， Vuex当然可以解决这类问题，不过就像 Vuex官方文档所说的，如果应用不够大，为避免代码繁琐冗余，最好不要使用它，今天我们介绍的是 vue.js 2.6 新增加的 Observable API ，通过使用这个 api 我们可以应对一些简单的跨组件数据状态共享的情况。
<!--more-->
### 用法
#### 第一步
首先创建一个 store.js，包含一个 store和一个 mutations，分别用来指向数据和处理方法。
```
//store.js
import Vue from 'vue';

export let store =Vue.observable({count:0,name:'李四'});
export let mutations={
    setCount(count){
        store.count=count;
    },
    changeName(name){
        store.name=name;
    }
}
```
#### 第二步
在组件xxx.vue中引用，在组件里使用数据和方法：

```
<template>
  <div id="app">
    <p>count:{{count}}</p>
    <p>name:{{name}}</p>
    <button @click="setCount(count+1)">+</button>
    <button @click="setCount(count-1)">-</button>
    <button @click="changeName(name2)">change</button>
  </div>
</template>

<script>

import { store, mutations } from "./store";

export default {
    name: "App",
    data() {
        return {
            name2: '改变后的name'
        }
    }
    computed: {
        count() {
            return store.count;
        },
        name() {
            return store.name
        }
    },
    methods: {
        setCount: mutations.setCount,
        changeName: mutations.changeName
    }
};
</script>
```
### 参考文章
[使用Vue.observable()进行状态管理](https://segmentfault.com/a/1190000019292569)