---
layout: post
title: "prolog小结"
date: 2016-07-19 18:41:13 +0800
comments: true
categories: prolog
---

prolog编程工具: swi-prolog

1. [ 1. cut-fail](#1)
2. [ 2. prolog 相关链接](#2)
3. [ 3. prolog 调试](#3)
4. [ 4. 可以尝试进行的主题阅读方式](#4)
5. [ 5. prolog基础知识点](#5)
6. [ 6. if-then-else的写法](#6)
7. [ 7. DCS写法](#7)
8. [ 8. p99问题集](#8)
9. [ 9. 几本不错的逻辑书籍](#9)
10. [10. 我购买的几本书籍](#10)
<!--more-->


<h2 id="1">1. cut-fail</h2>

结合The Science Of programming 和 Learn prolog now,觉得cut-fail的直觉应该如下：

把proof development的过程和prolog的过程 结合起来！ 也就是cut-fail其实就是只进行内层的循环，而不跳出来，即内层执行完则结束。
参考TSOP P52 some general hints on developping proofs.

![TSOP][16]

 <h2 id="1">2. prolog 相关链接</h2>

a. [swi-prolog][1] . 注意SWI-Prolog比较强大的地方是开源，而且提供较多的libraries,提供较多的predicates. 
b. [scisTus][2] .   文档介绍得不错，不好的地方就是not free. 所以可以当作学习的地方。
c. [Learn Prolog Now!][3]. 一个很好的入门教程 可以结合Leo Sterling的[The art of prolog][4],和William F. ClockSin的Programming in Prolog(2003) 可以进一步查查[github方向相关资料][5]
d. [Artificial Intelligence Programming in Prolog (AIPP)][6], EdinBurgh(prolog研究基地）的课程,内有很多pdf和ppt。
e. [samer_prolog][7] 提供了很多的库，值得学习。
f. [learn prolog now课程][9].  附带一个[中文翻译][10].
g. [cs9414][11]. INTRODUCTION TO PROLOG PROGRAMMING. 刚开始入门的就是它。
h. [ J.R.Fisher 教程][12]
j. 个人小介绍![introduction][15]

 <h2 id="1">3. prolog 调试</h2>

a. trace  进行详细的跟踪，会call exit（正常退出） ，如果失败则fail，fail则会执行redo，直到exit或者fail退出。
b. notrace 使用notrace退出详细的跟踪。 类似的debug,nodebug.

初始界面
![initial][17]

添加predicate的trace
![trace][13]

 <h2 id="1">4. 可以尝试进行的主题阅读方式</h2>


``` sh

主题：Lists		Short Description: 一个经典的数据结构，其中的append/3将会用于differentiate lists中，进一步的运用于DCS(define clause statement)

The art of prolog(1986) Sterling and shapiro :	TAOP	
Learn Prolog Now Patrick Blackburn, Johan Bosand Kristina Striegnitz :	LPN	分两步讲解：1.Lists 2.More Lists(append reverse)
The Craft of Prolog Richard A.O Keefe  :	TCOP	
Adventure in Prolog(1990) Dennis Merriitt	AIP	
An Introduction to prolog programming (1990) Patrick Saint-Dizier	AITPP	
Prolog Verses you(1989) An Intouduction to logic Programming   A. -L. Johansson  A.Ericson-Granskog A.Edman	PVY	
Programming in prolog(2003) using ISO   Wiliam F. Clocksin ,Chrisopher S. Mellish	PIP	
Cases and effects  William F.clockSin(1997)	CAE	
Prolog the standard  P. DeranSart ,A.Eddbail, L. Cervoni	PTS	
Prolog and Natural-Language Analysis  Fernando C. N. Pereira and Stuart M. Shieber(1987)	PANLA	

```


 <h2 id="5">5. prolog基础知识点</h2>

a. cd('路径'). 
  路径是你的文件系统路径。
b. pwd.        
  可以显示当前所在的路径。
c. [main]      
  其实相当于会去查询当前目录下的main.pl并把它加载进来(类似于consult('路径/main.pl').)，可以调用main里头的所有的predicates。
当然在文件内也可以写为：
``` prolog
:- [main].
```
d. file_search_path(demo,'路径') 
  demo可以是任何的字符串，路径写对了，以后想要访问路径下的文件只需要

``` prolog
:- [demo(文件名)].
```

<h2 id="6">6. if-then-else的写法：</h2>
  (A -> B; C).   如果A成立则执行B，忽略C，如果A不成立，则执行C，忽略B.

 <h2 id="7">7. DCS写法</h2>

``` prolog

s --> simple_s,conj,simple_s.
s --> simple_s.
simple_s --> np(subject), vp.
np(_) --> det, n;det,adjectives,n.
np(X) --> pro(X).
vp --> v, np(object).
vp --> v.
det --> [the].
det --> [a].
n --> [woman].
n --> [man].
v --> [shoots].
adjectives --> adjective.
adjectives --> adjective,adjective.
adjective --> [young].
adjective --> [beautiful].
adjective --> [sunshine].
adjective --> [lovely].
adjective --> [sweet].
adjective --> [handsome].
pro(subject) --> [he].
pro(subject) --> [she].
pro(subject) --> [i].
pro(object) --> [him].
pro(object) --> [her].
pro(object) --> [me].

conj --> [and].
conj --> [or].
conj --> [but].
conj --> [so].


```

 <h2 id="8">8. [p99问题集][8].</h2>

 编程界面![view][14]和编程想法:多文件多库编程，最好还是类似搭积木的过程。

 <h2 id="9">9. 几本不错的逻辑书籍</h2>

1. A Discipline of Programming (Edsgar Dijkstar写的书，较难懂）

2. The-Science-Of-Programming（Edsgar Dijkstar 替作者Gries写了书评较基础  是1的简化并加入了很多例子）

3. Heijenoort.From Frege to Godel.A Source Book in Mathematical Logic_ 1879-1931. 类似于上面的数学逻辑。

<h2 id="10">10. 我购买的几本书籍</h2>

[书籍][18]

[1]:http://www.swi-prolog.org/pldoc/index.html
[2]:https://sicstus.sics.se/sicstus/docs/latest/html/sicstus.html/
[3]:http://www.learnprolognow.org/lpnpage.php?pageid=online
[4]:https://github.com/konn/TAOP-Exercises
[5]:https://github.com/search?utf8=%E2%9C%93&q=Programming+in+Prolog
[6]:http://www.inf.ed.ac.uk/teaching/courses/aipp/#Lectures_
[7]:https://github.com/samer--/prolog
[8]:www.ic.unicamp.br/~meidanis/courses/mc336/2009s2/prolog/problemas/
[9]:http://www2.hawaii.edu/~nreed/
[10]:http://www.cnblogs.com/seaman-h-zhang/p/4615097.html
[11]:http://www.cse.unsw.edu.au/~billw/cs9414/notes/prolog/intro.html
[12]:http://www.cpp.edu/~jrfisher/www/prolog_tutorial/contents.html
[13]:/images/prolog/debug.png
[14]:/images/prolog/view.png
[15]:/images/prolog/introduction.png
[16]:/images/prolog/someGeneral.png
[17]:/images/prolog/swi-prolog-debug.png
[18]:http://jueqingsizhe66.github.io/blog/2016/07/19/da-yin-de-shu-ji-prolog-plus-scheme-plus-kong-qi-dong-li-xue-fang-mian/
