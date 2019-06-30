---
title: 玩转Sublime Text3(安装、卸载篇)
date: 2017-12-03 18:14:42
tags:
    - 工具
---
"工欲善其事，必先利其器"，作为前端开发，一款好的编辑器尤其重要，我尤其倾向与 Sublime Text3，首先介绍 它的安装与卸载
<!-- more -->

### 1.安装
   ```
   sudo add-apt-repository ppa:webupd8team/sublime-text-3
   sudo apt-get update
   sudo apt-get install sublime-text-installer
   ```

### 2.卸载
   ```
   sudo apt-get purge sublime-text-installer
   sudo apt-get autoremove
   ```
   或者
   ```
   sudo dpkg -P sublime-text
   sudo apt-get autoremove
   ```

   两种卸载方式在于当初如何安装sublime text 的，我是两个命令都执行后在执行安装的，它 已经在 Ubuntu16.04 上成功安装并为我工作了





