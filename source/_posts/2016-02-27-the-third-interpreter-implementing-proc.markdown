---
layout: post
title: "The third Interpreter implementing Proc"
date: 2016-02-27 14:35:50 +0800
comments: true
categories: scheme
---


Scheme解释器的好处是可以不断的拓展自己语言的能力，

1. 通过[The First-interpreter-from-eopl][1]创建了一个较为简单let-language，实现了较为简单的减法运算和let局部变量赋值等功能。
2. [The Second Interpreter from on][2]第一次扩展了基础的let-language，实现了四则运算，逻辑判断，列表操作,列表赋值等。
3. 这个版本的解释器进一步拓展了过程定义和过程调用的功能。
<!--more-->

+ [1. proc-language 的实现](#1) 
+ [2. Y-comb 的实现过程](#2) 
+ [3. 新语言的Y+实现](#3)

<h2 id = "1"> 1. proc-language 的实现 </h2>

### lang.scm增加了过程定义和调用

``` scheme
    ;;;new stuff
      (expression
       ("proc" "(" identifier ")" expression)
       proc-exp)
      
      (expression
       ("(" expression expression ")")
       call-exp)
```

### interp.scm增加了过程定义和调用

+ value-of 增加了proc-exp和call-exp的内部语言形式


``` scheme
      ;;;新增的proc-exp 和call-exp
        (proc-exp (var body)
                 (proc-val (procedure var body env)))
        
        (call-exp (rator rand)
                 (let ((proc (expval->proc (value-of rator env)))
                       (arg (value-of rand env)))
                   (apply-procedure proc arg)))
```

+ apply-procedure 用于proc的实现

``` scheme
 ;; proc-exp
 ;; apply-procedure : Proc * ExpVal -> ExpVal
  ;; Page: 79
  (define apply-procedure
    (lambda (proc1 val)
      (cases proc proc1
        (procedure (var body saved-env)
          (value-of body (extend-env var val saved-env))))))
```

## data-structures.scm增加了过程定义和调用

+ 在expval中拓展了proc?的判断

``` scheme
  (define-datatype expval expval?
    (num-val
     (value number?))
    (bool-val
     (boolean boolean?))
    (pair-val
     (car expval?)
     (cdr expval?))
    (emptylist-val)
    ;;新增的proc-exp
    (proc-val 
     (proc proc?))
    )
  
```

+ 增加expval->proc类型转换的过程

proc其实就是一个closure的具体实现。

``` scheme
  ;;;新增的expval->proc 和proc类
  ;; expval->proc : ExpVal -> Proc
  (define expval->proc
    (lambda (v)
      (cases expval v
        (proc-val (proc) proc)
        (else (expval-extractor-error 'proc v)))))
  
  ;; proc? : SchemeVal -> Bool
  ;; procedure : Var * Exp * Env -> Proc
  (define-datatype proc proc?
    (procedure
     (var symbol?)
     (body expression?)
     (env environment?)))
```


##  增加过程定义和调用的测试结果

<h3 id="error1"> 错误1 </h3>

```
. data-structures.scm:102:11: expression?: unbound identifier in module in: expression?
```

### 错误2

如果按照错误1,把具体的expression?实现放入`data-structures.scm`中，则会出现[错误1](#error1)的问题。

expression?现在定义如下,具体参考[Some notations][3]:
```
expression ::= (var-exp ())
           ::= (const-exp ())
           ::= (zero?-exp ())
           ::= (if-exp ())
           ::= (diff-exp ())
           ::= (cons-exp ())
           ::= (car-exp ())
           ::= (cdr-exp ())
           ::= (null?-exp ())
           ::= (emptylist-exp)
           ::= (list-exp ())
           ::= (let-exp ())
           ::= (proc-exp ())
           ::= (call-exp ())
           ::= (letrec-exp ())  ;;并未再此体现

           ::= (less?-exp ())
           ::= (greater?-exp ())
           ::= (equal?-exp ())

           ::= (print-exp ())
           ::= (unpack-exp ())
           ::= (cond-exp ())

           ::= (add-exp ())
           ::= (mult-exp ())
           ::= (div-exp ())

           ::= (let*-exp ())

```

expression?实现如下:


```
  ;; datatype ;;;
  (define-datatype expression expression?
    (var-exp
     (id symbol?))
    (const-exp
     (num number?))
    (zero?-exp
     (expr expression?))
    (if-exp
     (predicate-exp expression?)
     (true-exp expression?)
     (false-exp expression?))
    ;;四则运算
    (minus-exp
     (body-exp expression?))
    (diff-exp
     (exp1 expression?)
     (exp2 expression?))
    (add-exp
     (exp1 expression?)
     (exp2 expression?))
    (mult-exp
     (exp1 expression?)
     (exp2 expression?))
    (div-exp
     (exp1 expression?)
     (exp2 expression?))
    
    ;;逻辑比较
    (equal?-exp
     (exp1 expression?)
     (exp2 expression?))
    (less?-exp
     (exp1 expression?)
     (exp2 expression?))
    (greater?-exp
     (exp1 expression?)
     (exp2 expression?))
    
    ;;列表操作
    (emptylist-exp)
    (cons-exp
     (exp1 expression?)
     (exp2 expression?))
    (car-exp
     (body expression?))
    (cdr-exp
     (body expression?))
    (null?-exp
     (body expression?))
    ;;list实现
    (list-exp
     (args (list-of expression?)))
    
    ;; cond实现
    (cond-exp
     (conds (list-of expression?))
     (acts (list-of expression?)))
  
    ;;let let*
    (let-exp
     (vars (list-of symbol?))  ;;;symbol不能写成symbols
     (vals (list-of expression?))
     (body expression?))
    
    (let*-exp
     (vars (list-of symbol?)) ;(vars (list-of symbols?))
     (vals (list-of expression?))
     (body expression?))
    
    ;;print实现
    (print-exp
     (arg expression?))
    ;;unpack实现
    (unpack-exp
     (args (list-of symbol?));(args (list-of identifier?))
     (vals expression?)
     (body expression?))
    
    ;;proc
    (proc-exp
     (var (list-of symbol?))
     (body expression?))
    (call-exp
     (rator expression?)
     (rand (list-of expression?)))
    
    )
  
```

为什么添加上expression？反而出现如下的错误？

```
(require "data-structures.scm")
(require "lang.scm")  =》 There are some problems here.
top.scm:8:11: module: identifier already imported from: "data-structures.scm" at: expression in: "lang.scm"
  #(174 21)
  #(235 10)
module: identifier already imported from a different source in:
  expression
  "lang.scm"
  "data-structures.scm"
```

分析发现是因为lang.scm其实已经定义了expression?, `sllgen:make-define-datatypes`中通过the grammer定义了所有的expression?的实现。
所以如果在`top.scm`中同时导入`lang.scm` 和`data-structures.scm`则会报错，重复定义。
**`于是仅仅在data-structures.scm中增加require "lang.scm"即可`**,
<font size="10" color="red">结果通过！</font>


### 正确的测试结果
``` scheme
(run "proc(x) -(x, 1)")
(run "(proc(x) -(x,1)  30)")
(run "(proc(f)(f 30)  proc(x)-(x,1))")
(run "let x = 3 in -(x,1)")
;;new stuff, the Currying
(run "let f = proc (x) proc (y) -(x, -(0, y)) in ((f 10) 20)")

    (proc-val (procedure 'x (diff-exp (var-exp 'x) (const-exp 1)) (list (list 'i (num-val 1)) (list 'v (num-val 5)) (list 'x (num-val 10)))))
    (num-val 29)
    (num-val 29)
    (num-val 2)
    (num-val 30)
```


<h2 id="2"> 2. 一个y-combination的实现 </h2>

关于y-combination可以参考[从lambda到simple+complex解释器再到树形抽象][4]疑问关于y-comb的实现。
进一步可以参考:

- [Fixed-point combinator] [5]
- [Church and Turing] [6]
- [Neil Jones的computation and complexity,使用一种while语言] [7]
- Matthias Felleisen 和 Matthew Flatt 的[《Programming Languages and Lambda Calculi》] [8] 教你认识lambda
- [最经典的Y combinator] [9] 看了10遍以上


### Y-comb基本思想

1. 通过增加一层lambda表达式，进行更高一级抽象
2. beta规约原则 

比如 
``` scheme
(define fact 
 (lambda (op)
   (lambda ( x y) (op  x y))))

```

比如

``` scheme
(f arg) ==  (lambda (arg) (f arg))

```


最为基本的fact实现是(构建与已经存在的fact)

``` scheme
(define fact
 (lambda (x)
   (if (zero? x)
      0
      (fact (- x 1)))))

(fact 5)

120
```

而如果运用思想1(也就是隐藏fact的调用，而使用局部过程变量procedure),则变换为

``` scheme

(define fact
   (lambda (procedure)
     (lambda (x)
        (if (zero? x)
           0
           (procedure (- x 1))))))


((fact fact) 5)

120
```

于是有了下面的推导过程(把fact的表达式多给他还原 ，可以参考[还原方法Some-notations][3])

``` scheme
 (define fact
    ((lambda (procedure)
       ((lambda (func-arg)  ;;吸收5
          (lambda (n)
            (if (zero? n)
                1
                (* n (func-arg (- n 1))))))
        (lambda (arg) ((procedure procedure) arg))))
     (lambda (procedure)
       ((lambda (func-arg) ;;吸收5
          (lambda (n)
            (if (zero? n)
                1
                (* n (func-arg (- n 1))))))
        (lambda (arg) ((procedure procedure) arg))))))


  (((lambda (procedure)
      (lambda (n)
        (if (zero? n)
            1
            (* n ((procedure procedure) (- n 1))))))
    (lambda (procedure)
      (lambda (n)
        (if (zero? n)
            1
            (* n ((procedure procedure) (- n 1)))))))
   5)

 (define fact1
   (lambda (x)
   (letrec ((F (lambda (procedure)
       ((lambda (func-arg)  ;;吸收5
          (lambda (n)
            (if (zero? n)
                1
                (* n (func-arg (- n 1))))))
        (lambda (arg) ((procedure procedure) arg))))))
     ((F F) x))))


  (define Y
    (lambda (X)
      ((lambda (procedure)
         (X (lambda (arg) ((procedure procedure) arg))))
       (lambda (procedure)
         (X (lambda (arg) ((procedure procedure) arg)))))))

  (((lambda (X)
      ((lambda (procedure)
         (X (lambda (arg) ((procedure procedure) arg))))
       (lambda (procedure)
         (X (lambda (arg) ((procedure procedure) arg))))))
    (lambda (func-arg)
      (lambda (n)
        (if (zero? n)
            1
            (* n (func-arg (- n 1)))))))
   5)

;;如何吸收F 形成(F F)的形式 产生循环的调用过程
;; 只能是通过lambda 算子 构造proc 从而获得完整的表达式
  (((lambda (X)
      ((lambda (F)
         (lambda (arg) ((F F) arg)))  ;;一定要保证lambda封闭  
       (lambda (procedure)
         (X (lambda (arg) ((procedure procedure) arg))))))
    (lambda (func-arg)
      (lambda (n)
        (if (zero? n)
            1
            (* n (func-arg (- n 1)))))))
   5)

(define Y 
  (lambda (X)
      ((lambda (F)
         (lambda (arg) ((F F) arg)))  ;;一定要保证lambda封闭
       (lambda (procedure)
         (X (lambda (arg) ((procedure procedure) arg)))))))

((Y   (lambda (func-arg)
      (lambda (n)
        (if (zero? n)
            1
            (* n (func-arg (- n 1))))))) 5)
```

可以看到每增加一层(lambada (arg) ...) ，也就是(lambda (arg) (lambda (arg2)  (lambda (arg) ... )))，
也就是  `<procedure>::= (lambda (arg) (<expression> |<procedure>)) | <procedure>`。


<h2 id="3">新语言的Y+实现</h2>

``` scheme
(run "let makemult = proc (maker) proc (x)
      if zero?(x)
            then 0
      else
            -(((maker maker) -(x,1)), -4)
      in let times4 = proc (x) ((makemult makemult) x) in (times4 3)")

;; ==> (num-val 12)

;;times
(run "let makemult = proc (maker) proc (x) proc(y)
      if zero?(x) then 0
      else -((((maker maker) -(x, 1)) y), -(0, y))
      in let times = proc(x) proc(y)
                    (((makemult makemult) x) y)
      in ((times 3) 8)")

;; -> (num-val 24)

(run "let makemult = proc (maker) proc (x) proc(y)
      if zero?(x) then 0
      else -((((maker maker) -(x, 1)) y), -(0, y))
      in let times = proc(x) proc(y)
                    (((makemult makemult) x) y)
      in let f = proc(func) proc(num)
        if zero?(num)
           then 1
        else
           ((times ((func func) -(num, 1))) num)
      in let fact = proc (x) ((f f) x)
      in (fact 5)")


(num-val 12)
(num-val 24)
(num-val 120)
```

最终提取一个更为精简的y-comb

``` scheme


(run "let makerec = proc (f)
        let d = proc (x)
          proc (z) ((f (x x)) z)
        in proc (n) ((f (d d)) n)
     in let maketimes4 = proc (f) proc (x)
          if zero?(x)
             then 0
          else -((f -(x,1)), -4)
      in let times4 = (makerec maketimes4) in (times4 3)")

    (num-val 12)
```
## 结论

Y-comb是一个痛苦和希望的承载者(持保留意见)，韵味十足.又像一个走过岁月年轮，在家等待着归家儿女的思妇，祈祷，承担。
Y-Comb需要丝丝宁静,雨夜飘过。

[1]:http://jueqingsizhe66.github.io/blog/2016/02/25/first-interpreter-from-eopl/ 
[2]:http://jueqingsizhe66.github.io/blog/2016/02/27/the-second-interpreter-from-one/ 
[3]:http://jueqingsizhe66.github.io/blog/2016/02/26/some-notations-in-the-design-of-the-language/ 
[4]:http://jueqingsizhe66.github.io/blog/2015/05/17/cong-lambdadao-simple-plus-complexjie-shi-qi-zai-dao-shu-xing-chou-xiang/ 
[5]:https://en.wikipedia.org/wiki/Fixed-point_combinator#Fixed_point_combinators_in_lambda_calculus
[6]:http://www.yinwang.org/blog-cn/2013/07/13/church-turing/
[7]:http://www.diku.dk/~neil/comp2book2007/book-whole.pdf
[8]:http://www.cs.utah.edu/~mflatt/past-courses/cs7520/public_html/s06/notes.pdf
[9]:http://www.ece.uc.edu/~franco/C511/html/Scheme/ycomb.html
