---
layout: post
title: "First Interpreter from EOPL"
date: 2016-02-25 11:20:09 +0800
comments: true
categories: scheme
---

这是EOPL一个最为基本的解释器，后面的解释器都在此基础上进行改造，所以
得理解各个部分，并知道如何使用和改造。最终告诉的是解释器如何解释表达式(每一种语言的表达式)。
**解释器解释表达式的问题**。
<!--more-->

+ [1. 测试Interpreter程序](#1)
+ [2. 测试结果](#2)
+ [3. 反思结果](#3)
    + [3.1 非expressions部分](#3.1)
    + [3.2 expressions部分](#3.2)
+ [4. 是解决问题，还是抽象问题？科学性何在?](#4)
    + [4.1 科学性问题何在？](#4.1)
    + [4.2 一个解释器该解决哪些问题？](#4.2)
    + [4.3 抽象的目的是什么](#4.3)
+ [5.一个解释器的具体实现](#5)
    + [5.1 语言前端表现形式](#5.1) lang
    + [5.2 语言数据结构](#5.2) data-structures
    + [5.3 语言中间阶段](#5.3) environments
    + [5.4 语言核心阶段](#5.4) interpreter
    + [5.5 语言的初始化](#5.5) drscheme-init
    + [5.6 语言的测试程序](#5.6) tests
    + [5.7 语言前端封装测试](#5.7)Top



******
<h2 id="1"> 1. 测试Interpreter程序</h2>

```

;;测试一个
(run-tests! run equal-answer? 
   '(
      (positive "11" 11)
    ))

;;测试二个
(run-tests! run-fn equal-answer? 
   '(
     (positive "11" 11)   
     (if-eval-test-true "if zero?(-(11,11)) then 3 else 4" 3)
    ))
   
```


<h2 id="2"> 2. 测试结果</h2>

+ one test: positive
```
    test: positive
    correct

    no bugs found

```
+ two tests:positive if-else
```
    test: positive
    correct

    test: if-eval-test-true
    correct

    no bugs found
```

<h2 id="3"> 3. 反思结果</h2>

结果测试很理想并没有错误，并且可以在test中不断增加判断语句。
而run-test的解析分为两个部分

+ expressions 这部分的工作主要是value-of
+ 非expressions  这部分的主要工作就是sligen通过the-grammer的定义进行解析

<h3 id="3.1"> 3.1 非expressions部分</h3>

这部分需要配合value-of的define-datatype进行对应的变化，且看the-grammer代码

``` scheme

  (define the-grammar
    '((program (expression) a-program)

      (expression (number) const-exp)
      (expression
        ("-" "(" expression "," expression ")")
        diff-exp)
      
      (expression
       ("zero?" "(" expression ")")
       zero?-exp)

      (expression
       ("if" expression "then" expression "else" expression)
       if-exp)

      (expression (identifier) var-exp)

      (expression
       ("let" identifier "=" expression "in" expression)
       let-exp)   

      ))
```

可以看到if,then,else都被当作不变的地方，而其中三个expressions被传动到value-of做进一步的解析。


<h3 id="3.2"> 3. expressions部分</h3>

继续看value-of的部分，则是对if-exp,var-exp,let-exp,letrec-exp,diff-exp,zerp?-exp?等进行解析的过程。

``` scheme

(define value-of
    (lambda (exp env)
      (cases expression exp

        (const-exp (num) (num-val num))

        (var-exp (var) (apply-env env var))

        (diff-exp (exp1 exp2)
          (let ((val1 (value-of exp1 env))
                (val2 (value-of exp2 env)))
            (let ((num1 (expval->num val1))
                  (num2 (expval->num val2)))
              (num-val
                (- num1 num2)))))

        (zero?-exp (exp1)
          (let ((val1 (value-of exp1 env)))
            (let ((num1 (expval->num val1)))
              (if (zero? num1)
                (bool-val #t)
                (bool-val #f)))))
              
        (if-exp (exp1 exp2 exp3)
          (let ((val1 (value-of exp1 env)))
            (if (expval->bool val1)
              (value-of exp2 env)
              (value-of exp3 env))))

        (let-exp (var exp1 body)       
          (let ((val1 (value-of exp1 env)))
            (value-of body
              (extend-env var val1 env))))

        )))
```

可以参考[cases的实现][2],以及[cases的作用][3]
当然[TLS][1]也提供了比如\*const,\*application等的定义
``` scheme
;第一大部分 定义好整体框架
(define value
 (lambda (e)
  (meaning e (quote ()))))
(define meaning
 (lambda (e table)
  ((expression-to-action e) e table))
```

其中的`(expression-to-action)`就会产生对应的`*const*,*identifier*,*lambda*
,*cond*,*quote*,*application*`以及[TSS][4]中新增加的`*set*,*define*,*let*`。

这两个部分是比较有趣的地方，规范了语言的前端部分，犹如左右手交换来交换去和penrose-stair![penrose-stair][5]
,进一步可以看看[盗墓笔记(inception)的评述][6]


<h2 id="4"> 4. 是解决问题，还是抽象问题？科学性何在?</h2>

科学进步的每一步发展，不是通过不断抽象来直觉抽取出进步，而是通过直觉发现问题，在利用抽象的方式进行求解，从而获得进步。
而抽象其实就是通过**interface**和**implementation**来进行，

+ interface的定义就是通过[define-datatype][7]进行，
+ implementation则是通过cases进行。


<h3 id="4.1">4.1 科学性问题何在？</h3>

现象和本质的探寻，透过现象看到本质，现象的产生都有其内在的原理，科学关注的是内在的本质联系。
scheme这门语言可以透过不断的抽象，产生许许多多的语言，但是这些语言层是否真的对科学问题有帮助，
这是值得怀疑的地方！科学必须要有怀疑！科学必须找寻问题所在，从而探索产生原因（持保留意见）。


<h3 id="4.2">4.2 一个解释器该解决哪些问题？</h3>

最基本的我觉得应该有两个：

+ 词法解析问题
    解决我输入的字符串，该如何由scan&parse进行解析
+ 语义解析问题
    解决我得到的表达式(expression),它怎么由value-of进行解析，到底是const-exp,var-exp,proc-exp,let-exp等


其实语言系列的表达主要有

1. imperative language(C,fortran)
2. functional programming language(Lisp,ML,Haskell,Scheme)
3. message passing language(OOP: Java,C++,)

他们在解析程序的过程中都离不开上述的两个基本问题的解析，他们也被叫做**前端(front-end)**。![front-end][8]

而任何语言如果仅仅有这个value的过程，而不会产生任何的effects，
比如：

+ print
+ read
+ modify the memory state or file system

那就太没有意思了，所以语言的完成需要他拓充到操作系统的处理、IO处理、显示处理，增加它的趣味性，而不是一味的解决问题（持保留意见）

+ 词法解析使我们获得从字符串到对应的语言的const-exp等的exp形式
+ 语义解析部分则是我们对const-exp等的具体实现部分

所以本质上来说这两点也是抽象的组成部分（[接口和实现][9]）,所以本质上解决解释器的实现问题，其实就是**如何抽象的问题?**。

<h3 id="4.3">4.3 抽象的目的是什么？</h3>

在于解决现实出现的科学问题。
也就是这些问题都能够被设定的接口和实现通过递归进行解释，并最终获得结果(result or outcome)。

而为什么要绕来绕去的谈这些问题呢？ 因为interpreter的实现过程本质也是递归的思想，在不断规约到一个结论。

<h2 id="5">5. 一个解释器的具体实现</h2>

参考EOPL第三章.

一般过程是先思考新语言的表现形式lang.scm，设定几个数据类型放入data-structures.scm中,
然后我们需要给这两个阶段设定bridge，也就是环境封装.这样我们就可以写一个解释器interpreter,解释并获得最终结果。
简而言之，其实我们就是想看看计算机是如何解释我们输入的这句话，具体就是[**解释器如何解析输入的表达式**][10]

1. 语言[前端表现形式](#5.1) lang
2. 语言[数据结构](#5.2) data-structures
3. 语言[中间阶段](#5.3) environments
4. 语言[核心阶段](#5.4) interpreter
5. 语言的[初始化](#5.5) drscheme-init
6. 语言的[测试程序](#5.6) tests
7. 语言[前端封装测试](#5.7)Top

<h3 id="5.1">5.1 前端表现形式</h3>

保存为lang.scm

该阶段的主要目的就是定义新生成的语言该是如何使用的？也就是语法部分。

``` scheme 

(module lang

  ;; grammar for the LET language

  (lib "eopl.ss" "eopl")                
  
  (require "drscheme-init.scm")
  
  (provide (all-defined-out))

  ;;;;;;;;;;;;;;;; grammatical specification ;;;;;;;;;;;;;;;;
  
  (define the-lexical-spec
    '((whitespace (whitespace) skip)
      (comment ("%" (arbno (not #\newline))) skip)
      (identifier
       (letter (arbno (or letter digit "_" "-" "?")))
       symbol)
      (number (digit (arbno digit)) number)
      (number ("-" digit (arbno digit)) number)
      ))
  
  (define the-grammar
    '((program (expression) a-program)

      (expression (number) const-exp)
      (expression
        ("-" "(" expression "," expression ")")
        diff-exp)
      
      (expression
       ("zero?" "(" expression ")")
       zero?-exp)

      (expression
       ("if" expression "then" expression "else" expression)
       if-exp)

      (expression (identifier) var-exp)

      (expression
       ("let" identifier "=" expression "in" expression)
       let-exp)   

      ))
  
  ;;;;;;;;;;;;;;;; sllgen boilerplate ;;;;;;;;;;;;;;;;
  
  (sllgen:make-define-datatypes the-lexical-spec the-grammar)
  
  (define show-the-datatypes
    (lambda () (sllgen:list-define-datatypes the-lexical-spec the-grammar)))
  
  (define scan&parse
    (sllgen:make-string-parser the-lexical-spec the-grammar))
  
  (define just-scan
    (sllgen:make-string-scanner the-lexical-spec the-grammar))
  
  )
```
<h3 id="5.2">5.2 数据结构</h3>


保存为data-structures.scm

该阶段的主要目的就是定义对应的类型该具有什么样的接口，这些接口的实现部分在第四部分有具体的体现。
这部分就是程序的**类型接口的转换部分**。
``` scheme

(module data-structures (lib "eopl.ss" "eopl")

  ;; data structures for let-lang.

  (provide (all-defined-out))               ; too many things to list

;;;;;;;;;;;;;;;; expressed values ;;;;;;;;;;;;;;;;

;;; an expressed value is either a number, a boolean or a procval.

  (define-datatype expval expval?
    (num-val
      (value number?))
    (bool-val
      (boolean boolean?)))

;;; extractors:

  ;; expval->num : ExpVal -> Int
  ;; Page: 70
  (define expval->num
    (lambda (v)
      (cases expval v
	(num-val (num) num)
	(else (expval-extractor-error 'num v)))))

  ;; expval->bool : ExpVal -> Bool
  ;; Page: 70
  (define expval->bool
    (lambda (v)
      (cases expval v
	(bool-val (bool) bool)
	(else (expval-extractor-error 'bool v)))))

  (define expval-extractor-error
    (lambda (variant value)
      (eopl:error 'expval-extractors "Looking for a ~s, found ~s"
	variant value)))

;;;;;;;;;;;;;;;; environment structures ;;;;;;;;;;;;;;;;

;; example of a data type built without define-datatype

  (define empty-env-record
    (lambda () 
      '()))

  (define extended-env-record
    (lambda (sym val old-env)
      (cons (list sym val) old-env)))
  
  (define empty-env-record? null?)
  
  (define environment?
    (lambda (x)
      (or (empty-env-record? x)
          (and (pair? x)
               (symbol? (car (car x)))
               (expval? (cadr (car x)))
               (environment? (cdr x))))))

  (define extended-env-record->sym
    (lambda (r)
      (car (car r))))

  (define extended-env-record->val
    (lambda (r)
      (cadr (car r))))

  (define extended-env-record->old-env
    (lambda (r)
      (cdr r)))

)
```

<h3 id="5.3">5.3 中间阶段</h3>

保存为environments.scm

该阶段的主要目的是为了解释当遇到一些变量或者之后的解释器的lambda（将在下一个版本解释器体现），如何保存下来
，并供对应程序使用。
``` scheme
(module environments (lib "eopl.ss" "eopl") 
  
  ;; builds environment interface, using data structures defined in
  ;; data-structures.scm. 

  (require "data-structures.scm")

  (provide init-env empty-env extend-env apply-env)

;;;;;;;;;;;;;;;; initial environment ;;;;;;;;;;;;;;;;
  
  ;; init-env : () -> Env
  ;; usage: (init-env) = [i=1, v=5, x=10]
  ;; (init-env) builds an environment in which i is bound to the
  ;; expressed value 1, v is bound to the expressed value 5, and x is
  ;; bound to the expressed value 10.
  ;; Page: 69

  (define init-env 
    (lambda ()
      (extend-env 
       'i (num-val 1)
       (extend-env
        'v (num-val 5)
        (extend-env
         'x (num-val 10)
         (empty-env))))))

;;;;;;;;;;;;;;;; environment constructors and observers ;;;;;;;;;;;;;;;;

  (define empty-env
    (lambda ()
      (empty-env-record)))
  
  (define empty-env? 
    (lambda (x)
      (empty-env-record? x)))

  (define extend-env
    (lambda (sym val old-env)
      (extended-env-record sym val old-env)))

  (define apply-env
    (lambda (env search-sym)
      (if (empty-env? env)
	(eopl:error 'apply-env "No binding for ~s" search-sym)
	(let ((sym (extended-env-record->sym env))
	      (val (extended-env-record->val env))
	      (old-env (extended-env-record->old-env env)))
	  (if (eqv? search-sym sym)
	    val
	    (apply-env old-env search-sym))))))

  )
```

<h3 id="5.4">5.4 核心阶段</h3>

保存为interp.scm

该阶段的主要目的就是编写针对第3阶段生成的var-exp、const-exp等expval的解析，算是核心部分，最终的结果的产生地，
后面的几个阶段只不过是针对这部分内容做了一些前端的准备，算是死门，而这部分和前面的词法解析、类型转换接口算是一个语言的生门部分。
``` scheme

(module interp (lib "eopl.ss" "eopl")
  
  ;; interpreter for the LET language.  The \commentboxes are the
  ;; latex code for inserting the rules into the code in the book.
  ;; These are too complicated to put here, see the text, sorry.

  (require "drscheme-init.scm")

  (require "lang.scm")
  (require "data-structures.scm")
  (require "environments.scm")

  (provide value-of-program value-of)

;;;;;;;;;;;;;;;; the interpreter ;;;;;;;;;;;;;;;;

  ;; value-of-program : Program -> ExpVal
  ;; Page: 71
  (define value-of-program 
    (lambda (pgm)
      (cases program pgm
        (a-program (exp1)
          (value-of exp1 (init-env))))))

  ;; value-of : Exp * Env -> ExpVal
  ;; Page: 71
  (define value-of
    (lambda (exp env)
      (cases expression exp

        (const-exp (num) (num-val num))

        (var-exp (var) (apply-env env var))

        (diff-exp (exp1 exp2)
          (let ((val1 (value-of exp1 env))
                (val2 (value-of exp2 env)))
            (let ((num1 (expval->num val1))
                  (num2 (expval->num val2)))
              (num-val
                (- num1 num2)))))

        (zero?-exp (exp1)
          (let ((val1 (value-of exp1 env)))
            (let ((num1 (expval->num val1)))
              (if (zero? num1)
                (bool-val #t)
                (bool-val #f)))))
              
        (if-exp (exp1 exp2 exp3)
          (let ((val1 (value-of exp1 env)))
            (if (expval->bool val1)
              (value-of exp2 env)
              (value-of exp3 env))))

        (let-exp (var exp1 body)       
          (let ((val1 (value-of exp1 env)))
            (value-of body
              (extend-env var val1 env))))

        )))


  )

```

<h3 id="5.5">5.5 初始化</h3>

保存为drscheme-init.scm

该阶段主要目的是编写测试需要的套件，提供测试是否成功的判断标准。
``` scheme

;; drscheme-init.scm - compatibility file for DrScheme
;; usage: (require "drscheme-init.scm")

;;; makes structs printable, and provides basic functionality for
;;; testing.  This includes pretty-printing and tracing.


(module drscheme-init mzscheme

  ;; show the contents of define-datatype values
  (print-struct #t)

  (require (lib "pretty.ss"))
  (provide (all-from (lib "pretty.ss")))

  (require (lib "trace.ss"))
  (provide (all-from (lib "trace.ss")))

  (provide make-parameter)

  (provide 
   run-experiment
   run-tests!
   stop-after-first-error
   run-quietly
   )
  
  ;; safely apply procedure fn to a list of args.
  ;; if successful, return (cons #t val)
  ;; if eopl:error is invoked, returns (cons #f string), where string is the
  ;; format string generated by eopl:error.  If somebody manages to raise a 
  ;; value other than an exception, then the raised value is reported.
  
  (define apply-safely
    (lambda (proc args)
      (with-handlers ([(lambda (exn) #t)      ; catch any error
                       (lambda (exn)          ; evaluate to a failed test result
                         (cons #f 
                               (if (exn? exn)
                                   (exn-message exn)
                                   exn)))])  
        (let ([actual (apply proc args)])
          (cons #t actual)))))

  ;; run-experiment :
  ;;  ((a ...) -> b) * (a ...) * b * (b * b -> bool)
  ;;  -> (cons bool b)
  
  ;; usage: (run-experiment fn args correct-answer equal-answer?)
  ;; Applies fn to args.  Compares the result to correct-answer. 
  ;; Returns (cons bool b) where bool indicates whether the
  ;; answer is correct.

  (define run-experiment
    (lambda (fn args correct-answer equal-answer?)
      (let*
          ((result (apply-safely fn args))
           ;; ans is either the answer or the args to eopl:error
           (error-thrown? (not (car result)))
           (ans (cdr result)))
          
        (cons
         (if (eqv? correct-answer 'error)
             error-thrown?
             (equal-answer? ans correct-answer))
         ans))))
  
  (define stop-after-first-error (make-parameter #f))
  (define run-quietly (make-parameter #t))
   
  ;; run-tests! : (arg -> outcome) * (any * any -> bool) * (list-of test)
  ;;             -> unspecified

  ;; where:
  ;; test ::= (name arg outcome)
  ;; outcome ::= ERROR | any
  
  ;; usage: (run-tests! run-fn equal-answer? tests)

  ;; for each item in tests, apply run-fn to the arg.  Check to see if
  ;; the outcome is right, comparing values using equal-answer?.

  ;; print a log of the tests.

  ;; at the end, print either "no bugs found" or the list of tests
  ;; failed. 
  
  ;; Normally, run-tests! will recover from any error and continue to
  ;; the end of the test suite.  This behavior can be altered by
  ;; setting (stop-after-first-error #t).

  (define (run-tests! run-fn equal-answer? tests)
    (let ((tests-failed '()))
      (for-each
       (lambda (test-item)
         (let ((name (car test-item))
               (pgm (cadr test-item))
               (correct-answer (caddr test-item)))
           (printf "test: ~a~%" name)
           (let* ((result
                   (run-experiment
		     run-fn (list pgm) correct-answer equal-answer?))
                  (correct? (car result))
                  (actual-answer (cdr result)))
             (if (or
                   (not correct?)
                   (not (run-quietly)))
               (begin
                 (printf "~a~%" pgm)
                 (printf "correct outcome: ~a~%" correct-answer)
                 (printf "actual outcome:  ")
                 (pretty-display actual-answer)))
             (if correct?
                 (printf "correct~%~%")
                 (begin
                   (printf "incorrect~%~%")
                   ;; stop on first error if stop-after-first? is set:
                   (if (stop-after-first-error)
                       (error name "incorrect outcome detected")) 
                   (set! tests-failed
                         (cons name tests-failed)))))))
       tests)
      (if (null? tests-failed)
          (printf "no bugs found~%")
          (printf "incorrect answers on tests: ~a~%"
            (reverse tests-failed)))))

  )  
  
  
```

<h3 id="5.6">5.6 测试程序</h3>

保存为tests.scm

该阶段的主要目的就是编写测试语句，包括常量、变量、表达式、if、diff、let等语句的使用。

``` scheme
(module tests mzscheme
  
  (provide test-list)

  ;;;;;;;;;;;;;;;; tests ;;;;;;;;;;;;;;;;
  
  (define test-list
    '(
  
      ;; simple arithmetic
      (positive-const "11" 11)
      (negative-const "-33" -33)
      (simple-arith-1 "-(44,33)" 11)
  
      ;; nested arithmetic
      (nested-arith-left "-(-(44,33),22)" -11)
      (nested-arith-right "-(55, -(22,11))" 44)
  
      ;; simple variables
      (test-var-1 "x" 10)
      (test-var-2 "-(x,1)" 9)
      (test-var-3 "-(1,x)" -9)
      
      ;; simple unbound variables
      (test-unbound-var-1 "foo" error)
      (test-unbound-var-2 "-(x,foo)" error)
  
      ;; simple conditionals
      (if-true "if zero?(0) then 3 else 4" 3)
      (if-false "if zero?(1) then 3 else 4" 4)
      
      ;; test dynamic typechecking
      (no-bool-to-diff-1 "-(zero?(0),1)" error)
      (no-bool-to-diff-2 "-(1,zero?(0))" error)
      (no-int-to-if "if 1 then 2 else 3" error)

      ;; make sure that the test and both arms get evaluated
      ;; properly. 
      (if-eval-test-true "if zero?(-(11,11)) then 3 else 4" 3)
      (if-eval-test-false "if zero?(-(11, 12)) then 3 else 4" 4)
      
      ;; and make sure the other arm doesn't get evaluated.
      (if-eval-test-true-2 "if zero?(-(11, 11)) then 3 else foo" 3)
      (if-eval-test-false-2 "if zero?(-(11,12)) then foo else 4" 4)

      ;; simple let
      (simple-let-1 "let x = 3 in x" 3)

      ;; make sure the body and rhs get evaluated
      (eval-let-body "let x = 3 in -(x,1)" 2)
      (eval-let-rhs "let x = -(4,1) in -(x,1)" 2)

      ;; check nested let and shadowing
      (simple-nested-let "let x = 3 in let y = 4 in -(x,y)" -1)
      (check-shadowing-in-body "let x = 3 in let x = 4 in x" 4)
      (check-shadowing-in-rhs "let x = 3 in let x = -(x,1) in x" 2)

      ))
  )
```

<h3 id="5.7">5.7 前端封装测试</h3>

保存为top.scm

该阶段的目的就是测试这个新的解释器语言到底有没有用？他是否能够解释从tests中导入的
程序片段？是否测试通过。

``` scheme
(module top (lib "eopl.ss" "eopl")
  
  ;; top level module.  Loads all required pieces.
  ;; Run the test suite with (run-all).

  (require "drscheme-init.scm")
  (require "data-structures.scm")  ; for expval constructors
  (require "lang.scm")             ; for scan&parse
  (require "interp.scm")           ; for value-of-program
  (require "tests.scm")            ; for test-list
  
  ;; since this is the top-level module, we don't really need to
  ;; provide anything, but we do so just in case.  

  (provide run run-all)

  (provide test-all)

  (define (test-all) (run-all))

  ;; here are some other things that could be provided:

  ;;   (provide (all-defined-out))
  ;;   (provide (all-from "interp.scm"))
  ;;   (provide (all-from "lang.scm"))
  
  ;;;;;;;;;;;;;;;; interface to test harness ;;;;;;;;;;;;;;;;
  
  ;; run : String -> ExpVal
  ;; Page: 71
  (define run
    (lambda (string)
      (value-of-program (scan&parse string))))
  
  ;; run-all : () -> unspecified
  
  ;; runs all the tests in test-list, comparing the results with
  ;; equal-answer?  

  (define run-all
    (lambda ()
      (run-tests! run equal-answer? test-list)))
  
  (define equal-answer?
    (lambda (ans correct-ans)
      (equal? ans (sloppy->expval correct-ans))))
  
  (define sloppy->expval 
    (lambda (sloppy-val)
      (cond
        ((number? sloppy-val) (num-val sloppy-val))
        ((boolean? sloppy-val) (bool-val sloppy-val))
        (else
         (eopl:error 'sloppy->expval 
                     "Can't convert sloppy value to expval: ~s"
                     sloppy-val)))))
    
  ;; run-one : symbol -> expval

  ;; (run-one sym) runs the test whose name is sym
  
  (define run-one
    (lambda (test-name)
      (let ((the-test (assoc test-name test-list)))
        (cond
          ((assoc test-name test-list)
           => (lambda (test)
                (run (cadr test))))
          (else (eopl:error 'run-one "no such test: ~s" test-name))))))
 
  ;; (run-all)
  
  )


```

******

[1]: http://jueqingsizhe66.github.io/blog/2016/02/09/its-a-dead-program-dot-how-to-let-it-alive/#1
[2]: http://jueqingsizhe66.github.io/blog/2016/02/19/the-implementation-of-define-datetype/
[3]: http://jueqingsizhe66.github.io/blog/2016/02/23/casesde-zuo-yong/
[4]: http://jueqingsizhe66.github.io/blog/2016/02/09/its-a-dead-program-dot-how-to-let-it-alive/#2
[5]: /images/penrose-stair.png
[6]: http://df.xq0510.blog.163.com/blog/static/16013465820108704920944/ 
[7]: http://jueqingsizhe66.github.io/blog/2016/02/19/the-implementation-of-define-datetype/ 
[8]: /images/lisp/front-end.png
[9]: http://jueqingsizhe66.github.io/blog/2016/02/15/data-representation-the-same-interface-with-different-implementation/ 
[10]:https://mitpress.mit.edu/sicp/full-text/book/book-Z-H-35.html#%_sec_5.5 
