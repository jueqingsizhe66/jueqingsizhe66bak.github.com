---
layout: post
title: "So beautiful code"
date: 2015-05-12 02:29:11 +0800
comments: true
categories: scheme
---
So beautiful
``` scheme
(define (f x y)
(define (f-helper a b)
  (+ (* (square a))
      (* y b)
      (* a b)))
  (f-helper (+ 1 (* x y))
            (- 1 y)))

```
<!--more-->
====> f(x,y)=x(1+xy)^2 + y(1-y)+(1+xy)(1-y)
       ==>  a= 1+xy
            b= (1-y)
            f(x,y)=xa^2+yb+ab

====>Continue

``` scheme
(define (f x y)
  ((lambda (a b)
       (+ (* x (square a))
          (* y b)
          (* a b)))
    (+ 1 (* x y))
    (- 1 y)))
```

====>Use let to define the local variables
``` scheme
(define (f x y)
  (let ((a (+ 1 (* x y)))
         (b (- 1 y)))
       (+ (* x (square a))
          (* y b)
          (* a b))))

```
let is just the coat of the ((lambda (a b))

Of course we can use define
``` scheme
(define (f x y)
   (define a (+ 1 (* x y)))
   (define b (- 1 y))
 (+ (* x (square a))
    (* y b)
    (* a b)))
```
SICP_P42

