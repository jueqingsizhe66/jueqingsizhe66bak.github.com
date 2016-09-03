---
layout: post
title: "bicycle components by difference_list and accumulator"
date: 2016-08-24 22:31:24 +0800
comments: true
categories: prolog
---

包括我在内，也并未注意，之前博文导出的结果是和原始列表逆序的(reverse order).

+ [what-consist-of-the-bicycle][66]
+ [bicycleExtend][67]


<!--more-->

### difference list

``` prolog
:- module(bicycleDifference,[partsofdiff/2]).

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


%partsofdiff(X, P) :- 
%    partsacc(X, P, Hole), Hole = [].
partsofdiff(X, P) :-
    partsacc(X, P, []).

partsacc(X, [X|Hole], Hole) :- 
    basicpart(X).

partsacc(X, P, Hole) :-
    assemble(X, Subparts),
    partsoflist(Subparts, P, Hole).

partsoflist([], Hole, Hole).
partsoflist([P|Tail], Total, Hole) :-
    partsacc(P, Total, Hole1),
    partsoflist(Tail,Hole1, Hole).




```



### accumulator

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



% get the length of a list
list_len(L,N) :-
    lenacc(L,[],N).

lenacc([], A, A).
lenacc([H|T], A, N) :-
    A1 is A + 1,
    lenacc(T, A1, N).


list_lenno([], 0).
list_lenno([H|T], N) :-
    %   N is list_len1(T,N) + 1.
    list_lenno(T, N1 ),
    N is N1 + 1.


```

### what's the difference between difference list and accumulator?

####相同点

1.  基础条件一样，零部件都一样，可以说函数的定义特别想象。

####不同点


1. 结果不一致(该结果都是未显示完整的（默认最多显示9个，其实有19个部件),但是可以看出来
difference_list的确是正序排列，而accumulator是逆序排列。
``` prolog



[debug] 17 ?- |    partsofdiff(axle,Y).                                <=======difference_list========>

Y = [bolt, nut] .


[debug] 18 ?- |    partsof1(axle,Y).                                   <=======accumulator========>
Y = [nut, bolt] .


[debug] 19 ?- partsof1(bike,Y).                                        <=======accumulator========>
Y = [handle, fork, rear_frame, nut, bolt, gear, rim, spoke, nut|...] .



[debug] 20 ?- partsofdiff(bike,Y).                                     <=======difference_list========>
Y = [spoke, rim, gear, bolt, nut, spoke, rim, gear, bolt|...] .
```

2. 具体不同
```
   difference_list                           accumulator

