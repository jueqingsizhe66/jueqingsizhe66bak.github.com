---
layout: post
title: "编程的时候，你到底在想什么？"
date: 2015-06-08 00:00:05 +0800
comments: true
categories: Haskell
---

我静下心来思考，linux bash给你带来的是从__文件角度__的编程。
而比如c、fortran、java、python、haskell、scheme等更像是从__思维的细节角度__编程。
然而我又在想，如果程序仅仅是他展现出来的那样，是什么一步一步执行，那就太没意思？
所以我在思考程序给你带来的乐趣是什么？给你带来的成就感是什么？何为elegant code?
![芭蕾舞女](/images/balei.jpg)
<!--more-->

## [Makefile](http://blog.csdn.net/liang13664759/article/details/1771246)的项目管理
makefile给我们带来的感觉，也是从文件的角度进行编程，只不过他需要你的思维细节存在着__编译__和__链接__的过程。Makefile
给我们带来的乐趣也许更多的是对于整个项目文件的把握，让各个文件各司其职。


## [java](http://blog.sina.com.cn/s/blog_4e16c5a901000aky.html )存在包机制：
``` java

// Fighterplane.java
package com.resource;
/**
  * Classname：FighterPlane
  * @version 1.0 6.08.2015
  * @author yzl
 */
public class FighterPlane { 
    //变量定义
    //方法定义
}

```
在一个包内可以包含很多的java文件，他更多的是方便我们建立类的抽象，把很多的方法属性放在对应的包中，让我们
思维陷在一个一个的包中、类中。


## [Haskell](http://jueqingsizhe66.github.io/blog/2015/05/21/haskellku-wen-dang-jie-gou ) 也有一套包管理系统。
在这个系统里面,你需要不断的去思考每一个module中到底需要包括多少函数（一种映射关系），具体可以经常查看haskell的一些[优雅的代码](http://jueqingsizhe66.github.io/blog/categories/haskell/).通过haskell给你带来的最大的震撼就是__类型映射__，你会发现这个世界有迹可循。

+ [haskell elegant code](http://jueqingsizhe66.github.io/blog/2015/05/21/haskell-one-line/)
``` haskell
f x = 5 + 8/x
f x = (+) 5 (8/x)
f x = (+) 5 ((/) 8 x)
f x = ((+) 5) ((/) 8 x)
f x = ((+) 5) (((/) 8) x)
f x = ((+) 5).(((/) 8)) x

f= ((+) 5).((/) 8)
f= (5+).(8/)

```
+ [scheme elegant code](http://jueqingsizhe66.github.io/blog/2015/05/17/cong-lambdadao-simple-plus-complexjie-shi-qi-zai-dao-shu-xing-chou-xiang/)
``` scheme
(lambda (le)
    ((lambda (f)
      (f f))
     (lambda (f)
       (le (lambda (x) ((f f) x))))))

```
## [fortran](http://wenku.baidu.com/link?url=0qejjb1Ts8GQemVRic71KyAnduYVE3ldHsdIrtbbkB70fEkyxQfuga_nT3ITd8lvq-WIYvKeiJAU2KRD-EpaK4ZEUH2HuOH_5B-wEodzUtC) 的新语法中也提供了一套新的模块管理方法（只是为了方便编程管理、对于提速没什么关系）。
他和java当中的类思想很相近。fortran这门语言给你的思维改造其实不大，我一直不觉得他是一门很好的语言，但它的确是一门很好的科学计算语言，在数值计算中做了很大的优化。


暂时先从Makefile、java、Haskell、fortran这四个编程语言（都有[变量、表达式、函数](http://developer.51cto.com/art/201208/352423_1.htm)、模块，从低到高的逐次演化），想着重让自己理解的是__代码的优雅性__,代码让你看上一遍会印象深刻，会让你的思维活跃，而不像命令式语言真的让你木楞的感觉。刚才我发呆了一小会，思考本文的初衷:到底code乐趣是什么？为什么那么多人觉得没意思？

大家都知道得吃点苦才能干成一件事情（人这一辈子能成一件事情就不容易），可是不能坚持吃苦。__代码的优雅性体现在反复修改和反复的抽象，简单化的过程__。优雅的代码让你感觉干净，有时候bash写的代码真心有一种让你恶心，但是如果稍加修改用其他方式（减少很多${}的形式）,却又让你[豁然开朗](http://jueqingsizhe66.github.io/blog/2015/06/02/bashzhi-neng-fen-lei-sheng-ji-ban/)。

你在编成的时候，你想的是如何实现这个功能？于是你需要进行逐步分解，并最终组合起来，但是细节是很多人都抛弃了函数化和模块化的思想，不然你去看一下刚开始大家写的就像一条一条[汇编语言](http://www.cr173.com/soft/18964.html)一样，与机器对话。建立于生态系统之上的优雅代码，应该是让功能能够顺利实现的同时，也要增加代码的可读性。[《人月神话》](http://baike.baidu.com/link?url=UrkX_KZjr1AU4ROv15Pr-auiRs5NTHbhAx69j8aP617_KWg48cBCBAhn6auK8X8KvKpzoWJVhMNfZuf0z4R68K)[《重构代码》](http://www.chnxp.com.cn/soft/down-89638.html)等书籍都讲述着类似的道理。__美应该是反复修改的，并趋于简单，体现优雅__。


+ 扭转思维的滚珠，如果能够让你的这条滚珠适应大部分的情况，或者减少滚珠的数量，那就像优雅更迈进一步。
+ 思索思维的步骤，一个好的coder应该记得他需要[思考什么](http://mindhacks.cn/)，以及他不需要思考什么，因为优雅让其知道该去做什么。

你会发现，haskell给你的类型映射，深深的影响你的抽象映射；[scheme](http://jueqingsizhe66.github.io/blog/categories/scheme/)的递归思想，也不断的让你的思维细化，然而这种细化是优雅的，反复修改的。优雅源自气质，却是反复修改的，趋于简单。Haskell和scheme的气质相比于bash shell的粗俗（但是的确能够解决问题，而且有时候他也可以用更少的代码解决问题），他们是曲径通幽处，芬芳四溢，罄人心鼻。函数式编程（我理解是关系式编程，haskell表示为->)一直犹如跳着芭蕾舞的舞女一样，在时尚的c、java等命令语言静静生存。
![曲径通幽处](/images/qujin.jpg)

__代码需要修改，思维需要细化，同时思维也需要提升__；事情分先后，也要掌流程，万事不应皆掌握，得分而治之(这也是unix的哲学思想所在，用众多最优雅的代码，组合起来形成一朵美丽的潘多拉之盒)。


