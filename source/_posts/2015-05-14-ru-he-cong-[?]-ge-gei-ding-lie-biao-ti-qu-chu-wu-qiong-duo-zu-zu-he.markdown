---
layout: post
title: "如何从一个给定列表提取出无穷多组组合"
date: 2015-05-14 16:42:39 +0800
comments: true
categories: scheme
---

a + b + c =30
从一个列表中(1,3,5,7,11,13,15) 中提取三个元素，使其满足上式，该如何做？
第二个问题，如果我想从这7个元素中提取出来2个元素任意组合,3个,4个
我又该如何做？ 


如果我假定能够获得我想要的这些表，那么我完全可以把他们一一运用于
(map '+ all-table)中，一一计算出来

当我提取出来两个元素的时候，我是不是可以使用
(get-2 all-table all-table)
(get-3 all-table all-table all-table)

<!--more-->
## scheme and lisp的一些[区别点](http://www.oschina.net/question/563463_122233?sort=time)

1. 在Common Lisp眼中,一个符号的symbol-value和symbol-function是不一样的,而Scheme对两者不作区分。在Scheme里面，变量只有唯一对应的值，它可以是个函数，也可以是另一种对象。因此，在Scheme中就不需要#’或者funcall了。Common Lisp的：


        (let ((f #’(lambda (x) (1+ x))))
        (funcall f 2))

在Scheme中将变成：

        (let ((f (lambda (x) (1+ x))))
        (f 2))



##记住在Drracket Help中Pairs and Lists有更多的关于列表的操作


下面是我学了一天的列表操作(P27 P28未实现到scheme版本，主要问题:

1. remove-duplicate：去除一个lat中重复的元素
2. reduce  把一个最小包含()的列表 比如((a b) (c d)) 会变为(a b c d) ((a (b)) (c d)) ==>(a (b) c d)
3. sort: 排序，暂时没时间


##P01 最后一个元素
``` scheme

 '(1 3 5 7 9 11 13 15)
; (if (= (+ a b c) 30) '(a b c)
 ;   (define (first a)
      
     ;;Find the last box of a list. (查找列表最后一个元素) 
 (define (my-last lst)
   (if (null? (cdr lst))  ;;;(null? (cdr lst))
      lst
      (my-last (cdr lst))))
 
 (my-last '(13  5 6 7 8 33))
 ;'(33)
```
##P02  Find the last but one box of a list.(除最后一个元素外的列表)
``` scheme
 (define (my-but-last-1 lst)
   (if (null? (cddr lst)) ;;(null? (cddr  lst))
      lst
      (my-but-last-1 (cdr lst))))
 ;'(c d)
 
```
 
##P03  Find the K'th element of a list. (查找列表的第K个元素)

``` scheme
 (define (element-at lst n)
   (cond 
     ((null? lst) '())
     ((= n 1) (car lst))
     (else (element-at (cdr lst) (- n 1)))))
 
 (element-at '(a b c d e) 3)
```
##P04  Find the number of elements of a list.

``` scheme
 (define (length-lst lst)
   (if (null? lst)
      0
      (+ 1 (length-lst (cdr lst)))))
 
 (length-lst '(a b  c d e))
``` 
 
##P05  Reverse a list.
 
``` scheme
 (define (reverse-lst lst)
   (define (rec lst acc)
     (if (null? lst)
        acc
        (rec (cdr lst) (cons (car lst) acc))))
   (rec lst '()))
 
 (reverse-lst '(a b c d e))
``` 
 
##P06  Find out whether a list is a palindrome. 
 链表是否中心对称,将链表逆转,然后查看链表是否一样

``` scheme
 (define (palindrome lst)
   (let ((lst1 (reverse-lst lst)))
     (equal? lst lst1)))
 
 (palindrome '(a b c b a))
  (palindrome '(a a b c b a ))

```

##P07  Flatten a nested list structure.
Transform a list, possibly holding lists as elements into
a flat list by replacing each list with its elements (recursively).
  
``` scheme

(define (ncons lst lst1)
(cond
  ;;((null? lst)  '())
  ((null? (cdr lst)) (car lst))
  (else (cons (ncons (cdr lst) lst1) lst1))))
(ncons '(a b c d) '(c d))
(ncons '(a b  e c d) '(c d))
(ncons '(a b  e f c d) '(c d))
;;'(((d c d) c d) c d)

;;;如何提取公共部分 (cons x 
;;;> (cons 'a (cons 'b '(c d e)))
;;;'(a b c d e)
;;;> (cons 'a (cons 'b (cons 'c '(d e f))))
;;;'(a b c d e f);

;;;;后来我想到了 我们平时用的(null? lst) '())  
;;;;; 那么现在我们公共的模式其实是跟平时一样的
;;;;;(cons 'a (cons 'b (cons 'c '()))) 只不过是最里面的‘()
;;;;;替换成了 '(c d e)  或者lst1即可! 因为他只用一次,
;;;;; 把他放在最里面即可

;;;;; 当时我还想到一个思路
;;;1 把lst1 变幻方向 reverse-list
;;;2 然后利用(cons (car lst) (reverse-list lst1)
;;;;;但是问题出现,你的natural recusrion?无法体现!!
(define (ncons-true lst lst1)
(cond
((null? lst) lst1) 
(else (cons (car lst) (ncons-true (cdr lst) lst1)))))
(ncons-true '(a b c) '(d e f))

;;说明ncons-true 就是clisp的nconc的scheme写法
;;;有了ncons-true 这个类似之前的(sum term a next b)的工具
;;; 我们就可以办很多事情
;;据此可以改写reverse-list
(define (reverse-lst-2 lst)
(if (null? lst)
   '()
   (ncons-true (reverse-lst-2 (cdr lst)) (list (car lst)))))
(reverse-lst-2 '(a b c d))

```

Try Again you.

## P08  Flatten a nested list structure.

  Transform a list, possibly holding lists as elements into a flat
  list by replacing each list with its elements (recursively)

  (define (atom? x)
    (and (not (null?  x)) (not (pair?  x))))

得分为三种情况
1. null?
2. atom?
3. list?
```
```scheme

  (define (my-flatten tree)
    (cond 
      ((null? tree) '())
      ((atom? tree) (list tree))
      (else (ncons-true (my-flatten (car tree))
                                     (my-flatten (cdr tree))))))
   (my-flatten '(a (b (c d) e)))

```


##P09  Eliminate consecutive duplicates of list elements.

If a list contains repeated elements they should be replaced 
with a single copy of the element. 
The order of the elements should not be changed.
   
首先将相邻相同的元素通过collect组织到一起,然后对相同的元素处理,通过combine将结果返回

```scheme
   ; (compress '(a a a a b c c a a d e e e e))
;;;;;(A B C A D E)
   ;;1 compress module
   (define (compress lst)
         (define (rec lst acc prev)
           (cond ((null? lst) acc)
                ((equal? prev (car lst)) (rec (cdr lst) acc prev));;如果相等则剔除掉
                (else (rec (cdr lst) (cons (car lst) acc) (car lst)))))
         (if (null? lst)
            lst
            (reverse-lst-2 (rec lst '() '()))))
   ;; (compress '(a a  b b c  c))
;;;;'(a b c)
   ;;> (compress '(a a  b b c  c d d a a ))
;;'(a b c d a)
   ;;;还不错 达到目的了 !但是下面提出了更一般的表达方式
   ;;> (compress '(a a a b b))
;;;'(a b)
;;> (compress '((a a a) (b b))) ;;;;;;;;;;;!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!Compress的一个缺陷!
;;;'((a a a) (b b))   ;;;;补救方法是让他遍历 括号里面的内容 ,(atom? (car l))  (原先的操作) (else (cons (compress (car lst)) (compress (cdr lst))))
   ;;;collect目的就是对相同的元素进行分组
      (define (compress-* lst)
        (define (atom? x)
          (and (not (null? x)) (not (pair? x))))
         (define (rec lst acc prev)
           (cond ((null? lst) acc)
                ((atom? (car lst))
                 (cond
                   ((equal? prev (car lst)) (rec (cdr lst) acc prev));;如果相等则剔除掉
                   (else (rec (cdr lst) (cons (car lst) acc) (car lst)))))
                (else (cons (compress-* (car lst)) (compress-* (cdr lst))))))
         (if (null? lst)
            lst
          ;;  (reverse-lst-2 (rec lst '() '()))))
       (rec lst '() '())))
   ;;;> (compress-* '((a a a) (b b)))
;;;;'((b) (a))
      
      ;;;改进去掉了 reverse-lst-2
      ;;; (compress-* '((a a a) (b b)))
;;;'((a) (b))
   (define (collect lst)
     (define (rec lst temp acc)
       (cond 
         ((null? lst) (cons temp acc))
         ((equal? (car lst) (car temp)) ;;如果相邻两个元素是同一个
          (rec (cdr lst) (cons (car lst) temp) acc)) ;;则把他存入temp中,并进行下一次
         (else (rec (cdr lst) (list (car lst)) (cons temp acc)))));;; 如果不是,则新开一个list (list (car lst))  并把之前的temp加入到acc中!
     (if (null? lst)
        '()
        (reverse-lst-2 (rec (cdr lst) (list (car lst)) '()))))
   
   ;;;测试collect过程
   (collect '(a a b b b c c c a a a ))
   ;;;'((a a) (b b b) (c c c) (a a a))  结果很漂亮
   ;;(collect '((a a b b c c) b b b c c c a a a ))
;;;'(((a a b b c c)) (b b b) (c c c) (a a a))  ;;;----------------------->Collect的一个缺点
     (define (collect* lst)
        (define (atom? x)
          (and (not (null? x)) (not (pair? x))))
       (define (rec lst temp acc)
         (cond 
           ((null? lst) (cons temp acc))
           ((atom? (car lst))
            (cond
              ((equal? (car lst) (car temp)) ;;如果相邻两个元素是同一个
               (rec (cdr lst) (cons (car lst) temp) acc)) ;;则把他存入temp中,并进行下一次
             ;; (else (rec (cdr lst) (list (car lst)) (cons temp acc)))));;; 如果不是,则新开一个list (list (car lst))  并把之前的temp加入到acc中!
              (else (rec (cdr lst)  (list (car lst)) (cons temp acc)))))
         ;;  (else (cons (collect* (car lst) (cdr lst)))))) ;;;--------------->导致有问题
            (else (cons (collect* (car lst)) (collect* (cdr lst))))))
       (if (null? lst)
          '()
         ;; (reverse-lst-2 (rec (cdr lst) (list (car lst)) '()))))
          (reverse-lst-2 (rec (cdr lst) (list (car lst)) '()))))
     ;;;(collect*  '((a a a  b b b (c c  d d)) e e  f f))
;;;;'(((a a a b b b (c c d d))) (e e) (f f))
     
     ;;;;暂时有点问题!!
     ;;;(collect* '((a a a b b b) (c c d d) (e e) (f f)))
;;;'(((f f)) ((c c) (d d)))
     
  ;;;;这是类似于 (sum term a next b)级别的高阶函数
  (define (general-process combine fn lst)
          (define (rec lst acc)
            (if (null? lst)
               acc
               (rec (cdr lst) (combine (fn (car lst)) ;;;只是获取第一个元素,然后放入acc中,最后返回acc
                                      acc))))
          (reverse-lst-2 (rec (collect lst) '())))
  
  (define (compress-1 lst)
    (general-process cons (lambda (sublst) (car sublst)) lst))
   
  (compress-1 '(a a a b b b c c c  a a a b b b))
  ;;;'(a b c a b)
``` 
  
##P10  Pack consecutive duplicates of list elements into sublists.

``` scheme
  ;;;(collect '(a a b b c c  d d  e e f e f))  ;;用collect也可以实现
;;;'((a a) (b b) (c c) (d d) (e e) (f) (e) (f))
  
  
 ;;同样的我们也可以使用general-process实现一下

  (define (pack lst)
    (define (identity x) x)
    (general-process cons identity lst))
  
  (pack '(a a  b b  c c  a a))
  ;;'((a a) (b b) (c c) (a a))  为什么可以这样漂亮  其实主要是因为collect的作用!!collect本身已经帮你分好了
              ;;;所以感觉这个general-process 函数有点贼!!
  ;;;那么下面就来体现general-process的与众不同之处把!
```  
##P11  Run-length encoding of a list.

``` scheme
  ;;;Use the result of problem P09 to implement the so-called run-length
  ;;;encoding data compression method. Consecutive duplicates of elements are 
  ;;;encoded as lists (N E) where N is the number of duplicates of the element E.
  
  ;;Example:
;;;(encode '(a a a a b c c a a d e e e e))
  
  (define (encode lst)
    (general-process cons (lambda (sub) (list (length-lst sub) (car sub))) lst))
  
  (encode '(a a  a  b b b b  p c c   d d  a a a  e f))
```  
  
##P12  Modified run-length encoding.
  ;;;;Modify the result of problem P10 in such a way that if an element has no duplicates it is 
  ;;;simply copied into the result list. Only elements with duplicates are transferred as (N E) lists.

``` scheme
    (define (encode-upgrade lst)
    (general-process cons (lambda (sub) 
                                         (if (= (length-lst sub) 1) ;;;; 我们做了额外的操作,如果他们只是1的话,就保持原样输出
                                            (car sub)
                                            (list (length-lst sub) (car sub)))) lst)) 
  
  (encode-upgrade '(a a  a  b b b b  p c c   d d  a a a  e f))
```  
  
  ;;;;然而这时候我们  又需要尝试反着去处理那些数据
##P13  Decode a run-length encoded list.
  ;;;Given a run-length code list generated as specified in problem P11. Construct its uncompressed version
  
  
###这是一个不好的定义,对于宏这一大块 暂时有点模糊

``` scheme
  (define loop
    (lambda (x y fn lst)
      (if (<=  x y)
         (begin (fn (cadr lst)) (set! x (+ x 1))
               (loop x y)) '())))
 ;;(loop 1 (car '(3 a)) cons '(3 a 4 b))
  ;Perhaps the best you can do, at least with syntax-rules, 
;  is to use the macro to capture only the expressions to run, 
  ;and use non-macro code to deal with the looping:
  ;;(define-syntax-rule (id . pattern) template)
;Equivalent to
;(define-syntax id
 ; (syntax-rules ()
  ; [(id . pattern) template]))
  ;(define-syntax rotate
  ;(syntax-rules ()
   ; [(rotate a b) (swap a b)]   ;;模板1
   ; [(rotate a b c) (begin         ;;;模板2
    ;                 (swap a b)
     ;                (swap b c))]))
  
  ;;;When a pattern variable like c is followed by ... 
  ;;in a pattern, then it must be followed by ... in a template, too. 
  
  (define-syntax forloop
  (syntax-rules ()
    ((forloop start stop exps ...)
     (let ((j stop))
       (let loop ((i start))
         (when (<= i j)
           exps ...
           (loop (+ i 1))))))))
  (forloop 6 8 (display 'g))
  ;;;;> (forloop 6 8 (display 'g))
;;;;ggg
  
  ;;;Macros are expanded at compile time,(也就是说编译时候负责的是语法数的建立
 ;; 和展开,而在运行阶段runnable阶段才是求值阶段)
  ;;thus something like (begin exps ... (forloop (+ start 1) stop exps ...)) 
  ;;will expand forloop again and again, regardless of what the value 
  ;;of (+ start 1) is (which is evaluated at runtime).
  ;;;说得太对了!! 必须capture你所要重复的步骤,但不要加入foorloop
  ;;;因为他会不断的展开,直到内存泄漏
  ;;;就比如上面的forloop的不抓片段就是
  ;; (let ((j stop))
  ;;     (let loop ((i start))
   ;;      (when (<= i j)
   ;;        exps ...
   ;;        (loop (+ i 1))))))))  ;;;必须保证没有包含 forloop
  
  
  
 ;;;You can also use a do loop:
  
  (define-syntax forloop1
  (syntax-rules ()
    ((forloop start stop exps ...)
     (let ((j stop))
       (do ((i start (+ i 1)))
           ((> i j))
         exps ...)))))
  (forloop1 6 8 (display 'g))

```

``` scheme
;;;现在返回到我们的整体 decode the encode list ----这边暂时留下一个疑问!
;;;不知道如何实现http://blog.csdn.net/yuan_da_xing/article/details/8643869
;;;P12的problem
;;参考阅读http://blog.csdn.net/xiaojianpitt/article/details/7878429
(define (decode lst)
(reverse-lst-2 (general-process ncons-true 
                   (lambda (elem)
                       (let ((e (car elem)))
                         (if (atom? e) 
                            (list e)
                            ;;(forloop 1 (car e) (collect (cadr e))))))
                            (forloop 1 (car e) (display 'e)))))
                lst)))
;;;http://blog.csdn.net/xiaojianpitt/article/details/7878429
;;重要定义了一个forloop的方法
;;http://stackoverflow.com/questions/20810801/defining-a-for-loop-in-scheme# 
``` 
##P14  Run-length encoding of a list (direct solution).
  Implement the so-called run-length encoding data compression
  method directly. I.e. don't explicitly create the sublists
  containing the duplicates, as in problem P09, but only 
  count them. As in problem P11, 
  simplify the result list by replacing the singleton lists (1 X) by X.

``` scheme
  (define (encode-direct lst)
    (define (rec lst cur count acc)
      (cond ((null? lst) (reverse-lst-2 (cons (list count cur)
                                             acc)))
           ((equal? cur (car lst)) 
            (rec (cdr lst) cur (+  1 count) acc))
           (else (rec (cdr lst) (car lst) 1
                   (cons (list count cur) acc))))) ;;; 目的是分好组 map的任务就是跳出
    (map (lambda (e) (if (= (car e) 1) (cadr e) e))
        (if (null? lst)
           '()
           (rec (cdr lst) (car lst) 1 '()))))
(encode-direct '(a a a a b c c a a d e e e e))
```

##P15  Duplicate the elements of a list.

``` scheme
;; ((lambda (x) (forloop 1 4 (begin (print   x) ))) '(a b c))
;;'(a b c)'(a b c)'(a b c)'(a b c)
(define (fp lst)
  (cond
    ((null? lst) '())
    ;(else (cons (fp (cdr lst)) ((lambda (x) (forloop 1 4  (collect (list x)))) (car lst))))))
    (else (cons (fp (cdr lst)) ((lambda (x) (forloop 1 4  (print  x))) (car lst))))))
(fp '(a b c))

(define nums '(1 2 3 4 5 6 7 8))
(define (fnums num1)
  (lambda (num2)
    (expt num1 num2)))
(define ans (map (fnums 5) nums))
ans
;;(map (lambda (x) (expt 5 x)) '(1 2 3 4 5))
;;'(5 25 125 625 3125)

;;;;记住在Pairs and Lists有更多的关于列表的操作
(define (loop-until start end next f)
  (define (loop i)
    (cond
      ((=  i end) (cons (f i) '()))
      (else (cons (f i) (loop (next i))))))
  (loop start))
 ; (let loop ([i start])
 ;   (unless (done? i)
 ;     (if (< i end) (cons (f i)  (loop (next i)))
 ;        (cons (f i) '())) ;;;The last one! 
  ;       )))
(loop-until 1 10 (lambda (x) (+ x 1)) (lambda (x) (* x 2)))

  (define (decode1 lst)
    (define (next i) (+ i 1))
    (define (identity x) x)
   ; (define (pick i lst)
   ;   (if (= i 0) (car lst)
   ;      (pick (- i 1) (cdr lst))))
    (define (loop-until-local start end next f atom1)
      (define (loop i)
        (cond
         ;; ((=  i end) (cons (f (pick i lst)) '()))
           ((=  i end) (cons (f atom1) '()))
          (else (cons (f atom1) (loop (next i))))))
      (loop start))
  ;;;  (reverse-lst-2 (general-process ncons-true 
      (general-process ncons-true 
                       (lambda (elem)
                           (let ((e (car elem)))
                             (if (atom? e) 
                                (list e)
                                ;;(forloop 1 (car e) (collect (cadr e))))))
                                (loop-until-local 1 (car e) next identity (cadr e)))))
                    lst))
  ;;;> (decode1 '(4 a))
;;'(a 4)
;;> (decode1 '(a 4))
;;'(4 a)
;; (decode1 '((4 a) (2 e)))
;;'(a a a a e e)
  
  (define (general-rep lst n)
     (define (next i) (+ 1 i))
     (define (identity x) x)
     (define (loop-until-local start end next f atom1)
      (define (loop i)
        (cond
         ;; ((=  i end) (cons (f (pick i lst)) '()))
           ((=  i end) (cons (f atom1) '()))
          (else (cons (f atom1) (loop (next i))))))
      (loop start))
    ;;(map (lambda (x) (loop-until-local 1 n next identity x)) lst))
   ;; (compress (map (lambda (x) (loop-until-local 1 n next identity x)) lst))) ;;;想着让结果 只包含一个括号
     (my-flatten (map (lambda (x) (loop-until-local 1 n next identity x)) lst))) 

```
##P15 Duplicate the elements of a list

``` scheme
  (define (dupli lst)
    (general-rep lst 2))
  (dupli '(a b c))
```
##P16 Replicae the elements of a list a given number of times

``` scheme
  (define (repli lst n)
    (general-rep lst n))
  
  (repli '(a b c d) 4)
  
```  
##P17 Drop every N'th element from a list

``` scheme
  (define (drop lst n)
    (define (rec lst i)
              (cond ((null? lst) '())
                   ((= i n) (rec (cdr lst) 1))
                   (else (cons (car lst) (rec (cdr lst) (+ i 1))))))
    (rec lst 1))
  (drop '( a b c d e  ) 3)
``` 

``` scheme
  (define (pick n lst)
    (if (= n 0) (car lst)
       (pick (- n 1)  (cdr lst))))
  (pick 3  '(a b c d e))
```  
  
##P18 Split a list into two parts ;the length of the first part is given
  
 ;;;1  共有模式选取一个表 从0选到n即可
  ;;2  提供一个表  利用nthcdr   该过程接受两个参数 第一个参数表示开始位置 第二个参数表示总表 ,截取到表的截止位置
  ;;> (nthcdr 3 '(a b c d e))  --->测试成功
;;;'(d e)

``` scheme
      (define (nthcdr n lst)
      (cond
        ;((null? lst) '())
        ;((> n (length-lst lst)) '())
        ((null? lst) '())
        ((> n (length-lst lst)) '())
        ((= n 0) lst) ;; bug修复; 因为第一个就是n=0只不过一直没有
        ;((= n 1)  (cdr lst))
        (else (nthcdr (- n 1) (cdr lst)))))
  (define (copy-from lst start end)
    (define (copy lst len acc)
      (if (= len 0)
         (reverse acc)
         (copy (cdr lst) (- len 1) (cons (car lst) acc))))
    (copy (nthcdr start lst) (- end start) '()))
  
  (define (split lst cut-point)
    (list (copy-from lst 0 cut-point) (copy-from lst cut-point (length-lst lst))))
  (split  '(a b c d e f) 3)
        
    (split  '(a b c d e f) 2)
```    
    
##P19  Extract a slice from a list given tow indice,I and K ,the slice is the list containing the elements
    ;; between the I'th and K'th element of the original list (both limits incluede);;类似于subseq

``` scheme
    (define (slice lst start end)
      (copy-from lst (- start 1) end))
    
    (slice '(a b c d e f  g) 2 4)
```    
##P20  rotate a list N places to the left
    
cut the table into two parts,then change the position
    
``` scheme
   (define (rotate lst n)
     (let ((len (length-lst lst)))
       (if (> n 0)
          (ncons-true (copy-from lst n len) (copy-from lst 0 n)) ;;正方向
          (ncons-true (copy-from lst (+ len n) len) (copy-from lst 0 (+ len n)))))) ;;;反方向
   
   (rotate '(a b c d e  f) 2)
   (rotate '(a b c d e f) -2)
```   
##P21 Remove the Kth element from a list

``` scheme
   (define (remove-at lst n)
     (if (= n 1)
        (cdr lst)
        (cons (car lst) (remove-at (cdr lst) (- n 1)))))
   
   (remove-at '(a b  c d e f) 5)
   
```   
##P22 Insert an element at a given position into a list

``` scheme
   (define (insert-at elem pos lst)
     (if (= pos 1)
        (cons  elem lst)
        (cons (car lst) (insert-at elem (- pos 1) (cdr lst)))))
   
   (insert-at 'abc  2 '(1 2 3 4))
   
```   
##P23 Create a list containing all integers within a given range
   
``` scheme
   (define (range a b)
     (define (next i) (+ i 1))
     (define (f i) i)
     (define (loop-until-local start end next f )
      (define (loop i)
        (cond
         ;; ((=  i end) (cons (f (pick i lst)) '()))
           ((=  i end) (cons (f i) '()))
          (else (cons (f i) (loop (next i))))))
      (loop start))
     (loop-until-local a b next f))
   
   (range 1 10)
   (range 4 40)
```   
   
   
##P24 Extract a given number of randomly selected elements from a list
   随机抽取的元素放在list当中,洗牌算法

``` scheme
   (define (rnd-select lst n)
     (if (= n 0)
        '()
        (let ((r (random (length-lst lst))))
          (cons (pick r lst) (rnd-select (remove-at lst (+ 1 r)) (-  n 1))))))
   
   (rnd-select '(a b c d e f g h i j k) 3)
   
```   
##P25 Lotto:Draw  Ndifferent random numbers from the set 1...M
   组合P22 P23

``` scheme
   (define (lotto-select n m)
     (rnd-select (range 1 m) n))
   
   (lotto-select 6 49)
   ;;'(12 10 5 39 37 1)   每次运行都是不一样的
```   
   
##P26 使用random求解器  和rnd-select来让列表乱续

``` scheme
   (define (rnd-permu lst)
     (rnd-select lst (length-lst lst)))
   (rnd-permu '(a b c d e f ))
   
   ;;'(e d f b c a)
```   
   
##P27 Generate the combinations of K distinct objects chosen from the N elements of a list?
   in how many ways can a committee of 3 be chosen from a group of 12 people? 
   数学力的排列组合C(12,3)=220种可能。
   
1. 选择第0个元素,在剩下的链表中选取n-1个元素
2. 选择第1个元素,在剩下的链表中选取n-1个元素。。依次递归
3. n=1 选取当前链表的第一个元素

``` scheme
   (define (combine-1 lst n)
                     (cond ((or (< (length-lst lst) n) (zero? n)) '())
                          ((= n 1)
                           (map (lambda (x) (list x)) lst))
                           (else (ncons-true (map (lambda (elem) (cons (car lst) elem))
                                            (combine-1 (cdr lst) (- n 1)))
                                            (combine-1 (cdr lst) n)))))
   (define (combination n lst)
     (combine-1 lst n))
 ;  (combination 3 '(1 3 5 7 9 11 13 15))
   
   ;;> (apply + '(3 6 22))
;;31
   ;;;终于解决了 我想要的,但是核心的combine-1还是得再去重新理解
   (define (select30 lst)
    (if  (= (apply + lst) 30) (list lst 'correct) (list lst 'false)))
   (map select30  (combination 3 '(1 3 5 7 9 11 13 15)))
```   
   
##P28 Group the elements of a set into disjoint subsets
+ In how many ways can a group of 9 people work in 3 disjoint subgroups of 2,3 and 4 persons? write a function that generates all the possibilities and returns them in a list.

+ Example 
    (group3 '(aldo beat carla david evi flip gary hugo ida))
    ( ( (ALDO BEAT) (CARLA DAVID EVI) (FLIP GARY HUGO IDA) )... )
   
+ Generalize the above predicate in a way that we can specify a list of group sizes and the predicate will return a list of groups.
   
   Example:
    (group '(aldo beat carla david evi flip gary hugo ida) '(2 2 5))  
    (((ALDO BEAT) (CARLA DAVID) (EVI FLIP GARY HUGO IDA) )... )
   
   *Note that we do not want permutations of the group members;* i.e. ((ALDO BEAT) ...) is the same solution as ((BEAT ALDO) ...).
   However, we make a difference between ((ALDO BEAT) (CARLA DAVID) ...) and ((CARLA DAVID) (ALDO BEAT) ...).

   You may find more about this combinatorial problem in a good book on discrete mathematics under the term "multinomial coefficients".
   set-difference A B  找出A中元素不在B中的
  lstA lstB 都是一个lat

``` scheme
   (define (member? a lat)
     (cond
       ((null? lat) #f)
       ((equal? a (car lat)) #t)
       (else (member? a (cdr lat)))))
   (define (set-difference lstA lstB)
     (cond
       ((null? lstA) '())
       ((null? lstB) lstA)
       ((member? (car lstA) lstB)  (set-difference (cdr lstA) lstB))
       (else (cons (car lstA) (set-difference (cdr lstA) lstB)))))
   
   ;;正确实现了clisp的set-difference
   (set-difference '(a b c d e) '(a  d e g))
 (set-difference '(a (b c) d e) '(a c d e g))
  (set-difference '(a bc ) '())
  
  ;;Break 7 [8]> (reduce #'append '((a b) (c d)))
;;(A B C D)
;Break 7 [8]> (reduce #'append '((a b c) (c d)))
;(A B C C D)
;Break 7 [8]> (reduce #'append '((a b c) (c d) (e f))
;)
;(A B C C D E F)
;Break 7 [8]> (reduce #'append '((a b c) (c d) (e f) ((a c) (e f))))
;(A B C C D E F (A C) (E F))
;;;从这边可以看到reduce的作用就是开出一个括号来,其实相当于减1的效果
  ;;;也就是原先一个括号的没有括号  原先两个括号的变成一个括号 ,原先是原子的则报错!必须具有一个括号!

  然而scheme提供了fold-left  fold-right两个相似的过程  R6RS中才有!!

```

# 留下了很大的遗憾，P27 P28没有实现，贴上clisp版本

``` scheme
(defun lenght-lst (lst) 
 (if (null lst) 
 0 
 (1+ (lenght-lst (cdr lst)))))


(defun flatten-2 (lst) 
 (remove-duplicates (reduce #'append lst))) 
(defun get-n-elements-from (lst used n) 
 (let* ((f1 (flatten-2 used)) 
 (left (set-difference lst f1))) 
 (labels ((rec (lst n) 
 (cond ((or (null lst) 
 (< (lenght-lst lst) n)) 
 nil) 
 ((= n 1) (mapcar #'(lambda (x) 
 (list x)) 
 lst)) 
 (t (append 
 (rec (cdr lst) n) 
 (mapcar #'(lambda (x) 
 (cons (car lst) x)) 
 (rec (cdr lst) (1- n)))))))) 
 (rec left n)))) 
(defun group-list (lst ln) 
 (let ((init (mapcar #'(lambda (e) (list e)) 
 (get-n-elements-from lst nil (car ln))))) 
 (labels ((rec (ln acc) 
 (let ((len (lenght-lst ln))) 
 (cond ((= len 1) 
 (mapcar #'(lambda (e) 
 (nconc 
 (get-n-elements-from lst 
 e (car ln)) 
 e)) 
 acc)) 
 (t (rec (cdr ln) 
 (mapcan #'(lambda (e) 
 (let ((tlst 
 (get-n-elements-from 
 lst 
 e 
 (car ln)))) 
 (mapcar #'(lambda (new) 
 (cons new e)) 
 tlst))) 
 acc))))))) 
 (rec (cdr ln) init)))) 

(defun group (lst lstn) 
 (mapcar #'(lambda (e) 
 (reverse e)) 
 (group-list lst lstn))) 

 (defun group3 (lst) 
 (group lst '(2 3 4)))

```
