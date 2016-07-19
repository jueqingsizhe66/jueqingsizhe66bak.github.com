---
layout: post
title: "scheme环境的几种表现形式-represtations"
date: 2016-03-03 02:05:32 +0800
comments: true
categories: scheme
---

scheme的解释器的构造，都需要enviroment类型的参与，environment类型的抽象确是影响到语言的具体性能。
<!--more-->

主要有以下四种类型(另一种未实现)

1. [环境关联表形式出现(cons cons...)](#1) 
2. [环境的列表出现 (list var val env)](#2)
3. [环境的过程实现(lambda ()形式)](#3)
4. [环境的一种较为特殊的形式成堆变量的实现(list (list var) (list val))](#4)




<h2 id="1"> 1. 环境关联表形式出现 </h2>

此时是每一次都把一个键值对存入到环境中，逐次存入的过程(采用 *cons cons的形式*)。

``` scheme
((a . 1) (b . 1)  (c . 1))

empty-env  is '()
```

具体实现如下所示

``` scheme

(define empty-env
  (lambda() '()))

(define extend-env
  (lambda (var val env)
    (cons (cons var val)
	  env)))

(define apply-env
  (lambda (env search-var)
    (cond
     ((null? env)
      (report-no-binding-found search-var))
     ((eqv? (caar env) search-var)
      (cdr (car env)))
     (else
      (apply-env (cdr env) search-var)))))

(define report-no-binding-found
  (lambda (search-var)
    (error 'apply-env "No binding for: " search-var)))

(define e
  (extend-env 'd 6
    (extend-env 'y 8
       (extend-env 'x 7
	  (extend-env 'y 14
	    (empty-env))))))

(equal?? (apply-env e 'd) 6)
(equal?? (apply-env e 'y) 8)
(equal?? (apply-env e 'x) 7)
(equal?? (apply-env e 'd) 6)

```


一种比较特殊的就是
``` scheme
 a=1 b=2  a=3  c=5

 ((a 1 3) (b 2) (c 5))

 empty-env is '()
```



针对当前的关联表的一个很小的拓展

### 增加一个`has-binding` 实现

``` scheme

(define has-binding?
  (lambda (env var)
    (cond
     ((null? env) #f)
     ((eqv? (caar env) var) #t)
     (else
      (has-binding? (cdr env) var)))))


(define e
  (extend-env 'd 6
     (extend-env 'y 8
        (extend-env 'x 7
           (extend-env 'y 14
              (empty-env))))))

(equal?? (has-binding? e 'd) #t)
(equal?? (has-binding? e 'y) #t)
(equal?? (has-binding? e 'x) #t)
(equal?? (has-binding? e 'z) #f)

```


### 拓展一个列表输入的功能`extend-env*`

``` scheme
(define extend-env*
  (lambda (var-list val-list env)
    (if (null? var-list)
	env
	(let ((var (car var-list))
	      (val (car val-list)))
	  (extend-env* (cdr var-list)
		       (cdr val-list)
		       (extend-env var val env))))))

(equal?? (has-binding? (extend-env* '(A) '(1) e) 'A) #t)
(equal?? (has-binding? (extend-env* '(A B C) '(1 2 3) e) 'C) #t)

```

<h2 id="2"> 2.环境的列表出现  </h2>

此时使用一个标志位`extend-env`每一次都把一个键值对存入到list环境中，<font color="red">嵌套</font>存入的过程(采用 *cons cons的形式*)。

``` scheme
 (extend 
   (list 'extend-env var exp (extend                        ;;;====其实'extend-env 有点多余
                         (list 'extend-env var exp (extend  ;;;====其实'extend-env 有点多余
                                               (empty-env))))))

 (1 (2 (3 6)))
```


且看具体实现如下

``` scheme
;; empty-env : () -> Env
(define empty-env
  (lambda () (list 'empty-env)))

;; extend-env : Var * Schemeval * Env -> Env
(define extend-env
  (lambda (var val env)
    (list 'extend-env var val env)))     ;;;;;;;==================>关键位置

;; apply-env : Env * Var -> Schemeval
(define apply-env-rec
  (lambda (env search-var all)
    (cond
     ((eqv? (car env) 'empty-env)
      (report-no-binding-found search-var all))
     ((eqv? (car env) 'extend-env)
      (let ((saved-var (cadr env))        ;;;;;;;==================>关键位置  
	    (saved-val (caddr env))           ;;;;;;;==================>关键位置
	    (saved-env (cadddr env)))         ;;;;;;;==================>关键位置
	(if (eqv? search-var saved-var)
	    saved-val
	    (apply-env-rec saved-env search-var all))))  ;;;;;;;==================>不断更新最新的位置look up variable in the saved-env
     (else
      (report-invalid-env env)))))

(define apply-env
  (lambda (env search-var)
    (apply-env-rec env search-var env)))


(define display-env-rec
  (lambda (env)
    (if (eqv? (car env) 'extend-env)
	(let ((saved-var (cadr env))
	      (saved-env (cadddr env)))
	  (printf " ~a " saved-var)
	  (display-env-rec saved-env)))))


(define display-env
  (lambda (env)
    (printf "env: ")
    (display-env-rec env)
    (newline)))

(display-env e)

(define report-no-binding-found
  (lambda (search-var all)
    (display-env all)
    (error 'apply-env "No binding for" search-var)))

(define report-invalid-env
  (lambda (env)
    (error 'apply-env "Bad environment" env)))

(define e
  (extend-env 'd 6
      (extend-env 'y 8
         (extend-env 'x 7
            (extend-env 'y 14
                (empty-env))))))

(equal?? (apply-env e 'd) 6)
(equal?? (apply-env e 'y) 8)
(equal?? (apply-env e 'x) 7)


```

<h2 id="3"> 3.环境的过程实现</h2>


环境的过程实现是比较难理解的，因为该过程更少了递归的逐渐减少的表像，感觉所有都不变。

具体实现如下：

``` scheme
;; data definition:
;; Env = Var -> Schemeval

;; empty-env : () -> Env
(define empty-env
  (lambda ()
    (cons (lambda (search-var)
	    (report-no-binding-found search-var))
	  (lambda ()
	    #t))))


;; extend-env : Var * Schemeval * Env -> Env
(define extend-env
  (lambda (saved-var saved-val saved-env)
    (cons (lambda (search-var)
	    (if (eqv? search-var saved-var)
		saved-val
		(apply-env saved-env search-var)))
	  (lambda ()
	    #f))))

;; apply-env : Env * Var -> Schemeval
(define apply-env
  (lambda (env search-var)
    ((car env) search-var)))

(define empty-env?
  (lambda (env)
    ((cdr env))))

(define report-no-binding-found
  (lambda (search-var)
    (error 'apply-env "No binding for ~s" search-var)))

(define report-invalid-env
  (lambda (env)
    (error 'apply-env "Bad environment: ~s" env)))

(define e
  (extend-env 'd 6
     (extend-env 'y 8
        (extend-env 'x 7
           (extend-env 'y 14
              (empty-env))))))

(equal?? (apply-env e 'd) 6)
(equal?? (apply-env e 'y) 8)
(equal?? (apply-env e 'x) 7)

(equal?? (empty-env? (empty-env)) #t)
(equal?? (empty-env? e) #f)

```


<h2 id="4"> 4.环境的一种较为特殊的形式的实现 </h2>

如果若有的定义扎堆存在呢？也就是把所有变量放在一个列表中，值放在一个列表里面。

``` scheme
 x=3 y=4 z=5 c=6

 表现为 ((x y z c) (3 4 5 6))

 此时 empty-env is (()  ())
```



``` scheme

(define empty-env
  (lambda() '()))


(define extend-env
  (lambda (var val env)
    (cons (list
	   (list var) (list val))
          env)))

(define extend-env*
  (lambda (var-list val-list env)
    (if (null? var-list)
	env
	(cons (list var-list val-list)
	      env))))

;; return a pair, for distinguish with val is #f
(define apply-current
  (lambda (vars vals search-var)
    (if (null? vars)
	(cons #f '())
	(if (eqv? (car vars) search-var)
	    (cons #t (car vals))
	    (apply-current (cdr vars) (cdr vals) search-var)))))

(define apply-env
  (lambda (env search-var)
    (if (null? env)
     (report-no-binding-found search-var)
     (let ((val (apply-current (caar env) (cadar env) search-var)))
       (if (car val) (cdr val)
	   (apply-env (cdr env) search-var))))))

(define report-no-binding-found
  (lambda (search-var)
    (error 'apply-env "No binding for: " search-var)))


(define has-binding?
  (lambda (env var)
    (if (null? env)
	#f
	(let ((val (apply-current (caar env) (cadar env) var)))
	  (if (car val)
	      #t
	      (has-binding? (cdr env) var))))))


(define e (empty-env))
(equal?? e '())

(define e (extend-env 'z 10 e))
(equal?? e '(((z) (10))))

(define e (extend-env* '(a b c d) '(1 2 3 4) e))
(equal?? e '(((a b c d) (1 2 3 4)) ((z) (10))))

(equal?? (apply-env e 'z) 10)
(equal?? (apply-env e 'd) 4)

(equal?? (has-binding? e 'z) #t)
(equal?? (has-binding? e 'd) #t)
(equal?? (has-binding? e 'm) #f)

```


## 结论 

上述所谈的4种环境的实现，其实也就对应着四种不同的解释器的实现。环境实现的不同，对应者整个解释器的提取过程也就不一样。
整个解释器独立的解释每一个表达式的时候，所采用的方法也就会有所不一样。所以设计一个好的environment，对应的就是设计一个好的
数据结构，用于程序program解析的时候需要使用到的变量或者表达式数据，至关重要！
