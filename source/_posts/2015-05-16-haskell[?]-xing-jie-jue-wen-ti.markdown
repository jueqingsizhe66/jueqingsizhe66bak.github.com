---
layout: post
title: "Haskell一行解决问题"
date: 2015-05-16 18:30:02 +0800
comments: true
categories: Haskell
---

Haskell天生的适合解决数学的组合问题，以及无穷求解问题。
``` haskell
Prelude> [(a,b,c)|c<-[1,3..15],b<-[1,3..15],a<-[1,3..15],a+b+c==30]
[]

Prelude> [(a,b,c)|c<-[1..10],b<-[1..c],a<-[1..b],a^2+b^2==c^2]
[(3,4,5),(6,8,10)]
```
<!--more-->

``` haskell
Prelude> [adjection++" "++noun|adjection<-["sad","blue","dirty","dizzy"],noun<-["he","she","I"]]
["sad he","sad she","sad I","blue he","blue she","blue I","dirty he","dirty she","dirty I","dizzy he","dizzy she","dizzy I"]
```

##Haskell 无穷数列
Haskell的lists用数学上来讲叫做数列
而tuples则叫做数学上的集合。但是不合理的是tuples里面可以有重复的，所以不是一个sets其实

``` haskell
[3*x+2| x<-[3,6,9]] --记号"<-"表示属于,x依次取3,6,9,代入3*x+2,得到数列[11,20,29] 
[x*x| x<-[1..9], x `rem` 3==1] --给出x的范围,还限定x除3余1 
   [(x,y)|x<-[1,2,3],y<-[x..3]] --等于 [(1,1),(1,2),(1,3),(2,2),(2,3),(3,3)] 
```

``` haskell
下面再举几个无限数列的应用的例子: 
    
   squares = [ x*x | x<-[0..]] 
   isSquare n = elem n (takeWhile (<=n) squares)   --判断一个数是否为平方数 
    
   fibs = fibgen 1 1   --Fibonacci 数列 
   fibgen n1 n2 = n1 : fibgen n2 (n1+n2) 
    
   primes = map head (iterate crossout [2..])      --用筛法求素数   
    where  crossout (x:xs)=filter (not.divisible x) xs 
           divisible x y = y `rem` x == 0 
    
  prime = sieve [2..]         ---改进后的素数数列 
  sieve (x:xs) = x : sieve (filter (\y ->y `rem` x /= 0) xs) 
   
  有了这些定义后, take 100 prime 就是取前100个素数, prime(!!)1000 就是取第1000个素数, 因为数列的定义是无限的, 数列的计算是有限, 这样就无须为不同的需要定义不同长度的数列, Hakell处理无限数列的能力实在是令人叹服. 
```

##没有了变量，赋值，和控制语句
  你在作数学题的时候, 从来也没有过把变量看作是存储器, 给变量赋值的概念, 也没有用到for,while循环语句, 而在Haskell中正好抛弃了这些概念. 用Haskell解决问题的思路与人思路非常接近, 比如相当一部分函数以数学归纳法的方式来定义, 对数据的描述性的定义等. 它掩盖了非常细节的问题, 在更高的层次上处理问题. 这样就提高了编程的效率, 提高了代码的可重用性. 
    
   Haskell是世界上公认的语法最优美最简洁的一种语言。Haskell语言是写给人看的，而不是写给机器看的。另一方面，这也使得的Haskell的编译技术成为一个难点, 编译后的程序运行速度比C略慢一些。从以人为本的角度来看，程序员的时间比机器的时间更宝贵，所以Haskell是明智的选择。    
    
   以后各节将更多关注于Haskell编程方面的一些特性, 而不仅仅是做算术. Haskell的高效和强大将得到进一步的证实. 由于Haskell主要是在UNIX平台上发展起来的, 专门针对Windows的类库不是很多. 但Haskell的先进性是不容置疑的, 它的发展只是一个时间的问题

##Haskell教程还缺少什么？

1. Haskell缺少《The Little Scheme》提及的commentdant rules，那些规则适合所有的函数式编程。
2. Haskell减少了()的使用，更加直观的从数学的角度进行编程，更加强调了数学直觉，而忽略掉变量赋值和控制语句。然而方便的同时,也是远离了机器，而升华到语言逻辑上的"什么"的编程，而不是"怎样"的编程,所以对于debug的困难比较难做到，因为已经架构在变量赋值和控制语句之上。 从这点来说，Haskell,相对于命令式编程的c,c++,java等，是一个更高级别的编程。

Haskell的无限数列真的是我见过的编程语言最为强大的。
