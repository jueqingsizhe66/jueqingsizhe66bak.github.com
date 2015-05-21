---
layout: post
title: "Haskell的一次Homework"
date: 2015-05-17 22:09:09 +0800
comments: true
categories: Haskell
---

比较欣赏第三种解法的匠心独运。另外最后一处也引入了模式匹配的思想。
其实模式匹配，又或者Awk的[Map-reduce]( http://www.kuqin.com/shuoit/20140617/340631.html)等都源自范畴论的[Monad](http://www.jdon.com/idea/monad.html).
Monad其实是[Monoid]( http://www.jdon.com/idea/monoid.html)的一种endfunctor..
Monoid更象是数学的[复合函数](http://baike.baidu.com/link?url=7iYP9qxtCIrLmJfyDk1nLYlcfBIB17bXjOQmymFupddY0ZN9uiNrXfY8R6yrbFr1Kl9JBmSwyzkQ6EGcQEqlc_)的求解策略。f和g组合是g.f，g和h组合是h.g , 也就是说，f函数的输出作为g函数的输入，而且f函数的输出类型必须和g函数的输入类型相同。
我们将一个范畴有元素对象和态射箭头，态射箭头有组合和幺元两种，且满足结合律，这种范畴称为Monoid。
<!--more-->

``` haskell
import Data.Char
import Data.List
import Data.Maybe

-- AThis example uses 'succ' to get next letter

cipher :: String -> Int-> String
cipher "" n = ""
cipher str n = rotate (head str) n : cipher (tail str) n

rotate :: Char -> Int -> Char
rotate c 0 = c
rotate c n = rotate (next c) (n-1)

next :: Char -> Char
next c = if c=='z' then 'a' else succ c

-- BImporting a really useful library
-- designed by Preetam C. Jinka


cipher2 :: [Char] -> Int -> [Char]
cipher2 [] _ = []
cipher2 (s:ss) n = (rotate2 s n) : (cipher2 ss n)

rotate2 :: Char -> Int -> Char
rotate2 c 0 = c
rotate2 c n = int2letter ( mod ((letter2int c) + n) 26 )

letter2int c = ord c - 97
int2letter n = chr (n + 97)
	   
-- CHere's a one-line solution 
-- designed by James C. Sun


---Each pattern has the same type declaration
cipher3 :: [Char] -> Int -> [Char]
cipher3 s n = map (\c -> ([c..'z'] ++ ['a'..c]) !! mod n 26) s
---妙的地方在于map高阶函数 遍历s 到c c从新组装成一个c..c的列表通过
-- (!!):[a]->Int->[Char]的 最后组合返回



-- designed by Matthias pall Gissurarson

alphabet:: [Char]
alphabet = cycle ['a'..'z']

rotate4:: Int -> Char -> Char
rotate4 k a = alphabet !! (k + ( fromJust ( elemIndex a alphabet)))

cipher4:: [Char] -> Int -> [Char]
cipher4 str k = map (rotate4 k) str


-- designed By Ye
cipher5 :: [Char] -> Int -> [Char]
cipher5 [] _ = []
cipher5 (s:ss) n = (rotate5 s n) : (cipher5 ss n)

rotate5 :: Char -> Int -> Char
--rotate a b = intToDigit(digitToInt(a)+b)
rotate5 a b=chr((ord a)+b)


-- 模式匹配
--Pattern Matching
--A function can have multiple patterns
--Almost like overloading methods in Java or C++

guess :: Int -> [Char]
guess 42 = "correct!"
guess x  = "wrong guess!"
	    

--Each pattern has the same type declaration

```
