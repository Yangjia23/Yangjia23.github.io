---
title: vue.js 中关于插槽的使用
date: 2019-07-28 08:04:57
tags:
    Vue
---
在工作中经常会使用 `slot` 插槽，但很多时候只停留在普通的用法，`Vue.js` 3.0 中关于 `slot` 的用法又有那些修改呢？

<!-- more -->
最近在写[Litchi-UI](https://yangjia23.github.io/litchi-ui/)组件库中 `radio` 组件时，有个需求就是用户可以自定义渲染的内容，默认显示的是`label`字段。
马上行动起来，一起探讨这个需求已经如何实现吧

## 1. 需求分析
首先看下`radio`组件已有的功能，源代码如下

```html
<template>
    <div>
        // 普通使用
        <lc-radio value="111" v-model="value">选项1</lc-radio>
        <lc-radio value="222" v-model="value">选项2</lc-radio>
        
        // 组合使用
        <lc-radio v-model="radio" :datas="source" ></lc-radio>
    </div>
</template>

<script>
    export default {
    data () {
        return {
            value: '111',
            radio: null,
            source: [
                {label: '选项1',value: 1},
                {label: '选项2',value: 2},
                {label: '选项3',value: 3}
            ]
        }
    }
}
</script>
```
与熟悉的UI库不同，在组合使用上，改成通过 `datas` 直接传入一个数组，代表着所有的单选项，每个单选项显示值和选中值的字段默认是 `label` 和 `value`, 当然用户可以通过 `valueKey` 和 `labelKey` 来自定义

在此基础上，提出一个新的需求，用户可以自定义展示内容(默认展示内容是`label`字段或`labelKey`制定字段的值)，展示内容由用户决定，自然会想到用插槽 `slot`，同时需要在插槽上绑定每个单选项的数据，这样才能进行自定义

## 2. 作用域插槽 `slot-scope`
插槽中显示的内容通常都是父组件决定的，但如果想要显示子组件中的数据，首先需要在子组件中，给 `slot` 绑定上自身数据， 然后在父组件中就可以使用到绑定的数据了

- 组件 `<lc-radio>`
    ```html
    <div v-for="(option,index) in datas" :key="index">
        <slot name="item" :option="option"></slot>
    </div>

    ```
- 使用
    ```html
    <lc-radio :datas="source">
        <template slot="item" slot-scope="slotProps">
            {{ slotProps.option.title }} - {{slotProps.option.author}}
        </template>
    </lc-radio>

    <script>
        ...
        source: {
            {title: '选项1',value: 1, author: '张三'},
            {title: '选项2',value: 2, author: '李四'},
            {title: '选项3',value: 3, author: '王五'}
        }
        ...
    </script>
    ```


使用 `slot-scope` 写完后，查看[slot文档](https://cn.vuejs.org/v2/guide/components-slots.html)发现，`slot` 和 `slot-scope` 俩特性竟然已经被废弃，<b> 在Vue.js 3.0 中不会出现 </b>，取而代之是 `v-slot` 指令，至于修改原因，请查看 [RFC](https://github.com/vuejs/rfcs/blob/master/active-rfcs/0001-new-slot-syntax.md)

## 3.`v-slot` 指令
定义一个 `<article-demo>` 组件
```html
<div class="container">
  <header>
    <slot name="header"></slot>
  </header>
  <main>
    <slot></slot>
  </main>
  <footer>
    <slot name="footer"></slot>
  </footer>
</div>
```

以 `v-slot` 参数的形式向每个具名插槽填充内容
```HTML
<article-demo>
  <template v-slot:header>
    <h1>Here might be a page title</h1>
  </template>

  <p>A paragraph for the main content.</p>
  <p>And another one.</p>

  <template v-slot:footer>
    <p>Here's some contact info</p>
  </template>
</article-demo>
```

当给插槽绑定数据时，可通过给 `v-slot` 一个值来获取绑定的数据，获取的数据通常是一个对象，默认命名成 `slotProps`， 可随意修改，所有，上面的 `radio` 的使用可修改成如下版本

```HTML
<lc-radio v-model="value6" :datas="sourceList4">
    <template v-slot:item="{option}">
        {{ slotProps.option.title }} - {{slotProps.option.author}}
    </template>
</lc-radio>
```

因为，在使用插槽时，是通过 `slotProps` 对象来获取绑定的数据，对象有多少属性不受限制，所有，我们可以给插槽绑定上任意值，最终都可通过 `slotProps` 对象来获取到

## 4.总结
以上就是本文全部内容了，欢迎大家关注 [Litchi-UI](https://yangjia23.github.io/litchi-ui/) 个人UI组件库



