---
layout: post
title: "Thinking about the dot symbol in haskell"
date: 2015-05-20 17:30:36 +0800
comments: true
categories: Haskell
---

In haskell you can use () to embrace the function into an application
``` haskell
not odd 4   -->error
not (odd 4) -->right
```

you can also use $ (called a function application),it makes functions right
associative.
``` haskell
not $ odd 4
```
but more pretty and elegant method is use . 
<!--more-->

It composes functions in a readable manner
f(g(h(k(x)))) is ugly
(f.g.h.k)(x) is pretty

``` haskell
(not.odd) 4
(length.head.words) "university of NCEPU"
```


Haskell's all functions
+ Data.List
+ Data.Char
+ Data.Map
+ Data.Set
.....and more than 350 others
### In the Data.List module:

``` haskell
Main Data.List> concat ["under","stand","ing"]
"understanding"
Main Data.List> any (==0) [1,1,1,1,1,0,0]
True
Main Data.List> sort "helo"
"ehlo"

--And over 200 more!
```


### Also there are Data.Char

``` haskell
Main Data.List> import Data.Char
Main Data.List Data.Char> isNumber 'h'
False
Main Data.List Data.Char> toUpper 't'
Main Data.List Data.Char> Data.List.map ord ['A'..'F']
[65,66,67,68,69,70]

--and over 100 more!
```


### There are also some in Data.Map

``` haskell

Main Data.List Data.Char> import Data.Map
Main Data.List Data.Char Data.Map> let m = fromList[("CS","Computer Science"),("TLS","The Little Scheme"),("TSS","The Season Scheme")]
Main Data.List Data.Char Data.Map> m
fromList [("CS","Computer Science"),("TLS","The Little Scheme"),("TSS","The Season Scheme")]
Main Data.List Data.Char Data.Map> keys m
["CS","TLS","TSS"]


Main Data.List Data.Char Data.Map> Data.Map.lookup "CS" m
Just "Computer Science"

and over 200 more!!!!
```


### We can continue see Data.Set

``` haskell

Main Data.List Data.Char Data.Map Data.Set> let a =Data.Set.fromList[1..58]
Main Data.List Data.Char Data.Map Data.Set> let b =Data.Set.fromList[53..100]
Main Data.List Data.Char Data.Map Data.Set> Data.Set.intersection a b
fromList [53,54,55,56,57,58]

Main Data.List Data.Char Data.Map Data.Set> Data.Set.findMax $ Data.Set.union a b
100

--and even 100 more
```

*There are one problem: Some function multiple definition in the Data.List and Data.Map,So take note!*

Solve the strong passwd
``` haskell

strong x = length x > 14
           && any isLower x
           && any isUpper x
            && any isDigit x


-- Designed by Jared Candelaria

--import Data.Char

strong :: String -> Bool
strong password = all ($ password) requirements
    where
        requirements = [minLength 15, 
                        any isUpper, 
                        any isLower, 
                        any isDigit]
        minLength n str = n <= length str
```


