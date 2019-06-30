---
title: CSS系列 -- 布局整理
date: 2018-10-16 21:53:42
thumbnail: /gallery/thumbnail/desert.png
toc: true
tags:
     - CSS
     - 
---
css系列第一篇介绍 css布局，包括圣杯布局、双飞翼布局等
<!-- more -->
圣杯布局、双飞翼布局实现的都是**左右两栏固定宽度，中间部分自适应**

### **圣杯布局**

html 结构代码

 ```html
 <body>
    <header>Header</header>
    <main class="container">
        <section class="main"></section>
        <aside class="left"></aside>
        <aside class="right"></aside>
    </main>
    <footer></footer>
 </body>
 ```
 (注:) left,right 的宽度已知，main 内容自适应;
main 中间部分放前面，会优先加载

主要样式

```css
body{
  margin: 0;
  padding: 0;
  text-align: center;
}

header, footer{
  height: 50px;
  line-height: 50px;
  background-color: #acc6ef;
}

.left{
  width: 100px;
  background-color: #f254a8;
}

.right{
  width: 120px;
  background-color: #adf153;
}

.main{
  background-color: #f26343;
}
```
每部分设置背景色好区分，效果图如下
![](/images/20001.png)

设置 float、负margin

```css
.left{
  float: left;
}
.right{
  float: left;
}
.main{
  float: left;
  width: 100%;
}
```
main 设置 `float: left` 后，需添加 `width: 100%`，其宽度才能自适应，效果图如下：

![](/images/20002.png)

```css
.left{
    margin-left: -100%;
}

.right{
    margin-left: 120px;
}
```
![](/images/20003.png)

清除浮动、设置定位
设置 float、margin 后，中间部分脱离文档流， 导致 `footer` 上移，so:
```css
.footer{
    clear: both;
}
```

同时发现，左右部分盖住了中间的内容，所以给浮动元素的父元素 `container` 设置：

```css
.container{
    padding: 0 120px 0 100px;
}
```

此时效果图如下：
![](/images/20004.png)
需要把 `left、right ` 分别向左右拉

```css
.left{
  position: relative;
  left: -100px;
}
.right{
  position: relative;
  right: -120px;
}
```
(注：) `position: relative`： 元素相对于自身原本位置定位
最终效果图:
![](/images/20005.png)

[查看Demo](http://js.jirengu.com/qomob/7/edit?html,css,output)

---

### **双飞翼布局**
双飞翼布局在html结构上稍作调整：
```html
<main class="container">
    <section class="main">
        <div class="main-inner"></div>  
    </section>
    <aside class="left"></aside>
    <aside class="right"></aside>
</main>
```

在 `left, right` 遮挡住 `main` 内容时，给 `main` 设置 `.main-inner` 子元素，同时通过设置 margin、取消 left、right 的定位来达到效果


```css
.main-inner{
  margin: 0 120px 0 100px;
  background-color: #f26343;
  min-height: 200px;
}
```

[查看Demo](http://js.jirengu.com/pesex/1/edit)

