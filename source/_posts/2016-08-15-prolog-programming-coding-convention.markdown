---
layout: post
title: "prolog programming coding convention"
date: 2016-08-15 23:59:13 +0800
comments: true
categories: prolog
---

A good [prolog talking talk][1] tutorial  about the prolog programming coding convention. Prolog coding style is not constant,depends on you(plural or singular),
what's most important, you must keep it consistently.

<font color="blue">Your audience might even be yourself 2 minutes or 2 decades later.</font>

    Everybody knows this now,
    but it was once news: Programming languages are for human beings,not for computers.(But still there are many people forgeting it)

Writing programs for a human audience not only
makes them easier to read,
it makes them more reliable and even makes them run faster!
<font color="red">(Style->thinking about what you’re writing.)</font>

Style: changes in the layout means something to the reader.(too much layout process too much picture noise or writing noise).
<!--more-->
## 1.sequence

Your arguments should obey <font color="green">inputs come first, output come last.</font>

1. template(which is not be instantiate ,can be described in the comment text begin with +)
2. predicate(such as findall/3 first argument)
3. stream(the logical expression of one handware file)
4. collections(the list or the stack or the hashmap etc)
5. other options
6. outputs.

## 2.pronouncing

not spelling,but pronouncing.

Don't use the name [`xkcd`|`abc`],but use the [`tree`,`banana`]

Don't use multiple names likely to be pronounced alike.
<font color="red">People remember pronunciations, not spellings.</font>
Worst predicates have been seen: 
MENU2, MENUTWO, MENUTOO.


## 3.meaningful

you want to get the tree from trees with [tree|trees], rather than [head|tail],except that you've 
no idea what the rest is ?

good example:
```
[old|olds]
[new|news]
```


Also, you can add some auxiliary words with suffixes, such as 
```
_aux,_loop,_case,_1,_2 ,or the like
```

Also, use the desriptive variable names(with upper capital letters)
good exampls:

```
Sorted_list
Well_formed
Between_limits
Contains_duplications
Is_digits
Has_sublists
Input_list
Count
Tree
Result
```

Also, choose the predicate name to help show the argument order,

```
parent_children(X,Y).  so you know X is parent,while Y is a child(good)
parent(X,Y).           which is the parent between X and Y?(bad)
```



## 4.numbers

In one word, don't express number as words,

```
good            bad

pred1(yes)           pred_one(no)
pred2(yes)           pred_two(no)
```

In words, don't use digits for words,(use underscore to separte the words in names)

```
list_to_tree(yes)    list2tree(no)
```

## 5. code layout

Every subgoal should begin with new line and 4 spaces indent(had better not tabs(different editor may be different spaces)).


Make semicolons and if-then-else very prominent through indentation.

```
pred(A) :-   %One of several ways to do it
    (test1(A) ->
        goal1(A)
        ;
        goal2(A)
    ).
```
Most of us have been trained to overlook semicolons(which is important)

<font color="green">Space after commas that separate goals or arguments, but not list elements.</font>

```

pred([A,B], [D,E]):-
    goal(A, D),
    goal(B, E).

```

The comma has 3 uses in Prolog

+ conjunction
+ separate the list(without space after comma )
+ separate the arguments(with space after comma )

## 6. correct,and then efficiency

First your codes should be correct,that's most important!

then you should watch the efficiency:

```
Avoid  append,assert,retract
when speed is important.These are slower than most Prolog actions.
You may use them in prototyping and then modify the algorithm
to speed it up when needed
```

Also take care:
```
!        at end of last clause of a predicate
repeat   not followed by !
append   with a one-element list as its first argument
```

Prefer tail recursion, but don’t be fanatic!

Oh,fucking the hell good predicate : 
<div align="center"><font color="red">statistics</font></div>

The predicate will show the information of the swi-prolog or other prolog compilers.
## conclusion
One sentence in conclusion:

```
Never give up and never give in.Save your words.

```

## Reference:

+ Michael A. Covington :[Natural Language Processing for Prolog Programmers(1994)][2]
+ Michael A. Covington, Donald Nute, and André Vellino :[Prolog Programming in Depth(1997)][3]
+ Nugues, Pierre M. :[Language Processing with Perl and Prolog][4] <--a newer, more complete book 

[1]:http://www.covingtoninnovations.com/mc/PLCodingTalk.pdf 
[2]:http://www.covingtoninnovations.com/books/NLPPP.pdf 
[3]:http://www.covingtoninnovations.com/books/PPID.pdf 
[4]:http://www.springer.com/cn/book/9783642414633 

