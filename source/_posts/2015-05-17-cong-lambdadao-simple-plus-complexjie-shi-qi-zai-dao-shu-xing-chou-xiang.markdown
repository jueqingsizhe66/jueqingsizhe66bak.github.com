---
layout: post
title: "从lambda到simple+complex解释器再到树形抽象"
date: 2015-05-17 11:32:07 +0800
comments: true
categories: scheme
---

## 定义一个eternity函数，并尝试是否可以执行

``` scheme
(define eternity
(lambda (x)
   (eternity x)))
```

其实eternity永远在执行着，无法退出

<!--more-->


### 1.下面式子只能计算列表的长度<=0的,且永远不会调用eternity
; Function to calculate length of just empty list.
;
((lambda (len)
  (lambda (l)
    (cond
      ((null? l) 0)
      (else (+ 1 (len (cdr l)))))))
 eternity)


### 2.下面式子只能计算列表的长度<=1的,且永远不会调用eternity
``` scheme
; For lists with length <= 1
;
((lambda (mk-length)
  (mk-length mk-length))
 (lambda (mk-length)
   (lambda (l)
     (cond
       ((null? l) 0)
       (else (+ 1 ((mk-length eternity) (cdr l))))))))


```

### 3.下面式子只能计算列表的长度<=2的,且永远不会调用eternity
``` scheme
; For lists with length <= 2
;
((lambda (len)
  (lambda (l)
    (cond
      ((null? l) 0)
      (else (+ 1 (len (cdr l)))))))
 ((lambda (len)
   (lambda (l)
     (cond
       ((null? l) 0)
       (else (+ 1 (len (cdr l)))))))
  ((lambda (len)
    (lambda (l)
      (cond
        ((null? l) 0)
        (else (+ 1 (len (cdr l)))))))
   eternity)))
 
```


## 如果我们进行一次变换


### 1.把eternity替换成mk-length
  我们发现结果是它可以变成处理无限长度的列表了。
```
(((lambda (mk-length)
  (mk-length mk-length))
 (lambda (mk-length)
   (lambda (l)
     (cond
       ((null? l) 0)
       (else (+ 1 ((mk-length mk-length) (cdr l))))))))
 '(a b c 1 g))  ; 5

```

### 然而我们对他进一步提取发现报错了

``` scheme
((lambda (mk-length)
  (mk-length mk-length))
 (lambda (mk-length)
   ((lambda (len)
     (lambda (l)
       (cond
         ((null? l) 0)
         (else (+ 1 (len (cdr l)))))))
    (mk-length mk-length)))) ; Aborting!: maximum recursion depth exceeded

```


### 为什么不行？
原因在于 我们需要的是lambda类型的procedure才可以，而函数调用形式则是不行。
然而有一种转换方式如下：

(f x) ==> (lambda (arg) (f arg) x)

于是我们得到新的升级版的mk-length

``` scheme

((lambda (mk-length)
  (mk-length mk-length))
 (lambda (mk-length)
   ((lambda (len)
     (lambda (l)
       (cond
         ((null? l) 0)
         (else (+ 1 (len (cdr l)))))))
    (lambda (x) ((mk-length mk-length) x)))))
```

我们发现还是不够满意，我们想把中间的一坨拿到后面去，于是再次抽象一层
也就是我们需要 把(lambda (len) ... (cdr l))部分 移到最外头

``` scheme
((lambda (le)
  ((lambda (mk-length)
    (mk-length mk-length))
   (lambda (mk-length)
     (le (lambda (x) ((mk-length mk-length) x))))))
 (lambda (len)
   (lambda (l)
     (cond
       ((null? l) 0)
       (else (+ 1 (len (cdr l))))))))

```


然而如果我们把后面的(lambda (len))部分去掉
``` scheme
(lambda (le)
  ((lambda (mk-length)
    (mk-length mk-length))
   (lambda (mk-length)
     (le (lambda (x) ((mk-length mk-length) x))))))

```

他其实就是下面的骨架：
``` scheme
(lambda (le)
  ((lambda ()
    ())
   (lambda ()
     (le (lambda (x) (() x))))))
```

如果我们使用f来填加：

