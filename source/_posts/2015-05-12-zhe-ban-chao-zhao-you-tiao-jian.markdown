---
layout: post
title: "折半查找有条件"
date: 2015-05-12 02:46:03 +0800
comments: true
categories: scheme
---
You want to solve F(x)=0,
if f(a)<0<f(b), so the value must be inside the [a,b].

<!--more-->
``` scheme
#lang racket
(define (close-enough? x  y)
  (< (abs (-  x y)) 0.00001))

(define (average a b)
  (/ (+  a b) 2.0))
(define (search f neg-point pos-point)
   (let ((midpoint (average neg-point pos-point)))
     (if (close-enough? neg-point pos-point)
        midpoint
        (let ((test-value (f midpoint)))
          (cond ((positive? test-value)
                 (search f neg-point midpoint))
               ((negative? test-value) 
                (search f midpoint pos-point))
               (else midpoint))))))

;;必须判断一下，如果同号则无法使用折半查找
(define (half-interval-method f a b)
  (let ((a-value (f a))
          (b-value (f b)))
     (cond ((and (negative? a-value) (positive? b-value))
            (search f a b))
          ((and (negative? b-value) (positive? a-value))
           (search f  a b))
          (else
           (error "values are not of opposite sign" a b)))))

(half-interval-method (lambda (x) (- (* x x x) (* 2 x) 3))
                       1.0
                       2.0)
```
