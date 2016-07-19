---
layout: post
title: "通过Scheme读取文件"
date: 2016-02-22 22:54:21 +0800
comments: true
categories: scheme
---

1. 通过IO monad的open-input-file 读取文件
2. 通过read-line 读取接口文件
3. 通过name-loop命名loop来循环读取文件。

<!--more-->


+ 通过文件句柄读取文件，这边接口就是文件句柄
+ 现在还不清楚文件模式的几种类型
    - text
    - append
    - write（是否有）
+ eof-object?判断是否文件末尾
``` scheme
(define read-file
  (lambda (filename)
    (let ([port (open-input-file filename #:mode 'text)])
      (let loop ([line (read-line port)]
                 [all ""])
        (cond
         [(eof-object? line) all]
         [else
          (loop (read-line port)
                (string-append all line "\n"))])))))
```

从这边提取出一个最常用的let loop循环可以用于很多地方，重复几条语句分析所有程序
``` scheme

    (let loop ([start 0] [toks '()])
      (letv ([(tok newstart) (scan1 s start)])
        (cond
         [(eq? tok 'eof)
          (reverse toks)]
         [else
          (loop newstart (cons tok toks))])))))

```


另外，比如onstack? 用于判断是否存于栈
``` scheme


(define onstack?
  (lambda (u v stk)
    (let loop ([stk stk] [trace '()])
      (cond
       [(null? stk) #f]
       [(and (eq? u (car (car stk)))
             (eq? v (cdr (car stk))))
        (reverse (cons (car stk) trace))]
       [else
        (loop (cdr stk) (cons (car stk) trace))]))))

```
