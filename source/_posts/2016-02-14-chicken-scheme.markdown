---
layout: post
title: "chicken-scheme"
date: 2016-02-14 13:52:48 +0800
comments: true
categories: scheme
---

它的主要住用适用于产生exe文件，并且对应产生的c代码更加的有效，
运行效率较高。
<!--more-->

## 产生可运行文件

该可运行的文件是通过chicken生成egg文件，也就是c语言,然后编译成机器代码。
所以运行效率更高一些。

``` scheme
;;; hello-world.scm
(print "Hello, world!")

;;; Running it interpreted:
$ csi -s hello-world.scm 
Hello, world!

;;; Compiling and running the executable binary:
$ csc hello-world.scm 

$ ./hello-world
Hello, world!
```

[chicken-scheme][1]

[1]:http://www.call-cc.org/ 
