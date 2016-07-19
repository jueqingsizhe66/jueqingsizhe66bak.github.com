---
layout: post
title: "The Fifth interpreter with the implementation of letrec(Important)"
date: 2016-02-28 06:02:57 +0800
comments: true
categories: scheme
---

letrec的作用是可以定义内部函数。它的实现类似于let，只不过let实现的主要是局部变量，而他是局部的过程（持保留意见)
<!--more-->


## 错误1.

```
  (define-datatype environment environment?
    (empty-env)
    (extend-env
     (bvar symbol?)  ;;;这边是一个 所以是symbol?
     (bval expval?)
     (saved-env environment?))
    (extend-env-rec
     (id symbol?)
     (bvar (list-of symbol?)) ;;这边和proc-val的定义发生了冲突  一个是symbol? 另外一个则是(list-of symbol?)
     (body expression?)
     (saved-env environment?)))
```

而在实际的extend-env-rec的实现中，则是调用了proc-val，所以并且传入到proc-val，为了保证一致性

```
;;;;;;;;;;;;;;;; environment constructors and observers ;;;;;;;;;;;;;;;;
(define apply-env
  (lambda (env search-sym)
    (cases environment env
           (empty-env ()
                      (error 'apply-env "No binding for ~s" search-sym))
           (extend-env (var val saved-env)
                       (if (eqv? search-sym var)
                           val
                           (apply-env saved-env search-sym)))
           (extend-env-rec (p-name b-var p-body saved-env)
                           (if (eqv? search-sym p-name)
                             ;; (proc-val (procedure b-var p-body env))  ;;;注意这里的变化！
                              (proc-val (procedure b-var p-body env #t)) ;;修改了一个bug 目的是可以追踪的作用
                               (apply-env saved-env search-sym))))))
```
所以需要对应的修改proc-val

```
  ;; proc? : SchemeVal -> Bool
  ;; procedure : Var * Exp * Env -> Proc
  (define-datatype proc proc? ;;;注意这边的变化 影响到apply-procedure
    (procedure
     ;(var symbol?)  ;;;为了保证和environment的define-datatype一直改为(list-of symbol?)
     (var (list-of symbol?))
     (body expression?)
     (env environment?)
     (trace boolean?))) ;;;需要考虑#t #f
```
## 错误2.

```
> (run "letrec double(x)=+(x,6) in (double 10)")
enter: (x) = #(struct:num-val 10)
. . cdr: contract violation
  expected: pair?
  given: (num-val 10)
```

原因在于原先的call-exp并没有遍历的过程，而新改进的apply-procedure则是需要遍历，
```
        (call-exp (rator rand)
                 (let ((proc (expval->proc (value-of rator env)))
                       (arg (value-of rand env))) 
                   (apply-procedure proc arg)))
```

改进的call-exp
```
        (call-exp (rator rands)
                 (let ((proc (expval->proc (value-of rator env)))
                       (args (map (lambda (x) (value-of x env)) rands)))
                   (apply-procedure proc args)))
```


## 额外错误

```
 data-structures.scm:7:11: all-defined: not a provide sub-form in: (all-defined)
```

这个错误是因为现在都是在使用`(all-defined-out)`


### 为什么还是会出现如下的错误

``` scheme
> (run "letrec double(x)
            = if zero?(x) then 0 else -((double -(x,1)), 2)
       in (double 6)")
. . extend-env-rec: bad value for bvar field: '(x)
> (run "if zero?(3) then 0 else -(3,2)") 
(num-val 1)
```


