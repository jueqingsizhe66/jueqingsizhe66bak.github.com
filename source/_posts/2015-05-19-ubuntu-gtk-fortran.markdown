---
layout: post
title: "Ubuntu gtk-fortran"
date: 2015-05-19 17:03:25 +0800
comments: true
categories: fortran
---


Fortran：一门古老的[计算机数值科学计算语言](http://micro.ustc.edu.cn/Fortran/ZJDing/ ),1950.

Fortran的强项就是数值计算，图形编程是一个鸡肋所在，但是有时候又需要做一些图形的展示，借助于[GTK+ project](http://www.gtk.org/)可以方便我们达到目的.

GTK是c语言编写的源程序，运用到fortran需要相应的库转换,在github上找到一个[gtk-fortran库]( https://github.com/jueqingsizhe66/gtk-fortran).下面是ubuntu上面的搭建过程(windows 未实现,可以参考 gtk+makefile)
<!--more-->

## Ubuntu 搭建
### 环境搭建
GTK3+是跨平台的，既可以在windows上使用，也可以在linux上使用
下面的代码可以直接打包进一个gtk.sh文件，直接运行sh gtk.sh进行安装
``` sh
#1 刚装好的Ubuntu系统中已经有GCC了，但是这个GCC几乎什么文件都不能编译，因为缺少一些必须的头文件，所以要安装build-essential这个软件包
sudo apt-get install build-essential
#2 安装GTK/GNOME开发环境  需要下载一系列的安装包，时间比较长
sudo apt-get install gnome-devel gnome-devel-docs 
#3 用于在编译GTK程序时自动找出头文件及库文件位置　　
sudo apt-get install pkg-config
#4安装 devhelp GTK文档查看程序 
sudo apt-get install devhelp
#5安装 gtk/glib 的API参考手册及其它帮助文档 
sudo apt-get install libglib2.0-doc libgtk2.0-doc
#6  安装基于GTK的界面GTK是开发Gnome窗口的c/c++语言图形库
sudo apt-get install glade libglade2-dev
##或者sudo apt-get install glade-gnome glade-common glade-doc 
#7安装gtk3.0 或者 将gtk+3.0所需的所有文件统通下载安装完毕 (安装时间较长)
sudo apt-get install libgtk3*

## 查看信息
# 查看 2.x 版本 
#pkg-config --modversion gtk+-3.0 
# 查看pkg-config的版本
#$pkg-config -version 
#查看是否安装了gtk 
#pkg-config --list-all | grep gtk 
```

这个流程做完了，就可以进行gtk测试,当然仅仅是c语言的测试
### 测试文件
``` c
//Helloworld.c

#include <gtk/gtk.h>  

int main(int argc,char *argv[])  
{  
    GtkWidget    *window;  
    GtkWidget    *label;  

    gtk_init(&argc,&argv);  

    /* create the main, top level, window */  
    window = gtk_window_new(GTK_WINDOW_TOPLEVEL);  

    /* give it the title */  
    gtk_window_set_title(GTK_WINDOW(window),"Hello World");  

    /* connect the destroy signal of the window to gtk_main_quit 
     * when the window is about to be destroyed we get a notification and 
     * stop the main GTK+ loop 
     */  
    g_signal_connect(window,"destroy",G_CALLBACK(gtk_main_quit),NULL);  

    /* create the "Hello, World" label */  
    label = gtk_label_new("GTK可以运行了！");  

    /* and insert it into the main window */  
    gtk_container_add(GTK_CONTAINER(window),label);  

    /* make sure that everything, window and label, are visible */  
    gtk_widget_show_all(window);  

    /* start the main loop, and let it rest until the application is closed */  
    gtk_main();  

    return 0;  
}

```
### 编译
``` sh
gcc -o Helloworld Helloworld.c `pkg-config --cflags --libs gtk+-3.0`
```
### 运行
``` sh
./Helloworld

```

如果你得到一个小窗口"GTK可以运行了" 证明你的gtk环境是正确的。

然后我们开始进一步配置gtk-fortran

1. 下载
[https://github.com/jueqingsizhe66/gtk-fortran]( https://github.com/jueqingsizhe66/gtk-fortran)
2. 编译 

``` sh
mkdir build
cd build

在gtk-fortran目录下：/home/happycamp-of-fortran/gtk-fortran/build
cmake ..
make 
make install

```

会得到如下结果:
``` sh
Install the project...
-- Install configuration: "release"
-- Installing: /usr/local/lib/libgtk-2-fortran.a
-- Installing: /usr/local/lib/libgtk-2-fortran.so.0.1
-- Installing: /usr/local/lib/libgtk-2-fortran.so
-- Installing: /usr/local/include/gtk-2-fortran/atk.mod
-- Installing: /usr/local/include/gtk-2-fortran/cairo.mod
-- Installing: /usr/local/include/gtk-2-fortran/gdk.mod
-- Installing: /usr/local/include/gtk-2-fortran/gdk_pixbuf.mod
-- Installing: /usr/local/include/gtk-2-fortran/g.mod
-- Installing: /usr/local/include/gtk-2-fortran/gtk.mod
-- Installing: /usr/local/include/gtk-2-fortran/gtk_hl.mod
-- Installing: /usr/local/include/gtk-2-fortran/gtk_hl_container.mod
-- Installing: /usr/local/include/gtk-2-fortran/gtk_hl_button.mod
-- Installing: /usr/local/include/gtk-2-fortran/gtk_hl_entry.mod
-- Installing: /usr/local/include/gtk-2-fortran/gtk_hl_tree.mod
-- Installing: /usr/local/include/gtk-2-fortran/gtk_hl_menu.mod
-- Installing: /usr/local/include/gtk-2-fortran/gtk_hl_combobox.mod
-- Installing: /usr/local/include/gtk-2-fortran/gtk_hl_spin_slider.mod
-- Installing: /usr/local/include/gtk-2-fortran/gtk_hl_chooser.mod
-- Installing: /usr/local/include/gtk-2-fortran/gtk_hl_dialog.mod
-- Installing: /usr/local/include/gtk-2-fortran/gtk_hl_progress.mod
-- Installing: /usr/local/include/gtk-2-fortran/gtk_hl_accelerator.mod
-- Installing: /usr/local/include/gtk-2-fortran/gtk_hl_infobar.mod
-- Installing: /usr/local/include/gtk-2-fortran/gtk_hl_assistant.mod
-- Installing: /usr/local/include/gtk-2-fortran/gtk_hl_misc.mod
-- Installing: /usr/local/include/gtk-2-fortran/gtk_draw_hl.mod
-- Installing: /usr/local/include/gtk-2-fortran/gdk_pixbuf_hl.mod
-- Installing: /usr/local/include/gtk-2-fortran/gtk_sup.mod
-- Installing: /usr/local/include/gtk-2-fortran/pango.mod
-- Installing: /usr/local/include/gtk-2-fortran/gdk_events.mod
-- Installing: /usr/local/include/gtk-2-fortran/gtk_os_dependent.mod
-- Installing: /usr/local/bin/gtk-2-fortran-modscan
-- Installing: /usr/local/share/gtk-fortran/gtk-2-fortran-index.csv
-- Installing: /usr/local/share/gtk-fortran/gtk-2-enumerators.lis
-- Installing: /usr/local/lib/pkgconfig/gtk-2-fortran.pc
-- Installing: /usr/local/share/man/man1/gtk-2-fortran-modscan.1
```


3: 测试软件
然后你就会发现
cd 到 /home/happycamp-of-fortran/gtk-fortran/examples
然后
``` sh
root at javazhao-N53SM [19:27:20ä��午] in /home/happycamp-of-fortran/gtk-fortran/examples on git:master?
$ gfortran gtkhello2.f90 -o gtkhello2 `pkg-config --cflags --libs gtk-2-fortran` 

就可以了. 一定记住加入的是gtk-2-fortran 这个库。而不是gtk+-2.0或者gtk+-3.0,这两个库是针对c语言的。
$ ./gtkhello2 


```
