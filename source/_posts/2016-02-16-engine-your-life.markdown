---
layout: post
title: "Engine your life"
date: 2016-02-16 17:26:03 +0800
comments: true
categories: scheme
---

有很多时候在思考engine是什么东西？
偶尔在一本书中看到Engine包含三个部分

1. clock ticks时间戳（什么时候做什么事情） 
2. success procedure
3. failure procedure

也就是一个Engine交代了按照时间的发展，满足某个条件
该做什么事情，如果失败了该做什么东西。
![listen][3]

<!--more-->
在上一篇[博文][4]并未详细说出一个数据抽象Data Abstraction 其实就是由基本的
创建部分和判断部分两部分组成。创建部分也叫做constructor比如上一篇[博文][4]的
zero，successor，predecessor。而判断部分也叫做胃词部分predicate。

1. constructor
    + add
    + delete
    + modify
    + ...
2. observer
    + false?
    + null?
    + zero?
    + atom?
    + ...


而综合Engine和Data Abstraction,其实Engine也是由着constructor和observer两部分组成进行数据抽象，只不过可能引入时间的概念。

而下午坐在山间，感受着潺潺的流水，Engine正如这潺潺的泉水![waterDown][1]，绵绵不息。一直想着让程序中复合哲学、心理门，
多一些人情味，而不是机器的呆板味。而Engine的CPS思想正好让程序体现的变化和流动的感觉，犹如潺潺的泉水,遇物则绕，有路则行。

一盘池水，流下了似水流年，规则依然
![shuiku][2]

坚信**任何科学问题都可以通过子语言进行求解**，当前语言无法解决只能说现在的语言对问题的解释不清楚，得进一步进行**语言分层**,
[每一个未解之谜都是要被解开的。][5]

[1]: /images/life/waterDown.jpg
[2]: /images/life/reservoir.jpg
[3]: /images/life/listen.jpg
[4]: http://jueqingsizhe66.github.io/blog/2016/02/15/data-representation-the-same-interface-with-different-implementation/
[5]: http://tianchunbinghe.blog.163.com/blog/static/7001201542402420584/ 
