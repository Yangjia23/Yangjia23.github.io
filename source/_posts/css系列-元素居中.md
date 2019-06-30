---
title: CSS系列 -- 元素居中
date: 2018-10-17 22:50:19
thumbnail: /gallery/thumbnail/desert.png
toc: true
tags:
     - CSS  
---
css实现元素居中的方法有很多种，我们要根据具体的场景需求，选择合适的方法
<!-- more -->
#### 一、水平居中
  **1. 子元素为内联元素?**
  ```
    \\ html
    <div class="box">
        <span>HHHHHH</span>
    </div>

    \\ css
    .box{
        text-align: center;
    }
  ```
  **2. 子元素为块级元素？**
  ```
    \\ html
    <div class="parent">
        <div class="child">HHHHHH</div>
    </div>

    \\ css
    .child{
        width: 200px;
        margin: 0 auto;
    }
  ```
  通常子元素会有个宽度，不设宽度就是 100%；margin 也就无意义

  **3. 多个块级子元素？**

  ```
    \\ html
    <div class="parent">
        <div class="child">AAA</div>
        <div class="child">
            BBB
            BBB
            BBB
            BBB
        </div>
        <div class="child">
            CCC
            CCC
        </div>
    </div>

    \\ css
    .parent{
        background-color: #ccc;
        border: 1px solid @000;
        text-align: center;
    }

    .child{
        border: 1px solid red;
        margin: 5px;
        display: inline-block;
    }
  ```
  除了给子元素设置`display: inline-block`, 使用 `flex` 也可实现相同效果

  ```
    .parent{
        display: flex;
        justify-content: center;
    }
  ```

#### 二、垂直居中
  **1. 子元素为内联元素？**
  - 单行文本？
    ```
      \\ html
      <div class="parent">
        <a href="">Home</a>
        <a href="">About</a>
        <a href="">Blog</a>
      </div>

      \\ CSS
      .parent{
        background-color: #ccc;
        border: 1px solid @000;
        margin: 20px 0;
        padding: 50px;
      }
      .parent a{
        background: black;
        padding: 10px 20px;
        text-decoration: none;
      }
    ```
    元素上下添加相同的 `padding` 值，实际中使用更多的是设置 `line-height` 与 `heigtht` 等值
    <br>
      
    ```
      \\ html
      <div class="parent">
        <span>AAAAA</span>
      </div>

      \\ CSS
      .parent{
        height: 50px;
        line-height: 50px;
      }
      ```

- 多行文本？
    ```
      \\ html
      <div class="parent">
        <span>AAAAABBBBBCCCCC
        DDDDDEEEEEFFFFFGGGG</span>
      </div>

      \\ CSS
      .parent {
        display: table;
        height: 250px;
        background: white;
        width: 240px;
        margin: 20px;
      }
      .parent span {
        display: table-cell;
        background: black;
        color: white;
        vertical-align: middle;
      }
    ```

    table 子元素默认都是垂直居中，所以设置table默认样式也是实现垂直居中
    同样：`flex` 也能轻松完成此效果
    <br>
    ```
      .parent{
          display: flex;
          justify-content: center;
          flex-direction: column;
      }
    ```

**2. 子元素为块级元素？**
- 已知子元素height ？
    ```
      .parent{
          position: relative;
      }
      .child{
          position: absolute;
          width: 100px;
          height: 100px;
          top: 50%;
          margin-top: -50px;
      }
    ```
- 未知子元素height ?
    ```
      .parent{
          position: relative;
      }
      .child{
          position: absolute;
          top: 50%;
          transform: translateY(-50%);
      }
    ```
- 是否能使用 `flex`?
    ```
      .parent {
        display: flex;
        flex-direction: column;
        justify-content: center;
      }
    ```


#### 三、水平垂直居中
  **1. 已知宽高？**
  ```
    .parent {
        width: 300px;
        height: 300px;
        position: relative;
    }
    
    .child {
        position: absolute;
        width: 100px;
        height: 100px;
        top: 50%;
        left: 50%;
        margin-top: -50px;
        margin-left: -50px;
    }
   ```
  **2. 未知宽高？**
  ```
    .parent {
        width: 300px;
        height: 300px;
        position: relative;
    }
    
    .child {
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
    }
  ```

  **3. 使用flex ?**
  ```
    .parent {
        display: flex;
        justify-content: center;
        align-items: center;
    }
  ```

####四、总结
  `flex` 真好用，真香；能用 `flex` 就尽可以用， 同时flex 还可实现很多技巧，推荐阮一峰老师的 [flex教程](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html) 

前段时间又出现了个 `grid` 布局，还没尝过鲜...
