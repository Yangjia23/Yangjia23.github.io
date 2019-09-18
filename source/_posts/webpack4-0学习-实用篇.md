---
title: webpack4.0学习-实用篇
date: 2019-09-17 22:53:05
thumbnail: /gallery/thumbnail/webpack_001.jpg
toc: true
tags:
    - Webpack
---
学习 Webpack4.0 并搭建一个业务项目基本配置

<!-- more -->
Webpack 对于前端开发者来说肯定不会陌生，在 React, Vue 等框架中的脚手架都支持 Webpack 的配置，用户不需要配置，直接可以进行开发。为了了解具体其配置原理，必须庖丁解牛，一探究竟。

## 一、文件监听
发现源码发生变化时，自动重新构建出新的输出文件

## 1. 半自动
- 启动 webpack 命令时，带上 `--watch` 参数
- 在配置 `webpack.config.js` 中设置 `watch:true`

缺点： 每次构建完成，需要手动刷新浏览器

原理： webpack 轮询判断文件的最后修改时间是否发生变化，某个文件发生变化后，并不会立即进行打包， 而是先缓存起来， 等待 `aggregateTimeout` 时间内，将所有更改的文件一起进行打包

```js
module.exports = {
    watch: true,
    watchOptions: {
        ignore: '/node_modules/', // 不监听的文件或文件夹，默认空
        aggregateTimeout: 300, // 监听到发生变化后，会等 300ms 再去执行， 默认300ms
        poll: 1000, // 轮询频率， 默认每秒问 1000 次
    }
}

```
## 二、热更新

## 1. webpack-dev-serve

WDS 需要结合 `HotModuleReplacementPlugin` 插件使用

优点： 
  - 不用手动刷新浏览器
  - 不输出文件，而是放到内存中
  

## 2. webpack-dev-middleware

WDM 将 webpack 输出的文件传输给服务器

[参考demo]()

## 3. 原理

[]img

每个　Server 讲解。。。。

俩个阶段： 
 - 启动阶段：　XXX
 - 更新阶段：  XXX

## 三、文件指纹

打包输出文件名的后缀

### 1. 生成
- **Hash**: 和整个项目的构建关联，只有项目文件有修改，整个项目构建的　hash 值就会变化
- **Chunkhash**:　和　webpack 打包的　`chunk` 相关，　不同的　`entry` 生成不同的　`chunkhash` 值，　常用于 JS 文件
- **Contenthash**:　和文件的内容相关，内容不变，`contenthash` 则不变，　常用于　CSS 文件

### 2. 使用
- JS 的文件指纹, 设置　`output` 的　`filename`
    ```js
    module.exports = {
        entry: {
            app: './src/app.js'
            adminApp: './src/adminApp.js'
        },
        output: {
            filename: '[name][chunkhash:8].js'
            path: __dirname + '/dist'
        }
    }
    ```

- CSS 的文件指纹
上篇文章中，　webpack 对CSS 文件的处理，最终都是通过`<style>` 标签插入到页面中，并没有生成单独的CSS文件，　所有，我们需要使用`MiniCssExtractPlugin`插件，生成单独　CSS 文件，而文件指纹，只需设置　`MiniCssExtractPlugin` 的　`filename`, 使用 `[contenthash]`

    ```js
    module.exports = {
        entry: {
            app: './src/app.js'
            adminApp: './src/adminApp.js'
        },
        output: {
            filename: '[name][chunkhash:8].js'
            path: __dirname + '/dist'
        },
        plugins: [
            new MiniCssExtractPlugin({
                filename: '[name][contenthash:8].css'
            })
        ]
    }
    ```

- 图片的文件指纹
设置 `file-loader` 或　`url-loader` 的 name, 使用　`[hash]` 占位符
md5 占位符默认32 位

## 四、代码压缩
- JS 文件
    
    内置了 `uglifyjs-webpack-plugin`,  在 `production` 模式自动开启

- CSS 文件
    
    `css-loader` 在 1.0 版本之前可设置 `minimize` 参数为 `true` 即可

    现在，需要使用 `optimize-css-assets-webpack-plugin` 插件，同时配合使用 `cssnano` 工具压缩 CSS 代码

    ```js
    module.exports = {
        plugins: [
            new OptimizeCssAssetsPlugin({
                assetNameRegExp: /\.css$/g,
                cssProcessor: require('cssnano')
            })
        ]
    }
    ```
- HTML 文件
  
  使用 `html-webpack-plugin` 插件，设置压缩参数
  ```js
  module.exports = {
    plugins: [
      new HtmlWebpackPlugin({
        // ...
        minify: {
          html5: true, // 以html5的文档格式解析 html 的模板文件
          collapseWhitespace: true, //删除空白符与换行符
          preserveLineBreaks: false, // 删除换行
          minifyCSS: true, // 仅压缩内联 css
          minifyJS: true, // 仅压缩内联 js
          removeComments: false, // 移除注释
        }
      })
    ]
  }
  ```

## 五、自动清理构建目录

使用 `clean-webpack-plugin` 插件，避免每次构建前手动删除 dist, 默认会删除 `output` 指定的输出目录

```js
  module.exports = {
    plugins: [
      new CleanWebpackPlugin()
    ]
  }
```



