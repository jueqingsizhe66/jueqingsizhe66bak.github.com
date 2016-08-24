---
layout: post
title: "what consist of the bicycle?"
date: 2016-08-24 03:15:59 +0800
comments: true
categories: prolog
---

1. describe what consists of bicycle(not completely)
2. describe how to do with modules in prolog
3. describe how to replace quant() with colon(similar to json style)

if you want to know the codes below, you should read [Chapter3 and chapter7 in the book "Programming in Prolog, 5th ed (2003) by William F.Clocksin"][1].

Here is the old bicycle

![old bicycle][2].

<!--more-->

1. the original version without worry about the quantity of the basic parts

As you know every bicycle's structure can be simplified as below,

![simple][3]

then we can see the componets chart of the bicycle below which shows the hierarchy of the bicycle,

![struture][4]


Here we show the basic code below,

the test result is :


******
```
13 ?- partsof(axle,Y).
Y = [bolt, nut] .

14 ?- partsof(hub,Y).
Y = [gear, bolt, nut] .

15 ?- partsof(wheel,Y).
Y = [spoke, rim, gear, bolt, nut] .

16 ?- partsof(bike,Y).
Y = [spoke, rim, gear, bolt, nut, spoke, rim, gear, bolt|..

17 ?- partsof(bolt,Y).
Y = [bolt] .
```

We can see the components of the axle,hub,wheel,bike.And we knew that bolt is the basic part.

******

The predicates table

```
predicates            arith               arguments

basicpart             1                   ?part
assemble              2                   +assemble_part,list_of_part
partof                2                   +assemble_part,components_of_assemble
partsoflist           3                   +list, -result

partof1               2                   +assemble_part,components_of_assemble
partsacc              3                   +assemble, components_of_some_assemble,result
partsacclist          3                   +components_of_assemble,accumulator,result
```

``` prolog
%% basicpart are not made of smaller parts.
%% they simply combine with other basicparts
%% to form the assemble

basicpart(rim).
basicpart(spoke).
%basicpart(hub).
basicpart(handle).
basicpart(rear_frame).
basicpart(gear).
basicpart(bolt).
basicpart(nut).
basicpart(fork).

assemble(bike,[wheel,wheel,frame]).
%% maybe a quantitty of bolts and nuts.
assemble(wheel,[spoke,rim,hub]).
assemble(frame,[rear_frame,front_frame]).
assemble(front_frame,[fork,handle]).
%assemble(rear_frame,[]).
assemble(hub,[gear,axle]).
assemble(axle,[bolt,nut]).


partsof(X,[X]) :- 
    basicpart(X).

partsof(X,P) :-
    assemble(X,Subparts),
    partsoflist(Subparts,P).

partsoflist([],[]).
partsoflist([P|Tail],Total) :-
    partsof(P,Headparts),
    partsoflist(Tail,Tailparts),
    append(Headparts,Tailparts,Total).


%% first edition,we ignore how many of each
%% kind of basicpart
assemble(sentence,[noun_phrase,verb_phrase]).
assemble(noun_phrase,[determiner,noun]).
assemble(verb_phrase,[verb,[verb,noun]]).
assemble(determiner,[the,a,an]).
assemble(noun,[apple]).
assemble(noun,[fruit]).

basicpart(apple).
basicpart(fruit).

%% partsof1 : give a part, it will show you
%% the basic parts.
partsof1(X, P) :- partsacc(X,[],P).

partsacc(X,A,[X|A]) :- 
    basicpart(X).
partsacc(X,A,P) :-
    assemble(X,Subparts),
    partsacclist(Subparts,A,P).

partsacclist([],A,A).
partsacclist([P|Tail],A,Total):-
    partsacc(P,A,Hp),
    partsacclist(Tail,Hp,Total).


```

<font color="red">Noted: what we should figure out that, both partsof and partof1  can show the components of one assemble or parts.The difference
are partsof use append to adhere all the result ,while partof1 use the tail recursion(accumulator, the answer so far) to collect
all the result. </font>

