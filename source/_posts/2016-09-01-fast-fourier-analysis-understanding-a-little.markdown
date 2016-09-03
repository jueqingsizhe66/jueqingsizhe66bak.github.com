---
layout: post
title: "Fast Fourier Analysis Understanding a little"
date: 2016-09-01 14:15:57 +0800
comments: true
categories: prolog
---

假如有一个表达式`(a+b)*(a+b)`

我们可能这样计算

![dftlist][1]
`[n(1 ,a), n(2,b), n(3,op(+,1 ,2)), n(4,a),n(5,b), n(6, op(+,4,5)), n(7, op(*,3,6))]`

而我们也可以把共同的部分抽取出来，也就是把[DFT的计算结果][2](比如8-points dft就有8个结果)
抽取出相同的计算部分。
那我们可以得到如下的表达，

![fftlist][3]

`[n(12,a), ri(13(b), n(14,op(+,12,13)), n(15, op(*,14,14))]. `

为了看到这个过程，下头是一个来自[Clause and effects][4] 作者提供的node表示方法。
<!--more-->

为了看懂difference\_list可以采用[Logic for problem solving][5]提供的方法，也就是从正三角到graph解析sentence

正三角解析sentence图,

![trianglesenetence][6]

graph解析sentence图，

![graphsentence][7]

<div algin="left">
Dft执行结果:

```
[1] 64 ?- dft(p(8,0,W),X0).
X0 = a(0)+W^0*a(4)+W^0* (a(2)+W^0*a(6))+W^0* (a(1)+W^0*a(5)+W^0* (a(3)+W^0*a(7))) .

[1] 65 ?- dft(p(8,1,W),X1).
X1 = a(0)+W^4*a(4)+W^2* (a(2)+W^4*a(6))+W^1* (a(1)+W^4*a(5)+W^2* (a(3)+W^4*a(7))) .

[1] 66 ?- dft(p(8,2,W),X2).
X2 = a(0)+W^0*a(4)+W^4* (a(2)+W^0*a(6))+W^2* (a(1)+W^0*a(5)+W^4* (a(3)+W^0*a(7))) .

[1] 67 ?- dft(p(8,3,W),X3).
X3 = a(0)+W^4*a(4)+W^6* (a(2)+W^4*a(6))+W^3* (a(1)+W^4*a(5)+W^6* (a(3)+W^4*a(7))) .

[1] 68 ?- dft(p(8,4,W),X4).
X4 = a(0)+W^0*a(4)+W^0* (a(2)+W^0*a(6))+W^4* (a(1)+W^0*a(5)+W^0* (a(3)+W^0*a(7))) .

[1] 69 ?- dft(p(8,5,W),X5).
X5 = a(0)+W^4*a(4)+W^2* (a(2)+W^4*a(6))+W^5* (a(1)+W^4*a(5)+W^2* (a(3)+W^4*a(7))) .

[1] 70 ?- dft(p(8,6,W),X6).
X6 = a(0)+W^0*a(4)+W^4* (a(2)+W^0*a(6))+W^6* (a(1)+W^0*a(5)+W^4* (a(3)+W^0*a(7))) .

[1] 71 ?- dft(p(8,7,W),X7).
X7 = a(0)+W^4*a(4)+W^6* (a(2)+W^4*a(6))+W^7* (a(1)+W^4*a(5)+W^6* (a(3)+W^4*a(7))) .
```
</div>

<div algin="right">

FFT执行结果L:

