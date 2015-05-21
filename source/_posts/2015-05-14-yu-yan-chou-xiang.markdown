---
layout: post
title: "语言抽象"
date: 2015-05-14 10:21:48 +0800
comments: true
categories: scheme
---

我们每天都在做的事情
1.打开电脑，找到我们要做的事情，保存我们做完的事情，提交,我们可能挨训，也可能加班
2.我们可能去打球，我们可能跟朋友、亲人聊天
3.我们可能在周末的时候，跟女朋友、家人去看电影，去郊游
4.我们可能去参加一些团体活动……

<!--more-->

我们把我们要在电脑、生活上做的事情可以抽象化，虽然我们不知不觉的做着这些事情，
但是这是第一步叫做事件抽象（过程抽象）。随着发展，我们可以进行逻辑抽象，并最终达到[语言抽象][LanguageAbstract].

我们会发现我们所使用的计算机语言包含着主要的东西是
    1. 变量
    2. 函数
    3. 调用

可以参看[王垠的计算器][WangYin],里面第三节中提到Lambda Calculus是什么
    1. 变量: x
    2. 函数: (lambda (x) e)
    3. 调用: (e1 e2)

这是任何一门语言不可缺少的逻辑语言抽象部件，就好像数学家很早就认识到序列求和中的[抽象模式][Abstractmodel].

语言本身其实就是一个[求值器][Evaluator]
    > The evaluator, which determines the meaning of expressions in a programming language, is just another program. 
所以事实上，可以几乎把任何程序看作是某个语言的求值器，它本身不过就是另一个程序而已。

而为了更好地操作这些语言的元素，进行大型的系统模块化设计，我们还需要引进[两种模式][System]
1.对象实现(以不变的世界坐标系来说，其实对象是不断变化的，比如位置；此时坐标系是全局的)
2.流实现(以变化固体的角度来说，其实他是不变的；此时坐标系是局部的)


关于scheme的部分以后再补上。



[LanguageAbstract]:http://mitpress.mit.edu/sicp/full-text/book/book-Z-H-25.html#%_chap_4
[WangYin]:http://developer.51cto.com/art/201208/352423_2.htm 
[AbstractModel]:http://mitpress.mit.edu/sicp/full-text/book/book-Z-H-12.html#%_sec_1.3.1 
[Evaluator]: http://mitpress.mit.edu/sicp/full-text/book/book-Z-H-25.html#%_chap_4
[System]: http://mitpress.mit.edu/sicp/full-text/book/book-Z-H-19.html#%_chap_3
