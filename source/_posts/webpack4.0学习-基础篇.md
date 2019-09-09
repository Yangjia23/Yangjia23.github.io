---
title: 'Webpack4.0 学习 - 基础篇'
date: 2019-09-08 10:53:08
thumbnail: /gallery/thumbnail/webpack_001.jpg
toc: true
tags:
    - Webpack
---
学习 Webpack4.0 并搭建一个业务项目基本配置

<!-- more -->
Webpack 对于前端开发者来说肯定不会陌生，在 React, Vue 等框架中的脚手架都支持 Webpack 的配置，用户不需要配置，直接可以进行开发。为了了解具体其配置原理，必须庖丁解牛，一探究竟。

一、Webpack 介绍
Webpack 做为一个前端打包工具，其中最重要的概念就是模块。当 webpack 处理应用程序时，它会在内部构建一个依赖图(dependency graph)，此依赖图会映射项目所需的每个模块，并生成一个或多个 bundle

介绍 Webpack 中的一些核心概念，以下所有配置都是在项目根目录下的 `webpack.config.js` 文件中进行

- `entry`
    entry 指定入口文件，同时作为 Webpack 内部构建依赖图的开始。
    默认值`./src/index.js`, 但可以指定一个或多个入口

    ```javascript
    // 一个入口，String
    module.exports = {
        entry: './src/main.js'
    }
    // 多个入口, Object
    module.exports = {
        entry: {
            app: './src/app.js'
            adminApp: './src/adminApp.js'
        }
    }
    ```
- `output`
    output 控制 Webpack 如何向硬盘写入编译后的文件。
    即使存在多个`entry`配置，也只需要一个 `output` 配置

    - `filename`: 输出文件文件名
    - `path`: 输出文件路径

    ```javascript
    // 一个入口，String
    const path = require('path')

    module.exports = {
        entry: './src/main.js',
        output: {
            filename: 'bundle.js',
            path: path.resolve(__dirname, 'dist')
        }
    }
    // 使用 占位符 来确保每个文件具有唯一的名字
    module.exports = {
        entry: {
            app: './src/app.js'
            adminApp: './src/adminApp.js'
        },
        output: {
            filename: '[name].js',
            path: path.resolve(__dirname, 'dist')
        }
    }
    ```
- `mode`
    Webpack 根据 mode 的配置选项，使用相应环境的内置优化

    string | production (默认)，development, none
 
- `loader`
    111s
- `plugin`