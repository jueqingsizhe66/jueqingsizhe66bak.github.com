---
layout: post
title: "pandoc制作幻灯片with revealjs"
date: 2015-06-07 17:34:49 +0800
comments: true
categories: markdown
---

第一次看到github.io居然可以放上幻灯片,有点觉得不可思议
[sed_and_awk教程](http://dongweiming.github.io/sed_and_awk/ )。后来调研发现这是采用了
+ [轻量slideshow（by markdown、pandoc、revealjs）](http://foxswily.iteye.com/blog/1997397 )
+ [Markdown+Pandoc→HTML幻灯片速成 ](http://www.soimort.org/posts/165/ )
+ [pandoc将markdown转换输出HTML slide ](http://www.tuicool.com/articles/Bbiu22U )


<!--more-->

我新建了一个文件夹

```sh
mkdir genereateSlides
cd generateSlides
mkdir targets

#并在targets中放上reveal.js,相信大家知道怎么下载
 git clone https://github.com/hakimel/reveal.js

```

然后我抛弃掉dzslides,s5,slideous,slidy等，现在只是使用reveal.js

- 它支持md和org格式，这是我欣赏的
- 它支持一标题是水平切换，二标题是纵向切换，这也是我所欣赏，影响我的思维
    + 所以针对markdown你需要思考何时使用#号个数
    + 很对org-mode你需要考虑何时使用\*号个数
- 它支持列表 逐个显示，也就是+/-/1. 等如果变异的时候加上-i选项则会逐个显示。
- 可以用 ----------多个破折号强制生成新的幻灯片

在开始写作之前首先得注意md开头的前三行：
```sh
% Ye Zhaoliang 
% Direct Numerical Simulation for Homogenuous Turbulence
% June 07, 2015

```

```
pandoc slides.md -o targets/slides.html -t revealjs -s -V theme=beige

```
__之前，进行几次实验，发现结果不对！但是其实就是用了上述的命令，而多试几遍又可以了，This is strange!But now it works__

## 几种不同的幻灯片样式:

+ default：（默认）深灰色背景，白色文字
+ beige：米色背景，深色文字
+ sky：天蓝色背景，白色细文字
+ night：黑色背景，白色粗文字
+ serif：浅色背景，灰色衬线文字
+ simple：白色背景，黑色文字
+ solarized：奶油色背景，深青色文字


## 控制代码高亮风格的选项有：

+ --highlight-style pygments
+ --highlight-style kate
+ --highlight-style monochrome
+ --highlight-style espresso
+ --highlight-style haddock
+ --highlight-style tango
+ --highlight-style zenburn
