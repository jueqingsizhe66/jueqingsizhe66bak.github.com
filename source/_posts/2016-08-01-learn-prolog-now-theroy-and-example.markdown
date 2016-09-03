---
layout: post
title: "Learn Prolog Now Theroy and Example"
date: 2016-08-01 23:19:59 +0800
comments: true
categories: prolog
---

Prolog tutorials should be classified into two parts
    1. theroy part
    2. example part which will refer to the theroy part.

But the below still need some theroy part, only example part.
<!--more-->

<h1 id="theory part">Theory part</h1>

<font color='red'> The power of name and quantization</font>

Name give you the concept about the things what you look and hear or think.
While quantization give you the thinking accurance and let you debug what you think.


<h1 id="Example part">Example part</h1>

+ [1. chapter-1/exercises.pl](#1)
+ [2. chapter-1/practical-session.pl](#2)
+ [3. chapter-10/exercises.pl](#3)
+ [4. chapter-10/practical-session.pl](#4)
+ [5. chapter-11/exercises.pl](#5)
+ [6. chapter-11/practical-session.pl](#6)
+ [7. chapter-12/dcg.pl](#7)
+ [8. chapter-12/exercises.pl](#8)
+ [9. chapter-12/plainTextParser.pl](#9)
+ [10. chapter-12/practical-session.pl](#10)
+ [11. chapter-12/prettyPrinter.pl](#11)
+ [12. chapter-2/exercises.pl](#12)
+ [13. chapter-2/practical-session.pl](#13)
+ [14. chapter-3/exercises.pl](#14)
+ [15. chapter-3/practical-session.pl](#15)
+ [16. chapter-4/exercises.pl](#16)
+ [17. chapter-4/pratical-session.pl](#17)
+ [18. chapter-5/exercises.pl](#18)
+ [19. chapter-5/practical-session.pl](#19)
+ [20. chapter-6/exercises.pl](#20)
+ [21. chapter-6/practical-session.pl](#21)
+ [22. chapter-7/exercises.pl](#22)
+ [23. chapter-7/practical-session.pl](#23)
+ [24. chapter-8/exercises.pl](#24)
+ [25. chapter-8/practical-session.pl](#25)
+ [26. chapter-9/exercises.pl](#26)
+ [27. chapter-9/practical-session.pl](#27)

<br/>
<hr/>
<br/>
<h2 id="1"> 1. chapter-1/exercises.pl</h2>

``` prolog
%% Chapter 1 - Exercises
%%
%% author: Peter Urbak
%% version: 2012-10-04

%% Exercise 1.1

%% Which of the following sequences of characters are atoms, which are variables
%% and which are neither?

%% 1. vINCENT - atom
%% 2. Footmassage - variable
%% 3. variable23 - atom
%% 4. Variable2000 - variable
%% 5. big_kahuna_burger - atom
%% 6. 'big kahuna burger' - atom
%% 7. big kahuna burger - neither
%% 8. 'Jules' - atom
%% 9. _Jules - variable
%% 10. '_Jules' - atom

%% Exercise 1.2

%% Which of the following sequences of characters are atoms, which are
%% variables, which are complex terms, and which are not terms at all? Give the
%% functor and arity of each complex term.

%% loves(Vincent,mia) - complex term, functor: loves, arity: 2
%% 'loves(Vincent,mia)' - atom
%% Butch(boxer) - not a term
%% boxer(Butch) - complex term, functor: boxer, arity: 1
%% and(big(burger),kahuna(burger)) - complex term, functor: and, arity: 2
%% and(big(X),kahuna(X)) - complex term, functor: and, arity: 2
%% _and(big(X),kahuna(X)) - not a term
%% (Butch kills Vincent) - not a term
%% kills(Butch Vincent) - not a term
%% kills(Butch,Vincent - not a term

%% Exercise 1.3

%% How many facts, rules, clauses, and predicates are there in the following
%% knowledge base? What are the heads of the rules, and what are the goals they
%% contain?

%% woman(vincent).
%% woman(mia).
%% man(jules).

%% person(X) :- man(X); woman(X).
%% loves(X,Y) :- father(X,Y).
%% father(Y,Z) :- man(Y), son(Z,Y).
%% father(Y,Z) :- man(Y), daughter(Z,Y).

%% facts: 3
%% rules: 4
%% clauses: 7
%% predicates: 7

%% The commas in the goals column are not logical `AND`s, they are just
%% delimiting the separate goals of a rule.

%% |     Head     |         Goals          |
%% |--------------|------------------------|
%% | person(X)    | man(X), woman(X)       |
%% | loves(X, Y)  | father(X, Y)           |
%% | father(Y, Z) | man(Y), son(Z, Y)      |
%% | father(Y, Z) | man(Y), daughter(Z, Y) |

%% Exercise 1.4

%% Represent the following in Prolog:

%% 1. Butch is a killer.
killer(butch).
kt(butch).

%% 2. Mia and Marcellus are married.
married(mia, marcellus).

%% 3. Zed is dead.
dead(zed).

%% 4. Marcellus kills everyone who gives Mia a footmassage.
kills(marcellus, X) :- givesFootMassage(X, mia).

%% 5. Mia loves everyone who is a good dancer.
loves(mia, X) :- goodDancer(X).

%% 6. Jules eats anything that is nutritious or tasty.
eats(jules, X) :- nutritious(X).
eats(jules, X) :- tasty(X).

%% Exercise 1.5

%% Suppose we are working with the following knowledge base:

wizard(ron).
hasWand(harry).
quidditchPlayer(harry).
wizard(X) :- hasBroom(X), hasWand(X).
hasBroom(X) :- quidditchPlayer(X).

%% How does Prolog respond to the following queries?

%% 1. wizard(ron). -> true
%% 2. witch(ron). -> undefined procedure
%% 3. wizard(hermione). -> false
%% 4. witch(hermione). -> undefined procedure
%% 5. wizard(harry). -> true
%% 6. wizard(Y). -> Y = ron ; Y = harry.
%% 7.witch(Y). -> undefined procedure

```


<br/>
<hr/>
<br/>
<h2 id="2"> 2. chapter-1/practical-session.pl</h2>

``` prolog
%% Chapter 1 - Practical Session

%% ?- listing. -> display content of current knowledge base.
%% ?- [kb2]. -> load kb2.pl (prepend path how?).
%% ?- listing(playsAirGuitar). -> outputs:
%% listing(playsAirGuitar).
%% playsAirGuitar(jody).
%% playsAirGuitar(mia) :-
%%  listensToMusic(mia).
%% ...
%% true
while?
fsd

```


<br/>
<hr/>
<br/>
<h2 id="3"> 3. chapter-10/exercises.pl</h2>

``` prolog
%% Chapter 10 - Exercises

%% Exercise 10.1 - Suppose we have the following database:

p(1).
p(2) :- !.
p(3).

%% Write all of Prolog’s answers to the following queries:

%% ?-  p(X).
%% X = 1 ;
%% X = 2.

%% ?-  p(X),p(Y).
%% X = Y, Y = 1 ;
%% X = 1,
%% Y = 2 ;
%% X = 2,
%% Y = 1 ;
%% X = Y, Y = 2.

%% ?-  p(X),!,p(Y).
%% X = Y, Y = 1 ;
%% X = 1,
%% Y = 2.

%% Exercise 10.2 First, explain what the following program does:

class(Number, positive) :- Number > 0.
class(0, zero).
class(Number, negative) :- Number < 0.

%% This program examines whether the specified Number is positive, zero, or
%% negative.

%% Second, improve it by adding green cuts.
class(Number, positive) :- Number > 0, !.
class(0, zero) :- !.
class(Number, negative) :- Number < 0, !.

%% If we let the second argument be a variable, we stop as soon as we have either
%% determined that Number is positive, zero or negative and don't try the other
%% possibilities after we've gotten _one_ positive result.

%% Exercise 10.3 - Without using cut, write a predicate split/3 that splits a
%% list of integers into two lists: one containing the positive ones (and zero),
%% the other containing the negative ones. For example:

%% split([3,4,-5,-1,0,4,-9],P,N)
%% should return:

%% P = [3,4,0,4].
%% N = [-5,-1,-9].

split([], [], []).

split([HP | TL], [HP | TP], N) :-
  HP >= 0,
  split(TL, TP, N).

split([HN | TL], P, [HN | TN]) :-
  HN < 0,
  split(TL, P, TN).

%% Then improve this program, without changing its meaning, with the help of the
%% cut.

split([], [], []).

split([HP | TL], [HP | TP], N) :-
  HP >= 0, !, % green cut
  split(TL, TP, N).

split([HN | TL], P, [HN | TN]) :-
  HN < 0, !, % green cut
  split(TL, P, TN).

%% Note, if you use split to check if P or N is contained in L, the items have
%% to be ordered the same way as in L.

%% Exercise 10.4
%% Recall that in Exercise 3.3 we gave you the following knowledge base:

directTrain(saarbruecken, dudweiler).
directTrain(forbach, saarbruecken).
directTrain(freyming, forbach).
directTrain(stAvold, freyming).
directTrain(fahlquemont, stAvold).
directTrain(metz, fahlquemont).
directTrain(nancy, metz).

%% We asked you to write a recursive predicate travelFromTo/2 that told us when
%% we could travel by train between two towns.

%% Now, it’s plausible to assume that whenever it is possible to take a direct
%% train from A to B, it is also possible to take a direct train from B to
%% A. Add this information to the database. Then write a predicate route/3 which
%% gives you a list of towns that are visited by taking the train from one town
%% to another. For instance:

%% ?- route(forbach,metz,Route).
%% Route = [forbach,freyming,stAvold,fahlquemont,metz].

directPath(X, Y) :-
    directTrain(X, Y).

directPath(X, Y) :-
    directTrain(Y, X).

%% base case
route(Y, Y, RevL, L) :-
    reverse(RevL, L).

%% inductive case
route(X, Y, RevL, L) :-
    directPath(X, Z),
    not(member(Z, RevL)),
    route(Z, Y, [Z | RevL], L).

%% main
route(X, Y, L) :-
    route(X, Y, [X], L).

```


<br/>
<hr/>
<br/>
<h2 id="4"> 4. chapter-10/practical-session.pl</h2>

``` prolog
%% Chapter 10 - Practical Session

%% The purpose of this session is to help you get familiar with cuts and
%% negation as failure. First some keyboard exercises:

%% Try out all three versions of the max/3 predicate defined in the text: the
%% cut-free version, the green cut version, and the red cut version. As usual,
%% “try out” means “run traces on”, and you should make sure that you trace
%% queries in which all three arguments are instantiated to integers, and
%% queries where the third argument is given as a variable.

max(X,Y,Y) :- X  =< Y.
max(X,Y,X) :-  X > Y.

%% [trace]  ?- max(2,3,3).
%%    Call: (6) max(2, 3, 3) ?
%% ^  Call: (7) 2=<3 ?
%% ^  Exit: (7) 2=<3 ?
%%    Exit: (6) max(2, 3, 3) ?
%% true ;
%%    Redo: (6) max(2, 3, 3) ?
%%    Fail: (6) max(2, 3, 3) ?
%% false.

%% [trace]  ?- max(2,3,X).
%%    Call: (6) max(2, 3, _G348) ?
%% ^  Call: (7) 2=<3 ?
%% ^  Exit: (7) 2=<3 ?
%%    Exit: (6) max(2, 3, 3) ?
%% X = 3 ;
%%    Redo: (6) max(2, 3, _G348) ?
%% ^  Call: (7) 2>3 ?
%% ^  Fail: (7) 2>3 ?
%%    Fail: (6) max(2, 3, _G348) ?
%% false.

max(X,Y,Y) :- X  =< Y, !. % green cut
max(X,Y,X) :- X > Y.

%% [trace]  ?- max(2,3,3).
%%    Call: (7) max(2, 3, 3) ?
%% ^  Call: (8) 2=<3 ?
%% ^  Exit: (8) 2=<3 ?
%%    Exit: (7) max(2, 3, 3) ?
%% true.

%% [trace]  ?- max(2,3,X).
%%    Call: (7) max(2, 3, _G351) ?
%% ^  Call: (8) 2=<3 ?
%% ^  Exit: (8) 2=<3 ?
%%    Exit: (7) max(2, 3, 3) ?
%% X = 3.

max(X,Y,Y) :- X =< Y, !.
max(X,Y,X).

%% [trace]  ?- max(2,3,3).
%%    Call: (7) max(2, 3, 3) ?
%% ^  Call: (8) 2=<3 ?
%% ^  Exit: (8) 2=<3 ?
%%    Exit: (7) max(2, 3, 3) ?
%% true.

%% [trace]  ?- max(2,3,X).
%%    Call: (7) max(2, 3, _G351) ?
%% ^  Call: (8) 2=<3 ?
%% ^  Exit: (8) 2=<3 ?
%%    Exit: (7) max(2, 3, 3) ?
%% X = 3.

%% max3 has been simplified too much, i.e. the query below is wrong.
%% [trace]  ?- max(1,2,1).
%%    Call: (7) max(1, 2, 1) ?
%%    Exit: (7) max(1, 2, 1) ?
%% true.

%% Ok, time for a burger. Try out all the methods discussed in the text for
%% coping with Vincent’s preferences. That is, try out the program that uses a
%% cut-fail combination, the program that uses negation as failure correctly,
%% and also the program that mucks it up by using negation in the wrong place.

enjoys(vincent,X) :- big_kahuna_burger(X),!,fail.
enjoys(vincent,X) :- burger(X).

burger(X) :- big_mac(X).
burger(X) :- big_kahuna_burger(X).
burger(X) :- whopper(X).

big_mac(a).
big_kahuna_burger(b).
big_mac(c).
whopper(d).

%% [trace]  ?- enjoys(vincent,a).
%%    Call: (7) enjoys(vincent, a) ?
%%    Call: (8) big_kahuna_burger(a) ?
%%    Fail: (8) big_kahuna_burger(a) ?
%%    Redo: (7) enjoys(vincent, a) ?
%%    Call: (8) burger(a) ?
%%    Call: (9) big_mac(a) ?
%%    Exit: (9) big_mac(a) ?
%%    Exit: (8) burger(a) ?
%%    Exit: (7) enjoys(vincent, a) ?
%% true ;
%%    Redo: (8) burger(a) ?
%%    Call: (9) big_kahuna_burger(a) ?
%%    Fail: (9) big_kahuna_burger(a) ?
%%    Redo: (8) burger(a) ?
%%    Call: (9) whopper(a) ?
%%    Fail: (9) whopper(a) ?
%%    Fail: (8) burger(a) ?
%%    Fail: (7) enjoys(vincent, a) ?
%% false.

%% [trace]  ?- enjoys(vincent,b).
%%    Call: (7) enjoys(vincent, b) ?
%%    Call: (8) big_kahuna_burger(b) ?
%%    Exit: (8) big_kahuna_burger(b) ?
%%    Call: (8) fail ?
%%    Fail: (8) fail ?
%%    Fail: (7) enjoys(vincent, b) ?
%% false.

%% [trace]  ?- enjoys(vincent,c).
%%    Call: (7) enjoys(vincent, c) ?
%%    Call: (8) big_kahuna_burger(c) ?
%%    Fail: (8) big_kahuna_burger(c) ?
%%    Redo: (7) enjoys(vincent, c) ?
%%    Call: (8) burger(c) ?
%%    Call: (9) big_mac(c) ?
%%    Exit: (9) big_mac(c) ?
%%    Exit: (8) burger(c) ?
%%    Exit: (7) enjoys(vincent, c) ?
%% true ;
%%    Redo: (8) burger(c) ?
%%    Call: (9) big_kahuna_burger(c) ?
%%    Fail: (9) big_kahuna_burger(c) ?
%%    Redo: (8) burger(c) ?
%%    Call: (9) whopper(c) ?
%%    Fail: (9) whopper(c) ?
%%    Fail: (8) burger(c) ?
%%    Fail: (7) enjoys(vincent, c) ?
%% false.

%% [trace]  ?- enjoys(vincent,d).
%%    Call: (7) enjoys(vincent, d) ?
%%    Call: (8) big_kahuna_burger(d) ?
%%    Fail: (8) big_kahuna_burger(d) ?
%%    Redo: (7) enjoys(vincent, d) ?
%%    Call: (8) burger(d) ?
%%    Call: (9) big_mac(d) ?
%%    Fail: (9) big_mac(d) ?
%%    Redo: (8) burger(d) ?
%%    Call: (9) big_kahuna_burger(d) ?
%%    Fail: (9) big_kahuna_burger(d) ?
%%    Redo: (8) burger(d) ?
%%    Call: (9) whopper(d) ?
%%    Exit: (9) whopper(d) ?
%%    Exit: (8) burger(d) ?
%%    Exit: (7) enjoys(vincent, d) ?
%% true.

burger(X) :- big_mac(X).
burger(X) :- big_kahuna_burger(X).
burger(X) :- whopper(X).

big_mac(a).
big_kahuna_burger(b).
big_mac(c).
whopper(d).

neg(Goal) :- Goal, !, fail.
neg(Goal).

enjoys(vincent,X) :-
  burger(X),
  neg(big_kahuna_burger(X)).

%% [trace]  ?- enjoys(vincent,a).
%%    Call: (7) enjoys(vincent, a) ?
%%    Call: (8) burger(a) ?
%%    Call: (9) big_mac(a) ?
%%    Exit: (9) big_mac(a) ?
%%    Exit: (8) burger(a) ?
%%    Call: (8) neg(big_kahuna_burger(a)) ?
%%    Call: (9) big_kahuna_burger(a) ?
%%    Fail: (9) big_kahuna_burger(a) ?
%%    Redo: (8) neg(big_kahuna_burger(a)) ?
%%    Exit: (8) neg(big_kahuna_burger(a)) ?
%%    Exit: (7) enjoys(vincent, a) ?
%% true ;
%%    Redo: (8) burger(a) ?
%%    Call: (9) big_kahuna_burger(a) ?
%%    Fail: (9) big_kahuna_burger(a) ?
%%    Redo: (8) burger(a) ?
%%    Call: (9) whopper(a) ?
%%    Fail: (9) whopper(a) ?
%%    Fail: (8) burger(a) ?
%%    Fail: (7) enjoys(vincent, a) ?
%% false.

%% [trace]  ?- enjoys(vincent,b).
%%    Call: (7) enjoys(vincent, b) ?
%%    Call: (8) burger(b) ?
%%    Call: (9) big_mac(b) ?
%%    Fail: (9) big_mac(b) ?
%%    Redo: (8) burger(b) ?
%%    Call: (9) big_kahuna_burger(b) ?
%%    Exit: (9) big_kahuna_burger(b) ?
%%    Exit: (8) burger(b) ?
%%    Call: (8) neg(big_kahuna_burger(b)) ?
%%    Call: (9) big_kahuna_burger(b) ?
%%    Exit: (9) big_kahuna_burger(b) ?
%%    Call: (9) fail ?
%%    Fail: (9) fail ?
%%    Fail: (8) neg(big_kahuna_burger(b)) ?
%%    Redo: (8) burger(b) ?
%%    Call: (9) whopper(b) ?
%%    Fail: (9) whopper(b) ?
%%    Fail: (8) burger(b) ?
%%    Fail: (7) enjoys(vincent, b) ?
%% false.

%% [trace]  ?- enjoys(vincent,c).
%%    Call: (7) enjoys(vincent, c) ?
%%    Call: (8) burger(c) ?
%%    Call: (9) big_mac(c) ?
%%    Exit: (9) big_mac(c) ?
%%    Exit: (8) burger(c) ?
%%    Call: (8) neg(big_kahuna_burger(c)) ?
%%    Call: (9) big_kahuna_burger(c) ?
%%    Fail: (9) big_kahuna_burger(c) ?
%%    Redo: (8) neg(big_kahuna_burger(c)) ?
%%    Exit: (8) neg(big_kahuna_burger(c)) ?
%%    Exit: (7) enjoys(vincent, c) ?
%% true ;
%%    Redo: (8) burger(c) ?
%%    Call: (9) big_kahuna_burger(c) ?
%%    Fail: (9) big_kahuna_burger(c) ?
%%    Redo: (8) burger(c) ?
%%    Call: (9) whopper(c) ?
%%    Fail: (9) whopper(c) ?
%%    Fail: (8) burger(c) ?
%%    Fail: (7) enjoys(vincent, c) ?
%% false.

%% [trace]  ?- enjoys(vincent,d).
%%    Call: (7) enjoys(vincent, d) ?
%%    Call: (8) burger(d) ?
%%    Call: (9) big_mac(d) ?
%%    Fail: (9) big_mac(d) ?
%%    Redo: (8) burger(d) ?
%%    Call: (9) big_kahuna_burger(d) ?
%%    Fail: (9) big_kahuna_burger(d) ?
%%    Redo: (8) burger(d) ?
%%    Call: (9) whopper(d) ?
%%    Exit: (9) whopper(d) ?
%%    Exit: (8) burger(d) ?
%%    Call: (8) neg(big_kahuna_burger(d)) ?
%%    Call: (9) big_kahuna_burger(d) ?
%%    Fail: (9) big_kahuna_burger(d) ?
%%    Redo: (8) neg(big_kahuna_burger(d)) ?
%%    Exit: (8) neg(big_kahuna_burger(d)) ?
%%    Exit: (7) enjoys(vincent, d) ?
%% true.

burger(X) :- big_mac(X).
burger(X) :- big_kahuna_burger(X).
burger(X) :- whopper(X).

big_mac(a).
big_kahuna_burger(b).
big_mac(c).
whopper(d).

enjoys(vincent,X) :-
  burger(X),
  \+  big_kahuna_burger(X).

%% [trace]  ?- enjoys(vincent,a).
%%    Call: (7) enjoys(vincent, a) ?
%%    Call: (8) burger(a) ?
%%    Call: (9) big_mac(a) ?
%%    Exit: (9) big_mac(a) ?
%%    Exit: (8) burger(a) ?
%%    Call: (8) big_kahuna_burger(a) ?
%%    Fail: (8) big_kahuna_burger(a) ?
%%    Exit: (7) enjoys(vincent, a) ?
%% true ;
%%    Redo: (8) burger(a) ?
%%    Call: (9) big_kahuna_burger(a) ?
%%    Fail: (9) big_kahuna_burger(a) ?
%%    Redo: (8) burger(a) ?
%%    Call: (9) whopper(a) ?
%%    Fail: (9) whopper(a) ?
%%    Fail: (8) burger(a) ?
%%    Fail: (7) enjoys(vincent, a) ?
%% false.

%% [trace]  ?- enjoys(vincent,b).
%%    Call: (7) enjoys(vincent, b) ?
%%    Call: (8) burger(b) ?
%%    Call: (9) big_mac(b) ?
%%    Fail: (9) big_mac(b) ?
%%    Redo: (8) burger(b) ?
%%    Call: (9) big_kahuna_burger(b) ?
%%    Exit: (9) big_kahuna_burger(b) ?
%%    Exit: (8) burger(b) ?
%%    Call: (8) big_kahuna_burger(b) ?
%%    Exit: (8) big_kahuna_burger(b) ?
%%    Redo: (8) burger(b) ?
%%    Call: (9) whopper(b) ?
%%    Fail: (9) whopper(b) ?
%%    Fail: (8) burger(b) ?
%%    Fail: (7) enjoys(vincent, b) ?
%% false.

%% [trace]  ?- enjoys(vincent,c).
%%    Call: (7) enjoys(vincent, c) ?
%%    Call: (8) burger(c) ?
%%    Call: (9) big_mac(c) ?
%%    Exit: (9) big_mac(c) ?
%%    Exit: (8) burger(c) ?
%%    Call: (8) big_kahuna_burger(c) ?
%%    Fail: (8) big_kahuna_burger(c) ?
%%    Exit: (7) enjoys(vincent, c) ?
%% true ;
%%    Redo: (8) burger(c) ?
%%    Call: (9) big_kahuna_burger(c) ?
%%    Fail: (9) big_kahuna_burger(c) ?
%%    Redo: (8) burger(c) ?
%%    Call: (9) whopper(c) ?
%%    Fail: (9) whopper(c) ?
%%    Fail: (8) burger(c) ?
%%    Fail: (7) enjoys(vincent, c) ?
%% false.

%% [trace]  ?- enjoys(vincent,d).
%%    Call: (7) enjoys(vincent, d) ?
%%    Call: (8) burger(d) ?
%%    Call: (9) big_mac(d) ?
%%    Fail: (9) big_mac(d) ?
%%    Redo: (8) burger(d) ?
%%    Call: (9) big_kahuna_burger(d) ?
%%    Fail: (9) big_kahuna_burger(d) ?
%%    Redo: (8) burger(d) ?
%%    Call: (9) whopper(d) ?
%%    Exit: (9) whopper(d) ?
%%    Exit: (8) burger(d) ?
%%    Call: (8) big_kahuna_burger(d) ?
%%    Fail: (8) big_kahuna_burger(d) ?
%%    Exit: (7) enjoys(vincent, d) ?
%% true.

%%
burger(X) :- big_mac(X).
burger(X) :- big_kahuna_burger(X).
burger(X) :- whopper(X).

big_mac(a).
big_kahuna_burger(b).
big_mac(c).
whopper(d).

enjoys(vincent, X) :- \+ big_kahuna_burger(X), burger(X).

%% [trace]  ?- enjoys(vincent,a).
%%    Call: (7) enjoys(vincent, a) ?
%%    Call: (8) big_kahuna_burger(a) ?
%%    Fail: (8) big_kahuna_burger(a) ?
%%    Call: (8) burger(a) ?
%%    Call: (9) big_mac(a) ?
%%    Exit: (9) big_mac(a) ?
%%    Exit: (8) burger(a) ?
%%    Exit: (7) enjoys(vincent, a) ?
%% true ;
%%    Redo: (8) burger(a) ?
%%    Call: (9) big_kahuna_burger(a) ?
%%    Fail: (9) big_kahuna_burger(a) ?
%%    Redo: (8) burger(a) ?
%%    Call: (9) whopper(a) ?
%%    Fail: (9) whopper(a) ?
%%    Fail: (8) burger(a) ?
%%    Fail: (7) enjoys(vincent, a) ?
%% false.

%% [trace]  ?- enjoys(vincent,b).
%%    Call: (7) enjoys(vincent, b) ?
%%    Call: (8) big_kahuna_burger(b) ?
%%    Exit: (8) big_kahuna_burger(b) ?
%%    Fail: (7) enjoys(vincent, b) ?
%% false.

%% [trace]  ?- enjoys(vincent,c).
%%    Call: (6) enjoys(vincent, c) ?
%%    Call: (7) big_kahuna_burger(c) ?
%%    Fail: (7) big_kahuna_burger(c) ?
%%    Call: (7) burger(c) ?
%%    Call: (8) big_mac(c) ?
%%    Exit: (8) big_mac(c) ?
%%    Exit: (7) burger(c) ?
%%    Exit: (6) enjoys(vincent, c) ?
%% true ;
%%    Redo: (7) burger(c) ?
%%    Call: (8) big_kahuna_burger(c) ?
%%    Fail: (8) big_kahuna_burger(c) ?
%%    Redo: (7) burger(c) ?
%%    Call: (8) whopper(c) ?
%%    Fail: (8) whopper(c) ?
%%    Fail: (7) burger(c) ?
%%    Fail: (6) enjoys(vincent, c) ?
%% false.

%% [trace]  ?- enjoys(vincent,d).
%%    Call: (6) enjoys(vincent, d) ?
%%    Call: (7) big_kahuna_burger(d) ?
%%    Fail: (7) big_kahuna_burger(d) ?
%%    Call: (7) burger(d) ?
%%    Call: (8) big_mac(d) ?
%%    Fail: (8) big_mac(d) ?
%%    Redo: (7) burger(d) ?
%%    Call: (8) big_kahuna_burger(d) ?
%%    Fail: (8) big_kahuna_burger(d) ?
%%    Redo: (7) burger(d) ?
%%    Call: (8) whopper(d) ?
%%    Exit: (8) whopper(d) ?
%%    Exit: (7) burger(d) ?
%%    Exit: (6) enjoys(vincent, d) ?
%% true.

%% Now for some programming:

%% Define a predicate nuni/2 (”not unifiable”) which takes two terms as arguments
%% and succeeds if the two terms do not unify. For example:

%% nuni(foo,foo).
%% no

%% nuni(foo,blob).
%% yes

%% nuni(foo,X).
%% no

%% You should define this predicate in three different ways:

%% First (and easiest) write it with the help of = and \+.
notUnifiable(X, Y) :- \+ X = Y.

%% Second write it with the help of = , but don’t use \+.
neg(Goal)  :-  Goal,!,fail.
neg(Goal).

notUnifiable(X, Y) :- neg(X = Y).

%% Third, write it using a cut-fail combination. Don’t use = and don’t use \+.
notUnifiable(X, Y) :- X \= Y.

%% Define a predicate unifiable(List1,Term,List2) where List2 is the list of all
%% members of List1 that unify with Term. The elements of List2 should not be
%% instantiated by the unification. For example
%% unifiable([X,b,t(Y)],t(a),List]
%% should yield
%% List  =  [X,t(Y)].

%% Bit tricky:
%% if \+ H = Term succeeds we know that the H and Term cannot be unified so we
%% fail and go to the default to second myUnifiable and add H to List2.
%% if it fails we use a cut (i.e. it can be unified) we make a cut so that we
%% don't end up trying the second myUnifiable which would add it regardless to
%% List2 even though it can't be unified.
%% Alternative we could've used a double \+ \+ so we add H in the case where the
%% check succeeds to avoid confusion about the default case being the one that
%% adds H to List2.

myUnifiable([H | List1],Term,List2) :-
  \+ H = Term, !,
  myUnifiable(List1, Term, List2).

myUnifiable([H | List1], Term, [H | List2]) :-
  myUnifiable(List1, Term, List2).

myUnifiable([],Term,[]).

```


<br/>
<hr/>
<br/>
<h2 id="5"> 5. chapter-11/exercises.pl</h2>

``` prolog
%% Chapter 11 - Exercises

%% Exercise 11.1 Suppose we start with an empty database. We then give the
%% command:

assert(q(a,b)),  assertz(q(1,2)),  asserta(q(foo,blug)).

%% What does the database now contain?
q(foo,blug).
q(a,b).
q(1,2).

%5 We then give the command:
retract(q(1, 2)), assertz( (p(X) :- h(X)) ).

%% What does the database now contain?
q(foo,blug).
q(a,b).

:- dynamic p/1.

p(A) :- h(A).

%% We then give the command:
retractall(q(_,_)).

%% What does the database now contain?
p(A) :- h(A).

%% Exercise  11.2 Suppose we have the following database:

q(blob,blug).
q(blob,blag).
q(blob,blig).
q(blaf,blag).
q(dang,dong).
q(dang,blug).
q(flab,blob).

%% What is Prolog’s response to the queries:

%% ?- findall(X,q(blob,X),List).
%% List = [blug, blag, blig].

%% ?- findall(X,q(X,blug),List).
%% List = [blob, dang].

%% findall(X,q(X,Y),List).
%% List = [blob, blob, blob, blaf, dang, dang, flab].

%% ?- bagof(X,q(X,Y),List).
%% Y = blag,
%% List = [blob, blaf] ;
%% Y = blig,
%% List = [blob] ;
%% Y = blob,
%% List = [flab] ;
%% Y = blug,
%% List = [blob, dang] ;
%% Y = dong,
%% List = [dang].

%% ?- setof(X,Y^q(X,Y),List).
%% List = [blaf, blob, dang, flab].

%% Exercise  11.3 Write a predicate sigma/2 that takes an integer n > 0 and
%% calculates the sum of all integers from 1 to n. For example:

%% ?-  sigma(3,X).
%% X  =  6
%% yes

%% ?-  sigma(5,X).
%% X  =  15
%% yes

%% Write the predicate so that results are stored in the database (there should
%% never be more than one entry in the database for each value) and are reused
%% whenever possible. For example, suppose we make the following query:

%% ?-  sigma(2,X).
%% X  =  3
%% yes

%% ?-  listing.
%% sigmares(2,3).
%% Then, if we go on to ask

%% ?-  sigma(3,X).

%% Prolog should not calculate everything new, but should get the result for
%% sigma(2,3) from the database and only add 3 to that. It should then answer:

%% X  =  6
%% yes
%% ?-  listing.
%% sigmares(2,3).
%% sigmares(3,6).

%% base case
mySigma(0, Sum, Sum) :-
    !.

mySigma(N, Acc, Sum) :-
    sigmares(N, NewSum),
    Sum is Acc + NewSum,
    !.

%% inductive case
mySigma(N, Acc, Sum) :-
  is(DecN, -(N, 1)),
  is(NewAcc, +(Acc, N)),
  mySigma(DecN, NewAcc, Sum).

%% main
:- dynamic sigmares/2.

sigma(N, Sum) :- sigmares(N, Sum), !.

sigma(N, Sum) :- mySigma(N, 0, Sum),
  assert( sigmares(N, Sum) ).

```


<br/>
<hr/>
<br/>
<h2 id="6"> 6. chapter-11/practical-session.pl</h2>

``` prolog
%% Chapter 11 - Practical Session

%% Try the following two programming exercises:

%% 1. Sets can be thought of as lists that don’t contain any repeated
%% elements. For example, [a,4,6] is a set, but [a,4,6,a] is not (as it contains
%% two occurrences of a). Write a Prolog program subset/2 that is satisfied
%% when the first argument is a subset of the second argument (that is, when
%% every element of the first argument is a member of the second argument). For
%% example:

%% ?-  subset([a,b],[a,b,c])
%% yes

%% ?-  subset([c,b],[a,b,c])
%% yes

%% ?-  subset([],[a,b,c])
%% yes

%% Your program should be capable of generating all subsets of an input set by
%% backtracking. For example, if you give it as input

%% ?-  subset(X,[a,b,c])

%% it should successively generate all eight subsets of [a,b,c].

%% Generates all subsets of the given set (once each)

%% base case
subset_gen([], []).

%% inductive case
subset_gen(Subset, [H | Set]) :- subset_gen(Subset, Set).
subset_gen([H |Subset], [H | Set]) :- subset_gen(Subset, Set).

%% Checks if the first argument is a subset of the second

%% base case
subset_check([], _).

%% inductive case
subset_check([H | Subset], Set) :-
    member(H, Set),
    subset_check(Subset, Set).

%% main
subset(Subset, Set) :-
    var(Subset),
    subset_gen(Subset, Set).

subset(Subset, Set) :-
    nonvar(Subset),
    subset_check(Subset, Set).

%% 2. Using the subset predicate you have just written, and findall/3 , write a
%% predicate powerset/2 that takes a set as its first argument, and returns the
%% powerset of this set as the second argument. (The powerset of a set is the
%% set of all its subsets.) For example:

%% ?-  powerset([a,b,c],P)
%% should return

%% P  =  [[],[a],[b],[c],[a,b],[a,c],[b,c],[a,b,c]]
%% It doesn’t matter if the sets are returned in some other order. For example,

%% P  =  [[a],[b],[c],[a,b,c],[],[a,b],[a,c],[b,c]]

%% is fine too

%% main
powerset(Set, Powerset) :- setof(Subset, subset(Subset, Set), Powerset).

```


<br/>
<hr/>
<br/>
<h2 id="7"> 7. chapter-12/dcg.pl</h2>

``` prolog
%% dcg.pl

:- module(dcg,
    [s/4]).

%% Once you have done this, extend the DCG so that noun phrases can be modified
%% by adjectives and simple prepositional phrases (that is, it should be able to
%% handle noun phrases such as ``the small frightened woman on the table'' or
%% ``the big fat cow under the shower''). Then, further extend it so that the
%% distinction between first, second, and third person pronouns is correctly
%% handled (both in subject and object form).

%% 'small' = adjective, 'on the table' = prepositional phrase.

s(Number, s(Number, NP, VP)) --> np(Number, NP), vp(Number, VP).
s(Number, s(Number, NP, PP)) --> np(Number, NP), pp(PP).

np(Number, np(Number, DET, AP, N)) --> det(Number, DET), ap(AP), n(Number, N).
np(Number, np(Number, DET, N)) --> det(Number, DET), n(Number, N).

vp(Number, vp(Number, V, PP, NP)) --> v(Number, V), pp(PP).
vp(Number, vp(Number, V)) --> v(Number, V).

det(Number, det(Number, Word)) --> [Word], {lex(Word, Number, det)}.

ap(ap(ADJ1,ADJ2)) --> adj(size, ADJ1), adj(propadj, ADJ2).
ap(ap(ADJ)) --> adj(size, ADJ).
ap(ap(ADJ)) --> adj(propadj, ADJ).

pp(pp(PRE, NP)) --> pre(PRE), np(_, NP).

pre(pre(Word)) --> [Word], {lex(Word, prepos)}.

n(Number, n(Number, Word)) --> [Word], {lex(Word, Number, n)}.

v(Number, v(Number, Word)) --> [Word], {lex(Word, Number, v)}.

adj(Type, adj(Type, Word)) --> [Word], {lex(Word, Type, adj)}.

%% Lexicon
lex(the, _, det).
lex(a, singular, det).

lex(woman, singular, n).
lex(man, singular, n).
lex(men, plural, n).
lex(cow, singular, n).
lex(table, singular, n).
lex(shower, singular, n).

lex(shoots, singular, v).
lex(shoot, plural, v).

lex(small, size, adj).
lex(big, size, adj).
lex(frightened, propadj, adj).
lex(fat, propadj, adj).

lex(under, prepos).
lex(on, prepos).

%% ?- s(P,T,[the,small,frightened,woman,on,the,table],[]).
%% P = singular,
%% T = s(singular,
%%       np(singular,
%%      det(singular,
%%          the),
%%      ap(adj(size,
%%       small),
%%         adj(propadj,
%%       frightened)),
%%      n(singular,
%%        woman)),
%%       pp(pre(on),
%%      np(singular,
%%         det(singular,
%%       the),
%%         n(singular, table))))

%% Note: couldn't be arsed to complete the whole implementation, i.e. the
%% first/second/third person aspects, but it should be (almost) trivial to do.
```


<br/>
<hr/>
<br/>
<h2 id="8"> 8. chapter-12/exercises.pl</h2>

``` prolog
%% Chapter 12 - Exercises

%% Exercise 12.1 Write code that creates hogwart.houses, a file that that looks
%% like this:

%%           gryffindor
%% hufflepuff          ravenclaw
%%             slytherin

%% You can use the built-in predicates open/3 , close/1 , tab/2 , nl/1 , and
%% write/2 .

writeHouses :-
  open('hogwarts.houses', write, Stream),
  tab(Stream, 10), write(Stream, 'gryffindor'),
  nl(Stream),
  write(Stream, 'hufflepuff'), tab(Stream, 10), write(Stream, 'ravenclaw'),
  nl(Stream),
  tab(Stream, 10), write(Stream, 'slytherin'),
  close(Stream).

%% Exercise 12.2 Write a Prolog program that reads in a plain text file word by
%% word, and asserts all read words and their frequency into the Prolog
%% database. You may use the predicate readWord/2 to read in words. Use a
%% dynamic predicate word/2 to store the words, where the first argument is a
%% word, and the second argument is the frequency of that word.

:- dynamic word/2.

addWordToDatabase(W) :-
  word(W, X),
  retract(word(W, X)),
  Y is X + 1,
  assert( word(W, Y) ), !.

addWordToDatabase(W) :-
  assert( word(W, 1) ), !.

readPlainText(X) :-
  open(X, read, Stream),
  readWords(Stream),
  close(Stream).

readWords(InStream) :-
  \+ at_end_of_stream(InStream),
  readWord(InStream, W),
  write(W), nl,
  addWordToDatabase(W),
  readWords(InStream).

readWord(InStream, W) :-
  get_code(InStream, Char),
  checkCharAndReadRest(Char, Chars, InStream),
  atom_codes(W, Chars).

checkCharAndReadRest(10, [], _) :- !.

checkCharAndReadRest(32, [], _) :- !.

checkCharAndReadRest(-1, [], _) :- !.

checkCharAndReadRest(end_of_file, [], _) :- !.

checkCharAndReadRest(Char, [Char | Chars] , InStream) :-
  get_code(InStream, NextChar),
  checkCharAndReadRest(NextChar, Chars, InStream).

%% ?- readPlainText('foo.txt').
%% foo
%% bar
%% baz
%% bozz
%% newline
%% bozz
%% bar
%% baz
%% foo
%% end
%% of
%% file
%% false.

%% partial output from ?- listing.
word(newline, 1).
word(bozz, 2).
word(bar, 2).
word(baz, 2).
word(foo, 2).
word(end, 1).
word(of, 1).
word(file, 1).

```


<br/>
<hr/>
<br/>
<h2 id="9"> 9. chapter-12/plainTextParser.pl</h2>

``` prolog
%% plainTextParser.pl

:- module(plainTextParser,
   [parseSentence/2]).

%% Reading arbitrary input using get_code/2 and atom_codes/2

checkCharAndReadRest(46, [], _) :- !. % dot

checkCharAndReadRest(32, [], _) :- !. % blank character

checkCharAndReadRest(10, [], _) :- !. % new line

checkCharAndReadRest(-1, [], _) :- !. % end of stream

checkCharAndReadRest(end_of_file, [], _) :- !.

checkCharAndReadRest(Char, [Char | Chars] , InStream) :-
  get_code(InStream, NextChar),
  checkCharAndReadRest(NextChar, Chars, InStream).

readWord(InStream, W) :-
  get_code(InStream, Char),
  checkCharAndReadRest(Char, Chars, InStream),
  atom_codes(W, Chars).

parseSentence(InStream, Sentence) :-
  at_end_of_stream(InStream), !.

parseSentence(InStream, [Word | Sentence]) :-
  readWord(InStream, Word), !,
  parseSentence(InStream, Sentence).

parseSentences(InStream, Sentences) :-
  at_end_of_stream(InStream), !.

parseSentences(InStream, [Sentence | Sentences]) :-
  \+ at_end_of_stream(InStream),
  parseSentence(InStream, Sentence), !,
  parseSentences(InStream, Sentences).

testParser(InputFile) :-
  open(InputFile, read, InStream), !,
  parseSentences(InStream, Sentences), !,
  print(Sentences),
  close(InStream).

```


<br/>
<hr/>
<br/>
<h2 id="10"> 10. chapter-12/practical-session.pl</h2>

``` prolog
%% Chapter 12 - Practical Session

%% TODO: The DCG still doesn't work properly, so it should be fixed after Step
%% 7.

%% Step 1
%% Take the DCG that you built in the practical session of Chapter 8 and turn
%% it into a module, exporting the predicate s/3 , that is, the predicate that
%% lets you parse sentences and returns the parse tree as its first argument.

%% Done

%% Step 2
%% In the practical session of Chapter 9 , you had to write a program for
%% pretty printing parse trees onto the screen. Turn that into a module as
%% well.

%% Done

%% Step 3
%% Now modify the program so that it prints the tree not to the screen but to a
%% given stream. That means that the predicate pptree should now be a two-place
%% predicate taking the Prolog representation of a parse tree and a stream as
%% arguments.

%% Done
%% pptree(Term, Stream).

%% Step 4
%% Import both modules into a file and define a two-place predicate test which
%% takes a list representing a sentence (such as [a,woman,shoots]), parses it,
%% and writes the result to the file specified by the second argument of test.
%% Check that everything is working as it should.

:- use_module(prettyPrinter). % prints the tree structure of Terms.
:- use_module(dcg). % parses sentences according to its DCG.
:- use_module(plainTextParser). % parses english plain text into word lists.

test(Sentence) :-
  s(_, Term, Sentence, []),
  pptree(Term, _).

%% Step 5
%% Finally, modify test/2, so that it takes a filename instead of a sentence as
%% its first argument, reads in the sentences given in the file one by one,
%% parses them, and writes the sentence as well as the parsing result into the
%% output file. For example, if your input file looked like this:

%% [the,cow,under,the,table,shoots].
%% [a,dead,woman,likes,he].

%% the output file should look something like this:
%% [the,  cow,  under,  the,  table,  shoots]
%% s(
%%   np(
%%      det(the)
%%      nbar(
%%       n(cow))
%%      pp(
%%         prep(under)
%%         np(
%%        det(the)
%%        nbar(
%%             n(table)))))
%%   vp(
%%      v(shoots)))

%% [a,  dead,  woman,  likes,  he]
%% no

test(InputFile, OutputFile) :-
  open(InputFile, read, InStream), !,
  open(OutputFile, write, OutStream), !,
  process_sentences(InStream, OutStream), !,
  close(InStream), !,
  close(OutStream), !.

process_sentences(InStream, OutStream) :-
  at_end_of_stream(InStream), !.

process_sentences(InStream, OutStream) :-
  \+ at_end_of_stream(InStream),
  read(InStream, Sentence),
  process_sentence(Sentence, OutStream),
  process_sentences(InStream, OutStream).

process_sentence(Sentence, OutStream) :-
  s(_, Term, Sentence, []), !,
  write(OutStream, Sentence), write(OutStream, '.'), nl(OutStream),
  print(Term), nl, nl, !,
  pptree(Term, OutStream), !,
  nl(OutStream).

process_sentence(Sentence, OutStream) :-
  write(OutStream, Sentence), write(OutStream, '.'), nl(OutStream), !,
  write(OutStream, 'no'), nl(OutStream),
  nl(OutStream).

%% Done

%% Step 6
%% Now (if you are in for some real Prolog hacking) try to write a module that
%% reads in sentences terminated by a full stop or a line break from a file, so
%% that you can give your testsuite as

%% the cow under the table shoots.
%% a dead woman likes he.

%% instead of
%% [the,cow,under,the,table,shoots].
%% [a,dead,woman,likes,he].

%% TODO

%% Step 7
%% Make the testsuite environment more sophisticated, by adding information to
%% the input file about the expected output (in this case, whether the sentences
%% has a parse or not). Then modify the program so that it checks whether the
%% expected output matches the obtained output.

%% TODO

```


<br/>
<hr/>
<br/>
<h2 id="11"> 11. chapter-12/prettyPrinter.pl</h2>

``` prolog
%% prettyPrinter.pl

:- module(prettyPrinter,
   [pptree/2]).

%% Write a predicate pptree/1 that takes a complex term representing a tree,
%% such as s(np(det(a),n(man)),vp(v(shoots),np(det(a),n(woman)))), as its
%% argument and prints a nice and readable output for this tree.

%% Helper function

%% simple terms
termtype(Term, atom) :-
  atom(Term).
termtype(Term, integer) :-
  integer(Term).
termtype(Term, number) :-
  number(Term).
termtype(Term, constant) :-
  atomic(Term).
termtype(Term, variable) :-
  var(Term).

%% complex term
termtype(Term, complex_term) :-
  nonvar(Term),
  functor(Term, _, A),
  A > 0.

%% simple term
termtype(Term, simple_term) :-
  termtype(Term, variable).
termtype(Term, simple_term) :-
  termtype(Term, constant).

%% term
termtype(Term, term) :-
  termtype(Term, simple_term).
termtype(Term, term) :-
  termtype(Term, complex_term).

%% Actual Function

%% simple term; indent and print the term with no linebreak.
%% Example: mia
printterm(Term, Indent, Stream) :-
  termtype(Term, simple_term),
  tab(Stream, Indent), write(Stream, Term).

%% complex term containing only simple terms; indent and print the
%% term and its arguments.
%% Example: likes(mia, vincent).
printterm(Term, Indent, Stream) :-
  termtype(Term, complex_term),
  Term =.. [TermName | TermArguments],
  checkSimpleTypes(TermArguments),
  tab(Stream, Indent), write(Stream, Term).

checkSimpleTypes([]).

checkSimpleTypes([Head | Tail]) :-
  termtype(Head, simple_term),
  checkSimpleTypes(Tail).

%% complex term containing other complex terms; indent and print the term, then
%% call printterm on all its arguments.
printterm(Term, Indent, Stream) :-
  termtype(Term, complex_term),
  Term =.. [TermName | TermArguments],
  tab(Stream, Indent), write(Stream, TermName), write(Stream, '('),
  nl(Stream),
  NewIndent is Indent + 2,
  iterateArguments(TermArguments, NewIndent, Stream),
  write(Stream, ')').

iterateArguments([], _, _).

iterateArguments([Head | Tail], Indent, Stream) :-
  Tail == [],
  printterm(Head, Indent, Stream).

iterateArguments([Head | Tail], Indent, Stream) :-
  printterm(Head, Indent, Stream), nl(Stream),
  iterateArguments(Tail, Indent, Stream).

%% Main
%% pptree(Term, Stream) :-
%%  open('treeOutput.txt', write, Stream),
%%  printterm(Term, 0, Stream),
%%  close(Stream).

pptree(Term, OutStream) :-
  printterm(Term, 0, OutStream), !,
  nl(OutStream).

%% ?- pptree(s(np(det(a),n(man)),vp(v(shoots),np(det(a),n(woman))))).
%% s(
%%   np(
%%     det(a)
%%     n(man))
%%   vp(
%%     v(shoots)
%%     np(
%%       det(a)
%%       n(woman))))
%% true

%% TODO: If possible, try to see if you can generate actual syntax
%% using dot files.

```


<br/>
<hr/>
<br/>
<h2 id="12"> 12. chapter-2/exercises.pl</h2>

``` prolog
%% Chapter 2 - Exercises

%% author: Peter Urbak
%% version: 2012-10-04

%% Exercise 2.1

%% Which of the following pairs of terms unify? Where relevant,
%% give the variable instantiations that lead to successful unification.

%%  1. bread = bread -> true
%%  2. 'Bread' = bread -> false
%%  3. 'bread' = bread -> true
%%  4. Bread = bread -> Bread = bread.
%%  5. bread = sausage -> false
%%  6. food(bread) = bread -> false
%%  7. food(bread) = X -> X = food(bread).
%%  8. food(X) = food(bread) -> X = bread.
%%  9. food(bread, X) = food(Y, sausage) -> X = sausage, Y = bread.
%% 10. food(bread, X, beer) = food(Y, sausage, X) -> false
%% 11. food(bread, X, beer) = food(Y, kahuna_burger) -> false
%% 12. food(X) = X -> X = food(X).
%% 13. meal(food(bread), drink(beer)) = meal(X,Y) ->
%%     X = food(bread), Y = drink(beer).
%% 14. meal(food(bread), X) = meal(X, drink(beer)) -> false

%% Exercise 2.2

%% We are working with the following knowledge base:

house_elf(dobby).
witch(hermione).
witch('McGonagall').
witch(rita_skeeter).
magic(X) :- house_elf(X).
magic(X) :- wizard(X).
magic(X) :- witch(X).

%% Which of the following queries are satisfied? Where relevant, give all the
%% variable instantiations that lead to success.

%% 1. ?- magic(house_elf). -> false
%% 2. ?- wizard(harry). -> false
%% 3. ?- magic(wizard). -> false
%% 4. ?- magic('McGonagall'). -> true
%% 5. ?- magic(Hermione). ->
%%    Hermione = dobby;
%%    Hermione = hermione;
%%    Hermione = 'McGonagall';
%%    Hermione = rita_skeeter.

%% Draw the search tree for the fifth query magic(Hermione).

%%     --------------------------------------------------------------
%%     |                     ?- magic(Hermione)                     |
%%     --------------------------------------------------------------
%%                 /              |                     \
%% Hermione = _G1 /               | Hermione = _G1       \ Hermione = _G1
%%               /                |                       \
%% ---------------------  ------------------        ---------------------------------------
%% | ?- house_elf(_G1) |  | ?- wizard(_G1) |        |            ?- witch(_G1)            |
%% ---------------------  ------------------        ---------------------------------------
%%             |                  |                     /     |                     \
%% _G1 = dobby |                  |     _G1 = hermione /      | _G1 = 'McGonagall'   \ _G1 = rita_skeeter
%%             |                  |                   /       |                       \
%%           -----                X                -----    -----                    -----
%%           |   |                                 |   |    |   |                    |   |
%%           -----                                 -----    -----                    -----

%% Exercise 2.3 Here is a tiny lexicon and mini grammar with only one rule which
%% defines a sentence as consisting of five words: an article, a noun, a verb,
%% and again an article and a noun.

word(article,a).
word(article,every).
word(noun,criminal).
word(noun,'big kahuna burger').
word(verb,eats).
word(verb,likes).

sentence(Word1,Word2,Word3,Word4,Word5) :-
  word(article,Word1),
  word(noun,Word2),
  word(verb,Word3),
  word(article,Word4),
  word(noun,Word5).

%% What query do you have to pose in order to find out which sentences the
%% grammar can generate? List all sentences that this grammar can generate in
%% the order Prolog will generate them. Make sure that you understand why Prolog
%% generates them in this order.

%% ?- sentence(A, B, C, D, E). -> generates all possibilities:
%% e.g. the following is the first possibility since it uses all the first
%% examples of article, noun, verb listed.
%% A = a,
%% B = criminal,
%% C = eats,
%% D = a,
%% E = criminal ;

%% Exercise 2.4
%% Here are six English words:
%% abalone, abandon, anagram, connect, elegant, enhance.
%% They are to be arranged in a crossword puzzle like fashion in the grid given
%% below.
%%     V1V2V3
%%     _ _ _
%% H1 _______
%%     _ _ _
%% H2 _______
%%     _ _ _
%% H3 _______
%%     _ _ _


%% The following knowledge base represents a lexicon containing these words.
word(abalone,a,b,a,l,o,n,e).
word(abandon,a,b,a,n,d,o,n).
word(enhance,e,n,h,a,n,c,e).
word(anagram,a,n,a,g,r,a,m).
word(connect,c,o,n,n,e,c,t).
word(elegant,e,l,e,g,a,n,t).

%% Write a predicate crosswd/6 that tells us how to fill the grid, i.e. the
%% first three arguments should be the vertical words from left to right and the
%% following three arguments the horizontal words from top to bottom.

crossword(V1,V2,V3,H1,H2,H3) :-
  %% Make the word intersect at the right places.
  %% Use _ where we don't give a fuck about variable name.
  word(H1,_,H12V12,_,H14V22,_,H16V32,_),
  word(H2,_,H22V14,_,H24V24,_,H26V34,_),
  word(H3,_,H32V16,_,H34V26,_,H36V36,_),

  word(V1,_,H12V12,_,H22V14,_,H32V16,_),
  word(V2,_,H14V22,_,H24V24,_,H34V26,_),
  word(V3,_,H16V32,_,H26V34,_,H36V36,_)
.

%% ?- crosswd(H1,H2,H3,V1,V2,V3). ->
%% H1 = abandon, H2 = elegant, H3 = enhance, V1 = abalone, V2 = anagram,
%% V3 = connect ;
%% H1 = abalone, H2 = anagram, H3 = connect, V1 = abandon, V2 = elegant,
%% V3 = enhance ;
%% false.
```


<br/>
<hr/>
<br/>
<h2 id="13"> 13. chapter-2/practical-session.pl</h2>

``` prolog
%% Chapter 2 - Practical Session

%% \=/2 opposite of =/2
%% ?- a \= b -> true

%% Questions:

%% a \= a. -> false

%% 'a' \= a. -> false

%% A \= a. -> false

%% f(a) \= a. -> true

%% f(a) \= A. -> false

%% f(A) \= f(a). -> false

%% g(a,B,c) \= g(A,b,C). -> false

%% g(a,b,c) \= g(A,C). -> true

%% f(X) \= X. -> false (interesting).

%% trace.

%% f(a).
%% f(b).

%% g(a).
%% g(b).

%% h(b).

%% k(X) :- f(X),g(X),h(X).

%% Trace example:
%% [trace]  ?- k(X).
%%   Call: (6) k(_G386) ?
%%   Call: (7) f(_G386) ?
%%   Exit: (7) f(a) ?
%%   Call: (7) g(a) ?
%%   Exit: (7) g(a) ?
%%   Call: (7) h(a) ?
%%   Fail: (7) h(a) ?
%%   Redo: (7) f(_G386) ?
%%   Exit: (7) f(b) ?
%%   Call: (7) g(b) ?
%%   Exit: (7) g(b) ?
%%   Call: (7) h(b) ?
%%   Exit: (7) h(b) ?
%%   Exit: (6) k(b) ?
%% X = b.

%% ?- notrace.
%% ?- nodebug.
```


<br/>
<hr/>
<br/>
<h2 id="14"> 14. chapter-3/exercises.pl</h2>

``` prolog
%% Chapter 3 - Exercises

%% Exercise 3.1
%% Do you know these wooden Russian dolls, where smaller ones are contained in
%% bigger ones? Here is schematic picture of such dolls.
%% from outer to inner doll: katarina -> olga -> natsha -> irina.
%% First, write a knowledge base using the predicate directlyIn/2 which encodes
%% which doll is directly contained in which other doll. Then, define a
%% (recursive) predicate in/2, that tells us which doll is (directly or
%% indirectly) contained in which other doll. E.g. the query
%% in(katarina,natasha) should evaluate to true, while in(olga, katarina) should
%% fail.

directlyIn(katarina, olga).
directlyIn(olga, natsha).
directlyIn(natsha, irina).

in(X,Y) :- directlyIn(X,Y).
in(X,Y) :-
  directlyIn(X,Z),
  in(Z,Y).

%% Exercise 3.2
%% Define a predicate greater_than/2 that takes two numerals in the notation
%% that we introduced in this lecture (i.e. 0, succ(0), succ(succ(0)) ...) as
%% arguments and decides whether the first one is greater than the second
%% one. E.g:
%% ?- greater_than(succ(succ(succ(0))),succ(0)). -> true
%% ?- greater_than(succ(succ(0)),succ(succ(succ(0)))). -> no

greater_than(succ(X),0).
greater_than(succ(X),succ(Y)) :-
  greater_than(X,Y).

%% Exercise 3.3
%% Binary trees are trees where all internal nodes have exactly two childres. The
%% smallest binary trees consist of only one leaf node. We will represent leaf
%% nodes as leaf(Label). For instance, leaf(3) and leaf(7) are leaf nodes, and
%% therefore small binary trees. Given two binary trees B1 and B2 we can combine
%% them into one binary tree using the predicate tree: tree(B1,B2). So, from the
%% leaves leaf(1) and leaf(2) we can build the binary tree tree(leaf(1),
%% leaf(2)). And from the binary trees tree(leaf(1), leaf(2)) and leaf(4) we can
%% build the binary tree tree(tree(leaf(1), leaf(2)), leaf(4)).

%% Now, define a predicate swap/2, which produces a mirror image of the binary
%% tree that is its first argument. For example:
%% ?- swap(tree(tree(leaf(1), leaf(2)), leaf(4)),T).
%% T = tree(leaf(4), tree(leaf(2), leaf(1))).
%% true

swap(leaf(X), leaf(X)).

swap(tree(X, Y), tree(SwappedY, SwappedX)) :-
    swap(X, SwappedX),
    swap(Y, SwappedY).

%% Exercise 3.4
%% In the lecture, we saw the predicate
%% descend(X,Y) :- child(X,Y).
%% descend(X,Y) :- child(X,Z),
%%                 descend(Z,Y).
%% Could we have formulated this predicate as follows?
%% descend(X,Y) :- child(X,Y).
%% descend(X,Y) :- descend(X,Z),
%%                 descend(Z,Y).

%% Compare the declarative and the procedural meaning of this predicate
%% definition.
%% Hint: What happens when you ask the query descend(rose,martha)?

%% Declarative: true if Y is a direct child of X. True if there exists a Z which
%% Y descends and Z descends X.

%% Procedural: If Y is not a child of X, see if any of X's descendants have Y as
%% a descendant.

%% In all false cases it will loop forever (Out of local stack).
%% E.g. when calling descend(rose, martha). it will keep doing the following:

%% Call: descend(rose, _G408) ?
%% Call: child(rose, _G408) ?
%% Fail: child(rose, _G408) ?
%% Redo: descend(rose, _G408) ?

%% Exercise 3.5
%% We have the following knowledge base:

directTrain(nancy,metz).
directTrain(metz,fahlquemont).
directTrain(fahlquemont,stAvold).
directTrain(stAvold,forbach).
directTrain(forbach,saarbruecken).
directTrain(saarbruecken,dudweiler).
directTrain(freyming,forbach).

%% That is, this knowledge base holds facts about towns it is possible to travel
%% between by taking a direct train. But of course, we can travel further by
%% `chaining together' direct train journeys. Write a recursive predicate
%% travelBetween/2 that tells us when we can travel by train between two
%% towns. For example, when given the query
%% travelBetween(nancy,saarbruecken).
%% it should reply `yes'.

travelBetween(X,Y) :- directTrain(X,Y).
travelBetween(X,Y) :-
  directTrain(X,Z),
  travelBetween(Z,Y).

%% It is, furthermore, plausible to assume that whenever it is possible to take
%% a direct train from A to B, it is also possible to take a direct train from B
%% to A. Can you encode this in Prolog? You program should e.g. answer `yes' to
%% the following query:
%% travelBetween(saarbruecken,nancy).
%% Do you see any problems you program may run into?

travelBetween(X,Y) :- directTrain(X,Y).
travelBetween(X,Y) :- directTrain(Y,X).
travelBetween(X,Y) :-
  directTrain(X,Z),
  travelBetween(Z,Y).
travelBetween(X,Y) :-
  directTrain(Z,X),
  travelBetween(Z, Y).

%% You will end up in infinite loops since you can go both directions, so it is
%% possible to keep calling the same function over and over.

```


<br/>
<hr/>
<br/>
<h2 id="15"> 15. chapter-3/practical-session.pl</h2>

``` prolog
%% Chapter 3 - Practical Session

%% 1. Load descend1.pl, turn on trace, and pose the query
%% descend(martha,laura). This is the query that was discussed in the
%% notes. Step through the trace, and relate what you see on the screen to the
%% discussion in the text.

%% [trace]  ?- descend(martha, laura).
%%    Call: (6) descend(martha, laura) ?
%%    Call: (7) child(martha, laura) ? check if laura is direct child of martha
%%    Fail: (7) child(martha, laura) ?
%%    Redo: (6) descend(martha, laura) ?
%%    Call: (7) child(martha, _G385) ? Z = _G385
%%    Exit: (7) child(martha, charlotte) ? Z = charlotte
%%    Call: (7) descend(charlotte, laura) ? recursive call
%%    Call: (8) child(charlotte, laura) ?
%%    Fail: (8) child(charlotte, laura) ?
%%    Redo: (7) descend(charlotte, laura) ?
%%    Call: (8) child(charlotte, _G385) ?
%%    Exit: (8) child(charlotte, caroline) ? Z = caroline
%%    Call: (8) descend(caroline, laura) ?
%%    Call: (9) child(caroline, laura) ? laura is a child of caroline, success
%%    Exit: (9) child(caroline, laura) ?
%%    Exit: (8) descend(caroline, laura) ?
%%    Exit: (7) descend(charlotte, laura) ?
%%    Exit: (6) descend(martha, laura) ? return
%% true

%% _G385 is an anon variable which is recursively being set to and tested with
%% Martha's descendants.

%% 2. Still with trace on, pose the query descend(martha,rose) and count how
%% many steps it takes Prolog to work out the answer (that is, how many times do
%% you have to hit the return key). Now turn trace off and pose the query
%% descend(X,Y). How many answers are there?

%% 26 steps to return true and 13 to check for further answers -> false.
%% total: 39 steps, 2 answers.

%% 3. Load descend2.pl. This, remember, is the variant of descend1.pl in which
%% the order of both clauses is switched, and in addition, the order of the two
%% goals in the recursive goals is switched too. Because of this, even for such
%% simple queries as descend(martha,laura), Prolog will not terminate. Step
%% through an example, using trace, to confirm this.

%% Should insert trace output.

%% But wait! There are two more variants of descend1.pl that we have not considered. For a start, we could have written the recursive clause as follows:

descend(X,Y) :- child(X,Y).
descend(X,Y) :- descend(Z,Y),
               child(X,Z).

%% Let us call this variant descend3.pl. And one further possibility remains: we
%% could have written the recursive definition as follows:

descend(X,Y) :- child(X,Z), descend(Z,Y).
descend(X,Y) :- child(X,Y).

%% Let us call this variant descend4.pl.

%% Create (or download from the internet) the files descend3.pl and
%% descend4.pl. How do they compare to descend1.pl and descend2.pl? Can they
%% handle the query descend(martha,rose)? Can they handle queries involving
%% variables? How many steps do they need to find an answer? Are they slower or
%% faster than descend1.pl?

%% descend3.pl gives all true answers but runs out of local stack when
%% searching for a false descend().

%% descend4.pl only gives correct answers both when true and false.

%% The important thing to notice here is that when we have a p :- p, q
%% clause. There is a risk of getting an infinite loop for false results since
%% it will keep calling p on end.

%% descend4.pl is slower when second parameter is a child of the first, but
%% otherwise faster since it starts looking for descendants rather than
%% children. (Haven't tested but pretty sure'ish).

%% Draw the search trees for descend2.pl, descend3.pl and descend4.pl (the one
%% for descend1.pl was given in the text) and compare them. Make sure you
%% understand why the programs behave the way they do.

%% !!! NOT DONE !!! Cba to draw an ascii tree right now.

%% 5. Finally, load the file numeral1.pl. Turn on trace, and make sure that you
%% understand how Prolog handles both specific queries (such as
%% numeral(succ(succ(0)))) and queries involving variables (such as numeral(X)).

%% [trace]  ?- numeral(succ(succ(0))).
%%   Call: (6) numeral(succ(succ(0))) ?
%%   Call: (7) numeral(succ(0)) ?
%%   Call: (8) numeral(0) ?
%%   Exit: (8) numeral(0) ?
%%   Exit: (7) numeral(succ(0)) ?
%%   Exit: (6) numeral(succ(succ(0))) ?
%% true.

%% Basically it peels away each succ layer until it reaches numeral(0). and then
%% traverses back up returning true.

%% [trace]  ?- numeral(X).
%%   Call: (6) numeral(_G398) ?
%%   Exit: (6) numeral(0) ?
%% X = 0 ;
%%   Redo: (6) numeral(_G398) ?
%%   Call: (7) numeral(_G462) ?
%%   Exit: (7) numeral(0) ?
%%   Exit: (6) numeral(succ(0)) ?
%% X = succ(0) ;
%%   Redo: (7) numeral(_G462) ?
%%   Call: (8) numeral(_G464) ?
%%   Exit: (8) numeral(0) ?
%%   Exit: (7) numeral(succ(0)) ?
%%   Exit: (6) numeral(succ(succ(0))) ?
%% X = succ(succ(0)) ;
%%   Redo: (8) numeral(_G464) ?
%%   Call: (9) numeral(_G466) ?
%%   Exit: (9) numeral(0) ?
%%   Exit: (8) numeral(succ(0)) ?
%%   Exit: (7) numeral(succ(succ(0))) ?
%%   Exit: (6) numeral(succ(succ(succ(0)))) ?
%% X = succ(succ(succ(0))) .

%% Starts out returning the simplest answer which is the numeral(0) case -> X =
%% 0. It then looks to see if there are other possibilities, here we have
%% numeral(succ(X)) :- numeral(X). so it returns succ(0). and continues this way
%% enumerating succ(....succ(0)...).

%% Programming exercises.

%% 1. Imagine that the following knowledge base describes a maze. The facts
%% determine which points are connected, i.e., from which point you can get to
%% which other point in one step. Furthermore, imagine that all paths are
%% one-way streets, so that you can only walk them in one direction. So, you can
%% get from point 1 to point 2, but not the other way round.

connected(1,2).
connected(3,4).
connected(5,6).
connected(7,8).
connected(9,10).
connected(12,13).
connected(13,14).
connected(15,16).
connected(17,18).
connected(19,20).
connected(4,1).
connected(6,3).
connected(4,7).
connected(6,11).
connected(14,9).
connected(11,15).
connected(16,12).
connected(14,17).
connected(16,19).

%% Write a predicate path/2 that tells you from which point in the maze you can
%% get to which other point when chaining together connections given in the
%% above knowledge base. Can you get from point 5 to point 10? Which other point
%% can you get to when starting in point 1? And which points can be reached from
%% point 13?

path(X,Y) :- connected(X,Y).
path(X,Y) :-
  connected(X,Z),
  path(Z, Y).

% path(5, 10). -> true
% path(1, X). -> 2
% path(13, X). -> 14, 9, 17, 10, 18

%% 2. We are given the following knowledge base of travel information:

byCar(auckland,hamilton).
byCar(hamilton,raglan).
byCar(valmont,saarbruecken).
byCar(valmont,metz).

byTrain(metz,frankfurt).
byTrain(saarbruecken,frankfurt).
byTrain(metz,paris).
byTrain(saarbruecken,paris).

byPlane(frankfurt,bangkok).
byPlane(frankfurt,singapore).
byPlane(paris,losAngeles).
byPlane(bangkok,auckland).
byPlane(losAngeles,auckland).

%% Write a predicate travel/2 which determines whether it is possible to travel
%% from one place to another by `chaining together' car, train, and plane
%% journeys. For example, your program should answer `yes' to the query
%% travel(valmont,raglan).

%% Base cases
travel(X,Y) :- byCar(X,Y).
travel(X,Y) :- byPlane(X,Y).
travel(X,Y) :- byTrain(X,Y).

%% Inductive cases
travel(X,Y) :-
  byCar(X,Z),
  travel(Z,Y).
travel(X,Y) :-
  byPlane(X,Z),
  travel(Z,Y).
travel(X,Y) :-
  byTrain(X,Z),
  travel(Z,Y).

%% travel(valmont, raglan):
%% byCar(valmont, metz), byCar(valmont, saarbruecken)
%% byTrain(metz, frankfurt), byTrain(saarbruecken, frankfurt)
%% byPlane(frankfurt, bangkok)
%% byPlane(bangkok, auckland)
%% byCar(auckland, hamilton)
%% byCar(hamilton, raglan)
%% true.

%% 3. So, by using travel/2 to query the above database, you can find out that
%% it is possible to go from Vamont to Raglan. In case you are planning a
%% travel, that's already very good information, but what you would then really
%% want to know is how exactly to get from Valmont to Raglan. Write a predicate
%% travel/3 which tells you how to travel from one place to another. The program
%% should, e.g., answer `yes' to the query
%% travel(valmont,paris,go(valmont,metz,go(metz,paris))) and X =
%% go(valmont,metz,go(metz,paris,go(paris,losAngeles))) to the query
%% travel(valmont,losAngeles,X).

byCar(auckland,hamilton).
byCar(hamilton,raglan).
byCar(valmont,saarbruecken).
byCar(valmont,metz).

byTrain(metz,frankfurt).
byTrain(saarbruecken,frankfurt).
byTrain(metz,paris).
byTrain(saarbruecken,paris).

byPlane(frankfurt,bangkok).
byPlane(frankfurt,singapore).
byPlane(paris,losAngeles).
byPlane(bangkok,auckland).
byPlane(losAngeles,auckland).

%% Base cases
travel(X,Y, go(X,Y)) :- byCar(X,Y).
travel(X,Y, go(X,Y)) :- byPlane(X,Y).
travel(X,Y, go(X,Y)) :- byTrain(X,Y).

%% Inductive cases
travel(X,Y, go(X,Z,G)) :-
  byCar(X,Z),
  travel(Z,Y,G).

travel(X,Y, go(X,Z,G)) :-
  byPlane(X,Z),
  travel(Z,Y,G).

travel(X,Y, go(X,Z,G)) :-
  byTrain(X,Z),
  travel(Z,Y,G).

%% travel(valmont,paris,go(valmont,metz,go(metz,paris))) -> true
%% travel(valmont,losAngeles,X). ->
%% X = go(valmont,metz,go(metz,paris,go(paris,losAngeles))).
%% X = go(valmont,saarbruecken,go(saarbruecken,paris,go(paris,losAngeles))).

%% 4. Extend the predicate travel/3 so that it not only tells you via which
%% other cities you have to go to get from one place to another, but also how,
%% i.e. by car, train, or plane, you get from one city to the next.

byCar(auckland,hamilton).
byCar(hamilton,raglan).
byCar(valmont,saarbruecken).
byCar(valmont,metz).

byTrain(metz,frankfurt).
byTrain(saarbruecken,frankfurt).
byTrain(metz,paris).
byTrain(saarbruecken,paris).

byPlane(frankfurt,bangkok).
byPlane(frankfurt,singapore).
byPlane(paris,losAngeles).
byPlane(bangkok,auckland).
byPlane(losAngeles,auckland).

%% Base cases
travel(X,Y, go(byCar(X,Y))) :- byCar(X,Y).
travel(X,Y, go(byTrain(X,Y))) :- byTrain(X,Y).
travel(X,Y, go(byPlane(X,Y))) :- byPlane(X,Y).

%% Inductive cases
travel(X,Y, go(byCar(X,Z),G)) :-
  byCar(X,Z),
  travel(Z,Y,G).

travel(X,Y, go(byTrain(X,Z),G)) :-
  byTrain(X,Z),
  travel(Z,Y,G).

travel(X,Y, go(byPlane(X,Z),G)) :-
  byPlane(X,Z),
  travel(Z,Y,G).
```


<br/>
<hr/>
<br/>
<h2 id="16"> 16. chapter-4/exercises.pl</h2>

``` prolog
%% Chapter 4 - Exercises

% Exercise 4.1
%% How does Prolog respond to the following queries?

%% [a,b,c,d] = [a,[b,c,d]]. -> false
%% [a,b,c,d] = [a|[b,c,d]]. -> true
%% [a,b,c,d] = [a,b,[c,d]]. -> false
%% [a,b,c,d] = [a,b|[c,d]]. -> true
%% [a,b,c,d] = [a,b,c,[d]]. -> false
%% [a,b,c,d] = [a,b,c|[d]]. -> true
%% [a,b,c,d] = [a,b,c,d,[]]. -> false
%% [a,b,c,d] = [a,b,c,d|[]]. -> true
%% [] = _. -> true
%% [] = [_]. -> false
%% [] = [_|[]]. -> false

%%Exercise  4.2 Which of the following are syntactically correct lists? 
%%If the representation is correct, how many elements does the list have?

%%[1|[2,3,4]]               correct - 4 elements 
%%[1,2,3|[]]                correct - 3 elements
%%[1|2,3,4]                 syntactically incorrect
%%[1|[2|[3|[4]]]]           correct - 4 elements
%%[1,2,3,4|[]]              correct - 4 elements
%%[[]|[]]                   correct - 1 element
%%[[1,2]|4]                 syntactically incorrect
%%[[1,2],[3,4]|[5,6,7]]     correct - 5 elements

%%Exercise  4.3 Write a predicate second(X,List) which checks whether X is the second element of List .
second(X,List) :- List = [_,X|_].

%%4.4 Write a predicate swap12(List1,List2) which checks whether List1 is identical to List2 ,
%%except that the first two elements are exchanged.

swap12(L1,L2) :- L1 = [L1a,L1b|T], L2 = [L1b,L1a|T].

%% Exercise 4.5
%% Suppose we are given a knowledge base with the following facts:

tran(eins,one).
tran(zwei,two).
tran(drei,three).
tran(vier,four).
tran(fuenf,five).
tran(sechs,six).
tran(sieben,seven).
tran(acht,eight).
tran(neun,nine).

%% Write a predicate listtran(G,E) which translates a list of German number
%% words to the corresponding list of English number words. For example:

%% listtran([eins,neun,zwei],X).
%% should give:
%% X = [one,nine,two].

%% Your program should also work in the other direction. For example, if you
%% give it the query

%% listtran(X,[one,seven,six,two]).
%% it should return:
%% X = [eins,sieben,sechs,zwei].

%% Hint: to answer this question, first ask yourself `How do I translate the
%% empty list of number words?'. That's the base case. For non-empty lists,
%% first translate the head of the list, then use recursion to translate the
%% tail.

listtran([], []).
listtran([Hg | Tg], [He | Te]) :-
  tran(Hg, He),
  listtran(Tg, Te).

%% Exercise 4.6

%% Write a predicate twice(In,Out) whose left argument is a list, and whose
%% right argument is a list consisting of every element in the left list written
%% twice. For example, the query
%% twice([a,4,buggle],X).
%% should return
%% X = [a,a,4,4,buggle,buggle]).
%% And the query
%% twice([1,2,1,1],X).
%% should return
%% X = [1,1,2,2,1,1,1,1].

%% Hint: to answer this question, first ask yourself
%% `What should happen when the first argument is the empty list?'. That's the
%% base case. For non-empty lists, think about what you should do with the head,
%% and use recursion to handle the tail.

twice([],[]).
twice([Ha | Ta], [Ha, Ha | Tb]) :- twice(Ta, Tb).

%% Exercise 4.7
%% Draw the search trees for the following three queries:

%% ?- member(a,[c,b,a,y]).

%% ------------------------------
%% | ?- member(a, [c, b, a, y]) |
%% ------------------------------
%%               |
%%               |
%% ------------------------------
%% | ?- member(a, [b, a, y])    |
%% ------------------------------
%%               |
%%               |
%% ------------------------------
%% | ?- member(a, [a, y])       |
%% ------------------------------
%%               |
%%               |
%% ------------------------------
%% |                            |
%% ------------------------------

%% ?- member(x,[a,b,c]).

%% ---------------------------
%% | ?- member(x, [a, b, c]) |
%% ---------------------------
%%              |
%%              |
%% ---------------------------
%% | ?- member(x, [b, c])    |
%% ---------------------------
%%              |
%%              |
%% ---------------------------
%% | ?- member(x, [c])       |
%% ---------------------------
%%              |
%%              |
%% ---------------------------
%% | ?- member(x, [])        |
%% ---------------------------
%%              |
%%              X

%% ?- member(X,[a,b,c]).

%%                ---------------------------
%%                | ?- member(X, [a, b, c]) |
%%                ---------------------------
%%                    /                 \
%%          X = a    /                   \    X = _G1
%%                  /                     \
%% ---------------------------   ---------------------------
%% |                         |   | ?- memeber(_G1, [b, c]) |
%% ---------------------------   ---------------------------
%%                                   /                 \
%%                       _G1 = b    /                   \
%%                                 /                     \
%%                ---------------------------   ---------------------------
%%                |                         |   | ?- member(_G1, [c])     |
%%                ---------------------------   ---------------------------
%%                                                  /                 \
%%                                      _G1 = c    /                   \
%%                                                /                     \
%%                               ---------------------------   ---------------------------
%%                               |                         |   | ?- member(_G1, [])      |
%%                               ---------------------------   ---------------------------
%%                                                                          |
%%                                                                          X

```


<br/>
<hr/>
<br/>
<h2 id="17"> 17. chapter-4/pratical-session.pl</h2>

``` prolog
%% Chapter 4 - Practical Session

%% First, systematically carry out a number of traces on a2b/2 to make sure you
%% fully understand how it works. In particular:

%% 1. Trace some examples, not involving variables, that succeed. E.g., trace
%% the query a2b([a,a,a,a],[b,b,b,b]) and relate the output to the discussion in
%% the text.

%% [trace]  ?- a2b([a,a,a,a],[b,b,b,b]).
%%   Call: (7) a2b([a, a, a, a], [b, b, b, b]) ?
%%   Call: (8) a2b([a, a, a], [b, b, b]) ?
%%   Call: (9) a2b([a, a], [b, b]) ?
%%   Call: (10) a2b([a], [b]) ?
%%   Call: (11) a2b([], []) ?
%%   Exit: (11) a2b([], []) ?
%%   Exit: (10) a2b([a], [b]) ?
%%   Exit: (9) a2b([a, a], [b, b]) ?
%%   Exit: (8) a2b([a, a, a], [b, b, b]) ?
%%   Exit: (7) a2b([a, a, a, a], [b, b, b, b]) ?
%% true.

%% 2. Trace some simple examples that fail. Try examples involving lists of
%% different lengths (such as a2b([a,a,a,a],[b,b,b])) and examples involving
%% symbols other than a and b (such as a2b([a,c,a,a],[b,b,5,4])).

%% [trace]  ?- a2b([a,a,a,a],[b,b,b]).
%%   Call: (7) a2b([a, a, a, a], [b, b, b]) ?
%%   Call: (8) a2b([a, a, a], [b, b]) ?
%%   Call: (9) a2b([a, a], [b]) ?
%%   Call: (10) a2b([a], []) ?
%%   Fail: (10) a2b([a], []) ?
%%   Fail: (9) a2b([a, a], [b]) ?
%%   Fail: (8) a2b([a, a, a], [b, b]) ?
%%   Fail: (7) a2b([a, a, a, a], [b, b, b]) ?
%% false.

%% [trace]  ?- a2b([a,c,a,a],[b,b,5,4]).
%%   Call: (7) a2b([a, c, a, a], [b, b, 5, 4]) ?
%%   Call: (8) a2b([c, a, a], [b, 5, 4]) ?
%%   Fail: (8) a2b([c, a, a], [b, 5, 4]) ?
%%   Fail: (7) a2b([a, c, a, a], [b, b, 5, 4]) ?
%% false.

%% 3. Trace some examples involving variables. For example, try tracing
%% a2b([a,a,a,a],X) and a2b(X,[b,b,b,b]).

%% [trace]  ?- a2b([a,a,a,a],X).
%%   Call: (7) a2b([a, a, a, a], _G692) ?
%%   Call: (8) a2b([a, a, a], _G773) ?
%%   Call: (9) a2b([a, a], _G776) ?
%%   Call: (10) a2b([a], _G779) ?
%%   Call: (11) a2b([], _G782) ?
%%   Exit: (11) a2b([], []) ?
%%   Exit: (10) a2b([a], [b]) ?
%%   Exit: (9) a2b([a, a], [b, b]) ?
%%   Exit: (8) a2b([a, a, a], [b, b, b]) ?
%%   Exit: (7) a2b([a, a, a, a], [b, b, b, b]) ?
%% X = [b, b, b, b].

%% [trace]  ?- a2b(X,[b,b,b,b]).
%%   Call: (7) a2b(_G691, [b, b, b, b]) ?
%%   Redo: (7) a2b(_G691, [b, b, b, b]) ?
%%   Call: (8) a2b(_G773, [b, b, b]) ?
%%   Redo: (8) a2b(_G773, [b, b, b]) ?
%%   Call: (9) a2b(_G776, [b, b]) ?
%%   Redo: (9) a2b(_G776, [b, b]) ?
%%   Call: (10) a2b(_G779, [b]) ?
%%   Redo: (10) a2b(_G779, [b]) ?
%%   Call: (11) a2b(_G782, []) ?
%%   Exit: (11) a2b([], []) ?
%%   Exit: (10) a2b([a], [b]) ?
%%   Exit: (9) a2b([a, a], [b, b]) ?
%%   Exit: (8) a2b([a, a, a], [b, b, b]) ?
%%   Exit: (7) a2b([a, a, a, a], [b, b, b, b]) ?
%% X = [a, a, a, a]

%% 4. Make sure you understand what happens when both arguments in the query are
%% variables. For example, carry out a trace on the query a2b(X,Y).

%% [trace]  ?- a2b(X, Y).
%%   Call: (7) a2b(_G679, _G680) ?
%%   Exit: (7) a2b([], []) ?
%% X = Y, Y = [] ;
%%   Redo: (7) a2b(_G679, _G680) ?
%%   Call: (8) a2b(_G761, _G764) ?
%%   Exit: (8) a2b([], []) ?
%%   Exit: (7) a2b([a], [b]) ?
%% X = [a],
%% Y = [b] ;
%%   Redo: (8) a2b(_G761, _G764) ?
%%   Call: (9) a2b(_G767, _G770) ?
%%   Exit: (9) a2b([], []) ?
%%   Exit: (8) a2b([a], [b]) ?
%%   Exit: (7) a2b([a, a], [b, b]) ?
%% X = [a, a],
%% Y = [b, b] ;
%%   Redo: (9) a2b(_G767, _G770) ?
%%   Call: (10) a2b(_G773, _G776) ?
%%   Exit: (10) a2b([], []) ?
%%   Exit: (9) a2b([a], [b]) ?
%%   Exit: (8) a2b([a, a], [b, b]) ?
%%   Exit: (7) a2b([a, a, a], [b, b, b]) ?
%% X = [a, a, a],
%% Y = [b, b, b]

%% 5. Carry out a series of similar traces involving member. That is, carry out
%% traces involving simple queries that succeed (such as member(a,[1,2,a,b])),
%% simple queries that fail (such as member(z,[1,2,a,b])), and queries involving
%% variables (such as member(X,[1,2,a,b])). In all cases, make sure that you
%% understand why the recursion halts.

%% [trace]  ?- myMember(a,[1,2,a,b]).
%%   Call: (6) myMember(a, [1, 2, a, b]) ?
%%   Call: (7) myMember(a, [2, a, b]) ?
%%   Call: (8) myMember(a, [a, b]) ?
%%   Exit: (8) myMember(a, [a, b]) ?
%%   Exit: (7) myMember(a, [2, a, b]) ?
%%   Exit: (6) myMember(a, [1, 2, a, b]) ?
%% true

%% [trace]  ?- myMember(z,[1,2,a,b]).
%%   Call: (6) myMember(z, [1, 2, a, b]) ?
%%   Call: (7) myMember(z, [2, a, b]) ?
%%   Call: (8) myMember(z, [a, b]) ?
%%   Call: (9) myMember(z, [b]) ?
%%   Call: (10) myMember(z, []) ?
%%   Fail: (10) myMember(z, []) ?
%%   Fail: (9) myMember(z, [b]) ?
%%   Fail: (8) myMember(z, [a, b]) ?
%%   Fail: (7) myMember(z, [2, a, b]) ?
%%   Fail: (6) myMember(z, [1, 2, a, b]) ?
%% false.

%% [trace]  ?- myMember(X,[1,2,a,b]).
%%   Call: (6) myMember(_G935, [1, 2, a, b]) ?
%%   Exit: (6) myMember(1, [1, 2, a, b]) ?
%% X = 1 ;
%%   Redo: (6) myMember(_G935, [1, 2, a, b]) ?
%%   Call: (7) myMember(_G935, [2, a, b]) ?
%%   Exit: (7) myMember(2, [2, a, b]) ?
%%   Exit: (6) myMember(2, [1, 2, a, b]) ?
%% X = 2 ;
%%   Redo: (7) myMember(_G935, [2, a, b]) ?
%%   Call: (8) myMember(_G935, [a, b]) ?
%%   Exit: (8) myMember(a, [a, b]) ?
%%   Exit: (7) myMember(a, [2, a, b]) ?
%%   Exit: (6) myMember(a, [1, 2, a, b]) ?
%% X = a ;
%%   Redo: (8) myMember(_G935, [a, b]) ?
%%   Call: (9) myMember(_G935, [b]) ?
%%   Exit: (9) myMember(b, [b]) ?
%%   Exit: (8) myMember(b, [a, b]) ?
%%   Exit: (7) myMember(b, [2, a, b]) ?
%%   Exit: (6) myMember(b, [1, 2, a, b]) ?
%% X = b ;
%%   Redo: (9) myMember(_G935, [b]) ?
%%   Call: (10) myMember(_G935, []) ?
%%   Fail: (10) myMember(_G935, []) ?
%%   Fail: (9) myMember(_G935, [b]) ?
%%   Fail: (8) myMember(_G935, [a, b]) ?
%%   Fail: (7) myMember(_G935, [2, a, b]) ?
%%   Fail: (6) myMember(_G935, [1, 2, a, b]) ?
%% false.

%% Having done this, try the following.

%% 1. Write a 3-place predicate combine1 which takes three lists as arguments
%% and combines the elements of the first two lists into the third as follows:

%% ?- combine1([a,b,c],[1,2,3],X).
%% X = [a,1,b,2,c,3]

%% ?- combine1([foo,bar,yip,yup],[glub,glab,glib,glob],Result).
%% Result = [foo,glub,bar,glab,yip,glib,yup,glob]

combine1([],[],[]).
combine1([H1 | T1], [H2 | T2], [H1, H2 | T3]) :- combine1(T1, T2, T3).

%% 2. Now write a 3-place predicate combine2 which takes three lists as
%% arguments and combines the elements of the first two lists into the third as
%% follows:

%% ?- combine2([a,b,c],[1,2,3],X).
%% X = [[a,1],[b,2],[c,3]]

%% ?- combine2([foo,bar,yip,yup],[glub,glab,glib,glob],Result).
%% Result = [[foo,glub],[bar,glab],[yip,glib],[yup,glob]]

combine2([],[],[]).
combine2([H1 | T1], [H2 | T2], [[H1, H2] | T3]) :- combine2(T1, T2, T3).

%% 3. Finally, write a 3-place predicate combine3 which takes three lists as
%% arguments and combines the elements of the first two lists into the third as
%% follows:

%% ?- combine3([a,b,c],[1,2,3],X).
%% X = [join(a,1),join(b,2),join(c,3)]

%% ?- combine3([foo,bar,yip,yup],[glub,glab,glib,glob],R).
%% R = [join(foo,glub),join(bar,glab),join(yip,glib),join(yup,glob)]

combine3([],[],[]).
combine3([H1 | T1], [H2 | T2], [join(H1, H2) | T3]) :- combine3(T1, T2, T3).

%% Now, you should have a pretty good idea of what the basic pattern of
%% predicates for processing lists looks like. Here are a couple of list
%% processing exercises that are a bit more interesting. Hint: you can of course
%% use predicates that we defined earlier, like e.g. member/2 in your predicate
%% definition.

%% 1. Write a predicate mysubset/2 that takes two (unsorted?) lists (of
%% constants) as arguments and checks, whether the first list is a subset of the
%% second.

mysubset([],_).
mysubset([H1 | T1], T2) :-
  member(H1, T2),
  mysubset(T1, T2).

%% 2. Write a predicate mysuperset/2 that takes two (unsorted?) lists as
%% arguments and checks, whether the first list is a superset of the second.

mysuperset(_,[]).
mysuperset(T1, [H2 | T2]) :-
  member(H2, T1),
  mysuperset(T1, T2).
```


<br/>
<hr/>
<br/>
<h2 id="18"> 18. chapter-5/exercises.pl</h2>

``` prolog
%% Chapter 5 - Exercises

%% Exercise 5.1

%% How does Prolog respond to the following queries?
%% X = 3 * 4. -> X = 3*4.
%% X is 3*4. -> X = 12.
%% 4 is X. -> ERROR: is/2: Arguments are not sufficiently instantiated
%% X = Y. -> X = Y.
%% 3 is 1+2. -> true.
%% 3 is +(1,2). -> true.
%% 3 is X+2. -> ERROR: is/2: Arguments are not sufficiently instantiated
%% X is 1+2. -> X = 3.
%% 1+2 is 1+2. -> false.
%% is(X,+(1,2)). -> X = 3.
%% 3+2 = +(3,2). -> true.
%% *(7,5) = 7*5. -> true.
%% *(7,+(3,2)) = 7*(3+2). -> true.
%% *(7,(3+2)) = 7*(3+2). -> true.
%% *(7,(3+2)) = 7*(+(3,2)). -> true.

%% Exercise 5.2

%% 1. Define a 2-place predicate increment that holds only when its second
%% argument is an integer one larger than its first argument. For example,
%% increment(4,5) should hold, but increment(4,6) should not.

increment(X,Y) :- Y is X + 1.

%% ?- increment(4,5). -> true
%% ?- increment(4,6). -> false
%% ?- increment(4,X). -> X = 5.

%% 2. Define a 3-place predicate sum that holds only when its third argument is
%% the sum of the first two arguments. For example, sum(4,5,9) should hold, but
%% sum(4,6,12)should not.

sum(X,Y,Z) :- Z is (X + Y).

%% ?- sum(4,5,9). -> true.
%% ?- sum(4,6,12). -> false.

%% Exercise 5.3

%% Write a predicate addone/2 whose first argument is a list of integers, and
%% whose second argument is the list of integers obtained by adding 1 to each
%% integer in the first list. For example, the query

%% addone([1,2,7,2],X).
%% should give
%% X = [2,3,8,3].

addone([],[]).
addone([H1|T1], [H2|T2]) :-
  is(H2,+(H1,1)),
  addone(T1,T2).

%% ?- addone([1,2,7,2],X).
%% X = [2, 3, 8, 3].
```


<br/>
<hr/>
<br/>
<h2 id="19"> 19. chapter-5/practical-session.pl</h2>

``` prolog
%% Chapter 5 - Practical Session

%% The purpose of Practical Session 5 is to help you get familiar with Prolog's
%% arithmetic capabilities, and to give you some further practice in list
%% manipulation. To this end, we suggest the following programming exercises:

%% 1. In the text we discussed the 3-place predicate accMax which which returned
%% the maximum of a list of integers. By changing the code slightly, turn this
%% into a 3-place predicate accMin which returns the minimum of a list of
%% integers.

%% base case
accMin([],A,A).

%% recursive cases
%% greater than
accMin([H|T],A,Min) :-
    H < A,
    accMin(T,H,Min).

%% less than
accMin([H|T],A,Min) :-
    H >= A,
    accMin(T,A,Min).

min(List, Min) :-
  List = [H|_],
  accMin(List, H, Min).

%% 2. In mathematics, an n-dimensional vector is a list of numbers of length
%% n. For example, [2,5,12] is a 3-dimensional vector, and [45,27,3,-4,6] is a
%% 5-dimensional vector. One of the basic operations on vectors is scalar
%% multiplication. In this operation, every element of a vector is multiplied by
%% some number. For example, if we scalar multiply the 3-dimensional vector
%% [2,7,4] by 3 the result is the 3-dimensional vector [6,21,12]. Write a
%% 3-place predicate scalarMult whose first argument is an integer, whose second
%% argument is a list of integers, and whose third argument is the result of
%% scalar multiplying the second argument by the first. For example, the query

%% scalarMult(3,[2,7,4],Result).
%% should yield
%% Result = [6,21,12]

%% base case
scalarmult(Alpha, [], []).
%% inductive case
scalarmult(Alpha, [H1|T1], [H2|T2]) :-
  is(H2,*(H1,Alpha)),
  scalarmult(Alpha, T1, T2).

%% 3. Another fundamental operation on vectors is the dot product. This
%% operation combines two vectors of the same dimension and yields a number as a
%% result. The operation is carried out as follows: the corresponding elements
%% of the two vectors are multiplied, and the results added. For example, the
%% dot product of [2,5,6] and [3,4,1] is 6+20+6, that is, 32. Write a 3-place
%% predicate dot whose first argument is a list of integers, whose second
%% argument is a list of integers of the same length as the first, and whose
%% third argument is the dot product of the first argument with the second. For
%% example, the query

%% dot([2,5,6],[3,4,1],Result).
%% should yield
%% Result = 32

%% base case
accDot([],[],A,A).

%% inductive case
accDot([H1|T1], [H2|T2], A, Result) :-
  is(Anew,+(A,*(H1,H2))),
  accDot(T1, T2, Anew, Result).

dot(Vector1, Vector2, Result) :-
  accDot(Vector1, Vector2, 0, Result).

```


<br/>
<hr/>
<br/>
<h2 id="20"> 20. chapter-6/exercises.pl</h2>

``` prolog
%% Chapter 6 - Exercises

%% Exercise 6.1

%% Let's call a list doubled if it is made of two consecutive blocks of elements
%% that are exactly the same. For example, [a,b,c,a,b,c] is doubled (it's made
%% up of [a,b,c]followed by [a,b,c]) and so is [foo,gubble,foo,gubble]. On the
%% other hand, [foo,gubble,foo] is not doubled. Write a predicate doubled(List)
%% which succeeds when List is a doubled list.



%% doubled
doubled(List) :- append(X, X, List).

%% Exercise 6.2

%% A palindrome is a word or phrase that spells the same forwards and
%% backwards. For example, `rotator', `eve', and `nurses run' are all
%% palindromes. Write a predicate palindrome(List), which checks whether List is
%% a palindrome. For example, to the queries

%% ?- palindrome([r,o,t,a,t,o,r]).
%% and
%% ?- palindrome([n,u,r,s,e,s,r,u,n]).
%% Prolog should respond `yes', but to the query
%% ?- palindrome([n,o,t,h,i,s]).
%% Prolog should respond `no'.

%% Helper functions

%% reverse
%% base case
accRev([],A,A).
%% inductive case
accRev([H|T],A,R) :- accRev(T,[H|A],R).
%% main
rev(L,R) :- accRev(L,[],R).

%% Actual function

%% palindrome
%% main
palindrome(List) :-
  rev(List, List).

%% Exercise 6.3

%% 1. Write a predicate second(X,List) which checks whether X is the second
%% element of List.

second(X, [Y, X | List]).

%% 2. Write a predicate swap12(List1,List2) which checks whether List1 is
%% identical to List2, except that the first two elements are exchanged.


%% Actual function
swap12([],[]).
swap12([X, Y | List], [Y, X | List]).

%% 3. Write a predicate final(X,List) which checks whether X is the last element
%% of List.

%% Helper function

%% reverse
%% base case
accRev([],A,A).
%% inductive case
accRev([H|T],A,R) :- accRev(T,[H|A],R).
%% main
rev(L,R) :- accRev(L,[],R).

%% Actual function
final([],[]).
final(X,List) :-
  rev(List, [X|_]).

%% 4. Write a predicate toptail(InList,Outlist) which says `no' if inlist is a
%% list containing fewer than 2 elements, and which deletes the first and the
%% last elements of Inlist and returns the result as Outlist, when Inlist is a
%% list containing at least 2 elements. For example:

%% toptail([a],T).
%% no

%% toptail([a,b],T).
%% T=[]

%% toptail([a,b,c],T).
%% T=[b]

%% Hint: here's where append comes in useful.

toptail(InList, OutList):-
    append([_|OutList],[_], InList).

%% 5. Write a predicate swapfl(List1,List2) which checks whether List1 is
%% identical to List2, except that the first and last elements are
%% exchanged. Hint: here's where append comes in useful again.

%% Helper function

%% equal
%% base case
equal([],[]).
%% inductive case
equal([H1|T1], [H1|T2]) :- equal(T1,T2).

%% reverse
%% base case
accRev([],A,A).
%% inductive case
accRev([H|T],A,R) :- accRev(T,[H|A],R).
%% main
rev(L,R) :- accRev(L,[],R).

chopOffEnds([_|T],Outlist) :-
  rev(T, [_|RevOutlist]),
  rev(RevOutlist,Outlist).

%% Actual function

%% swapfl(List1, List2)
swapfl(List1,List2) :-
  compareFirstLast(List1,List2),
  compareFirstLast(List2,List1),
  chopOffEnds(List1, Outlist1),
  chopOffEnds(List2, Outlist2),
  equal(Outlist1,Outlist2).

compareFirstLast([H|T1],L2) :- rev(L2, [H|_]).

%% Exercise 6.4

%% And here is an exercise for those of you who, like me, like logic puzzles.

%% There is a street with three neighboring houses that all have a different
%% color. They are red, blue, and green. People of different nationalities live
%% in the different houses and they all have a different pet. Here are some more
%% facts about them:

%% The Englishman lives in the red house.
%% The jaguar is the pet of the Spanish family.
%% The Japanese lives to the right of the snail keeper.
%% The snail keeper lives to the left of the blue house.
%% Who keeps the zebra?

%% Define a predicate zebra/1 that tells you the nationality of the owner of the
%% zebra.

%% Hint: Think of a representation for the houses and the street. Code the four
%% constraints in Prolog. member and sublist might be useful predicates.

%% I believe this is the correct answer:
%% (green, Japanese, zebra) | (red, English, snail) | (blue, Spanish, jaguar)

prefix(P,L) :- append(P,_,L).
suffix(S,L) :- append(_,S,L).
sublist(SubL,L) :- suffix(S,L), prefix(SubL,S).

zebra_owner(ZebraOwner) :-
    Street = [_, _, _],
    member(house(red, englishman, _), Street),
    member(house(_, spanish, jaguar), Street),
    member(house(_, ZebraOwner, zebra), Street),
    sublist([house(_, _, snail), house(_, japanese, _)], Street),
    sublist([house(_, _, snail), house(blue, _, _)], Street).

```


<br/>
<hr/>
<br/>
<h2 id="21"> 21. chapter-6/practical-session.pl</h2>

``` prolog
%% Chapter 6 - Practical Session

%% The following traces will help you get to grips with the predicates discussed
%% in the text:

%% 1. Carry out traces of append with the first two arguments instantiated, and
%% the third argument uninstantiated. For example,
%% append([a,b,c],[[],[2,3],b],X) Make sure the basic pattern is clear.

myAppend([],L,L).
myAppend([H|T], L2, [H|L3]) :- myAppend(T,L2,L3).

%% [trace]  ?- myAppend([a,b,c],[[],[2,3],b],X).
%%   Call: (6) myAppend([a, b, c], [[], [2, 3], b], _G648) ?
%%   Call: (7) myAppend([b, c], [[], [2, 3], b], _G724) ?
%%   Call: (8) myAppend([c], [[], [2, 3], b], _G727) ?
%%   Call: (9) myAppend([], [[], [2, 3], b], _G730) ?
%%   Exit: (9) myAppend([], [[], [2, 3], b], [[], [2, 3], b]) ?
%%   Exit: (8) myAppend([c], [[], [2, 3], b], [c, [], [2, 3], b]) ?
%%   Exit: (7) myAppend([b, c], [[], [2, 3], b], [b, c, [], [2, 3], b]) ?
%%   Exit: (6) myAppend([a, b, c], [[], [2, 3], b], [a, b, c, [], [2, 3], b]) ?
%% X = [a, b, c, [], [2, 3], b].

%% 2. Next, carry out traces on append as used to split up a list, that is, with
%% the first two arguments given as variables, and the last argument
%% instantiated. For example, append(L,R,[foo,wee,blup]).

%% [trace]  ?- myAppend(L,R,[foo,wee,blup]).
%%    Call: (6) myAppend(_G949, _G950, [foo, wee, blup]) ?
%%    Exit: (6) myAppend([], [foo, wee, blup], [foo, wee, blup]) ?
%% L = [],
%% R = [foo, wee, blup] ;
%%    Redo: (6) myAppend(_G949, _G950, [foo, wee, blup]) ?
%%    Call: (7) myAppend(_G1024, _G950, [wee, blup]) ?
%%    Exit: (7) myAppend([], [wee, blup], [wee, blup]) ?
%%    Exit: (6) myAppend([foo], [wee, blup], [foo, wee, blup]) ?
%% L = [foo],
%% R = [wee, blup] ;
%%    Redo: (7) myAppend(_G1024, _G950, [wee, blup]) ?
%%    Call: (8) myAppend(_G1027, _G950, [blup]) ?
%%    Exit: (8) myAppend([], [blup], [blup]) ?
%%    Exit: (7) myAppend([wee], [blup], [wee, blup]) ?
%%    Exit: (6) myAppend([foo, wee], [blup], [foo, wee, blup]) ?
%% L = [foo, wee],
%% R = [blup] ;
%%    Redo: (8) myAppend(_G1027, _G950, [blup]) ?
%%    Call: (9) myAppend(_G1030, _G950, []) ?
%%    Exit: (9) myAppend([], [], []) ?
%%    Exit: (8) myAppend([blup], [], [blup]) ?
%%    Exit: (7) myAppend([wee, blup], [], [wee, blup]) ?
%%    Exit: (6) myAppend([foo, wee, blup], [], [foo, wee, blup]) ?
%% L = [foo, wee, blup],
%% R = [] ;
%%    Redo: (9) myAppend(_G1030, _G950, []) ?
%%    Fail: (9) myAppend(_G1030, _G950, []) ?
%%    Fail: (8) myAppend(_G1027, _G950, [blup]) ?
%%    Fail: (7) myAppend(_G1024, _G950, [wee, blup]) ?
%%    Fail: (6) myAppend(_G949, _G950, [foo, wee, blup]) ?
%% false.

%% 3. Carry out some traces on prefix and suffix. Why does prefix find shorter
%% lists first, and suffix longer lists first?

%% Because it uses myAppend whose base case is myAppend([],L,L). so the easiest
%% solution to prefix is the empty list and the whole list for suffix.

%% 4. Carry out some traces on sublist. As we said in the text, via backtracking
%% this predicate generates all possible sublists, but as you'll see, it
%% generates several sublists more than once. Do you understand why?

myAppend([],L,L).
myAppend([H|T], L2, [H|L3]) :- myAppend(T,L2,L3).
prefix(P,L) :- myAppend(P,_,L).
suffix(S,L) :- myAppend(_,S,L).
sublist(SubL,L) :- suffix(S,L), prefix(SubL,S).

%% ?- sublist(S,[1,2,3,4]).
%% S = [] ;
%% S = [1] ;
%% S = [1, 2] ;
%% S = [1, 2, 3] ;
%% S = [1, 2, 3, 4] ;
%% S = [] ;
%% S = [2] ;
%% S = [2, 3] ;
%% S = [2, 3, 4] ;
%% S = [] ;
%% S = [3] ;
%% S = [3, 4] ;
%% S = [] ;
%% S = [4] ;
%% S = [] ;
%% false.

%% Because it generates all possible sublists of each sublist, making all common
%% prefixes appear more than once (i.e. the empty lists appears n times in a
%% list of n elements).

%% 5. Carry out traces on both naiverev and rev, and compare their behavior.

myAppend([],L,L).
myAppend([H|T], L2, [H|L3]) :- myAppend(T,L2,L3).

naiverev([],[]).
naiverev([H|T],R) :-
  naiverev(T,RevT),
  myAppend(RevT,[H],R).

accRev([H|T],A,R) :- accRev(T,[H|A],R).
accRev([],A,A).

rev(L,R) :- accRev(L,[],R).

%% naive rev:
%% [trace]  ?- naiverev([1,2],R).
%%    Call: (7) naiverev([1, 2], _G344) ?
%%    Call: (8) naiverev([2], _G405) ?
%%    Call: (9) naiverev([], _G405) ?
%%    Exit: (9) naiverev([], []) ?
%%    Call: (9) myAppend([], [2], _G409) ?
%%    Exit: (9) myAppend([], [2], [2]) ?
%%    Exit: (8) naiverev([2], [2]) ?
%%    Call: (8) myAppend([2], [1], _G344) ?
%%    Call: (9) myAppend([], [1], _G407) ?
%%    Exit: (9) myAppend([], [1], [1]) ?
%%    Exit: (8) myAppend([2], [1], [2, 1]) ?
%%    Exit: (7) naiverev([1, 2], [2, 1]) ?
%% R = [2, 1].

%% rev:
%% [trace]  ?- rev([1,2],R).
%%    Call: (7) rev([1, 2], _G396) ?
%%    Call: (8) accRev([1, 2], [], _G396) ?
%%    Call: (9) accRev([2], [1], _G396) ?
%%    Call: (10) accRev([], [2, 1], _G396) ?
%%    Exit: (10) accRev([], [2, 1], [2, 1]) ?
%%    Exit: (9) accRev([2], [1], [2, 1]) ?
%%    Exit: (8) accRev([1, 2], [], [2, 1]) ?
%%    Exit: (7) rev([1, 2], [2, 1]) ?
%% R = [2, 1].

%% Now for some programming work:

%% 1. It is possible to write a one line definition of the member predicate by
%% making use of append. Do so. How does this new version of member compare in
%% efficiency with the standard one?

myAppend([],L,L).
myAppend([H|T], L2, [H|L3]) :- myAppend(T,L2,L3).

%% standard member
myMember(X, [X | T]).
myMember(X, [H | T]) :- myMember(X, T).

%% append member
myAppendMember(X, Ys) :- myAppend(_,[X|_],Ys).

%% myMember just runs through each element of the list while myAppendMember
%% tries to find list that concatenated with X gives Ys. myAppend is slower
%% since it has to do a Call/Redo while myMember only has to do a Call.

%% 2. Write a predicate set(InList,OutList) which takes as input an arbitrary
%% list, and returns a list in which each element of the input list appears only
%% once. For example, the query

%% set([2,2,foo,1,foo, [],[]],X).
%% should yield the result
%% X = [2,foo,1,[]].

%% Hint: use the member predicate to test for repetitions of items you have
%% already found.

%% Helper function (Only there to reverse list before returning).
accRev([H|T],A,R) :- accRev(T,[H|A],R).
accRev([],A,A).
rev(L,R) :- accRev(L,[],R).

%% Actual function
%% accSet(InList,AccList,Outlist).

%% base case
accSet([],L,L).
%% inductive case
accSet([X|Inlist], AccList, Outlist) :-
  not(member(X,AccList)),
  accSet(Inlist,[X|AccList],Outlist).
accSet([X|Inlist], AccList, Outlist) :-
  member(X,AccList),
  accSet(Inlist,AccList,Outlist).
%% main
set(Inlist,Outlist) :-
  rev(TempOutlist, Outlist),
  accSet(Inlist,[],TempOutlist).

%% 3. We `flatten' a list by removing all the square brackets around any lists it
%% contains as elements, and around any lists that its elements contain as
%% element, and so on for all nested lists. For example, when we flatten the
%% list
%% [a,b,[c,d],[[1,2]],foo] we get the list
%% [a,b,c,d,1,2,foo]
%% and when we flatten the list
%% [a,b,[[[[[[[c,d]]]]]]],[[1,2]],foo,[]]
%% we also get
%% [a,b,c,d,1,2,foo].

%% Write a predicate flatten(List,Flat) that holds when the first argument
%% List flattens to the second argument Flat. This exercise can be done without
%% making use of append.

% base case
accFlatten([],L,L).
accFlatten(X, AccList, [X|AccList]) :-
    X \= [],
    X \= [_|_].

%% inductive case
accFlatten([HList|TList], AccList, Flat) :-
    accFlatten(TList, AccList, NewAccList),
    accFlatten(HList, NewAccList, Flat).

%% main
flatten(List, Flat) :-
    accFlatten(List, [], Flat).

%% !!! TO DO !!!

%% 4. Opposite Zip

accRev([H|T],A,R) :- accRev(T,[H|A],R).
accRev([],A,A).
rev(L,R) :- accRev(L,[],R).


zip([], [], []).
zip([HX | TX], [HY | TY], [[HX , HY] | TZ]) :-
  zip(TX, TY, TZ).

revZipSimple(X, Y, R) :-
  rev(Y, RY),
  zip(X, RY, R).

revZip([], [], []).
revZip([HX | TX], [HY | TY], [[HX , HY] | TR]) :-
  revZip(TX, TY, TR).


foo([], [], []).
foo([HX|TX], [HY, TY], [HZ, TZ]) :- foo(TX, TY, TZ).

myAppend([],L,L).
myAppend([H|T], L2, [H|L3]) :- myAppend(T,L2,L3).

```


<br/>
<hr/>
<br/>
<h2 id="22"> 22. chapter-7/exercises.pl</h2>

``` prolog
%% Chapter 7 - Exercises

%% Exercise 7.1
%% Suppose we are working with the following DCG:

s --> foo,bar,wiggle.
foo --> [choo].
foo --> foo,foo.
bar --> mar,zar.
mar --> me,my.
me --> [i].
my --> [am].
zar --> blar,car.
blar --> [a].
car --> [train].
wiggle --> [toot].
wiggle --> wiggle,wiggle.
%% Write down the ordinary Prolog rules that correspond to these DCG rules.

%% s --> foo, bar, wiggle.
s(A, D) :-
    foo(A, B),
    bar(B, C),
    wiggle(C, D).

%% foo --> [choo].
foo([choo | A], A).

%% foo --> foo, foo.
foo(A, C) :-
    foo(A, B),
    foo(B, C).

%% bar --> mar, zar.
bar(A, C) :-
    mar(A, B),
    zar(B, C).

%% mar --> me, my.
mar(A, C) :-
    me(A, B),
    my(B, C).

%% me --> [i].
me([i | A], A).

%% my --> [am].
my([am | A], A).

%% zar -> blar, car.
zar(A, C) :-
    blar(A, B),
    car(B, C).

%% blar -> [a].
blar([a | A], A).

%% car --> [train].
car([train | A], A).

%% wiggle --> [toot].
wiggle([toot | A], A).

%% wiggle --> wiggle, wiggle.
wiggle(A, C) :-
    wiggle(A, B),
    wiggle(B, C).

%% What are the first three responses that Prolog gives to the query s(X,[])?
%% ?- s(X,[]).
%% X = [choo, i, am, a, train, toot] ;
%% X = [choo, i, am, a, train, toot, toot] ;
%% X = [choo, i, am, a, train, toot, toot, toot] ;
%% ...

%% Exercise 7.2

%% The formal language a^n b^n - {\epsilon} consists of all the strings in a^n
%% b^n except the empty string. Write a DCG that generates this language.

%% nonterminals
s --> l,r.
s --> l,s,r.

%% terminals
l --> [a].
r --> [b].

%% Exercise 7.3

%% Let a^n b^(2n) be the formal language which contains all strings of the
%% following form: an unbroken block of as of length n followed by an unbroken
%% block of bs of length 2n, and nothing else. For example, abb, aabbbb, and
%% aaabbbbbb belong to a^n b^(2n), and so does the empty string. Write a DCG
%% that generates this language.

%% nonterminals
s --> [].
s --> l,r,r.
s --> l,s,r,r.

%% terminals
l --> [a].
r --> [b].

```


<br/>
<hr/>
<br/>
<h2 id="23"> 23. chapter-7/practical-session.pl</h2>

``` prolog
%% Chapter 7 - Practical Session

%% First some keyboard exercises:

%% First, type in or download the simple append based recognizer discussed in
%% the text, and then run some traces. As you will see, we were not exaggerating
%% when we said that the performance of the append based grammar was very
%% poor. Even for such simple sentences as The woman shot a man you will see
%% that the trace is very long, and very difficult to follow.

s(Z) :- append(X,Y,Z), np(X), vp(Y).

np(Z) :- append(X,Y,Z), det(X), n(Y).

vp(Z) :-  append(X,Y,Z), v(X), np(Y).
vp(Z) :-  v(Z).

det([the]).
det([a]).

n([woman]).
n([man]).

v([shoots]).

%% [trace]  ?- s([the,woman,shoots,a,man]).
%%   Call: (6) s([the, woman, shoots, a, man]) ?
%%   Call: (7) lists:append(_G652, _G653, [the, woman, shoots, a, man]) ?
%%   Exit: (7) lists:append([], [the, woman, shoots, a, man], [the, woman, shoots, a, man]) ?
%%    Call: (7) np([]) ?
%%    Call: (8) lists:append(_G652, _G653, []) ?
%%    Exit: (8) lists:append([], [], []) ?
%%    Call: (8) det([]) ?
%%    Fail: (8) det([]) ?
%%    Fail: (8) lists:append(_G652, _G653, []) ?
%%    Fail: (7) np([]) ?
%%    Exit: (7) lists:append([the], [woman, shoots, a, man], [the, woman, shoots, a, man]) ?
%%    Call: (7) np([the]) ?
%%    Call: (8) lists:append(_G655, _G656, [the]) ?
%%    Exit: (8) lists:append([], [the], [the]) ?
%%    Call: (8) det([]) ?
%%    Fail: (8) det([]) ?
%%    Exit: (8) lists:append([the], [], [the]) ?
%%    Call: (8) det([the]) ?
%%    Exit: (8) det([the]) ?
%%    Call: (8) n([]) ?
%%    Fail: (8) n([]) ?
%%    Redo: (8) det([the]) ?
%%    Fail: (8) det([the]) ?
%%    Redo: (8) lists:append([the|_G649], _G659, [the]) ?
%%    Fail: (8) lists:append(_G655, _G656, [the]) ?
%%    Fail: (7) np([the]) ?
%%    Redo: (7) lists:append([the|_G646], _G656, [the, woman, shoots, a, man]) ?
%%    Exit: (7) lists:append([the, woman], [shoots, a, man], [the, woman, shoots, a, man]) ?
%%    Call: (7) np([the, woman]) ?
%%    Call: (8) lists:append(_G658, _G659, [the, woman]) ?
%%    Exit: (8) lists:append([], [the, woman], [the, woman]) ?
%%    Call: (8) det([]) ?
%%    Fail: (8) det([]) ?
%%    Exit: (8) lists:append([the], [woman], [the, woman]) ?
%%    Call: (8) det([the]) ?
%%    Exit: (8) det([the]) ?
%%    Call: (8) n([woman]) ?
%%    Exit: (8) n([woman]) ?
%%    Exit: (7) np([the, woman]) ?
%%    Call: (7) vp([shoots, a, man]) ?
%%    Call: (8) lists:append(_G661, _G662, [shoots, a, man]) ?
%%    Exit: (8) lists:append([], [shoots, a, man], [shoots, a, man]) ?
%%    Call: (8) v([]) ?
%%    Fail: (8) v([]) ?
%%    Exit: (8) lists:append([shoots], [a, man], [shoots, a, man]) ?
%%    Call: (8) v([shoots]) ?
%%    Exit: (8) v([shoots]) ?
%%    Call: (8) np([a, man]) ?
%%    Call: (9) lists:append(_G664, _G665, [a, man]) ?
%%    Exit: (9) lists:append([], [a, man], [a, man]) ?
%%    Call: (9) det([]) ?
%%    Fail: (9) det([]) ?
%%    Exit: (9) lists:append([a], [man], [a, man]) ?
%%    Call: (9) det([a]) ?
%%    Exit: (9) det([a]) ?
%%    Call: (9) n([man]) ?
%%    Exit: (9) n([man]) ?
%%    Exit: (8) np([a, man]) ?
%%    Exit: (7) vp([shoots, a, man]) ?
%%    Exit: (6) s([the, woman, shoots, a, man]) ?
%% true

%% Next, type in or download our second recognizer, the one based on difference
%% lists, and run more traces. As you will see, there is a dramatic gain in
%% efficiency. Moreover, even if you find the idea of difference lists a bit
%% hard to follow, you will see that the traces are very simple to understand,
%% especially when compared with the monsters produced by the append based
%% implementation!

s(X,Z) :- np(X,Y), vp(Y,Z).

np(X,Z) :- det(X,Y), n(Y,Z).

vp(X,Z) :-  v(X,Y), np(Y,Z).

vp(X,Z) :-  v(X,Z).

det([the|W],W).
det([a|W],W).

n([woman|W],W).
n([man|W],W).

v([shoots|W],W).

%% [trace]  ?- s([the,woman,shoots,a,man],[]).
%%    Call: (7) s([the, woman, shoots, a, man], []) ?
%%    Call: (8) np([the, woman, shoots, a, man], _G441) ?
%%    Call: (9) det([the, woman, shoots, a, man], _G441) ?
%%    Exit: (9) det([the, woman, shoots, a, man], [woman, shoots, a, man]) ?
%%    Call: (9) n([woman, shoots, a, man], _G441) ?
%%    Exit: (9) n([woman, shoots, a, man], [shoots, a, man]) ?
%%    Exit: (8) np([the, woman, shoots, a, man], [shoots, a, man]) ?
%%    Call: (8) vp([shoots, a, man], []) ?
%%    Call: (9) v([shoots, a, man], _G441) ?
%%    Exit: (9) v([shoots, a, man], [a, man]) ?
%%    Call: (9) np([a, man], []) ?
%%    Call: (10) det([a, man], _G441) ?
%%    Exit: (10) det([a, man], [man]) ?
%%    Call: (10) n([man], []) ?
%%    Exit: (10) n([man], []) ?
%%    Exit: (9) np([a, man], []) ?
%%    Exit: (8) vp([shoots, a, man], []) ?
%%    Exit: (7) s([the, woman, shoots, a, man], []) ?
%% true

%% Next, type in or download the DCG discussed in the text. Type listing so that
%% you can see what Prolog translates the rules to. How does your system
%% translate rules of the form Det --> [the]? That is, does it translate them to
%% rules like det([the|X],X), or does is make use of rules containing the
%% 'C'predicate?

s --> np,vp.
np --> det,n.
vp --> v,np.
vp --> v.
det --> [the].
det --> [a].
n --> [woman].
n --> [man].
v --> [shoots].

%% ?- listing.

%% s(A, C) :-
%%  np(A, B),
%%  vp(B, C).

%% np(A, C) :-
%%  det(A, B),
%%  n(B, C).

%% vp(A, C) :-
%%  v(A, B),
%%  np(B, C).

%% vp(A, B) :-
%%  v(A, B).

%% det([the|A], A).
%% det([a|A], A).

%% n([woman|A], A).
%% n([man|A], A).

%% v([shoots|A], A).
%% true.

%% translates it to det([the|A], X).

%% Now run some traces. Apart from variable names, the traces you observe here
%% should be very similar to the traces you observed when running the difference
%% list recognizer. In fact, you will only observe any real differences if your
%% version of Prolog uses a 'C' based translation.

%% [trace]  ?- s([the,woman,shoots,a,man],[]).
%%    Call: (7) s([the, woman, shoots, a, man], []) ?
%%    Call: (8) np([the, woman, shoots, a, man], _G441) ?
%%    Call: (9) det([the, woman, shoots, a, man], _G441) ?
%%    Exit: (9) det([the, woman, shoots, a, man], [woman, shoots, a, man]) ?
%%    Call: (9) n([woman, shoots, a, man], _G441) ?
%%    Exit: (9) n([woman, shoots, a, man], [shoots, a, man]) ?
%%    Exit: (8) np([the, woman, shoots, a, man], [shoots, a, man]) ?
%%    Call: (8) vp([shoots, a, man], []) ?
%%    Call: (9) v([shoots, a, man], _G441) ?
%%    Exit: (9) v([shoots, a, man], [a, man]) ?
%%    Call: (9) np([a, man], []) ?
%%    Call: (10) det([a, man], _G441) ?
%%    Exit: (10) det([a, man], [man]) ?
%%    Call: (10) n([man], []) ?
%%    Exit: (10) n([man], []) ?
%%    Exit: (9) np([a, man], []) ?
%%    Exit: (8) vp([shoots, a, man], []) ?
%%    Exit: (7) s([the, woman, shoots, a, man], []) ?
%% true

%% And now it's time to write some DCGs:

%% 1. The formal language aEven is very simple: it consists of all strings
%% containing an even number of as, and nothing else. Note that the empty string
%% belongs to aEven. Write a DCG that generates aEven.

s --> [].
s --> [a],s,[a].

%% 2. The formal language a^n b^(2m) c^(2m) d^n consists of all strings of the
%% following form: an unbroken block of as followed by an unbroken block of bs
%% followed by an unbroken block of cs followed by an unbroken block of ds, such
%% that the a and d blocks are exactly the same length, and the c and d blocks
%% are also exactly the same length and furthermore consist of an even number of
%% cs and ds respectively. For example, \epsilon, abbccd, and aaabbbbccccddd all
%% belong to a^n b^(2m) c^(2m) d^n. Write a DCG that generates this language.

s --> t.
s --> [a],s,[d].

t --> [].
t --> [b,b],t,[c,c].

%% 3. The language that logicians call `propositional logic over the
%% propositional symbols p, q, and r' can be defined by the following context
%% free grammar:

%% prop -> p
%% prop -> q
%% prop -> r
%% prop -> not prop
%% prop -> (prop and prop)
%% prop -> (prop or prop)
%% prop -> (prop implies prop)

%% Write a DCG that generates this language. Actually, because we don't know
%% about Prolog operators yet, you will have to make a few rather clumsy looking
%% compromises. For example, instead of getting it to recognize

%% not(p implies q)

%% you will have to get it recognize things like

%% [not, '(', p, implies, q, ')']

%% instead. But we will learn later how to make the output nicer, so write the
%% DCG that accepts a clumsy looking version of this language.

prop --> [p].
prop --> [q].
prop --> [r].

prop --> [not], prop.
prop --> ['('],prop, conj, prop,[')'].

conj --> [and].
conj --> [or].
conj --> [implies].
```


<br/>
<hr/>
<br/>
<h2 id="24"> 24. chapter-8/exercises.pl</h2>

``` prolog
%% Chapter 8 - Exercises

%% Exercise 8.1

%% Here's our basic DCG.

s --> np,vp.

np --> det,n.

vp --> v,np.
vp --> v.

det --> [the].
det --> [a].

n --> [woman].
n --> [man].

v --> [shoots].


%% Suppose we add the noun ``men'' (which is plural) and the verb
%% ``shoot''. Then we would want a DCG which says that ``The men shoot'' is ok,
%% `The man shoots'' is ok, ``The men shoots'' is not ok, and ``The man shoot''
%% is not ok. Change the DCG so that it correctly handles these sentences. Use
%% an extra argument to cope with the singular/plural distinction.

%% should probably have used a variable instead of having cases for both plural
%% and singular, stupid me.
s --> np(plural), vp(plural).
s --> np(singular),vp(singular).

np(plural) --> det(plural),n(plural).
np(singular) --> det(singular),n(singular).

vp(plural) --> v(plural),np(_).
vp(singular) --> v(singular),np(_).
vp(plural) --> v(plural).
vp(singular) --> v(singular).

det(plural) --> [the].
det(singular) --> [the].
det(singular) --> [a].

n(plural) --> [men].
n(singular) --> [woman].
n(singular) --> [man].

v(plural) --> [shoot].
v(singular) --> [shoots].

%% ?- s([the,men,shoots],[]).
%% false.

%% ?- s([the,man,shoot],[]).
%% false.

%% ?- s([the,men,shoot],[]).
%% true

%% ?- s([the,man,shoots],[]).
%% true

%% also catches the cases "a men shoot a men" and the likes.

%% Exercise 8.2

%% Translate the following DCG rule into the form Prolog uses:

%% kanga(V,R,Q) --> roo(V,R),jumps(Q,Q),{marsupial(V,R,Q)}.

kanga(V, R, Q, A, C) :-
    roo(V, R, A, B),
    jumps(Q, Q, B, C),
    marsupial(V, R, Q).

```


<br/>
<hr/>
<br/>
<h2 id="25"> 25. chapter-8/practical-session.pl</h2>

``` prolog
%% Chapter 8 - Practical Session

%% First some keyboard exercises:

%% 1. Trace some examples using the DCG which uses extra arguments to handle the
%% subject/object distinct, the DCG which produces parses, and the DCG which
%% uses extra tests to separate lexicon and rules. Make sure you fully
%% understand the way all three DCGs work.

%% [trace]  ?- s([the,woman,shoots,him],[]).
%%    Call: (7) s([the, woman, shoots, him], []) ?
%%    Call: (8) np(subject, [the, woman, shoots, him], _G403) ?
%%    Call: (9) det([the, woman, shoots, him], _G402) ?
%%    Exit: (9) det([the, woman, shoots, him], [woman, shoots, him]) ?
%%    Call: (9) n([woman, shoots, him], _G402) ?
%%    Exit: (9) n([woman, shoots, him], [shoots, him]) ?
%%    Exit: (8) np(subject, [the, woman, shoots, him], [shoots, him]) ?
%%    Call: (8) vp([shoots, him], []) ?
%%    Call: (9) v([shoots, him], _G402) ?
%%    Exit: (9) v([shoots, him], [him]) ?
%%    Call: (9) np(object, [him], []) ?
%%    Call: (10) det([him], _G402) ?
%%    Fail: (10) det([him], _G402) ?
%%    Redo: (9) np(object, [him], []) ?
%%    Call: (10) pro(object, [him], []) ?
%%    Exit: (10) pro(object, [him], []) ?
%%    Exit: (9) np(object, [him], []) ?
%%    Exit: (8) vp([shoots, him], []) ?
%%    Exit: (7) s([the, woman, shoots, him], []) ?
%% true

%% ?- s(T,[a,woman,shoots],[]).

%% [trace]  ?- s(T,[a,woman,shoots],[]).
%%    Call: (7) s(_G346, [a, woman, shoots], []) ?
%%    Call: (8) np(_G405, [a, woman, shoots], _G414) ?
%%    Call: (9) det(_G408, [a, woman, shoots], _G417) ?
%%    Exit: (9) det(det(a), [a, woman, shoots], [woman, shoots]) ?
%%    Call: (9) n(_G409, [woman, shoots], _G419) ?
%%    Exit: (9) n(n(woman), [woman, shoots], [shoots]) ?
%%    Exit: (8) np(np(det(a), n(woman)), [a, woman, shoots], [shoots]) ?
%%    Call: (8) vp(_G406, [shoots], []) ?
%%    Call: (9) v(_G415, [shoots], _G424) ?
%%    Exit: (9) v(v(shoots), [shoots], []) ?
%%    Call: (9) np(_G416, [], []) ?
%%    Call: (10) det(_G420, [], _G429) ?
%%    Fail: (10) det(_G420, [], _G429) ?
%%    Fail: (9) np(_G416, [], []) ?
%%    Redo: (8) vp(_G406, [shoots], []) ?
%%    Call: (9) v(_G415, [shoots], []) ?
%%    Exit: (9) v(v(shoots), [shoots], []) ?
%%    Exit: (8) vp(vp(v(shoots)), [shoots], []) ?
%%    Exit: (7) s(s(np(det(a), n(woman)), vp(v(shoots))), [a, woman, shoots], []) ?
%% T = s(np(det(a), n(woman)), vp(v(shoots)))

%% [trace]  ?- det([the],[]).
%%    Call: (7) det([the], []) ?
%%    Call: (8) lex(the, det) ?
%%    Exit: (8) lex(the, det) ?
%%    Call: (8) []=[] ?
%%    Exit: (8) []=[] ?
%%    Exit: (7) det([the], []) ?
%% true.

%% [trace]  ?- v([the],[]).
%%    Call: (7) v([the], []) ?
%%    Call: (8) lex(the, v) ?
%%    Fail: (8) lex(the, v) ?
%%    Fail: (7) v([the], []) ?
%% false.

%% 2. Carry out traces on the DCG for a^n b^n c^n that was given in the text
%% (that is, the DCG that gave the Count variable the values 0, succ(0),
%% succ(succ(0)), and so on). Try cases where the three blocks of as, bs, and cs
%% are indeed of the same length as well as queries where this is not the case.

%% [trace]  ?- s(Count,L,[]).
%%    Call: (7) s(_G674, _G675, []) ?
%%    Call: (8) ablock(_G674, _G675, _G745) ?
%%    Exit: (8) ablock(0, _G675, _G675) ?
%%    Call: (8) bblock(0, _G675, _G745) ?
%%    Exit: (8) bblock(0, _G675, _G675) ?
%%    Call: (8) cblock(0, _G675, []) ?
%%    Exit: (8) cblock(0, [], []) ?
%%    Exit: (7) s(0, [], []) ?
%% Count = 0,
%% L = [] ;
%%    Redo: (8) ablock(_G674, _G675, _G745) ?
%%    Call: (9) ablock(_G739, _G742, _G750) ?
%%    Exit: (9) ablock(0, _G742, _G742) ?
%%    Exit: (8) ablock(succ(0), [a|_G742], _G742) ?
%%    Call: (8) bblock(succ(0), _G742, _G750) ?
%%    Call: (9) bblock(0, _G745, _G753) ?
%%    Exit: (9) bblock(0, _G745, _G745) ?
%%    Exit: (8) bblock(succ(0), [b|_G745], _G745) ?
%%    Call: (8) cblock(succ(0), _G745, []) ?
%%    Call: (9) cblock(0, _G748, []) ?
%%    Exit: (9) cblock(0, [], []) ?
%%    Exit: (8) cblock(succ(0), [c], []) ?
%%    Exit: (7) s(succ(0), [a, b, c], []) ?
%% Count = succ(0),
%% L = [a, b, c] ;

%% Now for some programming. We suggest two exercises.

%% First, bring together all the things we have learned about DCGs for English
%% into one DCG. In particular, today we say how to use extra arguments to deal
%% with the subject/object distinction, and in the exercises you were asked to
%% use additional arguments to deal with the singular/plural distinction. Write
%% a DCG which handles both. Moreover, write the DCG in such a way that it will
%% produce parse trees, and makes use of a separate lexicon.

s(Number, s(Number, NP, VP)) --> np(Number, NP), vp(Number, VP).

np(Number, np(Number, DET, N)) --> det(Number, DET), n(Number, N).

vp(Number, vp(Number, V, NP)) --> v(Number, V), np(_, NP).
vp(Number, vp(Number, V)) --> v(Number, V).

det(Number, det(Number, Word)) --> [Word], {lex(Word, Number, det)}.

n(Number, n(Number, Word)) --> [Word], {lex(Word, Number, n)}.

v(Number, v(Number, Word)) --> [Word], {lex(Word, Number, v)}.

%% Lexicon
lex(the, _, det).
lex(a, singular, det).

lex(woman, singular, n).
lex(man, singular, n).
lex(men, plural, n).

lex(shoots, singular, v).
lex(shoot, plural, v).

%% Test output

%% s(P,T,[the,men,shoot,a,woman],[]).
%% P = plural,
%%  T =
%%  s(plural,
%%    np(plural,
%%       det(plural, the),
%%       n(plural, men)),
%%    vp(plural,
%%       v(plural, shoot),
%%       np(singular,
%%   det(singular, a),
%%   n(singular, woman))))

%% s(P,T,L,[]).
%% T =
%% s(singular,
%%   np(singular,
%%      det(singular, the),
%%      n(singular, woman)),
%%   vp(singular,
%%      v(singular, shoots),
%%      np(singular,
%%  det(singular, the),
%%  n(singular, woman))))

%% That is pretty cool.

%% Once you have done this, extend the DCG so that noun phrases can be modified
%% by adjectives and simple prepositional phrases (that is, it should be able to
%% handle noun phrases such as ``the small frightened woman on the table'' or
%% ``the big fat cow under the shower''). Then, further extend it so that the
%% distinction between first, second, and third person pronouns is correctly
%% handled (both in subject and object form).

%% small = adjective, on the table = prepositional phrase.

s(Number, s(Number, NP, VP)) --> np(Number, NP), vp(Number, VP).
s(Number, s(Number, NP, PP)) --> np(Number, NP), pp(PP).

np(Number, np(Number, DET, AP, N)) --> det(Number, DET), ap(AP), n(Number, N).
np(Number, np(Number, DET, N)) --> det(Number, DET), n(Number, N).

vp(Number, vp(Number, V, PP, NP)) --> v(Number, V), pp(PP).
vp(Number, vp(Number, V)) --> v(Number, V).

det(Number, det(Number, Word)) --> [Word], {lex(Word, Number, det)}.

ap(ap(ADJ1,ADJ2)) --> adj(size, ADJ1), adj(propadj, ADJ2).
ap(ap(ADJ)) --> adj(size, ADJ).
ap(ap(ADJ)) --> adj(propadj, ADJ).

pp(pp(PRE, NP)) --> pre(PRE), np(_, NP).

pre(pre(Word)) --> [Word], {lex(Word, prepos)}.

n(Number, n(Number, Word)) --> [Word], {lex(Word, Number, n)}.

v(Number, v(Number, Word)) --> [Word], {lex(Word, Number, v)}.

adj(Type, adj(Type, Word)) --> [Word], {lex(Word, Type, adj)}.

%% Lexicon
lex(the, _, det).
lex(a, singular, det).

lex(woman, singular, n).
lex(man, singular, n).
lex(men, plural, n).
lex(cow, singular, n).
lex(table, singular, n).
lex(shower, singular, n).

lex(shoots, singular, v).
lex(shoot, plural, v).

lex(small, size, adj).
lex(big, size, adj).
lex(frightened, propadj, adj).
lex(fat, propadj, adj).

lex(under, prepos).
lex(on, prepos).

%% ?- s(P,T,[the,small,frightened,woman,on,the,table],[]).
%% P = singular,
%% T = s(singular,
%%       np(singular,
%%      det(singular,
%%          the),
%%      ap(adj(size,
%%       small),
%%         adj(propadj,
%%       frightened)),
%%      n(singular,
%%        woman)),
%%       pp(pre(on),
%%      np(singular,
%%         det(singular,
%%       the),
%%         n(singular, table))))

%% Note: couldn't be arsed to complete the whole implementation, i.e. the
%% first/second/third person aspects, but it should be (almost) trivial to do.
```


<br/>
<hr/>
<br/>
<h2 id="26"> 26. chapter-9/exercises.pl</h2>

``` prolog
%% Chapter 9 - Exercises

%% Exercise 9.1

%% Which of the following queries succeed, and which fail?

%% ?- 12 is 2*6 -> true
%% ?- 14 =\= 2*6 -> true
%% ?- 14 = 2*7 -> false
%% ?- 14 == 2*7 -> false
%% ?- 14 \== 2*7 -> true
%% ?- 14 =:= 2*7 -> true
%% ?- [1,2,3|[d,e]] == [1,2,3,d,e] -> true
%% ?- 2+3 == 3+2 -> false
%% ?- 2+3 =:= 3+2 -> true
%% ?- 7-2 =\= 9-2 -> true
%% ?- p == 'p' -> true
%% ?- p =\= 'p' -> error
%% ?- vincent == VAR -> false.
%% ?- vincent=VAR,VAR==vincent -> VAR = vincent.

%% Exercise 9.2

%% How does Prolog respond to the following queries?

%% ?- .(a,.(b,.(c,[]))) = [a,b,c] -> true
%% ?- .(a,.(b,.(c,[]))) = [a,b|[c]] -> true
%% ?- .(.(a,[]),.(.(b,[]),.(.(c,[]),[]))) = X -> X = [[a], [b], [c]].
%% ?- .(a,.(b,.(.(c,[]),[]))) = [a,b|[c]] -> false

%% Exercise 9.3

%% Write a two-place predicate termtype(+Term,?Type) that takes a term and gives
%% back the type(s) of that term (atom, number, constant, variable etc.). The
%% types should be given back in the order of their generality. The predicate
%% should, e.g., behave in the following way.

%% ?- termtype(Vincent,variable).
%% yes

%% ?- termtype(mia,X).
%% X = atom ;
%% X = constant ;
%% X = simple_term ;
%% X = term ;
%% no

%% ?- termtype(dead(zed),X).
%% X = complex_term ;
%% X = term ;
%% no

%% simple terms
termtype(Term,atom) :-
  atom(Term).
termtype(Term,integer) :-
  integer(Term).
termtype(Term,number) :-
  number(Term).
termtype(Term,constant) :-
  atomic(Term).
termtype(Term,variable) :-
  var(Term).

%% complex term
termtype(Term,complex_term) :-
  nonvar(Term),
  functor(Term,_,A),
  A > 0.

%% simple term
termtype(Term,simple_term) :-
  termtype(Term,variable).
termtype(Term,simple_term) :-
  termtype(Term,constant).

%% term
termtype(Term,term) :-
  termtype(Term,simple_term).
termtype(Term,term) :-
  termtype(Term,complex_term).

%% Exercise 9.4

%% Write a program that defines the predicate groundterm(+Term) which tests
%% whether Term is a ground term. Ground terms are terms that don't contain
%% variables. Here are examples of how the predicate should behave:

%% ?- groundterm(X).
%% no

%% ?- groundterm(french(bic_mac,le_bic_mac)).
%% yes

%% ?- groundterm(french(whopper,X)).
%% no

%% convert complex term to list of its arguments and run through list and check
%% if each element is also a groundterm.

groundterm(Term) :-
  nonvar(Term),
  Term =.. [_ | X],
  groundterm_in_list(X).

groundterm_in_list([H|T]) :-
  groundterm(H),
  groundterm_in_list(T).

groundterm_in_list([]).

%% Exercise 9.5

%% Assume that we have the following operator definitions.

:- op(300, xfx, [are, is_a]).
:- op(300, fx, likes).
:- op(200, xfy, and).
:- op(100, fy, famous).

%% Which of the following is a wellformed term? What is the main operator? Give the bracketing.

%% ?- X is_a witch. -> true.
%% ?- harry and ron and hermione are friends. -> true
%% ?- harry is_a wizard and likes quidditch. -> false
%% ?- dumbledore is_a famous famous wizard. -> true

%% is_a(X, witch).
%% are(and(harry, and(ron, hermione)), friends).
%% is_a(dumbledore, famous(famous(wizard))).

```


<br/>
<hr/>
<br/>

<h2 id="27"> 27. chapter-9/practical-session.pl</h2>

``` prolog
%% Chapter 9 - Practical Session

%% In this practical session, we want to introduce some built-in predicates for
%% printing terms onto the screen. The first predicate we want to look at is
%% display/1, which takes a term and prints it onto the screen.

%% ?- display(loves(vincent,mia)).
%% loves(vincent, mia)
%% Yes

%% ?- display('jules eats a big kahuna burger').
%% jules eats a big kahuna burger
%% Yes

%% More strictly speaking, display prints Prolog's internal representation of terms.

%% ?- display(2+3+4).
%% +(+(2, 3), 4)
%% Yes

%% In fact, this property of display makes it a very useful tool for learning
%% how operators work in Prolog. So, before going on to learn more about how to
%% write things onto the screen, try the following queries. Make sure you
%% understand why Prolog answers the way it does.

%% ?- display([a,b,c]). -> .(a,.(b,.(c,[])))
%% ?- display(3 is 4 + 5 / 3). -> is(3,+(4,/(5,3)))
%% ?- display(3 is (4 + 5) / 3). -> is(3,/(+(4,5),3))
%% ?- display((a:-b,c,d)). -> :-(a,,(b,,(c,d)))
%% ?- display(a:-b,c,d). -> undefined procedure display/3

%% So, display is nice to look at the internal representation of terms in
%% operator notation, but usually we would probably prefer to print the user
%% friendly notation instead. Especially when printing lists, it would be much
%% nicer to get [a,b,c], instead of .(a.(b.(c,[]))). This is what the built-in
%% predicate write/1 does. It takes a term and prints it to the screen in the
%% user friendly notation.

%% ?- write(2+3+4).
%% 2+3+4
%% Yes

%% ?- write(+(2,3)).
%% 2+3
%% Yes

%% ?- write([a,b,c]).
%% [a, b, c]
%% Yes

%% ?- write(.(a,.(b,[]))).
%% [a, b]
%% Yes

%% And here is what happens, when the term that is to be written contains
%% variables.

%% ?- write(X).
%% _G204
%% X = _G204
%% yes

%% ?- X = a, write(X).
%% a
%% X = a
%% Yes

%% The following example shows what happens when you put two write commands one
%% after the other.

%% ?- write(a),write(b).
%% ab
%% Yes

%% Prolog just executes one after the other without putting any space in between
%% the output of the different write commands. Of course, you can tell Prolog to
%% print spaces by telling it to write the term ' '.

%% ?- write(a),write(' '),write(b).
%% a b
%% Yes

%% And if you want more than one space, for example five blanks, you can tell
%% Prolog to write '     '.

%% ?- write(a),write('     '),write(b).
%% a     b
%% Yes

%% Another way of printing spaces is by using the predicate tab/1. tab takes a
%% number as argument and then prints as many spaces as specified by that
%% number.

%% ?- write(a),tab(5),write(b).
%% a     b
%% Yes

%% Another predicate useful for formatting is nl. nl tells Prolog to make a
%% linebreak and to go on printing on the next line.

%% ?- write(a),nl,write(b).
%% a
%% b
%% Yes

%% In the last lecture, we saw how extra arguments in DCGs can be used to build
%% a parse tree. For example, to the query s(T,[a,man,shoots,a,woman],[]) Prolog
%% would answer s(np(det(a),n(man)),vp(v(shoots),np(det(a),n(woman)))). This is
%% a representation of the parse tree. It is not a very readable representation,
%% though. Wouldn't it be nicer if Prolog printed something like

%% s(
%%   np(
%%      det(a)
%%      n(man))
%%   vp(
%%     v(shoots)
%%     np(
%%       det(a)
%%       n(woman))))

%% for example?

%% Write a predicate pptree/1 that takes a complex term representing a tree,
%% such as s(np(det(a),n(man)),vp(v(shoots),np(det(a),n(woman)))), as its
%% argument and prints a nice and readable output for this tree.

%% Helper function

%% simple terms
termtype(Term, atom) :-
  atom(Term).
termtype(Term, integer) :-
  integer(Term).
termtype(Term, number) :-
  number(Term).
termtype(Term, constant) :-
  atomic(Term).
termtype(Term, variable) :-
  var(Term).

%% complex term
termtype(Term, complex_term) :-
  nonvar(Term),
  functor(Term, _, A),
  A > 0.

%% simple term
termtype(Term, simple_term) :-
  termtype(Term, variable).
termtype(Term, simple_term) :-
  termtype(Term, constant).

%% term
termtype(Term, term) :-
  termtype(Term, simple_term).
termtype(Term, term) :-
  termtype(Term, complex_term).

%% Actual Function

%% simple term; indent and print the term with no linebreak.
%% Example: mia
printterm(Term, Indent) :-
  termtype(Term, simple_term),
  tab(Indent), write(Term).

%% complex term containing only simple terms; indent and print the
%% term and its arguments.
%% Example: likes(mia, vincent).
printterm(Term, Indent) :-
  termtype(Term, complex_term),
  Term =.. [TermName | TermArguments],
  checkSimpleTypes(TermArguments),
  tab(Indent), write(Term).

checkSimpleTypes([]).

checkSimpleTypes([Head | Tail]) :-
  termtype(Head, simple_term),
  checkSimpleTypes(Tail).

%% complex term containing other complex terms; indent and print the term, then
%% call printterm on all its arguments.
printterm(Term, Indent) :-
  termtype(Term, complex_term),
  Term =.. [TermName | TermArguments],
  not(checkSimpleTypes(TermArguments)),
  tab(Indent), write(TermName), write('('), nl,
  NewIndent is Indent + 2,
  iterateArguments(TermArguments, NewIndent),
  write(')').

iterateArguments([Head], Indent) :-
  printterm(Head, Indent).

iterateArguments([Head | Tail], Indent) :-
  Tail \= [],
  printterm(Head, Indent), nl,
  iterateArguments(Tail, Indent).

%% %% Main
pptree(Term) :-
  printterm(Term, 0).

%% ?- pptree(s(np(det(a),n(man)),vp(v(shoots),np(det(a),n(woman))))).
%% s(
%%   np(
%%     det(a)
%%     n(man))
%%   vp(
%%     v(shoots)
%%     np(
%%       det(a)
%%       n(woman))))
%% true

%% In the practical session of Chapter 7, you were asked to write a DCG
%% generating propositional logic formulas. The input you had to use was a bit
%% awkward though. The formula not(p-->q) had to be represented as [not, '(', p,
%% implies, q, ')']. Now, that you know about operators, you can do something a
%% lot nicer. Write the operator definitions for the operators not, and, or,
%% implies, so that Prolog accepts (and correctly brackets) propositional logic
%% formulas. For example:

%% ?- display(not(p implies q)).
%% not(implies(p,q)).
%% Yes

%% ?- display(not p implies q).
%% implies(not(p),q)
%% Yes

:- op(800, fy, not).
:- op(1000, xfy, implies).
:- op(1000, xfy, and).
:- op(1000, xfy, or).

%% ?- display(p implies q and not r).
%% implies(p,and(q,not(r)))
%% true.


```


