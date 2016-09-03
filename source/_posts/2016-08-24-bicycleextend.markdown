---
layout: post
title: "bicycleExtend"
date: 2016-08-24 14:52:55 +0800
comments: true
categories: prolog
---
在[之前文章][40]已经写了关于自行车的组成部分，现在补充accumulator部分。

<!--more-->

## I.比较其他的实现，总结accumulaor规律

### 1.数组长度

```

%% 普通
list_lenno([], 0).
list_lenno([H|T], N) :-
    %   N is list_len1(T,N) + 1.
    list_lenno(T, N1 ),
    N is N1 + 1.

%% 尾递归

% get the length of a list
list_len(L,N) :-
    lenacc(L,[],N).

lenacc([], A, A).
lenacc([H|T], A, N) :-
    A1 is A + 1,
    lenacc(T, A1, N).


```


### 2.斐波那契数列（Fibonacci sequence） 


```


%% 普通版本
fibTail1(0, 1).
fibTail1(1, 1).
fibTail1(N, F) :- 
    N > 1,  
    Temp1 is N - 1, 
    Temp2 is N - 2,
    fibTail1(Temp1,F1),
    fibTail1(Temp2,F2),
    F is F1 + F2.



%% 尾递归 主要是NF1的作用
fibTail(0, 1).
fibTail(1, 1).
fibTail(N, F) :- 
    N > 1,  fibTail(N, 1, 1, F).


fibTail(2, F1, F2, F) :- 
    F is F1 + F2.
fibTail(N, F1, F2, F) :- 
    N > 2,  N1 is N - 1,  NF1 is F1 + F2, 
    fibTail(N1, NF1, F1, F).


```

### 3.n的阶乘

``` prolog
%% 普通

facTail1(0, 1).
facTail1(N, F) :-
    N1 is N - 1,
    facTail1(N1, F1),
    F is N*F1.


%% 尾递归
facTail(0, 1).
facTail(N, F) :- 
    N > 0,  facTail(N, 1, F).

facTail(1, F, F).
facTail(N, PP, F) :- 
    N > 1,  
    NPp is N*PP,  
    M is N-1,  facTail(M, NPp, F). 


```

### 4. 自行车部分

可以参考 [博文][40].

``` prolog


%% 普通
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


%% 尾递归
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

总结， 尾递归一般是在形式上的最后一个参数加上一个A(accumulator),然后去除中间过程append或者乘法或者加法，
把这个计算过程中放在了参数的计算并更新的过程(建了一个寄存器的效果，先把结果缓存下来(一般都是从1或者空集开始)).
也就是说。<font color="green"><strong>尾递归实现一定是得有一个wrapper(包裹),进行尾递归的初始化。</strong></font>

回顾递归的两个条件，

1. 边界条件(boundary condition)或者终止条件(terminal condition)
2. natural recursion(递归减小的过程 )或者recursion cases(比如sub 到0 或者`[_|R]`到空集的过程)

## II. improve version加上accumulator

计算结果,
``` prolog

29 ?- listing(partlistacc).
bicycleExtendAcc:partlistacc(A) :-
        partsof(1, A, [], B),
        collect(B, C),
        printpartlist(C).

true.
[debug] 25 ?- partlistacc(bike1).
1       handle
1       fork
12      nut
12      bolt
2       gear
2       rim
2       spoke
true .


26 ?- partlistacc(wheel1).
6       nut
6       bolt
1       gear
1       rim
1       spoke
true .

27 ?- partlistacc(hub1).
6       nut
6       bolt
1       gear
true .

28 ?- partlistacc(axle1).
6       nut
6       bolt
true .
```


below is the update code,

**change Place**

1. partsof(1, T,  P), 上身为一个wrapper包裹器. 
2. partsacc add an accumulator argument, the initial value is []
3. partsacc(N, X, A, [quant(X,N)|A]) 的边界条件发生改变，得保存A的值，构成累加器（核心步骤)
4. partsacc(N, X, A, P) 同时，partsacc的递归cases增加一个A，供给partsacclist使用
5. partsacclist的边界条件改变,`partsacclist(_, [],A, A).` 增加了累加器
6. 同理，partsacclist的natural recursion也发生改变,增加一个参数(累加器) `partsacclist(N,[quant(X,Num)|L], A, T) `
并把partsacc的计算结果，传递给下一步的partsacclist,同时删掉之前的append predicate，构成真正意义上的尾递归.(<font color="red">比较容易出错的地方</font>)


Noted: 在这个修改的过程中，思维中(也可以当做直觉)得有

+ 最小的单位是什么?
+ 每一个predicate的调用都会产生stack或者一个box的过程(不是特别重要)。
+ 一个predicate的参数排布都是从input到output的过程中，所以过程中体现着不断达到目标的过程(重要).


``` prolog
%%:- ensure_loaded([bicycle]).
:- module(bicycleExtendAcc, [partlistacc/1]).


