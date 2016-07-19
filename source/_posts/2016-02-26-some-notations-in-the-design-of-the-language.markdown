---
layout: post
title: "Some notations in the design of the language"
date: 2016-02-26 15:35:17 +0800
comments: true
categories: scheme
---

What does it mean by Kleene star? What can it be used for? 
It is not so important,but a good tools to record the grammer
of the language.目的就是如何更简洁地定义语法。
<!--more-->

## 几个基本字符

### Nonterminal Symbols

指代被定义的集合名字(These are the names of the sets being defined)，比如List-of-Int,List-of-Char...,
这些集合有时候也被叫做语义分类(syntactic categories)

而且一般是of连接词是小写，其他表征类型的是大写(capitalized)。
这是一个习惯，当然如果指引某个单一的元素的时候我们会用小写字母，
比如 Expression is a nonterminal, 可以写成
```
  e <- Expression  or
  e is an expression
```

当然我们也可以写成更为简单的形式，叫做Backus-Naur Form(BNF),即用尖括号包裹小写字母表达式，
```
 <expression>
```


### Terminal Symbols:

+ left parenthesis 左括号 `(`
+ right parenthesis 右括号 `)`
+ period 句号 点号  `.`

### Production(The rules)

The rules are called productions.每一个Production包含left-handside（lhs),right-handside(rhs),并用`::=`来连接
lhs和rhs，`::=`也被读作*is or can be*