```
[1] 74 ?- dft(p(8,0,W),X0),dft(p(8,1,W),X1),dft(p(8,2,W),X2),dft(p(8,3,W),X3),dft(p(8,4,W),X4),dft(p(8,5,W),X5),dft(p(8,6,W),X6),dft(p(8,7,W),X7),gen((X0;X1;X2;X3;X4;X5;X6;X7),[]-L,_),print(L).
X0 = a(0)+W^0*a(4)+W^0* (a(2)+W^0*a(6))+W^0* (a(1)+W^0*a(5)+W^0* (a(3)+W^0*a(7))),
X1 = a(0)+W^4*a(4)+W^2* (a(2)+W^4*a(6))+W^1* (a(1)+W^4*a(5)+W^2* (a(3)+W^4*a(7))),
X2 = a(0)+W^0*a(4)+W^4* (a(2)+W^0*a(6))+W^2* (a(1)+W^0*a(5)+W^4* (a(3)+W^0*a(7))),
X3 = a(0)+W^4*a(4)+W^6* (a(2)+W^4*a(6))+W^3* (a(1)+W^4*a(5)+W^6* (a(3)+W^4*a(7))),
X4 = a(0)+W^0*a(4)+W^0* (a(2)+W^0*a(6))+W^4* (a(1)+W^0*a(5)+W^0* (a(3)+W^0*a(7))),
X5 = a(0)+W^4*a(4)+W^2* (a(2)+W^4*a(6))+W^5* (a(1)+W^4*a(5)+W^2* (a(3)+W^4*a(7))),
X6 = a(0)+W^0*a(4)+W^4* (a(2)+W^0*a(6))+W^6* (a(1)+W^0*a(5)+W^4* (a(3)+W^0*a(7))),
X7 = a(0)+W^4*a(4)+W^6* (a(2)+W^4*a(6))+W^7* (a(1)+W^4*a(5)+W^6* (a(3)+W^4*a(7))),

L = [n(64, op(+ 49, 63)), n(63, op(*, 62, 52)), n(62, W^7), n(61, op(+ 42, 60)), n(60, op(*, 47, 44)), n(59, op(+ 31, 58)), n(58, op(*, 57, 38)), n(57, ... ^ ...), n(..., ...)|...] ,print(L).

[n(64,op(+ 49,63)),n(63,op(*,62,52)),n(62,_G3481^7),n(61,op(+ 42,60)),n(60,op(*,47,44)),n(59,op(+ 31,58)),n(58,op(*,57,38)),n(57,_G3481^5),n(56,op(+ 11,55)),n(55,op(*,24,21)),n(54,op(+ 49,53)),n(53,op(*,50,52)),n(52,op(+ 34,51)),n(51,op(*,47,36)),n(50,_G3481^3),n(49,op(+ 26,48)),n(48,op(*,47,29)),n(47,_G3481^6),n(46,op(+ 42,45)),n(45,op(*,27,44)),n(44,op(+ 15,43)),n(43,op(*,24,19)),n(42,op(+ 5,41)),n(41,op(*,24,9)),n(40,op(+ 31,39)),n(39,op(*,32,38)),n(38,op(+ 34,37)),n(37,op(*,27,36)),n(36,op(+ 16,35)),n(35,op(*,24,17)),n(34,op(+ 12,33)),n(33,op(*,24,13)),n(32,_G3481^1),n(31,op(+ 26,30)),n(30,op(*,27,29)),n(29,op(+ 6,28)),n(28,op(*,24,7)),n(27,_G3481^2),n(26,op(+ 1,25)),n(25,op(*,24,3)),n(24,_G3481^4),n(23,op(+ 11,22)),n(22,op(*,2,21)),n(21,op(+ 15,20)),n(20,op(*,2,19)),n(19,op(+ 16,18)),n(18,op(*,2,17)),n(17,a(7)),n(16,a(3)),n(15,op(+ 12,14)),n(14,op(*,2,13)),n(13,a(5)),n(12,a(1)),n(11,op(+ 5,10)),n(10,op(*,2,9)),n(9,op(+ 6,8)),n(8,op(*,2,7)),n(7,a(6)),n(6,a(2)),n(5,op(+ 1,4)),n(4,op(*,2,3)),n(3,a(4)),n(2,_G3481^0),n(1,a(0))]

```
</div>

FFT效果其实是下图的效果:

![butterfly][8]

FFT总共有64个计算，其中16个常量，48个加法和乘法，相对于之前DFT的112个加法和乘法,fft的运行效率肯定比较高.

fft显示代码(得配合[dft的结果][2]：

``` prolog
%% Note: only clauses for
%% add,mul,sequence and constant is given
%% you can add another clauses to generate
%% graph nodes for other operations.

% addition
gen(X+Y, L0-L3, A) :-
    !,
    gen(X, L0-L1, A1),
    gen(Y, L1-L2, A2),
    node(n(A, op(+ A1, A2)), L2-L3).

% multiple
gen(X*Y, L0-L3, A) :-
    !,
    gen(X, L0-L1, A1),
    gen(Y, L1-L2, A2),
    node(n(A, op(*, A1, A2)), L2- L3).

% sequence
gen((X;Y), L0-L2, _) :-
    !,
    gen(X, L0-L1, _),
    gen(Y, L1-L2, _).

% constant
    
gen(X, L0-L1, A) :-
    node(n(A, X), L0-L1).

%Node(n ,d)
%node(+TryNode, -OutDiffList)
% n is a computational node
% d front-back form difference_lsit
% three cases in the list d
%1.The first node is always in the list.
%2.Or, the node may not be added if it does the same thing as a node
%already in the list.
%3. Otherwise, add a new node with an incremented node number.

%Notice that not only does A1 name the new node
node(n(1, N), []-[n(1,N)]) :-
    !.
node(N, L-L) :-
    find(N, L),
    !.
node(n(A1, N1), [n(A, N)|T]-[n(A1,N1),n(A,N)|T]) :-
    A1 is A+1.


find(X, [X|_]) :- 
    !.
find(X,[_|T]) :-
    find(X, T).

% dft(p(8,0,W),X0), 
% dft(p(8,1,W),X1), 
% dft(p(8,2,W),X2), 
% dft(p(8,3,W),X3), 
% dft(p(8,4,W),X4), 
% dft(p(8,5,W),X5), 
% dft(p(8,6,W),X6), 
% dft(p(8,7,W),X7), 
%  dft(p(8,0,W),X0),dft(p(8,1,W),X1),dft(p(8,2,W),X2),dft(p(8,3,W),X3),dft(p(8,4,W),X4),dft(p(8,5,W),X5),dft(p(8,6,W),X6),dft(p(8,7,W),X7),gen((X0;X1;X2;X3;X4;X5;X6;X7),[]-L,_). 


```

有意思的是gen predicate配合上差分表(difference_list) 和accumulate两种风格。 还得对gen和node predicates做进一步研究。




[1]:/images/prolog/fft/dftlist.png
[2]:http://jueqingsizhe66.github.io/blog/2016/09/01/discrete-fourier-analysis-with-prolog-without-compute/ 
[3]:/images/prolog/fft/fftlist.png
[4]:http://www.springer.com/cn/book/9783540629719 
[5]:http://www.docin.com/p-286207487.html 
[6]:/images/prolog/fft/trianglesentence.png
[7]:/images/prolog/fft/graphsentence.png
[8]:/images/prolog/fft/butterfly.png
