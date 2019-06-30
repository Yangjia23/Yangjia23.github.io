---
title: 玩转Sublime Text3(插件篇)
date: 2017-12-03 18:33:41
tags:
    - 工具
---
跟着 [安装篇]() 成功安装好，紧接着介绍 Sublime Text3 的插件篇
<!-- more -->
### 1. 插件管理器 [Package Control](https://packagecontrol.io/installation)
   - 打开Sublime Text 控制台， View > Show Console 或 Ctrl + `
   - 访问 [Package Control](https://packagecontrol.io/installation) 官网，复制最新代码到控制台，enter执行等待，重启
   - 快捷键 Ctrl + Shift + p

### 2. 主题 [ayu](https://github.com/dempfi/ayu)
   - 安装
     > 1.Press cmd/ctrl + shift + p to open the command palette.
     > 2.Type install package and press enter. Then search for ayu

   - 启动
     >  1.settings Preferences > Setting - User
     >  2.For light theme:
           ```
	       "theme": "ayu-light.sublime-theme",
	       "color_scheme": "Packages/ayu/ayu-light.tmTheme",
           ```
     >  3.For mirage theme:
           ```
           "theme": "ayu-mirage.sublime-theme",
           "color_scheme": "Packages/ayu/ayu-mirage.tmTheme",
           ```
     >  4.For dark theme:
           ```
           "theme": "ayu-dark.sublime-theme",
           "color_scheme": "Packages/ayu/ayu-dark.tmTheme",
           ```
### 3. Emment
   [使用Emmet加速Web前端开发](https://www.w3cplus.com/tools/using-emmet-speed-front-end-web-development.html)
### 4. 侧栏右键功能增强 [SideBarEnhancements](https://github.com/SideBarEnhancements-org/SideBarEnhancements)
### 5. 格式化代码 html-css-js Prettify
   - 快捷键 Ctrl + Shift + H
   - 可能会提示 (默认路径未找到 Node.js), answer: 安装node.js ,保持插件路径与node安装路径一致
   - 查看node.js 安装路径
      ```
      // linux ,mac
      which node
      // window
      where node
      ```

---
目前就安装了这么几个，以后还会继续添加




    

   
