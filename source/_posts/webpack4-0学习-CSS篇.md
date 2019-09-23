---
title: webpack4.0学习-CSS篇
date: 2019-09-23 21:19:34
thumbnail: /gallery/thumbnail/webpack_001.jpg
toc: true
tags:
    - Webpack
---
学习 Webpack4.0 并搭建一个业务项目基本配置

<!-- more -->
Webpack 对于前端开发者来说肯定不会陌生，在 React, Vue 等框架中的脚手架都支持 Webpack 的配置，用户不需要配置，直接可以进行开发。为了了解具体其配置原理，必须庖丁解牛，一探究竟。

## 一、CSS

### 1. 前缀自动补齐
  使用 `PostCSS` 插件， `autoprefixer` 实现

  - 安装
    ```bash
    yarn add postcss-loader autoprefixer -D
    ```
  - 配置
    ```js
    module.exports = {
      module: {
        rules: [
          {
            test: /\.less$/,
            use: [
              'style-loader',
              'css-loader',
              'less-loader',
              {
                loader: 'postcss-loader',
                options: {
                  plugins: () => [
                    require('autoprefixer')({
                      // 指定兼容的浏览器版本： 最近2个版本，使用率大于 1%，最低IOS7
                      browsers: ['last 2 version', '>1%', 'ios 7'] 
                    })
                  ]
                }
              }
            ]
          }
        ]
      }
    }
    ```
### 2. 移动端 `rem` 转换
  移动端使用 `rem` (font-szie of the root element) 的主要是为例让页面更好的适配不同尺寸的机型

  `px` 与 `rem` 的对比

  - `px` 绝对单位
  - `rem` 相对单位
  
  使用 `px2rem-loader` ，将 `px` 转换成 `rem`, 然后在页面渲染的时候，动态计算根元素的 `font-size` 的值， 也就知道 `1rem` 所代表的值

  - 使用手淘的 [lib-flexible](https://github.com/amfe/lib-flexible) 库
  - 安装
    ```
    yarn add px2rem-loader -D
    yarn add lib-flexible -S
    ```

  - `px2rem-loader`配置
  ```js
    module.exports = {
      module: {
        rules: [
          {
            test: /\.less$/,
            use: [
              'style-loader',
              'css-loader',
              'less-loader',
              {
                loader: 'px2rem-loader',
                options: {
                  remUnit: 75, // rem 相对与 px 转换单位,1rem === 75px，所以750px的设计稿也就是 10rem
                  remPrecision: 8, // 转化时小数点位数
                }
              }
            ]
          }
        ]
      }
    }
  ```


## 二、资源内联
  意义： 减少HTTP网络请求数， CSS 内联避免页面闪动
  
  - `raw-loader` 内联 html
    ```html
    <head>${require('raw-loader!babel-loader!./meta.html')}</head>
    ```
  - `raw-loader` 内联 JS
    ```html
    <script>${require('raw-loader!babel-loader!../node_modules/lib-flexible/flexible.js')}</script>
    ```
  - CSS 内联
    - 借助 `style-loader`
      ```js
      module.exports = {
        module: {
          rules: [
            {
              test: /\.less$/,
              use: [
                {
                  loader: 'style-loader',
                  options: {
                    insertAt: 'top', // 样式插入 head 中
                    singleton: true, // 将所有的 style 标签合并成一个
                  }
                },
                'css-loader',
                'less-loader',
              ]
            }
          ]
        }
      }
      ```

    - 使用 `html-inline-css-webpack-plugin`
