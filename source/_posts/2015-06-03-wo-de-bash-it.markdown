---
layout: post
title: "我的Bash-It"
date: 2015-06-03 13:59:44 +0800
comments: true
categories: Linux
---

[Bash-it](https://github.com/jueqingsizhe66/bash-it)是一个开源的bash框架，
我的第一感觉是他的themes主题不错，所以就用了，这是我采用的rjorgenson的主题
![rjorgenson](/images/jiemian.png)
<!--more-->

Bash-it是源自于[oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh),
[zsh的一些相关展示](http://ohmyz.sh/)
主要是调用了一些python和ruby的相关插件。

##安装步骤

很简单

+ git clone --depth=1 https://github.com/jueqingsizhe66/bash-it ~/.bash-it (__如果不加入--depth=1会少一些文件,没有Enable文件夹__)
(或者把install.sh里面的enable改成available即可(因为作者可能删掉了enable)
)
+ cd ~/.bash-it
+ ./install

当然还需要安装一些[额外的插件](http://www.phodal.com/blog/use-bash-it-bash-framework/)

+ apt-get install docker()(有时候可能不需要)
    + sudo apt-get install docker.io 
    + sudo ln -sf /usr/bin/docker.io /usr/local/bin/docker 
    + sudo sed -i '$acomplete -F \_docker docker' /etc/bash_completion.d/docker.io 
    + chmod 777 /var/run/docker.sock
+ pip install arg (首先得安装python-pip)
+ 安装chruby [https://github.com/postmodern/chruby](https://github.com/postmodern/chruby)
+ apt-get install libpq-dev


现在安装完之后的感觉就是界面变得更好看些，其他没什么改变。

### 注意
Bash_it的配置脚本和openfoam的配置脚本冲突，暂时未找到原因。

### 补充theme配置
当你执行玩Bash-it的install脚本后，在~目录下会有~/.bashrc文件，修改其中的bash theme主题变量即可。
我现在使用的是doubletime_multiline_pyonly感觉还不错 