basicpart(rim).
basicpart(spoke).
%basicpart(hub).
basicpart(handle).
basicpart(rear_frame).
basicpart(gear).
basicpart(bolt).
basicpart(nut).
basicpart(fork).


assemble(bike1, [quant(wheel1,2),quant(frame1,1)]).
%% maybe a quantitty of bolts and nuts.
assemble(wheel1, [quant(spoke,1),quant(rim,1),quant(hub1,1)]).
assemble(frame1, [quant(rear_frame1,1),quant(front_frame1,1)]).
assemble(front_frame1, [quant(fork,1),quant(handle,1)]).
assemble(rear_frame1,[]).
assemble(hub1, [quant(gear,1), quant(axle1,1)]).
assemble(axle1, [quant(bolt,6), quant(nut,6)]).


partlistacc(T) :-
    partsof(1, T, P),
    collect(P, Q),
    printpartlist(Q).


partsof(N, X, P) :-
    partsacc(N, X, [], P).

partsacc(N, X, A, P) :-
    assemble(X, Subparts),
    partsacclist(N, Subparts, A, P).

%%change place
partsacc(N, X, A, [quant(X,N)|A]) :-
    basicpart(X).

partsacclist(_, [],A, A).
partsacclist(N,[quant(X,Num)|L], A, T) :-
    M is N* Num,
    partsacc(M, X, A, Xparts),
    partsacclist(N, L, Xparts, T).


%partsoflist(N, L, A, Restparts),
%append(Xparts, Restparts,  T).


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

## III. update version加上accumulator

测试结果：

``` prolog
32 ?- listing(partlist2acc).
bicycleEntend2Acc:partlist2acc(A) :-
        partsof_wrapper(1, A, B),
        collect(B, C),
        printpartlist(C).

true.
33 ?- partlist2acc(axle2).
6       nut
6       bolt
true .

37 ?- partlist2acc(hub2).
12      nut
12      bolt
2       gear
true .

35 ?- partlist2acc(wheel2).
24      nut
24      bolt
4       gear
2       rim
4       spoke
true .

36 ?- partlist2acc(bike2).
4       handle
2       fork
48      nut
48      bolt
8       gear
4       rim
8       spoke
true .
```