<font color="green">A little pity is that below improve version and update version didn't pose the solution with tail recursion.
Later on, we will add [them][16].</font>

2. the improve version with worry about the quantity of the basic parts

The result shows below:

******


``` prolog

4 ?- listing(partlist).
bicycleExtend:partlist(A) :-
        partsof(1, A, B),
        collect(B, C),
        printpartlist(C).

true.

5 ?- partlist(axle1).
6       bolt
6       nut
true .

6 ?- partlist(hub1).
1       gear
6       bolt
6       nut
true 
Unknown action:  (h for help)
Action? .

7 ?- partlist(wheel1).
1       spoke
1       rim
1       gear
6       bolt
6       nut
true .

8 ?- partlist(bike1).
2       spoke
2       rim
2       gear
12      bolt
12      nut
1       for



9 ?- partlist(bolt).
1       bolt
true .
```

We can know from the result above that, partlist can show the quantity of the every assemble exactly.


The predicates table

```
predicates            arith               arguments

basicpart             1                   ?part
assemble              2                   +assemble_part,list_of_part
partlist              1                   +assemble_part
partof                2                   +assemble_part,components_of_assemble
partsoflist           3                   +list, -result

```

******
``` prolog
%%:- ensure_loaded([bicycle]).
:- module(bicycleExtend, [partlist/1]).


basicpart(rim).
basicpart(spoke).
%basicpart(hub).
basicpart(handle).
basicpart(rear_frame).
basicpart(gear).
basicpart(bolt).
basicpart(nut).
basicpart(fork).


assemble(bike1,[quant(wheel1,2),quant(frame1,1)]).
%% maybe a quantitty of bolts and nuts.
assemble(wheel1,[quant(spoke,1),quant(rim,1),quant(hub1,1)]).
assemble(frame1,[quant(rear_frame1,1),quant(front_frame1,1)]).
assemble(front_frame1,[quant(fork,1),quant(handle,1)]).
assemble(rear_frame1,[]).
assemble(hub1,[quant(gear,1),quant(axle1,1)]).
assemble(axle1,[quant(bolt,6),quant(nut,6)]).


partlist(T) :-
    partsof(1,T,P),
    collect(P,Q),
    printpartlist(Q).


partsof(N, X, P) :-
    assemble(X, S),
    partsoflist(N, S, P).
partsof(N, X, [quant(X,N)]) :-
    basicpart(X).

partsoflist(_, [], []).
partsoflist(N,[quant(X,Num)|L],T) :-
    M is N* Num,
    partsof(M, X, Xparts),
    partsoflist(N, L, Restparts),
    append(Xparts, Restparts,  T).


collect([], []).
collect([quant(X,N)|R], [quant(X,Ntotal)|R2]) :-
    collectrest(X, N, R, O, Ntotal),
    collect(O, R2).

collectrest(_, N, [], [], N).
collectrest(X, N, [quant(X, Num)|Rest], Others, Ntotal) :-
    !,
    M is N + Num,
    collectrest(X, M, Rest, Others, Ntotal).
collectrest(X, N, [Other|Rest], [Other|Others], Ntotal) :-
    collectrest(X, N, Rest, Others, Ntotal).

printpartlist([]).
printpartlist([quant(X, N)|R]) :-
    write(''),write(N),
    put_char('\t'),write(X),nl,
    printpartlist(R).

```


3. the update version with changing the record style with json

Now we just change `quant(X,Num)` to `X:Num`, and let it work.

The result is below,

******

``` prolog

11 ?- listing(partlist2).
bicycleEntend2:partlist2(A) :-
        partsof(1, A, B),
        collect(B, C),
        printpartlist(C).

true.

12 ?- partlist2(axle2).
6       bolt
6       nut
true .

13 ?- partlist2(hub2).
2       gear
12      bolt
12      nut
true 
Unknown action:  (h for help)
Action? .

14 ?- partlist2(wheel2).
4       spoke
2       rim
4       gear
24      bolt
24      nut
true .

15 ?- partlist2(bicycle2).
false.

16 ?- partlist2(bike2).
8       spoke
4       rim
8       gear
48      bolt
48      nut
2       fork
4       handle
true .
```
******


