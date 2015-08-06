---
layout: post
title: "伟大的范畴理论"
date: 2015-05-21 20:56:51 +0800
comments: true
categories: Haskell
---


[范畴理论](http://yogsototh.github.io/Category-Theory-Presentation/)是数学上的一种新的语言和框架结构.
对于程序员来说，它是另一种思考方式，一种极度有效的方式来提取规律(Extremely
efficient for generalization)
Programming is doing Math.编成的工作其实就是数学的工作。

范畴是一种表达事物(things) 和路径(ways)  to go between things.
这可以参考[thinking like a git](http://think-like-a-git.net/)，因为他也是一种基于范畴理论，
而延伸出来的实际产物，方便程序员对于development的管理。
<!--more-->

A Category C is defined by:

+ Objects ob\(C\),
+ Morphisms hom\(C\),
+ a Composition law (∘) 

Functor : (ob->ob)
![Functor part1](http://yogsototh.github.io/Category-Theory-Presentation/categories/img/mp/functor.png )

Functor : (hom->hom)
![Functor part2](http://yogsototh.github.io/Category-Theory-Presentation/categories/img/mp/functor-morphism.png)



Endofunctors:
![自指](http://yogsototh.github.io/Category-Theory-Presentation/categories/img/mp/functor-morphism.png)


![一只可爱的猫](http://yogsototh.github.io/Category-Theory-Presentation/categories/img/fractalcat.jpg )
## Categories and functors form a category: Cat
+ ob(Cat) are categories
+ hom(Cat) are functors
+ ∘ is functor composition



## A Haskell Functor is a type F :: * -> * which belong to the type class Functor ; thus instantiate fmap :: (a -> b) -> (F a -> F b).

+ F: ob(Hask)→ob(Hask)
+ & fmap: hom(Hask)→hom(Hask)

## The couple (F,fmap) is a Hask's functor if for any x :: F a:
``` haskell
fmap id x = x
fmap (f.g) x= (fmap f . fmap g) x
```
