---
layout: post
title: "sequence understanding in prolog"
date: 2016-09-03 23:07:32 +0800
comments: true
categories: prolog
---

+ 1. [member](#member)
+ 2. [substitution and rember](#subst)
+ 3. [flattern](#flattern)
+ 4. [length](#length)
+ 5. [reverse](#reverse)
+ 6. [append](#append)
+ 7. [insertsort](#insertsort)
+ 8. [quicksort](#quicksort)
+ 9. [findall,bagof,setof](#findall)
+ 10. [interaction,union](#inter)
+ 11. [List to map](#map)
+ 12. [max,avg,cross-product](#algo)
+ 13. [Element At](#elementAt)

![list][1]

<!--more-->


Assume list is a top part, while others are down part, then `top->down` with [depth-first(<strong>because multiple clauses in your knowledge database</strong>) and breadth-first(because <strong>multiple subgoals in your main goal</strong>)][4].
Assume list is a bottom part, while others are up part, then `bottom->up` with depth-first and breadth-first.

lists thinking can also extend to the numbers organisation(111000101)

lists can list the separtion of the concept.
[Natural Language  = Logic(the graph of the relationship) + Control( The flow in the graph of the relationship) P130][4],
logic graph keeps the bees. The logic is like an puzzle.

[Algorithm  = Logic(the graph of the relationship) + Control( The flow in the graph of the relationship) P128][4],

每一次logic都需要一种格局(格局才能带领你继续走下去，也叫做前进的动力)，prolog给我的感觉是风筝和枯井,到底算法和自然语言是什么？还没想好。
![prolog][13]


<hr/>
<font color="red">Logic is the data structure(the statement is wrong)</font>. You can change the logic in the flow of the data just like switcher.
The conceputual separation of logic from control, so you can improve the algorithms by improving
their component without changing the logic components.(All your define in the prolog pl files are called
logic units, while leave the components of control implicitly to the prolog(such as depth-first or breadth-first))

If you are a strict prolog programmer, maybe you should care about the control part of the prolog, because it effect
the efficiency of the program execution(problem-solving stratege:depth-first and breadth-first) without affecting the 
meaning of the problem(the logic component define the significance of the program or algorithm or natural language).

So if the logic is the same, but the controller part is different , you can get the same result(only the different efficiency)
in particularly, replacing bottom-up with top-down and parallel execution etc.

The arguments for separting logic from control are like the arguments for separting procedures from data structure.

So<font color="red"> the Logic actually is the procedure, but not the data structure</font>(data structure can be changed without change the meaning of 
the program. While if you change the procedure(or function) ,the meaning of the programe have to change).

the separtion of the concept can let you know the alogrithm better, and replacing the inefficient controller part with better , without
changing the meaning of the procedure(or program = procedure+data\_structure).

So , the prolog code is mainly consists of the clauses which contain facts and rules.

Finally , the key to problem is start the right , correct ,more importantly clear logic.
logic is an line or edge, while controller is an swithcer which can change the ending line with different lines(backtracing(top-down),bottom-up).
logic is used to create database, while control is just querying the database(so it means different searching stratege)

所以想要存在控制，也就是存在着很多逻辑线，并且都可以达到问题的求解(To control, exist many logic lines which direct initial to the top of the prymaid). 单单一条线路，也叫做固定流水线或者固定流程.


if you ... then .. : bottom-up.
he likes you if you...: top-down.


Lists just means flowers bloom(花的绽放).

然而所有的logic都得基于具体对象(中国国情),来推演logic,且看[聪明的投资者:投资股票就是投资公司而不是行业或者个人][10] 和[投资者的未来:如果公司亏了怎么办？][11]。任何一本书，任何一个定理，任何一个观点，任何一个人，都有着它存在的假设条件(bottom-up 注意bottom的合理性)。观点的转化，定理的活用（没学会,但是在坚持）都是需要一个转化和引导的过程（也就是controller part),转化和引导到正常前提下的结论。

投资一个公司，最直接的数据就是行业数据和公司报表.研究prolog同样也要从行业的标准入手。
<h2 id="member">member</h2>

``` prolog

member(X, [X|_]).
member(X, [_|List]) :- 
    member(X, List).


```


<h2 id="subst">substitution and rember</h2>

substitution is also called replace.


<h2 id="flattern">flattern</h2>

flattern:


``` prolog

my_flatten(X,[X]) :- 
    \+ is_list(X).
my_flatten([],[]).
my_flatten([X|Xs],Zs) :- 
    my_flatten(X,Y), 
    my_flatten(Xs,Ys), 
    append(Y,Ys,Zs).

```

flattern another method by [Richard A.O'Keefe][14]:

```
some
```

<h2 id="length">length</h2>
length,参考[bicycleextend][2], 其中`list_len`就是length.

<h2 id="reverse">reverse</h2>


``` prolog
reverse([ ], [ ]).
reverse(L, RL) :- 
    reverse(L, [ ], RL).

reverse([ ], RL, RL).
reverse([X|L], PRL, RL) :- 
    reverse(L, [X|PRL], RL). %% Take note the X position




```

[nreverse][3]
``` prolog

benchmark1(Data, Out) :-
	nreverse(Data, Out).

data([1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,
	16,17,18,19,20,21,22,23, 24,25,26,27,28,29,30]).

nreverse([], []).
nreverse([X|L0], L) :-
	nreverse(L0, L1), concatenate(L1, [X], L).

concatenate([], L, L).
concatenate([X|L1], L2, [X|L3]) :-	
	concatenate(L1, L2, L3).

```



<h2 id="append">append</h2>

``` prolog

append1([], L, L).
append1([H|T2], L, [H|L3]) :- 
    append1(T2, L, L3).

prefix3(P, L) :- 
    append(P, _, L), 
    write(P), 
    nl, fail.
subfix3(S, L) :- 
    append(_, S, L).

```

<h2 id="insertsort">insertsort</h2>

``` prolog
/* isort([6, 3, 6, 2, 4, 64, 234], Sl) */

isort([ ], [ ]).
isort([X|UnSorted], AllSorted) :- 
    isort(UnSorted, Sorted), 
    insert(X, Sorted, AllSorted).

insert(X, [ ], [X]).
insert(X, [Y|L], [X, Y|L]) :- 
    X =< Y.
insert(X, [Y|L], [Y|IL]) :-  
    X > Y,  insert(X, L, IL).
```


<h2 id="quicksort"> quicksort</h2>
quicksort,来自[scistus][3]:

```

benchmark(Data, Out) :-
	qsort(Data, Out, []).

data([27,74,17,33,94,18,46,83,65,2,32,53,28,85,99,47,28,82,6,11,55,29,39,81,
90,37,10,0,66,51,7,21,85,27,31,63,75,4,95,99,11,28,61,74,18, 92,40,53,59,8]).

qsort([], R, R).
qsort([X|L], R, R0) :-
	partition(L, X, L1, L2),
	qsort(L2, R1, R0),
	qsort(L1, R, [X|R1]).

partition([],_,[],[]).
partition([X|L],Y,[X|L1],L2) :-
	X =< Y, !,
	partition(L,Y,L1,L2).
partition([X|L],Y,L1,[X|L2]) :-
	partition(L,Y,L1,L2).


```

测试代码：
``` prolog
data(X),benchmark(X,Out).
X = [27, 74, 17, 33, 94, 18, 46, 83, 65|...],
Out = [0, 2, 4, 6, 7, 8, 10, 11, 11|...].
```

通过[QuickSort4](#qsort4)可以看出他是根据找到的第一个X，然后划分为前后两个部分(这个思想有用！)
<hr/>
<br/>
<br/>
<h3 id="qsort1"><font color="red">Fig1:</font></h3>
![qsort1][15]
<hr/>
<br/>
<br/>
<h3 id="qsort2"><font color="red">Fig2:</font></h3>
![qsort2][16]
<hr/>
<br/>
<br/>
<h3 id="qsort3"><font color="red">Fig3:</font></h3>
![qsort3][17]
<hr/>
<br/>
<br/>
<h3 id="qsort4"><font color="red">Fig4:</font></h3>
![qsort4][18]
<hr/>
<br/>
<br/>
<h3 id="qsort5"><font color="red">Fig5:</font></h3>
![qsort5][19]
<hr/>
<br/>
<br/>
<h3 id="qsort6"><font color="red">Fig6:</font></h3>
![qsort6][20]
<hr/>
<br/>
<br/>
<h3 id="qsort7"><font color="red">Fig7:</font></h3>
![qsort7][21]
<hr/>
<br/>
<br/>
<h3 id="qsort8"><font color="red">Fig8:</font></h3>
![qsort8][22]
<hr/>
<br/>
<br/>
<h3 id="qsort9"><font color="red">Fig9:</font></h3>
![qsort9][23]
<hr/>
<br/>
<br/>
<h3 id="qsort10"><font color="red">Fig10:</font></h3>
![qsort10][24]
<hr/>
<br/>
<br/>
<h3 id="qsort11"><font color="red">Fig11:</font></h3>
![qsort11][25]
<hr/>
<br/>
<br/>
<h3 id="qsort12"><font color="red">Fig12:</font></h3>
![qsort12][26]
<hr/>
<br/>
<br/>
<h3 id="qsort14"><font color="red">Fig14:</font></h3>
![qsort14][28]
<hr/>
<br/>
<br/>
<h3 id="qsort15"><font color="red">Fig15:</font></h3>
![qsort15][29]
<hr/>
<br/>
<br/>
<h3 id="qsort16"><font color="red">Fig16:</font></h3>
![qsort16][30]
<hr/>
<br/>
<br/>
<h3 id="qsort17"><font color="red">Fig17:</font></h3>
![qsort17][31]
<hr/>
<br/>
<br/>
<h3 id="qsort18"><font color="red">Fig18:</font></h3>
![qsort18][32]
<hr/>
<br/>
<br/>
<h3 id="qsort19"><font color="red">Fig19:</font></h3>
![qsort19][33]
<hr/>
<br/>
<br/>
<h3 id="qsort20"><font color="red">Fig20:</font></h3>
![qsort20][34]
<hr/>
<br/>
<br/>
<h3 id="qsort21"><font color="red">Fig21:</font></h3>
![qsort21][35]
<hr/>
<br/>
<br/>
<h3 id="qsort22"><font color="red">Fig22:</font></h3>
![qsort22][36]
<hr/>
<br/>
<br/>
<h3 id="qsort23"><font color="red">Fig23:</font></h3>
![qsort23][37]
<hr/>
<br/>
<br/>
<h3 id="qsort24"><font color="red">Fig24:</font></h3>
![qsort24][38]
<hr/>
<br/>
<br/>
<h3 id="qsort25"><font color="red">Fig25:</font></h3>
![qsort25][39]
<hr/>
<br/>
<br/>
<h3 id="qsort26"><font color="red">Fig26:</font></h3>
![qsort26][40]
<hr/>
<br/>
<br/>
<h3 id="qsort27"><font color="red">Fig27:</font></h3>
![qsort27][41]
<hr/>
<br/>
<br/>
<h3 id="qsort28"><font color="red">Fig28:</font></h3>
![qsort28][42]
<hr/>
<br/>
<br/>
<h3 id="qsort29"><font color="red">Fig29:</font></h3>
![qsort29][43]
<hr/>
<br/>
<br/>
<h3 id="qsort30"><font color="red">Fig30:</font></h3>
![qsort30][44]
<hr/>
<br/>
<br/>
<h3 id="qsort31"><font color="red">Fig31:</font></h3>
![qsort31][45]
<hr/>
<br/>
<br/>
<h3 id="qsort32"><font color="red">Fig32:</font></h3>
![qsort32][46]
<hr/>
<br/>
<br/>
<h3 id="qsort33"><font color="red">Fig33:</font></h3>
![qsort33][47]
<hr/>
<br/>
<br/>
<h3 id="qsort34"><font color="red">Fig34:</font></h3>
![qsort34][48]
<hr/>
<br/>
<br/>
<h3 id="qsort35"><font color="red">Fig35:</font></h3>
![qsort35][49]
<hr/>
<br/>
<br/>
<h3 id="qsort36"><font color="red">Fig36:</font></h3>
![qsort36][50]
<hr/>
<br/>
<br/>
<h3 id="qsort37"><font color="red">Fig37:</font></h3>
![qsort37][51]
<hr/>
<br/>
<br/>
<h3 id="qsort38"><font color="red">Fig38:</font></h3>
![qsort38][52]
<hr/>
<br/>
<br/>
<h3 id="qsort39"><font color="red">Fig39:</font></h3>
![qsort39][53]
<hr/>
<br/>
<br/>
<h3 id="qsort40"><font color="red">Fig40:</font></h3>
![qsort40][54]
<hr/>
<br/>
<br/>
<h3 id="qsort41"><font color="red">Fig41:</font></h3>
![qsort41][55]
<hr/>
<br/>
<br/>
<h3 id="qsort42"><font color="red">Fig42:</font></h3>
![qsort42][56]
<hr/>
<br/>
<br/>
<h3 id="qsort43"><font color="red">Fig43:</font></h3>
![qsort43][57]
<hr/>
<br/>
<br/>
<h3 id="qsort44"><font color="red">Fig44:</font></h3>
![qsort44][58]
<hr/>
<br/>
<br/>
<h3 id="qsort45"><font color="red">Fig45:</font></h3>
![qsort45][59]
<hr/>
<br/>
<br/>
<h3 id="qsort46"><font color="red">Fig46:</font></h3>
![qsort46][60]
<hr/>
<br/>
<br/>
<h3 id="qsort47"><font color="red">Fig47:</font></h3>
![qsort47][61]
<hr/>
<br/>
<br/>
<h3 id="qsort48"><font color="red">Fig48:</font></h3>
![qsort48][62]
<hr/>
<br/>
<br/>
<h3 id="qsort49"><font color="red">Fig49:</font></h3>
![qsort49][63]
<hr/>
<br/>
<br/>
<h3 id="qsort50"><font color="red">Fig50:</font></h3>
![qsort50][64]
<hr/>
<br/>
<br/>
<h3 id="qsort51"><font color="red">Fig51:</font></h3>
![qsort51][65]
<hr/>
<br/>
<br/>
<h3 id="qsort52"><font color="red">Fig52:</font></h3>
![qsort52][66]
<hr/>
<br/>
<br/>
<h3 id="qsort53"><font color="red">Fig53:</font></h3>
![qsort53][67]
<hr/>
<br/>
<br/>
<h3 id="qsort54"><font color="red">Fig54:</font></h3>
![qsort54][68]
<hr/>
<br/>
<br/>
<h3 id="qsort55"><font color="red">Fig55:</font></h3>
![qsort55][69]
<hr/>
<br/>
<br/>
<h3 id="qsort56"><font color="red">Fig56:</font></h3>
![qsort56][70]
<hr/>
<br/>
<br/>
<h3 id="qsort57"><font color="red">Fig57:</font></h3>
![qsort57][71]
<hr/>
<br/>
<br/>
<h3 id="qsort58"><font color="red">Fig58:</font></h3>
![qsort58][72]
<hr/>
<br/>
<br/>
<h3 id="qsort59"><font color="red">Fig59:</font></h3>
![qsort59][73]
<hr/>
<br/>
<br/>
<h3 id="qsort60"><font color="red">Fig60:</font></h3>
![qsort60][74]
<hr/>
<br/>
<br/>
<h3 id="qsort61"><font color="red">Fig61:</font></h3>
![qsort61][75]
<hr/>
<br/>
<br/>
<h3 id="qsort62"><font color="red">Fig62:</font></h3>
![qsort62][76]
<hr/>
<br/>
<br/>
<h3 id="qsort63"><font color="red">Fig63:</font></h3>
![qsort63][77]
<hr/>
<br/>
<br/>
<h3 id="qsort64"><font color="red">Fig64:</font></h3>
![qsort64][78]
<hr/>
<br/>
<br/>
<h3 id="qsort65"><font color="red">Fig65:</font></h3>
![qsort65][79]
<hr/>
<br/>
<br/>
<h3 id="qsort66"><font color="red">Fig66:</font></h3>
![qsort66][80]
<hr/>
<br/>
<br/>
<h3 id="qsort67"><font color="red">Fig67:</font></h3>
![qsort67][81]
<hr/>
<br/>
<br/>
<h3 id="qsort68"><font color="red">Fig68:</font></h3>
![qsort68][82]
<hr/>
<br/>
<br/>
<h3 id="qsort69"><font color="red">Fig69:</font></h3>
![qsort69][83]
<hr/>
<br/>
<br/>
<h3 id="qsort70"><font color="red">Fig70:</font></h3>
![qsort70][84]
<hr/>
<br/>
<br/>
<h3 id="qsort71"><font color="red">Fig71:</font></h3>
![qsort71][85]
<hr/>
<br/>
<br/>
<h3 id="qsort72"><font color="red">Fig72:</font></h3>
![qsort72][86]
<hr/>
<br/>
<br/>
<h3 id="qsort73"><font color="red">Fig73:</font></h3>
![qsort73][87]
<hr/>
<br/>
<br/>
<h3 id="qsort74"><font color="red">Fig74:</font></h3>
![qsort74][88]
<hr/>
<br/>
<br/>
<h3 id="qsort75"><font color="red">Fig75:</font></h3>
![qsort75][89]
<hr/>
<br/>
<br/>
<h3 id="qsort76"><font color="red">Fig76:</font></h3>
![qsort76][90]
<hr/>
<br/>
<br/>
<h3 id="qsort77"><font color="red">Fig77:</font></h3>
![qsort77][91]
<hr/>
<br/>
<br/>
<h3 id="qsort78"><font color="red">Fig78:</font></h3>
![qsort78][92]
<hr/>
<br/>
<br/>
<h3 id="qsort79"><font color="red">Fig79:</font></h3>
![qsort79][93]
<hr/>
<br/>
<br/>
<h3 id="qsort80"><font color="red">Fig80:</font></h3>
![qsort80][94]



最终所有的数字被分为两个部分，大于27的和小于27的，然后接着再递归直到遍历所有数字的划分过程。

<h2 id="findall">findall, bagof, setof</h2>

``` prolog

allAge(harry,13).
allAge(draco,33).
allAge(ron,11).
allAge(hermione,53).
allAge(dumbledore,38).
allAge(hagrid,3).


% findall(X, allAge(X, Y), Out)).
% bagof(X, allAge(X, Y), Out).
% setof(X, allAge(X, Y), Out).
% findall(List,bagof(X,allAge(X,Y),List),Z).


```

测试代码
``` prolog
findall(X, allAge(X,Y),Out)
    .
Out = [harry, draco, ron, hermione, dumbledore, hagrid].
```

findall(+ColumnOfArgu,+predicate, -OutputList) 输出所有满足predicate条件的个体；不按照顺序

findall(Template, Enumarator, Instances). Instances是Template的对象序列。该序列满足Enumerator，并且按照prolog的
查询策略（proof of stratege of particular porlog system)进行排序输出。
The template May be single variables, may be any items containing any number of variables,even none.

之所以叫做Enumarator,是因为他是实际跟prolog的数据库进行交互，并且包含着变量一般比Template较多，这样就可以通过不在Template的
某个变量进行遍历，找到所有满足Template条件的变量，忽略Enumarator可能对应的不关心变量（Enumarate bindings for those variables
which are not occured in the Template)。

所以在遍历的过程中可能出现：

1. 该变量出现在Enumarator 但不在Template
2. 该变量即出现在Enumarator 也在Template


``` prolog
35 ?- bagof(X, allAge(X,Y),Out)
|    .
Y = 3,
Out = [hagrid] ;
Y = 11,
Out = [ron] ;
Y = 13,
Out = [harry] ;
Y = 33,
Out = [draco] ;
Y = 38,
Out = [dumbledore] ;
Y = 53,
Out = [hermione].
```

bagof按照Y的顺序逐个打印(类似于sql语句的select X from database group by Y)。
bagof(Template, Enumarator, InstancesList). 

然后还可以指定排列顺序，通过特殊的字符 `^`,被读作`存在`，比如`^X`叫做`存在X`

``` prolog
37 ?- bagof(X, Y^allAge(X,Y),Out).
Out = [harry, draco, ron, hermione, dumbledore, hagrid].



38 ?- bagof(X, X^allAge(X,Y),Out).
Y = 3,
Out = [hagrid] ;
Y = 11,
Out = [ron] ;
Y = 13,
Out = [harry] ;
Y = 33,
Out = [draco] ;
Y = 38,
Out = [dumbledore] ;
Y = 53,
Out = [hermione].
```

``` prolog

36 ?- setof(X, allAge(X,Y),Out).
Y = 3,
Out = [hagrid] ;
Y = 11,
Out = [ron] ;
Y = 13,
Out = [harry] ;
Y = 33,
Out = [draco] ;
Y = 38,
Out = [dumbledore] ;
Y = 53,
Out = [hermione].



39 ?- setof(X, X^allAge(X,Y),Out).
Y = 3,
Out = [hagrid] ;
Y = 11,
Out = [ron] ;
Y = 13,
Out = [harry] ;
Y = 33,
Out = [draco] ;
Y = 38,
Out = [dumbledore] ;
Y = 53,
Out = [hermione].

40 ?- setof(X, Y^allAge(X,Y),Out).
Out = [draco, dumbledore, hagrid, harry, hermione, ron].
```

setof按照X的顺序逐个打印(select X from database group by X)。
setof(Template, Enumarator, InstancesSet). 
注意 `Y^allAge(X,Y)` 按照Y的字母排序

<h2 id="inter"> interaction,union</h2>

``` prolog
intersection1([H|T], L, [H|U]) :- 
    member(H, L),
    !,
    intersection1(T, L, U).

intersection1([H|T], L , U) :-
    !,
    intersection1(T, L, U).

intersection1(_, _, []).

``` prolog

intersect之所以最终会终止，是因为U一直是空的状态，当[H|T]无法满足时候，也就只能执行第三步，
而第三步在L1(也就是[H|T])非空的时候，一直不被执行是因为cut的存在，阻止了递归.
为了更清楚看到结果，我们进行了调试：


<hr/>
<br/>
<br/>
<h3 id="intersect1"><font color="red">intersect1:</font></h3>
![intersect1][101]
<hr/>
<br/>
<br/>
<h3 id="intersect2"><font color="red">intersect2:</font></h3>
![intersect2][102]
<hr/>
<br/>
<br/>
<h3 id="intersect3"><font color="red">intersect3:</font></h3>
![intersect3][103]
<hr/>
<br/>
<br/>
<h3 id="intersect4"><font color="red">intersect4:</font></h3>
![intersect4][104]
<hr/>
<br/>
<br/>
<h3 id="intersect5"><font color="red">intersect5:</font></h3>
![intersect5][105]
<hr/>
<br/>
<br/>
<h3 id="intersect6"><font color="red">intersect6:</font></h3>
![intersect6][106]
<hr/>
<br/>
<br/>
<h3 id="intersect7"><font color="red">intersect7:</font></h3>
![intersect7][107]
<hr/>
<br/>
<br/>
<h3 id="intersect8"><font color="red">intersect8:</font></h3>
![intersect8][108]
<hr/>
<br/>
<br/>
<h3 id="intersect9"><font color="red">intersect9:</font></h3>
![intersect9][109]
<hr/>
<br/>
<br/>
<h3 id="intersect10"><font color="red">intersect10:</font></h3>
![intersect10][110]
<hr/>
<br/>
<br/>
<h3 id="intersect11"><font color="red">intersect11:</font></h3>
![intersect11][111]
<hr/>
<br/>
<br/>
<h3 id="intersect12"><font color="red">intersect12:</font></h3>
![intersect12][112]
<hr/>
<br/>
<br/>
<h3 id="intersect13"><font color="red">intersect13:</font></h3>
![intersect13][113]
<hr/>
<br/>
<br/>
<h3 id="intersect14"><font color="red">intersect14:</font></h3>
![intersect14][114]
<hr/>
<br/>
<br/>
<h3 id="intersect15"><font color="red">intersect15:</font></h3>
![intersect15][115]
<hr/>
<br/>
<br/>
<h3 id="intersect16"><font color="red">intersect16:</font></h3>
![intersect16][116]
<hr/>
<br/>
<br/>
<h3 id="intersect17"><font color="red">intersect17:</font></h3>
![intersect17][117]
<hr/>
<br/>
<br/>
<h3 id="intersect18"><font color="red">intersect18:</font></h3>
![intersect18][118]

类似的改造union1

``` prolog
union1([H|T], L, U) :- 
    member(H, L),
    !,
    union1(T, L, U).

union1([H|T], L ,[H|U]) :-
    !,
    union1(T, L, U).

union1([], L, [L]).

```

而这时结果不太对，

``` prolog
97 ?- union1([1,2,3],[2,3,4],U).
U = [1, [2, 3, 4]].
```


只需要改变最后一步即可，

``` prolog
union1([H|T], L, U) :- 
    member(H, L),
    !,
    union1(T, L, U).

union1([H|T], L ,[H|U]) :-
    !,
    union1(T, L, U).

%union1([], L, [L]).
union1([], L, L).
```

测试结果ok,

``` prolog
?99 ?- union1([1,2,3],[2,3,4],U).
U = [1, 2, 3, 4].

```

<h2 id="map">List to map </h2>

list表现为map数据结构：

``` prolog
pairupbad([], [], []).
pairupbad([Key|Keys], [Val|Vals], [[Key,Val]|Pairs]) :-
    pairupbad(Keys, Vals, Pairs).

lookupbad(Key, [[Key, Val]|_], Val).
lookupbad(Key, [_|Pairs], Val) :-
    loopupbad(Key, Pairs, Val).


pairupgood([], [], []).
pairupgood([Key|Keys], [Val|Vals], [Key-Val|Pairs]) :-
    pairupgood(Keys, Vals, Pairs).

lookupgood(Key, [Key-Val|_], Val).
lookupgood(Key, [_|Pairs], Val) :-
    loopupgood(Key, Pairs, Val).
```

测试结果: 

``` prolog
44 ?- pairupgood([h1,h2,h3,h4,h5,h6],[v1,v2,v3,v5,v6,v9],Pairs),lookupgood(h1,Pairs,Val).
Pairs = [h1-v1, h2-v2, h3-v3, h4-v5, h5-v6, h6-v9],
Val = v1 .

45 ?- pairupbad([h1,h2,h3,h4,h5,h6],[v1,v2,v3,v5,v6,v9],Pairs),lookupbad(h1,Pairs,Val).
Pairs = [[h1, v1], [h2, v2], [h3, v3], [h4, v5], [h5, v6], [h6, v9]],
Val = v1 .

```

当然也有list-to-enumarator, list-to-generators(实际上generator也是list，只不过好像类似于Iterator只能使用一次)

prolog总共有三种表示sequence序列的风格

+ lists
+ generators(data structure together with operations to get the next element and test whether
a sequence is finished),比如可能得配上element\_at，iterator等
+ enumerators(goals which enumerate the elements of a sequence , the next element being obtained by backtracing)



<div align="center"><strong><font color="red">最为重要的是，lists不应该仅仅理解为lists,head start and concatenating with Tail.But to thinking in terms of abstract notion(sequence),we can use sequence with lists,generator,emumerator, and use conventor, we can generator-to-list.we can also list-to-generator or list-to-emumartor(such as findall,bagof,setof)</font></strong></div>

比如，list-to-generators:

``` prolog

%maplist(+Pred,+Xs)
maplist(_,[]).
maplist(P, [X|Xs]):-
    call(P, X),
    maplist(P, Xs).
```

所以，到此为止，本文的标题应该改为:
<div align="center"><h2 id="sequnce">Sequence processing</h2></div>



<h2 id="algo">max,avg,cross-product</h2>

+ maximum

``` prolog
maximum([Head|Tail], Max) :-
    maximum(Tail, Head, Max).

maximum([], Max, Max).
maximum([Head|Tail], Max0, Max) :-
    (
        Head > Max0
    ->
        maximum(Tail, Head, Max)
    ;   
        maximum(Tail, Max0, Max)
    ).

```

测试结果：
```
63 ?- maximum([1,2,4,6,7,3243,4],O).
O = 3243.
```
+ average

average 可以通过下面清晰显示定义，

```  prolog
average(List, Average) :-
    list_sum(List, Sum),
    length(List,Length),
    Average is Sum/Length.

list_sum([],0).
list_sum([Head|Tail], Total) :-
    list_sum(Tail, Right),
    Total is Head + Right.
```


实际average实现如下，通过两个累积表(accumulator)，


``` prolog
average([Head|Tail], Average) :-
    average(Tail, Head, N,  1, D),
    Average is N / D.

average([], N, N, D, D).
average([Head|Tail], N0, N, D0, D) :-
    N1 is N0 + Head,
    D1 is D0 + 1,
    average(Tail, N1, N, D1, D).

```

测试结果：

```

Ȁ59 ?- average([1,22,3,4],X).
X = 7.5.

60 ?- average([1,2,3,4],X).
X = 2.5.
```

+ cross\-product:

![cross][95].

``` prolog
%% get the product of the sequence
product(P,Xs,Ys,PXYs) :-
    product1(Xs, Ys, PXYs, P).

product1([],_,[],_).
product1([X|Xs], Ys, PXYs0, P) :-
    product1(Ys, X, P, PXYs0, PXYs1),
    product1(Xs, Ys, PXYs1, P).

product1([], _, _) --> [].
product1([Y|Ys], X, P) -->
    {call(P, X, Y, PXY)},
    [PXY],
    product1(Ys, X, P).



```

导入到swi\-prolog的信息

```
77 ?- listing(product1).
product1([], _, [], _).
product1([B|E], A, D, C) :-
        product1(A, B, C, D, F),
        product1(E, A, F, C).

product1([], _, _, A, A).
product1([C|G], B, A, D, I) :-
        call(A, B, C, F),
        E=D,
        E=[F|H],
        product1(G, B, A, H, I).

true.
```

测试结果（不正确）
```
48 ?- product1(*,[1,2,3],[2,3,4],Res).
false.

```
未得到想要的结果，留待以后。

<h2 id="elementAt">Element At</h2>

``` prolog
elementAt([Head|Tail], N, Out) :-
    (
        N ==1 
    ->
        Head = Out
    ;
        N1 is N - 1,
        elementAt(Tail, N1, Out)
    ).

elementAt2([Head|Tail],1,Head):-!.
elementAt2([Head|Tail],N,Head) :-
    N1 is N-1,
    elementAt2(Tail, N1, Head).
```

elementAt2有点问题！！！

原因在于Head联合出现问题

Unify have three usages:

+ unify can match the head part of the goal.
+ unify can deliver the actual parameter to the corespongding term.
+ unify can deliver the result out from the term(such as accumulator and different\_list)

![ele1][96]
<hr/>
<br/>
![ele2][97]
<hr/>
<br/>

![ele3][98]
<hr/>
<br/>

![ele4][99]
<hr/>
<br/>

![ele5][100]
<hr/>
<br/>


修正后的结果如下:

``` prolog
elementAt2([Head|Tail],1,Head):-!.
% Head 肯定会跟Head unify 所以
%elementAt2([Head|Tail],N,Head) :-
elementAt2([Head|Tail],N,Out) :-
    N1 is N-1,
    elementAt2(Tail, N1, Out).


```



<h2> Reference:</h2>

+ [Logic For Problem Solving][4] :let you become a better logistics for problem(perfect good!).
+ [Principles of Database and Knowledge-Base Systems:Jeff Ullman ][5]
+ [Applying Prolog Programming Techniques ][6]
+ [Advanced PROLOG: Techniques and Applications(未找到pdf) : 	Peter Ross 	  ][7]
+ [Lecture Notes in Computer Science ][8]
+ [Logic,Programming and prolog][9]
+ [一些好用的ppt slide shows][12]

[1]: /images/prolog/list/list.gif
[2]:http://jueqingsizhe66.github.io/blog/2016/08/24/bicycleextend/ 
[3]:https://sicstus.sics.se/performance.html 
[4]:http://www.docin.com/p-286207487.html 
[5]:http://infolab.stanford.edu/~ullman/ullman-books.html#dscb 	 
[6]:http://www.zen116786.zen.co.uk/Bowles_etal_ApplyingPrologTechniques.pdf 
[7]:http://dl.acm.org/citation.cfm?id=534104 
[8]:http://diyhpl.us/~bryan/papers2/security/advances-in-cryptology/Advances%20in%20Cryptology%20-%20EUROCRYPT%202002(LNCS2332,%20Springer,%202002)(ISBN%203540435530)(557s).pdf 
[9]:http://www.cs.ubbcluj.ro/~csatol/log_funk/prolog/NilsonMaluszynski_Prolog.pdf 
[10]:http://yuedu.163.com/source/66a29a2482ad4172a3bba5ba4188a2c6_4?s=1  
[11]:http://baike.baidu.com/link?url=D5i8XBB8_iCsr22PBg27hie-U1pW971BOzFH3fFJwAxHt9A6H0Q5Sx2VRLUCYXV8TqKin8s_VB7IlEhRL8Emda 
[12]:http://es.slideshare.net/search/slideshow?searchfrom=header&q=prolog 
[13]: /images/prolog/list/whereProlog.gif
[14]:https://mitpress.mit.edu/books/craft-prolog 


[15]:/images/prolog/list/qsort/qsort1.png
[16]:/images/prolog/list/qsort/qsort2.png
[17]:/images/prolog/list/qsort/qsort3.png
[18]:/images/prolog/list/qsort/qsort4.png
[19]:/images/prolog/list/qsort/qsort5.png
[20]:/images/prolog/list/qsort/qsort6.png
[21]:/images/prolog/list/qsort/qsort7.png
[22]:/images/prolog/list/qsort/qsort8.png
[23]:/images/prolog/list/qsort/qsort9.png
[24]:/images/prolog/list/qsort/qsort10.png
[25]:/images/prolog/list/qsort/qsort11.png
[26]:/images/prolog/list/qsort/qsort12.png
[27]:/images/prolog/list/qsort/qsort13.png
[28]:/images/prolog/list/qsort/qsort14.png
[29]:/images/prolog/list/qsort/qsort15.png
[30]:/images/prolog/list/qsort/qsort16.png
[31]:/images/prolog/list/qsort/qsort17.png
[32]:/images/prolog/list/qsort/qsort18.png
[33]:/images/prolog/list/qsort/qsort19.png
[34]:/images/prolog/list/qsort/qsort20.png
[35]:/images/prolog/list/qsort/qsort21.png
[36]:/images/prolog/list/qsort/qsort22.png
[37]:/images/prolog/list/qsort/qsort23.png
[38]:/images/prolog/list/qsort/qsort24.png
[39]:/images/prolog/list/qsort/qsort25.png
[40]:/images/prolog/list/qsort/qsort26.png
[41]:/images/prolog/list/qsort/qsort27.png
[42]:/images/prolog/list/qsort/qsort28.png
[43]:/images/prolog/list/qsort/qsort29.png
[44]:/images/prolog/list/qsort/qsort30.png
[45]:/images/prolog/list/qsort/qsort31.png
[46]:/images/prolog/list/qsort/qsort32.png
[47]:/images/prolog/list/qsort/qsort33.png
[48]:/images/prolog/list/qsort/qsort34.png
[49]:/images/prolog/list/qsort/qsort35.png
[50]:/images/prolog/list/qsort/qsort36.png
[51]:/images/prolog/list/qsort/qsort37.png
[52]:/images/prolog/list/qsort/qsort38.png
[53]:/images/prolog/list/qsort/qsort39.png
[54]:/images/prolog/list/qsort/qsort40.png
[55]:/images/prolog/list/qsort/qsort41.png
[56]:/images/prolog/list/qsort/qsort42.png
[57]:/images/prolog/list/qsort/qsort43.png
[58]:/images/prolog/list/qsort/qsort44.png
[59]:/images/prolog/list/qsort/qsort45.png
[60]:/images/prolog/list/qsort/qsort46.png
[61]:/images/prolog/list/qsort/qsort47.png
[62]:/images/prolog/list/qsort/qsort48.png
[63]:/images/prolog/list/qsort/qsort49.png
[64]:/images/prolog/list/qsort/qsort50.png
[65]:/images/prolog/list/qsort/qsort51.png
[66]:/images/prolog/list/qsort/qsort52.png
[67]:/images/prolog/list/qsort/qsort53.png
[68]:/images/prolog/list/qsort/qsort54.png
[69]:/images/prolog/list/qsort/qsort55.png
[70]:/images/prolog/list/qsort/qsort56.png
[71]:/images/prolog/list/qsort/qsort57.png
[72]:/images/prolog/list/qsort/qsort58.png
[73]:/images/prolog/list/qsort/qsort59.png
[74]:/images/prolog/list/qsort/qsort60.png
[75]:/images/prolog/list/qsort/qsort61.png
[76]:/images/prolog/list/qsort/qsort62.png
[77]:/images/prolog/list/qsort/qsort63.png
[78]:/images/prolog/list/qsort/qsort64.png
[79]:/images/prolog/list/qsort/qsort65.png
[80]:/images/prolog/list/qsort/qsort66.png
[81]:/images/prolog/list/qsort/qsort67.png
[82]:/images/prolog/list/qsort/qsort68.png
[83]:/images/prolog/list/qsort/qsort69.png
[84]:/images/prolog/list/qsort/qsort70.png
[85]:/images/prolog/list/qsort/qsort71.png
[86]:/images/prolog/list/qsort/qsort72.png
[87]:/images/prolog/list/qsort/qsort73.png
[88]:/images/prolog/list/qsort/qsort74.png
[89]:/images/prolog/list/qsort/qsort75.png
[90]:/images/prolog/list/qsort/qsort76.png
[91]:/images/prolog/list/qsort/qsort77.png
[92]:/images/prolog/list/qsort/qsort78.png
[93]:/images/prolog/list/qsort/qsort79.png
[94]:/images/prolog/list/qsort/final.png
[95]:/images/prolog/list/cross.png
[96]:/images/prolog/list/elementAt2/element1.png
[97]:/images/prolog/list/elementAt2/element2.png
[98]:/images/prolog/list/elementAt2/element3.png
[99]:/images/prolog/list/elementAt2/element4.png
[100]:/images/prolog/list/elementAt2/element5.png

[101]:/images/prolog/list/intersect/intersect1.png
[102]:/images/prolog/list/intersect/intersect2.png
[103]:/images/prolog/list/intersect/intersect3.png
[104]:/images/prolog/list/intersect/intersect4.png
[105]:/images/prolog/list/intersect/intersect5.png
[106]:/images/prolog/list/intersect/intersect6.png
[107]:/images/prolog/list/intersect/intersect7.png
[108]:/images/prolog/list/intersect/intersect8.png
[109]:/images/prolog/list/intersect/intersect9.png
[110]:/images/prolog/list/intersect/intersect10.png
[111]:/images/prolog/list/intersect/intersect11.png
[112]:/images/prolog/list/intersect/intersect12.png
[113]:/images/prolog/list/intersect/intersect13.png
[114]:/images/prolog/list/intersect/intersect14.png
[115]:/images/prolog/list/intersect/intersect15.png
[116]:/images/prolog/list/intersect/intersect16.png
[117]:/images/prolog/list/intersect/intersect17.png
[118]:/images/prolog/list/intersect/intersect18.png
