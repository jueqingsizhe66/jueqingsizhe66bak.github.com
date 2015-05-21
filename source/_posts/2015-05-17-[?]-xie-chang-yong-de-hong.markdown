---
layout: post
title: "一些常用的宏"
date: 2015-05-17 14:24:23 +0800
comments: true
categories: scheme
---

无意中在一个scheme的[论坛](http://www.schemers.org/Documents/Standards/R5RS/HTML/)看到这个好东西，包含着几个常见的宏的编写
<!--more-->

```scheme
7.3  Derived expression types


 This section gives macro definitions for the derived expression types in terms of the primitive expression types (literal, variable, call, lambda, if, set!). See section 6.4 for a possible definition of delay.


(define-syntax cond
   (syntax-rules (else =>)
     ((cond (else result1 result2 ...))
      (begin result1 result2 ...))
     ((cond (test => result))
      (let ((temp test))
        (if temp (result temp))))
     ((cond (test => result) clause1 clause2 ...)
      (let ((temp test))
        (if temp
            (result temp)
            (cond clause1 clause2 ...))))
     ((cond (test)) test)
     ((cond (test) clause1 clause2 ...)
      (let ((temp test))
        (if temp
            temp
            (cond clause1 clause2 ...))))
     ((cond (test result1 result2 ...))
      (if test (begin result1 result2 ...)))
     ((cond (test result1 result2 ...)
            clause1 clause2 ...)
      (if test
          (begin result1 result2 ...)
          (cond clause1 clause2 ...)))))



(define-syntax case
   (syntax-rules (else)
     ((case (key ...)
        clauses ...)
      (let ((atom-key (key ...)))
        (case atom-key clauses ...)))
     ((case key
        (else result1 result2 ...))
      (begin result1 result2 ...))
     ((case key
        ((atoms ...) result1 result2 ...))
      (if (memv key '(atoms ...))
          (begin result1 result2 ...)))
     ((case key
        ((atoms ...) result1 result2 ...)
        clause clauses ...)
      (if (memv key '(atoms ...))
          (begin result1 result2 ...)
          (case key clause clauses ...)))))



(define-syntax and
   (syntax-rules ()
     ((and) #t)
     ((and test) test)
     ((and test1 test2 ...)
      (if test1 (and test2 ...) #f))))



(define-syntax or
   (syntax-rules ()
     ((or) #f)
     ((or test) test)
     ((or test1 test2 ...)
      (let ((x test1))
        (if x x (or test2 ...))))))



(define-syntax let
   (syntax-rules ()
     ((let ((name val) ...) body1 body2 ...)
      ((lambda (name ...) body1 body2 ...)
       val ...))
     ((let tag ((name val) ...) body1 body2 ...)
      ((letrec ((tag (lambda (name ...)
                       body1 body2 ...)))
         tag)
       val ...))))



(define-syntax let*
   (syntax-rules ()
     ((let* () body1 body2 ...)
      (let () body1 body2 ...))
     ((let* ((name1 val1) (name2 val2) ...)
        body1 body2 ...)
      (let ((name1 val1))
        (let* ((name2 val2) ...)
          body1 body2 ...)))))


 The following letrec macro uses the symbol <undefined> in place of an expression which returns something that when stored in a location makes it an error to try to obtain the value stored in the location (no such expression is defined in Scheme). A trick is used to generate the temporary names needed to avoid specifying the order in which the values are evaluated. This could also be accomplished by using an auxiliary macro.


(define-syntax letrec
   (syntax-rules ()
     ((letrec ((var1 init1) ...) body ...)
      (letrec "generate_temp_names"
        (var1 ...)
        ()
        ((var1 init1) ...)
        body ...))
     ((letrec "generate_temp_names"
        ()
        (temp1 ...)
        ((var1 init1) ...)
        body ...)
      (let ((var1 <undefined>) ...)
        (let ((temp1 init1) ...)
          (set! var1 temp1)
          ...
          body ...)))
     ((letrec "generate_temp_names"
        (x y ...)
        (temp ...)
        ((var1 init1) ...)
        body ...)
      (letrec "generate_temp_names"
        (y ...)
        (newtemp temp ...)
        ((var1 init1) ...)
        body ...))))



(define-syntax begin
   (syntax-rules ()
     ((begin exp ...)
      ((lambda () exp ...)))))


 The following alternative expansion for begin does not make use of the ability to write more than one expression in the body of a lambda expression. In any case, note that these rules apply only if the body of the begin contains no definitions.


(define-syntax begin
   (syntax-rules ()
     ((begin exp)
      exp)
     ((begin exp1 exp2 ...)
      (let ((x exp1))
        (begin exp2 ...)))))


 The following definition of do uses a trick to expand the variable clauses. As with letrec above, an auxiliary macro would also work. The expression (if #f #f) is used to obtain an unspecific value.


(define-syntax do
   (syntax-rules ()
     ((do ((var init step ...) ...)
          (test expr ...)
          command ...)
      (letrec
        ((loop
          (lambda (var ...)
            (if test
                (begin
                  (if #f #f)
                  expr ...)
                (begin
                  command
                  ...
                  (loop (do "step" var step ...)
                        ...))))))
        (loop init ...)))
     ((do "step" x)
      x)
     ((do "step" x y)
      y)))
```
