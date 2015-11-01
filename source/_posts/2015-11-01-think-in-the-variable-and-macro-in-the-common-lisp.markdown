---
layout: post
title: "Think in the variable and macro in the common lisp"
date: 2015-11-01 17:26:23 +0800
comments: true
categories: Lisp
---

As you all know we can use defun to define a function in the common lisp,

``` common-lisp

(defun add (x)
    (+ x 1))
(defun foo (a b c)
    (list  a b c))
```

Definition above is the most essential method to create a function. Due to the 

**convience** we will have to think about the variables and functions again.

Writing thinking path: from thinking the variables to functions and get the knowledge of the common lisp.

<!--more-->


## I. Variables

We we execute one function ,we should let the interpreter know:

1. call the Mac first? or call the compiler first?
2. Mac will not get the value of the variables ,but change the expression to  another forms
3. But compiler will directly get the value of the variables.

Variables can be shown with &key,&optional,&rest,&body forms etc.

### keyword variables

The variables start with colon( it means :) is the keyword variables, but there are exception,

such as in the defsystem(asdf grammer), you can depends on other system (depends on :macro-utilities)

here macro-utilities is called as a system,not the keyword variables.

``` common-lisp

(defun foo (&key a (b 20) (c 30 c-p)) 
    (list  a b c c-p))

```

### optional variables

``` common-lisp

(defun foo (a &optional (b 20) (c 30 c-p))  ;;c-supplied-p

    (list  a b c c-p))

;; it means the variables b and c is optional ,if b is not written in the
;; the parameter list, the interpreter will set b = 20,similary  c.

(defun make-rectangle (width &optional (height width)) ...)

;;; the rectangle will set height =width when height is not supplied.

(defun foo (a &optional (b 20) (c 30 c-supplied-p)) 

    (list  a b c c-supplied-p))


```

### rest variables


``` common-lisp

(defun format (stream string &rest values) ...)

(defun + (&rest numbers) ...)

(defmacro when (condition &rest body)
    `(if (not ,condition) (progn ,@body) ;; The difference between , and ,@ in the `() is ,@ will delete the brace more than once 

    ;; (if condition then-part [else-part])
)

```

### body  variables

``` common-lisp


```

### macro definition

I think Mac(Macro-> defmacro) is a form to change the input of the variables.Actually it is 

most powerful tool in the lisp programming

``` common-lisp

(format t "hello world")

(defun backwords (str) 
    (reverse str))

(backwords ("hello world" t format))  ;; take care not (backwords '("hello world" t format))
```

So when you execute the backwords macro, the interpreter will tell the Mac to do the works.

When  he finished his work. The compiler will continue execute the result of the Mac. That

is the process of the macro function.


## II. Functions

Orient Object Programming's CLOS is  to tell you how to rearrange the functions and variables into one class.

``` common-lisp


```


## III. Format function

format is a most powerful decorate function to output in the common lisp.

so every field in the format function , you should take note *~* ,such as *~a*, *~{~}*, *~%*, *~:a*, *~r*,*~d* etc


``` common-lisp


```

**added-later**