The predicates table

```
predicates            arith               arguments

basicpart             1                   ?part
assemble              2                   +assemble_part,list_of_part
partlist2              1                   +assemble_part
partof                2                   +assemble_part,components_of_assemble
partsoflist           3                   +list, -result

```


``` prolog
%%:- ensure_loaded([bicycle]).

:- module(bicycleEntend2, [partlist2/1]).


basicpart(rim).
basicpart(spoke).
%basicpart(hub).
basicpart(handle).
basicpart(rear_frame).
basicpart(gear).
basicpart(bolt).
basicpart(nut).
basicpart(fork).


assemble(bike2,[ wheel2:2, frame2:2]).
%% maybe a quantitty of bolts and nuts.
assemble(wheel2,[ spoke:4, rim:2, hub2:2]).
assemble(frame2,[ rear_frame2:1, front_frame2:1]).
assemble(front_frame2,[ fork:1, handle:2]).
assemble(rear_frame2,[]).
assemble(hub2,[ gear:2, axle2:2]).
assemble(axle2,[ bolt:6, nut:6]).



partlist2(T) :-
    partsof(1,T,P),
    collect(P,Q),
    printpartlist(Q).


partsof(N, X, P) :-
    assemble(X, S),
    partsoflist(N, S, P).
partsof(N, X, [X:N]) :-
    basicpart(X).

partsoflist(_, [], []).
partsoflist(N,[X:Num|L],T) :-
    M is N* Num,
    partsof(M, X, Xparts),
    partsoflist(N, L, Restparts),
    append(Xparts, Restparts,  T).


collect([], []).
collect([X:N|R], [X:Ntotal|R2]) :-
    collectrest(X, N, R, O, Ntotal),
    collect(O, R2).

collectrest(_, N, [], [], N).
collectrest(X, N, [X:Num|Rest], Others, Ntotal) :-
    !,
    M is N + Num,
    collectrest(X, M, Rest, Others, Ntotal).
collectrest(X, N, [Other|Rest], [Other|Others], Ntotal) :-
    collectrest(X, N, Rest, Others, Ntotal).

printpartlist([]).
printpartlist([X:N|R]) :-
    write(''),write(N),
    put_char('\t'),write(X),nl,
    printpartlist(R).

```


******


<div align="center"><font color="red">Noted: you can trace the codes with swi-prolog step by step, and watch what arguments are being instantiated, and what stacks structures
changes, which will help you understand the process of the bicycle modle </font></div>


Here we only show some debug procedure of the `partlist2(axle2).`,watch carefully how it runs.


<hr/>
![debug1][5]
<hr/>

![debug1][6]

<hr/>
![debug1][7]

<hr/>
![debug1][8]

<hr/>
![debug1][9]

<hr/>
![debug1][10]

<hr/>
![debug1][11]

<hr/>
![debug1][12]

<hr/>
![debug1][13]

<hr/>
![debug1][14]

<hr/>
![debug1][15]

<hr/>





[1]: http://ishare.iask.sina.com.cn/f/24264236.html 
[2]: /images/prolog/bicycle/bicycle1.jpg
[3]: /images/prolog/bicycle/bicycle2.png
[4]: /images/prolog/bicycle/bicyclestru.png
[5]: /images/prolog/bicycle/debug1.png
[6]: /images/prolog/bicycle/debug2.png
[7]: /images/prolog/bicycle/debug3.png
[8]: /images/prolog/bicycle/debug4.png
[9]: /images/prolog/bicycle/debug5.png
[10]: /images/prolog/bicycle/debug6.png
[11]: /images/prolog/bicycle/debug7.png
[12]: /images/prolog/bicycle/debug8.png
[13]: /images/prolog/bicycle/debug9.png
[14]: /images/prolog/bicycle/debug10.png
[15]: /images/prolog/bicycle/debug11.png
[16]: http://jueqingsizhe66.github.io/blog/2016/08/24/bicycleextend/
