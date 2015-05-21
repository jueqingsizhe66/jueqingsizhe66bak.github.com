---
layout: post
title: "The-Little-Scheme-And-Part-Of-TSS"
date: 2015-05-18 09:25:11 +0800
comments: true
categories: scheme
---


这部分内容是我练习TLS和TSS的全部学习资料，里面有详细的记录和一些心路历程，以后慢慢修改。
总行数5800左右。
<!--more-->
``` scheme
#lang racket

;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;;; I will introuduce The little scheme From the angles below
;;; 1:
;;;     why introuduce the concept
;;; 2:  
;;;     The main knowledge
;;; 3: 
;;;     Map-reduce 
;;;     .......
;;; 4:  How you describe the function works in  your own words? 
;;; 5:  How do you determine the output when you apply a function?Is it what you want?
;;;     no? why no?
;;;;Namely, Can you write down the definition of the function member? and its arguments
;;;     and refer to them as you go through the next group of questions????
;; Do not rush through this book! Read carefully
;;; From the introuduction chapter 
;;; else implicit said that if the condition is wrong do it recurly
;;;      also infer that after the recuring ,please go back to home along the Original road!
;;;  COns 's function: save the buffer to the stack
;;;  ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;;;;;;;;;;;;;;;;;;;;;;;The first part;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;;;
;;; The Basic operator
;;;   The primitive variables of scheme ::  car ,cdr(is pronounced as "could_er",cons,  null? zero?
;;;                                         eq?(compare two non-numericas atoms),
;;;                                         or (or asks two questions,one at a time. If the first one is true
;;;                                         .Otherwise it asks the second question and answers with whatever the 
;;;                                         second question answers)
;;;                                         numbers,add1,sub1
;;;  COns 's function: save the buffer to the stack
(define atom?
  (lambda (x)
    (and (not (pair? x)) (not (null? x)))))


;;;; New added after myracet1.rkt
;;;  I want to find ( a  h fd  fs)  but not ( a h (fd s) fs),so I want to create the lat?
;;;  can you describe what the function lat? does in your own words? *****************

;;;  lat? looks at each S-expression in a list ,in turn,and asks if each S-expression 
;;;  is an atom ,untill it runs out of S-expression. If it runs out without encountering 
;;;  a list,the value is #f. If it finds a list,the value is #f,false
;;;   What is the meaning of the question *else* ??  
;;;             *else* asks if else if true
;;;                is *else* true?  Yes,because the question else is always true!
;;;         so what is (else #f) means?  ----> as if else is true,If else is true--as it always is
;;; else implicit said that if the condition is wrong do it recurly
;;;      also infer that after the recuring ,please go back to home along the Original road!
;;;
;;;               --then the answer if #f ---false
;;;               In a word:   else is a question whose value is always (implicit)
(define lat?
  (lambda (l)
    (cond
      ((null? l) #t)
      ((atom? (car l)) (lat? (cdr l)))
      (else #f))))

;;;; New added after myracet1.rkt
;;;
(define >
  (lambda (n m)
    (cond 
      ((zero? n) #f)
      ((zero? m) #t)
      (else (> (sub1 n) (sub1 m))))))

;;; What is the maning of (> (sub1 n) (sub1 m))
;;;        Recur, but with both arguments reduced by one
(define dd
  (lambda (n m)
    (cond 
      ((zero? n) #f)
      ((zero? m) #t)
      (else (> (sub1 n) (sub1 m))))))

;;; Can you describe what leftmost does?
;;;      Here is our description:
;;;       "The function leftmost finds the leftmost atom in a non-empty list of S-expressions that does not
;;;       contain the empty list"
;;;
;;;       Is left a *-function?
;;;            It works on lists of S-expressions,but it only recurs on the car(so no)
;;;
;;;     Does leftmost need to ask questions about all three possible cases?
;;;       No,it only needs to ask two questions,We agreed that leftmost works on non-empty
;;;       list that don't contain empty list
;;;
;;;       So in a word,the two discussed before,1:recur in car(not together with cdr) 2:2 questions ,not 3 questions which S-expressions
;;;
;;;
;;;       Do you remember what (or ...) does?
;;;            (or as questions one at a time until it finds one that is true.Then (or ...) stops,making its value true.
;;;            If it cannot find a true argument,the value of (or...) is false
;;;
;;;     After so many illustration ,can you put in your own words what (and ....) does
;;;             We put in our words:(lispers' words)
;;;             "(and ...) ask questions one at a time until it finds one whose value is false.Then (and ...) stops with false.
;;;              If none of the expressions are false,(and ...) is true."
;;;
;;;     Both (and ...) and (or ...) can be expressed as abbreviations of (cond ...)-expressions:
;;;              (and a b) == (cond a b) (else #f))
;;;              (or a b) == (cond (a #t) (else b))
(define leftmost
  (lambda (l)
    (cond 
      ((atom? (car l)) (car l))
      (else (leftmost(car l))))))

(define rember
  (lambda (a lat)
    (cond 
      ((null? lat) (quote()))
      (else (cond
              ((eq? (car lat) a) (cdr lat))
              (else 
               (cons (car lat)
                     (rember a
                             (cdr lat)))))))))
;;; Can you write number? which is true if its argument is a numerica atom and false if it is anything else
;;;      No:number? like add1,sub1,zero?,car , cdr, cons,null?, eq?,and atom? is a primitive function
;;;
;;;      Can you write a function no-nums which gives as a final value a lat obtained by removing all the
;;;      numbers from the lat.

;;; Remember to use = for numbers
;;;                 eq? for all other atoms(non-numericas atoms)
;;;     eqan? contains two part to compare the two atoms: 1 a number   2:an atom
;;;     Can you assume that all funtions written using eq? can be generalized by replacing eq?
;;;          By eqan?   ------------------Yes,excpet,of course ,for eqan? itself
(define eqan?
  (lambda (a1 a2)
    (cond
      ((and (number? a1) (number? a2))
       (= a1 a2))
      ((or (number? a1) (number? a2))
       #f)
      (else (eq? a1  a2)))))

;;;after the introuction of (and  (or   define leftmost
;;; What is eqlist?
;;;
;;;  It is a function that determines if two lists are equals
;;;
;;; How many questions will eqlist? have to ask about it arguments?
;;;             Nine  --------------------------------------------------------------<<<<<The first time so many questions!
;;;
;;; Can you explain why there are nine questions?
;;;        Here are out words:
;;;         "Each arguments may be either
;;;             ----empty
;;;             ----an atom consed onto a list,or
;;;             ----a list consed onto a list
;;;        For example,at the same time as the first argument may be the empty list,the 
;;;              seconds argument could be the empty list or have an atom or a list in the car position."
;;;
;;;         Is it okay to asK (atom? (car l2)) in the second question?
;;;               Yes ,because we know that the second  list cannot be empty.otherwise the first question would have been true.
;;;
;;;         And why is the third question (null? l1)???
;;;              At that point ,we know that when the firs argument is empty,the second argument is neither empty list
;;;              nor a list with an atom as the first element .If(null? l1) is true now,the second argument must be a list whose first element
;;;              is also a list.
;;;
;;;         Does this mean the questions (and (null? l1) (null? l2)) and (or (null? l1) (null? l2)) 
;;;                suffice to determine the answer in the first three cases?
;;;             Yes .If the first question is true,eqlist? respoinse with #t; otherwise ,the answer if #f
(define eqlist?
  (lambda (l1 l2)
    (cond 
      ((and (null? l1) (null? l2)) #t)
      ((and (null? l1) (atom? (car l2))) #f)
      ((null? l1) #f)
      ((and (atom? (car l1)) (null? l2)) #f)
      ((and (atom? (car l1)) (atom? (car l2)))
       (and (eqan? (car l1) (car l2)) (eqlist? (cdr l1) (cdr l2))))
      ((atom? (car l1)) #f)
      ((null? l2) #f)
      ((atom? (car l2)) #f)
      (else
       (and (eqlist? (car l1) (car l2))
            (eqlist? (cdr l1) (cdr l2)))))))

(define eqlist1?
  (lambda (l1 l2)
    (cond 
      ((and (null? l1) (null? l2)) #t)
      ((or (null? l1) (null? l2)) #f)
      ((and (atom? (car l1)) (atom? (car l2)))
       (and (eqan? (car l1) (car l2)) (eqlist1? (cdr l1) (cdr l2))))
      ((or (atom? (car l1))
           (atom? (car l2)))
       #f)
      (else
       (and (eqlist1? (car l1) (car l2))
            (eqlist1? (cdr l1) (cdr l2)))))))

;;; So  What is an S-expresssion? 
;;;                                    An S-expression is either an atom or a (possibly empty) list of S-expressions.
;;;
;;;      How many questions does equal? ask to determine whether two S-empty are the same?
;;;           Four. The first argument  may be an atom or a list of S-expressions at the same time as the second argument may be
;;;           an atom or a list of S-expressions.
;;; can we summarize the second questions and the third question of the equal? 
;;;   as   (or (atom? s1) (atom? s2))
;;;              Yes, we can


(define equal?
  (lambda (s1 s2)
    (cond
      ((and (atom? s1) (atom? s2)) 
       (eqan? s1 s2))
      ((atom? s1) #f)
      ((atom? s2) #f)
      (else
       (eqlist? s1 s2)))))

(define equal1?
  (lambda (s1 s2)
    (cond
      ((and (atom? s1) (atom? s2)) 
       (eqan? s1 s2))
      ((or (atom? s1) (atom? s1))
       #f)
      (else
       (eqlist? s1 s2)))))

;;; The sixth chapter:
;;;    Is 1,3,1+3,1+3*4,cookie,3^y_5 all arithmetic expression?  Yes ,of course
;;;
;;; What is an arithmetic expression? in your words?
;;;       In ours:
;;;         "For the purpose of this chapter,an arithmetic expression is either an atom
;;;         (including numbers),or two arithmetic expressions combined by +,-,*,/,or expr.
;;;
;;; Is (n+3) an arithmetic expression?
;;;        Not really,since there are parentheses around n+3, our definition of arithmetic
;;;        expression does not mention parentheses....
;;;
;;; Could we think of (n+3) as an arithmetic expression?
;;;        Yes if we keep in mind that the parentheses are not really there (the empire's new clothes)
;;;
;;; What would you call (n+3)  
;;;                   we call it a representation for n+3
;;;         why is (n+3) a good representation?
;;;                  Because 
;;;                          1. (n+3) is an S-expression. 
;;;                               It can therefore serve as an argument for a function
;;;                          2.It stucturally resembles n+3
;;;
;;; True or false: (numbered? x)  True while x is 1
;;;
;;; How do you represent 3+4*5  ?                      ------> (3+4*5)
;;;
;;; What is numbered?     
;;;                     It is a function that determines whether a representation? of an arithmetic expression 
;;;                      contains only number besides the +,* and expr
;;;
;;; Now you can write a skeleton for numbered?
;;;           (define numbered?
;;;           (lambda (axep)
;;;           (cond 
;;;           (_________   _________)
;;;           (_________   _________)
;;;           (_________   _________)
;;;           (_________   _________))))
;;;
;;; What is the first question?        --------------(atom? axep)
;;; What is (eq? (car (cdr aexp)) (quote +))  ------It is the second question
;;; can you guess the third one (eq? (car (cdr aexp)) (quote *))
;;; And you must know the fourth one.                     (eq? (car (cdr aexp)) (quote )e)
;;;
;;; why do we ask four,instead of two,  
;;;          questions about the arithmetic expressions?
;;;          after all,arithmetic expressions like (1+3) are lats
;;;
;;;         ------------------------------Because we consider (1+3) as a representation of an arithmetic expression in 
;;;         list form,not as a list itself.And,an arithmetic expression is either a number,or two arithmetic expressions combined
;;;         by + ,*,expr.
;;;
(define numbered?
  (lambda (aexp)
    (cond
      ((atom? aexp) (number? aexp))
      ((eq? (car (cdr aexp)) (quote +))
       ...)
      ((eq? (car (cdr aexp)) (quote +))
       ...)
     ((eq? (car (cdr aexp)) (quote +))
       ...))))


(define numbered1?
  (lambda (aexp)
    (cond
      ((atom? aexp) (number? aexp))
      ((eq? (car (cdr aexp)) (quote +))
       (and (numbered? (car aexp))
            (numbered? 
              (car (cdr (cdr aexp))))))
      ((eq? (car (cdr aexp)) (quote +))
       (and (numbered? (car aexp))
            (numbered? 
              (car (cdr (cdr aexp))))))
     ((eq? (car (cdr aexp)) (quote +))
       (and (numbered? (car aexp))
            (numbered? 
              (car (cdr (cdr aexp)))))))))

;;; Since aexp was already understood to be an arithmetic expression,could we have written numbered? in a simpler way?

(defin numbered2?
       (lambda (aexp)
         (cond
           ((atom? aexp) (number? aexp))
           (else
             (and (numbered2? (car aexp))
                  (numbered2? 
                    (car (cdr (cdr aexp))))))))))

;;; why can we simplify ??-
;;;                        because we know we've got the function right!

;;; We want to value the arithmetic expressions? ------------------
;;;
;;; THe seventh commandments!!
;;;
;;; Recur on the subparts that are of the same nature:
;;;            On the sublists of a list
;;;            on the subexpressions of an arithmetic expression
;;;
;;; To (3 + 4)    's value
(define value
  (lambda (nexp)
    (cond
      ((atom? nexp) nexp)
      ((eq? (car (cdr nexp)) (quote +))
       (+ (value (car nexp))
          (value (car (cdr (cdr nexp))))))
      ((eq? (car (cdr nexp)) (quote *))
       (+ (value (car nexp))
          (value (car (cdr (cdr nexp))))))
      (else
       (expr (value (car nexp))
          (value (car (cdr (cdr nexp)))))))))

;;; Can you think of a different representation of arithmetic expressions?
;;;          There are several of thme
;;;
;;;          (3 4 +)
;;;          (+ 3 4)
;;;          (plus 3 4)
;;;          (+ (* 3 6) (expr 8 2))
;;;         TO value (+ 3 4)
      
(define value1
  (lambda (nexp)
    (cond
      ((atom? nexp) nexp)
      ((eq? (car nexp) (quote +))
       (+ (value1 (car (cdr nexp)))
          (value1 (car (cdr (cdr nexp))))))
      ((eq? (car nexp) (quote *))
       (+ (value1 (car (cdr nexp)))
          (value1 (car (cdr (cdr nexp))))))
      (else
       (expr (value1 (car nexp))
          (value1 (car (cdr (cdr nexp)))))))))

;;;(1 2) is not a arithmetic expressions ,so we  violated The Seventh Commandments. (1 3) is not a subpart that is 
;;;   representation of an arithmetic expression! We obviosuly  recurred on a list.But,remember,not alll lists are representations
;;;   of arithmetic expressions. We have to recur subexpressions.
;;;   (3) is the (cdr (cdr nexp)) ,but (3) is not an arithmetic expressions,because there are no parentheses in our 
;;;   definition of the arithmetic expressions.

(define 1st-sub-exp
  (lambda (aexp)
    (cond
      (else (car (cdr aexp))))))

;;; why do we ask else,
;;;                   because the first question is also the last question
;;;
;;; Can we get by without (cons. ..) if we don't need to ask questions?
;;;        Yes remember one-liners from chapter4
;;;

(define 1st-sub-exp1
  (lambda (aexp)
      (car (cdr aexp))))


(define 2nd-sub-exp
  (lambda (aexp)
    (cond
      (else (car (cdr (cdr aexp)))))))

(define operator
  (lambda (aexp)
    (car aexp)))


(define value2
  (lambda (nexp)
    (cond
      ((atom? nexp) nexp)
      ((eq? (operator nexp) (quote +))
       (+ (value (1st-sub-exp1 nexp))
          (value (2nd-sub-exp nexp))))
      ((eq? (operator nexp) (quote +))
       (* (value (1st-sub-exp1 nexp))
          (value (2nd-sub-exp nexp))))
      (else
       (expr (value (1st-sub-exp1 nexp))
          (value (2nd-sub-exp nexp)))))))

;;; so it we chang the (define operator ) and (1st-sub-exp1) exchangeabley ,the different form of arithmetic expressioned.


;;;       ********************************************************************************
;;; The Eighth Commandments:
;;;       Use help functions to abstract from representation
;;;       ********************************************************************************
;;;
;;;
;;;
;;;       Have we seen representation? before
;;;                                      Yes ,we just did not tell you that were representations.
;;;                             For what entities have we used representations?
;;;                             Truth-values!  Numbers!
;;;                                              Numbers are representations?
;;;                                                    Yes ,for example 4 stands for the concpet four. we close the symbol because
;;;                                                    we are accustomed to arabic representation.
;;;                                              What else could we have used?
;;;                                                (() () () ())  would have serve just as well
;;;                                                what about  (((((((())))))))?  How about (I V)"
;;;                                         Do you know how many primitives we need for numbers?
;;;                                          Four: number? ,zero?, add1,and sub1-------------------------------------------<
;;;                                                            ****Do You know how many primitives we need for list?
;;;                                                                   Four:   atom? null? car cdr
;;;                                         Let's try anouther representation for numbers. How shall we represent zero now/
;;;                                                       () is our choice
;;;                                         How is one represented?
;;;                                                       (()) 
;;;
;;;                                         How is two represented?
;;;                                                       (() () )
;;;                                          Got it,What's Three?
;;;                                                       (() () ())
;;; So now write thefunction to test zero
(define sero?
  (lambda (n)
    (null? n)))
;;; write a function that is like add1
(define edd1
  (lambda (n)
    (cons (quote ()) n)))

;;;what about sub1
(define zub1
  (lambda (n)
    (cdr n)))

;;; so we can rewrite +
(define plus
  (lambda (n m)
    (cond
      ((sero? m) n)
      (else (edd1 (+ n (zub1 m)))))))

;;; but what's lat????t
;;;(define lat?
;;;  (lambda (l)
;;;  (cond
;;;      ((null? l) #t)
;;;      ((atom? (car l)) (lat? (cdr l)))
;;;      (else #f))))
;;; So (lat? (1 2 3))     why did you ask???  but now (lat? ((()) (()()) (()()()))) It is very false,!!!
;;;                                                 So you must beware of shadows!!!bugs implicit!!



;;;                         
;;; Why dow we ask (number? aexp) when we know that aexp is an atom?
;;;                   Because we want to know if all arithmetic expressions that are atoms are numbers

;;; Does equal1? ask enough questions? --The first commandments!!  
;;;              Yes,The questions cover all four possible cases

;; Now rewrite the eqlist? using equal???                --------------equal? is the most common ,which  S-expression as the arguments
    
(define eqlist1?
  (lambda (l1 l2)
    (cond
      ((and (null? l1) (null? l2)) #t)
      ((or (null? l1) (null? l2)) #f)
      (else
       (and (equal? (car l1) (car l2))
            (eqlist? (cdr l1) (cdr l2)))))))

;;;; THe sixth Commandments
;;;     Simplify  only  after the function is correct.   ---------------------------------<<<<<<<<<<<

;;;; Can you describe what (rember1 a lat) do?
;;;          It takes an atom and a lat as its *arguments*,and makes a new *lat*
;;;          with the *first* occurrence of the atom in the old lat removed
;;;
;;;     How do we(lispers,implicitly) ask questions?
;;;            By using
;;;                   (cond
;;;                     (__________     ____________)
;;;                     (__________     ____________)
;;;                     (__________     ____________)
;;;                     ...
;;;                     (__________     ____________)).
;;;     How do we  remove the first occurrence of a in the rest of lat?
;;;             (rember a (cdr lat))
;;;
;;;
;;; Write down the function rember and its arguments, and refer to them as you go
;;; through the next sequence of questions(the unit test)
;;;    In our words:
;;;              "The function rember checked each atom of the lat , one at a time,
;;;               to see if it was the same  as the atom 'and'.If the car was not the
;;;               same as the atom, we saved it to be consed to the final value later.
;;;               when rember found the atom 'and',it dropped it ,and consed the previous
;;;               atoms back onto the rest of the lat
;;;  ---------------------------------cons --------------------------------------
(define rember
  (lambda (a lat)
    (cond 
      ((null? lat) (quote ()))
      (else cond
            ((eq? (car lat) a) (cdr lat))
            (else (cons (car lat)
                        (rember a
                                (cdr lat))))))))

;;; Can you rewrite rember so that it reflects the abouve description
;;;
(define rembersimple
  (lambda (a lat)
    (cond
      ((null? lat) (quote ()))
      ((eq? (car lat) a) (cdr lat))
      (else (cons (car lat)
                  (rember a (cdr lat)))))))

;;;;  What is the meaning of the  (cons (car lat) (rember a (cdr lat))) 
;;;
;;;           This says to refer to the funciton rember but with the 
;;;           argument lat replaced by (cdr lat) and  that after we arrive at a value for
;;;           (rember a (cdr lat)) we must cons (car lat) --bacon --onto it
;;;
;;;           This also says we recur using the function rember, with the argument lat
;;;           replaced by (cdr lat),and that after we arrive at a value for (rember a (cdr lat))
;;;           we must cons (car lat)---lettuce --onto it
;;;
;;;
;;; Here is rember1  after we replace lat by a list l of S-expression 
(define rember1
  (lambda (s l)
    (cond
      ((null? l) (quote()))
      ((atom? (car l))
       (cond 
         ((equal? (car l) s) (cdr l))
             (else (cons (car l)
                         (rember1 s (cdr l))))))
      (else
       (cond
         ((equal? (car l) s) (cdr l ))
         (else (cons (car l)
                     (rember1 s 
                             (cdr l)))))))))
;;;  And how does that differ?
;;;        The funciton rember now removes the first matching S-expression s in l,
;;;                  instead of the first matching atom a in lat.
;;;    Is rember a  "star" function now? 
;;;             NO.
;;;     Why not?
;;;          because rember recurs with the cdr of l only (not togeterh with the car of l ,,correspoding to (define leftmost)
;;;; can we simplify rember1 ?
(define rember2
  (lambda (s l)
    (cond
      ((null? l) (quote()))
      (else 
       (cond
         ((equal? (car l) s) (cdr l))
         (else
          (cons (car l)
                (rember2 s (cdr l)0))))))))

;;; Can rember2 be further simplified????????????????????????
;;;   Yes,the inner(cond...) asks questions that the outer(cond...) could ask1
(define rember3
  (lambda (s l)
    (cond 
      ((null? l) (quote ()))
      ((equal? (car l) s) (cdr l))
      (else (cons (car l)
                  (rember3 s (cdr l)))))))

;;; "...*" makes us think "oh my gawd"
(define rember*
  (lambda (a l)
    (cond 
      ((null? l ) (quote ()))
      ((atom? (car l))
       (cond 
        ((eq? (car l) a)
         (rember* a (cdr l)))
        (else (cons (car l)
                    (rember* a (cdr l))))))
      (else (cons (rember* a (car l))
                  (rember* a (cdr l)))))))
;; Notice that now we are recurring down the car of the list,instead of just the cdr of the list


(define insertL
       (lambda (new old lat)
         (cond
           ((null? lat) (quote ()))
           (else (cond
                   ((eq? (car lat) old)
                    (cons new 
                          (cons old (cdr lat))))
                   (else (cons (car lat)
                               (insertL new old 
                                        (cdr lat)))))))))

;;;; after ------------------------------------<the (define firsts  
;;;In your own words, what does (insertR new old lat) do?
;;;     In our words:
;;;             "It takes three arguments: the atoms new  and old,and lat.
;;;             The function insertR builds a lat with new inserted to the
;;;             right of the *first* occurence of old
;;;
;;;
;;;     Which arguments changes when we recur with insertR?
;;;           lat, because we can only  look at one of its atoms at a time.
;;;
;;;     Which questions do we ask?
;;;       First, we ask (null? lat) .Second we ask else,because else is 
;;;       always the last question.
;;;     Which questions do we ask about the first elements?????? Still rember the typical element
;;;       in the (define firsts)
;;;
;;;     First,we ask (eq? (car lat) old ) .Then we ask else,because there are no other interesting cases
;;;  
;;;  COns 's function: save the buffer to the stack
;;;
;;;  I want to write an procedure: which sort the list  afterwards?
;;;            So I want the change the function' return value?????????????????????????
;;;            What I should do??
(define insertR
  (lambda (new old lat)
    (cond
      ((null? lat) (quote ()))
      (else (cond
              ((eq? (car lat) old)
               (cons old 
                     (cons new (cdr lat))))
              (else (cons (car lat)
                           (insertR new old 
                                    (cdr lat)))))))))

;;;; after the (define makeset1)

(define subset
  (lambda (new old lat)
    (cond 
      ((null? lat) (quote ()))
      (else (cond
              ((eq? (car lat) old)
               (cons new (cdr lat)))
              (else (cons (car lat)
                          (subset new old 
                                  (cdr lat)))))))))

(define subset2
  (lambda (new o1 o2 lat)
    (cond
      ((null? lat) (quote ()))
      (else (cond
              ((eq? (car lat) o1)
               (cons new (cdr lat)))
              ((eq? (car lat) o2)
               (cons new (cdr lat)))
              (else (cons (car lat)
                          (subset2 new o1 o2
                                   (cdr lat)))))))))


(define rember5
  (lambda (a lat)
    (cond 
      ((null? lat) (quote ()))
      ((eq? (car lat) a) (cdr lat))
      (else (cons (car lat)
                  (rember5 a (cdr lat)))))))

(define factorial 
    (lambda (x)
      (if (<= x 1) 1
          (* x (factorial (- x 1))))))

;;;;  after rember ---------------------------------------------------<<<<<
;;; In  your own words , what does (firsts l) do
;;;          We tried the following:
;;;          "The function firsts takes one argument, a list,which is either
;;;          a null list or contains only non-empty lists. It builds another list
;;;          composed of the first S-expression(atom or list  or pair) of each internal list
;;;
;;;          REMEMBER THe Commandments
;;;
;;;          Why (define firsts
;;;                 (lambda (l)
;;;                  ...)                     Because we always state the function name,(lambda,and the 
;;;                                           argument(s) of the function
;;;          Why (cond ...)
;;;                                           Because we need to ask questions about the actual arguments
;;;          Why ((null? l) ...)
;;;                                           The first Commandments
;;;          Why (else
;;;                                           Because we only have two equstions to ask about the list l:
;;;                                                      either It is the null list,
;;;                                                                or it contains at least one non-empty list.
;;;                                                 And Because the last question is always else
;;;         Why (cons 
;;;                                           Because we are building  a lsit  -----The second commandments
;;;
;;;         Why (firsts (cdr l)) 
;;;                                           Because we can only  look at one S-expression at at time .
;;;                                                   To look at the rest,we must recur
;;;         why )))
;;;                                           Because these are the matching  parentheses for (cond, (lambda,
;;;                                           and (define, and they always appear at the end of a function 
;;;                                           definition
;;;
;;;        When we find a typical element of (firsts l) 
;;;                 what do we do with it?
;;;                                       --cons it onto the recursion --(firsts (cdr l))
;;;
;;;
;;;     The third commandments :
;;;                 When building a list, describe the first typical element,and then cons it onto the natural recursion
;;;         So What is the first typical element?
;;;                             ----------------------->  In the (define firsts  The typical element is (car (car l))
;;;            What  is the natural recursion?
;;;                            -----------------------> (firsts (cdr l))
;;;
;;;
;;;             ****************************we are still missing one important ingredient in our recipe!
;;;
;;;
;;;             So what is the meaning of (cons (car (car l))  (firsts (cdr l)))
;;;                           Save (car (car l)) ,and recur with (firsts (cdr l))
(define firsts
  (lambda (l)
    (cond
      ((null? l) (quote ()))
      (else (cons (car (car l))
                  (firsts (cdr l)))))))

(define multirember
  (lambda (a lat)
    (cond
      ((null? lat) (quote ()))
      (else
       (cond
         ((eq? (car lat) a)
          (multirember a (cdr lat)))
         (else (cons (car lat)
                     (multirember a (cdr lat)))))))))
;;; What is the meaning of (cons (car lat) (multirember a (cdr lat)))
;;;  Save (car lat) ---coffee ---- to be consed onto the value of (multirember a (cdr lat)) later
;;;  Now determine (multirember a (cdr lat))
;;;
;;;  THe Fourth commandments:
;;;         Always change at least one arguments while recuring.It must be changed to be closer to 
;;;         termination.The changing argument must be tested in the termination condition:
;;;                   When using cdr, test termination with null?, such as (null? lat)  lat is the 
;;;                   changing argument,so it occur in the terminal condition.

(define multiinsertR
  (lambda (new old lat)
    (cond
      ((null? lat) (quote ()))
      (else 
       (cond
         ((eq? (car lat) old)
          (cons (car lat)
                (cons new
                      (multiinsertR new old (cdr lat)))))
         (else (cons (car lat)
                     (multiinsertR new old (cdr lat)))))))))


(define multiinsertL
  (lambda (new old lat)
    (cond
      ((null? lat) (quote ()))
      (else 
       (cond
         ((eq? (car lat) old)
          (cons new
                (cons (car lat)
                      (multiinsertL new old (cdr lat)))))
         (else (cons (car lat)
                     (multiinsertL new old (cdr lat)))))))))

(define multisubset
  (lambda (new old lat)
    (cond
      ((null? lat) (quote ()))
      (else
       (cond
         ((eq? (car lat) old)
          (cons new
                (multisubset new old 
                             (cdr lat))))
         (else
          (cons (car lat)
                (multisubset new old 
                             (cdr lat)))))))))

(define add1
  (lambda (x)
    (+ x 1)))

(define sub1
  (lambda (x)
    (- x 1)))




; (define +
;   (lambda (n m)
;     (cond 
;       ((zero? m) n)
;       (else (add1 (+ n (sub1 m)))))))
; 
; (define -
;   (lambda (n m)
;     (cond 
;       ((zero? m) n)
;       (else (sub1 (- n (sub1 m)))))))
;
;
;
;;;  Can you describe how (- n m) works??
;;;    It takes two numbers as arguments,and reduces the second until it hits zeros.
;;;    It subtracts one from the result as many times as it did to cause the second 
;;;    one to reach zero!!!

;if add the box above the wrong to the expr define

;;;  What is the natural terminal condition for a list?
;;;              (null? l)
;;;
;;;  What is the natural terminal conditon for a tup?
;;;              (null? tup)
;;;              
;;; When we build a number from a list of numbers, what should the terminal condition line look like?
;;;               ((null? tup) 0)  ,just as ((null? l) (quote ()))  is ofter the terminal conditon line
;;;               for lists
;;;     So what is the terminal condition line of addtup?
;;;           What does addtup do?
;;;             It builds a number by totalling all the numbers in its arguments
;;;
;;;
;;; What is used in the natural recursion on a list?
;;;       (cdr lat)
;;;
;;; What is used in the natural recursion on a tup?
;;;        (cdr tup)
;;; Why?
;;;      Because the rest of a non-empty list is a list
;;;              The rest of a non-empty tup  is a tup
;;;
;;;
;;; How is a number defined?
;;;    It is either zero or it is one added to a res,
;;;           where rest is again  a number
;;;
;;; what is  the natural terminal condition for numbers?
;;;         (zero? n)
;;;
;;; What is the natural recursion on a number? (sub1 n)            (Cdr tup)  (cdr l)
;;;        (how many questions do we need to ask about a number?   ---------------< two
;;;
;;;        So The first commandments(revision)
;;;          When recurring on a list of atoms,lat, ask two questions about it: (null? lat) and else
;;;          When recurring on a number,n ,as two questions about it:(zero? n) and else
;;;
;;;
;;;    and What is the terminal condition line of addtup
;;;           ((null? tup) 0)
;;; So what is the natural recursion for addtup?
;;;           (addtup (cdr tup))
;;;
;;;     what does addtup use to build a number?
;;;               It uses + , because + builds numbers, too!!!!!!!!!!!!!!!!!!!!!!!!!!!
;;;
;;;
(define addtup
  (lambda (tup)
    (cond 
      ((null? tup) 0)
      (else (+ (car tup) (addtup (cdr tup)))))))

       ;;(cons (car tup) (addtup (cdr tup)))))))   The last line of function rember is the similar to the one in the addtup!!!!
       ;;
;;;; The fourth commandments(revision)
;;;  Always change at least one arguments while recurring. It must be changed to be closer to termination.The changing argument must
;;;  be tested in the termination condition:
;;;         when using cdr, test termination with  null? and
;;;         when using sub1, test termination with zero


(define expr
  (lambda (n m)
    (cond
      ((zero? m) 1)
      (else (* n (expr n (sub1 m)))))))


(define multiplication
  (lambda (n m)
    (cond 
      ((zero? m) 0)
      (else (+ n (multiplication n (sub1 m)))))))

(define divide
  (lambda ( n m)
    (cond
      ((< n m ) 0)
      (else (add1 (divide (- n m) m))))))


(define pick 
  (lambda (n lat)
    (cond 
      ((zero? (sub1 n)) (car lat))
      (else (pick (sub1 n) (cdr lat))))))

;;; Can you write number? which is true if its argument is a numerica atom and false if it is anything else
;;;      No:number? like add1,sub1,zero?,car , cdr, cons,null?, eq?,and atom? is a primitive function
;;;
;;;      Can you write a function no-nums which gives as a final value a lat obtained by removing all the
;;;      numbers from the lat.

;;; after (define rempick)  --------------------------------------------<<<<<<<<<<<<<<<
(define no-nums
  (lambda (lat)
    (cond
      ((null? lat) (quote ()))
      (else
       (cond
         ((number? (car lat))
          (no-nums (cdr lat)))
         (else (cons (car lat)
                     (no-nums (cdr lat)))))))))

(define all-nums
  (lambda (lat)
    (cond
      ((null? lat) (quote ()))
      (else 
       (cond
         ((number? (car lat))
          (cons (car lat)
                (all-nums (cdr lat))))
         (else (all-nums (cdr lat))))))))

;;;;     after the define of eqan? (define eqan?)
;;;what does occur do?
;;;        the function occur which counts the number of times an atom a appears in a lat
(define occur
  (lambda (a lat)
    (cond 
      ((null? lat) 0)
      (else
       (cond
         ((eq? (car lat) a)
          (add1 (occur a (cdr lat))))
         (else (occur a (cdr lat))))))))

(define one?
  (lambda (n)
    (cond 
      ((zero? n) #f)
      (else (zero? (sub1 n))))))

(define one1? 
  (lambda (n)
    (cond
      (= n 1)
      (else #f))))
(define one11?
  (lambda (n)
    (= n 1)))
;;; Guess how we can further simplify this  function ,maing it a one-liner
;;;       By removing the (cond )clause.

(define negativeone?
  (lambda (n)
    (cond 
      ((zero? n) #f)
      (else (zero? (add1 n))))))

(define rember*
  (lambda (a l)
    (cond
      ((null? l) (quote ()))
      ((atom? (car l))
       (cond 
         ((eq? (car l) a)
          (rember* a (cdr l)))
         (else (cons (car l)
                     (rember* a (cdr l))))))
     (else (cons (rember* a (car l))
           (rember* a (cdr l)))))))

;;; After rember*            (define rember*) -----------------------------------------<<<<<<<<
;;; How are insertR* and rember* similar?
;;;        0--------------------Each function asks three questions,. Each function recurs on the car
;;;                  of its argument when it finds out that the argument's car is a list
;;;                  namely,They both recur with car,whenever the car is alist,as well as with the cdr
;;; ******************************************************************************************************************
;;;        The first commandments(Final version)
;;;          When recurring on  a list of atoms,lat,ask two questions about it:
;;;                       (null? lat) and else
;;;          When recurring on a number,n ,ask two questions about it:
;;;                       (zero? n) and else.
;;;          When recurring on a list of S-expression,l ,ask three questtion,about it:
;;;                       (null? l),(atom? (car l)) , and else
;;;
;;;
;;; ******************************************************************************************************************
;;; How are rember* and multirember different?
;;;         The function multirember does not recur with the car. The function rember* recurs with the car when if
;;;           finds out that the car is a list
;;;
;;; So how are all *-functions similar?
;;;          They all ask three questions and recur with the car as well as   with the cdr,whenever the car is a list
;;;          Why?
;;;                             Because all *-functions work on lists that are either 
;;;                                                                      ---empty
;;;                                                                      ---an atom consed onto a list,or
;;;                                                                      ---a list consed onto a list
;;;
;;; ******************************************************************************************************************
;;;
;;;         TheFourthe commandments(final version)
;;;             Always change at least one argument while recurring. 
;;;              when recurring on alist of atoms, lat,use (cdr lat.
;;;              When recurring on a number,n ,use (sub1 n). And  
;;;              when recurring on a list of S-expressions,l,use (car l) and (cdr l) if neither (null? l) nor (atom? (car l) are true
;;;
;;;              It must be changed to be closer to termination. The changing argument must be tested in the termination condition:
;;;                     When using cdr, test termination with null? and
;;;                     When using sub1, test termination with zero?
;;; ******************************************************************************************************************
(define insertR*
  (lambda (new old l)
    (cond 
      ((null? l) (quote ()))
      ((atom? (car l))
       (cond 
         ((eq? (car l) old)
          (cons old
                (cons new (insertR* new old (cdr l)))))
         (else (cons (car l)
                     (insertR* new old
                               (cdr l)))))
       (else cons (insertR* new old
                            (car l))
             (insertR* new old (cdr l))))))

;;; 
(define insertL*
  (lambda (new old l)
    (cond 
      ((null? l) (quote ()))
      ((atom? (car l))
       (cond 
         ((eq? (car l) old)
          (cons new
                (cons old (insertR* new old (cdr l)))))
         (else (cons (car l)
                     (insertL* new old
                               (cdr l)))))
       (else cons (insertL* new old
                            (car l))
             (insertL* new old (cdr l))))))


(define occur*
  (lambda (a l)
    (cond
      ((null? l) 0)
      ((atom? (car l 0))
       (cond
         ((eq? (car l) a)
          (add1 (occur* a (cdr l))))
         (else 
          (occur* a (cdr l)))))
      (else
       (+ (occur* a (car l))
          (occur* a (cdr l)))))))

;;;;
;;;  I introuduce how the (or (null? l) (atom? a))  works,Then I can go on 
;;;    introuducing the member
;;; else implicit said that if the condition is wrong do it recurly
;;;      also infer that after the recuring ,please go back to home along the Original road!
(define member*
  (lambda (a l)
    (cond
      ((null? l) #f)
      ((atom? (car l))
       (or (eq? (car l) a)
           (member* a (cdr l))))
      (else
       (or (member* a (car l))
           (member* a (cdr l)))))))
;;;
;;; The function tells:
;;;          (condition return values)
;;; Can you write down the definition of the function member? and its arguments
;;;     and refer to them as you go through the next group of questions????
(define member?
  (lambda (a lat)
    (cond
      ((null? lat) #f)
      (else (or (eq? (car lat) a)
                (member? a (cdr lat)))))))



(define first
  (lambda (p)
    (cond (else (car p)))))
(define second
  (lambda (p)
    (cond
      (else (car (cdr p))))))

;;;;Could  after (define subset? )        write a function (intersect?)
(define intersect?
  (lambda (set1 set2)
    (cond
      ((null? set1) #f)
      (else
        (cond
          ((member? (car set1) set2) #t)
          (else 
            (intersect? 
              (cdr set1) set2)))))))

(define intersect1?
  (lambda (set1 set2)
    (cond
      ((null? set1) #f)
      ((member? (car set1) set2) #t)
      (else 
            (intersect1? 
              (cdr set1) set2)))))
(define intersect2?
  (lambda (set1 set2)
    (cond
      ((null? set1) #f)
      (else (or (member? (car set1) set2)
                (intersect1? (cdr set1) set2))))))

(define intersect
  (lambda (set1 set2)
    (cond
      ((null? set1) (quote ()))
      ((member? (car set1) set2)
       (cons (car set1)
             (intersect (cdr set1) set2)))
      (else
       (intersect (cdr set1) set2)))))
(define intersect
  (lambda (set1 set2)
    (cond
      ((null? set1) (quote ()))
      ((member? (car set1) set2)
       (cons (car set1)
             (intersect (cdr set1) set2)))
      (else
       (intersect (cdr set1) set2)))))

;;;; In our words: It is a function that return s all the atoms
;;;in set1 that are not in the set2,That is ,xxx is the (set) difference functions=
(define xxx
  (lambda (set1 set2)
    (cond
      ((null? set1) (quote ()))
      ((member? (car set1) set2)
       (xxx (cdr set1) set2)))
      (else
        (cons (car set1)
              (xxx (cdr set1) set2)))))

(define instersectall
  (lambda (l-set)
    (cond
      ((null? (cdr l-set)) (car l-set))
      (else
        (interset (car l-set)
                  (intersectall (cdr l-set)))))))


(define union
  (lambda (set1 set2)
     (cond
       ((null? set1) set2)
       ((member? (car set1) set2)
        (union (cdr set1) set2))
       (else
        (cons (car set1)
              (union (cdr set1) (set2)))))))
(define build 
  (lambda (s1 s2)
    (cond 
      (else (cons s1 
                  (cons s2 (quote ())))))))


;;;;;;;;;;;;;;;;;;;;;;;The first part;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;;;
;;;
;;;;;;;;;;;;;;;;;;;;;;;The second part;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;;;;;  myracet2.rkt ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;;;
;;;Can you write a function  insert-g that would insert either at the left or at the right!!
;;;
;;;Which pieces differ?
;;;              The second lines differ from each other ,In insertL it is:
;;;               ((eq? (car l) old)
;;;                (cons new (cons old (cdr l))))
;;;               But in the insertR it is:
;;;               ((eq? (car l) old)
;;;                (cons new (cons old (cdr l))))
;;;
;;;         Put the difference in words!
;;;               We say:
;;;                      "The two functions cons old and new in a different order onto the cdr of the list l
;;;
;;;         So how can we get rid of the difference >
;;;                You probably guessed it: by passing in a function that expresses tha appropriate consing.
(define seqL
  (lambda (new old l)
    (cons new (cons old l))))
(define seqR
  (lambda (new old l)
    (cons old (cons new l))))

;;; so you can (insert-g seqL)  (insert-g seqR)
;;; (define insertL (insert-g seqL))
;;; (define insertR (insert-g seqR))
;;;
;;;
(define insert-g
  (lambda (seq)
    (lambda (new old l)
      (cond
        ((null? l) (quote ()))
        ((eq? (car l) old)
         (seq new old (cdr l)))
        (else (cons (car l)
                    ((insert-g seq) new old
                                    (cdr l))))))))
;;; So we can define insertL again with insert-g *****************************^-^************************************
;;; Do not pass in seqL this time.
(define insertL1
  (insert-g
    (lambda (new old l)
      (cons new (cons old l)))))

;;;;;; Perfect--- it is  a great abstraction!
;;;
;;;Is it better?
;;;      Yes,Because youu do not need to remember as many names,you can
;;;      (rember func-name "your-mind") 
;;;                Where func-name is seqL
;;;
;;;                Do you remember the definition of subst
;;;                Yes! it looks like insert-L  insertR  .Just the answer of the second cond-line is differenct

(define subst
  (lambda (new old l)
    (cond
      ((null? l) (quote ()))
      ((eq? (car l) old)
       (cons new (cdr l)))
      (else (cons (car l)
                  (subst new old (cdr l)))))))

;subst insertL insertR the common property is else and different 
;operator so we can abstract them to the insert-g
(define seqS
  (lambda (new old l)
    (cons new l)))

(define subst1 (insert-g seqS))

(define seqrem
  (lambda (new old l)
    l))
(define yyy
  (lambda (a l)
    ((insert-g seqrem) #f a l)))
;;;;;What do you yyy is?
;;;            Surprise ! It is our old friend rember,
;;;            Step through the evaluation of (yyy a l)
;;;            where 
;;;                a  is sausage
;;;                and l is (pizza with sausage and bacon)
;;;             What role does #f paly?
;;;             New in the insertL and insertR   ,a is old in the insertL and insertR
;;;
;;;
;;;             **********************************************************************
;;;             THe ninth commandments:
;;;              Abstract the common patterns with a new function.
;;;
;;;             **********************************************************************
;;;

;;;So can you simplify the function (define value1)
;;; Can you write the funciton atom-to-function 
;;;         which:
;;;               1: Taes one argument x 
;;;               2: returns the function + if (eq? x (quote +))
;;;                  returns the function * if (eq? x (quote *))
;;;                  returns the function expr if (eq? x (quote expr))
;;;
(define atom-to-function
  (lambda (x)
    (cond
      ((eq? x (quote +) +))
      ((eq? x (quote *) *))
      (else expr))))
;;;(atom-to-function (operator nexp))

(define value3
  (lambda (nexp)
    ((atom? nexp) nexp)
    (else
      ((atom-to-function
         (operator nexp))
       (value3 (1st-sub-exp nexp))
       (value3 (2nd-sub-exp nexp))))))


(define multirember
  (lambda (a lat)
    (cond 
      ((null? lat) (quote ()))
      ((eq? (car lat) a)
       (multirember a (cdr lat)))
      (else (cons (car lat)
                  (multirember a (cdr lat)))))))

(define multirember-f
  (lambda (test?)
    (lambda (a lat)
      (cond 
        ((null? lat) (quote ()))
        ((test? a (car lat))
         ((multirember-f test?) a
                               (cdr lat)))
        (else (cons (car lat)
                    ((multirember test?)  a 
                                          (cdr lat))))))))

(define multirember-eq? (multirember-f 'eq?))

;;; After the insturciton os a pair---->  
(define first
  (lambda (p)
    (cond 
      (else (car p)))))
(define second 
  (lambda (p)
    (cond 
      (else (car (cdr p))))))
(define third
  (lambda (p)
    (cond 
      (else (car (cdr (cdr p)))))))
(define third1
  (lambda (l)
    (car (cdr (cdr l)))))
(define build
  (lambda (s1 s2)
    (cond 
      (else (cons s1
                  (cons s2 (quote ())))))))

;;;; After the (define fun?)
;;;
(define revrel
  (lambda (rel)
    (cond
      ((null? rel) (quote ()))
      (else
       (cons (build
              (second  (car rel))
              (first (car rel)))
             (revrel (cdr rel)))))))
;; maybe we can abstract the build part,because it's the pair part
;;
;; Now do you see how representabtion aids readability????
;;         second   first is the (car (car rel))         second is (car (cdr (car rel)
;; so it can hide the build inverse process 
;;
;; Suppose we had the funciton revpair that reversed the two componets of a pair like this:
;;
(define revpair;;hide the build process
  (lambda (pair)
    (build (second pair) (first pair))))

(define revrel1
  (lambda (rel)
    (cond
      ((null? rel) (quote ()))
      (else
       (cons (revpair (car rel));so it can be hided
             (revrel1 (cdr rel)))))))
(define member?
  (lambda (a lat)
    (cond
      ((null? lat) #f)
      (else (or (eq? (car lat) a)
                (member? a (cdr lat)))))))
  
;;; The 7 chapter ,,after the numbered
(define set?
  (lambda (lat)
    (cond
      ((null? lat) #t)
      (else
       (cond 
         ((member? (car lat) (cdr lat)) #f
               )
         (else
          (set? (cdr lat))))))))

;;; simplify set?              -------------------------<<<<<<
(define set1?
  (lambda (lat)
    (cond 
      ((null? lat) #t)
      ((member? (car lat) (cdr lat)) #f)
      (else (set? (cdr lat))))))
;;; Were you surprised to see the function member? appear in the definition of set?
;;;       You should not be,because we have written memeber? already,and now we can 
;;;       use it whenever we want
         

(define makeset
  (lambda (lat)
    (cond
      ((null? lat) (quote ()))
      ((member? (car lat) (cdr lat))
       (makeset (cdr lat)))
      (else (cons (car lat)
                  (makeset (cdr lat)))))))
(define makeset1
  (lambda (lat)
    (cond
      ((null? lat) (quote ()))
      (else (cons (car lat)
                  (makeset (multirember (car lat) (cdr lat))))))))
;;; Describe in your own words how the second definition of makeset works?????
;;;        Here are our words:
;;;         ""THe function makeset remembers to cons the first atom in the lat onto the result of the
;;;          natural recursion ,after removing all occurences of the first atom from the rest of the lat.
;;;
(define subset?
  (lambda (set1 set2)
    (cond
      ((null? set1) #t)
      (else (cond
              ((member? (car set1) set2)
               (subset? (cdr set1) set2))
              (else #f))))))

;;; can you  write a shorter version of subset?
(define subset1?
  (lambda (set1 set2)
    (cond
      ((null? set1) #t )
      ((member? (car set1) set2)
       (subset? (cdr set1) set2))
      (else #f)))

;;; Try to write subset1? with (and ...)

(define subset2?
  (lambda (set1 set2)
    (cond
      ((null? set1) #t)
      (else
        (and (member? (car set1) set2)
             (subset? (cdr set1) set2))))))

;;; equip list
;;;
(define eqset?
  (lambda (set1 set2)
    (cond
      ((subset? set1 set2)
       (subset? set2 set1))
      (else #f))))
(define eqset1?
  (lambda (set1 set2)
    (cond
      (else
      (and (subset? set1 set2)
       (subset? set2 set1))))))

(define eqset2?
  (lambda (set1 set2)
      (and (subset? set1 set2)
       (subset? set2 set1))))


(define firsts
  (lambda (rel)
    (cond
      ((null? rel) (quote ()))
      (else
       (cons (first (car rel))
             (firsts (cdr rel)))))))
(define seconds
  (lambda (rel)
    (cond
      ((null? rel) (quote ()))
      (else
       (cons (second (car rel))
             (seconds (cdr rel)))))))
;; one important known : in the (else,things is called recur
;; in the cond's first question is called the end condtion
;; multi and sigle is recognise by the doing's if satisfied the 
;; questions!
;;
;; After te define (pair)
;;
;;Is fun? a simple one-liner?  It sure is
;;
;;How do we reprensent a finite function?
;;       For us(lispers)m
;;                         a finite function is a list of pairs in which
;;                         no first element of any pair is the same as any
;;                         other first element
(define fun?
  (lambda (rel)
    (set? (firsts rel))))


;
(define fullfun?;;fun is also a relation
  (lambda (fun);;fullfun is based on the fun
    (set? (seconds fun))))

;;fullfun's another name is one-to-one
;;so we can define it another way
(define one-to-one?
  (lambda (fun)
    (fun? (revrel fun))))


(define expr
  (lambda (n m)
    (cond
      ((zero? m) 1)
      (else
       (* n (expr n (- m 1)))))))

(define cookies
  (lambda ()
    (bake
      (quote (350 degrees))
      (quote (12 minitus))
      (mix
        (quote (walnuts 1 cup))
        (quote (chocolate-chips 16 ounces))
        mix
        (mix
          (quote (flour 2 cups))
          (quote (oatmeal 2 cups))
          (quote (salt .5 teaspoon))
          (quote (baking-powder 1 teaspoon))
          (quote (baking-soda 1 teaspoon)))
        (mix
          (quote (eggs 2 large))
          (quote (vanilla 1 teaspoon))
          (cream
            (quote (butter 1 cup))
            (quote (sugar 2 cups))))))))
;;;  ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;Y
;;;The Fifth commandments:
;;;  When building a  value with +, always use 0 for the value of the terminating line,for adding 0 does not change the value of an addiction
;;;  When building a  value with *, always use 1 for the value of the termination line,for multiplying 1 does not change the value of a multiplication
;;;  When building a  value with cons, always consider () for the value of the terminating line
;;;  ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;Y
;;;
;;; What does (tup+ tup1 tup2) do?
;;;         It adds the first number of tup1 to the first number of tup2,then it adds the second number of tup1 to the second number of tup2,etc
;;;               building a tup of answers,for tups of the same length
;;; What is the unusal about tup+?
;;;         It looks at each element of two tups at the same time,or in other words, it recus on two tups.
;;;
;;; If you recur on one tup how many questions do you have to ask?
;;;         two,they are (null? tup) and else
;;;
;;; When recuring on two tups, how many question need to be asked about the tups? 
;;;      Four: if the first tup is empty or none-empty,and if the second tup is empty or non-emptyy
;;;      Do you mean the questions (and (null? tup1) (null? tup2)) (null? tup1) (null? tup2) and else 
;;;
;;;      But can the first tup be () at the same time as the second is other than ().
;;;         No ,because the tups must have the same length
;;;     Does this mean (and (null? tup1) (null? tup2))  and else are the only questions we need to ask?
;;;     Yes!
(define tup+
  (lambda (tup1 tup2)
    (cond
      ((and  (null? tup1) (null? tup2))
       (quote ()))
      (else
        (cons (+ (car tup1) (car tup2))
              (tup+ (cdr tup1) (cdr tup2)))))))
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;;;  Why  does the natural recursion include the cdr of bouth arguments?
;;;      Because the typical element of the final value use the car of both tups,so now
;;;      we are ready to consider the rest of both tups -----------------------------<<<<<<Yes the right logic
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
(define tup++
  (lambda (tup1 tup2)
    (cond
      ((and (null? tup1) (null? tup2)) 
       (quote ()))
      ((null? tup1) tup2)
      ((null? tup2) tup1)
      (else
        (cons (+ (car tup1) (car tup2))
              (tup++  
                (cdr tup1) (cdr tup2)))))))

;;;; can you simplify it????????????????????????????////
(define tup+++
  (lambda (tup1 tup2)
    (cond
      ((null? tup1) tup2)
      ((null? tup2) tup1)
      (else
        (cons (+ (car tup1) (car tup2))
              (tup+++  
                (cdr tup1) (cdr tup2)))))))



(define atom-to-function
  (lambda (x)
    (cond 
      ((eq? x (quote +)) +)
      ((eq? x (quote *)) *)
      ((eq? x (quote ^)) expr)
      ((eq? x (quote eq?)) eq?)
      (else -))))
; (rember-f (atom-to-function 'eq?) 'ff '(dfds gi ff sd))  
;
; In the common lisp:
;            L: (funcall test? (car l) a) ,use the funcall when invoking a function argument or a function that has 
;            not been defuned
(define rember-f
  (lambda (test? a l)
    (cond 
      ((null? l) (quote ()))
      (else
       (cond 
         ((test? (car l) a) (cdr l));;I think here needs atom2function
         (else (cons (car l)
                     (rember-f test? a 
                               (cdr l)))))))))

;;; The short version! 
(define rember1-f
  (lambda (test? a l)
    (cond 
      ((null? l) (quote ()))
      ((test? (car l) a) (cdr l));;I think here needs atom2function
      (else (cons (car l)
                     (rember-f test? a 
                               (cdr l)))))))
;;; What kind of values can functions return>
;;;                  Lists and atoms
;;;                       But in the eye of us ,all the things are the lists and atoms
;;;                        so we can return all the kind of values
;;;                        so what about function themselves? Yes
;;;                         such as (lambda (a l) ...) is a function of two arguments ,a and l! returns the function!
;;;                 This is calld "Curry-ing"
;;;     In the scheme ,Using (define ...)give the preceding function a name,
;;;     but in theCL: (define eq?-c  (a) (function (lambda (x) (eq x a))))
;;;     while the scheme : (define eq?-c (lambda (a) (lambda (x) (eq? x a))))

(define insertL-f
  (lambda (test?)
    (lambda (new old l)
      (cond 
        (( null? l) (quote ()))
        ((test? (car l) old)
         (cons new (cons old (cdr l))))
        (else (cons (car l)
                    ((insertL-f test?) new old
                                       (cdr l))))))))
(define insertR-f
  (lambda (test?)
    (lambda (new old l)
      (cond 
        (( null? l) (quote ()))
        ((test? (car l) old)
         (cons old (cons new (cdr l))))
        (else (cons (car l)
                    ((insertR-f test?) new old
                                       (cdr l))))))))
;;The basic principle
;;(name -->(null? --->(eq?    --->single (cons new (cdr l))    -->(else
;;                            --->multi (cons new (name (cdr l)))-->(else
;;                --->(eqan?   --->single (cons new (cdr l))    -->(else
;;                            --->multi (cons new (name (cdr l)))-->(else
;;                --->(equal?  --->single (cons new (cdr l))    -->(else
;;                            --->multi (cons new (name (cdr l)))-->(else
;;                --->(eqlist? --->single (cons new (cdr l))    -->(else
;;                            --->multi (cons new (name (cdr l)))-->(else
;;sometimes we abstuct the condition to adapt to different state
;; sometimes we abstuct the operations to simplify it
(define multirember1 
 (lambda (a lat)
   (cond
     ((null? lat) (quote ()))
     ((eq? (car lat) a)
      (multirember a (cdr lat)))
     (else (cons (car lat)
                 (multirember a (cdr lat)))))))
(define multirember1-f
  (lambda (test?)
    (lambda (a lat)
      (cond
        ((null? lat) (quote ()))
        ((test? a (car lat))
         ((multirember1-f test?) a 
                                (cdr lat)))
        (else (cons (car lat)
                    ((multirember1-f test?)  a 
                                            (cdr lat))))))))
;;((multirember1-f (atom-to-function 'eq?)) 'fd '(sgs fd gasdg fd))
;;'(sgs gasdg)
;;(define multirember1-eq (multirember1-f (atom-to-function 'eq?)))
;;Actually:
;;((multirember1-f eq?) 'fd '(gsd fd gjsi fd go))
;;direct apply function eq?,That doesn't need (eq?, I learn from
;; (multiremberT eq?-salad)
(define rembern
  (lambda (a l)
    ((insert-g seqrem) #f a l)));;seqrem doesn't need (seqrem solve my ?.
(define seqrem
  (lambda (new old l) 
    l))

(define multiremberT
  (lambda (test? lat)
    (cond
      ((null? lat) (quote ()))
      ((test? (car lat))
       (multiremberT test? (cdr lat)))
      (else
       (cons (car lat)
             (multiremberT test? 
                           (cdr lat)))))))
(define eq?-c
  (lambda (a)
    (lambda (x)
      (eq? x a))))
(define eq?-salad (eq?-c 'salad))
;;(multiremberT eq?-salad '(fsfd salad fsdf sald sg salad))

;;; What is the name of the third arguments that multiremberco uses for the natural recursion?
;;; what is the name of the third argument ? And do you know what col stands for?
;;;       The name col is short for "collector", A collector is sometimes called a continuation
(define multiremberco
  (lambda (a lat col)
    (cond 
      ((null? lat)
       (col (quote ()) (quote ())))
      ((eq? (car lat) a)
       (multiremberco a 
                      (cdr lat)
                      (lambda (newlat seen) 
                        (col newlat (cons (car lat) seen)))))
      (else
       (multiremberco a 
                      (cdr lat)
        )             (lambda (newlat seen)
                        (col (cons (car lat) newlat)
                             seen)))))))

(define a-friend
  (lambda (x y)
    (null? y)))
;(multiremberco 'tuna '(sfds tuna fsd jif) a-friend)
;#f
;> (multiremberco 'tuna '(sfds tuna1 fsd jif) a-friend)
;#t
;;how can  you get another value in the col
;;
;;can you write this definition differently!  Do you mean the new way where we put tuna into the definition?
;;  col is to a-frined what (car lat) is to tuna.   so we can replace the col with a-frined
;;
(define new-friend
  (lambda (newlat seen)
    (a-friend newlat
         (cons (quote tuna) seen))))

;;;; So ANd now?
;;;      multiremberco finds out that(null? lat) is true, which means that it uses the collector on two empty lists.
;;;        which collector is this?
;;;          It is new-friend.
;;; how does a-friend differ from new-friend ?
;;;              New-friend uses a-friend on the empty list and 
;;;               the value of (cons (quote tuna) (quote ()))
;;;
;;; And what does the old-collector do (a-friend) with such arguments?
;;;              It answers #f,because its second argument is (tuna),which is not the empty list!

;;;;;;;; I don't know that the list length ,so easy but I don't think it clearly!!!!
;;;What is terminal condition ?  (null? lat)
;;;what is terminal condition line ? ((null? lat) 0)   (terminal_contition terminal_value)
;;;what is the (+1 (length (cdr lat)))
;;;        build the number with +,so the terminal condition line is ((null? lat) 0)   use (cdr lat) to closer the terminal!
(define length
  (lambda (lat)
    (cond 
      ((null? lat) 0)
      (else (+ 1 (length (cdr lat)))))))

;;; a is tuna and lat is (and tuna)  col is a-friend
(define latest-friend
  (lambda (newlat seen)
    (a-friend (cons (quotee and) newlat)
              seen))))

;;The third collector : the last-friend
(define last-friend
  (lambda (newlat seen)
    (length newlat)))
;;;;;;;;;;;;;****************************************************************************
;;;;;;;;;;;;;****************************************************************************
;;;;;;;;;;;;;****************************************************************************
;;;;So  what does (multiremberco a lat f) do?
;;;      It looks at every atom of the lat to see whether 
;;;      it is eq? to a.Those atoms that are  not  are collected in one list ls1
;;;      it is eq? to a.The others which the answer is true    are collected in second list ls2
;;;
;;;      Finally  it determine the value of (f ls1 ls2)
;;;
;;;      Namely under the process of time(timestep=0,1,2,3,n-1),In the col it classify the data: use cons 
;;;              while at the last time(timestep=n) in the a-friend (col) it determine the cons datas!!

;;;;;;;;;;;;;****************************************************************************
;;;;;;;;;;;;;****************************************************************************
;;;;;;;;;;;;;****************************************************************************
;;;;;;;;;;;;;****************************************************************************
;;;        The Tenth commandments:
;;;            Build functions to collect more than one value at a time.
;;;
;;;;;;;;;;;;;****************************************************************************
;> (multiremberco 'tuna '(tunanfdfj tuan jsidjf tuna) last-friend)
;3

;;actually
;;The basic principle
;;(name -->(null? (col (quote()...)--->(eq?    --->single (cons new (cdr l))    -->(else
;;                            --->multi (cons new (name (cdr l)))-->(else
;;                --->(eqan?   --->single (cons new (cdr l))    -->(else
;;                            --->multi (cons new (name (cdr l)))-->(else
;;                --->(equal?  --->single (cons new (cdr l))    -->(else
;;                            --->multi (cons new (name (cdr l)))-->(else
;;                --->(eqlist? --->single (cons new (cdr l))    -->(else
;;                            --->multi (cons new (name (cdr l)))-->(else
;;sometimes we abstuct the condition to adapt to different state
;; sometimes we abstuct the operations to simplify it
;;multiremboco sometimes we want to change the return value,so co it

(define multiinsertLR
  (lambda (new oldL oldR lat)
    (cond
      ((null? lat) (quote ()))
      ((eq? (car lat) oldL)
       (cons new 
             (cons oldL
                   (multiinsertLR new oldL oldR
                                  (cdr lat)))))
      ((eq? (car lat) oldR)
       (cons oldR
             (cons new
                   (multiinsertLR new oldL oldR
                                 (cdr lat)))))
      (else
        (cons (car lat)
              (multiinsertLR new oldL oldR (cdr lat)))))))

;;;; The function multiinsertLRco is to multiinsertLR what multiremberco is to multirember
;;;           Does this mean that multiinsertLRco takes one more argument than multiinsertLR?
;;;            Yes! and what kind of argument is it? 
;;;                   It is a collector function.
(define multiinsertLRco
  (lambda (new oldL oldR lat col)
    (cond
      ((null? lat)
       (col (quote ()) 0 0))
      ((eq? (car lat) oldL)
       (multiinsertLRco new oldL oldR
                        (cdr lat)
                        (lambda (newlat L R)
                          (col (cons new 
                                     (cons oldL newlat))
                               (+ 1 L) R))))
      ((eq? (car lat) oldR)
       (multiinsertLRco new oldL oldR
                        (cdr lat)
                        (lambda (newlat L R)
                          (col (cons oldR
                                     (cons new newlat))
                               L (+ 1 R)))))
      (else
       (multiinsertLRco new oldL oldR 
                        (cdr lat)
                        (lambda (newlat L R)
                          (col (cons (car lat) newlat)
                               L R)))))))

(define even?
  (lambda (n)
    (= (* (/ n 2) 2) n)))

(define /  ;rewrite the divide to replace the origianl divide from Drracket
  (lambda (n m)
    (cond
      ((< n m) 0)
      (else
       (+ 1 (/ (- n m) m))))))
(define evens-only*
  (lambda (l)
    (cond
      ((null? l) (quote ()))
      ((atom? (car l))
       (cond
         ((even? (car l))
          (cons (car l)
                (evens-only* (cdr l))))
         (else
          (evens-only* (cdr l)))))
      (else (cons (evens-only* (car l))
                  (evens-only* (cdr l)))))))
(define atom?
  (lambda (x)
    (and (not (pair? x)) (not (null? x)))))

;;;  Can you explain what (evens-only*co (carl) ...) accomplishes?
;;;      It visits every number in the car of l and collects the list without odd numbers,the
;;;      product of the even numbers,and the sum of the odd numbers.
;;;      So it is the most complicated,actually not very easy to be understood!
;;;         You can understand with another lat,so you how to associate two lat!
;;;         Youknow (even? (car l)) is in  a lat,so it do +*cons in the lat's number!
;;;
(define evens-only*co
  (lambda (l col)
    (cond
      ((null? l)
       (col (quote ()) 1 0))
      ((atom? (car l))
       (cond 
         ((even? (car l))
          (evens-only*co (cdr l)
                         (lambda (newl p s)
                           (col (cons (car l) newl);if it is evens ,so cons it into the newl
                                (* (car l) p) s))))
         (else (evens-only*co (cdr l)
                              (lambda (newl p s)
                                (col newl p (+ (car l) s)))))))
       (else (evens-only*co (car l)
                            (lambda (al ap as)
                              (evens-only*co (cdr l)
                                             (lambda (dl dp ds)
                                               (col (cons al dl)
                                                    (* ap dp)
                                                    (+ as ds))))))))))
;the fourth collector :: the last-last-friend
(define the-last-last-friend
  (lambda (newl product sum)
    (cons sum 
          (cons product 
                newl))))
;> (evens-only*co '(45389 63 45 6 4 234  6 4 23 52 43) the-last-last-friend)
;'(45563 7008768 6 4 234 6 4 52)

;;;;  after the length !!!!(define length) -------------------------------------<
;;;  use car !!   zero?  sub1
(define pick
  (lambda (n lat)
    (cond
      ((zero? (sub1 n)) (car lat))
      (else (pick (sub1 n) (cdr lat))))))

(define rempick
  (lambda (n lat)
    (cond 
      ((zero? (sub1 n)) (cdr lat))
      (else (cons (car lat)
                  (rempick (sub1 n) (cdr lat))))
      ;;;; because n  lat is occur in the terminal condition!!!!!
;;; use the function one? to rewrite the function rempick that removes the nth atom from a lat.
;;;
;;;
(define rempick1
  (lambda (n lat)
    (cond 
      ((one? n) (cdr lat))
      (else (cons (car lat)
                  (rempick ((sub1 n)
                            (cdr lat))))))))


(define keep-looking
  (lambda (a sorn lat)
    (cond
      ((number? sorn)
       (keep-looking a (pick sorn lat) lat))
      (else (eq? sorn a)))))


(define looking
  (lambda (a lat)
    (keep-looking a (pick 1 lat) lat)))
;> (looking 'cavier '(6 2 4 cavier 5 7 3))
;#t

(define eternity
  (lambda (x)
    (eternity x)))
;;it doesn't stop ,because the goal is not absolutely, rather then
;; 相对的

;;; This is trivival; It's not even recursive
;;; What does shift do?
;;;   Here are our words:
;;;     The shift function takes a pair whose first component is a pair and builds a pair
;;;     by shifting the second part of the first component into the second component!
;;;
;;;     So what is the meaning of component?  --------Component can means atom,pair and list
;;;                  Now it means pair.
(define shift 
  (lambda (pair)
    (build (first (first pair))
           (build (second (first pair))
                  (second pair)))))
;> (shift '((fsd fd)(fd gs)))
;'(fsd (fd (fd gs)))
(define align
  (lambda (pora)
    (cond 
      ((atom? pora) pora)
      ((a-pair? (first pora))
       (align (shift pora)))
      (else (build (first pora)
                   (align (second pora)))))))
;;; What does it  have in common with keep-looking 
;;;          Both functions change their arguments for their recursive uses but in neither
;;;          case is the change guaranteed to get us closer to the goal
;;;
;;; Why are we not guaranteed that align makes progress?
;;;                In the second cond-line shift creates an argument for align that 
;;;                is not a part of the original argument.
;;;
;;; Which commandment does that violate?
;;;              The Seventh Commandment.
;;;
;;; Is the new argument at least  smaller than the original one?
;;;            It does not look that way
;;;
;;; Why not?
;;;           The function shift only rearranges the pair it gets.
;;; And?
;;;           Both the result and the argument of shift have the same number of atoms.
;;; Can you write a function that counts the number of atoms
;;; After the (define intersectall)
;;;
;;;align do what shift can't do?
;;;        Such as  (align '(a b))         but (shift '(a b)) will get wrong message.
(define a-pair?
  (lambda (x)
    (cond
      ((atom? x) #f)
      ((null? x) #f)
      ((null? (cdr x)) #f)
      ((null? (cdr (cdr x))) #t)
      (else #f))))
;;; how can you refer to the first S-expression of a pair?
;;;      By taking the car of the pari
;;; How can you refer to the second S-expression of a pair?
;;;      By taking the car of the cdr of the pair
;;;
;;; How can you build a pair with two atoms?
;;;  You cons the first ne onto the cons of the second one onto (). Thatis (cons x1 (cons x2 (quote ())))
;;;
;;;  How can you build a pair with two S-expressions?
;;;   you cns the first one onto the cons of the second one onto ().That is
;;;   (cons s1 (cons s2 (quote ()))) or (cons x1 (cons x2 (quote ())))
;;;
;;;   Did you notice the difference between the last two answers?
;;;     No ,there aren't any
;;;
;;;Is align a partial function?
;;;        We don't now yet.There may be arguments for which it keeps aligning things.
;;;
;;; Is there something else that changes about the arguments to align and its recursive uses?
;;;        Yes, there is .The first component of a  pair becomes simpler,though the second component becomes more complicated
;;;
;;; In what way is the first component simpler?
;;;       IT is only a part (IT intelligent technique   is equal to it) of the original pair's first component.
;;;
;;; Doesn't this mean that lenght * is the wrong function for determining the length of the argument? Can you find a better function
;;;         A better function should pay more attention to the first component
;;;
;;; How much more attention should we pay to the first component?
;;;          At least twice as much
;;;          Do you mean something like weight*
;;;     DOes This mean that the arguments get simpler?
;;;       Yes ,the weigth* of align's arguments become successively smaller
;;;     Is align a partial function?
;;;        No,it yields a value for every argument rather than the partial function looking and looking again.
;;;
(define length*
  (lambda (pora)
    (cond 
      (( atom? pora) 1)
      (else
       (+ (length* (first pora))
          (length* (second pora)))))))
(define weight*
  (lambda (pora)
    (cond 
      ((atom? pora) 1)
      (else
       (+ (* (weight* (first pora)) 2)
          (weight* (second pora)))))))
;;;  Here is shuffle which is like align but uses revpair from chapter7, instead of shift:(when the first component is a-pair?
;;;       THen do revpair.
;;; The function shuffle and revpair swap the components of pairs when the first component is a pair...
;;;
;;; Does this mean that shuffle is total?
;;;        We don't know yet.(actually  it is not.)
;;; Okay,let's do something interesting. What is the value of (shuffle x) where x is ((a b) (c d))
;;;
;;; And how are you going to do that?
;;;          We are going to determine the value (shuffle pora) where pora is ((c d) (a b))
;;;
;;; Doesn't this mean that we need to know the value of (shuffle (revpair pora))
;;;            where (revpair pora) is ((a b) (c d))  , yes we do.
;;;
;;; And?
;;;         The function shuffle is not total because it now swaps the components of the pair again, which means that we start all over.
(define shuffle
  (lambda (pora)
    (cond
      ((atom? pora) pora)
      ((a-pair? (first pora))
       (shuffle (revpair pora)))
      (else (build (first pora)
                   (shuffle (second pora)))))))
;> (shuffle '(a (b c)))
;'(a (b c))
;> (shuffle '(a b c))
;'(a b)
;> (shuffle '(a b))
;'(a b)
;;;;         Thankyou ,Lother Collatz(1910-1990)! It doesn't yield a value for 0, but otherwise nobody knows.
(define C
  (lambda (n)
    (cond
      ((one? n) 1)
      (else
        (cond
          ((even? n) (C (/ n 2)))
          (else (C (add1 (* 3 n)))))))))

;;;;          Thank you,Wilhelm ackermann(1853-1946)
(define A
  (lambda (n m)
    (cond
      ((zero? n) (+ 1 m))
      ((zero? m) (A (- 1 n) 1))
      (else (A (sub1 n)
               (A n (sub1 m)))))))
;;; What does A have in common with shuffle and looking?
;;;         A's arguments ,like shuffle's and looking's,  do not necessarily decrease for the recursion
;;;
;;; Dose A always give an answer? 
;;;         Yes,it's total
;;;
;;; Wouldn't it be great if we could write a function that tells us whether some function returns with a 
;;; value for every  argument.
;;;        It sure would.Now that we have seen function that never return a value or return a value so lat that 
;;;        it is too late ,we should have some tool like this around
;;;Okay,let's write it
;;;
;;;            It sounds complicated.A function can work for many different arguments
;;; Then let's make it simpler.For a warm-up exercise,let's focus on a function that checks whether some function stops
;;; for just the empty list,the simplest of all arguments.
;;;          That world simplify it a lot
;;; Here is the beginning of this function:
;;;
;;; (define will-stop?  (lambda (f) ...)))
;;;
;;;
;;;
;;; What does it do?  Do will-stop? return a value for all arguments?
;;; That's the easy part: we said that it either returns #t or #f,depending on whether the argument stops when applied to ()
;;;
;;; Is will-stop? total then?
;;;   Yes,it is.It always return #t or #f
;;;
;;;okay here is  a function that could be an interesting argument for will-stop?
(define last-try
  (lambda(x)
    (and (will-stop? last-try)
         (eternity x)))))

    ;;; what is (will-stop? last-try)
    ;;;
    ;;; What does it do?
    ;;;
    ;;; Here is our meaning:
    ;;;    "we took a really close look at the two possible cases.IF we can define will-stop?,
    ;;;     then (will-stop? last-try) must yield either #t or #f.Bu it cannot- due to the very
    ;;;     definition of what will-stop? is supposed to do.This must mean that will-stop? cannot 
    ;;;     be defined"
    ;;;
    ;;;     Because you know,let's make up some examples.
    ;;;     what is the value of (will-stop? last-try) (eternity (quote ()))))
    ;;;         That depends on the value of (will-stop? last-try)
    ;;;
    ;;;         we said that will-stop? will stop,so it must return #t or #f,
    ;;;         let's say (will-stop? last-try) is #f ,yes,itis .we predict just the opposite
    ;;;            if the value of (will-stop? last-try) is #f,which really means that last-try
    ;;;            will not stop..
    ;;;        So we must have been wrong about (will-stop? last-try) (because it cannot be stopped)
    ;;;               That's correct.It must return #t,because will-stop? always gives an answer. we
    ;;;               said it total(assume)
    ;;;         Fine. If(will-stop? last-try) is #t. What is the value of (last-try (quote ())) in the definition of last-try.
    ;;;           Now we just need to determine the value of (and #t (eternity (quote ()))
    ;;;           which is the same as the value of (eternity (quote ()))
    ;;;
    ;;;           What is the value of (eternity (quote ())) 
    ;;;                 It doesn't have a value.we know that it doesn't stop
    ;;;                 !
    ;;;         But that means that we were wrong again!!! The will-stop? neither return #f nor #t. What the hell
    ;;;                will-stop? will return?
    ;;;
    ;;;         So will-stop? cannot be defined.
    ;;;         Is this unique?
    ;;;                Yes , it is .It makes will-stop? the first function that we can describe percisely but cannot 
    ;;;                define in our language.
    ;;;
    ;;;         Is there any way around this problem?
    ;;;                No ,thank you  ,Alan M. Turing (1912-1954)
    ;;;                and Kurt Godel(1906-1978)
    ;;;
    ;;;         What is (define...)
    ;;;                 That is an interesting question. We just saw that (define...) doesn't work for will-stop?.
    ;;;
    ;;;             So what are the recurive definition?
    ;;;               Hold tight,take a deep breath,and plunge forward when you're ready.


;;;;;;;;;;;;;;;;;;;;;;;The second part;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;;;;;;;;;;;;;;;;;;;;;;;****************;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;;;
;;;
;;;
;;;;;;;;;;;;;;;;;;;;;;;The third part;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;;;;;   myracet3.rkt;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;

(define length 
  (lambda (l)
    (cond 
      ((null? l) 0)
      (else (+ 1 (length (cdr l)))))))
(define eternity
  (lambda (x)
    (eternity x)))
(lambda (l)
    (cond 
      ((null? l ) 0)
      (else (+ 1 (eternity (cdr l))))))
;length<=1
(lambda (l)
    (cond 
      ((null? l) 0)
      (else 
       (+ 1 
          ((lambda (l)
             (cond 
               ((null? l) 0)
               (else (+ 1 
                        (eternity (cdr l))))))
           (cdr l))))))

;length <=2
(lambda (l)
    (cond 
      ((null? l) 0)
      (else
       (+ 1
        ((lambda (l)
           (cond
             ((null? l) 0)
             (else 
              (+ 1
                 ((lambda (l)
                    (cond
                      ((null? l) 0)
                      (else
                       (+ 1
                          (eternity
                           (cdr l))))))
                  (cdr l))))))
         (cdr l))))))

;;; Now what do you thin recursion is?
;;;           What do you mean?
;;;
;;; Well,we have seen how to determine the length of a list with no items,with no more that
;;; one item,with no more than two items,and so on. How could we get the function length back.
;;;           If we could write an infinite function in the style of length0 ,length1,length2..
;;;           then we could write lengthinfinite,which would determine the length of all lists
;;;           we can make.
;;;     But we cannot write an infinite function. No we can't, (yes ,we can't mae it)
;;;
;;;      And we still have all these repetitions and patterns in these functions. yes we do.
;;;
;;;      What do these pattern look like?
;;;               All these programs contain a function that looks like length.Perhaps we should
;;;               abstract out this function: see The Ninth Commandment.
;;;     We need a function that looks just lie length but starts with (lambda (length) ...).
;;;
;;;Do you mean this?
;;;((lambda (length)
;;;(lambda (l)  (cond ((null? l) 0)  (else (add1 (length (cdr l)))))) eternity)
;;;       Yes,that's okay. It creates length0
;;;so rewrite length1  in the same style
;;;See below.
;;;
;;;
;we need a function that looks just like length 
;but starts with (lambda (length)...) and inside it is not 
;(lambda (length) but maybe (lambda (l)
;length0
(lambda (legnth)
  (lambda (l)
      (cond 
        ((null? l) 0)
        (else (add1 (length (cdr l))))))
    eternity)


;length1
((lambda (f)
    (lambda (l)
      (cond 
        ((null? l) 0)
        (else 
         (+ 1 (f (cdr l)))))))
   ((lambda (g)
      (lambda (l)
        (cond 
          ((null? l) 0)
          (else (+ 1 (g (cdr l)))))))
    eternity))

;;  Do we have to use length to name the argument?
;;         No ,we just used f and g. As long as we are consistent,everything's okay
;length2
;;length <=2
((lambda (length)
   (lambda (l)
     (cond
       ((null? l) 0)
       (else (+ 1 (length (cdr l)))))))
 ((lambda (length)
    (lambda (l)
      (cond
        ((null? l) 0)
        (else (+ 1 (length (cdr l)))))))
  ((lambda (length)
     (lambda (l)
       (cond
         ((null? l) 0)
         (else (+ 1 (length (cdr l)))))))
   eternity)))

;;; close,  But there are still repetitions.  True,let's get rid of it.
;;;
;;;  Where should we start?
;;;         Name the function that takes length as an argument and that returns a function that looks like length.
;;;  What's a good-name for this function?
;;;           how about mk-length  for "make length"
;;;
;let eternity move ahead , eternity in the first part
((lambda (mk-length)
   (mk-length eternity))
 (lambda (length)
   (lambda (l)
     (cond 
       ((null? l) 0)
       (else (+ 1 (length (cdr l))))))))

((lambda (mk-length)
   ;;提取池
   (mk-length 
    (mk-length eternity)))
 ;;发生池
 (lambda (length)
   (lambda (l)
     (cond 
       ((null? l) 0)
       (else (+ 1 (length (cdr l))))))))
;;length<=2
((lambda (mk-length)
   ;;提取池
   (mk-length 
    (mk-length 
     (mk-length eternity))))
 ;;发生池
 (lambda (length)
   (lambda (l)
     (cond 
       ((null? l) 0)
       (else (+ 1 (length (cdr l))))))))
 ;;length <=3
 ((lambda (mk-length)
   ;;提取池
   (mk-length 
    (mk-length 
     (mk-length
      (mk-length eternity)))))

  ;;;;              So   What is recursion like?
  ;;;                             It is like an infinite tower of applications of mk-length to ab arbitrary function.
  ;;;               Do you really need an infinite tower?
  ;;;                      Not really of course.Everytime we use length we only need a finite number,but we never know 
  ;;;                      know many.
  ;;;               could we guess how many we need?
  ;;;                     Sure,but we may not guess a large enough number!
  ;;;               When do  we  find out that we didn't guess a large enough number?
  ;;;                     When we apply the function eternity that is  passed to innermost mk-length.
  ;;;               What if we could create another application of mk-length to eternity at this point?
  ;;;                       That would only postpone the problem by one and besides,how could we do that?
  ;;;
  ;;;                Well,since nobody cares what function we passwd to mk-length we could pass it mk-length initially.
  ;;;                      That's the right idea(right?       )             And then we invoke mk-length on eternity and the 
  ;;;                      result of this on the cdr so that we get one more piece of this tower.
 ;;发生池
 (lambda (length)
   (lambda (l)
     (cond 
       ((null? l) 0)
       (else (+ 1 (length (cdr l))))))))
 
 ;;how if we replace the eternity with the mk-length
 
 ;length0; 因为没有人关心我们到底我们传递什么给mk-length 如果他是无穷的话
 ;那我们可不可以第一次初始化的时候就传递给他 mk-length 来替代eternity呢？
 ;;; Yes,we could even use mk-length instead of length.
 ((lambda (mk-length)
     (mk-length mk-length))
   (lambda (length)
     (lambda (l)
       (cond 
         ((null? l) 0)
         (else (+ 1 
                  (length (cdr l))))))))
 ;;和上面一样
 ((lambda (mk-length)
     (mk-length mk-length))
   (lambda (mk-length)
     (lambda (l)
       (cond 
         ((null? l) 0)
         (else (+ 1 
                  (mk-length (cdr l))))))))
 ;; WHy would we want to do that?
 ;;         All names are equal, but some names are more equal than others! (with apologies to George Orwell(1903-1950)
 ;;         True: as long as we use the names consistently ,we are just fine.
 ;;
 ;;     And mk-length is a far more equal  name than length. If we use a name like mk-length, it is a  contstant reminder that
 ;;      the first argument to mk-length is mk-length
 
 ;;我们为什么想着上面的做法呢？
 ;;因为所有的名字都是一样的，但是有些名字是更一样比其他
 ((lambda (mk-length)
    (mk-length mk-length))
  (lambda (mk-length)
    (lambda (l)
      (cond 
        ((null? l) 0)
        (else (add1 
               ((mk-length eternity)
                (cdr l))))))))
 
 ((lambda (mk-length)
    (mk-length mk-length))
  (lambda (mk-length)
    ((lambda (length)
       (lambda (l)
         (cond
           ((null? l) 0)
           (else (add1 (length (cdr l)))))))
     (mk-length mk-length))))

;;; How about this?
;;;     Yes,this looks just fine.
;;; Let's see thether it works?
;;;      Yes ,okay
;;; First,we need to know the value of it?

 
 ;;; what would  you call this  function above?
 ;;;        It's length,of course
 ;;;
 ;;;    How does it work?
 ;;;           It keeps adding recurive uses by  passing mk-length to itself,just as it is about to expire
 ;;;
 ;;;           One problem is left: it  no longer contains the function that looks like length.
 ;;;
 ;;;           can you fix the (mk-length mk-length) .?
 ;;;            We could extract this new application of mk-length to itself and call it length
 ;;;        WHy?
 ;;;             Because it really makes the function length.
 
 ;;since (mk-length mk-length returns a fucntion of one argument,
 ;;does (lambda (x) ((mk-length mk-length) x)) retrun a function of
 ;;one argument? Actually,it is a function
 ;;
 ;;Okay ,let's do this to the application of mk-length to itself
  ((lambda (mk-length)
    (mk-length mk-length))
  (lambda (mk-length)
    (lambda (l)
      (cond 
        ((null? l) 0)
        (else (add1 
               ((lambda (x)
                  ((mk-length mk-length) x))
                (cdr l))))))))
  
  ;;move out the new function so that we get length back
  ;;Why can't you move out (lambda (x) ((mk-length mk-length) x))  
  ;;       
  ;;            Because you add the (lambda (length)  which is like the definition of define.(actually are)
  ;;            also here the meaning of length is  (lambda(x) ((mk-length mk-length) x)),so you can use the
  ;;            usage of lambda(length) and the definition of length in the same S-expressions!!(By zhaoliang)
  ;;                    You know the same S-expression (the same parathese here)
  ;;
  ;;            Is it okay to move out the function?
  ;;                     Yes,we just always did the opposite by replacing a name with its value. Here we
  ;;                     extract a value and give it a name.
  ;;            Can we extract the function in the box(p171) that looks like length and give it a name
  ;;                          Yes,it does not depend on mk-length at all!
  ((lambda (mk-length)
     (mk-length mk-length))
   (lambda (mk-length)
     ((lambda (length)
        (lambda (l)
          (cond 
            ((null? l) 0)
            (else
             (add1 (length (cdr l)))))))
      (lambda (x)
        ((mk-length mk-length) x)))))


;;;;;;;;;;;;;;;;;;;;;;;The third part;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;;;;;;;;;;;;;;;;;;;;;;;***************;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;;;;;;;;;;;;;;;;;;;;;;;The fourth part;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;;;;;;  myracet4.rkt ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;

;;;; Is this the right function!             ------you can check back the nottation above.
;;;
;;;Here the definition of le(lambda (length)...) and the usage of le (lambda (le)...) is in the same 
;;;S-expressions!!(the same parathese)
;;;
((lambda (le)
   ((lambda (mk-length)
      (mk-length mk-length))
    (lambda (mk-length)
      (le (lambda (x)
            ((mk-length mk-length) x))))))
 (lambda (length)
   (lambda (l)
     (cond
       ((null? l) 0)
       (else (add1 (length (cdr l))))))))

;;;; Let's separate the funtion that makes length from the function that looks like length.
(lambda (le)
  ((lambda (mk-length)
     (mk-length mk-length))
   (lambda (mk-length)
     (le (lambda (x)
           ((mk-length mk-length) x))))))
;;; Does this function have a name?
;;;         Yes ,it is called the application-order Y combinator.
(define Y
  (lambda (le)
    ((lambda (f) 
       (f f))
     (lambda (f)
       (le (lambda (x)
             ((f f) x)))))))
;;; the above function is replace the mk-length with  f ,so it looks more common!      BUt
;;;      it lacks the definition of (le)  just the usage of le.so it means the Y combinator.
;;;
;;;      (lambda (length)
;;;            (lambda  (l)
;;;                 ...           ---< it is the definition of function length
;;;
;;;     ((lambda  (le)
;;;          ...
;;;        )
;;;     (lambda (length)
;;;         ...
;;;         ))
;;;                             ----<  it is the definition of function le is the (lambda (length)...)) and the use 
;;;                             of the le is in the (lambda (le) ...)   Take care what the left parathese and right 
;;;                             parathese I utilize.
;;;                             ----------------------which is similar to apply-closure that contains three part
;;;                                                            table-of   form-of    body-of


;; An entry is a pair of lists whose first list is a set. Also ,the two lists must be of equal 
;; length.Make up some examples for entries
;;              Here are some examples:
;;              ((appetizer entree beverage)
;;              (pate boeuf vin))
;;
;;              and
;;              ((appetizer entree beverage)
;;              (beer beer beer)  ---------------------------< but the second list can be not a set
;;
;;              and 
;;              (( beverage dessert)
;;               ((food is) (number one with us)))
;;
;;
;;How can we build an entry from a set of names and a list of values?
;;           (define new-entry build)           ---->Try to build our examples with this function.
;;
;;What is (lookup-in-entry name entry)
;;     where name is entree
;;
;;     and entry is ((appetizer entree beverage)
;;                   (food tastes  good))
;;                ------------------------------>tastes
;;
;;      What if name is dessert
;;             In this case we would like to leave the decision about what to do with the user of lookup-in-entry
;;      How can we accomplish this?
;;             lookup-in-entry takes an additional argument this is invoked when name is not found in the first 
;;                list of an entry.
;;
;;      How many arguments do you think this extra function should take?
;;            We think it should take one , name. Why?
;;
;;;;;;;;;;;;;;;;;;;;;;;The fourth part;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;;;;;;;;;;;;;;;;;;;;;;;***************;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;;;;;;;;;;;;;;;;;;;;;;;The fifth part;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;;;;;myracet5.rkt ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;;;
;;;

(define atom?
  (lambda (x)
    (and (not (pair? x)) (not (null? x)))))
  
(define build
  (lambda (s1 s2)
    (cond
      (else (cons s1
                  (cons s2 (quote ())))))))
((lambda (nothing)
     (cond
       (nothing (quote something))
       (else (quote nothing))))
   #t)

;;;; Here is our dfinition of lookup-in-entry:
(define lookup-in-entry
  (lambda (name entry entry-f)
    (lookup-in-entry-help name
                          (first entry)
                          (second entry)
                          (entry-f))))
;;;Finish the function lookup-in-entry-help-----------------
;;;
;;;
(define lookup-in-entry-help
  (lambda (name names values entry-f)
    (cond
      ((null? names) (entry-f name))
      ((eq? (car names) name)
       (car values))
      (else (lookup-in-entry-help name
                                  (cdr names)
                                  (cdr values)
                                  entry-f)))))
;;;     A table(based on entries) (also   called an environment) is a list of entries.Here is one example: The empty table
;;;     ,represented by ()
;;;     Make up some others.
;;;      Here is another one:
;;;                          (((appetizer entree beverage)
;;;                            (pate boeuf vin))
;;;                            ((beverage desert)
;;;                             ((food is) (number one with us)))
;;;                           )
;;;
;;;         Define the function extend-table which takes an entry and a table (possibly the empty one)
;;;         and create a new table by putting the new entry in front of the old table.
;;;         (define extend-table cons)
;;;
;;; What is  
;;;         (lookup-in-table name table table-f)
;;;          where 
;;;                 name is entree
;;;                 table is (((entree dessert)
;;;                            (spaghetti spumoni))
;;;                           ((appetizer entree beverage)
;;;                            (food tastes good)))
;;;                 and 
;;;                    table-f is (lambda (name) ...)
;;;
;;;             -----> It could be either spaghetti or tastes,but lookup-in-table searches the list of entries in order
;;;                    So it is spaghetti
;;;
;;; Can you describe what the following function 
;;; represents:
;;;            (lambda (name)
;;;                (lookup-in-table name
;;;                     (cdr table)
;;;                     table-f))
;;;
;;;                     ---------------------------> This function is the action to take when the name is not found in the first entry
;;;                                                  In one table there are a set of entries (set means each of the entries is different 
;;;                                                  from each other)
;;;
;;; Have we chosen a good representation for expressions?
;;;           Yes .They are all S-expressions so they can be data for functions.
;;; What kind of functions?
;;;           For example ,value(later on ,the value is defined as another name  compiler.(like gcc  gfortran  ifort  python perl etc)
;;;
;;; Do you remember value from chapter 6?
;;;          Recall that value is the function that returns the natural value of expressions.
;;;
;;; What is the value of
;;;  (car (quote (a b c)))
;;;         We don't even know  what (quote (a b c)) is .---------------------------------<<<<The original state of you learning english!
;;;                                                                          You don't know 'am' 'are' 'is' etc.
;;;
;;;What is the value of
;;;      (cons rep-a
;;;        (cons rep-b
;;;          (cons rep-c
;;;             (quote ()))))
;;;
;;;     Where 
;;;       rep-a is a
;;;       rep-b is b
;;;     and
;;;       rep-c is c
;;;                                         ==================>> It is the same as (a b c)
;;;
;;;And what is the value of
;;; (cons rep-car
;;;     (cons (cons rep-quote
;;;              (cons 
;;;                  (cons rep-a
;;;                     (cons rep-b
;;;                        (cons rep-c
;;;                             (quote ()))))
;;;                  (quote ())))
;;;       (quote ())))
;;;       where 
;;;         rep-car is car
;;;         rep-quote is quote
;;;         rep-a is a
;;;         rep-b is b
;;;         and
;;;         rep-c is c
;;;                                        ====================>> It is the representation of the expressions:
;;;                                                                      (car (quote (a b c)))
;;;
;;;                                                                      ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^
;;;                                                                      The above is  what I say: Lisp is the data with program!
;;;                                                                              Data can be code,code can also become data!
;;;
;;; What is value of
;;;      (car (quote (a b c)))           
;;;                                       =====================>> a
;;; What is (value e)
;;;      where 
;;;          e is (car (quote (a b c)))
;;;                                       =====================>> a
;;; What is (value e)
;;;       where 
;;;           e is (quote (car (quote (a b c))))
;;;                                       =====================>> (car (quote (a b c)))
;;;
;;; What is (value e)
;;;        where
;;;            e is (add1 6)
;;;                                        ====================>> 7
;;; What is (value e)
;;;        where 
;;;            e is 6
;;;                                        ====================>> 6 .because numbers are constants
;;; What is (value e)
;;;        where 
;;;            e is (quote nothing)
;;;                                        ====================>> nothing
;;; What is (value e)
;;;         where
;;;           e is ((lambda (nothing)
;;;                    (cons nothing (quote ())))
;;;                 (quote 
;;;                    (from nothing comes something)))
;;;                                        ====================>> ((from nothing come something)).
;;;
;;;
;;;
;;; What is (value e)
;;;        where 
;;;            e is ((lambda (nothing)
;;;                     (cond
;;;                        (nothing (quote something))
;;;                        (else (quote nothing))))
;;;                  #t)             -------------------------< It means nothing is #t(The semtatics of language (the infer of language))
;;;
;;;                                       =====================>> something
;;;
;;; What is the type of e
;;;         where  
;;;              e is 6
;;;                                        ====================>> *const
;;; What is the type of e
;;;         where 
;;;              e is #f
;;;                                       ======================>> *const
;;; What is (value e)
;;;         where 
;;;              e is #f                 ======================>> #f
;;;; What is the type of e
;;;         where 
;;;              e is cons
;;;                                       ======================>> *const             ----------------------->I think is must be somethin wrong 
;;;                                                                                   ----------------------->I think it is primitive car
;;;                                                                                   ----------------------->I am wrong ,primitive is also *const
;;;;; What is the type of e
;;;         where 
;;;              e is car
;;;                                       ======================>> (primitive car)
;;;
;;;;;; What is the type of e
;;;         where 
;;;              e is (quote nothing)
;;;                                       ======================>> *quote
;;; What is the type of e
;;;         where 
;;;              e is nothing
;;;                                       ======================>> *identifier
;;;;;; What is the type of e
;;;         where 
;;;              e is (lambda (x y) (cons x y))
;;;                                       ======================>> *lambda
;;; What is the type of e
;;;         where 
;;;              e is ((lambda (nothing)
;;;                      (cond
;;;                          (nothing (quote something))
;;;                          (else (quote nothing))))
;;;                     #t)
;;;                                       ======================>> *application
;;; What is the type of e
;;;         where 
;;;              e is (cond
;;;                      (nothing  (quote something))
;;;                      (else (quote nothing)))
;;;                                       ======================>> *cond
;;;
;;; How many types do you think there are?
;;;                          we found six:
;;;                                         *const
;;;                                         *quote
;;;                                         *identifier
;;;                                         *lambda
;;;                                         *cond
;;;                                         and
;;;                                           *application
;;; How do you think we should represent types?
;;;              We choose functions.We call these functions "action";------------------------------------<In python ,it is called as objects
;;;                                                                   or,in oop(orient object programming) ,it is called as objects
;;;
;;; If actions are functions that do "the right thing"  when applied to the appropriate type of expression,
;;;                 What should value do?
;;;     You guessed it. It would have to find out the type of expression it was passed  and then use the associated action.
;;;
;;; Do you remember atom-to-function from chapter8?
;;;         We found atom-to-function usefule when we rewrote value for numbered expressions.

;;;
;;;
(define lookup-in-table
  (lambda (name table table-f)
    (cond 
      ((null? table) (table-f name))
      (else (lookup-in-entry name
                             (car table)
                             (lambda (name)
                               (lookup-in-table name
                                                (cdr table)
                                                (table-f))))))))
(lambda (name table table-f)
    (lookup-in-table name
                     (cdr table)
                     (table-f)))
;;; Below is a function that produces the correct action (or function) for each possible S-expression:
;;;
;;; The top level make-up
(define expression-to-action
  (lambda (e)
    (cond
      ((atom? e) (atom-to-action e))
      (else (list-to-action e)))))

;;; IN the III-formed S-expressions suchas (quote a b), () , (lambda (#t) #t),(lambda (4) 4)  ,(lambda (car) car)
;;;         (lambda a),(cond (3 c) (else b) (5 a)) , And (1 2) are not considered here.They can be detected by an 
;;;         appropriate function to which S-expressions are submitted before they are passed to value.

(define atom-to-action
  (lambda (e)
    (cond
      ((number? e) *const)
      ((eq? e #t) *const)
      ((eq? e #f) *const)
      ((eq? e (quote cons)) *const)
      ((eq? e (quote car)) *const)
      ((eq? e (quote cdr)) *const)
      ((eq? e (quote null?)) *const)
      ((eq? e (quote eq?)) *const)
      ((eq? e (quote atom?)) *const)
      ((eq? e (quote zero?)) *const)
      ((eq? e (quote add1)) *const)
      ((eq? e (quote sub1)) *const)
      ((eq? e (quote number?)) *const)
      (else *identifier))))
;;;; so *const is consed by three parts: primitive part ,number , #t or #f part.(not include string part, because string part is called *identifier)
;;;
;;;By zhaoliang
;;;It means that the following identifier can see the list as its function's argument!    --------------------------<
(define list-to-action
  (lambda (e)
    (cond
      ((atom? (car e))
       (cond
         ((eq? (car e) (quote quote))
          *quote)
         ((eq? (car e) (quote lambda))
          *lambda)
         ((eq? (car e) (quote cond))
          *cond)
         (else *application)))
      (else *application))))

;;;;; So the compiler is comming on the way............................
;;;       If we assuming the expression-to-action works,we can use it to define value and meanning
;;;
;;;
(define value
  (lambda (e)
    (meaning e (quote ()))))  ;;;Now table is empty set.
(define meaning
  (lambda (e table)
    ((expression-to-action e) e table)))

;;;; The function value, together with all the functions it uses ,is called an interpreter.(Note that: all the relevent function value use
;;;
;;;The function value approximates the function eval  available in Schemes(and Lisp)
;;;
;;;How many arguments should actions take according to the above?
;;;
;;;             Two,the expression e and a table
;;;
;;;
;;; Here is the action for constants:
;;;
;;;
;;;
(define *const
  (lambda (e table)
    (cond 
      ((number? e) e)
      ((eq? e #t) #t)
      ((eq? e #f) #f)
      (else (build (quote primitive) e)))))  ;;;;-----------------<<<< Yes,primitive car,primitive con,primitive cdr

;;; Here is the action for constants:
;;; 
;;;
(define *quote
  (lambda (e table)
    (text-of e)))

(define first
  (lambda (p)
    (cond 
      (else (car p)))))

(define second
  (lambda (p)
    (cond
      (else car (cdr p)))))

(define third
  (lambda (p)
    (cond 
      (else car (cdr (cdr p))))))

(define text-of second)

;;; Have we used the table yet?
;;;                   No, But we will in a moment.
;;;
;;; Why do we need the table?
;;;                   To remember the values of identifiers
;;;
;;;; Given that the table contains the values of identifiers ,write the action of *identifier
;;;
(define *identifier
  (lambda (e table)
    (lookup-in-table e table initial-table)))

;;; Here is the initial-table --------------------------------------------->>>>>
(define initial-table
  (lambda (name)
    (car (quote ()))))

;;; When is it used?
;;;                       Let's hope never. Why?
;;
;;What is the value of (lambda (x) x)
;;         We don't know yet, But we know that it must be the representation for a non-primitive
;;                  function.
;;
;;  How are non-primitive functions different from primitives?
;;   We know what primitives do;  non-primitives are defined by their argumentss and their function bodier
;;                                                     ^^^^^ ^^ ^^^^^ ^^^^^^^^^^ ^^^ ^^^^^ ^^^^^^^^ ^^^^^^
;;                                                     What does it mean?
;;                                                      arguments (x y ...)       function bodier: x
;;  So when we want to use a non-primitive we need to remember its formal arguments and its function body.
;;       At least. Fortunately this is just the cdr of a lambda expression! Because the car of the lambda expression is lambda
;;
;;  And what else do we need to remember?
;;                             
;;                              We wll also put the table in,just in case we might need it later
;;
;;  And How do we represent this?
;;                   In a list,of course.
;;
;; Here is the action *lambda
;;
(define *lambda
  (lambda (e table)
    (build (quote non-primitive)
           (cons table (cdr e)))))
;;; What is (meanning a table)
;;;        where
;;;              e is (lambda (x) (cons x y))
;;;        and
;;;              table is (((y z) ((8) 9)))
;;;
;;;              ==========================================>>> (non-primitive
;;;                                                                   (   (((y z) ((8) 9)))         (x)             (cons x y)   ))
;;;                                                                       ^^^^^^^^^^^^^^^^^         ^^^             ^^^^^^^^^^^^^^^
;;;                                                                           table                 formals             body
;;;It is probably a good idea to define some help functions for getting back the parts in this three elements
;;;list (i.e. the tables,the formal arguments,and the body).
;;;
(define table-of first)
(define formals-of second)
(define body-of third)


;;; Describe (cond...) in your own words
;;;     
;;;      It is a special form that takes any number of cond-lines. It considers
;;;      each line in turn. If the question part on the left is false, it looks
;;;      at the rest of the lines.Otherwise it proceeds to answer the right part.
;;;       If it sees an else-line,it treats that cond-line as if its question part
;;;       were true.

;;;;
;;; Here is the function evcon that does what we said in words
;;;
(define evcon
  (lambda (lines table)
    (cond 
      ((else? (question-of (car lines)))
       (meaning (answer-of (car lines))
                table))
      ((meaning (question-of (car lines))
                table)
       (meaning (answer-of (car lines))
                table))
      (else (evcon (cdr lines) table)))))

;;;; else? determine whether the (car expression)  is else or not!
(define else?
  (lambda (x)
    (cond
      ((atom? x) (eq? x (quote else)))
      (else #f))))
;;; didn't we violate the First Commandment?
;;;          Yes,we don't ask (null? lines), so one of the questions in every cond better be true
(define question-of first)
(define answer-of second)

;;; Now use the function evcon to write the *cond action
;;; 
;;; Here is the action for *cond
;;;
(define *cond
  (lambda (e table)
    (evcon (cond-lines-of e) table)))
;;; Aren't these help functions useful?
;;;          Yes they make things quite a bit more readable.But you already knew that.
;;;
;;; Do you understand *cond now?
;;;           Perhaps not
;;;
;;; How can you become familiar with it?
;;;         The best way is to try an example.
;;;         A good one (examples ) is
;;;                (*cond e table)
;;;                  where 
;;;                       e is (cond (coffee klatsch) (else (party))
;;;                  and 
;;;                       table is (((coffee) (#t))
;;;                                 ((klatsch party) (5 (6))))
(define cond-lines-of cdr)

;;; Have we seen how the table gets used?
;;;            Yes,*lambda and *identifier use it
;;;
;;;But how do the identifiers get into the table?
;;;            In the only action we have not defined:
;;;                      *application
;;;                      ^^^^^^^^^^^^
;;;                      ||||||||||||
;;; How is an application represented?
;;;       
;;;        An application us a list of  expressions whose car position contains an expression whose value is a function.
;;;
;;;How does an application differ from a special form, like (and ...) (or ...) or (cond ...)
;;;     An application must always determine the meaning of all its arguments.
;;; Before we can apply a function ,do we have to get the meaning of all of its arguments?
;;;       Yes.
;;;
;;; Write a function evils that takes a list of (representations of) arguments and a table, and
;;;  returns a list composed of the meaning of each argument
;;;
(define evlis
  (lambda (args table)
    (cond
      ((null? args) (quote ()))
      (else
       (cons (meaning (car args) table)  ;;; really  importan  for every argument!
             (evlis (cdr args) table))))))
;;;; What else do we need before we can determine the meaning of an application?
;;;
;;;We need to find out what its function-of means
;;;
;;;And what then?
;;;  
;;;  Then we apply the meaning of the function to the meaning of the arguments
;;;
;;;  Here is *application
;;;
(define *application
  (lambda (e table)
    (apply
     (meaning (function-of e) table)
     (evlis (arguments-of e) table))))
;;; Of course, we just have to define apply , function-of, and arguments-of correctly
;;;
(define function-of car)

(define arguments-of cdr)

;;;; How many different kinds of functions are there?
;;;
;;;     Two: primitive and non-primitive
;;; What are the two representation of functions?
;;;
;;; (primitive primitive-name)
;;; and (non-primitive (table formals body))
;;;      The list (table formals body) is called a  closure record.
;;;
;;;      Write primitive? and non-primitive?
(define primitive?
  (lambda (l)
    (eq? (first l) (quote primitive))))
(define non-primitive?
  (lambda (l)
    (eq? (first l) (quote non-primitive))))
;;;
;;; If fun does not evaluate to either a primitive or a non-primitive as
;;;         in the expression ((lambda (x) (x 5)) 3) ------<<< This expression cannot be detected in the now preocedure
;;;         there is no answer. The function apply approximatly the funcion apply available in Scheme (and lisp)
;;;
(define apply
  (lambda (fun vals)
    (cond
      ((primitive? fun)
       (apply-primitive
        (second fun) vals))
      ((non-primitive? fun)
       (apply-closure
        (second fun) vals)))))

;;;; This is the definition of apply-primitive
;;;
(define apply-primitive
  (lambda (name vals)
    (cond
      ((eq? name (quote cons))
       (cons (first vals) (second vals)))
      ((eq? name (quote car))
       (car (first vals)))
      ((eq? name (quote cdr))
       (cdr (first vals)))
      ((eq? name (quote null?))
       (null? (first vals)))
      ((eq? name (quote eq?))
       (eq? (first vals) (second vals)))
      ((eq? name (quote atom?))
       (:atom? (first vals)))
      ((eq? name (quote zero?))
       (zero? (first vals)))
      ((eq? name (quote add1))
       (add1 (first vals)))
      ((eq? name (quote sub1))
       (sub1 (first vals)))
      ((eq? name (quote number?))
       (number? (first vals))))))
(define :atom?
  (lambda (x)
    (cond
      ((atom? x) #t)
      ((null? x) #f)
      ((eq? (car x) (quote primitive))
       #t)
      ((eq? (car x) (quote non-primitive))
       #t)
      (else #f))))
;;;Is apply-closure  the only function left?
;;;        Yes,and apply-closure must extent the table
;;;                    extent the table?
;;;
;;;     how could we first find the result of (f a b)
;;;     where
;;;       f is (lambda (x y) (cons x y))
;;;       a is 1
;;;       and 
;;;       b  is (2)
;;;
;;;       ===================That's tricky. But we know what to do to find the meaning of (cons x y)
;;;                                               where 
;;;                                                     table is (((x y)
;;;                                                                (1 (2)))).
;;;
;;;  Table is the contain of the variables defined by you!!!
;;;  Why can we do this?
;;;           Here ,we don't need apply-closure
;;;
;;; Can you generalize the last two steps?
;;;                                 Applying a non-primitive function --a closure---to
;;;                                 a list of values  is the same as finding the meaning  of 
;;;                                 closure's body with its table extended by an entry of the form
;;;                                 (formals values)
;;;                                 In this entry,formals is the formals of closure and values is 
;;;                                 the result of evils
;;;
(define apply-closure
  (lambda (closure vals)
    (meaning (body-of closure)
             (extend-table
              (new-entry
               (formals-of closure)
               vals)
              (table-of closure)))))
;;;; This is a complicated function and it deserves an example.
;;;         In the following,
;;;              closure is ((((u v w)          ;;; entry name
;;;                            (1 2 3))         ;;; entry value
;;;                            ((x y  z)        ;;;;entry name
;;;                             (4 5 6)))       ;;;; entry value
;;;                            (x y)             ;;; formal
;;;                            (cons z x))        ;;; body 
(define new-entry build)
(define extend-table cons)


;;;;;;;;;;;;;;;;;;;;;;;The fifth part;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;;;;;;;;;;;;;;;;;;;;;;;****************;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;;;;;;;;;;;;;;;;;;;;;;;The sixth part;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;;;;; myracet6.rkt ;;;;;;;;;;;;;;;;;;;;;;;;;
;;;
;;;

(define member1?
  (lambda (a lat)
    (cond
      ((null? lat) #f)
      (else (or (eq? a (car lat))
                (member1? a (cdr lat)))))))
;;the most common thinking,find it and then continul
(define is-first?
  (lambda (a lat)
    (cond
      ((null? lat) #f)
      (else (eq? (car lat) a)))))

(define two-in-a-row?
  (lambda (lat)
    (cond
      ((null? lat) #f)
      (else
       (or (is-first? (car lat) (cdr lat))
           (two-in-a-row? (cdr lat)))))))
;;;the development, more close,but if-first-b must use
;;the two-in-a-row? ,so can we write a version which 
;;didn't use two-in-a-row? at all?
(define is-first-b?
  (lambda (a lat)
    (cond
      ((null? lat) #f)
      (else (or (eq? (car lat) a )
                (two-in-a-row? lat))))))
(define two-in-a-row-b?
  (lambda (lat)
    (cond 
      ((null? lat) #f)
      (else
       (is-first-b? (car lat) (cdr lat))))))
;;the is-first-b? only do the job about eq?,so why can I
;;let is-first-b? also do another thing,recur. So I
;;don't need two-in-a-row? again.
(define is-first-c?
  (lambda (preceding lat)
    (cond
      ((null? lat) #f)
      (else
       (or (eq? (car lat) preceding)
           (is-first-c? (car lat)
                        (cdr lat)))))))
(define two-in-a-row-c?
  (lambda (lat)
    (cond 
      ((null? lat) #f)
      (else (is-first-c? (car lat)
                         (cdr lat))))))
;;although two-in-a-row-c? and two-in-a-row-b? look
;;the same in the shape;
;;but the essence is not the same!
;;version b, is just encasual the is-first? and two-
;;in-a-row?
;;version c, is updated version, used it by itself?

(define sum-of-prefixes-b
  (lambda (sonssf tup)
    (cond
      ((null? tup) (quote ()))
      (else (cons (+ sonssf (car tup))
                  (sum-of-prefixes-b
                   (+ sonssf (car tup));;thinking hole
                   (cdr tup)))))))
;;not leave before,important for your thinking
;;(sum-of-prefixes-b 0 '(1 3 5))
;;two arguments

(define pick
  (lambda (n lat)
    (cond
      ((eq? n 1) (car lat))
      (else 
       (pick (- n 1) (cdr lat))))))


(define scramble-b
  (lambda (tup rev-pre)
    (cond
      ((null? tup) (quote ()))
      (else
       (cons (pick (car tup)
                   (cons (car tup) rev-pre))
             (scramble-b (cdr tup)
                         (cons (car tup) rev-pre)))))))
;;>  (scramble-b '(1 1 1 3 4 2 1 1 9 2) (quote ()))
;;'(1 1 1 1 1 4 1 1 1 9)
;;(scramble-b '(2 1 1 3 4 2 1 1 9 2) (quote ())) have a bug
;;> (scramble-b '(1 2 1 3 4 2 1 1 9 2) (quote ()))
;;'(1 1 1 2 2 4 1 1 1 9)

;;the base procedure
;;result     rev-pre
;;1          1
;;1 1        2(tup di er wei) 1
;;1 1 1      1(tup di san wei) 2 1
;;1 1 1 2    3 1 2 1 (from here to select the index 3 so is 2)
;;1 1 1 2 2  4 3 1 2 1  (select the index 4 of lat)
;;1 1 1 2 2 4    2 4 3 1 2 1
;;1 1 1 2 2 4 1  1 2 4 3 1 2 1
;;1 1 1 2 2 4 1 1     1 1 2 4 3 1 2 1
;;1 1 1 2 2 4 1 1 1   9 1 1 2 4 2 1 2 1
;;1 1 1 2 2 4 1 1 1 9   2 9 1 1 2 4 2 1 2 1
;;actually in the list of the lat,
;;the first number in the rev-pre expose the target I 
;;should get from the rev-pre,so on the one-hand,I should
;;pick-up the value according the index from varient and
;;increasing lat ,on the second hand I should constuct
;;the varient list of revpre!
;;step1 car the current tup 
;;step2 and put it into the list of revpre
;;step3 watch the index it on the list of revpre
;;step4 put the index value into the results! ok!

;;;;;;;;;;;;;;;;;;;;;;;The sixth part;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;

;
; Chapter 11 of The Seasoned Schemer:
; Welcome Back to the Show
;
; Code examples assemled by Peteris Krumins (peter@catonmat.net).
; His blog is at http://www.catonmat.net  --  good coders code, great reuse.
;
; Get yourself this wonderful book at Amazon: http://bit.ly/8cyjgw
;

; Remember member? from The Little Schemer? (http://bit.ly/4GjWdP)
; It finds if an element 'a' is in a list of atoms 'lat'.
;
(define member?
  (lambda (a lat)
    (cond
      ((null? lat) #f)
      (else (or (eq? a (car lat))
                (member? a (cdr lat)))))))



; member? helper function
;
(define member1?
  (lambda (a l)
    (letrec
      ((yes? (lambda (l)
               (cond
                 ((null? l) #f)
                 ((eq? (car l) a) #t)
                 (else (yes? (cdr l)))))))
      (yes? l))))


; is-first? function finds out whether the next element in lat, if there is
; one, is identical to this element.
;
(define is-first?
  (lambda (a lat)
    (cond
      ((null? lat) #f)
      (else (eq? (car lat) a)))))

; two-in-a-row? function determines whether any atom occurs twice in a row.
;
(define two-in-a-row?
  (lambda (lat)
    (cond
      ((null? lat) #f)
      (else
        (or (is-first? (car lat) (cdr lat))
            (two-in-a-row? (cdr lat)))))))

; Examples of two-in-a-row?
;
(two-in-a-row? '(Italian sardines spaghetti parsley))           ; false
(two-in-a-row? '(Italian sardines sardines spaghetti parsley))  ; true
(two-in-a-row? '(Italian sardines more sardines spaghetti))     ; false

; Another version of two-in-a-row? that leaves decision of what to do to
; is-first-b?
;
(define two-in-a-row-2?
  (lambda (lat)
    (cond
      ((null? lat) #f)
      (else
       (is-first-b? (car lat) (cdr lat))))))

; is-first-b? function for two-in-a-row-2?
;
(define is-first-b?
  (lambda (a lat)
    (cond
      ((null? lat) #f)
      (else (or (eq? (car lat) a)
                (two-in-a-row-2? lat))))))

; Examples of two-in-a-row-2?
;
(two-in-a-row-2? '(Italian sardines spaghetti parsley))           ; false
(two-in-a-row-2? '(Italian sardines sardines spaghetti parsley))  ; true
(two-in-a-row-2? '(Italian sardines more sardines spaghetti))     ; false

; Another version of two-in-a-row? that recurs itself instead of using
; is-first?
;
(define two-in-a-row-b?
  (lambda (preceding lat)
    (cond
      ((null? lat) #f)
      (else (or (eq? (car lat) preceding)
                (two-in-a-row-b? (car lat) (cdr lat)))))))

; The final version of two-in-a-row?
;
(define two-in-a-row-final?
  (lambda (lat)
    (cond
      ((null? lat) #f)
      (else (two-in-a-row-b? (car lat) (cdr lat))))))

(two-in-a-row-final? '(Italian sardines spaghetti parsley))           ; false
(two-in-a-row-final? '(Italian sardines sardines spaghetti parsley))  ; true
(two-in-a-row-final? '(Italian sardines more sardines spaghetti))     ; false

; Helper function for upcoming sum-of-prefixes function
;
(define sum-of-prefixes-b
  (lambda (sonssf tup)     ; sonssf stands for 'sum of numbers seen so far'
    (cond
      ((null? tup) '())
      (else (cons (+ sonssf (car tup))
                  (sum-of-prefixes-b
                   (+ sonssf (car tup))
                   (cdr tup)))))))

; sum-of-prefixes function finds the running sum of a list of numbers
;
(define sum-of-prefixes
  (lambda (tup)
    (sum-of-prefixes-b 0 tup)))

; Examples of sum-of-prefixes
;
(sum-of-prefixes '(2 1 9 17 0))   ; '(2 3 12 29 29)
(sum-of-prefixes '(1 1 1 1 1))    ; '(1 2 3 4 5)
(sum-of-prefixes '(1 1 1))        ; '(1 2 3)

;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;                                                                            ;
; The eleventh commandment                                                   ;
;                                                                            ;
; Use additional arguments when a function needs to know what the other      ;
; arguments to the function have been like so far.                           ;
;                                                                            ;
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;

; Remember the pick function from chapter 4 of The Little Schemer?
;
(define pick
  (lambda (n lat)
    (cond
      ((one? n) (car lat))
      (else (pick (sub1 n) (cdr lat))))))

; It uses one? and sub1 helper functions
;
(define one?
  (lambda (n) (= n 1)))

(define sub1
  (lambda (n) (- n 1)))

; scramble-b is a helper function for scramble
;
(define scramble-b
  (lambda (tup rev-pre)
    (cond
      ((null? tup) '())
      (else
       (cons (pick (car tup) (cons (car tup) rev-pre))
             (scramble-b (cdr tup)
                         (cons (car tup) rev-pre)))))))

; scramble
(define scramble
  (lambda (tup)
    (scramble-b tup '())))

; Examples of scramble
;
(scramble '(1 1 1 3 4 2 1 1 9 2))       ; '(1 1 1 1 1 4 1 1 1 9)
(scramble '(1 2 3 4 5 6 7 8 9))         ; '(1 1 1 1 1 1 1 1 1)
(scramble '(1 2 3 1 2 3 4 1 8 2 10))    ; '(1 1 1 1 1 1 1 1 2 8 2)

;
; Chapter 12 of The Seasoned Schemer:
; Take Cover
;
; Code examples assemled by Peteris Krumins (peter@catonmat.net).
; His blog is at http://www.catonmat.net  --  good coders code, great reuse.
;
; Get yourself this wonderful book at Amazon: http://bit.ly/8cyjgw
;

; add1 primitive
;
(define add1
  (lambda (x) (+ x 1)))

; The Y-combinator
;
(define Y
  (lambda (le)
    ((lambda (f) (f f))
     (lambda (f)
       (le (lambda (x) ((f f) x)))))))

; length function written via Y-combinator and applied to '(a b c)
;
((Y (lambda (length)
     (lambda (l)
       (cond
         ((null? l) 0)
         (else (add1 (length (cdr l)))))))) '(a b c))  ; produces output 3

; No need to pass 'a' around in multirember
; Use Y-combinator not to pass it around
;
(define multirember
  (lambda (a lat)
    ((Y (lambda (mr)
          (lambda (lat)
            (cond
              ((null? lat) '())
              ((eq? a (car lat)) (mr (cdr lat)))
              (else
               (cons (car lat) (mr (cdr lat))))))))
     lat)))

; Example of multirember
;
(multirember 'a '(a b c a a a x))             ; '(b c x)

; multirember via letrec
;
(define multirember-letrec
  (lambda (a lat)
    ((letrec
       ((mr (lambda (lat)
              (cond
                ((null? lat) '())
                ((eq? a (car lat)) (mr (cdr lat)))
                (else
                  (cons (car lat) (mr (cdr lat))))))))
       mr)
     lat)))

; Example of multirember-letrec
;
(multirember-letrec 'a '(a b c a a a x))      ; '(b c x) 

; Structure of letrec
;
; ((letrec ((mr ...)) mr) values)

; Another way to write multirember via letrec
;
(define multirember-letrec-2
  (lambda (a lat)
    (letrec
      ((mr (lambda (lat)
             (cond
               ((null? lat) '())
               ((eq? a (car lat)) (mr (cdr lat)))
               (else
                 (cons (car lat) (mr (cdr lat))))))))
       (mr lat))))

; Another test of applying multirember-letrec-2
;
(multirember-letrec-2 'a '(a b c a a a x))      ; '(b c x) 

;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;                                                                            ;
; The twelfth commandment                                                    ;
;                                                                            ;
; Use (letrec ...) to remove arguments that do not change for recursive      ;
; applications.                                                              ;
;                                                                            ;
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;

; Yet another way to write multirember via letrec
;
(define multirember-letrec-3
  (letrec
    ((mr (lambda (a lat)
           (cond
             ((null? lat) '())
             ((eq? (car lat) a) (mr a (cdr lat)))
             (else
               (cons (car lat) (mr a (cdr lat))))))))
    mr))

; Test multirember-letrec-3
;
(multirember-letrec-3 'a '(a b c a a a x))      ; '(b c x) 

; The member? function determines if the given element is in the list
;
(define member?
  (lambda (a lat)
    (cond
      ((null? lat) #f)
      ((eq? (car lat) a) #t)
      (else (member? a (cdr lat))))))

; Test member?
;
(member? 'x '(a b c x d e f))   ; #t
(member? 'x '(a b c d e f))     ; #f

; member? via letrec
;
(define member-letrec?
  (lambda (a l)
    ((letrec
       ((yes? (lambda (l)
                (cond
                  ((null? l) #f)
                  ((eq? (car l) a) #t)
                  (else (yes? (cdr l)))))))
     yes?)
    l)))

; Test member-letrec?
;
(member-letrec? 'x '(a b c x d e f))   ; #t
(member-letrec? 'x '(a b c d e f))     ; #f

; Another member? via letrec
;
(define member-letrec-2?
  (lambda (a l)
    (letrec
      ((yes? (lambda (l)
               (cond
                 ((null? l) #f)
                 ((eq? (car l) a) #t)
                 (else (yes? (cdr l)))))))
      (yes? l))))

; Test member-letrec-2?
;
(member-letrec-2? 'x '(a b c x d e f))   ; #t
(member-letrec-2? 'x '(a b c d e f))     ; #f

; The union function takes two sets and merges them
;
(define union
  (lambda (set1 set2)
    (cond
      ((null? set1) set2)
      ((member? (car set1) set2) 
       (union (cdr set1) set2))
      (else
        (cons (car set1) (union (cdr set1) set2))))))

; Example of union
;
(union
  '(tomatoes and macaroni casserole)
  '(macaroni and cheese)) ; '(tomatoes and macaroni casserole cheese)

; union via letrec
;
(define union-letrec
  (lambda (set1 set2)
    (letrec
      ((U (lambda (set)
            (cond
              ((null? set) set2)
              ((member? (car set) set2)
               (U (cdr set)))
              (else
                (cons (car set) (U (cdr set))))))))
      (U set1))))

; Test of union-letrec
;
(union-letrec
  '(tomatoes and macaroni casserole)
  '(macaroni and cheese)) ; '(tomatoes and macaroni casserole cheese)

;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;                                                                            ;
; The thirteenth commandment                                                 ;
;                                                                            ;
; Use (letrec ...) to hide and to protect functions.                         ;
;                                                                            ;
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;

(define union-letrec-protected
  (lambda (set1 set2)
    (letrec
      ((U (lambda (set)
            (cond
              ((null? set) set2)
              ((M? (car set) set2)
               (U (cdr set)))
              (else
                (cons (car set) (U (cdr set)))))))
       (M?
         (lambda (a lat)
           (cond
             ((null? lat) #f)
             ((eq? (car lat) a) #t)
             (else
               (M? a (cdr lat)))))))
      (U set1))))

; Test of union-letrec-protected
;
(union-letrec-protected
  '(tomatoes and macaroni casserole)
  '(macaroni and cheese)) ; '(tomatoes and macaroni casserole cheese)

; Fixing M? to follow 12th commandment
;
(define union-letrec-protected-12
  (lambda (set1 set2)
    (letrec
      ((U (lambda (set)
            (cond
              ((null? set) set2)
              ((M? (car set) set2)
               (U (cdr set)))
              (else
                (cons (car set) (U (cdr set)))))))
       (M?
         (lambda (a lat)
           (letrec
             ((N? (lambda (lat)
                    (cond
                      ((null? lat) #f)
                      ((eq? (car lat) a) #t)
                      (else
                        (N? (cdr lat)))))))
             (N? lat)))))
      (U set1))))

; Test of union-letrec-protected-12
;
(union-letrec-protected-12
  '(tomatoes and macaroni casserole)
  '(macaroni and cheese)) ; '(tomatoes and macaroni casserole cheese)

; The two-in-a-row? checks if a lat contains two equal elements
;
(define two-in-a-row?
  (lambda (lat)
    (letrec
      ((W (lambda (a lat)
            (cond
              ((null? lat) #f)
              ((eq? a (car lat)) #t)
              (else
                (W (car lat) (cdr lat)))))))
      (cond
        ((null? lat) #f)
        (else (W (car lat) (cdr lat)))))))

; Test two-in-a-row?
; 
(two-in-a-row? '(Italian sardines spaghetti parsley))           ; #f
(two-in-a-row? '(Italian sardines sardines spaghetti parsley))  ; #t
(two-in-a-row? '(Italian sardines more sardines spaghetti))     ; #f

; Another version of two-in-a-row?
;
(define two-in-a-row-2?
  (letrec
    ((W (lambda (a lat)
          (cond
            ((null? lat) #f)
            ((eq? a (car lat)) #t)
            (else
              (W (car lat) (cdr lat)))))))
    (lambda (lat)
      (cond
        ((null? lat) #f)
        (else (W (car lat) (cdr lat)))))))

; Test two-in-a-row-2?
;
(two-in-a-row-2? '(Italian sardines spaghetti parsley))           ; #f
(two-in-a-row-2? '(Italian sardines sardines spaghetti parsley))  ; #t
(two-in-a-row-2? '(Italian sardines more sardines spaghetti))     ; #f

; The sum-of-prefixes finds the running sum of a list of numbers
;
(define sum-of-prefixes
  (lambda (tup)
    (letrec
      ((S (lambda (sss tup)
            (cond
              ((null? tup) '())
              (else
                (cons (+ sss (car tup))
                      (S (+ sss (car tup)) (cdr tup))))))))
      (S 0 tup))))

; Examples of sum-of-prefixes
;
(sum-of-prefixes '(2 1 9 17 0))   ; '(2 3 12 29 29)
(sum-of-prefixes '(1 1 1 1 1))    ; '(1 2 3 4 5)
(sum-of-prefixes '(1 1 1))        ; '(1 2 3)



(define intersect
  (lambda (set1 set2)
    (cond
      ((null? set1) '())  ; don't forget the 1st commandment
      ((member? (car set1) set2)
       (cons (car set1) (intersect (cdr set1) set2)))
      (else
        (intersect (cdr set1) set2)))))

; It needs member? helper function
;
(define member?
  (lambda (a l)
    (letrec
      ((yes? (lambda (l)
               (cond
                 ((null? l) #f)
                 ((eq? (car l) a) #t)
                 (else (yes? (cdr l)))))))
      (yes? l))))

; Examples of intersect
;
(intersect '(a b x c d) '(q w e x r t y a))     ; '(a x)
(intersect '(a b x c d) '())                    ; '()
(intersect '() '())                             ; '()
(intersect '() '(a b x c d))                    ; '()
(intersect '(a b x c d) '(a b x c d))           ; '(a b x c d)

; We forgot the 12th commandment - use letrec to remove arguments
; that do not change for recursive applications
;
(define intersect-letrec
  (lambda (set1 set2)
    (letrec
      ((I (lambda (set)
            (cond
              ((null? set) '())
              ((member? (car set) set2)
               (cons (car set) (I (cdr set))))
              (else
                (I (cdr set)))))))
      (I set1))))

; Test of intersect-letrec
;
(intersect-letrec '(a b x c d) '(q w e x r t y a))     ; '(a x)
(intersect-letrec '(a b x c d) '())                    ; '()
(intersect-letrec '() '())                             ; '()
(intersect-letrec '() '(a b x c d))                    ; '()
(intersect-letrec '(a b x c d) '(a b x c d))           ; '(a b x c d)

; The intersectall function finds intersect of a bunch of sets
;
(define intersectall
  (lambda (lset)
    (cond
      ((null? lset) '())
      ((null? (cdr lset)) (car lset))
      (else
        (intersect (car lset)
                   (intersectall (cdr lset)))))))

; Examples of intersectall


; Examples of intersectall
;
(intersectall '((a) (a) (a)))                   ; '(a)
(intersectall '((a) () (a)))                    ; '()
(intersectall '())                              ; '()
(intersectall '((a b c d) (b c d e) (c d e f))) ; '(c d)



(define intersectall-letrec
  (lambda (lset)
    (letrec
      ((A (lambda (lset)
            (cond
              ((null? (cdr lset)) (car lset))
              (else
                (intersect (car lset)
                           (A (cdr lset))))))))
      (cond
        ((null? lset) '())
        (else (A lset))))))

; Tests of intersectall-letrec
;
(intersectall-letrec '((a) (a) (a)))                   ; '(a)
(intersectall-letrec '((a) () (a)))                    ; '()
(intersectall-letrec '())                              ; '()
(intersectall-letrec '((a b c d) (b c d e) (c d e f))) ; '(c d)




; Introducing letcc
;
(define intersectall-letcc
  (lambda (lset)
    (call-with-current-continuation
      (lambda (hop)
        (letrec
          ((A (lambda (lset)
                (cond
                  ((null? (car lset)) (hop '()))
                  ((null? (cdr lset)) (car lset))
                  (else
                    (intersect (car lset)
                               (A (cdr lset))))))))
          (cond
            ((null? lset) '())
            (else (A lset))))))))

; Tests of intersectall-letcc
;
(intersectall-letcc '((a) (a) (a)))                   ; '(a)
(intersectall-letcc '((a) () (a)))                    ; '()
(intersectall-letcc '())                              ; '()
(intersectall-letcc '((a b c d) (b c d e) (c d e f))) ; '(c d)



; intersectall that returns abruptly and promptly
;
(define intersectall-ap
  (lambda (lset)
    (call-with-current-continuation
      (lambda (hop)
        (letrec
          ((A (lambda (lset)
                (cond
                  ;;;hop one
                  ((null? (car lset)) (hop '()))
                  ((null? (cdr lset)) (car lset))
                  (else
                    (I (car lset)
                       (A (cdr lset)))))))
           (I (lambda (s1 s2)
                (letrec
                  ((J (lambda (s1)
                        (cond
                          ((null? s1) '())
                          ((member? (car s1) s2)
                           ;;;get the one you need
                           (cons (car s1) (J (cdr s1))))
                          (else
                            (J (cdr s1)))))))
                  (cond
                    ;;;hop two
                    ((null? s2) (hop '()))
                    (else (J s1)))))))
          (cond
            ((null? lset) '())
            (else (A lset))))))))

; Tests of intersectall-ap
;
(intersectall-ap '((a) (a) (a)))                   ; '(a)
(intersectall-ap '((a) () (a)))                    ; '()
(intersectall-ap '())                              ; '()
(intersectall-ap '((a b c d) (b c d e) (c d e f))) ; '(c d)

; rember function via letrec
;
;
;
; Chapter 14 of The Seasoned Schemer:
; Let There Be Names
;
; Code examples assemled by Peteris Krumins (peter@catonmat.net).
; His blog is at http://www.catonmat.net  --  good coders code, great reuse.
;
; Get yourself this wonderful book at Amazon: http://bit.ly/8cyjgw
;

; atom? primitive
;
(define atom?
  (lambda (x)
   (and (not (pair? x)) (not (null? x)))))  

; add1 primitive
;
(define add1
  (lambda (n)
    (+ n 1)))

; The leftmost function finds the leftmost atom in a non-empty list of
; s-expressions that does not contain the empty list.
;
(define leftmost
  (lambda (l)
    (cond
      ((atom? (car l)) (car l))
      (else (leftmost (car l))))))

; Example of leftmost
;
(leftmost '(((a) b) (c d)))         ; 'a

; Example of not-applicable leftmost
;
; (leftmost '((() a) ()))           ; not applicable because leftmost item is an empty list

; Let's fix this.
;
(define leftmost-fixed
  (lambda (l)
    (cond
      ((null? l) '())
      ((atom? (car l)) (car l))
      (else
        (cond
          ((atom? (leftmost-fixed (car l)))
           (leftmost-fixed (car l)))
          (else (leftmost-fixed (cdr l))))))))

; Examples of leftmost-fixed
;
(leftmost-fixed '(((x) b) (c d)))   ; 'x
(leftmost-fixed '(((x) ()) () (e))) ; 'x
(leftmost-fixed '(((() x) ())))     ; 'x
(leftmost-fixed '(((()) ())))       ; '()

(define leftmost-let
  (lambda (l)
    (cond
      ((null? l) '())
      ((atom? (car l)) (car l))
      (else
        (let ((a (leftmost-let (car l))))
          (cond
            ((atom? a) a)
            (else (leftmost-let (cdr l)))))))))

; Tests of leftmost-let
;
(leftmost-let '(((y) b) (c d)))   ; 'y
(leftmost-let '(((y) ()) () (e))) ; 'y
(leftmost-let '(((() y) ())))     ; 'y
(leftmost-let '(((()) ())))       ; '()

; The rember1* function removes the leftmost occurrence of a in l
;
(define rember1*
  (lambda (a l)
    (cond
      ((null? l) '())
      ((atom? (car l))
       (cond
         ((eq? (car l) a) (cdr l))
         (else
           (cons (car l) (rember1* a (cdr l))))))
      (else
        (cond
          ((equal? (rember1* a (car l)) (car l)) ; if the list with 'a' removed doesn't change
           (cons (car l) (rember1* a (cdr l))))  ; then recurse
          (else
            (cons (rember1* a (car l)) (cdr l)))))))) ; otherwise remove 'a'

; Examples of rember1*
;
(rember1*
  'salad
  '((Swedish rye) (French (mustard salad turkey)) salad))
; ==> '((Swedish rye) (French (mustard turkey)) salad)

(rember1*
  'meat
  '((pasta meat) pasta (noodles meat sauce) meat tomatoes))
; ==> '((pasta) pasta (noodles meat sauce) meat tomatoes)

; rember1* rewritten using the 12th commandment
;
(define rember1*-letrec
  (lambda (a l)
    (letrec
      ((R (lambda (l)
            (cond
              ((null? l) '())
              ((atom? (car l))
               (cond
                 ((eq? (car l) a) (cdr l))
                 (else
                   (cons (car l) (R (cdr l))))))
              (else
                (cond
                  ((equal? (R (car l)) (car l)) ; if the list with 'a' removed doesn't change
                   (cons (car l) (R (cdr l))))  ; then recurse
                  (else
                    (cons (R (car l)) (cdr l))))))))) ; otherwise remove 'a'
      (R l))))

; Tests of rember1*-letrec
;
(rember1*-letrec
  'salad
  '((Swedish rye) (French (mustard salad turkey)) salad))
; ==> '((Swedish rye) (French (mustard turkey)) salad)

(rember1*-letrec
  'meat
  '((pasta meat) pasta (noodles meat sauce) meat tomatoes))
; ==> '((pasta) pasta (noodles meat sauce) meat tomatoes)

; rember* rewritten using the 12th commandment and let
;
(define rember1*-letrec-let
  (lambda (a l)
    (letrec
      ((R (lambda (l)
            (cond
              ((null? l) '())
              ((atom? (car l))
               (cond
                 ((eq? (car l) a) (cdr l))
                 (else
                   (cons (car l) (R (cdr l))))))
              (else
                (let ((av (R (car l))))
                  (cond
                    ((equal? (car l) av)         ; if the list with 'a' removed didn't change
                     (cons (car l) (R (cdr l)))) ; then recurse
                    (else
                      (cons av (cdr l))))))))))  ; otherwise remove 'a'
      (R l))))

; Tests of rember1*-letrec-let
;
(rember1*-letrec-let
  'salad
  '((Swedish rye) (French (mustard salad turkey)) salad))
; ==> '((Swedish rye) (French (mustard turkey)) salad)

(rember1*-letrec-let
  'meat
  '((pasta meat) pasta (noodles meat sauce) meat tomatoes))
; ==> '((pasta) pasta (noodles meat sauce) meat tomatoes)

;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;                                                                            ;
; The fifteenth commandment (preliminary version)                            ;
;                                                                            ;
; Use (let ...) to name the values of repeated expressions.                  ;
;                                                                            ;
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;

; The depth* function finds the max depth of an s-expression
;
(define depth*
  (lambda (l)
    (cond
      ((null? l) 1)
      ((atom? (car l)) (depth* (cdr l)))
      (else
        (cond
          ((> (depth* (cdr l))
              (add1 (depth* (car l))))
           (depth* (cdr l)))
          (else
            (add1 (depth* (car l)))))))))

; Examples of depth*
;
(depth* '((pickled) peppers (peppers pickled)))                          ; 2
(depth* '(margarine ((bitter butter) (makes) (batter (bitter))) butter)) ; 4
(depth* '(c (b (a b) a) a))                                              ; 3

; depth* rewritten using let
;
(define depth*-let
  (lambda (l)
    (cond
      ((null? l) 1)
      ((atom? (car l)) (depth*-let (cdr l)))
      (else 
        (let ((a (add1 (depth*-let (car l))))
              (d (depth*-let (cdr l))))
          (cond
            ((> d a) d)
            (else a)))))))

; Tests of depth*-let
;
(depth*-let '((pickled) peppers (peppers pickled)))                          ; 2
(depth*-let '(margarine ((bitter butter) (makes) (batter (bitter))) butter)) ; 4
(depth*-let '(c (b (a b) a) a))                                              ; 3

; Another version of depth*
;
(define depth*-let-2
  (lambda (l)
    (cond
      ((null? l) 1)
      (else
        (let ((d (depth*-let-2 (cdr l))))
          (cond
            ((atom? (car l)) d)
            (else
              (let ((a (add1 (depth*-let-2 (car l)))))
                (cond
                  ((> d a) d)
                  (else a))))))))))

; Tests of depth*-let
;
(depth*-let-2 '((pickled) peppers (peppers pickled)))                          ; 2
(depth*-let-2 '(margarine ((bitter butter) (makes) (batter (bitter))) butter)) ; 4
(depth*-let-2 '(c (b (a b) a) a))                                              ; 3

;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;                                                                            ;
; The fifteenth commandment (revised version)                                ;
;                                                                            ;
; Use (let ...) to name the values of repeated expressions in a function     ;
; definition if they may be evaluated twice for one and the same use of the  ;
; function.                                                                  ;
;                                                                            ;
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;

; depth* via if
;
(define depth*-if
  (lambda (l)
    (cond
      ((null? l) 1)
      ((atom? (car l))
       (depth*-if (cdr l)))
      (else
        (let ((a (add1 (depth* (car l))))
              (d (depth* (cdr l))))
          (if (> d a) d a))))))

; Tests of depth*-if
;
(depth*-if '((pickled) peppers (peppers pickled)))                          ; 2
(depth*-if '(margarine ((bitter butter) (makes) (batter (bitter))) butter)) ; 4
(depth*-if '(c (b (a b) a) a))                                              ; 3

; depth* via max
;
(define depth*-max
  (lambda (l)
    (cond
      ((null? l) 1)
      ((atom? (car l))
       (depth*-max (cdr l)))
      (else
        (max
          (add1 (depth*-max (car l)))
          (depth*-max (cdr l)))))))

; Tests of depth*-max
;
(depth*-max '((pickled) peppers (peppers pickled)))                          ; 2
(depth*-max '(margarine ((bitter butter) (makes) (batter (bitter))) butter)) ; 4
(depth*-max '(c (b (a b) a) a))                                              ; 3

; Not interested in scramble problem, skipping it.

; leftmost via letcc
;
(define leftmost-letcc
  (lambda (l)
    (call-with-current-continuation
      (lambda (skip)
        (lm l skip)))))

; lm is leftmost-letcc's helper function
;
(define lm
  (lambda (l out)
    (cond
      ((null? l) '())
      ((atom? (car l)) (out (car l)))
      (else
        (let () ; can also use 'begin'
          (lm (car l) out)
          (lm (cdr l) out))))))

(leftmost-letcc '(((x) b) (c d)))   ; 'x
(leftmost-letcc '(((x) ()) () (e))) ; 'x
(leftmost-letcc '(((() x) ())))     ; 'x
(leftmost-letcc '(((()) ())))       ; '()

; letfmost following 13th and 14th commandments
;
(define leftmost-1314
  (letrec
    ((lm (lambda (l out)
           (cond
             ((null? l) '())
             ((atom? (car l)) (out (car l)))
             (else
               (begin
                 (lm (car l) out)
                 (lm (cdr l) out)))))))
    (lambda (l)
      (call-with-current-continuation
        (lambda (skip)
          (lm l skip))))))

(leftmost-1314 '(((x) b) (c d)))   ; 'x
(leftmost-1314 '(((x) ()) () (e))) ; 'x
(leftmost-1314 '(((() x) ())))     ; 'x
(leftmost-1314 '(((()) ())))       ; '()

; another way to follow 13th and 14th commandments
;
(define leftmost-13142
  (lambda (l)
      (letrec
        ((lm (lambda (l out)
               (cond
                 ((null? l) '())
                 ((atom? (car l)) (out (car l)))
                 (else
                   (begin
                     (lm (car l) out)
                     (lm (cdr l) out)))))))
        (call-with-current-continuation
          (lambda (skip)
            (lm l skip))))))

(leftmost-13142 '(((x) b) (c d)))   ; 'x
(leftmost-13142 '(((x) ()) () (e))) ; 'x
(leftmost-13142 '(((() x) ())))     ; 'x
(leftmost-13142 '(((()) ())))       ; '()

; yet another way
;
(define leftmost-131422
  (lambda (l)
    (call-with-current-continuation
      (lambda (skip)
        (letrec
          ((lm (lambda (l)
                 (cond
                   ((null? l) '())
                   ((atom? (car l)) (skip (car l)))
                   (else
                     (begin
                       (lm (car l))
                       (lm (cdr l))))))))
          (lm l))))))

(leftmost-131422 '(((x) b) (c d)))   ; 'x
(leftmost-131422 '(((x) ()) () (e))) ; 'x
(leftmost-131422 '(((() x) ())))     ; 'x
(leftmost-131422 '(((()) ())))       ; '()

; rember1* via letcc
;
(define rember1*-letcc
  (lambda (a l)
    (letrec
      ((rm (lambda (a l oh)
             (cond
               ((null? l) (oh 'no))
               ((atom? (car l))
                (if (eq? (car l) a)
                  (cdr l)
                  (cons (car l) (rm a (cdr l) oh))))
               (else
                 (let ((new-car
                         (call-with-current-continuation
                           (lambda (oh)
                             (rm a (car l) oh)))))
                   (if (atom? new-car)
                     (cons (car l) (rm a (cdr l) oh))
                     (cons new-car (cdr l)))))))))
      (let ((new-l
              (call-with-current-continuation
                (lambda (oh)
                  (rm a l oh)))))
        (if (atom? new-l)
          l
          new-l)))))

; Tests of rember1*-letcc
;
(rember1*-letcc
  'salad
  '((Swedish rye) (French (mustard salad turkey)) salad))
; ==> '((Swedish rye) (French (mustard turkey)) salad)

(rember1*-letcc
  'meat
  '((pasta meat) pasta (noodles meat sauce) meat tomatoes))
; ==> '((pasta) pasta (noodles meat sauce) meat tomatoes)

(rember1*-letcc
  'a
  '((foo bar) baz))
; ==> '((foo bar) baz)


;
; Chapter 16 of The Seasoned Schemer:
; Ready, Set, Bang!
;
; Code examples assemled by Peteris Krumins (peter@catonmat.net).
; His blog is at http://www.catonmat.net  --  good coders code, great reuse.
;
; Get yourself this wonderful book at Amazon: http://bit.ly/8cyjgw
;

; sub1 primitive
;
(define sub1
  (lambda (n)
    (- n 1)))

; add1 primitive
;
(define add1
  (lambda (n)
    (+ n 1)))

; atom? primitive
;
(define atom?
  (lambda (x)
   (and (not (pair? x)) (not (null? x)))))  

; member? helper function
;
(define member?
  (lambda (a l)
    (letrec
      ((yes? (lambda (l)
               (cond
                 ((null? l) #f)
                 ((eq? (car l) a) #t)
                 (else (yes? (cdr l)))))))
      (yes? l))))


; Code examples start here
;
(define sweet-tooth
  (lambda (food)
    (cons food (cons 'cake '()))))

(define last 'angelfood)

(sweet-tooth 'fruit)        ; '(fruit cake)
last                        ; 'angelfood

; The sweet-toothL function saves the last food
;
(define sweet-toothL
  (lambda (food)
    (set! last food)
    (cons food (cons 'cake '()))))

(sweet-toothL 'chocolate)   ; '(chocolate cake)
last                        ; 'chocolate

(sweet-toothL 'fruit)       ; '(fruit cake)
last                        ; 'fruit

(define ingredients '())

; The sweet-toothR function builds a list of foods
;
(define sweet-toothR
  (lambda (food)
    (set! ingredients
      (cons food ingredients))
    (cons food (cons 'cake '()))))

(sweet-toothR 'chocolate)   ; '(chocolate cake)
ingredients                 ; '(chocolate)

(sweet-toothR 'fruit)       ; '(fruit cake)
ingredients                 ; '(fruit chocolate)

(sweet-toothR 'cheese)      ; '(cheese cake)
ingredients                 ; '(cheese fruit chocolate)

(sweet-toothR 'carrot)      ; '(carrot cake)
ingredients                 ; '(carrot cheese fruit chocolate)

; The deep function wraps pizza in n parenthesis
;
(define deep
  (lambda (m)
    (cond
      ((zero? m) 'pizza)
      (else
        (cons (deep (sub1 m)) '())))))

; Example of deep
;
(deep 3)                    ; '(((pizza)))
(deep 0)                    ; 'pizza

; The deepR1 function remembers the numbers deep has seen so far
;
(define Ns1 '())
(define deepR1
  (lambda (n)
    (set! Ns1 (cons n Ns1))
    (deep n)))

; Examples of deepR1
;
(deepR1 3)                  ; '(((pizza)))
Ns1                         ; (3)
(deepR1 0)                  ; 'pizza
Ns1                         ; (0 3)

; The deepR function remembers the numbers and the results
;
(define Ns '())
(define Rs '())
(define deepR
  (lambda (n)
    (let ((result (deep n)))
      (set! Ns (cons n Ns))
      (set! Rs (cons result Rs))
      result)))

; Examples of deepR
;
(deepR 3)                   ; '(((pizza)))
Ns                          ; '(3)
Rs                          ; '((((pizza))))
(deepR 5)                   ; '(((((pizza)))))
Ns                          ; '(5 3)
Rs                          ; '((((((pizza))))) (((pizza))))
(deepR 3)                   ; '(((pizza)))
Ns                          ; '(3 5 3)
Rs                          ; '((((pizza))) (((((pizza))))) (((pizza))))

;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;                                                                            ;
; The nineteenth commandment                                                 ;
;                                                                            ;
; Use (set! ...) to remember valuable things between two distinct uses of a  ;
; function.                                                                  ;
;                                                                            ;
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;

; The find function finds pizza in Rs
;
(define find
  (lambda (n Ns Rs)
    (letrec
      ((A (lambda (ns rs)
            (cond
              ((= (car ns) n) (car rs))
              (else
                (A (cdr ns) (cdr rs)))))))
      (A Ns Rs))))

; Examples of find
;
(find 3 Ns Rs)              ; '(((pizza)))
(find 5 Ns Rs)              ; '(((((pizza)))))
;(find 7 Ns Rs)             ; not applicable at this time

; The deepM function either uses find or computes pizza (temporary version)
;
(define deepM-tmp
  (lambda (n)
    (if (member? n Ns)
      (find n Ns Rs)
      (deepR n))))

Ns                          ; '(3 5 3)
Rs                          ; '((((pizza))) (((((pizza))))) (((pizza))))

(set! Ns (cdr Ns))
(set! Rs (cdr Rs))

Ns                          ; '(5 3)
Rs                          ; '((((((pizza))))) (((pizza))))

; The final deepM version
;
(define deepM
  (lambda (n)
    (if (member? n Ns)
      (find n Ns Rs)
      (let ((result (deep n)))
        (set! Rs (cons result Rs))
        (set! Ns (cons n Ns))
        result))))

; Examples of deepM
(deepM 3)                   ; '(((pizza)))
(deepM 6)                   ; '((((((pizza))))))

; Redefining deep
;
(define deep
  (lambda (m)
    (cond
      ((zero? m) 'pizza)
      (else (cons (deepM (sub1 m)) '())))))

(deepM 9)                   ; '(((((((((pizza)))))))))

Ns                          ; '(9 8 7 6 5 3)

; Redefining deepM to folow 16th commandment
;
(define deepM
  (let ((Rs '())
        (Ns '()))
    (lambda (n)
      (if (member? n Ns)
        (find n Ns Rs)
        (let ((result (deep n)))
          (set! Rs (cons result Rs))
          (set! Ns (cons n Ns))
          result)))))

; Tests of the new deepM
;
(deepM 10)                  ; '((((((((((pizza))))))))))
(deepM 16)                  ; '((((((((((((((((pizza))))))))))))))))

; Better answer for find on empty lists
;
(define find
  (lambda (n Ns Rs)
    (letrec
      ((A (lambda (ns rs)
            (cond
              ((null? ns) #f)
              ((= (car ns) n) (car rs))
              (else
                (A (cdr ns) (cdr rs)))))))
      (A Ns Rs))))

; And a better deepM
;
(define deepM
  (let ((Rs '())
        (Ns '()))
    (lambda (n)
      (let ((exists (find n Ns Rs)))
        (if (atom? exists)
          (let ((result (deep n)))
            (set! Rs (cons result Rs))
            (set! Ns (cons n Ns))
            result)
          exists)))))

; Example of the new deepM
;
(deepM 10)                  ; '((((((((((pizza))))))))))
(deepM 16)                  ; '((((((((((((((((pizza))))))))))))))))
(deepM 0)                   ; 'pizza


;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;                                                                            ;
;              Take a deep breath or a deep pizza, now.                      ;
;                                                                            ;
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;

; Our good, old friend length
;
(define lengthz
  (lambda (l)
    (cond
      ((null? l) 0)
      (else (add1 (lengthz (cdr l)))))))

; Test length
;
(lengthz '())        ; 0
(lengthz '(a b x))   ; 3

; length via set!
;
(define lengthz
  (lambda (l) 0))

(set! lengthz
  (lambda (l)
    (cond
      ((null? l) 0)
      (else (add1 (lengthz (cdr l)))))))

; Test length
;
(lengthz '())        ; 0
(lengthz '(a b x))   ; 3

; length via set! again
;
(define lengthz
  (let ((h (lambda (l) 0)))
    (set! h
      (lambda (l)
        (cond
          ((null? l) 0)
          (else (add1 (h (cdr l)))))))
    h))

; Test length
;
(lengthz '())        ; 0
(lengthz '(a b x))   ; 3

;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;                                                                            ;
; The seventeenth commandment (final version)                                ;
;                                                                            ;
; Use (set! x ...) for (let ((x ...)) ...) only if there is at least one     ;
; (lambda ... between it and the (let ...), or if the new value for x is a   ;
; function that refers to x                                                  ;
;                                                                            ;
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;

; Another way to write length
;
(define h1              ; h1 is actually an anonymous name
  (lambda (l) 0))

(define lengthz
  (let ()
    (set! h1
      (lambda (l)
        (cond
          ((null? l) 0)
          (else (add1 (h1 (cdr l)))))))
    h1))

; Test length
;
(lengthz '())        ; 0
(lengthz '(a b x))   ; 3

; Another way
;
(define h2              ; h2 is actually an anonymous name
  (lambda (l)
    (cond
      ((null? l) 0)
      (else (add1 (h2 (cdr l)))))))

(define lengthz
  (let () h2))

; Test length
;
(lengthz '())        ; 0
(lengthz '(a b x))   ; 3

; length again
;
(define lengthz
  (let ((h (lambda (l) 0)))
    (set! h
      (lambda (l)
        (cond
          ((null? l) 0)
          (else (add1 (h (cdr l)))))))
    h))

; Test length
;
(lengthz '())        ; 0
(lengthz '(a b x))   ; 3

; Let's eliminate parts that are specific to length
;
(define L
  (lambda (lengthz)
    (lambda (l)
      (cond
        ((null? l) 0)
        (else (add1 (lengthz (cdr l))))))))

(define lengthz
  (let ((h (lambda (l) 0)))
    (set! h
      (L (lambda (arg) (h arg))))
    h))

; Test length
;
(lengthz '())        ; 0
(lengthz '(a b x))   ; 3

; Y-bang - the applicative-order imperative y-combinator
; (discovered by Peter Landin)
;
(define Y-bang
  (lambda (f)
    (letrec
      ((h (f (lambda (arg) (h arg)))))
      h)))

(define lengthz (Y-bang L))

; Test length
;
(lengthz '())        ; 0
(lengthz '(a b x))   ; 3

; depth* via Y-bang
;
(define D
  (lambda (depth*)
    (lambda (s)
      (cond
        ((null? s) 1)
        ((atom? (car s)) (depth* (cdr s)))
        (else
          (max (add1 (depth* (car s))) (depth* (cdr s))))))))

(define depth* (Y-bang D))

; Test depth*
;
(depth* '())                ; 1
(depth* '(((pizza)) ()))    ; 3

; The bizarre function
;
(define biz
  (let ((x 0))
    (lambda (f)
      (set! x (add1 x))
      (lambda (a)
        (if (= a x)
          0
          (f a))))))

; Another way to write bizarre
;
(define x1 0)               ; anonymous var
(define biz
  (lambda (f)
    (set! x1 (add1 x1))
    (lambda (a)
      (if (= a x1)
        0
        (f a)))))

; The Y-Combinator
;
(define Y
  (lambda (le)
    ((lambda (f) (f f))
     (lambda (f)
       (le (lambda (x) ((f f) x)))))))

((Y biz) 5)

; ((Y-bang biz) 5)          ; doesn't compute... why?


;
; Chapter 17 of The Seasoned Schemer:
; We Change, Therefore We Are!
;
; Code examples assemled by Peteris Krumins (peter@catonmat.net).
; His blog is at http://www.catonmat.net  --  good coders code, great reuse.
;
; Get yourself this wonderful book at Amazon: http://bit.ly/8cyjgw
;

; The atom? primitive
;
(define atom?
  (lambda (x)
   (and (not (pair? x)) (not (null? x)))))  

; The sub1 primitive
;
(define sub1
  (lambda (n)
    (- n 1)))

; The add1 primitive
;
(define add1
  (lambda (n)
    (+ n 1)))

; The deep function wraps pizza in m parenthesis
;
(define deep
  (lambda (m)
    (if (zero? m)
      'pizza
      (cons (deep (sub1 m)) '()))))

; Examples of deep
;
(deep 3)                ; '(((pizza)))
(deep 0)                ; 'pizza

; The deepM function remembers calls to deep
;
; The deepM function uses find function to find n in Ns and return 
; the correct value from Rs
;
(define find
  (lambda (n Ns Rs)
    (letrec
      ((A (lambda (ns rs)
            (cond
              ((null? ns) #f)
              ((= (car Ns) n) (car Rs))
              (else
                (A (cdr ns) (cdr rs)))))))
      (A Ns Rs))))

(define deepM
  (let ((Rs '())
        (Ns '()))
    (letrec
      ((D (lambda (m)
            (if (zero? m)
              'pizza
              (cons (deepM (sub1 m)) '())))))
      (lambda (n)
        (let ((exists (find n Ns Rs)))
          (if (atom? exists)
            (let ((result (D n)))
              (set! Rs (cons result Rs))
              (set! Ns (cons n Ns))
              result)
            exists))))))

; Examples of deepM
;
(deepM 3)               ; '(((pizza)))
(deepM 0)               ; 'pizza

; No need to use letrec in deepM
;
(define deepM-letrec
  (let ((Rs '())
        (Ns '())
        (D (lambda (m)
              (if (zero? m)
                'pizza
                (cons (deepM-letrec (sub1 m)) '())))))
    (lambda (n)
      (let ((exists (find n Ns Rs)))
        (if (atom? exists)
          (let ((result (D n)))
            (set! Rs (cons result Rs))
            (set! Ns (cons n Ns))
            result)
          exists)))))

; Test of the new deepM
;
(deepM-letrec 3)               ; '(((pizza)))
(deepM-letrec 0)               ; 'pizza

; No need for D in deepM
;
(define deepM-D
  (let ((Rs '())
        (Ns '()))
    (lambda (n)
      (let ((exists (find n Ns Rs)))
        (if (atom? exists)
          (let ((result ((lambda (m)
                           (if (zero? m)
                             'pizza
                             (cons (deepM-D (sub1 m)) '()))) n)))
            (set! Rs (cons result Rs))
            (set! Ns (cons n Ns))
            result)
          exists)))))

; Test of the new deepM
;
(deepM-D 3)               ; '(((pizza)))
(deepM-D 0)               ; 'pizza

; No need for the 2nd lambda 
;
(define deepM-2nd
  (let ((Rs '())
        (Ns '()))
    (lambda (n)
      (let ((exists (find n Ns Rs)))
        (if (atom? exists)
          (let ((result (if (zero? n)
                            'pizza
                            (cons (deepM-2nd (sub1 n)) '()))))
            (set! Rs (cons result Rs))
            (set! Ns (cons n Ns))
            result)
          exists)))))

; Test of the new deepM
;
(deepM-2nd 3)               ; '(((pizza)))
(deepM-2nd 0)               ; 'pizza

; The consC function counts number of conses needed to build
; n-deep pizza
;
(define consC
  (let ((N 0))
    (lambda (x y)
      (set! N (add1 N))
      (cons x y))))

; No way to get N out of this consC, so we need the counter
; function that will hold the count
;
(define counter 0)

; And a modification of consC
;
(define consC
  (let ((N 0))
    (set! counter (lambda() N))
    (lambda (x y)
      (set! N (add1 N))
      (cons x y))))
        
; And we need to modify deep
;
(define deep
  (lambda (m)
    (if (zero? m)
      'pizza
      (consC (deep (sub1 m)) '()))))

(deep 5)            ; '(((((pizza)))))
(counter)           ; 5

(deep 7)            ; '(((pizza))
(counter)           ; 12                ;;; (5 + 7) 

; Let's determine how many cons'es are necessary to find
; values of (deep 0) ... (deep 1000).
;
(define supercounter
  (lambda (f)
    (letrec
      ((S (lambda (n)
            (if (zero? n)
              (f n)
              (let ()
                (f n)
                (S (sub1 n)))))))
      (S 1000)
      (counter))))

; Try out supercounter
;
(supercounter deep)         ; 500512    ;;; not 500500 as expected

; Need to wipe out counter before using it again
;
(define set-counter 0)

(define consC
  (let ((N 0))
    (set! counter (lambda() N))
    (set! set-counter
      (lambda (x) (set! N x)))
    (lambda (x y)
      (set! N (add1 N))
      (cons x y))))

(set-counter 0)

; Try out supercounter again
;
(supercounter deep)

; How many conses are used by (deepM 5)?
; Need to modify deepM to use consC first.
;
(define deepM-consC
  (let ((Rs '())
        (Ns '()))
    (lambda (n)
      (let ((exists (find n Ns Rs)))
        (if (atom? exists)
          (let ((result
                  (if (zero? n)
                    'pizza
                    (consC (deepM-consC (sub1 n)) '()))))
            (set! Rs (cons result Rs))
            (set! Ns (cons n Ns))
            result)
          exists)))))

; How many conses are used by (deepM 5)?
;
(deepM 5)           ; '(((((pizza)))))
(counter)           ; 500505    ;;; because we forgot to reset counter

(set-counter 0)
(deepM-consC 5)     ; '(((((pizza)))))
(counter)           ; 5

(deep 7)            ;
(counter)           ; ??? the book says it should be 0, I get 12, why?

(supercounter deepM-consC)  ; ??? how did this work? it didn't take an argument?

; Book has also talks about conses in rember* function but I am not interested
; in it at the moment.



;
; Chapter 18 of The Seasoned Schemer:
; We Change, Therefore We Are the Same!
;
; Code examples assemled by Peteris Krumins (peter@catonmat.net).
; His blog is at http://www.catonmat.net  --  good coders code, great reuse.
;
; Get yourself this wonderful book at Amazon: http://bit.ly/8cyjgw
;

; The atom? primitive
;
(define atom?
  (lambda (x)
   (and (not (pair? x)) (not (null? x)))))  

; The sub1 primitive
;
(define sub1
  (lambda (n)
    (- n 1)))

; The add1 primitive
;
(define add1
  (lambda (n)
    (+ n 1)))

; The lots function creates lots of eggs
;
(define lots
  (lambda (n)
    (cond
      ((zero? n) '())
      (else
        (cons 'egg (lots (sub1 n)))))))

; The lenkth function counts the eggs
;
(define lenkth
  (lambda (l)
    (cond
      ((null? l) 0)
      (else
        (add1 (lenkth (cdr l)))))))

; Examples of lots and lenkth
;
(lots 3)                ; '(egg egg egg)
(lots 5)                ; '(egg egg egg egg egg)
(lots 12)               ; '(egg egg egg egg egg egg egg egg egg egg egg egg)
(lenkth (lots 3))       ; 3
(lenkth (lots 5))       ; 5
(lenkth (lots 15))      ; 15

; Create 4 eggs from 3 eggs
;
(cons 'egg (lots 3))    ; '(egg egg egg egg)

; consC, counter, set-counter from chapter 17
;
(define counter (lambda() 0))
(define set-counter (lambda () 0))
(define consC
  (let ((N 0))
    (set! counter (lambda() N))
    (set! set-counter
      (lambda (x) (set! N x)))
    (lambda (x y)
      (set! N (add1 N))
      (cons x y))))

; Add an egg at the end
;
(define add-at-end
  (lambda (l)
    (cond
      ((null? (cdr l))
       (consC (car l) (cons 'egg-end '())))
      (else
        (consC (car l) (add-at-end (cdr l)))))))

; Example of add-at-end
;
(add-at-end (lots 3))   ; '(egg egg egg egg-end)

(counter)               ; 3

; Add an egg at the end without making any new conses except for the last one
;
(define add-at-end-too
  (lambda (l)
    (letrec
      ((A (lambda (ls)
            (cond
              ((null? (cdr ls))
               (set-cdr! ls (cons 'egg-end2 '())))
              (else
                (A (cdr ls)))))))
      (A l)
      l)))

; Example of add-at-end-too
;
(set-counter 0)
(add-at-end-too (lots 3))   ; '(egg egg egg egg-end2)
(counter)                   ; 0

; kons the magnificent
;
(define kons
  (lambda (kar kdr)
    (lambda (selector)          ; returns lambda (selector)
      (selector kar kdr))))     ; calls selector with kar and kdr arguments

; kar
;
(define kar
  (lambda (c)                   ; applies selector on (a d) and returns 'a (car)
    (c (lambda (a d) a))))

; kdr
;
(define kdr
  (lambda (c)                   ; applies selector on (a d) and returns d (cdr)
    (c (lambda (a d) d))))

; Examples of kons kar kdr
;
(kar (kons 'a '()))                 ; 'a
(kdr (kons 'a '()))                 ; '()
(kar (kdr (kons 'a (kons 'b '())))) ; 'b
(kar (kons 'a (kons 'b '())))       ; 'a

; Another cons
;
(define bons
  (lambda (kar)
    (let ((kdr '()))
      (lambda (selector)
        (selector
          (lambda (x) (set! kdr x))
          kar
          kdr)))))

; Another kar
;
(define bar
  (lambda (c)
    (c (lambda (s a d) a))))

; Another kdr
;
(define bdr
  (lambda (c)
    (c (lambda (s a d) d))))

; set-kdr
;
(define set-kdr
  (lambda (c x)
    ((c (lambda (s a d) s)) x)))

; create kons using set-kdr and bons
;
(define kons2
  (lambda (a d)
    (let ((c (bons a)))
      (set-kdr c d)
      c)))

; Example of kons2 bar and bdr
;
(bar (kons2 'a '(1 2 3)))       ; 'a
(bdr (kons2 'a '(1 2 3)))       ; '(1 2 3)

; Now eggs again
;
(define dozen (lots 12))

dozen           ; '(egg egg egg egg egg egg egg egg egg egg egg egg)
                ; used 12 conses

(define bakers-dozen (add-at-end dozen))

bakers-dozen    ; '(egg egg egg egg egg egg egg egg egg egg egg egg egg-end)
                ; used 13 conses (25 total now)

(define bakers-dozen-too (add-at-end-too dozen))

bakers-dozen-too    ; '(egg egg egg egg egg egg egg egg egg egg egg egg egg-end egg-end2)
                    ; used 1 cons (26 total)

(define bakers-dozen-again (add-at-end dozen))

bakers-dozen-again  ; '(egg egg egg egg egg egg egg egg egg egg egg egg egg-end egg-end2 egg-end)
                    ; used 14 conses (40 total)

(define same?
  (lambda (c1 c2)
    (cond
      ((and (null? c1) (null? c2)) #t)
      ((or (null? c1) (null? c2)) #f)
      (else
       (let ((t1 (cdr c1))
             (t2 (cdr c2)))
         (set-cdr! c1 1)
         (set-cdr! c2 2)
         (let ((v (= (cdr c1) (cdr c2))))
           (set-cdr! c1 t1)
           (set-cdr! c2 t2)
           v))))))

(same? dozen dozen)                     ; #t
(same? dozen bakers-dozen)              ; #f
(same? dozen bakers-dozen-too)          ; #t
(same? dozen bakers-dozen-again)        ; #f
(same? bakers-dozen bakers-dozen-too)   ; #f   ;;; the book says #t???

; The last-kons function returns the last cons in a non-empty kons-list
;
(define last-kons
  (lambda (ls)
    (cond
      ((null? (cdr ls)) ls)
      (else (last-kons (cdr ls))))))

(define long (lots 12))                 ; '(egg egg egg egg egg egg egg egg egg egg egg egg)

(set-cdr! (last-kons long) long)        ; #0 = '(egg egg egg egg egg egg egg egg egg egg egg . #0#) 

; The finite-lenkth function returns length of a list
; or #f if it's an infinite list
;
(define finite-lenkth
  (lambda (p)
    (call-with-current-continuation
      (lambda (infinite)
        (letrec
          ((C (lambda (p q)
                (cond
                  ((same? p q) (infinite #f))
                  ((null? q) 0)
                  ((null? (cdr q)) 1)
                  (else
                    (+ (C (sl p) (qk q)) 2)))))
           (qk (lambda (x) (cdr (cdr x))))
           (sl (lambda (x) (cdr x))))
          (cond
            ((null? p) 0)
            (else
              (add1 (C p (cdr p))))))))))

; Examples of finite-lenkth
;
(define not-so-long (lots 5))         ; '(egg egg egg egg egg)
(finite-lenkth not-so-long)           ; 5
(finite-lenkth long)                  ; #f

; Guy's Favorite Pie
;
(define mongo
  (cons 'pie
   (cons 'a
    (cons 'la
     (cons 'mode '())))))
(set-cdr! (cdr (cdr (cdr mongo))) (cdr mongo))

; mongo

;
; Chapter 19 of The Seasoned Schemer:
; Absconding with the Jewels
;
; Code examples assemled by Peteris Krumins (peter@catonmat.net).
; His blog is at http://www.catonmat.net  --  good coders code, great reuse.
;
; Get yourself this wonderful book at Amazon: http://bit.ly/8cyjgw
;

; The atom? primitive
;
(define atom?
  (lambda (x)
   (and (not (pair? x)) (not (null? x)))))  

; The sub1 primitive
;
(define sub1
  (lambda (n)
    (- n 1)))

; Our friend deep
;
(define deep
  (lambda (m)
    (cond
      ((zero? m) 'pizza)
      (else (cons (deep (sub1 m)) '())))))

; Example of deep
;
(deep 6)                    ; '((((((pizza))))))

; Six layers creates six layered pizza
;
(define six-layers
  (lambda (p)
    (cons (cons (cons
    (cons (cons (cons p '()) '()) '()) '()) '()) '())))

; Example of six-layers
;
(six-layers 'pizza)         ; '((((((pizza))))))

; Four layers makes a four layered pizza
;
(define four-layers
  (lambda (p)
    (cons (cons (cons
    (cons p '()) '()) '()) '())))

; Example of four-layers
;
(four-layers 'pizza)        ; '((((pizza))))

; The deepB function does layering
;
(define toppings 0)
(define deepB
  (lambda (m)
    (cond
      ((zero? m)
       (call-with-current-continuation
         (lambda (jump)
           (set! toppings jump)
           'pizza)))
      (else
        (cons (deepB (sub1 m)) '())))))

; Example of deepB
;
(deepB 6)                   ; '((((((pizza)))))), but what does jump do?

; Six layers again
;
(six-layers 'mozzarella)    ; '((((((mozarella))))))

; Now the toppings
;
(toppings 'mozzarella)      ; '((((((mozarella))))))
(toppings 'cake)            ; '((((((cake))))))
(toppings 'pizza)           ; '((((((pizza))))))

; What about his
;
(cons (toppings 'cake) '()) ; it's still '((((((cake)))))) and not '(((((((cake)))))))

;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;                                                                            ;
; The twentieth commandment                                                  ;
;                                                                            ;
; When thinking about a value created with (letcc ...), write down the       ;
; function that is equivalent but does not forget. Then, when you use it,    ;
; remember to forget.                                                        ;
;                                                                            ;
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;

; Deep&co
;
(define deep&co
  (lambda (m k)
    (cond
      ((zero? m) (k 'pizza))
      (else
        (deep&co (sub1 m)
         (lambda (x) (k (cons x '()))))))))

; Examples of deep&co
;
(deep&co 0 (lambda (x) x))          ; 'pizza
(deep&co 6 (lambda (x) x))          ; '((((((pizza))))))
(deep&co 2 (lambda (x) x))          ; '((pizza))

; Deep&coB
;
(define deep&coB
  (lambda (m k)
    (cond
      ((zero? m)
       (let ()
         (set! toppings k)
         (k 'pizza)))
    (else
      (deep&coB (sub1 m)
        (lambda (x)
          (k (cons x '()))))))))

; Examples of deep&coB
;
(deep&coB 6 (lambda (x) x))         ; '((((((pizza))))))
(deep&coB 4 (lambda (x) x))         ; '((((pizza))))

; toppings is now four-layers

(toppings 'cake)                    ; '((((cake))))
(cons
  (toppings 'cake)
  (toppings 'cake))                 ; '(((((cake)))) (((cake))))

; Remember two-in-a-row?
;
(define two-in-a-row?
  (letrec
    ((W (lambda (a lat)
          (cond
            ((null? lat) #f)
            (else
              (let ((nxt (car lat)))
                (or
                  (eq? nxt a)
                  (W nxt (cdr lat)))))))))
    (lambda (lat)
      (cond
        ((null? lat) #f)
        (else (W (car lat) (cdr lat)))))))

; Examples of two-in-a-row?
;
(two-in-a-row? '(mozzarella cake mozzarella))      ; #f
(two-in-a-row? '(mozzarella mozzarella cake))      ; #t

; walk
;
(define leave '0)
(define walk
  (lambda (l)
    (cond
      ((null? l) '())
      ((atom? (car l))
       (leave (car l)))
      (else
        (begin
          (walk (car l))
          (walk (cdr l)))))))

; we need leave
(define start-it
  (lambda (l)
    (call-with-current-continuation
      (lambda (here)
        (set! leave here)
        (walk l)))))

; Example of start-it
;
(start-it '((potato) (chips (chips (with))) fish))  ; 'potato

; waddle, just like walk but remembers where it left
;
(define fill 0)
(define waddle
  (lambda (l)
    (cond
      ((null? l) '())
      ((atom? (car l))
       (begin
         (call-with-current-continuation
           (lambda (rest)
             (set! fill rest)
             (leave (car l))))
         (waddle (cdr l))))
      (else
        (begin
          (waddle (car l))
          (waddle (cdr l)))))))

; A new start
;
(define start-it2
  (lambda (l)
    (call-with-current-continuation
      (lambda (here)
        (set! leave here)
        (waddle l)))))

; Example of start-it2
;
(start-it2 '((donuts) (cheerios (cheerios (spaghettios))) donuts))
; ===> 'donuts

; get-next gets the next atom
;
(define get-next
  (lambda (x)
    (call-with-current-continuation
      (lambda (here-again)
        (set! leave here-again)
        (fill 'go)))))

; Examples of get-next
;
(get-next 'go)      ; 'cheerios
(get-next 'go)      ; 'cheerios
(get-next 'go)      ; 'paghettios
(get-next 'go)      ; 'donuts
(get-next 'go)      ; '()

; get-first
;
(define get-first
  (lambda (l)
    (call-with-current-continuation
      (lambda (here)
        (set! leave here)
        (waddle l)
        (leave '())))))

; Examples of get-first and get-next
;
(get-first '(fish (chips)))     ; 'fish
(get-next 'go)                  ; 'chips
(get-next 'go)                  ; '()

; And now the two-in-a-row*?
;
(define two-in-a-row*?
  (letrec
    ((T? (lambda (a)
           (let ((n (get-next 0)))
             (if (atom? n)
               (or (eq? n a) (T? n))
               #f))))
     (get-next
       (lambda (x)
         (call-with-current-continuation
           (lambda (here-again)
             (set! leave here-again)
             (fill 'go)))))
     (fill (lambda (x) x))
     (waddle
       (lambda (l)
         (cond
           ((null? l) '())
           ((atom? (car l))
            (begin              ; or (let() ...
              (call-with-current-continuation
                (lambda (rest)
                  (set! fill rest)
                  (leave (car l))))
              (waddle (cdr l))))
           (else
             (begin
               (waddle (car l))
               (waddle (cdr l)))))))
     (leave (lambda (x) x)))
    (lambda (l)
      (let ((fst (call-with-current-continuation
                   (lambda (here)
                     (set! leave here)
                     (waddle l)
                     (leave '())))))
        (if (atom? fst) (T? fst) #f)))))

; Examples of two-in-a-row*?
;
(two-in-a-row*? '(((food) ()) (((food)))))      ; #t
(two-in-a-row*? '(a (b (c (d))) () () (d)))     ; #t
(two-in-a-row*? '())                            ; #f
(two-in-a-row*? '(((((a))))))                   ; #f


```
