---
layout: post
title: "ubuntu系统运用"
date: 2015-06-10 13:50:55 +0800
comments: true
categories: Linux
---

ubuntu系统，比如你用
``` sh
tar -cvf \*.tar.gz 某个文件夹
```
会发现可以多窗口地执行。其实这也说明了一点linux系统的多用户、多线程地开发系统、使用系统。

shell-script是一个比较有用的工具，沟通着用户(user)和内核(kernel)之间,我的理解是shell就是一个解释器(从lisp的解释器的角度来说,相互学习从函数式过渡到命令式)

<!--more-->

所以我会尝试着把我所有需要安装的software(app),都囊括在一个sh文件当中，所以我重装的时候只需要一个批处理即可。

下面我分成以下5个部分

+ 1系统的Git管理
+ 2图形部分
+ 3语言部分
+ 4编辑部分
+ 5配置部分

一一写上我会执行的操作，当然这些程序组合成为我自己使用的linux系统。方便我和linux的内核进行交流。

## 1系统的Git管理

```sh
apt-get install git,gitk,git-doc #rememeber how to configure
apt-get install git-flow #升级版的管理

git config --global user.name "ye zhaoliang"
git config --global user.email "zhaoturkkey@163.com"
ssh-keygen -t rsa -C "zhaoturkkey@163.com"
```

## 2图形部分

有时候你希望把你的相关数据打印成图片，这样方便交流
``` sh
apt-get install gnuplot
apt-get install graphviz
```

## 3语言部分
从更细小的程度理解你的思维，需要一些编程语言
``` sh
apt-get install gfortran 
apt-get install gcc 
apt-get install python
# drracket
# haskell
```

## 4编辑部分

``` sh
apt-get install vim   #rememeber how to configure
apt-get install vim-gtk

apt-get install emacs
```


## 5配置部分

在ubuntu管理系统经常会需要配置用户目录下的dotfiles，
比如登录相关文件.bashrc,.bash_logout,.bash_profile.
我会尝试着把我相关需要的文件夹备份在一个tar包。

``` sh
tar -xvf ./bash-it.tar.gz -C ~ # remember vcprompt
tar -xvf ./vim.tar.gz -C ~ # remember vim :BundleInstall
```


#结论
通过上面五个部分，无法体现我使用linux的基本感觉

``` sh
recur()
{
 查找(进入某个目录)
 判断(If,case)
 修改（配置 export 设置环境变量）
 recur
}
```

cfaj写的<shell-script:>和<pro-bash-programming>
主要教会我们的是我使用下划线新建一个临时函数，并使用下划线函数的大写字母来表征函数的返回值。然后使用${}变量引用来实现
版本号的截取并使用case进行分类选择，还有比如不同的系统的case选择。


### 补充一个操作

如果你想要使得子程序可以掉哦你个函数，必须使用export命令导出，类似于变量

``` sh

function func()
{
    echo "导出函数"
    }

export -f func
# 使用export -f 导出函数，只用于bash
# 使用export  DATE  等，导出系统变量

```

只有经过这样你的func才变成公有化。当然你也可以把脚本添加到path路径中。

###  谨记一个失误

无论是使用tar -cvf 打包，还是生成某个程序等，一定要检查一遍，是否能够运行，
当时就确认你的操作无误，否则后来会后悔(曾经打包了一个系统 发现都不能用了！
而那个系统也被我删掉了，可见我有多懊恼)
``` sh
tar: 跳转到下一个头
tar: 由于前次错误，将以上次的错误状态退出

当然有人说看看是不是需要使用tar -zxvf 而不是tar -xvf
我说我压缩的时候根本就没有用gzip，当然我也试了没效果，后来都删掉了。
```
