---
layout: post
title: "cases的作用"
date: 2016-02-23 12:10:12 +0800
comments: true
categories: scheme
---

为什么要实现[cases][1],目的在于进一步抽象简化过程。
<!--more-->

## 原先的程序

          procedure :   Var * Exp * Env ->  Proc
    apply-procedure :   Proc * ExpVal   ->  ExpVal

``` scheme
 ;; procedure : Var * Exp * Env -> Proc
 ;; Page: 79
  (define procedure
    (lambda (var body env)
      (lambda (val)
        (value-of body (extend-env var val env)))))
  
  ;; apply-procedure : Proc * ExpVal -> ExpVal
  ;; Page: 79
  (define apply-procedure
    (lambda (proc val)
      (proc val)))
```


## 简化的程序
    apply-procedure : Proc * ExpVal -> ExpVal

``` scheme
  ;; apply-procedure : Proc * ExpVal -> ExpVal
  ;; Page: 79
  (define apply-procedure
    (lambda (proc1 val)
      (cases proc proc1
        (procedure (var body saved-env)
          (value-of body (extend-env var val saved-env))))))

```

+ 通过cases的proc1找到输入的expressions（scheme的expressions即是数据）,转移val分配到extend-env中，
+ 通过变量名和变量列表`(var body...)`引入var，
+ 并形成`(var val)`词汇表打入saved-env，
+ 最后利用value-of解析body，也就是解析由`(var body saved-env)`构成的closure，递归解析所有的body,
  如果proc1满足`(procedure (var body saved-env))`的形式，则通过value-of进行解析.

saved-env是假定的保存的环境变量，而对应的apply-procedure的最终结果是`(proc1 val)`

## value-of实现

它是在语言设计阶段比较重要的实现，在其中可以存在很多的变种。

``` scheme
  ;; value-of : Exp * Env -> ExpVal
  (define value-of
    (lambda (exp env)
      (cases expression exp

        ;\commentbox{ (value-of (const-exp \n{}) \r) = \n{}}
        (const-exp (num) (num-val num))

        ;\commentbox{ (value-of (var-exp \x{}) \r) = (apply-env \r \x{})}
        (var-exp (var) (apply-env env var))

        ;\commentbox{\diffspec}
        (diff-exp (exp1 exp2)
          (let ((val1 (value-of exp1 env))
                (val2 (value-of exp2 env)))
            (let ((num1 (expval->num val1))
                  (num2 (expval->num val2)))
              (num-val
                (- num1 num2)))))

        ;\commentbox{\zerotestspec}
        (zero?-exp (exp1)
          (let ((val1 (value-of exp1 env)))
            (let ((num1 (expval->num val1)))
              (if (zero? num1)
                (bool-val #t)
                (bool-val #f)))))
              
        ;\commentbox{\ma{\theifspec}}
        (if-exp (exp1 exp2 exp3)
          (let ((val1 (value-of exp1 env)))
            (if (expval->bool val1)
              (value-of exp2 env)
              (value-of exp3 env))))

        ;\commentbox{\ma{\theletspecsplit}}
        (let-exp (var exp1 body)       
          (let ((val1 (value-of exp1 env)))
            (value-of body
              (extend-env var val1 env))))
        
        (proc-exp (var body)
          (proc-val (procedure var body env)))

        (call-exp (rator rand)
          (let ((proc (expval->proc (value-of rator env)))
                (arg (value-of rand env)))
            (apply-procedure proc arg)))

        )))
```

+ 在value-of中，如果exp满足`if-exp (exp1 exp2 exp3)`则解析乘对应的，
+ 依次类推,当看到`let-exp (va exp1 body)`的形式则对应的进行演变.

[1]: http://jueqingsizhe66.github.io/blog/2016/02/19/the-implementation-of-define-datetype/
