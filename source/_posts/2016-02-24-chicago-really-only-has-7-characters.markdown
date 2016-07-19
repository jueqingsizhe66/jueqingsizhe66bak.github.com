---
layout: post
title: "Chicago really only has 7 characters?"
date: 2016-02-24 17:19:30 +0800
comments: true
categories: scheme
---

我在听[SICP的优酷视频][1]时，听到“芝加哥也可以说是具有7个字符，同时他也可以表述为美国的中西部伊利诺伊州的全美第三大国际大都市”.
它可以仅仅表示语言的表面意思，也可以表示语言中的隐含意思。这体现了语言实现的一种魅力，Chicago really only has 7 characters?
<!--more-->

## Value

+ Value，价值。
+ scheme的每一个程序都是由exressions构成，每一个expression都对应一个expression value，简称expval。
+ expval对应的有exp-val和den-val。
+ exp-val表示表达式的值，也就是可能是变量绑定也可能是列表(表达式)的值；而den-val单单只是变量绑定。

为了分析每一个价值的过程，姑且把程序写为<<expressions>>

如果`expressions=((lambda (x) (+ x 3)) 10)`

那么scheme如何解析？

假定我们的解析程序叫做value-of,init-env是我们的初始环境(变量绑定或者字典),[10]表示scheme-val(每一种语言都有着自己的value),
[x=10]init-env表示拓展的环境

``` scheme
(value-of <<expressions>>)
         =(value-of <<((lambda (x) (+ x 3)) 10)>>)
         =(value-of ((lambda (x) (+ x 3)) 10) init-env)
         =(apply-procedure (value-of <<(lambda (x) (+ x 3))>> init-env) 
                           (value-of 10 init-env))
         =(apply-procedure (procedure x <<(+ x 3))>> init-env) 
                           [10])
         =(value-of <<(+ x 3))>> [x=10]init-env) 
         =(apply-procedure (value-of + [x=10]init-env)
                           (value-of x [x=10]init-env)
                           (value-of 3 [x=10]init-env)
                )
         = (apply-procedure (procedure + [x=10]init-env)
                           [10]
                           [3] 
                )
         =(+ (val-num [10])  (val-num [3]))
         =(+ 10 3)

```

刚开始
+ 也许你不可能替代<<>>表达式的意思，
+ 你也可能不理解解析10的过程，
+ 你也分不清出解析lambda和+的不同（一种是自定义程序，一种是内置operator），
+ 甚至你不可以静下心来好好分析我正在说的话。

我想表达的是，程序给你的是program or data，而你作为语言的创造者或者使用者，你需要进一步体味program是什么？
**Does program really has only 7 characters?**(思考你写过的程序的各个部分的用途，如何来的？)

进一步的解析可以[参考][2]。

## 源语言到目标语言的转变

有一种程序员喜欢利用各种语言所具有的特色，组合使用语言形成自己的程序，内部是控制流动和数据流动。而从第一种语言
也就是源语言(source language)开始，利用变换，输入到第二种、第三种…… ，最终输入到目标语言(target language).程序并不是
我们想的单一性的语言调用，每一个问题都可以通过新构造一种新的语言进行求解(持保留意见)。现在已经有人能够作出输入一种输入和输出
结果，从而产生你要的scheme程序，也就是产生对应的语言解决这个问题。然而我们更需要思考的是变的表面含义。"变"的是什么？
可深入了解Dan. Friedman的[EOPL][3]程序设计书籍。

[1]:http://v.youku.com/v_show/id_XMTQ3NDEwODUyNA==.html 
[2]:http://jueqingsizhe66.github.io/blog/2016/02/09/its-a-dead-program-dot-how-to-let-it-alive/#4 
[3]:http://www.eopl3.com/ 