+ lhs一般是nonterminal symbol,表征的是语义类(syntactic category)
+ rhs一般是由terminal symbols(比如left and right parenthesis ,or period)和nonterminal symbols 组合起来。
+ rhs的主要作用是用来按照其他语义分类(每一行都叫做一种语义分类）和Terminal symbolsl指定(specify)对应语义类
  的构造成员的方法(a method of constructing the members of the syntactic category)


1. `Int ::= (1 2 3 ....)`

2. `Double ::= (1.11246 1.2 1.3 ...)`

3. `Char ::= (a b c ...)`

4. `Float ::= (1.1 1.2 1.3 ...)`

5. `List-of-Int ::= ()`
   `List-of-Int ::= (Int . List-of-Int)`
6. `Lists ::= ()|(Scheme Val . List)`

当然有时候我们也可以忽略第二个语义类的List-of-Int,比如

7. `List-of-Int ::= ()`
   `            ::= (Int . List-of-Int)`

当然我们也可以只用一行来表示语义类（归一为一条语义）：使用vertical bar(| ),也被读作or

8. `List-of-Int ::= ()|(Int . List-of-Int)`

另外我们也可以使用一种更为简洁的形式： 使用 Kleene Star利用大括号(flower braces), `{}*`

9. `List-of-Int ::= ({Int}*)`

其中`*`表示的是0个或者0个以上，也可以变换为`+`。
`+`:表示的是一个或者一个以上.

10. `List-of-Int ::= ({Int}+)`

当然我们也可以规定Int之间的分割标志,利用seperation list

11. `List-of-Int ::= ({Int}*(,))  `
表示的是
```
8
1,2
3,4,6
```
List-of-Int ::= (`{Int}*(;)`)  
```
8
1;2
3;4;6
```

******
## 语义推导(Syntactic derivation)练习

+ (14 . ()) is a List-of-Int?
```
List-of-Int
=> (Int . (List-of-Int))
=> (14  . (List-of-Int))
=> (14  . ())

so, it is true.
```


+ (-7 . (3 . (14 . ()))) is a List-of-Int?
```
List-of-Int
=> (Int . (List-of-Int))
=> (-7  . (List-of-Int))
=> (-7  . (3 . (List-of-Int)))
=> (-7  . (3 . (14 . (List-of-Int))))
=> (-7  . (3 . (14 . ())))

so, it is true.
```

+ (a b c) is a S-list?

因为 
```
S-list = ({S-exp}(`*`))
S-exp  = Symbol | S-list
```
```
S-list
=> ({S-exp}(*))
=> {Symbol | S-list}(*)
=> {a | S-list}(*)
=> {a b |S-list}(*)
=> {a b c}

so, it is S-list
```

+ (bar (biz 4 6) (foo 1 2)) is a binary tree?

二叉树是非常重要的数据结构，它的定义形式是

```
  Bintree ::= Int| (Symbol Bintree Bintree)
```

所以我们可以这样解析

```
 Bintree
=>(Int|(Symbol Bintree Bintree))
=>(bar (Int|(Symbol Bintree Bintree)) (Int|(Symbol Bintree Bintree)))
=>(bar (biz (Int|(Symbol Bintree Bintree)) (Int|(Symbol Bintree Bintree))) 
       (foo (Int|(Symbol Bintree Bintree)) (Int|(Symbol Bintree Bintree))))
=>(bar (biz 4 6) 
       (foo (Int|(Symbol Bintree Bintree)) (Int|(Symbol Bintree Bintree))))
=>(bar (biz 4 6) 
       (foo 1 2))

So,it is true

```

+ (lambda (x) (+ x 6)) is a lambda calculus

lambda calculus 是程序语言设计中十分重要的小型语言。该门语言仅仅包含

1. 变量引用(varient references)
2. 单参过程(procedures that takes a single argument)
3. 过程调用(procedure calls)

定义如下

```
   LcExp ::= Identifier
         ::= (lambda (Identifier) LcExp)
         ::= (LcExp LcExp)
```

所以我们可以判断如下

```
 LcExp
=> Identifier|(lambda (Identifier) LcExp)|(LcExp LcExp)
=> (lambda (Identifier) LcExp)
=> (lambda (x) Identifier|(lambda (Identifier) LcExp)|(LcExp LcExp))
=> (lambda (x) (LcExp LcExp))
=> (lambda (x) (+ Identifier|(lambda (Identifier) LcExp)|(LcExp LcExp)
                  Identifier|(lambda (Identifier) LcExp)|(LcExp LcExp)))
=> (lambda (x) (+ x
                  Identifier|(lambda (Identifier) LcExp)|(LcExp LcExp)))
=> (lambda (x) (+ x
                  6))

So, it is true.


```


## 结论

上米那说的List-of-Int,Bintree，Lambda calculus等都可以叫做一种语义类，是一门语言
包含的原始类型。上面的syntatic derivation也可以被用来当作理论证明的一种入门，本质是递归理论。

而其实他们都可以当作最为基本的解释器，因为他们在不断的解析他们所识别的expressions，上面的LcExp,
List-exp等都叫做expressions，也是解释器的元素。

而假如我们有一个函数 
```
nth-element : List * Int -> SchemeVal
```
他说明的是nth-element是一个接受List和Int作为形参并返回SchemeVal的过程，也就是它属于LcExp范畴。


## subst采用kleene star进行书写

subst的定义是
```
subst : Sym * Sym * S-list -> S-list
```

下面的实现过程从最为原始的只能处理lat的形式变为能够处理任意嵌套的expressions形式。

``` scheme
#lang racket

;;替换在slist中old值出现的值为new值（引用值）
(define subst-original
  (lambda (new old slist)
    (if (null? slist)
	'()
	(cons
	       (if (eqv? (car slist) old)
               new
               (car slist))
	 (subst new old (cdr slist))))))

;;map改进失败
(define subst-wrong
  (lambda (new old lst)
    (map (lambda (x) (if (eq? x old) new old)) lst)))
;;通过map 简化subst-original的原始写法
(define subst-map
  (lambda (new old lst)
    (map (lambda (x) (if (eq? x old) new x)) lst)))

;;;通过subst改进S-expression to list-expression
(define subst
  (lambda (new old slist)
    (if (null? slist)
	'()
	(cons
	 (let ((sexp (car slist)))
	   (if (symbol? sexp)
	       (if (eqv? sexp old)
               new
               sexp)
	       (subst new old sexp)))
	 (subst new old (cdr slist))))))

;;;通过map 改进subst
(define subst-map-improve
  (lambda (new old lst)
    (map (lambda (x) (if (symbol? x) (if (eq? x old) new x) (subst-map new old x))) lst)))

(subst-original  'a 'b '(a b c (b b) d b))
;'(a a c (a a) d a)
(subst-map 'a 'b '(a b c (b b) d b))
;'(a a c (b b) d a)
(subst-wrong 'a 'b '(a b c (b b) d b))  ;;;错误在于所有的非old值都替换为old值了
;'(b a b b b a)
(subst 'a 'b '(a b c (b b) d b))
;;'(a a c (a a) d a)
(subst-map-improve 'a 'b '(a b c (b b) d b))
;'(a a c (a a) d a)
```

这个拓展的例子虽然和表达式的注释主题关系不大，但是确是对于递归递归理论的进一步阐述。递归要求满足两个条件。

1. 递归出口(null? slt)  (zero? (car lst)) ...
2. 逐渐朝向递归出口的算子 (cdr lst) (- n 1) ...

## define-datatype的运用

具体实现可以查看[The Implementation of Define-datatype][1].


### Lc-Exp 和S-list的具体实现
上文所提记得Lc-Exp的定义为：
```
    lc-exp  ::=identifier
            ::=(lambda (identifier) Lc-exp
            ::=(lc-exp lc-exp)

```

它使用define-datatype的实现如下：
```
(define-datatype lc-exp lc-exp?
    (var-exp (var identifier?))
    (lambda-exp (bound-var identifier?)
                (body lc-exp?))
    (app-exp (rator lc-exp?)
             (rand lc-exp?))
)

```
而S-list的定义再现：
```
   S-list   ::=({S-exp}*) 
   S-exp:=Symbol|S-list
```

他对应的实现如下

```
(define-datatype s-list s-list?
    (empty-s-list)
    (non-empty-s-list
        (first s-exp?)
        (rest s-list?))
)
(define-datatype s-exp s-exp?
    (symbol-s-exp (sym symbol?))
    (s-list-s-exp (slst s-list?))
)

```

### expression 和 expval的具体定义和实现

在Interpreter的过程中肯定涉及到expression的设计和value值的表现（**把exp和val的键值对叫做解释器的字典**）
expression的具体定义是
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
```

具体实现如下所示:

```
;;; datatype ;;;
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
  (diff-exp
   (exp1 expression?)
   (exp2 expression?))
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
  (list-exp
   (args (list-of expression?)))
  (let-exp
   (vars (list-of symbols?))
   (vals (list-of expression?))
   (body expression?))
  (proc-exp
   (var (list-of symbol?))
   (body expression?))
  (call-exp
   (rator expression?)
   (rand (list-of expression?))))

```

expval的定义(主要是用于scheme value)如下:

```
 expval ::= (num-val ())
        ::= (bool-val ())
        ::= (proc-val ())
        ::= (pair-val ())
        ::= (emptylist-val)
```

具体实现如下 所示：
```
;;; an expressed value is either a number, a boolean or a procval.
(define-datatype expval expval?
  (num-val
   (value number?))
  (bool-val
   (boolean boolean?))
  (proc-val
   (proc proc?))
  (pair-val
   (car expval?)
   (cdr expval?))
  (emptylist-val))

```

proc的具体定义如下，
```
  proc ::= (procedure ())
```

proc的实现如下所示：
```
;; proc? : SchemeVal -> Bool
;; procedure : Var * Exp * Env -> Proc
(define-datatype proc proc?
  (procedure
   (var (list-of symbol?))
   (body expression?)
   (env environment?)))


```

当然具体的Define-datatype的定义还得配合[cases的作用][2]进行实现。

[1]:http://jueqingsizhe66.github.io/blog/2016/02/19/the-implementation-of-define-datetype/ 
[2]:http://jueqingsizhe66.github.io/blog/2016/02/23/casesde-zuo-yong/ 
