---
layout: post
title: "The Second Interpreter from One"
date: 2016-02-27 01:04:52 +0800
comments: true
categories: scheme
---

在[First Interpreter From EOPL][1]中我们定义了一个最为基本的解释器，包含解释以下解释部分

1. const-exp
2. diff-exp
3. zero?-exp
4. if-exp
5. var-exp
6. let-exp

这六种是比较基础的形式，在此基础上我们可以增加四则运算，加入list操作，加入逻辑比较
<!--more-->

+ [1. 增加四则运算](#1) 
    + [1.1 lang.scm修改四则运算](#1.1)
    + [1.2 interp.scm修改四则运算](#1.2)
    + [1.3 测试四则运算结果](#1.3)
+ [2. 增加逻辑比较](#2)
    + [2.1 lang.scm修改逻辑比较](#2.1)
    + [2.2 interp.scm修改逻辑比较](#2.2)
    + [2.3 测试逻辑比较结果](#2.3)
+ [3. 加入列表操作](#3) 
    + [3.1 lang.scm修改列表操作](#3.1)
    + [3.2 interp.scm修改列表操作](#3.2)
    + [3.3 data-structures.scm增加了expval值类型和4个操作](#3.3)
    + [3.4 测试修改列表操作结果](#3.4)
+ [4. list具体实现](#4)
    + [4.1 lang.scm修改list具体实现](#4.1)
    + [4.2 interp.scm修改list具体实现](#4.2)
    + [4.3 data-structures.scm修改list的具体实现](#4.3) 
    + [4.3 测试list具体实现结果](#4.4)
+ [5. cond条件比较](#5)
    + [5.1 lang.scm修改cond条件比较](#5.1)
    + [5.2 interp.scm修改cond条件比较](#5.2)
    + [5.3 测试cond条件比较结果](#5.3)
+ [6. print显示](#6)
    + [6.1 lang.scm修改print显示](#6.1)
    + [6.2 interp.scm修改print显示](#6.2)
    + [6.3 测试print显示结果](#6.3)
+ [7. let的改进和let\*的加入](#7)
    + [7.1 let改进之interp.scm](#7.1)
    + [7.2 let改进之lang.scm](#7.2)
    + [7.3 let改进结果](#7.3)
    + [7.4 let\*的lang.scm具体实现](#7.4)
    + [7.5 let\*的interp.scm具体实现](#7.5)
    + [7.6 let\*的测试结果](#7.6)
+ [8. unpack列表赋值](#8)
    + [8.1 unpack的interp.scm修改](#8.1)
    + [8.2 unpack的lang.scm修改](#8.2)
    + [8.3 测试unpack修改结果](#8.3)


******

<h2 id="1"> 1. 增加四则运算  </h2>

<h3 id="1.1"> 1.1 lang.scm修改四则运算</h3>

在the-grammer增加如下expressions

``` scheme
      ;;;new add + * /
    (expression ("+" "(" expression "," expression ")") add-exp)
    (expression ("*" "(" expression "," expression ")") mult-exp)
    (expression ("/" "(" expression "," expression ")") div-exp)
```

<h3 id="1.2"> 1.2 interp.scm修改四则运算</h3>

在value-of增加如下expressions

``` scheme
           (add-exp (exp1 exp2)
		    (let ((val1 (value-of exp1 env))
			  (val2 (value-of exp2 env)))
		      (let ((num1 (expval->num val1))
			    (num2 (expval->num val2)))
			(num-val
			 (+ num1 num2)))))
           (mult-exp (exp1 exp2)
                     (let ((val1 (value-of exp1 env))
                           (val2 (value-of exp2 env)))
                       (let ((num1 (expval->num val1))
                             (num2 (expval->num val2)))
                         (num-val
                          (* num1 num2)))))
           (div-exp (exp1 exp2)
		    (let ((val1 (value-of exp1 env))
			  (val2 (value-of exp2 env)))
		      (let ((num1 (expval->num val1))
			    (num2 (expval->num val2)))
			(num-val
			 (/ num1 num2)))))
```

<h3 id="1.3"> 1.3 测试四则运算结果</h3>

在原先减法的基础上增加了乘法、加法和除法。
``` scheme
(run "let x = -(4,1) in +(x,1)")
(run "let x = -(4,1) in *(x,2)")
(run "let x = -(4,1) in /(x,2)")

结果:
    (num-val 4)
    (num-val 6)
    (num-val 1 1/2)
```

<h2 id="2"> 2. 增加逻辑比较  </h2>

<h3 id="2.1"> 2.1 lang.scm修改逻辑比较</h3>

``` scheme
 ;;增加逻辑比较
    (expression ("equal?" "(" expression "," expression ")") equal?-exp)
    (expression ("less?" "(" expression "," expression ")") less?-exp)
    (expression ("greater?" "(" expression "," expression ")") greater?-exp)
    (expression ("minus" "(" expression ")") minus-exp)
```
<h3 id="2.2"> 2.2 interp.scm修改逻辑比较</h3>
``` scheme
           (equal?-exp (exp1 exp2)
                    (let ((val1 (value-of exp1 env))
                          (val2 (value-of exp2 env)))
                      (let ((num1 (expval->num val1))
                            (num2 (expval->num val2)))
                        (bool-val
                         (= num1 num2)))))

           (less?-exp (exp1 exp2)
                    (let ((val1 (value-of exp1 env))
                          (val2 (value-of exp2 env)))
                      (let ((num1 (expval->num val1))
                            (num2 (expval->num val2)))
                        (bool-val
                         (< num1 num2)))))

           (greater?-exp (exp1 exp2)
                    (let ((val1 (value-of exp1 env))
                          (val2 (value-of exp2 env)))
                      (let ((num1 (expval->num val1))
                            (num2 (expval->num val2)))
                        (bool-val
                         (> num1 num2)))))
        (minus-exp (body-exp)
                  (let ((val1 (value-of body-exp env)))
                    (let ((num (expval->num val1)))
                      (num-val (- 0 num)))))
```

<h3 id="2.3"> 2.3 测试增加逻辑比较结果</h3>

``` scheme
(run "if less?(1, 2) then 1 else 2")
(run "if greater?(2, 1) then minus(1) else minus(2)")

    (num-val 1)
    (num-val -1)
```

<h2 id="3"> 3. 加入列表操作  </h2>

<h3 id="3.1"> 3.1 lang.scm修改列表操作</h3>

``` scheme
;;; 增加list比较
        ;;new stuff
    (expression ("cons" "(" expression "," expression ")") cons-exp)
    (expression ("car" "(" expression ")") car-exp)
    (expression ("cdr" "(" expression ")") cdr-exp)
    (expression ("emptylist") emptylist-exp)
    (expression ("null?" "(" expression ")") null?-exp)
```
<h3 id="3.2"> 3.2 interp.scm修改列表操作</h3>

``` scheme
        ;;;增加了list操作
        (emptylist-exp ()
                      (emptylist-val))
        (cons-exp (exp1 exp2)
                 (let ((val1 (value-of exp1 env))
                       (val2 (value-of exp2 env)))
                   (pair-val val1 val2)))
        (car-exp (body)
                (expval-car (value-of body env)))
        (cdr-exp (body)
                (expval-cdr (value-of body env)))
        (null?-exp (exp)
                  (expval-null? (value-of exp env)))
```
<h3 id="3.3"> 3.3 data-structures.scm增加了expval值类型和4个操作</h3>

+ 修改expval部分

增加了`pair-val`和`emptylist-val` 两个语言的新值，这也是区分之前的[四则运算](#1)和[逻辑比较](#2)的过程

``` scheme
    (pair-val
     (car expval?)
     (cdr expval?))
    (emptylist-val)
```

+ 增加了4个expval类型变换操作

``` scheme

;;增加4个expval->操作
(define expval->pair
  (lambda (v)
    (cases expval v
	   (pair-val (car cdr)
		     (cons car cdr))
	   (else (expval-extractor-error 'pair v)))))

(define expval-car
  (lambda (v)
    (cases expval v
	   (pair-val (car cdr) car)
	   (else (expval-extractor-error 'car v)))))

(define expval-cdr
  (lambda (v)
    (cases expval v
	   (pair-val (car cdr) cdr)
	   (else (expval-extractor-error 'cdr v)))))

(define expval-null?
  (lambda (v)
    (cases expval v
	   (emptylist-val () (bool-val #t))
	   (else (bool-val #f)))))

```

<h3 id="3.4"> 3.4 测试修改列表操作结果</h3>

``` scheme
(run "cons(1, 2)")
(run "car (cons (1, 2))")
(run "cdr (cons (1, 2))")
(run "null? (emptylist)")
(run "null? (cons (1, 2))")

(run "let x = 4
        in cons(x,
          cons(cons(-(x,1),
                    emptylist),
                   emptylist))")

    (pair-val (num-val 1) (num-val 2))
    (num-val 1)
    (num-val 2)
    (bool-val #t)
    (bool-val #f)
    (pair-val (num-val 4) (pair-val (pair-val (num-val 3) (emptylist-val)) (emptylist-val)))
```

错误的测试：

``` scheme
> (run "car (3 5 3)")
. . parsing: at line 1: looking for ")", found number 5 in production
((string "car") (string "(") (non-term expression) (string ")") (reduce #<procedure:car-exp>))
> (run "car (3,5)")
. . parsing: at line 1: looking for ")", found literal-string111 "," in production
((string "car") (string "(") (non-term expression) (string ")") (reduce #<procedure:car-exp>))
> (run "null? ()")
. . parsing: at line 1: nonterminal <expression> can't begin with literal-string111 ")"
> (run "car (cons (3,5))")
(num-val 3)
```

具体原因是car-exp的定义是通过expval-car的实现，而expval-car接受的是cons创造的数据结构，所以出现了问题。

``` scheme
  (car-exp (body) (expval-car (value-of body env)))


(pair-val
 (car expval?)
 (cdr expval?))
(emptylist-val)


(define expval-car
  (lambda (v)
    (cases expval v
     (pair-val (car cdr) car)   ;;v在这边是expval类型,更具体来说是pair-val,其他类型没有对应的操作
                                ;;而在当前的情况细 pair-val只能通过cons创建！！
     (else (expval-extractor-error 'car v)))))


并需要增加pair-var?的数据类型

  (define-datatype expval expval?
    (num-val
     (value number?))
    (bool-val
     (boolean boolean?))
    (pair-val
     (car expval?)
     (cdr expval?))
    (emptylist-val))


可以参考value-of中关于cons-exp的定义：
    (cons-exp (exp1 exp2)
             (let ((val1 (value-of exp1 env))
                   (val2 (value-of exp2 env)))
               (pair-val val1 val2)))
```

### 注意操作符后面不是必须直接跟上括号(空格会被直接忽略) 而是应该保证括号的对称性。
``` scheme
> (run "* (3,4")
. . parsing: at line 1: looking for ")", found end-marker #f in production
((string "*") (string "(") (non-term expression) (string ",") (non-term expression) (string ")") (reduce #<procedure:mult-exp>))
> (run "+(3,4)")
(num-val 7)
> (run "*(3,4)")
(num-val 12)
```


<h2 id="4"> 4.list的具体实现 </h2>

<h3 id="4.1"> 4.1 lang.scm修改list具体实现</h3>

``` scheme

    (expression ("list" "(" (separated-list expression ",") ")" ) list-exp)
```
<h3 id="4.2"> 4.2 interp.scm修改list具体实现</h3> 

1. 增加一个apply-elm用于list操作。

``` scheme
  ;; used as map for the list
(define apply-elm
  (lambda (env)
    (lambda (elem)
      (value-of elem env))))
```

2. 增加了list类型

``` scheme
  ;;; 增加了list操作
  (list-exp (args)
           (list-val (map (apply-elm env) args)))
```

<h3 id="4.3"> 4.3 data-structures.scm修改list的具体实现 </h3>


增加了list-val的实现:

``` scheme

;;;增加了list的具体实现
  (define list-val
  (lambda (args)
    (if (null? args)
	(emptylist-val)
	(pair-val (car args)
		  (list-val (cdr args))))))

```


<h3 id="4.4"> 4.4 测试list具体实现的结果</h3>

``` scheme
(run "list(1, 2, 3)")
(run "car(cdr(list(1, 2, 3)))")
(run "let x = 4
      in list(x, -(x,1), -(x,3))")

    (pair-val (num-val 1) (pair-val (num-val 2) (pair-val (num-val 3) (emptylist-val))))
    (num-val 2)
    (pair-val (num-val 4) (pair-val (num-val 3) (pair-val (num-val 1) (emptylist-val))))

```
<h2 id="5">5. cond条件比较 </h2>

<h3 id="5.1"> 5.1 lang.scm修改cond条件比较 </h3>

``` scheme
;;增加了cond具体语法
;; new stuff
(expression ("cond" (arbno expression "==>" expression) "end") cond-exp)

```
<h3 id="5.2"> 5.2 interp.scm修改cond条件比较 </h3>

1. 增加一个cond-val，之所以不在类似[加入列表实现](#3)和[list具体实现](#4)添加val转换，是因为cond-val涉及到value-of
程序，所以需要放在interp.scm中,放在apply-elm之后即可.

``` scheme
;;增加了cond-val的处理
;;new stuff
(define cond-val
  (lambda (conds acts env)
    (cond ((null? conds)
	   (error 'cond-val "No conditions got into #t"))
	  ((expval->bool (value-of (car conds) env))
	   (value-of (car acts) env))
	  (else
	   (cond-val (cdr conds) (cdr acts) env)))))
```

2. 在value-of中增加了具体的实现
``` scheme
        ;;;增加了cond操作
        	   (cond-exp (conds acts)
		     (cond-val conds acts env))
```

<h3 id="5.3"> 5.3 测试cond条件比较结果</h3>

``` scheme
(run "less?(1, 2)")
(run "cond less?(1, 2) ==> 2 end")
(run "cond less?(2, 1) ==> 1 greater?(2, 2) ==> 2  greater?(3, 2) ==> 3 end")

    (bool-val #t)
    (num-val 2)
    (num-val 3)
```

<h2 id="6"> 6. print显示 </h2>

<h3 id="6.1"> 6.1 lang.scm修改print显示</h3>

``` scheme
  ;; new stuff
    (expression ("print" "(" expression ")") print-exp)
```
<h3 id="6.2"> 6.2 interp.scm修改print显示</h3>

``` scheme
        ;;;增加了print
        (print-exp (arg)
                  (let ((val (value-of arg env)))
                    ;(print val)  ;;编译不通过 改为display
                    (display val) ;
                    (num-val 1)))
```

<h3 id="6.3"> 6.3 测试print显示结果</h3>

``` scheme
(run "print( less? (1, 2))")
    #(struct:bool-val #t)(num-val 1)
```

<h2 id="7"> 7. let的改进和let*的加入 </h2>

### 一个问题

怎么会有问题？

```
 (run "let x = 30
      in let x = -(x,1)
             y = -(x,2)
         in -(x, y)")
. . parsing: at line 3: looking for "in", found identifier y in production
((string "let") (term identifier) (string "=") (non-term expression) (string "in") (non-term expression) (reduce #<procedure:let-exp>))
```

分析原先的let设计程序
```
        (let-exp (var exp1 body)       
                (let ((val1 (value-of exp1 env)))
                  (value-of body
                           (extend-env var val1 env))))
```
在这个具体实现其实并没有实现多重let的过程，所以导致有问题，所以下一步的关键问题是如何实现嵌套的let-exp解析。


<h3 id="7.1"> let改进之interp.scm 修改 </h3>

1. 增加两个let-exp的辅助程序

``` scheme
  ;; let-exp的嵌套实现
  
  (define value-of-vals
    (lambda (vals env)
      (if (null? vals)
         '()
         (cons (value-of (car vals) env)
              (value-of-vals (cdr vals) env)))))
  
  (define extend-env-list
    (lambda (vars vals env)
      (if (null? vars)
         env
         (let ((var1 (car vars))
               (val1 (car vals)))
           (extend-env-list (cdr vars) (cdr vals) (extend-env var1 val1 env))))))
```
2. 改变letexp的具体实现
``` scheme

 ;       (let-exp (var exp1 body)       
 ;               (let ((val1 (value-of exp1 env)))
 ;                (value-of body
 ;                          (extend-env var val1 env))))
        
        (let-exp (vars vals body)
                (let ((_vals (value-of-vals vals env)))
                  (value-of body (extend-env-list vars _vals env))))
```

<h3 id="7.2"> 7.2 let改进之lang.scm  </h3>

``` scheme
  ;    (expression
  ;     ("let" identifier "=" expression "in" expression)
  ;     let-exp)
      (expression 
        ("let" (arbno identifier "=" expression) "in" expression) 
        let-exp)
```

还是没改好？

```
link: bad variable linkage;
 reference to a variable that is uninitialized
  reference phase level: 0
  variable module: "/home/canbetter/let-lang/lang.scm"
  variable phase: 0
  reference in module: "/home/canbetter/let-lang/interp.scm" in: a-program?
```

原因出现在let语法中增加了 `(arbno ...)`操作。

这个问题可以赘述到cond-exp的实现,其中也涉及到arbno的第一次键入。但是为什么会出现未初始化变量的情况？


但是主要还是因为保存得不彻底！！导致了并未初始化，重新保存即可。


<h3 id="7.3"> 7.3 let改进结果 </h3>

``` scheme
(run "let x = -(4,1) in let x =+(x,1) in -(x,10)")

    (num-val -6)
(run "let x = -(4,1) in let x =+(x,1)  y=-(x,10) in -(x,y)")

    (num-val 11) ;; x=4 y=-(3,10)=-7  所以-(x,y)=11而不是 10
```

那么如何实现一个let\*，使得let中的x变化的值立即反映到y中。

<h3 id="7.4"> 7.4 let*的lang.scm具体实现 </h3>

该实现方式基本上和[let的语法](#7.2)一样
``` scheme
      (expression ("let*" (arbno identifier "=" expression) "in" expression) let*-exp)
      
```

<h3 id="7.5"> 7.5 let*的interp.scm具体实现 </h3>

1. 增加了一个extend-env-list-iter操作,由于也是存在value-of所以放入interp.scm中。

``` scheme
  (define extend-env-list-iter
    (lambda(vars vals env)
      (if (null? vars)
         env
         (let ((var1 (car vars))
               (val1 (value-of (car vals) env)))
           (extend-env-list-iter (cdr vars) (cdr vals)
                                (extend-env var1 val1 env))))))
```

2. let\*的类型定义
``` scheme

        (let*-exp (vars vals body)
                 (value-of body (extend-env-list-iter vars vals env)))
```
<h3 id="7.6"> 7.6 let*的测试结果 </h3>

``` scheme
(run "let x = 30
      in let x = -(x,1)
             y = -(x,2)
         in -(x, y)")

(run "let x = 30
      in let* x = -(x,1)
             y = -(x,2)
         in -(x, y)")


    (num-val 1)
    (num-val 2)
```

<h2 id="8"> 8. unpack列表赋值 </h2>

<h3 id="8.1"> 8.1 unpack的interp.scm修改 </h3>

1. 增加了extend-env-list-exp的操作，用于针对unpack的特殊的环境拓展,当然可以把它放在data-structures.scm中

``` scheme
  ;;; 关于unpack的操作
  (define extend-env-list-exp
    (lambda (vars vals env)
      (if (null? vars)
         env
         (let ((var1 (car vars))
               (val1 (expval-car vals)))
           (extend-env-list-exp (cdr vars)
                               (expval-cdr vals)
                               (extend-env var1 val1 env))))))
```

2. unpack-exp的具体实现:
``` scheme
        (unpack-exp (vars vals body)
                   (let ((_vals (value-of vals env)))
                     (value-of body (extend-env-list-exp vars _vals env))))
```


<h3 id="8.2"> 8.2 unpack的lang.scm修改   </h3>

``` scheme
;;new stuff
    (expression ("unpack" (arbno identifier) "=" expression "in" expression) unpack-exp)
```

<h3 id="8.3"> 8.3 测试unpack修改结果  </h3>

``` scheme
;; new testcase
(run "let u = 7
      in unpack x y = cons(u, cons(3,emptylist))
      in -(x,y)")


(num-val 4)  ;;;(x y)= (7 3)  -(7,3)=4
```


## 结论

这是这对[First Interpreter From EOPL][1] 的8个方面的拓展，对于认识语言的设计有很大的帮助。



[1]:http://jueqingsizhe66.github.io/blog/2016/02/25/first-interpreter-from-eopl/ 
