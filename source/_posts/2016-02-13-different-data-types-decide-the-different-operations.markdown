---
layout: post
title: "Different data types decide the different operations"
date: 2016-02-13 13:33:06 +0800
comments: true
categories: scheme
---

Data types such as list,vector,array,linkedlist,tree etc, all have the self operations 
to add,delete,modify the content referred.
<!--more-->

## List

``` scheme
    (define list-sum
     (lambda (lst)
      (if (null? lst)
       0
       (+ (car lst) (list-sum (cdr lst))))))
    (list-sum '(1 2 3 4))

```

## Vector

``` scheme
    (define vector-sum
     (lambda (vec)
      (define vector-sum-help
       (lambda (vec n)
        ;(if (eq? n 0)
            (if (zero? n)
             (vector-ref vec 0)
             (+ (vector-ref vec n) (vector-sum-help vec (- n 1))))))
       (let ((n (vector-length vec)))
        (if (zero? n)
         0
         (vector-sum-help vec (- n 1)))))) ;;;wrong  (n)
     (vector-sum (vector 1 2 3 4))
```

## The box function in the TSS

``` scheme
     box 的作用虽然有点类似 kons kar kdr
     (define kons (lambda (kar kdr) (lambda (selector) (selector kar kdr))))
    (define kar (lambda (c) (c (lambda (a d) a))))
    (define kdr (lambda (c) (c (lambda (a d) d))))
(define kdr2 (lambda (c) (c (lambda (a d) (quote d)))))

    (kar (kons 'a 'b))
    (kdr (kons 'a 'b))
    (kdr2 (kons 'a 'b))
```

    Now let's see the function of the box.

``` scheme

    (define (box it)
     (lambda (sel)
      (sel it (lambda (new)
               (set! it new)))))

    (define (setbox box new)
     (box (lambda (it set)
           (set new))))

    (define (unbox box)
     (box (lambda (it set)
           it)))
```

    我们使用的前提是通过box进行cons的作用
    比如setbox
```
    ((lambda (it set) (set new)) it (lambda (new) (set! it new)))
```
也就是 it对一个it 而set对应 (lambda (new) (set! it new))
    所以这边真正起作用的就是第一个lambda里面的(set new)返回值
    比如unbox
```
    ((lambda (it set) it) it (lambda (new) (set! it new)))
```
也就是此时 it依然对应it set对应的是(lambda (new) (set! it new))
    所以这边真正起作用的就是第一个lambda里面的it返回值

    所以box只不过是提供了一种映射结构，想到那关于数据结构的创建，把所有的基于这个框架的
    变体放入首个位置，类似closure的table位置
