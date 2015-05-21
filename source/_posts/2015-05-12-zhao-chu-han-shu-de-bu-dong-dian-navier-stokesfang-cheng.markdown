---
layout: post
title: "找出函数的不动点--Navier-Stokes方程"
date: 2015-05-12 03:13:50 +0800
comments: true
categories: scheme
---

##Beautiful code!

数x为函数的f的不动点（f可以代表N-S方程），如果满足f(x)=x,则称x
为函数f的不动点。
性质 f(x),f(f(x)),f(f(f(x)))...., change the x's value if f(x)=x,then
you found!
<!--more-->

## Source code
``` scheme
(define tolerance 0.000001)
(define (fixed-point f first-guess)
  (define (close-enough? v1 v2)
    (< (abs (-  v1 v2)) tolerance))
  (define (try guess)
    (let ((next (f guess)))
      (if (close-enough? guess next)
         next
         (try next))))
  (try first-guess))

;;;solve cos(x)=x
(fixed-point cos 1.0)


;;(cos 0.7390855263619245)
;;0.7390848683867142
;;(abs (-  (cos 0.7390855263619245) 0.7390855263619245))
;;6.579752103164083e-07


;;;solve y=siny+cosy   x=f(x) ====> Navier Stokes equation
(fixed-point (lambda (y) (+ (sin y) (cos y))) 1.0)

;;It is what we want!!!
;;;(abs (- ((lambda (y) (+ (sin y) (cos y))) 1.2587277968014188) 1.2587277968014188)) 
;;6.26112570678572e-07


;;;;One method to calculate the square value
;;because x^2 = y  so x = y/x (x =f (x) next we  will use the fixed-point thinking) we should continue change the x's value!
(define (sqrt-iter guess x)
  (if (good-enough? guess x)
     guess
     (sqrt-iter (improve guess x)
               x)))

(define (square x)
  (* x x))
;;;good-enough? is similar to close-enough？
(define (good-enough? x y)
  (< (abs (- (square x) y)) 0.00001))

;;x^2=y   x=y/x   x+x=y/x+x  x=(y/x+x)/2
(define (improve guess x)
  (average guess (/ x guess)))

(define (mysqrt x)
  (sqrt-iter 1.0 x))


;;(mysqrt 4)
;;2.0000000929222947

;;;Use fixed-point thinking 

(define (mysqrt_death_fixed x)
  (fixed-point (lambda (y) (/ x y))
              1.0))

;;(mysqrt_death_fixed 4)
;;;wrong !!!  can not convenge!!!! why?

;;because   y2= x/y1  y3=x/y2=x/(x/y1)=y1 ====>death recycle===> so we use average

(define (mysqrt_fixed x)
  (fixed-point (lambda (y) (average y (/  x y)))
              1.0))

(mysqrt_fixed 4)

;;;ok fine
;;2.000000000000002====>这也叫做平均阻尼定义！常用于数值计算当中
;;在不动点的搜寻中，作为帮助收敛的手段！！！！
```
