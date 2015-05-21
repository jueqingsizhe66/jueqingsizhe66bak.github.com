---
layout: post
title: "One-parameter-Or-many-parameters?"
date: 2015-05-21 00:12:30 +0800
comments: true
categories: Haskell
---

I have a doubt about how to call the function when 
we see the type declartion

:: (Double,Double,Double) -> (Double,Double)
It is one parameter, but it must be a tuple with three tuple elements

::Double->Double->Double
What does it mean?

<!--more-->
``` haskell
areAsceneding :: Integer->Integer->Integer->Bool
areAsceneding a b c=a<b && b<c

areasceneding 1 2 3
--get true
areasceneding 1 4 2
--get false
``` haskell
Actually ,multiply parameter can changed into one parameter method by
currying!!

You must care about the () means tuple data structure. 
                        [] means list  data structure.


## [绑定Binding](http://www.jdon.com/idea/haskell.html)

　　Haskell使用=实现绑定，前面我们说=是引用透明的案例，是一种数学意义，实际是将两者绑定了。

x = 2                   -- 两个连字符号表示注解
y = 3                   --    
main = let z = x + y    -- let 引入一个本地绑定
       in print z       -- 程序将打印 5
　　Binding名称不能大写，绑定可以定义一个或多个参数的函数，函数和参数使用空格，这是Haskell区别其他语言的简洁之处：

add arg1 arg2 = arg1 + arg2 -- 定义函数add
five = add 2 3 -- 调用函数add

　　括号可以包装复合表达式：

bad = print add 2 3 -- error! (打印只能有一个参数)
main = print (add 2 3) -- ok, 使用一个参数5作为打印参数
 
      
## 变量是不可变的

　　与命令式语言变量不同,Haskell绑定变量 不可变的：
x = 5
x = 6 -- 错误, 不能再绑定x

类型有下面几种：

Bool - 有两个元素： True 或 False
Char - unicode符合集合
Int - 固定大小整数
Integer - 无限大小整数
Double - IEEE 浮点数字
type1 -> type2 - 一个输入类型type1 到输出类型 type2的函数
(type1, type2, ..., typeN) - 类型数组tuple
() - 零数组tuple, 称为unit (类似C的void); 这个类型只有一个值：空。

[Char]-> means  list pk tuple.
