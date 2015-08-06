---
layout: post
title: "The greatest weapons"
date: 2015-05-17 19:01:59 +0800
comments: true
categories: Haskell
---


Everything in Haskell has a Type
Here are some Type declarations. 

These are your greatest weapons. 
<!--more-->
head :: [a] -> a -- gets the first element of a list

tail :: [a] -> [a] -- gets everything but the first element

last :: [a] -> a -- gets the last element of a list

init :: [a] -> [a] -- gets everything but the last element

(++) :: [a] -> [a] -> [a] -- concatenates two lists together

(:) :: a -> [a] -> [a] -- prepends an element to a list

fst  :: (a,b) -> a -- gets the first element of a tuple

snd  :: (a,b) -> b -- gets the second element of a tuple
	    
(!!) :: [a]->Int a

### 数学中λ-calculus的表示法为：

1. λx.t
2. λx[t]

“λ”不具有任何特殊意思，标识着其表达式中的可取一数值x带入。
t是表达式（例如x+3、x^2+2x+1等）。

### 代入数值p的表示法为：

1. (λx.t)p
2. (λx[t])p

******
### Lisp|scheme的λ表示法与数值代入：
``` scheme
(lambda (x) (+ x 1))
((lambda (x) (+ x 1) 10))
```

### Haskell：
``` haskell
\x -> x+1
(\x -> x+1)10

Prelude> map (\x->x+1) [1..10]
[2,3,4,5,6,7,8,9,10,11]

\ para1 para2->(return value) 
\ (para1,para2) -> (return value)
```


### Lambda Calculus

前面提到了Haskell是基于__Lambda Calculus__的，所以在学习Haskell之前，我们有必要了解一下Lambda Caculus的一些基本的内容，方便我们后面正式介绍Haskell。其实，Lambda Calculus是所有函数式语言的基础，要学习FP，最好都了解一下Lambda Calculus。下面对Lambda Calculus做一个简单的介绍：
#### 基本的语法：Lambda Calculus的核心是表达式(Expression)，用FP语言写的程序执行的过程，本质上就是对表达式求值的过程
 
expression := variable | function | application
funciton := λvariable.expression (.前面部分为定义(definition)，后面部分为函数体(body))
application := expression expression

另外一种表述方法:
``` sh
<expr> ::= <identifier> 
<expr> ::= lambda <identifier-list>. <expr> 
<expr> ::= (<expr> <expr>)
```

前两条语法用于生成lambda表达式（lambda函数），如： 
lambda x y. x + y 
haskell里面为了简洁起见用“\”来代替希腊字母lambda，它们形状比较相似。故而上面的定义也可以写成： 
\ x y. x + y

#### 变量的bound与free:

λx.xy →x is bound, y is free


#### 表达式化简（Reduction)的基本法则:
``` sh
α−reduction: λx.E→λy.E[y/x]
β−reduction:((λx.E)z)→E[z/x]
η−reduction(if x is not free in E): λx.(Ex)→E
```

+ Alpha转换公理：例如，“lambda x y. x + y”转换为“lambda a b. a + b”。换句话说，函数的参数起什么名字没有关系，可以随意替换，只要函数体里面对参数的使用的地方也同时注意相应替换掉就是了。

+ Beta转换公理：例如，“(lambda x y. x + y) 2 3”转换为“2 + 3”。这个就更简单了，也就是说，当把一个lambda函数用到参数身上时，只需用实际的参数来替换掉其函数体中的相应变量即可。

[软件工程里面的一条黄金定律：“任何问题都可以通过增加一个间接层来解决](http://mindhacks.cn/2006/10/15/cantor-godel-turing-an-eternal-golden-diagonal/)
#### 变量替换
[y/x]E →  substitute  all occurrences of x in E to y


以上是Lambda Calculus的一些基础知识，读者朋友现在不理解没有关系，等后面讲到Haskell中相关的部分就可以理解了。对于抑制不住自已的好奇心的朋友，可以在这里：[http://en.wikipedia.org/wiki/Lambda_calculus](http://en.wikipedia.org/wiki/Lambda_calculus)做进一步的了解。


在之前做的[lambda的推导](http://jueqingsizhe66.github.io/blog/2015/05/17/cong-lambdadao-simple-plus-complexjie-shi-qi-zai-dao-shu-xing-chou-xiang/)其实都只是针对单个参数，包括[Haskell也是强调单参数的调用](http://shuklan.com/haskell/lec03.html#/0/15)，如果多惨数那就是用curry来解决(funny多参数)
关于多参数的lambda推到比较少见，但是多参数都是可以通过curry实现。

比如Haskell的实现:
``` haskell
Prelude> :type take
take :: Int -> [a] -> [a]
Prelude> let takeFive = take 5
Prelude> :type takeFive
takeFive :: [a] -> [a]
Prelude> takeFive [1..]
[1,2,3,4,5]
	   
```
