---
layout: post
title: "shuffle cards"
date: 2016-02-14 14:37:43 +0800
comments: true
categories: scheme
---

这个程序是参考[洗牌][1]而来，主要定位于scheme的算法运用实现。
<!--more-->


源代码如下：
``` scheme
#lang racket
;四种花色分别为黑桃(spade)、红心(heart)、梅花(club)、方块(diamond)
 
(define suits '("王" "黑桃" "红桃" "梅花" "方块"))
(define faces '("B" "L" "A" "2" "3" "4" "5" "6" "7" "8" "9" "10" "J" "Q" "K"))
 
;生成一张牌
(define (creat-card suit face)
    (cons suit face)
)
;生成n副牌
(define (creat-cards n mysuits myfaces)
        (define (afaces suit myfaces)
                (if (null? myfaces)
                    myfaces
                    (append (list (creat-card suit (car myfaces))) 
                             (afaces suit (cdr myfaces))
                     
                    )
                )
        )
        (define (asuits suits myfaces)
                (if (null? suits)
                    suits
                    (append  (afaces (car suits) myfaces)
                             (asuits (cdr suits) myfaces)
                    )
                 
                )
         
        )
         
        ;(make-list n (asuits mysuits myfaces))
        (define (n-cards n alist return) 
                (if (= n 0)
                    return
                    (n-cards (- n 1) alist (append return alist))
                )
        )
        ;(n-cards 4 '(a b c d e) '())
        (n-cards n (asuits mysuits myfaces) '())
)
;显示牌 
(define (print-cards alist . number)
        (define (iter alist n num)
                (if (= 0 (remainder n num))
                    (newline)
                    'ok
                )
                (if (null? alist)
                    (newline)
                    (begin (display (car alist)) 
                           (display "  ")
                           (iter (cdr alist) (+ n 1) num)
                    )
                )
        )
        (if (null? number)
            (iter alist 0 5)
            (iter alist 0 (car number))
        )
)
;获取牌的花色
(define (get-suit card)
    ((lambda (x) (cond ((equal? x "黑桃") 4)
                       ((equal? x "红桃") 3)
                       ((equal? x "梅花") 2)
                       ((equal? x "方块") 1)
                       ((equal? x "王")   0)
                 )
     )
     
     (car card)
    )
)
 
;获取牌的数值
(define (get-face card)
    ((lambda (x) (cond ((equal? x "A") 1)
                       ((equal? x "2") 2)
                       ((equal? x "3") 3)
                       ((equal? x "4") 4)
                       ((equal? x "5") 5)
                       ((equal? x "6") 6)
                       ((equal? x "7") 7)
                       ((equal? x "8") 8)
                       ((equal? x "9") 9)
                       ((equal? x "10") 10)
                       ((equal? x "J") 11)
                       ((equal? x "Q") 12)
                       ((equal? x "K") 13)
                       ((equal? x "B") 30)
                       ((equal? x "L") 20)
                 )
     )
     
     (cdr card)
    )
)
;洗牌
(define (shuffle-card cards)
        (define (append-amb x y return)
                (cond ((and (null? x) (null? y)) return)
                      ((null? x) (append return y))
                      ((null? y) (append return x))
                      (else      (append-amb (cdr y) 
                                             (cdr x) 
                                             (append return (append (list (car x)) (list (car y))))
                                 )
                      )
                 
                )
             
        )
        ;--
        (define (shuffle scards)
                ((lambda (x) 
                        (append-amb (list-tail scards x) (list-head scards x) '()))
                 (random (length scards))
                  
                )
        )
        ;--
        (define (iter cards n)
                (if (= n 0)
                    cards
                    (iter (shuffle cards) (- n 1))
                )
        )
        ;--
        (iter cards (length cards))
        ;(shuffle cards)
)
;分牌(deal-card '(1 2 3 4 5 6 7 8) 4) => return ((1 5) (2 6) (3 7) (4 8))
;;还没完成
(define (deal-card alist n)
        (define (iter alist n return)
            (if (null? alist)
                return
                ;(append )
                'ok
            )
        )
        (iter alist n '())
             
)
;--------------------------------
;返回list中k以前的sublist
(define (list-head lst k)
    (if (zero? k)
        '()
        (append (list (car lst)) (list-head (cdr lst) (- k 1)) )
     
    )
)
;-------------test---------------
(define x (creat-card "梅花" "8"))
(define k (creat-cards 1 (cdr suits) (cdr (cdr faces))))
(define n '(1 2 3 4 5 6 7 8 9 10))
(print-cards k)
(print-cards (list-head k 9))
(print-cards (list-tail k 9) 13)
;(print-cards (shuffle-card k))
;------------------
;(display (shuffle-card n))
```

## 运行结果

    (黑桃 . A)  (黑桃 . 2)  (黑桃 . 3)  (黑桃 . 4)  (黑桃 . 5)  
    (黑桃 . 6)  (黑桃 . 7)  (黑桃 . 8)  (黑桃 . 9)  (黑桃 . 10)  
    (黑桃 . J)  (黑桃 . Q)  (黑桃 . K)  (红桃 . A)  (红桃 . 2)  
    (红桃 . 3)  (红桃 . 4)  (红桃 . 5)  (红桃 . 6)  (红桃 . 7)  
    (红桃 . 8)  (红桃 . 9)  (红桃 . 10)  (红桃 . J)  (红桃 . Q)  
    (红桃 . K)  (梅花 . A)  (梅花 . 2)  (梅花 . 3)  (梅花 . 4)  
    (梅花 . 5)  (梅花 . 6)  (梅花 . 7)  (梅花 . 8)  (梅花 . 9)  
    (梅花 . 10)  (梅花 . J)  (梅花 . Q)  (梅花 . K)  (方块 . A)  
    (方块 . 2)  (方块 . 3)  (方块 . 4)  (方块 . 5)  (方块 . 6)  
    (方块 . 7)  (方块 . 8)  (方块 . 9)  (方块 . 10)  (方块 . J)  
    (方块 . Q)  (方块 . K)  

    (黑桃 . A)  (黑桃 . 2)  (黑桃 . 3)  (黑桃 . 4)  (黑桃 . 5)  
    (黑桃 . 6)  (黑桃 . 7)  (黑桃 . 8)  (黑桃 . 9)  

    (黑桃 . 10)  (黑桃 . J)  (黑桃 . Q)  (黑桃 . K)  (红桃 . A)  (红桃 . 2)  (红桃 . 3)  (红桃 . 4)  (红桃 . 5)  (红桃 . 6)  (红桃 . 7)  (红桃 . 8)  (红桃 . 9)  
    (红桃 . 10)  (红桃 . J)  (红桃 . Q)  (红桃 . K)  (梅花 . A)  (梅花 . 2)  (梅花 . 3)  (梅花 . 4)  (梅花 . 5)  (梅花 . 6)  (梅花 . 7)  (梅花 . 8)  (梅花 . 9)  
    (梅花 . 10)  (梅花 . J)  (梅花 . Q)  (梅花 . K)  (方块 . A)  (方块 . 2)  (方块 . 3)  (方块 . 4)  (方块 . 5)  (方块 . 6)  (方块 . 7)  (方块 . 8)  (方块 . 9)  
    (方块 . 10)  (方块 . J)  (方块 . Q)  (方块 . K)  


[1]: http://www.oschina.net/code/snippet_2363209_47912