``` scheme
(lambda (le)
    ((lambda (f)
      (f f))
     (lambda (f)
       (le (lambda (x) ((f f) x))))))
```

他其实就是Y-lambda. 我们所谓的define，let,set!都可以从lambda 推出来。

### let宏
``` scheme

(define-syntax
    (syntax-rules ()
        ((let ((var expr) ...) body ...) ((lambda (var ...) body ...) expr ...))))

```


也就是我们以后完全可以用
``` scheme

(define Y
  (lambda (le)
    ((lambda (f) (f f))
     (lambda (f)
       (le (lambda (x) ((f f) x)))))))
```

Y-lambda 执行一个 代表lambda的函数子f。

比如,求最大值(*注意：(lambda (func-arg) func-arg是可以不存在的部分,永远不会被执行*)):
``` scheme
;; 求个数

((Y (lambda (len)
   (lambda (l)
     (cond
       ((null? l) 0)
       (else (+ 1 (len (cdr l))))))))  '(3 2 3  6 1))

;;; 求最大值
 ((Y (lambda (func-arg)
        (lambda (l)
          (cond
            ((null? l) 'no-list)
            ((null? (cdr l)) (car l))
            (else (max (car l) (func-arg (cdr l)))))))) '(3 2 5 3 425))
```


另外我们发现y-lambda可以写成更优雅、对称的形式:
``` scheme
(define Y
  (lambda (le)
    ((lambda (f)
      (le (f f))) ;;; 
     (lambda (f)
       (le (lambda (x) ((f f) x)))))))

```

Add le()  is not necessay
But the code can be 
More beautiful and symmetry

### 然而其实我的想法只不过是他的复制
``` scheme
(define Y1
  (lambda (le)
    ((lambda (fun)
      (fun fun))  ;; 这是基于抽象的方式,也就是把后面的(lambda (f))  带入其中

     (lambda (f)
       (le (lambda (x) ((f f) x)))))))

((Y1 
 (lambda (fun-arg)
   (lambda (l)
     (cond
       ((null? l) 0)
       (else (+ 1 (fun-arg (cdr l)))))))) '(3 5 2 42))
```

## 简单计算器解释器


来自[wangyin](http://pythoner.org/wiki/774/)
``` scheme
#lang racket
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;

;;; 以下三个定义 env0, ent-env, lookup 是对环境(environment)的基本操作:

;; 空环境
(define env0 '())

;; 扩展。对环境 env 进行扩展，把 x 映射到 v，得到一个新的环境
(define ext-env
  (lambda (x v env)
    (cons `(,x . ,v) env)))

;; 查找。在环境中 env 中查找 x 的值
(define lookup
  (lambda (x env)
    (let ([p (assq x env)])
      (cond
       [(not p) x]
       [else (cdr p)]))))

;; 闭包的数据结构定义，包含一个函数定义 f 和它定义时所在的环境
(struct Closure (f env))