partsofdiff(X, P):- partsacc(X, P, [])      partsof1(X, P) :- partsacc(X,[],P).
partsacc(X, [X|Hole], Hole)                 partsacc(X, A, [X|A])
partsacc(X, P, Hole)                        partsacc(X, A, P)
partsacclist(Subparts, P, Hole)             partsacclist(Subparts, A, P)
partsoflist([], Hole, Hole)                 partsoflist([], A, A)
partsoflist([P|Tail], Total, Hole)          partsoflist([P|Tail], A, Total)
partsacc(P, Total, Hole1)                   partsacc(P, A, Hp)                <----------主要不同点
partsoflist(Tail, Hole1, Hole)              partsoflist(Tail, Hp, Total)
```

可以看出主要不同点是Hole和A(Accumulator) 在不同的位置而已，基本上实现的功能是一样的，但是
由于difference_list(头递归)先处理头部，再处理尾部, 而Tail Recursion则是先处理尾部(尾部递归),
但是为什么得到的结果一样？ 关键不在于`partsacc(X, [X|Hole],Hole)`, 而在于`partsacc(P, Total, Hole1) and partsacc(P, A, Hp)`

且看我们的调试结果，

3. 调试结果

<hr/>
<br/>

The debug result by the difference list:

![png][1]
![png][2]
![png][3]
![png][4]
![png][5]
![png][6]
![png][7]
![png][8]
![png][9]
![png][10]
![png][11]
![png][12]
![png][13]
![png][14]
![png][15]
![png][16]
![png][17]
![png][18]
![png][19]
![png][20]
![png][21]
![png][22]
![png][23]
![png][24]
![png][25]
![png][26]
![png][27]
![png][28]
![png][29]
![png][30]
![png][31]
![png][32]
![png][33]

<hr/>
<br/>
The debug result by accumulator :

![png][34]
![png][35]
![png][36]
![png][37]
![png][38]
![png][39]
![png][40]
![png][41]
![png][42]
![png][43]
![png][44]
![png][45]
![png][46]
![png][47]
![png][48]
![png][49]
![png][50]
![png][51]
![png][52]
![png][53]
![png][54]
![png][55]
![png][56]
![png][57]
![png][58]
![png][59]
![png][60]
![png][61]
![png][62]
![png][63]
![png][64]
![png][65]

<hr/>
<br/>

之所以废了这么多篇幅调试difference_list, 是因为该技术可以用于自然语言的解析当中,可以参考[A Little Funny About the Sentence Parsing][68] 。

[1]:/images/prolog/bicycle3/diff1.png
[2]:/images/prolog/bicycle3/diff2.png
[3]:/images/prolog/bicycle3/diff3.png
[4]:/images/prolog/bicycle3/diff4.png
[5]:/images/prolog/bicycle3/diff5.png
[6]:/images/prolog/bicycle3/diff6.png
[7]:/images/prolog/bicycle3/diff7.png
[8]:/images/prolog/bicycle3/diff8.png
[9]:/images/prolog/bicycle3/diff9.png
[10]:/images/prolog/bicycle3/diff10.png
[11]:/images/prolog/bicycle3/diff11.png
[12]:/images/prolog/bicycle3/diff12.png
[13]:/images/prolog/bicycle3/diff13.png
[14]:/images/prolog/bicycle3/diff14.png
[15]:/images/prolog/bicycle3/diff15.png
[16]:/images/prolog/bicycle3/diff16.png
[17]:/images/prolog/bicycle3/diff17.png
[18]:/images/prolog/bicycle3/diff18.png
[19]:/images/prolog/bicycle3/diff19.png
[20]:/images/prolog/bicycle3/diff20.png
[21]:/images/prolog/bicycle3/diff21.png
[22]:/images/prolog/bicycle3/diff22.png
[23]:/images/prolog/bicycle3/diff23.png
[24]:/images/prolog/bicycle3/diff24.png
[25]:/images/prolog/bicycle3/diff25.png
[26]:/images/prolog/bicycle3/diff26.png
[27]:/images/prolog/bicycle3/diff27.png
[28]:/images/prolog/bicycle3/diff28.png
[29]:/images/prolog/bicycle3/diff29.png
[30]:/images/prolog/bicycle3/diff30.png
[31]:/images/prolog/bicycle3/diff31.png
[32]:/images/prolog/bicycle3/diff32.png
[33]:/images/prolog/bicycle3/diff33.png


[34]:/images/prolog/bicycle3/accu1.png
[35]:/images/prolog/bicycle3/accu2.png
[36]:/images/prolog/bicycle3/accu3.png
[37]:/images/prolog/bicycle3/accu4.png
[38]:/images/prolog/bicycle3/accu5.png
[39]:/images/prolog/bicycle3/accu6.png
[40]:/images/prolog/bicycle3/accu7.png
[41]:/images/prolog/bicycle3/accu8.png
[42]:/images/prolog/bicycle3/accu9.png
[43]:/images/prolog/bicycle3/accu10.png
[44]:/images/prolog/bicycle3/accu11.png
[45]:/images/prolog/bicycle3/accu12.png
[46]:/images/prolog/bicycle3/accu13.png
[47]:/images/prolog/bicycle3/accu14.png
[48]:/images/prolog/bicycle3/accu15.png
[49]:/images/prolog/bicycle3/accu16.png
[50]:/images/prolog/bicycle3/accu17.png
[51]:/images/prolog/bicycle3/accu18.png
[52]:/images/prolog/bicycle3/accu19.png
[53]:/images/prolog/bicycle3/accu20.png
[54]:/images/prolog/bicycle3/accu21.png
[55]:/images/prolog/bicycle3/accu22.png
[56]:/images/prolog/bicycle3/accu23.png
[57]:/images/prolog/bicycle3/accu24.png
[58]:/images/prolog/bicycle3/accu25.png
[59]:/images/prolog/bicycle3/accu26.png
[60]:/images/prolog/bicycle3/accu27.png
[61]:/images/prolog/bicycle3/accu28.png
[62]:/images/prolog/bicycle3/accu29.png
[63]:/images/prolog/bicycle3/accu30.png
[64]:/images/prolog/bicycle3/accu31.png
[65]:/images/prolog/bicycle3/accu32.png
[66]:http://jueqingsizhe66.github.io/blog/2016/08/24/what-consist-of-the-bicycle/ 
[67]:http://jueqingsizhe66.github.io/blog/2016/08/24/bicycleextend/ 
[68]:http://jueqingsizhe66.github.io/blog/2016/08/02/a-little-funny-about-the-sentence-parsing/ 
