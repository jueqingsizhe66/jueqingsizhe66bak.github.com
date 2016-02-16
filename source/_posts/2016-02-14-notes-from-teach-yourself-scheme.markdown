---
layout: post
title: "Notes from Teach yourself scheme"
date: 2016-02-14 11:56:24 +0800
comments: true
categories: scheme
---

The notes is published for rethinking what I am reading 
in the book called [《Teach Yourself Scheme in Fixnum Days》][3].
It is a good book for introducing scheme(you'd better have read
the TLS)
<!--more-->
## Forms
All expression can be inducted into Forms,such as define,begin,set! forms.
Before, I only said the define,begin,set! expression without forms.

## Data Types

1. list
2. vector
3. procedure
4. port (原来port也是数据类型的一种，这样就并入了文件的IO)
   当然也包括open-string-port相当于创建了fortran的[内部文件][4]的作用。

所有的形式(forms)在scheme其实都是一种对于数据的representation.要反复思考这个过程。

> Data abstraction divides a data type into two pieces: an interface
> and an implementation. The interface tells us what the data of the type
> represents,what the operations on the data are,and what properties these
> operations may be relied to have.The implementation provides a specific
> representation of the data and code for the operations that make use
> of that data representation.

## implicit and explicit

lambda 的内部自带(begin ...)的作用，也就是可以连接多个expressions（forms）。
而if没有，对应的when，unless conditions则是存在隐式的implicit begin的作用。
并且when和unless其实就差在一个not表达式

+ lambda implicit begin
+ when   implicit begin
+ unless implicit begin
+ if     explicit begin

## macro expansion

1. `(.. , ...)    叫做comon 形式
2. `(... ,@ ...)  叫做comon slice形式

## Jumps(CPS)

Continuation is the special contribution of the book.

[很多其他的scheme解释器和编译器.][1] [Mit_scheme][2]
[1]: http://tunes.org/wiki/scheme.html 
[2]: http://groups.csail.mit.edu/mac/projects/scheme/index.html 
[3]: http://ds26gte.github.io/tyscheme/index.html 
[4]: http://v.fcode.cn/video-internal_file.html 
