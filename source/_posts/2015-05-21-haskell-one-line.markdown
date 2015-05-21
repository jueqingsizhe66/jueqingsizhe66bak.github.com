---
layout: post
title: "Haskell-one-line"
date: 2015-05-21 10:28:55 +0800
comments: true
categories: Haskell
---

Haskell have many beautiful features,such as monad(combination),so we can
write very elegant code(compact code).
<!--more-->
Reference [Here](http://www.shuklan.com/haskell/lec08.html#/0/17)
verbose:
``` haskell

module Main where

import System.IO

main = do
  theInput <- readFile "input.txt"
  putStrLn (countLines theInput)

countLines :: String -> String
countLines str = (show (length (lines str)))
```
elegant:
``` haskell
module Main where

import System.IO

main = readFile "input.txt" >>= print.length.lines
	      
```


## [Function Application](http://shuklan.com/haskell/lec06.html#/0/13)

verbose:
``` haskell
not (odd 4)
not $ odd 4
```
elegant:
The . Function
It composes functions in a readable manner

``` haskell

f(g(h(k(x)))) is ugly
(f.g.h.k)(x) is pretty
 (not.odd) 4
 (length.head.words) "University of Virginia"
```

上面写的elegant风格都可以叫做point-free风格，它使用Monad的结合律，

比如下面转换一般代码到point-free风格的方法：
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

另一个常用的转换写法：
``` haskell
add a b = a+ b  ---> add =(+)
f s =length (head (words s)) ---> f= length.head.words

```


Add later
