---
layout: post
title: "Haskell在脑海里的踪迹"
date: 2015-05-29 14:59:44 +0800
comments: true
categories: Haskell
---


基于范畴理论，在每一个范畴里面都包含一个objects和fmap关系，利用functor映射到不同的category。
可以继续思考的是一个fmap:(\*->\*)
一个fmap是可以充当形参的，因为他正如在一个[List module](https://www.haskell.org/onlinereport/list.html#sect17.3)中
一个一个的破折号和箭头构成的map关系被放进List这个category中。
<!--more-->
``` haskell
module List ( 
    elemIndex, elemIndices,
    find, findIndex, findIndices,
    nub, nubBy, delete, deleteBy, (\\), deleteFirstsBy,
    union, unionBy, intersect, intersectBy,
    intersperse, transpose, partition, group, groupBy,
    inits, tails, isPrefixOf, isSuffixOf,
    mapAccumL, mapAccumR,
    sort, sortBy, insert, insertBy, maximumBy, minimumBy,
    genericLength, genericTake, genericDrop,
    genericSplitAt, genericIndex, genericReplicate,
    zip4, zip5, zip6, zip7,
    zipWith4, zipWith5, zipWith6, zipWith7,
    unzip4, unzip5, unzip6, unzip7, unfoldr,

    -- ...and what the Prelude exports
    -- []((:), []), -- This is built-in syntax
    map, (++), concat, filter,
    head, last, tail, init, null, length, (!!),
    foldl, foldl1, scanl, scanl1, foldr, foldr1, scanr, scanr1,
    iterate, repeat, replicate, cycle,
    take, drop, splitAt, takeWhile, dropWhile, span, break,
    lines, words, unlines, unwords, reverse, and, or,
    any, all, elem, notElem, lookup,
    sum, product, maximum, minimum, concatMap, 
    zip, zip3, zipWith, zipWith3, unzip, unzip3
    ) where
```


我们所看到
``` haskell
data  Bool  =  False | True deriving 
                             (Read, Show, Eq, Ord, Enum, Bounded)
data Int = -65532|...|-1|0|1|2|...|65532
data Integer = ...-2|-1|0|1|2|3|4|...
```
从这边我们可以看到False ，True， 1，2，3，4....叫做Data constructor
而Bool,Integer叫做Type Constructor,所以你完全可以把类型当作数据，而这个类型包含表达式、函数、数据。


类也包含
+ 原子类型 Integer  Char   Integer->Char(functions mapping from Integer to Char)
+ 结构类型 [Integer] 数字列表 \(Integer,Char\)数字字符tuple

比如：
``` haskell
1::Int
inc::Integer->Integer
[1]::[Integer]
('b',4)::[Char,Integer]

::可以读作 has type类型为
```

******
我们也会看到很多类的继承
``` haskell
  class Foo a => Bar a where ...
  
  instance (Eq a, Show a) => Foo [a] where ...
  
  instance Num a => Bar [a] where ...


  class (Eq a) => Ord a where
    (<),(<=),(>=),(>)::a->a->Bool
    max,min          ::a->a->a
    ----此时Eq叫做Ord的子类
```

其实更多的可以参考[Haskell 98 Report](https://www.haskell.org/onlinereport/standard-prelude.html#sect8.2)

当然你事先得知道类的定义
``` haskell
class cx => C u where { cbody }
```

class 方法被叫做haskell里的顶层声明。
******

而有一些比较有趣的lambda转换
``` haskell
\x->\y->(+) x  y --->  \x y -> (+) x  y

(x+) --> \y->x+y
(+y) --> \x->x+y
(+)  --> \x y -> x+y

(.) :: (b->c) ->(a-b)->(a->c)
f.g  = \x-> f (g x)
```



在你编写函数的时候：
一个数据组和可以变为(x:xs) == x+xs完整的数据组和 ，只不过x代表这个数据组和的head部分，而xs表示tail部分
而如果 s@(x:xs) 其中的s代表就是整个数据组和了，这是一个不错的技巧。

``` haskell
f (x:xs) = x:x:xs
f s@(x:xs) = x:s
```

当然你也可能遇到Wild-cards情况
``` haskell
head (x:_) = x
head (_:xs) = xs
```


******
另外一个分支关于模式匹配的相似效果是case定义
f p11 ... p1k = e1
...
f pn1 ... pnk = en
其中pij是模式组合，

如果你使用case进行书写
``` haskell
f x1 x2 ... xk = case (x1,...,xk) of (p11,...,p1k) -> e1
                                        ...
                                      (pn1,...,pnk)->en
```


比如下面：
``` haskell
--采用模式匹配方式
take _ [] = []
take 0 [] = []
take n (x:xs) = x:take (n-1) xs


--采用case方式
take m ys = case (m,ys) of
    (0,_) -> []
    (_,[]) -> []
    (n,x:xs) -> x:take (n-1) xs
```

******
一个比较重要的functor的定义，因为它用于范畴之间的链接，主要是一个Functor类的实现和fmap函数的定义

``` haskell
class Functor f where
    fmap :: (a->b) -> f a->f b


对应的Functor实例是
instance Functor Tree where
    fmap f (Leaf x) = Leaf (f x)
    fmap f (Branch t1 t2) = Branch (f t1) (f t2)

之所以写上这个你会发现，我们使用
Date Tree a= Leaf a| Branch a a
只是定义了类型，但是该类型（或者说大点，叫做范畴）并没有定义fmap，基本上所有的calss类型定义完之后都需要使用instance Functor进行fmap的
定义,这样对象和映射的组合才够成了范畴。
```

这也是为什么他比较重要，因为我们可能一直在用，但是并未知晓。当然上面需要特别注意的是实例声明额时候使用的是Tree，而不是Tree a.
也就是haskell和c++\java不同的是类型变量和methods的定义是分开的。
Haskell类类似于java的interface，定义的是一个协议，而不是一个定义一个对象本身。而且最重要的一个区别是haskell没有访问控制
修饰符比如public 、private。取而代之的是使用module模块系统用来隐藏和显示一个类的组成。


其实写到这边，模块系统你可能有所理解，当然模式匹配在函数定义中也是训练的很多，参考[Haskell库文档结构]( http://jueqingsizhe66.github.io/blog/2015/05/21/haskellku-wen-dang-jie-gou/)
但是关于instance，你接触、练习还是有点少，所以这部分内容可以继续探索，包括IOMonad（一个新的IO范畴，可以当做一种抽象数据类型）
也包括图形化的制作等。
