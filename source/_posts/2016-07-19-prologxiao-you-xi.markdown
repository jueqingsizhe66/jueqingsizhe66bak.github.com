---
layout: post
title: "prolog小游戏"
date: 2016-07-19 19:35:37 +0800
comments: true
categories: prolog
---

![games][1]
<!--more-->

prolog 作为一门logic programming language,逻辑推理是它的强项。游戏也是一个逻辑推理的过程，prolog照样可以，通过该游戏的写作可以让你熟悉

1. 事实和规则。
2. built-in predicates.
3. IO
4. cut-fail操作



游戏源代码如下，进一步参考[prolog教程][2].

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


[1]:/images/prolog/games.png
[2]:http://tieba.baidu.com/p/128101598 
