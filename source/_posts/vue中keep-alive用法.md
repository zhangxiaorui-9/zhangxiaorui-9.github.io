---
title: vue中keep-alive用法
date: 2018-08-05 18:17:37
categories: Vue
tags: [keep-alive]
---
## keep-alive 简介
**keep-alive** 是 Vue 内置的一个组件，可以使被包含的组件保留状态，或避免重新渲染。
<!--more-->
用法也很简单：
```
<keep-alive>
  <component>
    <!-- 该组件将被缓存！ -->
  </component>
</keep-alive>
```
### props
- include - 字符串或正则表达，只有匹配的组件会被缓存
- exclude - 字符串或正则表达式，任何匹配的组件都不会被缓存
```
// 组件 a
export default {
  name: 'a',
  data () {
    return {}
  }
}
```

```
<keep-alive include="a">
  <component>
    <!-- name 为 a 的组件将被缓存！ -->
  </component>
</keep-alive>
```

```
<keep-alive exclude="a">
  <component>
    <!-- 除了 name 为 a 的组件都将被缓存！ -->
  </component>
</keep-alive>
```
## 遇见 vue-router
router-view 也是一个组件，如果直接被包在 keep-alive 里面，所有路径匹配到的视图组件都会被缓存。
```
<keep-alive>
    <router-view>
        <!-- 所有路径匹配到的视图组件都会被缓存！ -->
    </router-view>
</keep-alive>
```
如果只想 router-view 里面某个组件被缓存，怎么办？
- 使用 include/exclude
- 增加 router.meta 属性

### 使用 include/exclude
```
// 组件 a
export default {
  name: 'a',
  data () {
    return {}
  }
}
```

```
<keep-alive include="a">
    <router-view>
        <!-- 只有路径匹配到的视图 a 组件会被缓存！ -->
    </router-view>
</keep-alive>
```
exclude 例子类似。

> 缺点：需要知道组件的 name，项目复杂的时候不是很好的选择

### 增加 router.meta 属性

```
// routes 配置
export default [
  {
    path: '/',
    name: 'home',
    component: Home,
    meta: {
      keepAlive: true // 需要被缓存
    }
  }, {
    path: '/:id',
    name: 'edit',
    component: Edit,
    meta: {
      keepAlive: false // 不需要被缓存
    }
  }
]
```

```
<keep-alive>
    <router-view v-if="$route.meta.keepAlive">
        <!-- 这里是会被缓存的视图组件，比如 Home！ -->
    </router-view>
</keep-alive>

<router-view v-if="!$route.meta.keepAlive">
    <!-- 这里是不被缓存的视图组件，比如 Edit！ -->
</router-view>
```

## 使用 router.meta 拓展
假设这里有 3 个路由： A、B、C。  

**需求：**

1. 默认显示 A
2. B 跳到 A，A 不刷新
3. C 跳到 A，A 刷新

在 A 路由里面设置 meta 属性：
```
{
path: '/',
name: 'A',
component: A,
meta: {
    keepAlive: true // 需要被缓存
}
}
```
在 B 组件里面设置 beforeRouteLeave：
```
export default {
    data() {
        return {};
    },
    methods: {},
    beforeRouteLeave(to, from, next) {
        // 设置下一个路由的 meta
        to.meta.keepAlive = true; // 让 A 缓存，即不刷新
        next();
    }
};
```
在 C 组件里面设置 beforeRouteLeave：
```
export default {
    data() {
        return {};
    },
    methods: {},
    beforeRouteLeave(to, from, next) {
        // 设置下一个路由的 meta
        to.meta.keepAlive = false; // 让 A 不缓存，即刷新
        next();
    }
};
```
这样便能实现 B 回到 A，A 不刷新；而 C 回到 A 则刷新。
## keep-alive 生命周期钩子函数

当引入keep-alive 的时候，页面第一次进入，钩子的触发顺序created-> mounted-> activated，退出时触发deactivated。当再次进入（前进或者后退）时，只触发activated。
## 总结

路由大法不错，不需要关心哪个页面跳转过来的，只要 router.go(-1) 就能回去，不需要额外参数。

## 参考资料
[vue-router 之 keep-alive](https://www.jianshu.com/p/0b0222954483)