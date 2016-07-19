---
layout: post
title: "The Fourth Interpreter about the traceproc"
date: 2016-02-27 21:49:18 +0800
comments: true
categories: scheme
---

程序的过程追踪能力体现着一个独特的优势，本文在[First Interpreter][1],[Second Interpreter][2],[Third Interpreter][3]基础上实现新的解释器功能。

实现效果:
```
    enter: x = #(struct:num-val 30)
    exting: result = #(struct:num-val 29)
```
<!--more-->

<font size="2" color="red">老阮不狂谁会得？出门一笑大江横！</font>
## lang.scm修改程序追踪部分

``` scheme
      ;;增加了trace-proc
          (expression ("traceproc" "(" identifier ")" expression) traceproc-exp)
```
至于proc的语法并不需要做任何的修改。

## data-structures.scm 进行部分修改

1. 在修改trace-proc实现的情况下还得修改proc?

原先的proc?实现
``` scheme
  ;; proc? : SchemeVal -> Bool
  ;; procedure : Var * Exp * Env -> Proc
  (define-datatype proc proc?
    (procedure
     (var symbol?)
     (body expression?)
     (env environment?)))
```

改进后的`proc?`实现,注意trace 是`boolean?`类型。
``` scheme
  ;; proc? : SchemeVal -> Bool
  ;; procedure : Var * Exp * Env -> Proc
  (define-datatype proc proc?
    (procedure
     (var symbol?)
     (body expression?)
     (env environment?)
     (trace boolean?)))
```

## interp.scm 修改程序追踪部分

由于proc?的类型实现发生了变化，所以需要进行对应的修改apply-procedure和value-of。

### apply-procedure的修改
原先的apply-procedure实现：

``` scheme

(define apply-procedure
    (lambda (proc1 val)
      (cases proc proc1
        (procedure (var body saved-env)
          (value-of body (extend-env var val saved-env))))))
```

改进后的apply-procedure增加了trace?的判断

``` scheme
;; apply-procedure : Proc * ExpVal -> ExpVal
  ;; Page: 79
  (define apply-procedure
    (lambda (proc1 val)
      (cases proc proc1
        (procedure (var body saved-env trace?)
                  (if trace? (printf "enter: ~a = ~a\n" var val) '**proc**)
                  (let ((value (value-of body (extend-env var val saved-env))))
                    (if trace? (printf "exting: result = ~a\n" value) '**proc**)
                    value)))))
```

### value-of 的修改

原先的value-of实现：
``` scheme
  (proc-exp (var body)
                 (proc-val (procedure var body env)))
```

新改进的的value-of 实现:
``` scheme
    (proc-exp (var body)
                 (proc-val (procedure var body env #f)))
    (traceproc-exp (var body)
                  (proc-val (procedure var body env #t)))
```

## 测试追踪修改的结果

``` scheme
(run "proc(x) -(x, 1)")
(run "(proc(x) -(x,1)  30)")
(run "let f = traceproc (x) -(x,1) in (f 30)")
(run "(proc(f)(f 30)  proc(x)-(x,1))")
(run "let x = 3 in -(x,1)")

    (proc-val (procedure 'x (diff-exp (var-exp 'x) (const-exp 1)) (list (list 'i (num-val 1)) (list 'v (num-val 5)) (list 'x (num-val 10))) #f))
    (num-val 29)
    enter: x = #(struct:num-val 30)
    exting: result = #(struct:num-val 29)
    (num-val 29)
    (num-val 29)
    (num-val 2)
```

******
<font size="9" color="yellow">老阮不狂谁会得？出门一笑大江横！</font>

[1]:http://jueqingsizhe66.github.io/blog/2016/02/25/first-interpreter-from-eopl/ 
[2]:http://jueqingsizhe66.github.io/blog/2016/02/27/the-second-interpreter-from-one/ 
[3]:http://jueqingsizhe66.github.io/blog/2016/02/27/the-third-interpreter-implementing-proc/ 
