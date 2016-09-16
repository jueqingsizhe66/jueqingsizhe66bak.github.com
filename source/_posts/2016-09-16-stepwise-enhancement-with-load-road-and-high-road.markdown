---
layout: post
title: "Stepwise enhancement with load-road and high-road"
date: 2016-09-16 20:50:26 +0800
comments: true
categories: prolog
---
Good Programmer with experience should identify and clarify the pattern of the procedure(or called Skeleton which means the 
control flow of the procedure), 
then programmer can enhance the skeleton by applying a technique(which means the implement of the feature branch). 

All the programmer can be designed with[ top-down method][12],and then with the goal driven , we can achieve 
every goal by the bottom-up method. We have talked about the [Sequence Understanding in Prolog][13], which we 
can get the idea that procedure can be split into two parts control part(dataStructure) and logic part.

Here below are two roads under the developement of the prolog programe design(正如所有的prolog计算过程包含两部folding and unfolding ,unfolding的目的是找到definition of
the predicate, folding的目的是找到对应的predicate的某一个goal):

1. low road(Stepwise enhancement with skeleton and techniques)
2. high road(High-order function)
![hgi2][19]

<!--more-->


<h2 id="low-road">第一种程序设计方法:Stepwise enhancement</h2>

The method of stepwise enhancement (Lakhotia, 1989) was originally conceived as an
adaptation of stepwise refinement to Prolog. It was advocated as a way to systematically
construct Prolog programs which exploiting Prolog's high-level features. The key idea
underlying stepwise enhancement is to visualize a program or solution in terms of its
central control flow, or skeleton, and techniques which perform computations while the
control flow of the skeleton is followed. Techniques can be developed independently and
combined automatically using the method of composition.(涉及到三个关键字:skeleton ,techiniques,
composition).
而我注意的是techniques是skeleton的<font color="red">extra or another</font> arguments
(or goals(computation procedure) right hand side).

也就是说stepwise programming development 一般包含三个步骤，

1. Identify the skeleton structure constituting the program
2. Create  the technique branch using the standard prolog programming techiniques(such as accumulating ,different_list etc)
3. Compose the separate techiques branches into one main stream to give the final elegant program.

For example, the most common data structure for logic programs is the list, and many programs are
based on skeletons for traversing lists,such as

<hr/>
<hr/>
<br/>
<h3 id="skeleton"> Procedure1: the skeleton of traversing the lists:</h3>

``` prolog

is_list([]).
is_list([X|Xs]) :- 
    is_list(Xs).
```

<h3 id="length"> Procedure1.2: the techinique length for the skeleton</h3>

``` prolog
length([], 0).
length([X|Xs], N) :-
    length(Xs, N1),
    N1 is N + 1.
```
<h3 id="sum"> Procedure1.3: the techinique sum for the skeleton</h3>

``` prolog
sum([], 0).
sum([X|Xs], Sum):-
    sum(Xs, Sum1),
    Sum1 is Sum + X.

```

<h3 id="sum_length"> Procedure1.4: the techinique sum_length for the skeleton</h3>
``` prolog
sum_length([], 0, 0).
sum_length([X|Xs], Sum, N):-
    sum_length(Xs, Sum1, N1),
    Sum1 is Sum + X,
    N1 is N + 1.

```


一般来说，techiniques捕捉Prolog的编程方法，比如

1. build.Building  a data structure(build case) (in the left hand side)
2. calculate.performming calculations(calcuate case) in recursive codes(in the right hand side)
3. composition.也可以结合,比如accumulate method, different_list method

1和2是使用最频繁的prolog techiniques。一般在calculate阶段，也就是在程序的body中增加extra goals(比如算术表达式), 而在
build阶段，也就是在程序的head中增加extra arguments，builds the data structure.
Informally, a programming technique interleaves some additional(额外的 extra, another) computation around the control flow of a skeleton program. The additional
computation might calculate a value or produce a side effect such as screen output.
Syntactically, techniques may rename predicates, add arguments to predicates, add goals to
clauses, and/or add clauses to programs(增加predicate参数，增加clauses的目标). Unlike skeletons, techniques are not programs but
can be conceived as a family of operations that can be applied to a program to produce a program.

A **technique** applied to a skeleton yields an **enhancement**. An **enhancement** which preserves
the computational behavior of the skeleton is called an **extension**(enhancement和extension的定义类似)

下面两个例子member and tree从两个不同的skeletons进一步揭示了skeleton and techinique在软件设计过程中的不可替代的作用: 

