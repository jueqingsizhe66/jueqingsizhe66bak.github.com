---
layout: post
title: "vim-haskell"
date: 2015-05-18 14:04:37 +0800
comments: true
categories: Haskell
---

Haskell vim可以通过vimbar 安装到vim中。vimball是一个vim插件，可以很方便地帮你安装vba格式的插件。

<!--more-->

[直接用vim打开vba格式的文件，输入:so %即可安装]( http://blog.csdn.net/fudesign2008/article/details/7297949)，然后:q退出。

删除插件也很方便，直接在vim里输入:RmVimball 插件名

不用安装vimBall(vim7.3之后 自带)


方法1：[Haskell vimmode安装文件下载](http://projects.haskell.org/haskellmode-vim/vimfiles/haskellmode-20101118.vba)

方法2：使用bundles
 ``` sh
 Bundle 'https://github.com/jueqingsizhe66/haskellmode-vim'
 ```


安装说明
1. vim vbafiles
2. :so %;
3. [configure vimrc](http://projects.haskell.org/haskellmode-vim/)

都需要配置docdir和browser，doc如果没有去windows底下的haskell phamtom拷贝一份 

windows:
``` sh
let g:haddock_browser="E:\Program\ Files\Mozilla Firefox\firefox.exe"
let g:haddock_docdir="C:\Program\ Files\Haskell\ Platform\2014.2.0.0\doc\html" 

```

ubuntu :

``` sh

let g:haddock_browser="/usr/bin/opera"
let g:haddock_docdir="/home/happycamp-of-lisp/doc"

```

*最后建议还是emacs漂亮一些！*

