---
title: 玩转 Sublime Text3(常见问题篇)
date: 2017-12-04 19:31:11
tags:
    - 工具
---
最后一篇，记录一些使用过程中遇到的问题
<!-- more -->

### 1. 无法输入中文
   参考链接: [Ubuntu16.04下Sublime Text 3解决无法输入中文的方法](http://blog.csdn.net/Bleachswh/article/details/51674552)
   - 1. 保存下面的代码到文件sublime_imfix.c(位于~目录)
   ```
   #include <gtk/gtkimcontext.h>
	void gtk_im_context_set_client_window (GtkIMContext *context,
	          GdkWindow    *window)
	{
	  GtkIMContextClass *klass;
	  g_return_if_fail (GTK_IS_IM_CONTEXT (context));
	  klass = GTK_IM_CONTEXT_GET_CLASS (context);
	  if (klass->set_client_window)
	    klass->set_client_window (context, window);
	  g_object_set_data(G_OBJECT(context),"window",window);
	  if(!GDK_IS_WINDOW (window))
	    return;
	  int width = gdk_window_get_width(window);
	  int height = gdk_window_get_height(window);
	  if(width != 0 && height !=0)
	    gtk_im_context_focus_in(context);
	}
   ```
  - 2.若出现gtk/gtkimcontent.h没有那个文件或目录的错误,则输入以下命令后再编译
    ```
    sudo apt-get install libgtk2.0-dev
    ```
  - 3.将上一步的代码编译成共享库libsublime-imfix.so，命令
    ```
    gcc -shared -o libsublime-imfix.so sublime_imfix.c  `pkg-config --libs --cflags gtk+-2.0` -fPIC
    ```
  - 4.将libsublime-imfix.so拷贝到sublime_text所在文件夹
    ```
    sudo mv libsublime-imfix.so /opt/sublime_text/
    ```
  - 5.修改文件/usr/bin/subl的内容
      ```
      sudo gedit /usr/bin/subl
      ```
      将
      ```
      #!/bin/sh
      exec /opt/sublime_text/sublime_text "$@"
      ```
      修改为
      ```
      #!/bin/sh
      LD_PRELOAD=/opt/sublime_text/libsublime-imfix.so exec /opt/sublime_text/sublime_text "$@"
      ```
  - 6.为了使用鼠标右键打开文件时能够使用中文输入，还需要修改文件sublime_text.desktop的内容
      ```
      sudo gedit /usr/share/applications/sublime-text.desktop
      ```
      将[Desktop Entry]中的字符串
      ```
      Exec=/opt/sublime_text/sublime_text %F
      ```
      修改为
      ```
      Exec=bash -c "LD_PRELOAD=/opt/sublime_text/libsublime-imfix.so exec /opt/sublime_text/sublime_text %F"
      ```
      将[Desktop Action Window]中的字符串
      ```
      Exec=/opt/sublime_text/sublime_text -n
      ```
      修改为
      ```
      Exec=bash -c "LD_PRELOAD=/opt/sublime_text/libsublime-imfix.so exec /opt/sublime_text/sublime_text -n"
      ```
      将[Desktop Action Document]中的字符串
      ```
      Exec=/opt/sublime_text/sublime_text --command new_file
      ```
      修改为
      ```
      Exec=bash -c "LD_PRELOAD=/opt/sublime_text/libsublime-imfix.so exec /opt/sublime_text/sublime_text --command new_file"
      ```

### 2. 禁止自动更新提醒
   - 菜单：Preferences -> Settings-User（首选项->设置用户）
   - 保存一下代码
     ```
     { "update_check":false }
     ```
   - restart Sublime Text
   - 若未注册过 sublime Text,可能不管用
   - 搜索最新破解注册码(有能力还是得多支持正版) > help > License > restart

