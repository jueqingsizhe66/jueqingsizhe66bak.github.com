---
layout: post
title: "My own customed vim"
date: 2014-05-14 13:54:05 +0800
comments: true
categories: vim
keyword: vim bundle vundle
---

<!--more-->
#Installation#

###Backup your old vim configuration files:####

``` sh
    mv ~/.vim ~/.vim.orig
    mv ~/.vimrc ~/.vimrc.orig
```

###Clone and install this repo:###
``` sh vimconfig  http://pan.baidu.com/s/1qWEvMPm  下载
    到我的百度盘[下载][1].vim，复制到 ~主目录下，即可,已经有vundle了。
    ln -s ~/.vim/vimrc ~/.vimrc
```


###Install bundles ### 
Launch vim(ignore the errors and they will disappear after installing needed plugins)and run:
``` vim
    :BundleInstall
``` 

###Thst's it!###

[1]:  http://pan.baidu.com/s/1qWEvMPm