``` prolog
%%:- ensure_loaded([bicycle]).

:- module(bicycleEntend2Acc, [partlist2acc/1]).


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



partlist2acc(T) :-
    partsof_wrapper(1,T,P),
    collect(P,Q),
    printpartlist(Q).

partsof_wrapper(N, X, P) :-
    partsof(N, X, [], P).

partsof(N, X, A, P) :-
    assemble(X, Subparts),
    partsoflist(N, Subparts, A, P).

partsof(N, X, A, [X:N|A]) :-
    basicpart(X).

partsoflist(_, [], A, A).
partsoflist(N, [X:Num|L], A, T) :-
    M is N* Num,
    partsof(M, X, A, Xparts),
    partsoflist(N, L, Xparts, T).


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


## IV. 有趣的collect函数

![collect][41]
collect函数有意思的是

`collectrest(X, N, [quant(X, Num)|Rest], Others, Ntotal)`, 这是一个匹配的过程，
也就是说如果当前和后面的值的X匹配上，则要进行加法运算，否则不用。本质来算collect的
过程就是匹配累加计数的过程。

下面是针对`partlistacc(bike1)`的两步调试过程，

![debug][1]
<hr/>
<br/>
![debug][2]
<hr/>
<br/>
![debug][3]
<hr/>
<br/>
![debug][4]
<hr/>
<br/>
![debug][5]
<hr/>
<br/>
![debug][6]
<hr/>
<br/>
![debug][7]
<hr/>
<br/>
![debug][8]
<hr/>
<br/>
![debug][9]
<hr/>
<br/>
![debug][10]
<hr/>
<br/>
![debug][11]
<hr/>
<br/>
![debug][12]
<hr/>
<br/>
![debug][13]
<hr/>
<br/>
![debug][14]
<hr/>
<br/>
![debug][15]
<hr/>
<br/>
![debug][16]
<hr/>
<br/>
![debug][17]
<hr/>
<br/>
![debug][18]
<hr/>
<br/>
![debug][19]
<hr/>
<br/>
![debug][20]
<hr/>
<br/>
![debug][21]
<hr/>
<br/>
![debug][22]
<hr/>
<br/>
![debug][23]
<hr/>
<br/>
![debug][24]
<hr/>
<br/>
![debug][25]
<hr/>
<br/>
![debug][26]
<hr/>
<br/>
![debug][27]
<hr/>
<br/>
![debug][28]
<hr/>
<br/>
![debug][29]
<hr/>
<br/>
![debug][30]
<hr/>
<br/>
![debug][31]
<hr/>
<br/>
![debug][32]
<hr/>
<br/>
![debug][33]
<hr/>
<br/>
![debug][34]
<hr/>
<br/>
![debug][35]
<hr/>
<br/>
![debug][36]
<hr/>
<br/>
![debug][37]
<hr/>
<br/>
![debug][38]
<hr/>
<br/>
![debug][39]
<hr/>
<br/>


<div align="center"><font color="red">Noted, you can add the module call in the one file,just like below,</font></div>

``` prolog
:-[bicycle].
:- use_module(bicycleExtend, [partlist/1]).
:- use_module(bicycleExtendAcc, [partlistacc/1]).
:- use_module(bicycleEntend2, [partlist2/1]).

```




[1]:/images/prolog/bicycle2/debug1.png
[2]:/images/prolog/bicycle2/debug2.png
[3]:/images/prolog/bicycle2/debug3.png
[4]:/images/prolog/bicycle2/debug4.png
[5]:/images/prolog/bicycle2/debug5.png
[6]:/images/prolog/bicycle2/debug6.png
[7]:/images/prolog/bicycle2/debug7.png
[8]:/images/prolog/bicycle2/debug8.png
[9]:/images/prolog/bicycle2/debug9.png
[10]:/images/prolog/bicycle2/debug10.png
[11]:/images/prolog/bicycle2/debug11.png
[12]:/images/prolog/bicycle2/debug12.png
[13]:/images/prolog/bicycle2/debug13.png
[14]:/images/prolog/bicycle2/debug14.png
[15]:/images/prolog/bicycle2/debug15.png
[16]:/images/prolog/bicycle2/debug16.png
[17]:/images/prolog/bicycle2/debug17.png
[18]:/images/prolog/bicycle2/debug18.png
[19]:/images/prolog/bicycle2/debug19.png
[20]:/images/prolog/bicycle2/debug20.png
[21]:/images/prolog/bicycle2/debug21.png
[22]:/images/prolog/bicycle2/debug22.png
[23]:/images/prolog/bicycle2/debug23.png
[24]:/images/prolog/bicycle2/debug24.png
[25]:/images/prolog/bicycle2/debug25.png
[26]:/images/prolog/bicycle2/debug26.png
[27]:/images/prolog/bicycle2/debug27.png
[28]:/images/prolog/bicycle2/debug28.png
[29]:/images/prolog/bicycle2/debug29.png
[30]:/images/prolog/bicycle2/debug30.png
[31]:/images/prolog/bicycle2/debug31.png
[32]:/images/prolog/bicycle2/debug32.png
[33]:/images/prolog/bicycle2/debug33.png
[34]:/images/prolog/bicycle2/debug34.png
[35]:/images/prolog/bicycle2/debug35.png
[36]:/images/prolog/bicycle2/debug36.png
[37]:/images/prolog/bicycle2/debug37.png
[38]:/images/prolog/bicycle2/debug38.png
[39]:/images/prolog/bicycle2/debug39.png
[40]:http://jueqingsizhe66.github.io/blog/2016/08/24/what-consist-of-the-bicycle/ 
[41]: /images/prolog/bicycle2/collect.png 

