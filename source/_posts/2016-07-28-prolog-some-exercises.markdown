---
layout: post
title: "prolog some exercises"
date: 2016-07-28 19:31:58 +0800
comments: true
categories: prolog
---

Below is some prolog codes files for practice.

![exercise process][2]
<!--more-->

<h2 id ="main">main procedure</h2>

``` prolog
%% add the current folds into data base for prolog query
:- [nego].
:- [nego2].
:- [nego3].
:- [nani].
%%:- [nature].
:- [fathers].
:- [descendC].
:- [likes].
file_search_path(helo, 'nature_language/').
:- [helo(nature)].
:- [helo(natureExtend)].

file_search_path(printM,'printModule/').
:- use_module(printM(printMovie)).
:- use_module(printM(printActor)).


file_search_path(readM,'readFile/').
:- [readM(testRead)].
:- [readM(testRead2)].

:- [readHouses].
maxme(A,B,Z):-
    (A>=B->Z=A;Z=B).

```

<hr/>
<h2 id="filestruct"> file contents </h2>

+ [1.nego](#1).
+ [2.nego2](#2).
+ [3.nego3](#3).
+ [4.nani](#4).
+ [5.fathers](#5).
+ [6.descendC](#6).
+ [7.likes](#7).
+ [8.helo(nature)](#8).
+ [9.helo(natureExtend)](#9).
+ [10.use_module(printM(printMovie))](#10).
+ [11.use_module(printM(printActor))](#11).
+ [12.readM(testRead)](#12).
+ [13.readM(testRead2)](#13).
+ [14.readHouses](#14).


<hr/>

<h2 = id="1">1. nego</h2>

``` prolog

enjoys(vincent,X) :- bigKahu(X),!,fail.
enjoys(vincent,X) :- burger(X).

burger(X) :- bigMac(X).
burger(X) :- bigKahu(X).
burger(X) :- hopper(X).

bigMac(a).
bigMac(c).
bigKahu(b).
hopper(d).
```

<hr/>
<h2 = id="2">2. nego2</h2>

``` prolog
neg(Goal) :- Goal,!,fail.
neg(Goal).

enjoys2(vincent,X) :- burger2(X),neg(bigKahu2(X)).

burger2(X) :- bigMac2(X).
burger2(X) :- bigKahu2(X).
burger2(X) :- hopper2(X).

bigMac2(a).
bigMac2(c).
bigKahu2(b).
hopper2(d).


```

<hr/>
<h2 = id="3">3. nego3</h2>

``` prolog
enjoys3(vincent,X) :- burger3(X),\+bigKahu3(X).

burger3(X) :- bigMac3(X).
burger3(X) :- bigKahu3(X).
burger3(X) :- hopper3(X).

bigMac3(a).
bigMac3(c).
bigKahu3(b).
hopper3(d).


```


<hr/>
<h2 = id="4">4. nani</h2>

[Nani is one game][1]

``` prolog
:-dynamic here/1.

room(kitchen).
room(office).
room(hall). 
room('dining room').
room(cellar). 
door(office, hall).
door(kitchen, office).
door(hall, 'dining room').
door(kitchen, cellar).
door('dining room', kitchen).

location(desk, office).
location(apple, kitchen). 
location(flashlight, desk). 
location('washing machine', cellar).
location(nani, 'washing machine').
location(broccoli, kitchen).
location(crackers, kitchen).
location(knife, kitchen).
location(computer, office).

location(envelope, desk). 
location(stamp, envelope).
location(key, envelope). /* more interesting thing*/

edible(apple).
edible(crackers).
tastes_yucky(broccoli).

/* You are here now in the  game.*/
here(kitchen). 
where_food(X,Y) :- location(X,Y),edible(X).
where_food(X,Y) :- location(X,Y),tastes_yucky(X).

connect(X,Y):-door(X,Y).
connect(X,Y):-door(Y,X).


/*list all things in the correspondign room*/
list_things(Place) :- location(X,Place),tab(2),write(X),nl,fail.

list_things(_).  /* always correct  AnyPlace's value we don't care, 只能用_ underscore)*/


list_things_to_eat(Place) :- where_food(X,Place),tab(2),write(X),nl,fail.
list_things_to_eat(_) .


list_connections(Place) :- connect(X,Place),tab(2),write(X),nl,fail.
list_connections(_).


look:- here(Place), write('You are in the '),write(Place),write(' now!'),nl,write('You can see:'),nl,
list_things(Place),write('what the things you can eat  are below:'),nl,list_things_to_eat(Place),
write('You can also go to '),nl,list_connections(Place).






c_to_f(C,F):- F is C*9/5+32.
freezeing(F):-F=<32.

goto(Place) :- can_go(Place),move(Place),look.
can_go(Place) :- here(X),connect(X,Place).
can_go(_) :- write('You can''t get there from here.'),nl,fail.
move(Place):- retract(here(Y)),asserta(here(Place)).


take(X):- can_take(X),take_object(X).

can_take(Thing) :-here(Place),location(Thing,Place).
can_take(Thing) :- write('There is no '),write(Thing),write(' Here'),nl,fail.

take_object(X):- retract(location(X,_)),asserta(hava(X)),write('taken'),nl.


backtracking_assert(X):- asserta(X). 
backtracking_assert(X):- retract(X),fail. 


is_contained_in(T1,T2) :- location(T1,T2).
is_contained_in(T1,T2) :- location(X,T2),is_contained_in(T1,X).


object(candle,red   ,small,1).
object(apple ,red   ,small,1).
object(apple ,green ,small,1).
object(apple ,blue  ,big,50).
object(desk,brwon,dimension(6,3,3),90).
object(desk,color(brwon),size(dimension(6,3,3)),weight(90)).
object(desk,color(brwon),size(large),weight(90)).

location_s(object(candle,red    ,small,1),kitchen).
location_s(object(apple ,bluired ,small,1),kitchen).
location_s(object(apple ,green   ,small,1),kitchen).
location_s(object(apple ,blue  ,big,50),kitchen).


can_take_s(Thing) :- here(Room),location_s(object(Thing,_,small,_),Room).
can_take_s(Thing) :- here(Room),location_s(object(Thing,_,big,_),Room),write('The '),write(Thing),
                write(' is too big to carry!'),nl,fail.
can_take_s(Thing) :- here(Room),not(location_s(object(Thing,_,_,_),Room)),write('There is no '),
                write(Thing),write('Here.'),nl,fail.

list_things_s(Place) :- location_s(object(Thing,Color,Size,Weight),Place),
    write('A '),write(Size),tab(1),write(Color),tab(1),write(Thing),write(',weighing '),
    write_weight(Weight),nl,fail.
list_things_s(_).

write_weight(1):-write('1 pound').
write_weight(W):-W>1,write(W),write(' pounds').


loc_list([apple,broccoil,crackers],kitchen).
loc_list([desk,computer],office).
loc_list([flashlight,envelope],desk).
loc_list([stamp,key],envelope).
loc_list(['Washing machine'],cellar).
loc_list([nani],'Washing machine').
loc_list([],hall). /* empty list*/

/*length1([zha,df,gs],H).*/
length1([],0).
length1([H|T],N) :- length1(T,M),N is M+1.

member(X,[X|List]).
member(X,[Element|List]) :- member(X,List).

prefix([],List).
prefix([X|Prefix],[X|List]):- prefix(Prefix,List).

suffix(Suffix,Suffix).
prefix(Suffix,[X|List]) :- suffix(Suffix,List).

vowel(X) :- member(X,[a,e,i,o,u]).
digit(D) :- member(D,['0','1','2','3','4','5','6','7','8','9']).

/*reverse([ ],[ ]).
reverse([X|L],Rev) :- reverse(L,RL), append(RL,[X],Rev).*/

/*reverse([a,b,v],Rl).*/
reverse([ ],[ ]).
reverse(L,RL) :- reverse(L,[ ],RL).

reverse([ ],RL,RL).
reverse([X|L],PRL,RL) :- reverse(L,[X|PRL],RL).

/* isort([6,3,6,2,4,64,234],Sl) */

isort([ ],[ ]).
isort([X|UnSorted],AllSorted) :- isort(UnSorted,Sorted),
                                 insert(X,Sorted,AllSorted).

insert(X,[ ],[X]).
insert(X,[Y|L],[X,Y|L]) :- X =< Y.
insert(X,[Y|L],[Y|IL]) :-  X > Y, insert(X,L,IL).


facTail(0,1).
facTail(N,F) :- N > 0, facTail(N,1,F).

facTail(1,F,F).
facTail(N,PP,F) :- N > 1, NPp is N*PP, M is N-1, 
       facTail(M,NPp,F). 


fibTail(0,1).
fibTail(1,1).
fibTail(N,F) :- N > 1, fibTail(N,1,1,F).

fibTail(2,F1,F2,F) :- F is F1 + F2.
fibTail(N,F1,F2,F) :- N > 2, N1 is N - 1, NF1 is F1 + F2,
    fibTail(N1,NF1,F1,F).

/*
primes(PL) :- natlist(2,L2), sieve(L2,PL).

sieve([ ],[ ]).
sieve([P|L],[P|IDL]) :- sieveP(P,L,PL), sieve(PL,IDL).

sieveP(P,[ ],[ ]). 
sieveP(P,[N|L],[N|IDL]) :- N mod P  >  0, sieveP(P,L,IDL).
sieveP(P,[N|L],   IDL)  :- N mod P =:= 0, sieveP(P,L,IDL).
*/
/*
merge( List1, List2, List3 ) :-
 ( List1 = [], !, List3 = List2 );
 ( List2 = [], !, List3 = List1 );
 ( List1 = [X|L1], List2 = [Y|L2 ),
((X < Y, ! Z = X, merge( L1, List2, L3 ) );
( Z = Y, merge( List1, L2, L3 ) )),
  List3 = [Z|L3].
*/

merge([],List2,List2).
merge(List1,[],List1).

merge([X|List1],[Y|List2],[Z|List3]) :- X<Y,!,merge(List1,List2,List3).
merge(List1,[Y|List2],[Y|List3]) :- merge(List1,List,List3).

```

<hr/>
<h2 = id="5">5. fathers</h2>

fathers is about the family tree.

``` prolog
male(sai).
male(xishan).
male(fanxi).
male(fansi).
male(zhaoliang).
female(ma).
female(ximei).
female(xiaoying).
female(xuexi).
father(sai,fanxi).
father(sai,fansi).
father(fanxi,zhaoliang).
father(fanxi,xiaoying).
mother(ma,fanxi).
mother(ma,fansi).
mother(ximei,xiaoying).
mother(ximei,zhaoliang).
wife(sai,ma).
wife(fanxi,ximei).
wife(famsi,xuexi).

grandfather(X,Y) :- father(X,Z),father(Z,Y).
grandmother(X,Y):-mother(X,Z),father(Z,Y).
dauthter(X,Y):-father(X,Y),female(Y).
dauthter(X,Y):-mother(X,Y),female(Y).
son(X,Y):-father(X,Y),male(Y).
son(X,Y):-mother(X,Y),male(Y).

parent(sai,fanxi).
parent(ma,fanxi).
parent(fanxi,zhaoliang).
parent(ximei,zhaoliang).
parent(fanxi,xiaoying).
parent(ximei,xiaoying).
parent(zhaoliang,zhaoliangson).
ancestor(X,Y):-parent(X,Y).
ancestor(X,Y):-parent(X,Z),ancestor(Z,Y).
sister(X,Y):-parent(Z,X),parent(Z,Y),female(X),X\=Y.
brother(X,Y):-father(Z,X),father(Z,Y),male(X),male(Y),X\=Y,write(Y),tab(2),write(X),nl,fail. 
uncle(X,Y) :- brother(X,Z),father(Z,Y). 
isMale(X):-father(X,_).  
isMale(X):-wife(X,_). 
isMale(X):-male(X). 
isFemale(X):-mother(X,_).  
isFemale(X):-wife(_,X).
isFemale(X):-female(X).
aunt(X,Y) :- parent(L,Y),male(L),brother(L,Z),wife(Z,X).

```

<hr/>
<h2 = id="6">6. descendC</h2>

``` prolog
child(martha,charlotte).
child(charlotte,caroline).
child(caroline,laura).
child(laura,rose).


descend(X,Y) :- child(X,Y).
descend(X,Y) :- child(X,Z),
    descend(Z,Y).

```

<hr/>
<h2 = id="7">7. likes</h2>

``` prolog
%% Demo coming from http://clwww.essex.ac.uk/course/LG519/2-facts/index_18.html
%%
%% Please load this file into SWI-Prolog
%%
%% Sam's likes and dislikes in food
%%
%% Considering the following will give some practice
%% in thinking about backtracking.
%%
%% You can also run this demo online at
%% http://swish.swi-prolog.org/?code=https://github.com/SWI-Prolog/swipl-devel/raw/master/demo/likes.pl&q=likes(sam,Food).

/** <examples>
?- likes(sam,dahl).
?- likes(sam,chop_suey).
?- likes(sam,pizza).
?- likes(sam,chips).
?- likes(sam,curry).
*/

likes(sam,Food) :-
        indian(Food),
        mild(Food).
likes(sam,Food) :-
        chinese(Food).
likes(sam,Food) :-
        italian(Food).
likes(sam,chips).

indian(curry).
indian(dahl).
indian(tandoori).
indian(kurma).

mild(dahl).
mild(tandoori).
mild(kurma).

chinese(chow_mein).
chinese(chop_suey).
chinese(sweet_and_sour).

italian(pizza).
italian(spaghetti).

```

<hr/>
<h2 = id="8">8. helo(nature)</h2>

one simple nature language.

``` prolog
s --> simple_s,conj,simple_s.
s --> simple_s.
simple_s --> np(subject), vp.
np(_) --> det, n;det,adjectives,n.
np(X) --> pro(X).
vp --> v, np(object).
vp --> v.
det --> [the].
det --> [a].
n --> [woman].
n --> [man].
v --> [shoots].
adjectives --> adjective.
adjectives --> adjective,adjective.
adjective --> [young].
adjective --> [beautiful].
adjective --> [sunshine].
adjective --> [lovely].
adjective --> [sweet].
adjective --> [handsome].
pro(subject) --> [he].
pro(subject) --> [she].
pro(subject) --> [i].
pro(object) --> [him].
pro(object) --> [her].
pro(object) --> [me].

conj --> [and].
conj --> [or].
conj --> [but].
conj --> [so].


```

<hr/>
<h2 = id="9">9. helo(natureExtend)</h2>

``` prolog

sh(Count) --> as(Count), bs(Count), cs(Count).
as(0) --> [].
as(NewCnt) --> [a], as(Cnt), {NewCnt is Cnt + 1}.
bs(0) --> [].
bs(NewCnt) --> [b], bs(Cnt), {NewCnt is Cnt + 1}.
cs(0) --> [].
cs(NewCnt) --> [c], cs(Cnt), {NewCnt is Cnt + 1}.
```

<hr/>
<h2 = id="10">10. printM(printMovie)</h2>

``` prolog

% This is the file: printMovies.pl
:- module(printMovies,[printMovies/1]).
printMovies(Director):-
    setof(Film,directed(Director,Film),List),
    displayList(List).

displayList([]):- nl.
displayList([X|L]):-
    write(X), nl,
    displayList(L).


```

<hr/>
<h2 = id="11">11. printM(printActor)</h2>

``` prolog
% This is the file: printActors.pl
:- module(printActors,[printActors/1]).
printActors(Film):-
    setof(Actor,starring(Actor,Film),List),
    displayList(List).

displayList([]):- nl.
displayList([X|L]):-
    write(X), tab(1),
    displayList(L).

```

<hr/>
<h2 = id="12">12. readM(testRead)</h2>

``` prolog
mainRead:-
    open('./readFile/mat.txt',read,S),
    read(S,H1),
    read(S,H2),
    read(S,H3),
    read(S,H4),
    close(S),
    write([H1,H2,H3,H4]), nl.

```

mat.txt: 

```
gryffindor，sdfasdf.
hufflepuf中文f.
ravenclaw.
slytherin.

```
<hr/>
<h2 = id="13">13. readM(testRead2)</h2>

``` prolog

mainRead2:-
    open('./readFile/mat.txt',read,S),
    readHouses(S,Houses),
    close(S),
    write(Houses), nl.

readHouses(S,[]):-
    at_end_of_stream(S), !.

readHouses(S,[X|L]):-
    \+ at_end_of_stream(S), !,
    read(S,X),
    readHouses(S, L).
```

<hr/>
<h2 = id="14">14. readHouses</h2>

house.txt:

```
gryffindor.
hufflepuff.
ravenclaw.
slytherin

```



``` prolog

/*
mainRead :- 
    open('house.txt',read,S),
    read(S,H1),
    read(S,H2),
    read(S,H3),
    %    read(S,H4),
    close(S),
    write([H1,H2,H3]),nl.
*/



mainReadHouse:-
    open('house.txt',read,S),
    readHouses(S,Houses),
    close(S),
    write(Houses),nl.

readHouses(S,[]) :-
    at_end_of_stream(S).

readHouses(S,[X|L]) :-
    \+ at_end_of_stream(S),
    read(S,X), 
    readHouses(S,L).

```

<hr/>


[1]: http://jueqingsizhe66.github.io/blog/2016/07/19/prologxiao-you-xi/ 
[2]: /images/prolog/prolog_exercise.png 
