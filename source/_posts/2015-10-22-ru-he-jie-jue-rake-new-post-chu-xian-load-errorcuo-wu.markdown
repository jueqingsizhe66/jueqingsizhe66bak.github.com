---
layout: post
title: "如何解决rake new_post[]出现load error错误"
date: 2015-10-22 23:41:06 +0800
comments: true
categories: Linux
---

在使用octopress的ruby+rails搭建的github博客系统时候，有时候会出现
一些问题，比如搭建不成功等，具体不提及。本文仅仅解决一个rake新博文的时候
可能遇到的一个问题。
<!--more-->

## 问题再现
```
/usr/local/bin/rake:23:in `load': cannot load such file -- /usr/share/rubygems-integration/all/gems/rake-10.3.2/bin/rake (LoadError)
    from /usr/local/bin/rake:23:in `<main>'

```

## 解决办法

source /usr/local/rvm/scripts/rvm这是一个bug，经常需要执行这个，因为可能rvm就被关掉了，一关掉，rake就报错！！！切记

可以把这句话放到~/.bashrc当中。
