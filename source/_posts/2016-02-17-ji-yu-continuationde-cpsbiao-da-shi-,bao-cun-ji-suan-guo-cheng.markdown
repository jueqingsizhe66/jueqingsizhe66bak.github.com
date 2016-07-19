---
layout: post
title: "基于continuation的cps表达式，保存计算过程"
date: 2016-02-17 16:55:50 +0800
comments: true
categories: scheme
---

CPS其实是实现branch分支的一种scheme技术，对于程序流程的控制具有
重要的作用。下面就一个细节对CPS进行简单说明。
<!--more-->

``` scheme
(define r #f)
(+ 1 (call/cc
(lambda (k)
(set! r k)
(+ 2 (k 3)))))

(r 0)
(r 10)

结果：
    4     ==> (+ 1 3)
    1     ==> (+ 1 0)
    11    ==> (+ 1 10)
分析：
    一般形式是 (+ 1 [])
    []代表着continuation的值，而整个continuation的返回值确是(+ 3 [c])
    在第一种情况下是3,紧接着0,最后10

```


## 如果改变+1 为+3呢？

``` scheme

(+ 3 (call/cc
(lambda (k)
(set! r k)
(+ 2 (k 3)))))

(r 0)
(r 10)

结果:
    6     ==>(+ 3 3)
    3     ==>(+ 3 0)
    13    ==>(+ 3 10)


分析：
    一般形式是 (+ 3 [])
    []代表着continuation的值，而整个continuation的返回值确是(+ 3 [c])
    在第一种情况下是3,紧接着0,最后10
```

## CPS分析最好记住下面的分析方法

具体的过程参考[Dead_Program][1],包括rember8的修改。
``` scheme
Let's trace (rember8  (lambda (x) x))

ls | k

'(1 2 8 3 4 6 7 8 5) | (lambda (x) x) = id
'(2 8 3 4 6 7 8 5)   | (lambda (x) (id (cons 1 x))) = k2
'(8 3 4 6 7 8 5)     | (lambda (x) (k2 (cons 2 x))) = k3  ==>不断存储计算结果

Once we hit the 8, we apply (k (cdr ls)) where k is k3 and ls is '(8 3
4 6 7 8 5)

(k3 '(3 4 6 7 8 5)) = (k2 (cons 2 '(3 4 6 7 8 5)))      ==> what is k2?
(k2 '(2 3 4 6 7 8 5)) = (id (cons 1 '(2 3 4 6 7 8 5)))  ==> what is id?
(id '(1 2 3 4 6 7 8 5)) = '(1 2 3 4 6 7 8 5)            ==> what is final result?

And we're done.

```

之所以写这个note的目的也是为了能够分析continuation的返回值。

[1]:http://jueqingsizhe66.github.io/blog/2016/02/09/its-a-dead-program-dot-how-to-let-it-alive/ 
