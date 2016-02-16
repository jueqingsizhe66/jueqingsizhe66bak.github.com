---
layout: post
title: "Data Representation-The Same interface with Different implementation"
date: 2016-02-15 16:39:26 +0800
comments: true
categories: scheme
---

我们设定一个接口，该接口实现包括三个constructor和1个observer

1. zero
2. successor
3. predecessor
4. zero?

当然也可以拓展该接口包括scheme-val->my-val,my-val->scheme-val,plus等。
下面看一下如何实现三个相同接口的实现，从而体味data abstraction(data 
representation or data boundary).
<!--more-->

## unary implementation


### 辅助测试程序
``` scheme
(define-syntax equal??
  (syntax-rules ()
    ((_ test-exp correct-ans)
     (let ((evaluate-ans test-exp))
       (if (not (equal? evaluate-ans correct-ans))
          (printf "~s returned ~s~%, should have returned ~s~%"
                 'test-exp
                 evaluate-ans
                 correct-ans)
          'OK)))))

(define report-unit-tests-completed
(lambda (fn-name)
  (printf "unit tests completed: ~s~%" fn-name)))

```

``` scheme
(let ()
  (define zero (lambda () '()))
  (define is-zero? (lambda (n) (null? n)))
  (define successor (lambda (n) (cons #t n)))
  (define predecessor (lambda (n) (cdr n)))
  
  (define plus
    (lambda (x  y)
      (if (is-zero? x)
         y
         (successor (plus (predecessor x) y)))))
 (report-unit-tests-completed 'unary-representation)
  )

```
## scheme value implementation
``` scheme
(let()
  (define  zero (lambda () 0))
  (define is-zero? (lambda (n) (zero? n)))
  (define successor (lambda (n) (+ n 1)))
  (define predecessor (lambda (n) (- n 1)))

  (define plus
    (lambda (x y)
      (if (is-zero? x)
         y
         (successor (plus (predecessor x) y)))))

  (equal??  (plus 3 7) 10)
 (printf "~s test completed.~% " 'scheme-number-presentation)
  )


```

## 5-inverse implementation
``` scheme

(let ()
  (define zero (lambda () 5))
  (define is-zero? (lambda (n) (=  n 5)))
  (define successor (lambda (n) (- n 5)))
  (define predecessor (lambda (n) (+ n 5)))
  (define plus
    (lambda (x y)
      (if (is-zero? x)
         y
         (successor (plus (predecessor x) y)))))

  (define scheme-int->my-int
    (lambda (n)
      (if (zero? n) (zero)
         (successor (scheme-int->my-int  (- n 1))))))

  (define my-int->scheme-int
    (lambda  (n)
      (if (is-zero? n) 0
         (+ 1 (my-int->scheme-int (predecessor n))))))

  (equal??
   (my-int->scheme-int
    (plus (scheme-int->my-int 3)
         (scheme-int->my-int 7)))
   10)
(printf "~s unit tested completed" 'reverse-number-representation)
  )
```


## 拓展整体的接口程序

在上面的(let ....) 中都可以添加如下的接口slots,
你会发现他们的实现在任意一种实现都是一样的过程，只要4个基本接口
实现一样那他们的result is the same.

``` scheme
  (define scheme-int->my-int
    (lambda (n)
      (if (zero? n) (zero)
         (successor (scheme-int->my-int  (- n 1))))))

  (define my-int->scheme-int
    (lambda  (n)
      (if (is-zero? n) 0
         (+ 1 (my-int->scheme-int (predecessor n))))))

  (define plus
    (lambda (x y)
      (if (is-zero? x)
         y
         (successor (plus (predecessor x) y)))))

```



通过者三个实现过程认识[v] is the data representation of the v in certain
interface. 只要确定接口，那么实现有n种情况只要满足你的要求，从而达到问题的
求解。
