---
layout: post
title: "scheme IO实现来自于CPS或者haskell的monod范畴思想"
date: 2016-02-17 10:58:46 +0800
comments: true
categories: scheme
---

scheme中IO的实现类似于CPS在原来函数的基础上增加了一个控制变量。
<!--more-->

之前曾经写过[CPS的控制过程][1]

## original style
``` scheme
(define rember8
  (lambda (ls)
    (cond
      [(null? ls) '()]
      [(= (car ls) 8) (cdr ls)]
      [else (cons (car ls) (rember8 (cdr ls)))])))

```

## cps style
``` scheme

;1
(define rember8
  (lambda (ls k)
    (cond
      [(null? ls) (k '())]                           ==> k
      [(= (car ls) 8) (cdr ls)]
      [else (cons (car ls) (rember8 (cdr ls)))])))

;2
(define rember8
  (lambda (ls k)
    (cond
      [(null? ls) (k '())]
      [(= (car ls) 8) (k (cdr ls))]                  ==>k
      [else (rember8 (cdr ls) (lambda (x) (cons (car ls) x)))]))) ==> rember8

;3
(define rember8
  (lambda (ls k)
    (cond
      [(null? ls) (k '())]
      [(= (car ls) 8) (k (cdr ls))]
      [else (rember8 (cdr ls) (lambda (x) (k (cons (car ls) x)))]))) ==>  k 
```

这个k的作用将在IO中体现出来。
``` scheme

(define o (open-output-file "greeting.txt"))
(display "hello" o)
(write-char #\space o)
(display 'world o)
(newline o)
(close-output-port o)


```

    结果：
![result][2]


## 其他IO语法

+ open-input-file

``` scheme

;;打开文件句柄 用于读操作
(define in10 (open-input-file "greeting.txt"))
;;利用read-char 读取单个字符char
(read-char in10)
(read-char in10)
(read-char in10)
;;利用read 读取一个字word
(read in10)
(read in10)
;;利用read-line读取一行字符
(read-line in10)
(read-line in10)
(read in10)

;;结果
    #\h             ==>read-char
    #\e             ==>read-char
    #\l             ==>read-char
    'lo             ==>read
    'world          ==>read
    ""              ==>read-line
    "made in DaXi"  ==>read-line
    #<eof>          ==>read
```
+ open-input-string

``` scheme
 (define i (open-input-string "hello world"))
 (read-char i)
    #\h       ==>read-char
 (read-char i)
    #\e       ==>read-char
 (read-char i)
    #\l       ==>read-char
 (read i)
    'lo       ==>read
 (read i)
    'world    ==>read
 (read i)
    #<eof>    ==>read
```

+ open-output-string

``` scheme

;;;定义字符串变量
(define op (open-output-string))
(write 'hello op)
(write-char #\, op)
(display " " op)
(display "world" op)

;;获取存取的值
(get-output-string op)


;;结果
    "hello,world"    ==> get-output-string
```


其中o的作用其实就是类似于k的作用，都是体现者一种程序的控制一种续延（具体未讲的十分清楚，待以后补充load等）。

[1]:http://jueqingsizhe66.github.io/blog/2016/02/09/its-a-dead-program-dot-how-to-let-it-alive/ 
[2]: /images/greeting.png
