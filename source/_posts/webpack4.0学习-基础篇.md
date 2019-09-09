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

## 一、Webpack 介绍

Webpack 做为一个前端打包工具，其中最重要的概念就是模块。当 webpack 处理应用程序时，它会在内部构建一个依赖图(dependency graph)，此依赖图会映射项目所需的每个模块，并生成一个或多个 bundle

介绍 Webpack 中的一些核心概念，以下所有配置都是在项目根目录下的 `webpack.config.js` 文件中进行

## 1. entry

entry 指定入口文件，同时作为 Webpack 内部构建依赖图的开始。默认值`./src/index.js`, 但可以指定一个或多个入口

- 单入口   `String`

   ```javascript
   module.exports = {
     entry: './src/main.js'
   }
   ```

- 多入口   `Object`

    ```javascript
        module.exports = {
            entry: {
                app: './src/app.js'
                adminApp: './src/adminApp.js'
            }
        }
    ```

## 2. output
output 控制 Webpack 如何向硬盘写入编译后的文件。即使存在多个`entry`配置，也只需要一个 `output` 配置

  `filename`: 输出文件文件名
  `path`: 输出文件路径

- 单入口
```javascript
const path = require('path')

module.exports = {
    entry: './src/main.js',
    output: {
        filename: 'bundle.js',
        path: path.resolve(__dirname, 'dist')
    }
}
```

- 多入口， 使用占位符来确保每个文件具有唯一的名字
```javascript
const path = require('path')
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
## 3. mode

Webpack 根据 mode 的配置选项，使用相应环境的内置优化。
string | production (默认)，development, none
 
## 4. loader
Webpack 默认支持JS和JSON俩种文件类型，通过 loader 去支持其它文件类型并将它们转换成有效的模块，并且添加到依赖图中

loader 本身是一个函数，源文件作为参数，返回转换后的结果

```javascript
module.exports = {
    module: {
        rules: [
            {
                test: /\.txt$/, // 匹配规则
                use: 'row-loader', // 指定使用的loader名称
            }
        ]
    }
}
```

## 5. plugin
plugin 用于`bundle`文件的优化，资源管理和环境变量注入等。plugin 作用于整个构建过程

## 二、常用 loader

## 1. js
使用 `babel` 转译 `JavaScript` 文件
- 安装
    ```
    yarn add -D babel-loader @babel/core @babel/preset-env
    ```
    使用 `babel-loader` 来转换 `JS`, 该loader 依赖于 `babel`, 同时为了使用到最新的`JavaScript`, 所以还需要安装 `@babel/core`， `@babel/preset-env`


- Webpack 配置

    ```javascript
    module.exports = {
        module: {
            rules: [
                {
                    test: /\.js$/,
                    use: 'babel-loader'
                }
            ]
        }
    }
    ```
- \.babelrc 文件配置
    ```JSON
    {
        "presets": [ 
            "@babel/preset-env",
        ]
    }
    ```
  
  
## 2. jsx
jsx 作为 js 的扩展，需要通过 babel 进行转换，需要安装 `@babel/preset-react`

- 安装
    ```
    yarn add -D @babel/preset-react
    ```
- \.babelrc 文件配置
    ```JSON
    {
        "presets": [
            "@babel/preset-react",
        ]
    }
    ```

## 3. css 及 less
以 `less` 文件为例，探讨 webpack 配置

- 安装
    ```
    yarn add -D less less-loader css-loader style-loader
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
                        'less-loader'
                    ]
                }
            ]
        }
    }
    ```

    `loader` 从右到左进行取值/执行，以上面demo为例，主要步骤如下:
    - 从 `less-loader` 开始执行，将less文件转换成 css 文件
    - 然后再执行 `css-loader`, 处理 css 文件中的 `imports` 与 `url(...)`
    - 最后执行 `style-loader`, 将样式通过 `<style>` 标签插入到页面的`head`中 


## 4. 静态资源
静态资源主要是图片、字体等，常用的loader有 `file-loader` 与 `url-loader`, 二者区别如下：
- `file-loader`: 将文件复制到 `dist` 或制定文件夹，在原本包含图片的地方插入新的图片url
- `url-loader`:  `url-loader` 封装了 `file-loader`, 但不依赖 `file-loader`, 同时，`file-loader`可通过`limit`选项设置字节限制，当文件大小小于限制时，则返回 `DataURL`,也就是`base64`转码

以 `url-loader` 处理图片为例，探究webpack如何处理静态资源
- 安装
   ```
   yarn add -D url-loader
   ```

- 配置
    ```js
    module.exports = {
        module: {
            rules: [
                {
                    test: /\.(png|jpe?g|gif|svg)$/,
                    use: {
                        loader: 'url-loader',
                        options: {
                            limit: 8192
                        }
                    }
                }
            ]
        }
    }
    ```

## 三、总结
以上是 Webpack 的常用基础配置，接下来，会继续探讨Webpack 中的文件监听、热更新及其原理等等，继续加油
