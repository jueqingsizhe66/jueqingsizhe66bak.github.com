---
layout: post
title: "let and letrec makes programming orient object"
date: 2016-02-16 21:35:15 +0800
comments: true
categories: scheme
---

面向对象已经大行其道，而在scheme是如何实现，本文仅仅是刹那的思路，
可以通过let来定义局部变量，而可以通过letrec定义method的过程来实现
面向对象的数据抽象的能力。
<!--more-->

## let 不具备局部传递功能
``` scheme
(define  x 10)  ;;; x的值不会影响到局部变量的值
(let ((x 1) (y 2) (z 3))
   (list  x y z))

(define  x 10) ;;;x的值会影响到局部变量的值
(let ((x 1) (y x) (z 3))
   (list  x y z))

;结果
    (1 2 3)
    (1 10 3) 
```

### 第一次注意看let的实现

``` scheme

(define-syntax
    (syntax-rules ()
        ((let ((var expr) ...) body ...) ((lambda (var ...) body ...) expr ...))))
```

(let ((x 2) (y 3)) (+  x y)) 转变为
((lambda (x y) (+ x y)) 2 3) 的形式.

## let star 具备局部传递功能
``` scheme
(let* (( x  1) (y  x) (z 3))
  (list x  y z))


;结果
    (1 1 3)
```
另外有一种叫做fluid-let的暂时没有在这边出现


## letrec 具备自指的能力(递归)
如果想定义一个procedure局部变量？
如果用let 或者let\*都无法达到此目的，因为他们不能自指（也就是没有递归的能力）
而如果是letrec则是拥有此功能

``` scheme
(letrec
    ((my-even? (lambda (x)
              (if (zero? x) #t
                 (my-odd? (-  x 1)))))
     (my-odd? (lambda (x)
                (if (zero? x) #f
                   (my-even? (- x 1))))))
  (my-even? 10) ;
  (my-odd? 10) ;;只会打印出最后一个结果
  )


;结果
    #f
```

### Named Let 一种更加紧凑的letrec

在上文，let是用来指定局部变量，而letrec是来指定procedure，
现在我们可以用Named let来使letrec写起来更加紧凑些，但是
不适合书写多个局部方法的情况，比如：

``` scheme
(letrec ((countdown (lambda (i)
                      (if(= i 0) 'ole
                         (begin
                           (display i)
                           (newline)
                           (countdown (- i 1)))))))
  (countdown 10)) 

结果:
    10
    9
    8
    7
    6
    5
    4
    3
    2
    1
    'ole
```

``` scheme
(let countdown ((i 10))
  (if (= i 0) 'ole
     (begin
       (display i)
       (newline)
       (countdown (- i 1)))))

结果:
    10
    9
    8
    7
    6
    5
    4
    3
    2
    1
    'ole
```
## 进一步实现留待以后
