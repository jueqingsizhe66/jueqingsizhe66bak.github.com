---
layout: post
title: "插入排序"
date: 2016-02-28 16:42:54 +0800
comments: true
categories: scheme
---


排序算法是一种比较常见的算法，一般包括冒泡排序，插入排序，快速排序等。
本文主要是关于插入排序。
<!--more-->


## 如何把一个数字插入到一个列表中

```
;; using insert sort here

(define insert
  (lambda (lst elem)
    (cond ((null? lst) (list elem))
	  ((< elem (car lst))
	   (cons elem lst))
	  (else (cons (car lst)
		      (insert (cdr lst) elem))))))

(define sort-rec
  (lambda (prev now)
    (if (null? now)
	prev
	(sort-rec (insert prev (car now))
		  (cdr now)))))
(define sort
  (lambda (lst)
    (sort-rec '() lst)))


```


## 提取出判断条件

利用pred代表小于号
```
(define insert
  (lambda (lst elem pred)
     (cond
        ((null? lst) (list elem))
        ((pred elem (car lst))
          (cons elem lst))
        (else (cons (car lst)
                    (insert (cdr lst) elem pred))))))
```


结果：

```
(insert '(1 2 3) '4 <)
    (1 2 3 4)
(insert '(1 2 3) '0 <)
    (0 1 2 3)
(insert '(1 3 2) '4 <)
    (1 3 2 4)    ==> 这不是我们想要的
```

分析发现 insert仅仅是在判断满足条件的时候，直接退出，不管后面的大小问题，
<font color="red">我们不可以判断该元素之后的所有的字符的情况</font>。

然而该程序当作一个最小的模块，也就是加入我逐个提出判断列表，最终实现的是每提取一个元素
都和新构成的排序列表进行判断，那么也就算判断完成。


## 对一个列表进行插入排序

在原先insert的基础上，书写了sort-rec和sort-predicate

+ sort-rec 逐个判断最新的元素和新的临时列表的比较
+ sort-predicate 仅仅是选择判断的标准。

```
(define sort-rec
  (lambda (prev now pred)
    (if (null? now)
        prev
        (sort-rec (insert prev (car now) pred)
                  (cdr now)
                  pred))))

(define sort/predicate
   (lambda (pred lst)
      (sort-rec '() lst pred)))

```


+ sort-rec的prev在逐渐的变大，而now的部分在不断的变小
+ 也就是 sort-rec 的第一号位置不断变大，第二号位由于cdr作用不断变小。
``` scheme

(sort/predicate < '(1 6 5 7))

    prev          now
1.  ()         (1 6 5 7)
2.  (1)        (6 5 7)
3.  (1 6)      (5 7)
4.  (1 5 6)    (7)
5.  (1 5 6 7)  ()

计算结束，返回prev的值
```

于是我们就可以获得正确的结果

```
(sort/predicate < '(1 6 5 7))
    (1 5 6 7)
```

## 利用内部define和letrec改写程序


+ 采用内部的define实现

可以保护函数的实现（只被使用一次）

``` scheme
(define sort/predicate1
  (lambda (pred lst)
    (define sort-rec
      (lambda (prev now pred)
        (if (null? now)
           prev
           (sort-rec (insert prev (car now) pred)
                    (cdr now)
                    pred))))
    (define insert
      (lambda (lst elem pred)
        (cond
          ((null? lst) (list elem))
          ((pred elem (car lst))
           (cons elem lst))
          (else (cons (car lst)
                     (insert (cdr lst) elem pred))))))

     (sort-rec '() lst pred)))

      (sort/predicate1 < '(1 6 5 7))
    
```
+ 采用letrec实现sort/predicate,主要是用`(letrec (() ...) ...)` 结构

``` scheme

(define sort/predicate2
  (lambda (pred lst)
    (letrec ((sort-rec
              (lambda (prev now pred)
                (if (null? now)
                   prev
                   (sort-rec (insert prev (car now) pred)
                            (cdr now)
                            pred))))
             (insert
              (lambda (lst elem pred)
                (cond
                  ((null? lst) (list elem))
                  ((pred elem (car lst))
                   (cons elem lst))
                  (else (cons (car lst)
                             (insert (cdr lst) elem pred)))))))
      
      (sort-rec '() lst pred))))

(sort/predicate2 < '(1 6 5 7))

```

##  结论

1. 每隔离一层抽象，相当于增加了一种变化
2. 了解插入排序的实现过程。