;; 解释器的递归定义（接受两个参数，表达式 exp 和环境 env）
;; 共 5 种情况（变量，函数，调用，数字，算术表达式）
(define interp1
  (lambda (exp env)
    (match exp                                          ; 模式匹配 exp 的以下情况（分支）
      [(? symbol? x) (lookup x env)]                    ; 变量
      [(? number? x) x]                                 ; 数字
      [`(lambda (,x) ,e)                                ; 函数
       (Closure exp env)]
      [`(,e1 ,e2)                                       ; 调用
       (let ([v1 (interp1 e1 env)]
             [v2 (interp1 e2 env)])
         (match v1
           [(Closure `(lambda (,x) ,e) env1)
            (interp1 e (ext-env x v2 env1))]))]
      [`(,op ,e1 ,e2)                                   ; 算术表达式
       (let ([v1 (interp1 e1 env)]
             [v2 (interp1 e2 env)])
         (match op
           ['+ (+ v1 v2)]
           ['- (- v1 v2)]
           ['* (* v1 v2)]
           ['/ (/ v1 v2)]))])))

;; 解释器的“用户界面”函数。它把 interp1 包装起来，掩盖第二个参数，初始值为 env0
(define interp
  (lambda (exp)
    (interp1 exp env0)))

;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
(interp (+ 2 65))
(interp (* 3 2))
(interp (/ 3 23))



```


## 复杂解释器

分为五个部分
+ \*const
+ \*cond
+ \*identifier(loop-up-evironment)
+ \*quote 
+ \*lambda
+ \*Aplication(Not Promitive)


具体可以看<The little scheme>

``` scheme
#lang racket
;;don't look down on it, it is the full interpretor!
;;Take care to read it!
(define atom?
  (lambda (x)
    (and (not (pair? x)) (not (null? x)))))
  
(define build
  (lambda (s1 s2)
    (cond
      (else (cons s1
                  (cons s2 (quote ())))))))
((lambda (nothing)
     (cond
       (nothing (quote something))
       (else (quote nothing))))
   #t)

(define lookup-in-entry
  (lambda (name entry entry-f)
    (lookup-in-entry-help name
                          (first entry)
                          (second entry)
                          (entry-f))))
(define lookup-in-entry-help
  (lambda (name names values entry-f)
    (cond
      ((null? names) (entry-f name))
      ((eq? (car names) name)
       (car values))
      (else (lookup-in-entry-help name
                                  (cdr names)
                                  (cdr values)
                                  entry-f)))))
(define lookup-in-table
  (lambda (name table table-f)
    (cond 
      ((null? table) (table-f name))
      (else (lookup-in-entry name
                             (car table)
                             (lambda (name)
                                                (cdr table)
                                                (table-f))))))))
(lambda (name table table-f)
    (lookup-in-table name
                     (cdr table)
                     (table-f)))
(define expression-to-action
  (lambda (e)
    (cond
      ((atom? e) (atom-to-action e))
      (else (list-to-action e)))))
(define *const
  (lambda (e table)
    (cond 
      ((number? e) e)
      ((eq? e #t) #t)
      ((eq? e #f) #f)
      (else (build (quote primitive) e)))))
(define atom-to-action
  (lambda (e)
    (cond
      ((number? e) *const)
      ((eq? e #t) *const)
      ((eq? e #f) *const)
      ((eq? e (quote cons)) *const)
      ((eq? e (quote car)) *const)
      ((eq? e (quote cdr)) *const)
      ((eq? e (quote null?)) *const)
      ((eq? e (quote eq?)) *const)
      ((eq? e (quote atom?)) *const)
      ((eq? e (quote zero?)) *const)
      ((eq? e (quote add1)) *const)
      ((eq? e (quote sub1)) *const)
      ((eq? e (quote number?)) *const)
      (else *identifier))))
(define list-to-action
  (lambda (e)
    (cond
      ((atom? (car e))
       (cond
         ((eq? (car e) (quote quote))
          *quote)
         ((eq? (car e) (quote lambda))
          *lambda)
         ((eq? (car e) (quote cond))
          *cond)
         (else *application)))
      (else *application))))

(define *quote
  (lambda (e table)
    (text-of e)))
(define first
  (lambda (p)
    (cond 
      (else (car p)))))
(define second
  (lambda (p)
    (cond
      (else car (cdr p)))))
(define third
  (lambda (p)
    (cond 
      (else car (cdr (cdr p))))))
(define text-of second)

(define table-of first)
(define formals-of second)
(define body-of third)
(define *identifier
  (lambda (e table)
    (lookup-in-table e table initial-table)))
(define initial-table
  (lambda (name)
    (car (quote ()))))
(define *lambda
  (lambda (e table)
    (build (quote non-primitive)
           (cons table (cdr e)))))

(define evcon
  (lambda (lines table)
    (cond 
      ((else? (question-of (car lines)))
       (meaning (answer-of (car lines))
                table))
      ((meaning (question-of (car lines))
                table)
       (meaning (answer-of (car lines))
                table))
      (else (evcon (cdr lines) table)))))

(define else?
  (lambda (x)
    (cond
      ((atom? x) (eq? x (quote else)))
      (else #f))))
(define question-of first)
(define answer-of second)

(define *cond
  (lambda (e table)
    (evcon (cond-lines-of e) table)))
(define cond-lines-of cdr)

(define evlis
  (lambda (args table)
    (cond
      ((null? args) (quote ()))
      (else
       (cons (meaning (car args) table)
             (evlis (cdr args) table))))))
(define *application
  (lambda (e table)
    (apply
     (meaning (function-of e) table)
     (evlis (arguments-of e) table))))
(define function-of car)
(define arguments-of cdr)
(define primitive?
  (lambda (l)
    (eq? (first l) (quote primitive))))
(define non-primitive?
  (lambda (l)
    (eq? (first l) (quote non-primitive))))
(define apply
  (lambda (fun vals)
    (cond
      ((primitive? fun)
       (apply-primitive
        (second fun) vals))
      ((non-primitive? fun)
       (apply-closure
        (second fun) vals)))))

(define apply-primitive
  (lambda (name vals)
    (cond
      ((eq? name (quote cons))
       (cons (first vals) (second vals)))
      ((eq? name (quote car))
       (car (first vals)))
      ((eq? name (quote cdr))
      ((eq? name (quote cdr))
       (cdr (first vals)))
      ((eq? name (quote null?))
       (null? (first vals)))
      ((eq? name (quote eq?))
       (eq? (first vals) (second vals)))
      ((eq? name (quote atom?))
       (:atom? (first vals)))
      ((eq? name (quote zero?))
       (zero? (first vals)))
      ((eq? name (quote add1))
       (add1 (first vals)))
      ((eq? name (quote sub1))
       (sub1 (first vals)))
      ((eq? name (quote number?))
       (number? (first vals))))))
(define :atom?
  (lambda (x)
    (cond
      ((atom? x) #t)
      ((null? x) #f)
      ((eq? (car x) (quote primitive))
       #t)
      ((eq? (car x) (quote non-primitive))
       #t)
      (else #f))))
(define apply-closure
  (lambda (closure vals)
    (meaning (body-of closure)
             (extend-table
              (new-entry
               (formals-of closure)
               vals)
              (table-of closure)))))
(define new-entry build)
(define extend-table cons)

(define value
  (lambda (e)
    (meaning e (quote ()))))
(define meaning
  (lambda (e table)
    ((expression-to-action e) e table)))

(value (+ 3 4 (* 3 1)))

```

## 树形抽象

1. 一个lat 中的一个删除

``` scheme

(define rember3
  (lambda (s l)
    (cond 
      ((null? l) (quote ()))
      ((equal? (car l) s) (cdr l))
      (else (cons (car l)
                  (rember3 s (cdr l)))))))

```


2. 一个list中的一个删除

``` scheme

(define multirember
  (lambda (a lat)
    (cond
      ((null? lat) (quote ()))
      (else
       (cond
         ((eq? (car lat) a)
          (multirember a (cdr lat)))
         (else (cons (car lat)
                     (multirember a (cdr lat)))))))))

```

3. 一个list中的全部删除

``` scheme


(define rember*
  (lambda (a l)
    (cond
      ((null? l) (quote ()))
      ((atom? (car l))
       (cond 
         ((eq? (car l) a)
          (rember* a (cdr l)))
         (else (cons (car l)
                     (rember* a (cdr l))))))
     (else (cons (rember* a (car l))
           (rember* a (cdr l)))))))


```
4 1中的rember升级
加入谓词表达式的参数
``` scheme
(define rember-f
  (lambda (test? a l)
    (cond 
      ((null? l) (quote ()))
      (else
       (cond 
         ((test? (car l) a) (cdr l));;I think here needs atom2function
         (else (cons (car l)
                     (rember-f test? a 
                               (cdr l)))))))))

```

5. 2中的multirember升级

``` scheme

(define multirember-f
  (lambda (test?)
    (lambda (a lat)
      (cond 
        ((null? lat) (quote ()))
        ((test? a (car lat))
         ((multirember-f test?) a
                               (cdr lat)))
        (else (cons (car lat)
                    ((multirember test?)  a 
                                          (cdr lat))))))))

(define multirember-eq? (multirember-f 'eq?))

```

6. 5中的multirember-f进一步升级

加入了可以处理的col,作为一个后处理小程序
``` scheme


(define multiremberco
  (lambda (a lat col)
    (cond 
      ((null? lat)
       (col (quote ()) (quote ())))
      ((eq? (car lat) a)
       (multiremberco a 
                      (cdr lat)
                      (lambda (newlat seen) 
                        (col newlat (cons (car lat) seen)))))
      (else
       (multiremberco a 
                      (cdr lat)
                      (lambda (newlat seen)
                        (col (cons (car lat) newlat)
                             seen)))))))

(define a-friend
  (lambda (x y)
    (null? y)))
;(multiremberco 'tuna '(sfds tuna fsd jif) a-friend)
;#f
;> (multiremberco 'tuna '(sfds tuna1 fsd jif) a-friend)
;#t
;;how can  you get another value in the col
(define new-friend
  (lambda (newlat seen)
    (a-friend newlat
         (cons (quote tuna) seen))))

```

### 额外话题 insertleft insertright eventonly比较上面的rember,理解col
``` scheme

(define multiinsertLR
  (lambda (new oldL oldR lat)
    (cond
      ((null? lat) (quote ()))
      ((eq? (car lat) oldL)
       (cons new 
             (cons oldL
                   (multiinsertLR new oldL oldR
                                  (cdr lat)))))
      ((eq? (car lat) oldR)
       (cons oldR
             (cons new
                   (multiinsertLR new oldL oldR
                                 (cdr lat))))))))

(define multiinsertLRco
  (lambda (new oldL oldR lat col)
    (cond
      ((null? lat)
       (col (quote ()) 0 0))
      ((eq? (car lat) oldL)
       (multiinsertLRco new oldL oldR
                        (cdr lat)
                        (lambda (newlat L R)
                          (col (cons new 
                                     (cons oldL newlat))
                               (+ 1 L) R))))
      ((eq? (car lat) oldR)
       (multiinsertLRco new oldL oldR
                        (cdr lat)
                        (lambda (newlat L R)
                          (col (cons oldR
                                     (cons new newlat))
                               L (+ 1 R)))))
      (else
       (multiinsertLRco new oldL oldR 
                        (cdr lat)
                        (lambda (newlat L R)
                          (col (cons (car lat) newlat)
                               L R)))))))


(define evens-only*co
  (lambda (l col)
    (cond
      ((null? l)
       (col (quote ()) 1 0))
      ((atom? (car l))
       (cond 
         ((even? (car l))
          (evens-only*co (cdr l)
                         (lambda (newl p s)
                           (col (cons (car l) newl);if it is evens ,so cons it into the newl
                                (* (car l) p) s))))
         (else (evens-only*co (cdr l)
                              (lambda (newl p s)
                                (col newl p (+ (car l) s)))))))
       (else (evens-only*co (car l)
                            (lambda (al ap as)
                              (evens-only*co (cdr l)
                                             (lambda (dl dp ds)
                                               (col (cons al dl)
                                                    (* ap dp)
                                                    (+ as ds))))))))))
;the fourth collector :: the last-last-friend
(define the-last-last-friend
  (lambda (newl product sum)
    (cons sum 
          (cons product 
                newl))))
;> (evens-only*co '(45389 63 45 6 4 234  6 4 23 52 43) the-last-last-friend)
;'(45563 7008768 6 4 234 6 4 52)


```

7. 进一步升级multirember 提取公共形式

insert-g可以变幻出 rember
 insert-r insert-l  subset等

``` scheme

(define rembern
  (lambda (a l)
    ((insert-g seqrem) #f a l)));;seqrem doesn't need (seqrem solve my ?.
(define seqrem
  (lambda (new old l) 
    l))

```

*那么insert-g到底是什么?*
``` scheme

(define insert-g
  (lambda (seq)
    (lambda (new old l)
      (cond
        ((null? l) (quote ()))
        ((eq? (car l) old)
         (seq new old (cdr l)))
        (else (cons (car l)
                    ((insert-g seq) new old
                                    (cdr l))))))))

```

所以现在我们可以定义subst
``` scheme
(define seqS
  (lambda (new old l)
    (cons new l)))

(define seqL
    (lambda (new old l)
     (cons new (cons old l))))

(define seqR
    (lambda (new old l)
        (cons (old (cons new l)))))

(define subst1 (insert-g seqS))
(define insertL (insert-g seqL))
(define insertR (insert-g seqR))

```


经过这四个级别的抽象我们得到一般的具备公共模式的insert-g
当然该函数还可以继续抽象。
