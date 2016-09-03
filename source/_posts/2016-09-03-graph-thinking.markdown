---
layout: post
title: "graph thinking"
date: 2016-09-03 14:37:42 +0800
comments: true
categories:  prolog
---


图论，我并不是想说图论理论（也讲不出来）.我只是想用node和edge的关系来解释对象间的关系，类似于
[thinking like a git][3] 中提及的[node and edge][2]. 之所以提及他是因为他老是被遗忘，但又很有用，简化了
问题，并且可以用于[重构问题的解法:参考the craft of prolog 第一章][4].
<!--more-->

The basic notation meaning:

``` prolog
0   = 空集
I   = {(X,Y)| X=Y}.
r'  = {(Y,X)| r(X,Y)}.
r+s = {(X,Y)| r(X,Y) or s(X,Y))}. = = {(X,Y)| r(X,Y) ; s(X,Y))}.
r&s = {(X,Y)|r(X,Y) and s(X,Y)}. = {(X,Y)|r(X,Y) , s(X,Y)}.
diff = {(X,Y)| X\=Y }.
r.s = {(X,Y)| exist Z, r(X,Z), s(Z,Y)}.

r^0 = I.
r^n = r.r^(n-1).

%% 链状
r^* = (sum fron 0 to infinite)r^n.
    = I+r.r^*.
r^+ = (sum from 1 to infinite)r^n.
    = r.r^*.
```

由于period or dot在prolog有特殊的用途，并且为了表达正则表达式(regex expression) ，所以改变表达式中的点号为question mark.

<font color="red">注意:</font><br/>

+ question mark can be replaced by one atom or variable
+ 注意理解r,s等使用different\_list的思路去理解,也就是在解析表达式的过程中把r.s(X,Y) auto extend 到r(X,Z),s(Z,X).

``` prolog
%% 基本关系
0   = 空集
I   = {(X,Y)| X=Y}.
r'  = {(Y,X)| r(X,Y)}.
r+s = {(X,Y)| r(X,Y) or s(X,Y))}. = = {(X,Y)| r(X,Y) ; s(X,Y))}.
r&s = {(X,Y)|r(X,Y) and s(X,Y)}. = {(X,Y)|r(X,Y) , s(X,Y)}.
diff = {(X,Y)| X\=Y }.
r?s = {(X,Y)| exist Z, r(X,Z), s(Z,Y)}.

%% 单条链
r^0 = I.
r^n = r?r^(n-1).

%% 整张图
r* = (sum fron 0 to infinite)r^n.
    = I+r?r*.
r+ = (sum from 1 to infinite)r^n.
    = r?r*.
```

<font color="red">注意:</font>

+ r+本来想写成`r^+` 但是感觉加上一个`^`,不太喜欢这种标注，于是去掉。而`r^0`则
不想写成`r0`以示区别。
+ 除了diff和&不符合正则表达式的风格，其他比如`+  ? * +`则符合正则表达式的风格
+ 每一个statement都是用period or dot句号作为结束标志。

One example:

Assume, parent(P,C) :- P is a parent of C.
```
    
    childof   = parent`.
    sibling   = (childof?parent)&diff.
    cousin    = (childof?sibling?parent).
    ancestor  = parent+.  
    related   = (childof + parent)*.


```


<font color="red">注意:</font>

+ 注意childof是当前节点往后找到一对(X,Y) pair配对的关系(他的定义是{(Y,X)|...})，而ancestor则是往前追溯一系列的(X,Y)
因为他的定义是{(X,Y)|...}


另外一个例子

```
    r* = I + r + r^2 + r^3 +...+ r^(n-1) + (r^n)+
```

+ step1: compute the ` I + r + r^2 + r^3 +...+ r^(n-1) `(start), the set of nodes can be reached from the start in fewer than n steps
+ step2: compute the relation `r^n` , which represents taking n steps at a time.
+ step3: search the graph whose initial nodes were found in step1, and whose edges result from applying the relation found in step2


有用？我暂时未体味出来，但是倒是可以通过正则表达式去理解二元算法计算的过程和图论的节点和边的关系。(原作者是这样解释的：
Because the number of states, you can reach by taking two steps is often less than the square of the number of states  you can reach by taking one step. This can greatly reduce the branching factor of the state space)

[1]:http://baike.baidu.com/link?url=UH9qSwIDSqYhEEXUt_n0-UHrre7cxRKK4JhhlcjIZ3S_0o0PVTa6d0GNEF5LKRxANjk8C8fA-RO6L_8VhqKKda 
[2]:http://think-like-a-git.net/sections/graph-theory/nodes-and-edges.html 
[3]:http://think-like-a-git.net/ 
[4]:https://mitpress.mit.edu/books/craft-prolog 