1. [the skeleton of testing member](#skeletonMember)
2. [the skeleton for the tree definition](#tree)



<hr/>
<hr/>
<br/>


<h3 id="skeletonMember">Procedure2: the skeleton for testing member </h3>
``` prolog

skel([], Ys).
skel([X|Xs], Ys) :-
    member(X, Ys),
    skel(Xs, Ys).
skel([X|Xs], Ys) :-
    \+ member(X, Ys),
    skel(Xs, Ys).


```


### Procedure2.1: the techinique  union for [the skeleton](#skeletonMember)

union(Xs, Ys, Us) , union is  the union of the elements in  Xs and Ys.

``` prolog

union([], Ys, Ys).
union([X|Xs], Ys, Us) :-
    member(X, Ys),
    union(Xs, Ys, Us).
union([X|Xs], Ys, [X|Us]) :-
    \+ member(X, Ys),
    union(Xs, Ys, Us).


```

注意第三个Union.

### Procedure2.2: the techinique intersection for [the skeleton](#skeletonMember)

intersection(Xs, Ys, Is) , intersection is  the intersection of the elements in  Xs and Ys.

``` prolog

intersection([], Ys, Ys).
intersection([X|Xs], Ys, [X|Is]) :-
    member(X, Ys),
    intersection(Xs, Ys, Is).
intersection([X|Xs], Ys, Is) :-
    \+ member(X, Ys),
    intersection(Xs, Ys, Is).


```

注意第二个intersection.



### Procedure2.3: the techinique union and intersection for [the skeleton](#skeletonMember)

union_intersect(Xs, Ys, Is) , union_intersect is  the union and intersect ,respectively, of the elements in  Xs and Ys.

``` prolog

union_intersect([], Ys, Ys, []).
union_intersect([X|Xs], Ys, Us, [X|Is]) :-
    member(X, Ys),
    union_intersect(Xs, Ys, Us, Is).
union_intersect([X|Xs], Ys, [X|Us], Is) :-
    \+ member(X, Ys),
    union_intersect(Xs, Ys, Us, Is).



```

<hr/>
<hr/>
<br/>

[skeleton轮廓和techniques技术](#low-road)可以定义软件设计过程中的重复的单元(增加软件的可重用行reusability和可维护性maintainability)，只不过更多的是从时间的单维角度出发(类似[git branches][18])。而[高阶函数(H.O. Fn)](#High-order)方法更多的是从
空间(高度或者深度)的抽象角度出发，进一步提取其中重复的单元。

为了更进一步解释skeleton和techniques 引入了树结构的skeleton定义

<h3 id="tree">3. the skeleton for the tree definition </h3>

``` prolog

is_tree(leaf(X)).
is_tree(tree(L, R)):-
    is_tree(L),
    is_tree(R).

branch(leaf(X)).
branch(tree(L, R)) :-
    branch(L).

branch(tree(L, R)) :-
branch(R).

   
```

其中is_tree 定义了遍历整棵树的skeleton(也可以看做树的分支的类型定义），而branch仅仅是针对于一个分支的predicate.

### 3.1 The technique prod and sum for the skeleton [tree](#tree)

此处is_tree分别被重命名为prod_leaves 和sum_leaves.

``` prolog
prod_leaves(leaf(X), X).
prod_leaves(tree(L, R), Prod):-
    prod_leaves(L, LProd),
    prod_leaves(R, RProd),
    Prod is LProd * RProd.

sum_leaves(leaf(X), X).
sum_leaves(tree(L, R), Sum):-
    sum_leaves(L, LSum),
    sum_leaves(R, RSum),
    Sum is LSum * RSum.

```

<hr/>
<hr/>
<br/>
### 3.2 The technique composition of prod and sum for the skeleton [tree](#tree)

 The (syntactic) operation for combining enhancements is called **composition**.

``` prolog
prod_sum_leaves(leaf(X), X, X).
prod_sum_leaves(tree(L, R), Prod, Sum):-
    prod_sum_leaves(L, Lprod, Lsum),
    prod_sum_leaves(R, Rprod, Rsum),
    Prod is Lprod * Rprod,
    Sum  is Lsum * Rsum.


```

<hr/>
<hr/>
<br/>

<h3 id="accumulate-calculate">3.3 The technique accumulate calculate for the skeleton tree</h3>
A different programming technique uses accumulators(累加, 其实就是尾递归). The accumulator-calculate (累加计算)
technique adds two arguments to the defining predicate in the skeleton to allow for global variables in
a program. The first argument is used to record the current value of the variable in question
and the second contains the final result of the computation. The base case relates the input
and output arguments, often via unification. One difference between calculate and accumulate-calculate
is in the need to add an auxiliary predicate(一般都需要多一个辅助函数或者叫做帮助函数). Another is that goals and initial values need to be placed differently.

``` prolog
sum_leaves(Tree, Sum) :-
    accum_sum_trees(Tree, 0, Sum).

%% 辅助函数
accum_sum_trees(leaf(X), accum, Sum):-
    Sum is accum + X.
accum_sum_trees(tree(L,R), Accum, Sum) :-
    accum_sum_trees(L, Accum, Accum1),
    accum_sum_trees(R, Accum1, Sum).

```

<hr/>
<hr/>
<br/>

<h3 id="accumulate-build"> 3.4 The technique accumulate build for the skeleton tree</h3>

accumulate-build和 [accumulate-calculate](#accumulate-calculate) 基本上一样，只不过在初始化和最后一步计算存在不一样。

比较
```
----------+-------------------------------------------+------------------------------------------------+
          |    accumulate-build                       |       accumulate-calculate                     |
-------—--+-------------------------------------------+------------------------------------------------+
初始化     |   accum_sum_trees(Tree, 0, Sum)            |      accum_sum_trees(Tree, [], Sum)            |
----------+-------------------------------------------+------------------------------------------------+
最后一步   |  accum_sum_trees(leaf(X), accum, Sum)     |     accum_sum_trees(leaf(X), accum, [X|Sum]).  |
          |     :- Sum is accum + X.                  |                                                |
----------+-------------------------------------------+------------------------------------------------+


```

注意和[accumulate-calculate](#accumulate-calculate)不同的地方.
``` prolog
sum_leaves(Tree, Sum) :-
    accum_sum_trees(Tree, 0, Sum).

%% 辅助函数
accum_sum_trees(leaf(X), accum, [X|Sum]).
accum_sum_trees(tree(L,R), Accum, Sum) :-
    accum_sum_trees(L, Accum, Accum1),
    accum_sum_trees(R, Accum1, Sum).

```
<hr/>
<hr/>
<br/>


<h2 id="High-order">One new software development method: H.O. Fn(High-order function)</h2>

![high-order][20]
sum,product,length actually have the similar skeleton(no!! here foldr is called high order function)), right below:

[一般foldr使用depth-first，而foldl使用breadth-first(比较喜欢用 tail recursion)][1]

``` prolog
foldr(F, B, [], B).
foldr(F, B, [A|As], R) :-
    foldr(F, B, As, R1),
    call(F, A, R1, R).

sum(As, S) :- foldr(plus, 0, As, S).
product(As, S) :- foldr(times, 0, As, P).
length(As, S) :- foldr(add1, 0, As, L).
add1(_,TailLen, Len) :- Len is TailLen + 1.
```


``` prolog

1 ?- listing(foldr).
foldr(_, A, [], A).
foldr(A, B, [D|C], F) :-
        foldr(A, B, C, E),
        call(A, D, E, F).

true.
```

关键点:

1. with high-order predicate(such as foldr) traverse certain data structure(such as list , or map (key-value)).
2. call.

也就是说foldr将产生计算需要的instance,该实例作用于列表当中。

补充foldl,注意隐式（DCG写法)和显式(accumulator)两个版本。

``` prolog

%% explicit accumulator version
foldl(FC, FB, [], A0, A):-
    call(FB, A0, A).
foldl(FC, FB, [X|Xs], A0, A) :-
    call(FC, X, A0, A1),
    foldr(FC, FB, Xs, A1, A).

%% DCG(implicit accumulator) version
foldl1(FC, FB, []) -->
    call(FB).
foldl1(FC, FB, [X|Xs] -->
    call(FC, X),
    foldl1(FC, FB, Xs).

```

进一步地，可以参看[Naish: high-order function development for program][15], 另外Dam Friedman在[the little scheme][21]中写得"[insert-g][22]"就是高阶函数的体现。

## 结论

通过low-road(apply one techinique into skeleton)的skeleton和techniques，可以理解high-road(apply many techiniques into skeleton)的设计思想，结合accumulate or different_list技术(比如foldl)可以达到更加理想的算法速度,并且系统化(system)程序设计,high-road是对于
low-road的泛化(low-road是基础研究过程),足够多的low-road才能达到high-road。

## Reference

1. [Stepwise Enhancement and High order Programming in Prolog][1] <font color="red">very important</font> 类似于naish的工作
2. [Skeletons and Techniques as a Normative Approach to Program Development in Logic-based Languages ][2] <font color="red"><--important--></font>
3. [relative correctness of prolog programs ][3]
4. [All Publications By Leon Sterling][4]
5. [Applying Techniques to Skeletons][5]<font color="red"><--important-></font>
6. [Composition Based on Skeletons and Techniques ][6]<font color="red"><--important--></font>
7. [Constructing Provably Correct Logic Programs ][7]
8. [The art of prolog][8]
9. [ Characterizing Similar Computational Behavior of Logic Programs][9]
10. [EOPL的不同解释器interpreter: skeletons and techniques(such as let letrec..)][10]
11. [The little scheme的member insert rember subst or can be substituted with insert-g ][11]
12. [Patterns for prolog Programming][14]
13. [Naish: a high-order function programming(high road) 区别于skeleton and techiniques(the low road)][15]
14. [A tool to support step-wise enhancement in prolog][16] -->未下载
15. [Developing Software Testing Programs in Prolog Using Stepwise Enhancement ][17] -->未下载
16. [A Higher Order Reconstruction of Stepwise Enhancement by naish ][15]<font color="red">very important</font>


## 新词汇

1. stepwise
2. [skeleton](#skeleton)(the main stream of the program design, delete the extra argument in the head or additional goals in the body)
3. [techniques](#length)(one special feature branch in the main stream)
4. [composition](#sum_length)(combine the different techiniques into skeleton)
4. [low-road](#low-road)(skeletons , techiniques ,and composition)
5. [high-road](#High-order)(H.O.Fn)
6. high-order function(H.O. Fn)
7. Enhancement(skeleton+techiniques 或者H.o.Fn)
8. calculate(one calculate procedure,such as sum and production)
9. build(build a data structure)
10. [accumulate-calculate](#accumulate-calculate)(accumulate and calculate)
11. [accumulate-build](#accumulate-build)(accumulator and build)

[1]: http://www.docin.com/p-951686297-f3.html
[2]:https://www.researchgate.net/publication/2626929_Skeletons_and_Techniques_as_a_Normative_Approach_to_Program_Development_in_Logic-based_Languages 
[3]:http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.49.7645&rep=rep1&type=pdf 
[4]:https://www.researchgate.net/profile/Leon_Sterling/publications 
[5]:https://www.researchgate.net/publication/220986744_Applying_Techniques_to_Skeletons?ev=prf_pub 
[6]: https://www.researchgate.net/publication/2746164_Composition_Based_on_Skeletons_and_Techniques
[7]:https://www.researchgate.net/publication/2714160_Constructing_Provably_Correct_Logic_Programs?ev=auth_pub 
[8]:https://mitpress.mit.edu/books/art-prolog 
[9]:https://www.researchgate.net/publication/2649459_Characterizing_Similar_Computational_Behavior_of_Logic_Programs?ev=auth_pub 
[10]:http://jueqingsizhe66.github.io/blog/2016/02/28/the-fifth-interpreter-with-the-implementation-of-letrec-important/ 
[11]:http://jueqingsizhe66.github.io/blog/2015/05/18/the-little-scheme-and-part-of-tss/ 
[12]:http://www.docin.com/p-286207487.html 
[13]: http://jueqingsizhe66.github.io/blog/2016/09/03/sequence-understanding-in-prolog/
[14]:http://link.springer.com/chapter/10.1007%2F3-540-45628-7_15 
[15]:http://www.cs.kuleuven.ac.be/publicaties/rapporten/cw/CW253/NaishSterling.pdf 
[16]:https://www.researchgate.net/publication/220933772_A_Tool_to_Support_Stepwise_Enhancement_in_Prolog 
[17]:https://www.researchgate.net/publication/273980589_Developing_Software_Testing_Programs_in_Prolog_Using_Stepwise_Enhancement 
[18]:http://think-like-a-git.net/sections/graphs-and-git.html 
[19]: /images/prolog/skeleton/two-roads-two-enhancement0.png 
[20]: /images/prolog/skeleton/two-roads-two-enhancement.png 
[21]:http://www.ccs.neu.edu/home/matthias/BTLS/ 
[22]:http://jueqingsizhe66.github.io/blog/2015/05/18/the-little-scheme-and-part-of-tss/ 
