---
layout: post
title: "It's a dead program.How to let it alive?"
date: 2016-02-09 23:20:18 +0800
comments: true
categories: scheme
---

All programs are data.
All intepreters are program.
All Handware and software,or type checkers etc are interpreter.

I think data conceives the soul,not only the fixed process,but changing vari-language.

<!--more-->

* [1.The Little Scheme interpreter](#1)
* [2.The Season Scheme interpreter](#2)
* [3.The CPS control struture](#3)
    * [3.1 how to add arguments?](#3.1)
    * [3.2 what is the small stuff?](#3.2)
* [4. how to evaluate or interpreter  a procedre?](#4)
    * [4.1 ordinary tracing](#4.1)
    * [4.2 cps tracing](#4.2)
* [5. It is not the end](#5)

<h2 id="1">The Little Scheme</h2>

Here below is the interpreter from TLS


``` scheme
#lang racket
;;简单环境配置
    (define atom?
     (lambda (x)
      (and (not (pair? x)) (not (null? x)))))


    ;第一大部分 定义好整体框架
    (define value
     (lambda (e)
      (meaning e (quote ()))))
    (define meaning
     (lambda (e table)
      ((expression-to-action e) e table)))

    ;;第二大部分 解析atom and list action

    (define expression-to-action
     (lambda (e)
      (cond
       ((atom? e) (atom-to-action e))
       (else (list-to-action e)))))
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

    ;; 第三大部分 定义 6星==5个special forms 和一个general forms（*application)

    ;;;环境额外配置 这些配置仅仅是为了增加可读性 无任何编程技巧
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
    ;; *identifier
    (define table-of first)
    (define formals-of second)
(define body-of third)
    ;;*cond
    (define question-of first)
    (define answer-of second)
(define cond-lines-of cdr)
    ;;*application
    (define function-of car)
(define arguments-of cdr)

    (define build
     (lambda (s1 s2)
      (cond
       (else (cons s1
              (cons s2 (quote ())))))))
    ;;1st star
    (define *const
     (lambda (e table)
      (cond
       ((number? e) e)
       ((eq? e #t) #t)
       ((eq? e #f) #f)
       (else (build (quote primitive) e)))))
    ;;2nd star
    (define *quote
     (lambda (e table)
      (text-of e)))
    ;;3rd star
    (define *lambda
     (lambda (e table)
      (build (quote non-primitive)
       (cons table (cdr e)))))
    ;;4th star
    (define *identifier
     (lambda (e table)
      (lookup-in-table e table initial-table)))
    (define initial-table
     (lambda (name)
      (car (quote ()))))
    (define lookup-in-table
     (lambda (name table table-f)
      (cond
       ((null? table) (table-f name))
       (else (lookup-in-entry name
              (car table)
              ; (lambda (name)
                  ; (cdr table)
                  ; (table-f)))))))
(lambda (name)
 (lookup-in-table name
  (cdr table)
  table-f)))))))
    (define lookup-in-entry
     (lambda (name entry entry-f)
      (lookup-in-entry-help name
       (first entry)
       (second entry)
       entry-f)))
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

    ;; 5th star
    (define *cond
     (lambda (e table)
      (evcon (cond-lines-of e) table)))
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


    ;;6th star 你需要解析arguments的各个部分这是evlis的工作
    (define *application
     (lambda (e table)
      (apply
       (meaning (function-of e) table)
       (evlis (arguments-of e) table))))

    (define evlis
     (lambda (args table)
      (cond
       ((null? args) (quote ()))
       (else
        (cons (meaning (car args) table)
         (evlis (cdr args) table))))))


    ;;第四大部分 进行 apply的定义 反向运用primitive和non-primitive 去除核心的primitive和non-primitive前缀，并升级环境

    (define apply
     (lambda (fun vals)
      (cond
       ((primitive? fun)
        (apply-primitive
         (second fun) vals))
       ((non-primitive? fun)
        (apply-closure
         (second fun) vals)))))

    (define primitive?
     (lambda (l)
      (eq? (first l) (quote primitive))))
    (define non-primitive?
     (lambda (l)
      (eq? (first l) (quote non-primitive))))
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
         (number? (first vals)))))))
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

    ;;closure=table+arguments+body ;;arguments 通过formals-of获取 body-of获取body
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


    ;;测试这个解释器
    ;(value (cons (* (+ 1 3) 4) 'hello)) ;;不通过
    (value (+ 2 4))
(value (* 15 (+ 2 4)))
    ;;(value (cons (quote (* (+ 1 3) 4)) (cons 'h '()))) ;;测试不通过

```

<h2 id="2"> The Season scheme</h2>


Here below is the interpreter from TLS


``` scheme
#lang racket
;; The Seasoned Schemer
;; chapter 20
;; What's in Store ?

(define abort '())

(define global-table '())

(define (add1 n)
 (+ n 1))

(define (sub1 n)
 (- n 1))

    (define (atom? a)
     (and (not (pair? a))
      (not (null? a))))

(define (text-of x)
 (cadr x))

(define (formals-of x)
 (cadr x))

(define (body-of x)
 (cddr x))

(define (ccbody-of x)
 (cddr x))

(define (name-of x)
 (cadr x))

    (define (right-side-of x)
     (if (null? (cddr x))
      0
      (caddr x)))

(define (cond-lines-of x)
 (cdr x))

    (define (else? x)
     (if (atom? x)
      (eq? x 'else)
#f))

(define (question-of x)
 (car x))

(define (answer-of x)
 (cadr x))

(define (function-of x)
 (car x))

(define (arguments-of x)
 (cdr x))


(define (lookup table name)
 (table name))

    (define (extend name1 val table)
     (lambda (name2)
      (if (eq? name1 name2)
       val
       (table name2))))

    (define (define? e)
     (eq? (and (pair? e)
           (car e)) 'def))

(define (*define e)
 (set! global-table
  (extend (name-of e)
   (box (the-meaning (right-side-of e)))
   global-table)))

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

(define (the-meaning e)
 (meaning e lookup-in-global-table))

(define (lookup-in-global-table name)
 (lookup global-table name))

(define (meaning e table)
 ((expression-to-action e) e table))

(define (*quote e table)
 (text-of e))

(define (*identifier e table)
 (unbox (lookup table e)))

(define (*set e table)
 (setbox
  (lookup table (name-of e))
  (meaning (right-side-of e) table)))

    (define (*lambda e table)
     (lambda (args)
      (beglis (body-of e)
       (multi-extend (formals-of e)
        (box-all args)
        table))))

    ;; (define (beglis es table)
            ;; (cond
                ;; ((null? (cdr es))
                    ;; (meaning (car es) table))
                ;; (else ((lambda (val)
                            ;; (beglis (cdr es) table))
                        ;; (meaning (car es) table)))))

    ;; (define (beglis es table)
            ;; (let ((m (meaning (car es) table)))
                ;; (if (null? (cdr es))
                    ;; m
                    ;; ((lambda (val)
                            ;; (beglis (cdr es) table)) m))))

    ;; (define (beglis es table)
            ;; (let ((m (meaning (car es) table)))
                ;; (if (null? (cdr es))
                    ;; m
                    ;; (let ((val m))
                        ;; (beglis (cdr es) table)))))

    (define (beglis es table)
     (let ((m (meaning (car es) table))
           (d (cdr es)))
      (if (null? d)
       m
       (beglis d table))))

    ;; (define (box-all vals)
            ;; (if (null? vals)
                ;; '()
                ;; (cons (box (car vals))
                    ;; (box-all (cdr vals)))))

    ;; (define (box-all vals)
            ;; (letrec
                ;; ((rec
                        ;; (lambda (vals acc)
                            ;; (if (null? vals)
                                ;; acc
                                ;; (rec (cdr vals)
                                    ;; (cons (box (car vals)) acc))))))
                ;; (rec (reverse vals) '())))

    (define (box-all vals)
     (let loop ((vals (reverse vals))
                (acc '()))
      (if (null? vals)
       acc
       (loop (cdr vals)
        (cons (box (car vals)) acc)))))

    (define (multi-extend names vals table)
     (if (null? names)
      table
      (extend (car names)(car vals)
       (multi-extend (cdr names)(cdr vals)
        table))))

    (define (*application e table)
     ((meaning (function-of e) table)
      (evlis (arguments-of e) table)))

    ;; (define (evlis args table)
            ;; (if (null? args)
                ;; '()
                ;; ((lambda (val)
                        ;; (cons val
                            ;; (evlis (cdr args) table)))
                    ;; (meaning (car args) table))))

    ;; (define (evlis args table)
            ;; (if (null? args)
                ;; '()
                ;; (cons (meaning (car args) table)
                    ;; (evlis (cdr args) table))))

    ;; (define (evlis args table)
            ;; (letrec
                ;; ((rec
                        ;; (lambda (args table acc)
                            ;; (if (null? args)
                                ;; acc
                                ;; (rec (cdr args)
                                    ;; table
                                    ;; (cons (meaning (car args) table)
                                        ;; acc))))))
                ;; (rec (reverse args) table '())))

    (define (evlis args table)
     (let loop ((args (reverse args))
                (table table)
                (acc '()))
      (if (null? args)
       acc
       (loop (cdr args) table
        (cons (meaning (car args) table)
         acc)))))

    (define (a-prim p)
     (lambda (args-in-a-list)
      (p (car args-in-a-list))))

    (define (b-prim p)
     (lambda (args-in-a-list)
      (p (car args-in-a-list)
       (cadr args-in-a-list))))

(define (*const e table)
 (cond
  ((number? e) e)
  ((eq? e #t) #t)
  ((eq? e #f) #f)
  ((eq? e 'cons)(b-prim cons))
  ((eq? e 'car )(a-prim car))
  ((eq? e 'cdr)(a-prim cdr))
  ((eq? e 'eq?)(b-prim eq?))
  ((eq? e 'atom?)(a-prim atom?))
  ((eq? e 'null?)(a-prim null?))
  ((eq? e 'zero?)(a-prim zero?))
  ((eq? e 'add1)(a-prim add1))
  ((eq? e 'sub1)(a-prim sub1))
  ((eq? e 'number)(a-prim number?))))

(define (*cond e table)
 (evcon (cond-lines-of e) table))

(define (evcon lines table)
 (cond
  ((else? (question-of (car lines)))
   (meaning (answer-of (car lines)) table))
  ((meaning (question-of (car lines)) table)
   (meaning (answer-of (car lines)) table))
  (else (evcon (cdr lines) table))))

(define (*letcc e table)
 (let/cc skip
  (beglis (ccbody-of e)
   (extend (name-of e)
    (box (a-prim skip) table)))))

(define (value e)
 (let/cc the-end
  (set! abort the-end)
  (if (define? e)
   (*define e)
   (the-meaning e))))

(define (the-empty-table name)
 (abort
  (cons 'no-answer
   (cons name '()))))

    (define (expression-to-action e)
     (if (atom? e)
      (atom-to-action e)
      (list-to-action e)))

(define (atom-to-action e)
 (cond
  ((number? e) *const)
  ((eq? e #t) *const)
  ((eq? e #f) *const)
  ((eq? e 'cons) *const)
  ((eq? e 'car) *const)
  ((eq? e 'cdr) *const)
  ((eq? e 'null?) *const)
  ((eq? e 'eq?) *const)
  ((eq? e 'atom?) *const)
  ((eq? e 'zero?) *const)
  ((eq? e 'add1) *const)
  ((eq? e 'sub1) *const)
  ((eq? e 'number?) *const)
  (else *identifier)))

    (define (list-to-action e)
     (let ((a (car e)))
      (if (atom? a)
       (let ((prim-of? (lambda (x) (eq? x a ))))
        (cond
         ((prim-of? 'quote) *quote)
         ((prim-of? 'lambda) *lambda)
         ((prim-of? 'letcc) *letcc)
         ((prim-of? 'set!) *set)
         ((prim-of? 'cond) *cond)
         (else *application)))
       *application)))


(set! global-table (lambda (name)
                    (the-empty-table name)))
    (value (cons 'a 'b))
(value (+ (- 3 2) 6))


```

<h2 id="3">The CPS control struture</h2>

Here below is the method how to change the ordinary subroutine to the continuation passing style [Ref.](https://cgi.soic.indiana.edu/~c311/lib/exe/fetch.php?media=cps-notes.scm).

+ First rule: whenever we see a lambda in the code we want to CPS, we have to add an argument, and then process the body  
+ Second rule: "Don't sweat the small stuff!" 

<h3 id="3.1">how to add argument?</h3>

- orignial style:

``` scheme
(define rember8
  (lambda (ls)
    (cond
      [(null? ls) '()]
      [(= (car ls) 8) (cdr ls)]
      [else (cons (car ls) (rember8 (cdr ls)))])))
```

- incompleted cps style:

``` scheme
(define rember8
  (lambda (ls k)
    (cond
      [(null? ls) '()]
      [(= (car ls) 8) (cdr ls)]
      [else (cons (car ls) (rember8 (cdr ls)))])))
```

<h3 id="3.2">what is the small stuff?</h3>

Small stuff is stuff we know will terminate right away.
Don't sweat the small stuff if we know it will be evaluated.
Don't sweat the small stuff if it *might* be evaluated, but instead
pass it to k.

为了更好辨别，我们改为如下形式

``` scheme

(define rember8
  (lambda (ls k)
    (cond
      [(null? ls) (*k* '())]
      [(= (car ls) 8) (*k* (cdr ls))]
      [else (*rember8* (cdr ls) (lambda (x) (*k* (cons (car ls) x))))])))
```

凡是加上\*号的，都代表者continuation的过程。
    Why don't null?, =, car, cdr, and cons count? Because they're just
    small stuff, and when we combine small stuff together in small ways,
    the combination remains small.

    Second, all arguments are small stuff. Yep, even the lambda in the
    else line, because lambda is *always* small stuff.

- complete cps style:

``` scheme

(define rember8
  (lambda (ls k)
    (cond
      [(null? ls) (k '())]
      [(= (car ls) 8) (k (cdr ls))]
      [else (rember8 (cdr ls) (lambda (x) (k (cons (car ls) x)))])))
```



<h2 id="4">how to evaluate or interpreter  a procedre?</h2>

<h3 id="4.1">ordinary tracing</h3>

  we will develop a unimplemented language to do give an explanation to it.

  Here below is the main language(we can change it ,but now it is not the main cause.)

``` scheme

;1 eval
(define eval
   (lambda (Exp Env )
 (cond 
    ((number? Exp)  Exp)
    ((symbol? Exp) (looking Exp Env))
    ((eq? (car Exp) 'QUADE) (cadr Exp))
    ((eq? (car Exp) 'lambda) 
        (list 'Closure (cdr Exp) Env))
    ((eq? (car Exp) 'Cond) 
        （Evcond (cadr Exp） Env))
    (else
        (apply  
           (eval (car Exp) Env)
           (Evlist (cdr Exp) Env)))))) 

;2 apply
(define apply
   (lambda (PROC ARGS)
     (cond 
        ((primitive? PROC) 
          (Apply-primitive Proc ARGS))
        ((EQ? (car proc) 'closure)
          (EVAL  (cadadr proc)
            (Bind (caadr proc) 
                ARGS
               (Caddr proc))))
         (else error)))))

;;;Attention, primitvie?  apply-primitive  is not finished

;3 Evlist
(define Evlist
  (lambda (l Env)
    (cond 
      ((eq? (car l) '()) '())
      (else
         (cons (eval (car l) Env)
               (eval (cdr l) Env))))))

;4 Evcond
(define Evcond
   (lambda (clauses Env)
     (cond
        ((eq? clauses '()) '())
        ((eq? (caar clauses) 'else) 
            (eval (cadar clauses) Env))
         ((false? (Eval (caar clauses) Env))
            (Evcond (cdr clauses) Env))
         (else
            (Eval (cadar clauses) Env)))))

;;;Attention false? is not finished.

;5 looking
(define looking
   (lambda (sym env)
     (cond 
       ((eq? env '()) (error 'unbound-value))
       (else
         ((lambda (vcell)
             (cond 
               ((eq? vcell '())
                  (looking sym (cdr env)))
               (else (cdr vcell))))
           (assq sym (car env)))))))

;6 assq
(define assq
    (lambda (syms alist)
       (cond
         ((eq? alist '()) '())
         ((eq? syms (caar alist)) (car alist))
         (else (assq syms (cdr alist))))))


;7 Bind
(define Bind
   (lambda (vars vals Env)
     (cons (pair-up  vars vals) env)))

;8 Pair-up
(define Pair-up
   (lambda (vars vals)
     (cond 
        ((eq? vars '())
            (cond 
               ((eq? vals? '()) '())
               (else (error 'Too-much-arguments))))
         ((eq? vals '()) (error 'Too-few-arguments))
         (else 
           (cons (cons (car vars)  (car vals))
                  (pair-up (cdr vars) (cdr vals))))))
```

So now we can use the language below to interpret the form 

```
(eval '(((lambda (x）（lambda (y) (+  x y))) 3) 4) e0)

```

Good explanation begin(kernal part)：

```


(apply (eval '((lambda (x) (lambda (y) (+ x y))） 3）  e0)
       (Evlist '(4) e0))

(apply (eval '((lambda (x) (lambda (y) (+ x y))） 3）  e0)
       （cons (eval '(4) e0) (evlist '() e0)))

(apply (eval '((lambda (x) (lambda (y) (+ x y))） 3）  e0)
       （cons 4 '()))

(apply (eval '((lambda (x) (lambda (y) (+ x y))） 3）  e0)
       '(4))


(apply (apply (eval '(lambda (x) (lambda (y) (+ x y))  e0)
       '(3) )
      '(4))

(apply (apply '(closure ((x) (lambad (y) (+ x y))) e0) 
       '(3) )
      '(4))

(apply (eval (lambda (y) (+ x y)) e1)
      '(4))

(apply '(closure ((y) (+ x y)) e1)
      '(4))

(eval '(+ x y) e2)

(apply (Eval '+ e2)  (Evlist '(x y) e2))

(apply '(add0101)  '(3 4))   ;add0101 is the assemble executable binary code(speed fast)


（+ 3 4） = 7
```

Finished~  Good ~  Well done.

<h3 id="4.2">cps tracing</h3>

```
Let's trace (rember8  (lambda (x) x))

ls | k

'(1 2 8 3 4 6 7 8 5) | (lambda (x) x) = id
'(2 8 3 4 6 7 8 5)   | (lambda (x) (id (cons 1 x))) = k2
'(8 3 4 6 7 8 5)     | (lambda (x) (k2 (cons 2 x))) = k3

Once we hit the 8, we apply (k (cdr ls)) where k is k3 and ls is '(8 3
4 6 7 8 5)

(k3 '(3 4 6 7 8 5)) = (k2 (cons 2 '(3 4 6 7 8 5)))
(k2 '(2 3 4 6 7 8 5)) = (id (cons 1 '(2 3 4 6 7 8 5)))
(id '(1 2 3 4 6 7 8 5)) = '(1 2 3 4 6 7 8 5)

And we're done.

```


<h2 id="5"> It is not the end</h2>

how to change the procedure above? Leave to you.

