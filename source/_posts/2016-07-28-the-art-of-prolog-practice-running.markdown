---
layout: post
title: "the art of prolog practice(Running)"
date: 2016-07-28 21:08:27 +0800
comments: true
categories: prolog
---




The reference materials below come from [Konn The art of prolog exercises][1]


[One useful tools for writing blog][2]

+ [I. the useful tool improvement](#tool)
+ [II. the direcotry structure](#directory)
+ [III. all the codes](#codes)
<!--more-->

<h1 id ="tool">I. the useful tool improvement</h1>

![allfilesinfoldstoonefile][3]
``` sh
#!/bin/bash - 
#===============================================================================
#
#          FILE: browserNew.sh
# 
#         USAGE: ./browserNew.sh 
# 
#   DESCRIPTION: 
# 
#       OPTIONS: ---
#  REQUIREMENTS: ---
#          BUGS: ---
#         NOTES: ---
#        AUTHOR: Ye Zhaoliang (), zhaoturkkey@163.com
#  ORGANIZATION: YZL
#       CREATED: 2016年07月28日 21:00
#      REVISION:  ---
#===============================================================================

#---  FUNCTION  ----------------------------------------------------------------
#          NAME:  generateChapter
#   DESCRIPTION:  针对不同额文件采用不同的处理方法，判断目录 和判断脚本是一个重要的操作
#                 但是更为重要的是 find获得文集拿的绝对路径(相对于当前文件)
#    PARAMETERS:  
#       RETURNS:  
#-------------------------------------------------------------------------------

## Function definition 

generateChapter() # @Description : 对不同文件进行不同处理
                  # @usage       : generatechapter
{
    count=1;
    # 妙用find 得到当前目录的相对路径 不需要不断的进入目录
    for var in `find . -name "*"`
    do
        if [[ -d  $var ]] # < cannot . Error
        then
            echo "${currentDir}${var#.}是一个目录" # 使用#号来删除之前的点号
    # 为什么会想到使用 ${var##*/}因为var是一个相对当前文件的绝对路径，所以经过测试发现他和${0}等效如果截取到最大长度的/的话
        elif [[ "${var##*/}" == "${0}" ]] # 一定要注意等式两边有空格
        then
            echo "${currentDir}${var#.} ${0} 脚本文件不处理"
        else
            getFile ${currentDir}${var#.} $count # 这边需要去除到第一个点号,这是才得到的处理方法
            count=$(($count+1))
        fi

    done
}



#---  FUNCTION  ----------------------------------------------------------------
#          NAME:  getFile
#   DESCRIPTION:  在每个文件的头部添加一些信息
#    PARAMETERS:  
#       RETURNS:  
#-------------------------------------------------------------------------------
getFile() #@ Description: 细节处理  包括产生开头 产生边框等
          #@ Usage      : getFile
{
    headString="<h2 id=\""$2"\"> "$2". "$1"</h2>"
    echo $2
    startString="\`\`\` prolog "
    endString="\`\`\` "
       #echo "" >>summary.sh
    #echo -e "\033[44;37m -------------------<<<<<<<<<"$1" started>>>>>>>>>-------------------\033[0m" >>summary.sh
    #echo  "# -------------------<<<<<<<<<"$1" started>>>>>>>>>-------------------" >>summary.sh
    echo "+ ["$2". "$1"](#"$2")" >> structure.sh
    echo $headString >> summary.sh
    echo "" >> summary.sh
    echo $startString >> summary.sh
   # echo "#" >>summary.sh
   # echo "#" >>summary.sh
    sed 'p' $1 >> summary.sh
    #echo "#" >>summary.sh
    #echo "#" >>summary.sh
    #echo -e "\033[43;37m --------------------<<<<<<<<<"$1" ended >>>>>>>>>-------------------\033[0m" >> summary.sh
    #echo "# --------------------<<<<<<<<<"$1" ended >>>>>>>>>-------------------" >> summary.sh
    echo "" >> summary.sh
    echo $endString >> summary.sh
    echo "<hr/>" >>summary.sh
    echo "" >> summary.sh
    echo "" >> summary.sh

}

## variable initial 
currentDir=`pwd`


## processing information 
# 如果存在 summary.sh 证明已经存在了，就先把删掉， 因为这里是
# 采用追加的方式，而不是写入。
if [[ -e ./summary.sh ]]
then 
    rm -rf ./summary.sh
fi
if [[ -e ./structure.sh ]]
then 
    rm -rf ./structure.sh
fi

# 调用产生所有的文件夹内的数据到一个文件中
generateChapter
cat summary.sh >> structure.sh

```

The bash shell some extend material:

+ [The arithmetic operation in the shell][5]
+ [The bash OOP][6]
+ [The OOP practice][7]
+ [Your blog about shell][8]

The useful command in the Vim for deleting some characters:
``` sh
:237,3708s/\/e\/TAPP\///g
```

<h1 id = "directory">II. the direcotry structure</h1>


![Taop][4]
```
E:.
│  bible.pl
│  course.pl
│  logicgate-stractured.pl
│  logicgate.pl
│  README
│
├─ch02
│      bible.pl
│      graph.pl
│
├─ch03
│      bintree.pl
│      boolean.pl
│      hanoi.pl
│      lists.pl
│      natural.pl
│      symbolic.pl
│
├─ch05
│      disjoint.pl
│
├─ch08
│      2ex.pl
│      3ex.pl
│      accum.pl
│      elegance.pl
│      hanoi.pl
│
├─ch09
│      compound.pl
│      ex01.pl
│      ex02.pl
│      flatten.pl
│      symbolic.pl
│
├─ch10
│      ex01.pl
│      frozen.pl
│      matches.pl
│      numbervar.pl
│      var.pl
│
├─ch11
│      default.pl
│      ex01.pl
│      ex03.pl
│      ex04.pl
│      greencut.pl
│      redcut.pl
│
├─ch12
│      editor.pl
│      ex04-02.pl
│      ex05.pl
│      exeditor.pl
│      fail.pl
│      io.pl
│      memoise.pl
│      shell.pl
│
└─ch13
        sect02.pl


```

+ [1. bible.pl](#1)
+ [2. ch02/bible.pl](#2)
+ [3. ch02/graph.pl](#3)
+ [4. ch03/bintree.pl](#4)
+ [5. ch03/boolean.pl](#5)
+ [6. ch03/hanoi.pl](#6)
+ [7. ch03/lists.pl](#7)
+ [8. ch03/natural.pl](#8)
+ [9. ch03/symbolic.pl](#9)
+ [10. ch05/disjoint.pl](#10)
+ [11. ch08/2ex.pl](#11)
+ [12. ch08/3ex.pl](#12)
+ [13. ch08/accum.pl](#13)
+ [14. ch08/elegance.pl](#14)
+ [15. ch08/hanoi.pl](#15)
+ [16. ch09/compound.pl](#16)
+ [17. ch09/ex01.pl](#17)
+ [18. ch09/ex02.pl](#18)
+ [19. ch09/flatten.pl](#19)
+ [20. ch09/symbolic.pl](#20)
+ [21. ch10/ex01.pl](#21)
+ [22. ch10/frozen.pl](#22)
+ [23. ch10/matches.pl](#23)
+ [24. ch10/numbervar.pl](#24)
+ [25. ch10/var.pl](#25)
+ [26. ch11/default.pl](#26)
+ [27. ch11/ex01.pl](#27)
+ [28. ch11/ex03.pl](#28)
+ [29. ch11/ex04.pl](#29)
+ [30. ch11/greencut.pl](#30)
+ [31. ch11/redcut.pl](#31)
+ [32. ch12/editor.pl](#32)
+ [33. ch12/ex04-02.pl](#33)
+ [34. ch12/ex05.pl](#34)
+ [35. ch12/exeditor.pl](#35)
+ [36. ch12/fail.pl](#36)
+ [37. ch12/io.pl](#37)
+ [38. ch12/memoise.pl](#38)
+ [39. ch12/shell.pl](#39)
+ [40. ch13/sect02.pl](#40)
+ [41. course.pl](#41)
+ [42. logicgate-stractured.pl](#42)
+ [43. logicgate.pl](#43)
<hr/>

<h1 id="codes">III. all the codes</h1>

<h2 id="1"> 1. bible.pl</h2>

``` prolog
father(terach, abraham).
father(terach, abraham).
father(terach, nachor).
father(terach, nachor).
father(terach, haran).
father(terach, haran).
father(abraham, isaac).
father(abraham, isaac).
father(haran, lot).
father(haran, lot).
father(haran, milcah).
father(haran, milcah).
father(haran, yiscah).
father(haran, yiscah).


mother(sarah, isaac).
mother(sarah, isaac).


male(terach).
male(terach).
male(abraham).
male(abraham).
male(nachor).
male(nachor).
male(haran).
male(haran).
male(isaac).
male(isaac).
male(lot).
male(lot).


female(sarah).
female(sarah).
female(milcah).
female(milcah).
female(yiscah).
female(yiscah).


likes(_, pomegrnates).
likes(_, pomegrnates).


son(X,Y) :- parent(Y,X), male(X).
son(X,Y) :- parent(Y,X), male(X).
daughter(X,Y) :- parent(Y,X), female(X).
daughter(X,Y) :- parent(Y,X), female(X).
parent(X, Y) :- father(X,Y).
parent(X, Y) :- father(X,Y).
parent(Y, X) :- mother(X,Y).
parent(Y, X) :- mother(X,Y).
grandparent(X, Y) :- parent(X, Z), parent(Z, Y).
grandparent(X, Y) :- parent(X, Z), parent(Z, Y).
grandfather(X, Y) :- grandparent(X, Y), male(X).
grandfather(X, Y) :- grandparent(X, Y), male(X).
grandmother(X, Y) :- grandparent(X, Y), female(X).
grandmother(X, Y) :- grandparent(X, Y), female(X).


procreated(Man, Woman) :- father(Man, Child), mother(Woman, Child).
procreated(Man, Woman) :- father(Man, Child), mother(Woman, Child).


uncle(Uncle, NieceOrNephew) :- brother(Uncle, Parent), parent(Parent, NieceOrNephew).
uncle(Uncle, NieceOrNephew) :- brother(Uncle, Parent), parent(Parent, NieceOrNephew).
niece(Niece, UncleOrAunt) :-
niece(Niece, UncleOrAunt) :-
	sibling(UncleOrAunt, Parent), parent(Parent, Niece),
	sibling(UncleOrAunt, Parent), parent(Parent, Niece),
	female(Niece).
	female(Niece).


sibling(Sib1, Sib2) :-
sibling(Sib1, Sib2) :-
	father(Father, Sib1), father(Father, Sib2),
	father(Father, Sib1), father(Father, Sib2),
	mother(Mother, Sib1), mother(Mother, Sib2), Sib1 \= Sib2.
	mother(Mother, Sib1), mother(Mother, Sib2), Sib1 \= Sib2.


cousin(Cousin1, Cousin2) :-
cousin(Cousin1, Cousin2) :-
	parent(Parent1, Cousin1), parent(Parent2, Cousin2),
	parent(Parent1, Cousin1), parent(Parent2, Cousin2),
	sibling(Parent1, Parent2).
	sibling(Parent1, Parent2).


brother(Brother, Sib) :- sibling(Brother, Sib), male(Brother).
brother(Brother, Sib) :- sibling(Brother, Sib), male(Brother).
sister(Sister, Sib) :- sibling(Sister, Sib), female(Sister).
sister(Sister, Sib) :- sibling(Sister, Sib), female(Sister).


mother(Woman) :- mother(Woman, Child).
mother(Woman) :- mother(Woman, Child).


married_couple(Wife, Husband) :- procreated(Husband, Wife).
married_couple(Wife, Husband) :- procreated(Husband, Wife).


mother_in_law(MotherInLaw, Husband) :-
mother_in_law(MotherInLaw, Husband) :-
	mother(MotherInLaw, Wife), married_couple(Wife, Husband).
	mother(MotherInLaw, Wife), married_couple(Wife, Husband).
mother_in_law(MotherInLaw, Wife) :-
mother_in_law(MotherInLaw, Wife) :-
	mother(MotherInLaw, Husband), married_couple(Wife, Husband).
	mother(MotherInLaw, Husband), married_couple(Wife, Husband).


brother_in_law(BrotherInLaw, Husband) :-
brother_in_law(BrotherInLaw, Husband) :-
	brother(BrotherInLaw, Wife), married_couple(Wife, Husband).
	brother(BrotherInLaw, Wife), married_couple(Wife, Husband).
brother_in_law(BrotherInLaw, Wife) :-
brother_in_law(BrotherInLaw, Wife) :-
	brother(BrotherInLaw, Husband), married_couple(Wife, Husband).
	brother(BrotherInLaw, Husband), married_couple(Wife, Husband).
brother_in_law(BrotherInLaw, Person) :-
brother_in_law(BrotherInLaw, Person) :-
	sister(Sister, Person), married_couple(Sister, BrotherInLaw).
	sister(Sister, Person), married_couple(Sister, BrotherInLaw).
son_in_law(SonInLaw, Parent) :-
son_in_law(SonInLaw, Parent) :-
	married_couple(Daughter, SonInLaw),
	married_couple(Daughter, SonInLaw),
	parent(Parent, Daughter).
	parent(Parent, Daughter).


or_gate(Input1, Input2, Output) :-
or_gate(Input1, Input2, Output) :-
	transistor(Input1, Output, ground),
	transistor(Input1, Output, ground),
	resistor(power, Output).
	resistor(power, Output).
or_gate(Input1, Input2, Output) :-
or_gate(Input1, Input2, Output) :-
	transistor(Input2, Output, ground),
	transistor(Input2, Output, ground),
	resistor(power, Output).
	resistor(power, Output).


nor_gate(Input1, Input2, Output) :-
nor_gate(Input1, Input2, Output) :-
	or_gate(Input1, Input2, X), inverter(X, Output).
	or_gate(Input1, Input2, X), inverter(X, Output).
```
<hr/>


<h2 id="2"> 2. ch02/bible.pl</h2>

``` prolog
father(terach, abraham).
father(terach, abraham).
father(terach, nachor).
father(terach, nachor).
father(terach, haran).
father(terach, haran).
father(abraham, isaac).
father(abraham, isaac).
father(haran, lot).
father(haran, lot).
father(haran, milcah).
father(haran, milcah).
father(haran, yiscah).
father(haran, yiscah).


mother(sarah, isaac).
mother(sarah, isaac).


male(terach).
male(terach).
male(abraham).
male(abraham).
male(nachor).
male(nachor).
male(haran).
male(haran).
male(isaac).
male(isaac).
male(lot).
male(lot).


female(sarah).
female(sarah).
female(milcah).
female(milcah).
female(yiscah).
female(yiscah).


likes(_, pomegrnates).
likes(_, pomegrnates).


son(X,Y) :- parent(Y,X), male(X).
son(X,Y) :- parent(Y,X), male(X).
daughter(X,Y) :- parent(Y,X), female(X).
daughter(X,Y) :- parent(Y,X), female(X).
parent(X, Y) :- father(X,Y).
parent(X, Y) :- father(X,Y).
parent(Y, X) :- mother(X,Y).
parent(Y, X) :- mother(X,Y).
grandparent(X, Y) :- parent(X, Z), parent(Z, Y).
grandparent(X, Y) :- parent(X, Z), parent(Z, Y).
grandfather(X, Y) :- grandparent(X, Y), male(X).
grandfather(X, Y) :- grandparent(X, Y), male(X).
grandmother(X, Y) :- grandparent(X, Y), female(X).
grandmother(X, Y) :- grandparent(X, Y), female(X).


procreated(Man, Woman) :- father(Man, Child), mother(Woman, Child).
procreated(Man, Woman) :- father(Man, Child), mother(Woman, Child).


uncle(Uncle, NieceOrNephew) :- brother(Uncle, Parent), parent(Parent, NieceOrNephew).
uncle(Uncle, NieceOrNephew) :- brother(Uncle, Parent), parent(Parent, NieceOrNephew).
niece(Niece, UncleOrAunt) :-
niece(Niece, UncleOrAunt) :-
	sibling(UncleOrAunt, Parent), parent(Parent, Niece),
	sibling(UncleOrAunt, Parent), parent(Parent, Niece),
	female(Niece).
	female(Niece).


sibling(Sib1, Sib2) :-
sibling(Sib1, Sib2) :-
	father(Father, Sib1), father(Father, Sib2),
	father(Father, Sib1), father(Father, Sib2),
	mother(Mother, Sib1), mother(Mother, Sib2), Sib1 \= Sib2.
	mother(Mother, Sib1), mother(Mother, Sib2), Sib1 \= Sib2.


cousin(Cousin1, Cousin2) :-
cousin(Cousin1, Cousin2) :-
	parent(Parent1, Cousin1), parent(Parent2, Cousin2),
	parent(Parent1, Cousin1), parent(Parent2, Cousin2),
	sibling(Parent1, Parent2).
	sibling(Parent1, Parent2).


brother(Brother, Sib) :- sibling(Brother, Sib), male(Brother).
brother(Brother, Sib) :- sibling(Brother, Sib), male(Brother).
sister(Sister, Sib) :- sibling(Sister, Sib), female(Sister).
sister(Sister, Sib) :- sibling(Sister, Sib), female(Sister).


mother(Woman) :- mother(Woman, Child).
mother(Woman) :- mother(Woman, Child).


married_couple(Wife, Husband) :- procreated(Husband, Wife).
married_couple(Wife, Husband) :- procreated(Husband, Wife).


mother_in_law(MotherInLaw, Husband) :-
mother_in_law(MotherInLaw, Husband) :-
	mother(MotherInLaw, Wife), married_couple(Wife, Husband).
	mother(MotherInLaw, Wife), married_couple(Wife, Husband).
mother_in_law(MotherInLaw, Wife) :-
mother_in_law(MotherInLaw, Wife) :-
	mother(MotherInLaw, Husband), married_couple(Wife, Husband).
	mother(MotherInLaw, Husband), married_couple(Wife, Husband).


brother_in_law(BrotherInLaw, Husband) :-
brother_in_law(BrotherInLaw, Husband) :-
	brother(BrotherInLaw, Wife), married_couple(Wife, Husband).
	brother(BrotherInLaw, Wife), married_couple(Wife, Husband).
brother_in_law(BrotherInLaw, Wife) :-
brother_in_law(BrotherInLaw, Wife) :-
	brother(BrotherInLaw, Husband), married_couple(Wife, Husband).
	brother(BrotherInLaw, Husband), married_couple(Wife, Husband).
brother_in_law(BrotherInLaw, Person) :-
brother_in_law(BrotherInLaw, Person) :-
	sister(Sister, Person), married_couple(Sister, BrotherInLaw).
	sister(Sister, Person), married_couple(Sister, BrotherInLaw).
son_in_law(SonInLaw, Parent) :-
son_in_law(SonInLaw, Parent) :-
	married_couple(Daughter, SonInLaw),
	married_couple(Daughter, SonInLaw),
	parent(Parent, Daughter).
	parent(Parent, Daughter).


father(X, Y, pa(X, Y)) :-
father(X, Y, pa(X, Y)) :-
	parent(X,Y), male(X).
	parent(X,Y), male(X).


grandparent(Ancestor, Descendant) :-
grandparent(Ancestor, Descendant) :-
	parent(Ancestor, Person), parent(Person, Descendant).
	parent(Ancestor, Person), parent(Person, Descendant).
greatgrandparent(Ancestor, Descendant) :-
greatgrandparent(Ancestor, Descendant) :-
	parent(Ancestor, Person), grandparent(Person, Descendant).
	parent(Ancestor, Person), grandparent(Person, Descendant).
greatgreatgrandparent(Ancestor, Descendant) :-
greatgreatgrandparent(Ancestor, Descendant) :-
	parent(Ancestor, Person), greatgrandparent(Person, Descendant).
	parent(Ancestor, Person), greatgrandparent(Person, Descendant).


ancestor(Ancestor, Descendant) :-
ancestor(Ancestor, Descendant) :-
	parent(Ancestor, Descendant).
	parent(Ancestor, Descendant).
ancestor(Ancestor, Descendant) :-
ancestor(Ancestor, Descendant) :-
	parent(Ancestor, Person), ancestor(Person, Descendant).
	parent(Ancestor, Person), ancestor(Person, Descendant).
```
<hr/>


<h2 id="3"> 3. ch02/graph.pl</h2>

``` prolog
edge(a, b).
edge(a, b).
edge(a, c).
edge(a, c).
edge(b, d).
edge(b, d).
edge(c, d).
edge(c, d).
edge(d, e).
edge(d, e).
edge(f, g).
edge(f, g).


connected(Node, Node).
connected(Node, Node).
connected(Node1, Node2) :-
connected(Node1, Node2) :-
	edge(Node1, Link), connected(Link, Node2).
	edge(Node1, Link), connected(Link, Node2).


above(Block1, Block2) :- on(Block1, Block2).
above(Block1, Block2) :- on(Block1, Block2).
above(Block1, Block2) :- on(Block1, Other), above(Other, Block2).
above(Block1, Block2) :- on(Block1, Other), above(Other, Block2).
```
<hr/>


<h2 id="4"> 4. ch03/bintree.pl</h2>

``` prolog
% binary_tree(Tree) :- Tree は 二分木である。
% binary_tree(Tree) :- Tree は 二分木である。
binary_tree(void).
binary_tree(void).
binary_tree(tree(Element, Left, Right)) :-
binary_tree(tree(Element, Left, Right)) :-
	binary_tree(Left), binary_tree(Right).
	binary_tree(Left), binary_tree(Right).


% tree_member(Element, Tree) :- Element は 二分木 Tree の要素である。
% tree_member(Element, Tree) :- Element は 二分木 Tree の要素である。
tree_member(X, tree(X, Left, Right)).
tree_member(X, tree(X, Left, Right)).
tree_member(X, tree(Y, Left, Right)) :- tree_member(X, Left).
tree_member(X, tree(Y, Left, Right)) :- tree_member(X, Left).
tree_member(X, tree(Y, Left, Right)) :- tree_member(X, Right).
tree_member(X, tree(Y, Left, Right)) :- tree_member(X, Right).


% isotree(Tree1, Tree2) :- Tree1, Tree2 は同型な二分木である。
% isotree(Tree1, Tree2) :- Tree1, Tree2 は同型な二分木である。
isotree(void, void).
isotree(void, void).
isotree(tree(X, Left1, Right1), tree(X, Left2, Right2)) :-
isotree(tree(X, Left1, Right1), tree(X, Left2, Right2)) :-
	isotree(Left1, Left2), isotree(Right1, Right2).
	isotree(Left1, Left2), isotree(Right1, Right2).
isotree(tree(X, Left1, Right1), tree(X, Left2, Right2)) :-
isotree(tree(X, Left1, Right1), tree(X, Left2, Right2)) :-
	isotree(Left1, Right2), isotree(Right1, Left2).
	isotree(Left1, Right2), isotree(Right1, Left2).


% substitute(X, Y, TreeX, TreeY) :-
% substitute(X, Y, TreeX, TreeY) :-
%	二分木 TreeY は、二分木 TreeX 中に現れるX を総て Y で置換したものである。
%	二分木 TreeY は、二分木 TreeX 中に現れるX を総て Y で置換したものである。
substitute(X, Y, void, void).
substitute(X, Y, void, void).
substitute(X, Y, tree(X, Left, Right), tree(Y, Left1, Right1)) :-
substitute(X, Y, tree(X, Left, Right), tree(Y, Left1, Right1)) :-
	substitute(X, Y, Left, Left1), substitute(X, Y, Right, Right1).
	substitute(X, Y, Left, Left1), substitute(X, Y, Right, Right1).
substitute(X, Y, tree(Z, Left, Right), tree(Z, Left1, Right1)) :-
substitute(X, Y, tree(Z, Left, Right), tree(Z, Left1, Right1)) :-
	X \= Z,
	X \= Z,
	substitute(X, Y, Left, Left1), substitute(X, Y, Right, Right1).
	substitute(X, Y, Left, Left1), substitute(X, Y, Right, Right1).


% pre_order(Tree, Pre) :-
% pre_order(Tree, Pre) :-
%	Pre は Tree の節点を前置順序で並べたものである。
%	Pre は Tree の節点を前置順序で並べたものである。
pre_order(tree(X,L,R), Xs) :-
pre_order(tree(X,L,R), Xs) :-
	pre_order(L, Ls), pre_order(R, Rs), append([X|Ls], Rs, Xs).
	pre_order(L, Ls), pre_order(R, Rs), append([X|Ls], Rs, Xs).
pre_order(void, []).
pre_order(void, []).


% in_order(Tree, In) :-
% in_order(Tree, In) :-
%	In は Tree の節点を中置順序で並べたものである。
%	In は Tree の節点を中置順序で並べたものである。
in_order(tree(X,L,R), Xs) :-
in_order(tree(X,L,R), Xs) :-
	in_order(L, Ls), in_order(R, Rs), append(Ls, [X|Rs], Xs).
	in_order(L, Ls), in_order(R, Rs), append(Ls, [X|Rs], Xs).
in_order(void, []).
in_order(void, []).


% post_order(Tree, Post) :-
% post_order(Tree, Post) :-
%	Post は Tree の節点を中置順序で並べたものである。
%	Post は Tree の節点を中置順序で並べたものである。
post_order(tree(X,L,R), Xs) :-
post_order(tree(X,L,R), Xs) :-
	post_order(L, Ls), post_order(R, Rs),
	post_order(L, Ls), post_order(R, Rs),
	append(Rs, [X], Rs1), append(Ls, Rs1, Xs).
	append(Rs, [X], Rs1), append(Ls, Rs1, Xs).
post_order(void, []).
post_order(void, []).


% subtree(S, T) :- 二分木 S は二分木 T の部分木である。
% subtree(S, T) :- 二分木 S は二分木 T の部分木である。
subtree(S, S).
subtree(S, S).
subtree(S, tree(X, Left, Right)) :- subtree(S, Left).
subtree(S, tree(X, Left, Right)) :- subtree(S, Left).
subtree(S, tree(X, Left, Right)) :- subtree(S, Right).
subtree(S, tree(X, Left, Right)) :- subtree(S, Right).


% sum_tree(Tree, Sum) :- Sum は二分木 Tree の合計である。
% sum_tree(Tree, Sum) :- Sum は二分木 Tree の合計である。
sum_tree(void, 0 ).
sum_tree(void, 0 ).
sum_tree(tree(N, L, R), Sum) :-
sum_tree(tree(N, L, R), Sum) :-
	sum_tree(L, SumL), sum_tree(R, SumR),
	sum_tree(L, SumL), sum_tree(R, SumR),
	plus(SumL, SumR, Sum1), plus(Sum1, N, Sum).
	plus(SumL, SumR, Sum1), plus(Sum1, N, Sum).


% ordered(TreeOfIntegers) :- 二分木 TreeOfIntegers は整数を要素に含む順序木である。
% ordered(TreeOfIntegers) :- 二分木 TreeOfIntegers は整数を要素に含む順序木である。
ordered(void).
ordered(void).
ordered(tree(X, L, R)) :- ordered_left(X, L), ordered_right(X, R).
ordered(tree(X, L, R)) :- ordered_left(X, L), ordered_right(X, R).


ordered_left(X, void).
ordered_left(X, void).
ordered_left(X, tree(Y, L, R)) :-
ordered_left(X, tree(Y, L, R)) :-
	ordered(L), ordered(R), Y < X.
	ordered(L), ordered(R), Y < X.


ordered_right(X, void).
ordered_right(X, void).
ordered_right(X, tree(Y, L, R)) :-
ordered_right(X, tree(Y, L, R)) :-
	ordered(L), ordered(R), Y > X.
	ordered(L), ordered(R), Y > X.


% tree_insert(X, Tree, Tree1) :- X を二分順序木 Tree に挿入すると二分順序木 Tree1 になる。
% tree_insert(X, Tree, Tree1) :- X を二分順序木 Tree に挿入すると二分順序木 Tree1 になる。
tree_insert(X, void, tree(X, void, void)).
tree_insert(X, void, tree(X, void, void)).
tree_insert(X, tree(X, L, R), tree(X, L, R)).
tree_insert(X, tree(X, L, R), tree(X, L, R)).
tree_insert(X, tree(Y, L, R), tree(Y, L, R1)) :-
tree_insert(X, tree(Y, L, R), tree(Y, L, R1)) :-
	X > Y, tree_insert(X, R, R1).
	X > Y, tree_insert(X, R, R1).
tree_insert(X, tree(Y, L, R), tree(Y, L1, R)) :-
tree_insert(X, tree(Y, L, R), tree(Y, L1, R)) :-
	X < Y, tree_insert(X, L, L1).
	X < Y, tree_insert(X, L, L1).

```
<hr/>


<h2 id="5"> 5. ch03/boolean.pl</h2>

``` prolog
:- op(200, fx, '~').
:- op(200, fx, '~').
:- op(500, xfy, 'or').
:- op(500, xfy, 'or').
:- op(400, xfy, 'and').
:- op(400, xfy, 'and').


% satisfiable(Formula) :- ブール式 Formula を真にする様な代入例が存在する。
% satisfiable(Formula) :- ブール式 Formula を真にする様な代入例が存在する。
satisfiable(true).
satisfiable(true).
satisfiable(X and Y) :- satisfiable(X), satisfiable(Y).
satisfiable(X and Y) :- satisfiable(X), satisfiable(Y).
satisfiable(X or Y) :- satisfiable(X).
satisfiable(X or Y) :- satisfiable(X).
satisfiable(X or Y) :- satisfiable(Y).
satisfiable(X or Y) :- satisfiable(Y).
satisfiable(~X) :- invalid(X).
satisfiable(~X) :- invalid(X).


% invalid(Formula) :- ブール式 Formula を偽にする様な代入例が存在する。
% invalid(Formula) :- ブール式 Formula を偽にする様な代入例が存在する。
invalid(false).
invalid(false).
invalid(X or Y) :- invalid(X), invalid(Y).
invalid(X or Y) :- invalid(X), invalid(Y).
invalid(X and Y) :- invalid(X).
invalid(X and Y) :- invalid(X).
invalid(X and Y) :- invalid(Y).
invalid(X and Y) :- invalid(Y).
invalid(~X) :- satisfiable(X).
invalid(~X) :- satisfiable(X).


% boolean_formula(Formula) :- Formula はブール式である。
% boolean_formula(Formula) :- Formula はブール式である。
boolean_formula(true).
boolean_formula(true).
boolean_formula(false).
boolean_formula(false).
boolean_formula(~X) :- boolean_formula(X).
boolean_formula(~X) :- boolean_formula(X).
boolean_formula(X and Y) :- boolean_formula(X), boolean_formula(Y).
boolean_formula(X and Y) :- boolean_formula(X), boolean_formula(Y).
boolean_formula(X or Y) :- boolean_formula(X), boolean_formula(Y).
boolean_formula(X or Y) :- boolean_formula(X), boolean_formula(Y).


% regular_product_form(Formula) :- 論理式 Formula は積標準形である。
% regular_product_form(Formula) :- 論理式 Formula は積標準形である。
regular_product_form(X) :- sum_form(X).
regular_product_form(X) :- sum_form(X).
regular_product_form(X and Y) :- regular_product_form(X), sum_form(Y).
regular_product_form(X and Y) :- regular_product_form(X), sum_form(Y).


literal(true).
literal(true).
literal(false).
literal(false).
literal(~X) :- literal(X).
literal(~X) :- literal(X).


sum_form(X or Y) :- sum_form(X), literal(Y).
sum_form(X or Y) :- sum_form(X), literal(Y).
sum_form(Lit) :- literal(Lit).
sum_form(Lit) :- literal(Lit).


% negation_inwards(F1, F2) :- 論理式 F2 は F1 をリテラル以外に否定を持たない形にしたもの。
% negation_inwards(F1, F2) :- 論理式 F2 は F1 をリテラル以外に否定を持たない形にしたもの。
negation_inwards(X, X) :- literal(X).
negation_inwards(X, X) :- literal(X).
negation_inwards(~(X and Y), X1 or Y1) :-
negation_inwards(~(X and Y), X1 or Y1) :-
	negation_inwards(~X, X1), negation_inwards(~Y, Y1).
	negation_inwards(~X, X1), negation_inwards(~Y, Y1).
negation_inwards(~(X or Y), X1 and Y1) :-
negation_inwards(~(X or Y), X1 and Y1) :-
	negation_inwards(~X, X1), negation_inwards(~Y, Y1).
	negation_inwards(~X, X1), negation_inwards(~Y, Y1).

```
<hr/>


<h2 id="6"> 6. ch03/hanoi.pl</h2>

``` prolog
append([], Ys, Ys).
append([], Ys, Ys).
append([X|Xs], Ys, [X|Zs]) :- append(Xs, Ys, Zs).
append([X|Xs], Ys, [X|Zs]) :- append(Xs, Ys, Zs).


hanoi(0, A, B, C, []).
hanoi(0, A, B, C, []).
hanoi(s(N), A, B, C, Moves) :-
hanoi(s(N), A, B, C, Moves) :-
	hanoi(N, A, C, B, Ms1),
	hanoi(N, A, C, B, Ms1),
	hanoi(N, C, B, A, Ms2),
	hanoi(N, C, B, A, Ms2),
	append(Ms1, [to(A, B)|Ms2], Moves).
	append(Ms1, [to(A, B)|Ms2], Moves).


t(X) :- X.
t(X) :- X.
```
<hr/>


<h2 id="7"> 7. ch03/lists.pl</h2>

``` prolog
% list(Xs) :- Xs はリストである。
% list(Xs) :- Xs はリストである。
list([]).
list([]).
list([X|Xs]) :- list(Xs).
list([X|Xs]) :- list(Xs).


% member_(Element, List) :- Element は List の要素である。
% member_(Element, List) :- Element は List の要素である。
member_(X, Xs) :- append(As, [X|Bs], Xs).
member_(X, Xs) :- append(As, [X|Bs], Xs).


% prefix(Prefix, List) :- Prefix は List の接頭部である。
% prefix(Prefix, List) :- Prefix は List の接頭部である。
prefix(Xs, Ys) :- append(Xs, Zs, Ys).
prefix(Xs, Ys) :- append(Xs, Zs, Ys).


% suffix(Suffix, List) :- Suffix は List の接尾部である。
% suffix(Suffix, List) :- Suffix は List の接尾部である。
suffix(Xs, Ys) :- append(Zs, Xs, Ys).
suffix(Xs, Ys) :- append(Zs, Xs, Ys).


% sublist(Sub, List) :- Sub はリスト List の部分リストである。
% sublist(Sub, List) :- Sub はリスト List の部分リストである。
sublist(Xs, AsXsBs) :- append(As, XsBs, AsXsBs), append(Xs, Bs, XsBs).
sublist(Xs, AsXsBs) :- append(As, XsBs, AsXsBs), append(Xs, Bs, XsBs).


% append(Xs, Ys, XsYs) :- XsYs はリストXs, Ys を連結した結果である。
% append(Xs, Ys, XsYs) :- XsYs はリストXs, Ys を連結した結果である。
append([], Ys, Ys) :- list(Ys).
append([], Ys, Ys) :- list(Ys).
append([X|Xs], Ys, [X|Zs]) :- append(Xs, Ys, Zs).
append([X|Xs], Ys, [X|Zs]) :- append(Xs, Ys, Zs).


% adjacent(X, Y, Zs) :- リスト Zs 中で X と Y が隣り合っている。
% adjacent(X, Y, Zs) :- リスト Zs 中で X と Y が隣り合っている。
adjacent(X, Y, Zs) :- append(As, [X, Y|Bs], Zs).
adjacent(X, Y, Zs) :- append(As, [X, Y|Bs], Zs).


% last(X, Xs) :- X がリスト Xs の最後の要素である。
% last(X, Xs) :- X がリスト Xs の最後の要素である。
last(X, Xs) :- append(As, [X], Xs).
last(X, Xs) :- append(As, [X], Xs).


% reverse_(List, Tsil) :- リスト Tsil はリスト List を反転させたものである。
% reverse_(List, Tsil) :- リスト Tsil はリスト List を反転させたものである。
reverse_(Xs, Ys) :- reverse_(Xs, [], Ys).
reverse_(Xs, Ys) :- reverse_(Xs, [], Ys).
reverse_([X|Xs], Acc, Ys) :- reverse_(Xs, [X|Acc], Ys).
reverse_([X|Xs], Acc, Ys) :- reverse_(Xs, [X|Acc], Ys).
reverse_([], Ys, Ys) :- list(Ys).
reverse_([], Ys, Ys) :- list(Ys).


% length_(Xs, N) :- リスト Xs の長さは N である。
% length_(Xs, N) :- リスト Xs の長さは N である。
length_([], 0 ).
length_([], 0 ).
length_([X|Xs], s(N)) :- length_(Xs, N).
length_([X|Xs], s(N)) :- length_(Xs, N).


subsequence([X | Xs], [X | Ys]) :- subsequence(Xs, Ys).
subsequence([X | Xs], [X | Ys]) :- subsequence(Xs, Ys).
subsequence(Xs, [Y|Ys]) :- subsequence(Xs, Ys).
subsequence(Xs, [Y|Ys]) :- subsequence(Xs, Ys).
subsequence([], Ys).
subsequence([], Ys).


adjacent_(X, Y, [X, Y| Zs]) :- list(Zs).
adjacent_(X, Y, [X, Y| Zs]) :- list(Zs).
adjacent_(X, Y, [Z | Zs]) :- adjacent_(X, Y, Zs).
adjacent_(X, Y, [Z | Zs]) :- adjacent_(X, Y, Zs).


last_(X, [X]).
last_(X, [X]).
last_(X, [Y|Ys]) :- last_(X, Ys).
last_(X, [Y|Ys]) :- last_(X, Ys).


natural_number( 0 ).
natural_number( 0 ).
natural_number(s(N)) :- natural_number(N).
natural_number(s(N)) :- natural_number(N).


plus_(0, X, X) :- natural_number(N).
plus_(0, X, X) :- natural_number(N).
plus_(s(N), M, s(X)) :- plus_(N, M, X).
plus_(s(N), M, s(X)) :- plus_(N, M, X).


% double(List, ListList) :-  List の全要素が ListsList 中に二度ずつ現れる。
% double(List, ListList) :-  List の全要素が ListsList 中に二度ずつ現れる。
double([], []).
double([], []).
double([X|Xs], [X,X|Ys]) :- double(Xs, Ys).
double([X|Xs], [X,X|Ys]) :- double(Xs, Ys).


sum_a(0, []).
sum_a(0, []).
sum_a(Sum, [X|Xs]) :- sum_a(Sum1, Xs), plus_(Sum1, X, Sum).
sum_a(Sum, [X|Xs]) :- sum_a(Sum1, Xs), plus_(Sum1, X, Sum).


sum_b(0, []).
sum_b(0, []).
sum_b(Sum, [0|Xs]) :- sum_b(Sum, Xs).
sum_b(Sum, [0|Xs]) :- sum_b(Sum, Xs).
sum_b(s(Sum), [s(X)|Xs]) :- sum_b(Sum, [X|Xs]).
sum_b(s(Sum), [s(X)|Xs]) :- sum_b(Sum, [X|Xs]).


delete([X|Xs], X, Ys) :- delete(Xs, X, Ys).
delete([X|Xs], X, Ys) :- delete(Xs, X, Ys).
delete([X|Xs], Y, [X|Zs]) :- X \= Y, delete(Xs, Y, Zs).
delete([X|Xs], Y, [X|Zs]) :- X \= Y, delete(Xs, Y, Zs).
delete([],X,[]).
delete([],X,[]).


select([X|Xs], X, Xs).
select([X|Xs], X, Xs).
select([X|Xs], Y, [X|Zs]) :- X \= Y, select(Xs, Y, Zs).
select([X|Xs], Y, [X|Zs]) :- X \= Y, select(Xs, Y, Zs).


sort_([X|Xs], Ys) :- sort(Xs, Zs), insert(X,Zs,Ys).
sort_([X|Xs], Ys) :- sort(Xs, Zs), insert(X,Zs,Ys).
sort_([], []).
sort_([], []).
ordered([X]).
ordered([X]).
ordered([X,Y|Ys]) :- X =< Y, ordered([Y|Ys]).
ordered([X,Y|Ys]) :- X =< Y, ordered([Y|Ys]).
permutations([], []).
permutations([], []).
permutations(Xs, [Z|Zs]) :- select(Xs, Z, Ys), permutations(Ys, Zs).
permutations(Xs, [Z|Zs]) :- select(Xs, Z, Ys), permutations(Ys, Zs).
insert(X,[],[X]).
insert(X,[],[X]).
insert(X,[Y|Ys],[Y|Zs]) :- X > Y, insert(X, Us, Zs).
insert(X,[Y|Ys],[Y|Zs]) :- X > Y, insert(X, Us, Zs).
insert(X,[Y|Ys],[X,Y|Xs]) :- X =< Y.
insert(X,[Y|Ys],[X,Y|Xs]) :- X =< Y.


quicksort([X|Xs], Ys) :-
quicksort([X|Xs], Ys) :-
	partitions(Xs,X,Littles, Bigs),
	partitions(Xs,X,Littles, Bigs),
	quicksort(Littles, Ls),
	quicksort(Littles, Ls),
	quicksort(Bigs, Bs),
	quicksort(Bigs, Bs),
	append(Ls, [X|Bs], Ys).
	append(Ls, [X|Bs], Ys).
quicksort([], []).
quicksort([], []).


partitions([X|Xs], Y, [X|Ls], Bs) :- X =< Y, partitions(Xs, Y, Ls, Bs).
partitions([X|Xs], Y, [X|Ls], Bs) :- X =< Y, partitions(Xs, Y, Ls, Bs).
partitions([X|Xs], Y, Ls, [X|Bs]) :- X > Y, partitions(Xs, Y, Ls, Bs).
partitions([X|Xs], Y, Ls, [X|Bs]) :- X > Y, partitions(Xs, Y, Ls, Bs).
partitions([],Y,[],[]).
partitions([],Y,[],[]).


substitute(X, Y, [], []).
substitute(X, Y, [], []).
substitute(X, Y, [X|Xs], [Y|Ys]) :- substitute(X, Y, Xs, Ys).
substitute(X, Y, [X|Xs], [Y|Ys]) :- substitute(X, Y, Xs, Ys).
substitute(X, Y, [Z|Xs], [Z|Ys]) :- X \= Z, substitute(X, Y, Xs, Ys).
substitute(X, Y, [Z|Xs], [Z|Ys]) :- X \= Z, substitute(X, Y, Xs, Ys).


nonmember(X, []).
nonmember(X, []).
nonmember(X, [Y|Ys]) :- X \= Y, nonmember(X, Ys).
nonmember(X, [Y|Ys]) :- X \= Y, nonmember(X, Ys).


no_doubles([], []).
no_doubles([], []).
no_doubles([X|Xs], Zs) :- member_(X, Xs), no_doubles(Xs, Zs).
no_doubles([X|Xs], Zs) :- member_(X, Xs), no_doubles(Xs, Zs).
no_doubles([X|Xs], [X|Ys]) :- nonmember(X, Xs), no_doubles(Xs, Ys).
no_doubles([X|Xs], [X|Ys]) :- nonmember(X, Xs), no_doubles(Xs, Ys).



```
<hr/>


<h2 id="8"> 8. ch03/natural.pl</h2>

``` prolog
% natural_number(X) :- X は自然数である.
% natural_number(X) :- X は自然数である.
natural_number(0).
natural_number(0).
natural_number(s(X)) :- natural_number(X).
natural_number(s(X)) :- natural_number(X).


% X <= Y :- X, Y は自然数であり、X は Y より小さいか等しい。
% X <= Y :- X, Y は自然数であり、X は Y より小さいか等しい。
:-op(600, xfy, '<=').
:-op(600, xfy, '<=').
0 <= X :- natural_number(X).
0 <= X :- natural_number(X).
s(X) <= s(Y) :- X <=  Y.
s(X) <= s(Y) :- X <=  Y.


% plus(X, Y, Z) :- X, Y, Z は自然数であり、 Z は X と Y の和である
% plus(X, Y, Z) :- X, Y, Z は自然数であり、 Z は X と Y の和である
plus_(0,X,X) :- natural_number(X).
plus_(0,X,X) :- natural_number(X).
plus_(s(X), Y, s(Z)) :- plus_(X, Y, Z).
plus_(s(X), Y, s(Z)) :- plus_(X, Y, Z).


% times(X, Y, Z) :-
% times(X, Y, Z) :-
% 	X, Y, Z が 自然数のとき、ZはXとYの積である。
% 	X, Y, Z が 自然数のとき、ZはXとYの積である。
times(0,X,0).
times(0,X,0).
times(s(X), Y, Z) :- times(X,Y,W), plus_(W,Y,Z).
times(s(X), Y, Z) :- times(X,Y,W), plus_(W,Y,Z).


% exp(N, X, Y) :-
% exp(N, X, Y) :-
%	N, X, Y が自然数のとき、Yは XのN乗である。
%	N, X, Y が自然数のとき、Yは XのN乗である。
exp(s(X), 0, 0 ).
exp(s(X), 0, 0 ).
exp(0, s(X), s(0 )).
exp(0, s(X), s(0 )).
exp(s(N), X, Y) :- exp(N,X,Z), times(Z, X, Y).
exp(s(N), X, Y) :- exp(N,X,Z), times(Z, X, Y).


% factorial(N, X) :- X は N の階乗である。
% factorial(N, X) :- X は N の階乗である。
factorial(0,s(0 )).
factorial(0,s(0 )).
factorial(s(N), Y) :- factorial(N, F2), times(s(N), F2, Y).
factorial(s(N), Y) :- factorial(N, F2), times(s(N), F2, Y).


% minimum(N1, N2, Min) :- 自然数 N1, N2 の最小値は Min である。
% minimum(N1, N2, Min) :- 自然数 N1, N2 の最小値は Min である。
minimum(X, Y, X) :- X <= Y.
minimum(X, Y, X) :- X <= Y.
minimum(X, Y, Y) :- Y <= X.
minimum(X, Y, Y) :- Y <= X.


:-op(600, xfy, '/=').
:-op(600, xfy, '/=').
0 /= s(X) :- natural_number(X).
0 /= s(X) :- natural_number(X).
s(X) /= s(Y) :- X /= Y.
s(X) /= s(Y) :- X /= Y.


:-op(600, xfy, '<<').
:-op(600, xfy, '<<').
X << Y :- X <= Y, X /= Y.
X << Y :- X <= Y, X /= Y.


:-op(600, xfy, '=>').
:-op(600, xfy, '=>').
X => Y :- Y <= X.
X => Y :- Y <= X.


:-op(600, xfy, '>>').
:-op(600, xfy, '>>').
X >> Y :- Y >> X.
X >> Y :- Y >> X.


% mod_(X, Y, Z) :- Z は X を Y で割った剰余である。
% mod_(X, Y, Z) :- Z は X を Y で割った剰余である。
mod(X, Y, X) :- X << Y.
mod(X, Y, X) :- X << Y.
mod(X, Y, Z) :- plus_(X1, Y, X), mod(X1, Y, Z).
mod(X, Y, Z) :- plus_(X1, Y, X), mod(X1, Y, Z).


ackermann(0, N, s(N)) :- natural_number(N).
ackermann(0, N, s(N)) :- natural_number(N).
ackermann(s(M), 0,Val) :- ackermann(M, s(0), Val).
ackermann(s(M), 0,Val) :- ackermann(M, s(0), Val).
ackermann(s(M), N, Val) :-
ackermann(s(M), N, Val) :-
	ackermann(s(M), N, Val1), ackermann(M, Val1, Val).
	ackermann(s(M), N, Val1), ackermann(M, Val1, Val).


gcd(X, 0, X) :- X>> 0.
gcd(X, 0, X) :- X>> 0.
gcd(0, X, X) :- X >> 0.
gcd(0, X, X) :- X >> 0.
gcd(X,X,X) :- X>> 0.
gcd(X,X,X) :- X>> 0.
gcd(X, Y, G) :- X >> Y, plus_(W1, Y, X), gcd(W1, Y, G).
gcd(X, Y, G) :- X >> Y, plus_(W1, Y, X), gcd(W1, Y, G).
gcd(X, Y, G) :- X << Y, gcd(Y, X, G).
gcd(X, Y, G) :- X << Y, gcd(Y, X, G).


even(0 ).
even(0 ).
even(s(s(N))) :- even(N).
even(s(s(N))) :- even(N).


odd(s(0 )).
odd(s(0 )).
odd(s(s(N))) :- odd(N).
odd(s(s(N))) :- odd(N).


fib(0, 0).
fib(0, 0).
fib(s(0 ), s(0 )).
fib(s(0 ), s(0 )).
fib(s(s(N)), X) :- fib(N, Y) , fib(s(N), Z), plus_(Y, Z, X).
fib(s(s(N)), X) :- fib(N, Y) , fib(s(N), Z), plus_(Y, Z, X).


% div(X, Y, Z) :- X, Y, Z が自然数のとき、ZはXとYの商である。
% div(X, Y, Z) :- X, Y, Z が自然数のとき、ZはXとYの商である。
div(X, Y, 0) :- X << Y.
div(X, Y, 0) :- X << Y.
div(N, Y, s(Z)) :- plus_(W, Y, N), div(W, Y, Z).
div(N, Y, s(Z)) :- plus_(W, Y, N), div(W, Y, Z).
```
<hr/>


<h2 id="9"> 9. ch03/symbolic.pl</h2>

``` prolog
% natural_number(X) :- X は 自然数である。
% natural_number(X) :- X は 自然数である。
natural_number(0).
natural_number(0).
natural_number(s(N)) :- natural_number(N).
natural_number(s(N)) :- natural_number(N).


% polynomial(Expression, X) :- Expression は X の多項式である。
% polynomial(Expression, X) :- Expression は X の多項式である。
polynomial(X, X).
polynomial(X, X).
polynomial(Term, X) :- constant(Term).
polynomial(Term, X) :- constant(Term).
polynomial(Term1 + Term2, X) :-
polynomial(Term1 + Term2, X) :-
	polynomial(Term1, X), polynomial(Term2, X).
	polynomial(Term1, X), polynomial(Term2, X).
polynomial(Term1 - Term2, X) :-
polynomial(Term1 - Term2, X) :-
	polynomial(Term1, X), polynomial(Term2, X).
	polynomial(Term1, X), polynomial(Term2, X).
polynomial(Term1 * Term2, X) :-
polynomial(Term1 * Term2, X) :-
	polynomial(Term1, X), polynomial(Term2, X).
	polynomial(Term1, X), polynomial(Term2, X).
polynomial(Term1 / Term2, X) :-
polynomial(Term1 / Term2, X) :-
	polynomial(Term1, X), constant(Term2).
	polynomial(Term1, X), constant(Term2).
polynomial(Term^N, X) :-
polynomial(Term^N, X) :-
	polynomial(Term, X), natural_number(N).
	polynomial(Term, X), natural_number(N).


constant(N) :- natural_number(N).
constant(N) :- natural_number(N).
n( 0, 0 ).
n( 0, 0 ).
n(M, s(N)) :- plus(N1, 1, M), n(N1, N).
n(M, s(N)) :- plus(N1, 1, M), n(N1, N).


%% 記号微分
%% 記号微分
% derivative(Expression, X, DifferentiatedExpression) :-
% derivative(Expression, X, DifferentiatedExpression) :-
%	 DifferentiatedExpression は式 Expression の X に関する導関数である。
%	 DifferentiatedExpression は式 Expression の X に関する導関数である。
derivative(X, X, s( 0 )).
derivative(X, X, s( 0 )).
derivative(X^s(N), X, s(N)*X^N).
derivative(X^s(N), X, s(N)*X^N).
derivative(sin(X), X, cos(X)).
derivative(sin(X), X, cos(X)).
derivative(cos(X), X, -sin(X)).
derivative(cos(X), X, -sin(X)).
derivative(e^X, X, e^X).
derivative(e^X, X, e^X).
derivative(log(X), X, 1/X).
derivative(log(X), X, 1/X).
derivative(F + G, X, DF + DG) :-
derivative(F + G, X, DF + DG) :-
	derivative(F, X, DF), derivative(G, X, DG).
	derivative(F, X, DF), derivative(G, X, DG).
derivative(F - G, X, DF - DG) :-
derivative(F - G, X, DF - DG) :-
	derivative(F, X, DF), derivative(G, X, DG).
	derivative(F, X, DF), derivative(G, X, DG).
derivative(F * G, X, F*DG + DF*G) :-
derivative(F * G, X, F*DG + DF*G) :-
	derivative(F, X, DF), derivative(G, X, DG).
	derivative(F, X, DF), derivative(G, X, DG).
derivative(1/F, X, -DF/(F*F)) :-
derivative(1/F, X, -DF/(F*F)) :-
	derivative(F, X, DF).
	derivative(F, X, DF).
derivative(F/G, X, (DF*G - F*DG)/(G*G)) :-
derivative(F/G, X, (DF*G - F*DG)/(G*G)) :-
	derivative(F, X, DF), derivative(G, X, DG).
	derivative(F, X, DF), derivative(G, X, DG).


%% この様な形式だと、合成関数の導関数を記述できない。
%% この様な形式だと、合成関数の導関数を記述できない。
%% unary_term 形式だと可能
%% unary_term 形式だと可能
% derive(Expression, X, DifferentiatedExpression) :-
% derive(Expression, X, DifferentiatedExpression) :-
%	 DifferentiatedExpression は式 Expression の X に関する導関数である。
%	 DifferentiatedExpression は式 Expression の X に関する導関数である。
derive(X, X, s( 0 )).
derive(X, X, s( 0 )).
derive(unary_term(exp, X), X, unary_term(exp, X)).
derive(unary_term(exp, X), X, unary_term(exp, X)).
derive(X^s(N), X, s(N)*X^N).
derive(X^s(N), X, s(N)*X^N).
derive(unary_term(sin, X), X, unary_term(cos, X)).
derive(unary_term(sin, X), X, unary_term(cos, X)).
derive(unary_term(cos, X), X, -unary_term(sin, X)).
derive(unary_term(cos, X), X, -unary_term(sin, X)).
derive(unary_term(log, X), X, 1/X).
derive(unary_term(log, X), X, 1/X).
derive(-F, X, DF) :- derive(F, X, -DF).
derive(-F, X, DF) :- derive(F, X, -DF).
derive(-F, X, -DF) :- derive(F, X, DF).
derive(-F, X, -DF) :- derive(F, X, DF).
derive(F + G, X, DF + DG) :- derive(F, X, DF), derive(G, X, DG).
derive(F + G, X, DF + DG) :- derive(F, X, DF), derive(G, X, DG).
derive(F - G, X, DF - DG) :- derive(F, X, DF), derive(G, X, DG).
derive(F - G, X, DF - DG) :- derive(F, X, DF), derive(G, X, DG).
derive(F * G, X, DF*G + F*DG) :- derive(F, X, DF), derive(G, X, DG).
derive(F * G, X, DF*G + F*DG) :- derive(F, X, DF), derive(G, X, DG).
derive(1/F, X, -DF/(F*F)) :- derive(F, X, DF).
derive(1/F, X, -DF/(F*F)) :- derive(F, X, DF).
derive(F/G, X, (DF*G - F*DG)/(G*G)) :- derive(F, X, DF), derive(G, X, DG).
derive(F/G, X, (DF*G - F*DG)/(G*G)) :- derive(F, X, DF), derive(G, X, DG).
derive(unary_term(F, U), X, DF * DU) :-
derive(unary_term(F, U), X, DF * DU) :-
	derive(unary_term(F, U), U, DF), derive(U, X, DU).
	derive(unary_term(F, U), U, DF), derive(U, X, DU).


% regular_form(Formula) :- Formula は正規化された算術和形式である。
% regular_form(Formula) :- Formula は正規化された算術和形式である。
regular_form(X) :- constant(X).
regular_form(X) :- constant(X).
regular_form(A + B) :- constant(A), regular_form(B).
regular_form(A + B) :- constant(A), regular_form(B).
```
<hr/>


<h2 id="10"> 10. ch05/disjoint.pl</h2>

``` prolog
joint(Xs, Ys) :- member(X, Xs), member(X, Ys).
joint(Xs, Ys) :- member(X, Xs), member(X, Ys).
disjoint(Xs, Ys) :- not(joint(Xs,Ys)).
disjoint(Xs, Ys) :- not(joint(Xs,Ys)).
member(X, [X|Xs]).
member(X, [X|Xs]).
member(X, [Y|Xs]) :- \=(X,Y), member(X,Xs).
member(X, [Y|Xs]) :- \=(X,Y), member(X,Xs).
```
<hr/>


<h2 id="11"> 11. ch08/2ex.pl</h2>

``` prolog
% gcd(X,Y,Z) :- Z は，整数X, Y の最大公約数である.
% gcd(X,Y,Z) :- Z は，整数X, Y の最大公約数である.
gcd(I, 0, I).
gcd(I, 0, I).
gcd(I,J, Gcd):-
gcd(I,J, Gcd):-
	J > 0, R is I mod J, gcd(J, R, Gcd).
	J > 0, R is I mod J, gcd(J, R, Gcd).


% factorial(N, F) :- F は N の階乗である.
% factorial(N, F) :- F は N の階乗である.
factorial(N, F) :-
factorial(N, F) :-
	N > 0, N1 is N - 1, factorial(N1, F1), F is N * F1.
	N > 0, N1 is N - 1, factorial(N1, F1), F is N * F1.
factorial(0, 1).
factorial(0, 1).


% triangle(N, T) :- T は N 番目の三角数である.
% triangle(N, T) :- T は N 番目の三角数である.
triangle(0,0).
triangle(0,0).
triangle(N, T) :-
triangle(N, T) :-
	N > 0, N1 is N - 1, triangle(N1, T1), T is T1 + N.
	N > 0, N1 is N - 1, triangle(N1, T1), T is T1 + N.


% power(X, N, V) :- N, X, Y が自然数のとき，V は X の N 乗である．
% power(X, N, V) :- N, X, Y が自然数のとき，V は X の N 乗である．
power(X, 0, 1).
power(X, 0, 1).
power(X, N, P) :-
power(X, N, P) :-
	N>0, N1 is N - 1, power(X, N1, P1), P is P1 * X.
	N>0, N1 is N - 1, power(X, N1, P1), P is P1 * X.
```
<hr/>


<h2 id="12"> 12. ch08/3ex.pl</h2>

``` prolog
triangle(N, T) :- triangle(N, 0, T).
triangle(N, T) :- triangle(N, 0, T).
triangle(N, Acc, T) :-
triangle(N, Acc, T) :-
	N > 0, N1 is N - 1, Acc1 is Acc + N, triangle(N1, Acc1, T).
	N > 0, N1 is N - 1, Acc1 is Acc + N, triangle(N1, Acc1, T).
triangle(0, T, T).
triangle(0, T, T).


power(X, N, V) :- power(X, N, 1, V).
power(X, N, V) :- power(X, N, 1, V).
power(X, N, Acc, V) :-
power(X, N, Acc, V) :-
	N > 0, N1 is N - 1, Acc1 is Acc * X, power(X, N1, Acc1, V).
	N > 0, N1 is N - 1, Acc1 is Acc * X, power(X, N1, Acc1, V).
power(X, 0, V, V).
power(X, 0, V, V).


timeslist(Is, Prod) :- timeslist(Is, 1, Prod).
timeslist(Is, Prod) :- timeslist(Is, 1, Prod).
timeslist([I|Is], Acc, Prod) :-
timeslist([I|Is], Acc, Prod) :-
	Acc1 is Acc * I, timeslist(Is, Acc1, Prod).
	Acc1 is Acc * I, timeslist(Is, Acc1, Prod).
timeslist([], Prod, Prod).
timeslist([], Prod, Prod).


area(XYs, Area) :- area(XYs, 0, Area).
area(XYs, Area) :- area(XYs, 0, Area).
area([Tuple], Area, Area).
area([Tuple], Area, Area).
area([(X1, Y1), (X2,Y2) | XYs], Acc, Area) :-
area([(X1, Y1), (X2,Y2) | XYs], Acc, Area) :-
	Acc1 is Acc + (X1 * Y2 - Y1*X2)/2, area([(X2, Y2)|XYs], Acc1, Area).
	Acc1 is Acc + (X1 * Y2 - Y1*X2)/2, area([(X2, Y2)|XYs], Acc1, Area).


minimum([X|Xs], M) :- minimum(Xs, X, M).
minimum([X|Xs], M) :- minimum(Xs, X, M).
minimum([Y|Xs], X, M) :-
minimum([Y|Xs], X, M) :-
	Y < X, minimum(Xs, Y, M).
	Y < X, minimum(Xs, Y, M).
minimum([Y|Xs], X, M) :-
minimum([Y|Xs], X, M) :-
	Y >= X, minimum(Xs, X, M).
	Y >= X, minimum(Xs, X, M).
minimum([], M, M).
minimum([], M, M).


mlength(Xs,N) :- mlength(Xs, 0, N).
mlength(Xs,N) :- mlength(Xs, 0, N).
mlength([X|Xs], Acc, L) :-
mlength([X|Xs], Acc, L) :-
	Acc1 is Acc + 1, mlength(Xs, Acc1, L).
	Acc1 is Acc + 1, mlength(Xs, Acc1, L).
mlength([], L, L).
mlength([], L, L).


range(M,N,Ns) :- range(M, N, [], Ns).
range(M,N,Ns) :- range(M, N, [], Ns).
range(M, N, Acc, Ns) :-
range(M, N, Acc, Ns) :-
	M<N, N1 is N - 1, range(M, N1, [N|Acc], Ns).
	M<N, N1 is N - 1, range(M, N1, [N|Acc], Ns).
range(N, N, Ms, [N|Ms]).
range(N, N, Ms, [N|Ms]).
```
<hr/>


<h2 id="13"> 13. ch08/accum.pl</h2>

``` prolog
factorial(N,F) :- factorial(N,1,F).
factorial(N,F) :- factorial(N,1,F).
factorial(I,N,T,F) :-
factorial(I,N,T,F) :-
	I < N, I1 is I + 1, T1 is T * I1, factorial(I1, N, T1, F).
	I < N, I1 is I + 1, T1 is T * I1, factorial(I1, N, T1, F).
factorial(N,N,F,F).
factorial(N,N,F,F).


factorial(N,T,F) :-
factorial(N,T,F) :-
	N > 0, N1 is N - 1, T1 is T * N, factorial(N1, T1, F).
	N > 0, N1 is N - 1, T1 is T * N, factorial(N1, T1, F).
factorial(0,F,F).
factorial(0,F,F).


% between(I,J,K) :- K は，整数 I, J の両端を含む区間内の整数である.
% between(I,J,K) :- K は，整数 I, J の両端を含む区間内の整数である.
between(I, J, I) :- I =< J.
between(I, J, I) :- I =< J.
between(I, J, K) :-
between(I, J, K) :-
	I < J, I1 is I +1, between(I1, J, K).
	I < J, I1 is I +1, between(I1, J, K).


sumlist([I|Is], Sum) :- sumlist(Is, IsSum), Sum is I + IsSum.
sumlist([I|Is], Sum) :- sumlist(Is, IsSum), Sum is I + IsSum.
sumlist([], 0).
sumlist([], 0).


suml(Is, Sum) :- suml(Is, 0, Sum).
suml(Is, Sum) :- suml(Is, 0, Sum).
suml([I|Is], Acc, Sum) :-
suml([I|Is], Acc, Sum) :-
	Acc1 is Acc + I, suml(Is, Acc1, Sum).
	Acc1 is Acc + I, suml(Is, Acc1, Sum).
suml([], Sum, Sum).
suml([], Sum, Sum).


dot([X|Xs], [Y|Ys], Dot) :- dot(Xs, Ys, D1), Dot is D1 + X*Y.
dot([X|Xs], [Y|Ys], Dot) :- dot(Xs, Ys, D1), Dot is D1 + X*Y.
dot([], [], 0).
dot([], [], 0).


inner_product(Xs, Ys, P) :- inner_product(Xs, Ys, 0, P).
inner_product(Xs, Ys, P) :- inner_product(Xs, Ys, 0, P).
inner_product([X|Xs], [Y|Ys], Acc, P) :-
inner_product([X|Xs], [Y|Ys], Acc, P) :-
	Acc1 is Acc + X*Y, inner_product(Xs, Ys, Acc1, P).
	Acc1 is Acc + X*Y, inner_product(Xs, Ys, Acc1, P).
inner_product([], [], P, P).
inner_product([], [], P, P).
```
<hr/>


<h2 id="14"> 14. ch08/elegance.pl</h2>

``` prolog
% area(Chain, Area) :-
% area(Chain, Area) :-
% Area は，頂点のリスト Chain で囲まれた多角形の面積でる．
% Area は，頂点のリスト Chain で囲まれた多角形の面積でる．
% 但し，各頂点の座標は整数の対 (X, Y) で表わされる.
% 但し，各頂点の座標は整数の対 (X, Y) で表わされる.
area([Tuple], 0).
area([Tuple], 0).
area([(X1, Y1), (X2,Y2) | XYs], Area) :-
area([(X1, Y1), (X2,Y2) | XYs], Area) :-
	area([(X2, Y2)|XYs], Area1),
	area([(X2, Y2)|XYs], Area1),
	Area is (X1 * Y2 - Y1*X2)/2 + Area1.
	Area is (X1 * Y2 - Y1*X2)/2 + Area1.


maximum([X|Xs], M) :- maximum(Xs,X,M).
maximum([X|Xs], M) :- maximum(Xs,X,M).
maximum([X|Xs], Y, M) :- X =< Y, maximum(Xs, Y, M).
maximum([X|Xs], Y, M) :- X =< Y, maximum(Xs, Y, M).
maximum([X|Xs], Y, M) :- X >  Y, maximum(Xs, X, M).
maximum([X|Xs], Y, M) :- X >  Y, maximum(Xs, X, M).
maximum([], M, M).
maximum([], M, M).


mlength([X|Xs], N) :- N > 0, N1 is N-1, mlength(Xs, N1).
mlength([X|Xs], N) :- N > 0, N1 is N-1, mlength(Xs, N1).
mlength([], 0).
mlength([], 0).


mlen([X|Xs], N) :- length(Xs, N1), N is N1 + 1.
mlen([X|Xs], N) :- length(Xs, N1), N is N1 + 1.
mlen([], 0).
mlen([], 0).


range(M,N,[M|Ns]) :- M1 is M+1, range(M1, N, Ns).
range(M,N,[M|Ns]) :- M1 is M+1, range(M1, N, Ns).
range(N,N,[N]).
range(N,N,[N]).
```
<hr/>


<h2 id="15"> 15. ch08/hanoi.pl</h2>

``` prolog
append([], Ys, Ys).
append([], Ys, Ys).
append([X|Xs], Ys, [X|Zs]) :- append(Xs, Ys, Zs).
append([X|Xs], Ys, [X|Zs]) :- append(Xs, Ys, Zs).


hanoi(0, A, B, C, []).
hanoi(0, A, B, C, []).
hanoi(N, A, B, C, Moves) :- >(N,0), is(M, -(N,1)), hanoi(M, A, C, B, Ms1), hanoi(M, C, B, A, Ms2), append(Ms1, [to(A, B)|Ms2], Moves).
hanoi(N, A, B, C, Moves) :- >(N,0), is(M, -(N,1)), hanoi(M, A, C, B, Ms1), hanoi(M, C, B, A, Ms2), append(Ms1, [to(A, B)|Ms2], Moves).
t(X) :- X.
t(X) :- X.
```
<hr/>


<h2 id="16"> 16. ch09/compound.pl</h2>

``` prolog
constant(X) :- atom(X).
constant(X) :- atom(X).
constant(X) :- integer(X).
constant(X) :- integer(X).


% subterm(Sub, Term) :- Sub は基底項 Term の部分項である.
% subterm(Sub, Term) :- Sub は基底項 Term の部分項である.
subterm(Term, Term).
subterm(Term, Term).
subterm(Sub, Term) :-
subterm(Sub, Term) :-
	compound(Term), functor(Term, F, N), subterm(N, Sub, Term).
	compound(Term), functor(Term, F, N), subterm(N, Sub, Term).
subterm(N, Sub, Term) :-
subterm(N, Sub, Term) :-
	N > 1, N1 is N - 1, subterm(N1, Sub, Term).
	N > 1, N1 is N - 1, subterm(N1, Sub, Term).
subterm(N,Sub,Term) :-
subterm(N,Sub,Term) :-
	arg(N, Term,  Arg), subterm(Sub, Arg).
	arg(N, Term,  Arg), subterm(Sub, Arg).


% substitute(Old, New, OldTerm, NewTerm) :-
% substitute(Old, New, OldTerm, NewTerm) :-
%	NewTerm は， OldTerm 中の Old を New で置き換えたものである.
%	NewTerm は， OldTerm 中の Old を New で置き換えたものである.
substitute(Old, New, Old, New).
substitute(Old, New, Old, New).
substitute(Old, New, Term, Term) :-
substitute(Old, New, Term, Term) :-
	constant(Term), Term \= Old.
	constant(Term), Term \= Old.
substitute(Old, New, Term, Term1) :-
substitute(Old, New, Term, Term1) :-
	compound(Term),
	compound(Term),
	functor(Term, F, N),
	functor(Term, F, N),
	functor(Term1, F, N),
	functor(Term1, F, N),
	substitute(N, Old, New, Term, Term1).
	substitute(N, Old, New, Term, Term1).


substitute(N, Old, New, Term, Term1) :-
substitute(N, Old, New, Term, Term1) :-
	N > 0,
	N > 0,
	arg(N, Term, Arg),
	arg(N, Term, Arg),
	substitute(Old, New, Arg, Arg1),
	substitute(Old, New, Arg, Arg1),
	arg(N, Term1, Arg1),
	arg(N, Term1, Arg1),
	N1 is N - 1,
	N1 is N - 1,
	substitute(N1, Old, New, Term, Term1).
	substitute(N1, Old, New, Term, Term1).
substitute(0, Old, New, Term, Term1).
substitute(0, Old, New, Term, Term1).


% subtermu(Sub, Term) :- subterm の univ 版.
% subtermu(Sub, Term) :- subterm の univ 版.
subtermu(Term, Term).
subtermu(Term, Term).
subtermu(Sub, Term) :-
subtermu(Sub, Term) :-
	compound(Term), Term =.. [F|Args], subtermu_list(Sub, Args).
	compound(Term), Term =.. [F|Args], subtermu_list(Sub, Args).
subtermu_list(Sub, [Arg|Args]) :-
subtermu_list(Sub, [Arg|Args]) :-
	subtermu(Sub, Arg).
	subtermu(Sub, Arg).
subtermu_list(Sub, [Arg|Args]) :-
subtermu_list(Sub, [Arg|Args]) :-
	subtermu_list(Sub, Args).
	subtermu_list(Sub, Args).
```
<hr/>


<h2 id="17"> 17. ch09/ex01.pl</h2>

``` prolog
constant(X) :- atom(X).
constant(X) :- atom(X).
constant(X) :- integer(X).
constant(X) :- integer(X).


% flatten1(Xs, Ys) :- Ys は Xs の要素のリストである.
% flatten1(Xs, Ys) :- Ys は Xs の要素のリストである.
flatten1(Xs, Ys) :- flatten1(Xs, [], Ys).
flatten1(Xs, Ys) :- flatten1(Xs, [], Ys).
flatten1([X|Xs], Acc, Ys) :-
flatten1([X|Xs], Acc, Ys) :-
	flatten1(Xs, Acc, Acc1), flatten1(X, Acc1, Ys).
	flatten1(Xs, Acc, Acc1), flatten1(X, Acc1, Ys).
flatten1(X, Acc, [X|Acc]) :-
flatten1(X, Acc, [X|Acc]) :-
	constant(X), X \= [].
	constant(X), X \= [].
flatten1([], Ys , Ys).
flatten1([], Ys , Ys).
```
<hr/>


<h2 id="18"> 18. ch09/ex02.pl</h2>

``` prolog
constant(X) :- integer(X).
constant(X) :- integer(X).
constant(X) :- atom(X).
constant(X) :- atom(X).


% occurences(Sub, Term, N) :- 基底項 Term 中には部分項 Sub が全部で N 個.
% occurences(Sub, Term, N) :- 基底項 Term 中には部分項 Sub が全部で N 個.
occurences(Term, Term, 1).
occurences(Term, Term, 1).
occurences(Sub, Term, N) :-
occurences(Sub, Term, N) :-
	compound(Term), Term =.. [F|Args] , occurences_list(Sub, Args, N).
	compound(Term), Term =.. [F|Args] , occurences_list(Sub, Args, N).
occurences_list(Sub, [Arg|Args], N) :-
occurences_list(Sub, [Arg|Args], N) :-
	occurences(Sub, Arg, N1),
	occurences(Sub, Arg, N1),
	occurences_list(Sub, Args, N2),
	occurences_list(Sub, Args, N2),
	N is N1 + N2.
	N is N1 + N2.
occurences_list(Sub, [Arg|Args], N) :-
occurences_list(Sub, [Arg|Args], N) :-
	not( occurences(Sub, Arg, M)),
	not( occurences(Sub, Arg, M)),
	occurences_list(Sub, Args, N).
	occurences_list(Sub, Args, N).
occurences_list(Sub, [], 0).
occurences_list(Sub, [], 0).


% position(Sub, Term, Pos) :- Sub は 基底項 Term の Pos の位置に現れる. 
% position(Sub, Term, Pos) :- Sub は 基底項 Term の Pos の位置に現れる. 
position(Term, Term, []).
position(Term, Term, []).
position(Sub, Term, Pos) :-
position(Sub, Term, Pos) :-
	compound(Term), functor(Term, F, N), position(N, Sub, Term, Pos).
	compound(Term), functor(Term, F, N), position(N, Sub, Term, Pos).
position(N, Sub, Term, Pos) :-
position(N, Sub, Term, Pos) :-
	N > 1,
	N > 1,
	N1 is N - 1,
	N1 is N - 1,
	position(N1, Sub, Term, Pos).
	position(N1, Sub, Term, Pos).
position(N, Sub, Term, [N|P2]) :-
position(N, Sub, Term, [N|P2]) :-
	arg(N, Term, Arg),
	arg(N, Term, Arg),
	position(Sub, Arg, P2).
	position(Sub, Arg, P2).


:- op(700, xfx, =...).
:- op(700, xfx, =...).
Term =... [F|Args] :-
Term =... [F|Args] :-
	functor(Term, F, N), args(N, Term, [], Args).
	functor(Term, F, N), args(N, Term, [], Args).
args(N, Term, Acc, Args) :-
args(N, Term, Acc, Args) :-
	N > 0, N1 is N-1, arg(N, Term, Arg), args(N1, Term, [Arg|Acc], Args).
	N > 0, N1 is N-1, arg(N, Term, Arg), args(N1, Term, [Arg|Acc], Args).
args(0,Term,Acc,Acc).
args(0,Term,Acc,Acc).


% この様に定義すると，functor_(X, func, 3). などとして使えるが，
% この様に定義すると，functor_(X, func, 3). などとして使えるが，
% functor_(f(a,b,c), F, N). は停止しない．
% functor_(f(a,b,c), F, N). は停止しない．
functor_(Term, F, N) :- length(Args, N), Term =.. [F|Args].
functor_(Term, F, N) :- length(Args, N), Term =.. [F|Args].


% この定義だとその逆．
% この定義だとその逆．
functor__(Term, F, N) :- Term =.. [F|Args], length(Args, N).
functor__(Term, F, N) :- Term =.. [F|Args], length(Args, N).


% arg はおおむねちゃんと動くはず
% arg はおおむねちゃんと動くはず
arg_(N, Term, Arg) :-
arg_(N, Term, Arg) :-
	Term =.. [F | Args],
	Term =.. [F | Args],
	sel(N, Args, Arg).
	sel(N, Args, Arg).
sel(N, [A2|Args], Arg) :-
sel(N, [A2|Args], Arg) :-
	N > 1,
	N > 1,
	N1 is N - 1,
	N1 is N - 1,
	sel(N1, Args, Arg).
	sel(N1, Args, Arg).
sel(1, [Arg|Args], Arg).
sel(1, [Arg|Args], Arg).






% substitute(Old, New, OldTerm, NewTerm) :-
% substitute(Old, New, OldTerm, NewTerm) :-
%	NewTerm は 基底項 OldTerm 中の項 Old を New に置き換えたものである．
%	NewTerm は 基底項 OldTerm 中の項 Old を New に置き換えたものである．
substitute(Old, New, Old, New).
substitute(Old, New, Old, New).
substitute(Old, New, Term, Term) :-
substitute(Old, New, Term, Term) :-
	constant(Term), Term \= Old.
	constant(Term), Term \= Old.
substitute(Old, New, Term, Term1) :-
substitute(Old, New, Term, Term1) :-
	compound(Term),
	compound(Term),
	Term  =.. [F|Args1],
	Term  =.. [F|Args1],
	substitute_list(Old, New, Args1, Args2),
	substitute_list(Old, New, Args1, Args2),
	Term1 =.. [F|Args2].
	Term1 =.. [F|Args2].


substitute_list(Old, New, [Arg| Args1], [Arg2| Args2]) :-
substitute_list(Old, New, [Arg| Args1], [Arg2| Args2]) :-
	substitute(Old, New, Arg, Arg2),
	substitute(Old, New, Arg, Arg2),
	substitute_list(Old, New, Args1, Args2).
	substitute_list(Old, New, Args1, Args2).
substitute_list(Old, New, [], []).
substitute_list(Old, New, [], []).
```
<hr/>


<h2 id="19"> 19. ch09/flatten.pl</h2>

``` prolog
constant(X) :- atom(X).
constant(X) :- atom(X).
constant(X) :- integer(X).
constant(X) :- integer(X).


% flatten1(Xs, Ys) :- Ys は Xs の要素のリストである.
% flatten1(Xs, Ys) :- Ys は Xs の要素のリストである.
flatten1([X|Xs], Ys3) :-
flatten1([X|Xs], Ys3) :-
	flatten2(X, Ys1), flatten2(Xs, Ys2), append(Ys1, Ys2, Ys3).
	flatten2(X, Ys1), flatten2(Xs, Ys2), append(Ys1, Ys2, Ys3).
flatten1(X, [X]) :-
flatten1(X, [X]) :-
	constant(X), X \= [].
	constant(X), X \= [].
flatten1([], []).
flatten1([], []).


flatten2(Xs, Ys) :- flatten2(Xs, [], Ys).
flatten2(Xs, Ys) :- flatten2(Xs, [], Ys).
flatten2([X|Xs], S, Ys) :-
flatten2([X|Xs], S, Ys) :-
	list(X), flatten2(X, [Xs|S], Ys).
	list(X), flatten2(X, [Xs|S], Ys).
flatten2([X|Xs], S, [X|Ys]) :-
flatten2([X|Xs], S, [X|Ys]) :-
	constant(X), X \= [], flatten2(Xs, S, Ys).
	constant(X), X \= [], flatten2(Xs, S, Ys).
flatten2([], [X|S], Ys) :-
flatten2([], [X|S], Ys) :-
	flatten2(X, S, Ys).
	flatten2(X, S, Ys).
flatten2([],[],[]).
flatten2([],[],[]).
list([X|Xs]).
list([X|Xs]).
```
<hr/>


<h2 id="20"> 20. ch09/symbolic.pl</h2>

``` prolog
% polynomial(Expression, X) :- Expression は X の多項式である。
% polynomial(Expression, X) :- Expression は X の多項式である。
polynomial(X, X).
polynomial(X, X).
polynomial(Term, X) :- constant(Term).
polynomial(Term, X) :- constant(Term).
polynomial(Term1 + Term2, X) :-
polynomial(Term1 + Term2, X) :-
	polynomial(Term1, X), polynomial(Term2, X).
	polynomial(Term1, X), polynomial(Term2, X).
polynomial(Term1 - Term2, X) :-
polynomial(Term1 - Term2, X) :-
	polynomial(Term1, X), polynomial(Term2, X).
	polynomial(Term1, X), polynomial(Term2, X).
polynomial(Term1 * Term2, X) :-
polynomial(Term1 * Term2, X) :-
	polynomial(Term1, X), polynomial(Term2, X).
	polynomial(Term1, X), polynomial(Term2, X).
polynomial(Term1 / Term2, X) :-
polynomial(Term1 / Term2, X) :-
	polynomial(Term1, X), constant(Term2).
	polynomial(Term1, X), constant(Term2).
polynomial(Term^N, X) :-
polynomial(Term^N, X) :-
	polynomial(Term, X), integer(N).
	polynomial(Term, X), integer(N).


constant(N) :- integer(N).
constant(N) :- integer(N).


%% 記号微分
%% 記号微分
% derivative(Expression, X, DifferentiatedExpression) :-
% derivative(Expression, X, DifferentiatedExpression) :-
%	 DifferentiatedExpression は式 Expression の X に関する導関数である。
%	 DifferentiatedExpression は式 Expression の X に関する導関数である。
derivative(X, X, 1).
derivative(X, X, 1).
derivative(X^N, X, N*X^N1) :- N > 0, N1 is N - 1.
derivative(X^N, X, N*X^N1) :- N > 0, N1 is N - 1.
derivative(sin(X), X, cos(X)).
derivative(sin(X), X, cos(X)).
derivative(cos(X), X, -sin(X)).
derivative(cos(X), X, -sin(X)).
derivative(e^X, X, e^X).
derivative(e^X, X, e^X).
derivative(log(X), X, 1/X).
derivative(log(X), X, 1/X).
derivative(F + G, X, DF + DG) :-
derivative(F + G, X, DF + DG) :-
	derivative(F, X, DF), derivative(G, X, DG).
	derivative(F, X, DF), derivative(G, X, DG).
derivative(F - G, X, DF - DG) :-
derivative(F - G, X, DF - DG) :-
	derivative(F, X, DF), derivative(G, X, DG).
	derivative(F, X, DF), derivative(G, X, DG).
derivative(F * G, X, F*DG + DF*G) :-
derivative(F * G, X, F*DG + DF*G) :-
	derivative(F, X, DF), derivative(G, X, DG).
	derivative(F, X, DF), derivative(G, X, DG).
derivative(1/F, X, -DF/(F*F)) :-
derivative(1/F, X, -DF/(F*F)) :-
	derivative(F, X, DF).
	derivative(F, X, DF).
derivative(F/G, X, (DF*G - F*DG)/(G*G)) :-
derivative(F/G, X, (DF*G - F*DG)/(G*G)) :-
	derivative(F, X, DF), derivative(G, X, DG).
	derivative(F, X, DF), derivative(G, X, DG).
derivative(-F, X, -DF) :- derivative(F, X, DF).
derivative(-F, X, -DF) :- derivative(F, X, DF).


% univ を使うことにより，合成関数の微分が記述出来る!
% univ を使うことにより，合成関数の微分が記述出来る!
derivative(F_G_X, X, DF*DG) :-
derivative(F_G_X, X, DF*DG) :-
	F_G_X =.. [F, G_X],
	F_G_X =.. [F, G_X],
	derivative(F_G_X, G_X, DF),
	derivative(F_G_X, G_X, DF),
	derivative(G_X, X, DG).
	derivative(G_X, X, DG).
```
<hr/>


<h2 id="21"> 21. ch10/ex01.pl</h2>

``` prolog
range(M,N,Xs) :- nonvar(M), nonvar(N), range1(M,N,Xs).
range(M,N,Xs) :- nonvar(M), nonvar(N), range1(M,N,Xs).
range(M,N,Xs) :- nonvar(Xs), range2(M,N,Xs).
range(M,N,Xs) :- nonvar(Xs), range2(M,N,Xs).


range1(M,N,[M|Ns]) :- M < N, M1 is M + 1, range1(M1, N, Ns).
range1(M,N,[M|Ns]) :- M < N, M1 is M + 1, range1(M1, N, Ns).
range1(N,N,[N]).
range1(N,N,[N]).


range2(M,N,[M|Ns]) :- M1 is M + 1, range2(M1, N, Ns).
range2(M,N,[M|Ns]) :- M1 is M + 1, range2(M1, N, Ns).
range2(N,N,[N]).
range2(N,N,[N]).


% between(I,J,K) :- K は，整数 I, J の両端を含む区間内の整数である.
% between(I,J,K) :- K は，整数 I, J の両端を含む区間内の整数である.
between(I, J, I) :- I =< J.
between(I, J, I) :- I =< J.
between(I, J, K) :-
between(I, J, K) :-
	I < J, I1 is I +1, between(I1, J, K).
	I < J, I1 is I +1, between(I1, J, K).




% plus_(X, Y, Z) :- X に Y を加えるとなんと Z になるらしい.
% plus_(X, Y, Z) :- X に Y を加えるとなんと Z になるらしい.
plus_(X,Y,Z) :- nonvar(X), nonvar(Y), Z is X + Y.
plus_(X,Y,Z) :- nonvar(X), nonvar(Y), Z is X + Y.
plus_(X,Y,Z) :- nonvar(Y), nonvar(Z), X is Z - Y.
plus_(X,Y,Z) :- nonvar(Y), nonvar(Z), X is Z - Y.
plus_(X,Y,Z) :- nonvar(Z), nonvar(X), Y is Z - X.
plus_(X,Y,Z) :- nonvar(Z), nonvar(X), Y is Z - X.
plus_(X,Y,Z) :-
plus_(X,Y,Z) :-
	var(X), var(Y), nonvar(Z),
	var(X), var(Y), nonvar(Z),
	between(0, Z, X),
	between(0, Z, X),
	Y is Z - X.
	Y is Z - X.
```
<hr/>


<h2 id="22"> 22. ch10/frozen.pl</h2>

``` prolog
%TAOP の freeze は普通の処理系に実装されていないので注意．
%TAOP の freeze は普通の処理系に実装されていないので注意．
% このコードは動かない．
% このコードは動かない．
% subterm(Sub, Term) :- Sub は基底項 Term の部分項である.
% subterm(Sub, Term) :- Sub は基底項 Term の部分項である.
subterm(Term, Term).
subterm(Term, Term).
subterm(Sub, Term) :-
subterm(Sub, Term) :-
	compound(Term), functor(Term, F, N), subterm(N, Sub, Term).
	compound(Term), functor(Term, F, N), subterm(N, Sub, Term).
subterm(N, Sub, Term) :-
subterm(N, Sub, Term) :-
	N > 1, N1 is N - 1, subterm(N1, Sub, Term).
	N > 1, N1 is N - 1, subterm(N1, Sub, Term).
subterm(N,Sub,Term) :-
subterm(N,Sub,Term) :-
	arg(N, Term,  Arg), subterm(Sub, Arg).
	arg(N, Term,  Arg), subterm(Sub, Arg).


% occurs_in(Sub, Term) :- Sub は項 Term の部分項である.
% occurs_in(Sub, Term) :- Sub は項 Term の部分項である.
occurs_in(X, Term) :-
occurs_in(X, Term) :-
	copy_term(X, Xf), copy_term(Term, Termf), subterm(Xf, Termf).
	copy_term(X, Xf), copy_term(Term, Termf), subterm(Xf, Termf).
```
<hr/>


<h2 id="23"> 23. ch10/matches.pl</h2>

``` prolog
constant(X) :- atom(X).
constant(X) :- atom(X).
constant(X) :- integer(X).
constant(X) :- integer(X).
constant(X) :- float(X).
constant(X) :- float(X).


% unify(Term1, Term2) :-
% unify(Term1, Term2) :-
%	項 Term1, Term2 は単一化可能である．
%	項 Term1, Term2 は単一化可能である．
%       出現検査しないで許されるのは小学生までだよね!.
%       出現検査しないで許されるのは小学生までだよね!.
unify(X, Y) :-
unify(X, Y) :-
	var(X), var(Y), X = Y.
	var(X), var(Y), X = Y.
unify(X, Y) :-
unify(X, Y) :-
	var(X), nonvar(Y), not_occurs_in(X,Y), X = Y.
	var(X), nonvar(Y), not_occurs_in(X,Y), X = Y.
unify(X, Y) :-
unify(X, Y) :-
	var(Y), nonvar(X), not_occurs_in(Y,X), Y = X.
	var(Y), nonvar(X), not_occurs_in(Y,X), Y = X.
unify(X, Y) :-
unify(X, Y) :-
	nonvar(X), nonvar(Y),
	nonvar(X), nonvar(Y),
	constant(X), constant(Y),
	constant(X), constant(Y),
	X = Y.
	X = Y.
unify(X,Y) :-
unify(X,Y) :-
	nonvar(X), nonvar(Y), compound(X), compound(Y),
	nonvar(X), nonvar(Y), compound(X), compound(Y),
	term_unify(X,Y).
	term_unify(X,Y).


term_unify(X,Y) :-
term_unify(X,Y) :-
	functor(X, F, N), functor(Y, F, N), unify_args(N, X, Y).
	functor(X, F, N), functor(Y, F, N), unify_args(N, X, Y).
unify_args(N, X, Y) :-
unify_args(N, X, Y) :-
	N > 0, unify_arg(N, X, Y), N1 is N-1, unify_args(N1, X,Y).
	N > 0, unify_arg(N, X, Y), N1 is N-1, unify_args(N1, X,Y).
unify_args(0,X,Y).
unify_args(0,X,Y).


unify_arg(N, X, Y) :-
unify_arg(N, X, Y) :-
	arg(N, X, ArgX), arg(N, Y, ArgY), unify(ArgX, ArgY).
	arg(N, X, ArgX), arg(N, Y, ArgY), unify(ArgX, ArgY).


not_occurs_in(X, Y) :-
not_occurs_in(X, Y) :-
	var(Y), X \== Y.
	var(Y), X \== Y.
not_occurs_in(X, Y) :-
not_occurs_in(X, Y) :-
	nonvar(Y), constant(Y).
	nonvar(Y), constant(Y).
not_occurs_in(X, Y) :-
not_occurs_in(X, Y) :-
	nonvar(Y), compound(Y), functor(Y,F,N), not_occurs_in(N,X,Y).
	nonvar(Y), compound(Y), functor(Y,F,N), not_occurs_in(N,X,Y).
not_occurs_in(N,X,Y) :-
not_occurs_in(N,X,Y) :-
	N > 0, arg(N,Y,Arg), not_occurs_in(X, Arg),
	N > 0, arg(N,Y,Arg), not_occurs_in(X, Arg),
	N1 is N - 1, not_occurs_in(N1, X, Y).
	N1 is N - 1, not_occurs_in(N1, X, Y).
not_occurs_in(0,X,Y).
not_occurs_in(0,X,Y).


% subterm(Sub, Term) :- Sub は基底項 Term の部分項である.
% subterm(Sub, Term) :- Sub は基底項 Term の部分項である.
subterm(Term, Term).
subterm(Term, Term).
subterm(Sub, Term) :-
subterm(Sub, Term) :-
	compound(Term), functor(Term, F, N), subterm(N, Sub, Term).
	compound(Term), functor(Term, F, N), subterm(N, Sub, Term).
subterm(N, Sub, Term) :-
subterm(N, Sub, Term) :-
	N > 1, N1 is N - 1, subterm(N1, Sub, Term).
	N > 1, N1 is N - 1, subterm(N1, Sub, Term).
subterm(N,Sub,Term) :-
subterm(N,Sub,Term) :-
	arg(N, Term,  Arg), subterm(Sub, Arg).
	arg(N, Term,  Arg), subterm(Sub, Arg).


% occurs_in(Sub, Term) :- Sub は項 Term の部分項である.
% occurs_in(Sub, Term) :- Sub は項 Term の部分項である.
occurs_in(X, Term) :-
occurs_in(X, Term) :-
	subterm(Sub, Term), X == Sub.
	subterm(Sub, Term), X == Sub.

```
<hr/>


<h2 id="24"> 24. ch10/numbervar.pl</h2>

``` prolog
%TAOP の freeze は普通の処理系に実装されていないので注意．
%TAOP の freeze は普通の処理系に実装されていないので注意．
% このコードは動かない．
% このコードは動かない．
% subterm(Sub, Term) :- Sub は基底項 Term の部分項である.
% subterm(Sub, Term) :- Sub は基底項 Term の部分項である.
subterm(Term, Term).
subterm(Term, Term).
subterm(Sub, Term) :-
subterm(Sub, Term) :-
	compound(Term), functor(Term, F, N), subterm(N, Sub, Term).
	compound(Term), functor(Term, F, N), subterm(N, Sub, Term).
subterm(N, Sub, Term) :-
subterm(N, Sub, Term) :-
	N > 1, N1 is N - 1, subterm(N1, Sub, Term).
	N > 1, N1 is N - 1, subterm(N1, Sub, Term).
subterm(N,Sub,Term) :-
subterm(N,Sub,Term) :-
	arg(N, Term,  Arg), subterm(Sub, Arg).
	arg(N, Term,  Arg), subterm(Sub, Arg).


% occurs_in(Sub, Term) :- Sub は項 Term の部分項である.
% occurs_in(Sub, Term) :- Sub は項 Term の部分項である.
occurs_in(X, Term) :-
occurs_in(X, Term) :-
	numbervars(Term, 1, N).
	numbervars(Term, 1, N).
```
<hr/>


<h2 id="25"> 25. ch10/var.pl</h2>

``` prolog
% plus_(X, Y, Z) :- X に Y を加えるとなんと Z になるらしい.
% plus_(X, Y, Z) :- X に Y を加えるとなんと Z になるらしい.
plus_(X,Y,Z) :- nonvar(X), nonvar(Y), Z is X + Y.
plus_(X,Y,Z) :- nonvar(X), nonvar(Y), Z is X + Y.
plus_(X,Y,Z) :- nonvar(Y), nonvar(Z), X is Z - Y.
plus_(X,Y,Z) :- nonvar(Y), nonvar(Z), X is Z - Y.
plus_(X,Y,Z) :- nonvar(Z), nonvar(X), Y is Z - X.
plus_(X,Y,Z) :- nonvar(Z), nonvar(X), Y is Z - X.


% length_(Xs,N) :- りすと Xs の長さは N なんだって!!!.
% length_(Xs,N) :- りすと Xs の長さは N なんだって!!!.
length_(Xs,N) :- nonvar(Xs), length1(Xs, N).
length_(Xs,N) :- nonvar(Xs), length1(Xs, N).
length_(Xs,N) :- var(Xs), nonvar(N), length2(Xs, N).
length_(Xs,N) :- var(Xs), nonvar(N), length2(Xs, N).
length1([X|Xs], N) :- length(Xs, N1), N is N1 + 1.
length1([X|Xs], N) :- length(Xs, N1), N is N1 + 1.
length1([], 0).
length1([], 0).
length2([X|Xs], N) :- N > 0, N1 is N - 1, length(Xs, N1).
length2([X|Xs], N) :- N > 0, N1 is N - 1, length(Xs, N1).
length2([], 0).
length2([], 0).


constant(X) :- atom(X).
constant(X) :- atom(X).
constant(X) :- integer(X).
constant(X) :- integer(X).
constant(X) :- float(X).
constant(X) :- float(X).


% ground_(Term) :- Term は基底項であることが科学によって証明された!!!!!.
% ground_(Term) :- Term は基底項であることが科学によって証明された!!!!!.
ground_(Term) :-
ground_(Term) :-
	nonvar(Term), constant(Term).
	nonvar(Term), constant(Term).
ground_(Term) :-
ground_(Term) :-
	nonvar(Term),
	nonvar(Term),
	compound(Term),
	compound(Term),
	functor(Term, F, N),
	functor(Term, F, N),
	ground_(N, Term).
	ground_(N, Term).
ground_(N, Term) :-
ground_(N, Term) :-
	N > 0,
	N > 0,
	arg(N, Term, Arg),
	arg(N, Term, Arg),
	ground_(Arg),
	ground_(Arg),
	N1 is N - 1,
	N1 is N - 1,
	ground_(N1, Term).
	ground_(N1, Term).
ground_(0, Term).
ground_(0, Term).


% unify(Term1, Term2) :-
% unify(Term1, Term2) :-
%	項 Term1, Term2 は単一化可能である．出現検査？なにそれおいしい？.
%	項 Term1, Term2 は単一化可能である．出現検査？なにそれおいしい？.
unify(X, Y) :-
unify(X, Y) :-
	var(X), var(Y), X = Y.
	var(X), var(Y), X = Y.
unify(X, Y) :-
unify(X, Y) :-
	var(X), nonvar(Y), X = Y.
	var(X), nonvar(Y), X = Y.
unify(X, Y) :-
unify(X, Y) :-
	var(Y), nonvar(X), Y = X.
	var(Y), nonvar(X), Y = X.
unify(X, Y) :-
unify(X, Y) :-
	nonvar(X), nonvar(Y),
	nonvar(X), nonvar(Y),
	constant(X), constant(Y),
	constant(X), constant(Y),
	X = Y.
	X = Y.
unify(X,Y) :-
unify(X,Y) :-
	nonvar(X), nonvar(Y), compound(X), compound(Y),
	nonvar(X), nonvar(Y), compound(X), compound(Y),
	term_unify(X,Y).
	term_unify(X,Y).


term_unify(X,Y) :-
term_unify(X,Y) :-
	functor(X, F, N), functor(Y, F, N), unify_args(N, X, Y).
	functor(X, F, N), functor(Y, F, N), unify_args(N, X, Y).
unify_args(N, X, Y) :-
unify_args(N, X, Y) :-
	N > 0, unify_arg(N, X, Y), N1 is N-1, unify_args(N1, X,Y).
	N > 0, unify_arg(N, X, Y), N1 is N-1, unify_args(N1, X,Y).
unify_args(0,X,Y).
unify_args(0,X,Y).


unify_arg(N, X, Y) :-
unify_arg(N, X, Y) :-
	arg(N, X, ArgX), arg(N, Y, ArgY), unify(ArgX, ArgY).
	arg(N, X, ArgX), arg(N, Y, ArgY), unify(ArgX, ArgY).
```
<hr/>


<h2 id="26"> 26. ch11/default.pl</h2>

``` prolog
% pension(Person, Pension) :- Pension は Person が受け取る年金の種類だぴょん.
% pension(Person, Pension) :- Pension は Person が受け取る年金の種類だぴょん.
pension(X, invalid_pension) :- invalid(X).
pension(X, invalid_pension) :- invalid(X).
pension(X, old_age_pension) :- over_65(X), paid_up(X).
pension(X, old_age_pension) :- over_65(X), paid_up(X).
pension(X, supplementary_benefit) :- over_65(X).
pension(X, supplementary_benefit) :- over_65(X).
invalid(mc_tavish).
invalid(mc_tavish).
over_65(mc_tavish). over_65(mc_donald). over_65(mc_duff).
over_65(mc_tavish). over_65(mc_donald). over_65(mc_duff).
paid_up(mc_tavish). paid_up(mc_donald).
paid_up(mc_tavish). paid_up(mc_donald).


entitlement(X, Y) :- pension(X, Y).
entitlement(X, Y) :- pension(X, Y).
entitlement(X, nothing) :- not( pension(X, Y) ).
entitlement(X, nothing) :- not( pension(X, Y) ).
```
<hr/>


<h2 id="27"> 27. ch11/ex01.pl</h2>

``` prolog
quicksort([X|Xs], Ys) :-
quicksort([X|Xs], Ys) :-
	partitions(Xs,X,Littles, Bigs),
	partitions(Xs,X,Littles, Bigs),
	quicksort(Littles, Ls),
	quicksort(Littles, Ls),
	quicksort(Bigs, Bs),
	quicksort(Bigs, Bs),
	append(Ls, [X|Bs], Ys).
	append(Ls, [X|Bs], Ys).
quicksort([], []).
quicksort([], []).


partitions([X|Xs], Y, [X|Ls], Bs) :- X =< Y, !, partitions(Xs, Y, Ls, Bs).
partitions([X|Xs], Y, [X|Ls], Bs) :- X =< Y, !, partitions(Xs, Y, Ls, Bs).
partitions([X|Xs], Y, Ls, [X|Bs]) :- X > Y, !, partitions(Xs, Y, Ls, Bs).
partitions([X|Xs], Y, Ls, [X|Bs]) :- X > Y, !, partitions(Xs, Y, Ls, Bs).
partitions([],Y,[],[]) :- !.
partitions([],Y,[],[]) :- !.


constant_to(Y, X) :- integer(Y), !.
constant_to(Y, X) :- integer(Y), !.
constant_to(Y, X) :- atom(Y), !, Y \= X.
constant_to(Y, X) :- atom(Y), !, Y \= X.
constant_to(Term, X) :-
constant_to(Term, X) :-
	compound(Term), !,
	compound(Term), !,
	functor(Term, F, N),
	functor(Term, F, N),
	constant_term(N, Term, X).
	constant_term(N, Term, X).
constant_term(N, Term, X) :-
constant_term(N, Term, X) :-
	N > 0, !,
	N > 0, !,
	arg(N, Term, Arg),
	arg(N, Term, Arg),
	constant_to(Arg, X),
	constant_to(Arg, X),
	N1 is N - 1,
	N1 is N - 1,
	constant_term(N1, Term, X).
	constant_term(N1, Term, X).
constant_term(0, Term, X) :- !.
constant_term(0, Term, X) :- !.


%% 記号微分
%% 記号微分
% derivative(Expression, X, DifferentiatedExpression) :-
% derivative(Expression, X, DifferentiatedExpression) :-
%	 DifferentiatedExpression は式 Expression の X に関する導関数である。
%	 DifferentiatedExpression は式 Expression の X に関する導関数である。
derivative(X, X, 1) :- !.
derivative(X, X, 1) :- !.
derivative(X^N, X, N*X^N1) :- !, N > 0, N1 is N - 1.
derivative(X^N, X, N*X^N1) :- !, N > 0, N1 is N - 1.
derivative(sin(X), X, cos(X)) :- !.
derivative(sin(X), X, cos(X)) :- !.
derivative(cos(X), X, -sin(X)) :- !.
derivative(cos(X), X, -sin(X)) :- !.
derivative(e^X, X, e^X) :- !.
derivative(e^X, X, e^X) :- !.
derivative(log(X), X, 1/X) :- !.
derivative(log(X), X, 1/X) :- !.
derivative(F + G, X, DF + DG) :-
derivative(F + G, X, DF + DG) :-
	!,
	!,
	derivative(F, X, DF), derivative(G, X, DG).
	derivative(F, X, DF), derivative(G, X, DG).
derivative(F - G, X, DF - DG) :-
derivative(F - G, X, DF - DG) :-
	!,
	!,
	derivative(F, X, DF), derivative(G, X, DG).
	derivative(F, X, DF), derivative(G, X, DG).
derivative(F * G, X, F*DG + DF*G) :-
derivative(F * G, X, F*DG + DF*G) :-
	!,
	!,
	derivative(F, X, DF), derivative(G, X, DG).
	derivative(F, X, DF), derivative(G, X, DG).
derivative(1/F, X, -DF/(F*F)) :-
derivative(1/F, X, -DF/(F*F)) :-
	!,
	!,
	derivative(F, X, DF).
	derivative(F, X, DF).
derivative(F/C, X, DF/C) :-
derivative(F/C, X, DF/C) :-
	constant_to(C, X), !,
	constant_to(C, X), !,
	derivative(F, X, DF).
	derivative(F, X, DF).


derivative(F/G, X, (DF*G - F*DG)/(G*G)) :-
derivative(F/G, X, (DF*G - F*DG)/(G*G)) :-
	!,
	!,
	derivative(F, X, DF), derivative(G, X, DG).
	derivative(F, X, DF), derivative(G, X, DG).
derivative(-F, X, -DF) :- !, derivative(F, X, DF).
derivative(-F, X, -DF) :- !, derivative(F, X, DF).
derivative(F_G_X, X, DF*DG) :-
derivative(F_G_X, X, DF*DG) :-
	!,
	!,
	F_G_X =.. [F, G_X],
	F_G_X =.. [F, G_X],
	derivative(F_G_X, G_X, DF),
	derivative(F_G_X, G_X, DF),
	derivative(G_X, X, DG).
	derivative(G_X, X, DG).
derivative(C, X, 0) :- constant_to(C, X), !.
derivative(C, X, 0) :- constant_to(C, X), !.


sort_([X|Xs], Ys) :- !, sort_(Xs, Zs), insert(X, Zs, Ys).
sort_([X|Xs], Ys) :- !, sort_(Xs, Zs), insert(X, Zs, Ys).
sort_([], []) :- !.
sort_([], []) :- !.


insert(X, [], [X]) :- !.
insert(X, [], [X]) :- !.
insert(X, [Y|Ys], [Y|Zs]) :- X > Y, !, insert(X, Ys, Zs).
insert(X, [Y|Ys], [Y|Zs]) :- X > Y, !, insert(X, Ys, Zs).
insert(X, [Y|Ys], [X,Y|Ys]) :- X =< Y, !.
insert(X, [Y|Ys], [X,Y|Ys]) :- X =< Y, !.
```
<hr/>


<h2 id="28"> 28. ch11/ex03.pl</h2>

``` prolog
:- op(700, xfx, \===).
:- op(700, xfx, \===).
X \=== Y :- X == Y, !, fail.
X \=== Y :- X == Y, !, fail.
X \=== Y.
X \=== Y.


nonvar_(X) :- var(X), !, fail.
nonvar_(X) :- var(X), !, fail.
nonvar_(X).
nonvar_(X).
```
<hr/>


<h2 id="29"> 29. ch11/ex04.pl</h2>

``` prolog
constant(X) :- integer(X).
constant(X) :- integer(X).
constant(X) :- atom(X).
constant(X) :- atom(X).


% substitute(Old, New, OldTerm, NewTerm) :-
% substitute(Old, New, OldTerm, NewTerm) :-
%	NewTerm は， OldTerm 中の Old を New で置き換えたものである.
%	NewTerm は， OldTerm 中の Old を New で置き換えたものである.
substitute(Old, New, Old, New) :- !.
substitute(Old, New, Old, New) :- !.
substitute(Old, New, Term, Term) :-
substitute(Old, New, Term, Term) :-
	constant(Term), !, Term \= Old.
	constant(Term), !, Term \= Old.


substitute(Old, New, Term, Term1) :-
substitute(Old, New, Term, Term1) :-
	compound(Term),!,
	compound(Term),!,
	functor(Term, F, N),
	functor(Term, F, N),
	functor(Term1, F, N),
	functor(Term1, F, N),
	substitute(N, Old, New, Term, Term1).
	substitute(N, Old, New, Term, Term1).


substitute(0, Old, New, Term, Term1) :- !.
substitute(0, Old, New, Term, Term1) :- !.
substitute(N, Old, New, Term, Term1) :-
substitute(N, Old, New, Term, Term1) :-
	!,
	!,
	arg(N, Term, Arg),
	arg(N, Term, Arg),
	substitute(Old, New, Arg, Arg1),
	substitute(Old, New, Arg, Arg1),
	arg(N, Term1, Arg1),
	arg(N, Term1, Arg1),
	N1 is N - 1,
	N1 is N - 1,
	substitute(N1, Old, New, Term, Term1).
	substitute(N1, Old, New, Term, Term1).

```
<hr/>


<h2 id="30"> 30. ch11/greencut.pl</h2>

``` prolog
% merge(Xs, Ys, Zs) :-
% merge(Xs, Ys, Zs) :-
%	Zs は，整数の順序リストXs, Ys を併合して得られる整数の順序リストさ.
%	Zs は，整数の順序リストXs, Ys を併合して得られる整数の順序リストさ.
merge([X|Xs], [Y|Ys], [X|Zs]) :- X < Y, !, merge(Xs, [Y|Ys], Zs).
merge([X|Xs], [Y|Ys], [X|Zs]) :- X < Y, !, merge(Xs, [Y|Ys], Zs).
merge([X|Xs], [Y|Ys], [X,Y|Zs]) :- X = Y, !,merge(Xs, Ys, Zs).
merge([X|Xs], [Y|Ys], [X,Y|Zs]) :- X = Y, !,merge(Xs, Ys, Zs).
merge([X|Xs], [Y|Ys], [Y|Zs]) :- X > Y, !,merge([X|Xs], Ys, Zs).
merge([X|Xs], [Y|Ys], [Y|Zs]) :- X > Y, !,merge([X|Xs], Ys, Zs).
merge(Xs, [], Xs) :- !.
merge(Xs, [], Xs) :- !.
merge([], Ys, Ys) :- !.
merge([], Ys, Ys) :- !.


% minimum(X, Y, Min) :- Min は，数 X, Y の最小値である.
% minimum(X, Y, Min) :- Min は，数 X, Y の最小値である.
minimum(X, Y, X) :- X =< Y, !.
minimum(X, Y, X) :- X =< Y, !.
minimum(X, Y, Y) :- Y < X, !.
minimum(X, Y, Y) :- Y < X, !.


constant(X) :- integer(X).
constant(X) :- integer(X).


% polynomial(Term, X) :- Term は X の多項式でありんす.
% polynomial(Term, X) :- Term は X の多項式でありんす.
polynomial(X,X) :- !.
polynomial(X,X) :- !.
polynomial(Term, X) :- constant(Term), !.
polynomial(Term, X) :- constant(Term), !.
polynomial(Term1 + Term2, X) :-
polynomial(Term1 + Term2, X) :-
	!, polynomial(Term1, X), polynomial(Term2, X).
	!, polynomial(Term1, X), polynomial(Term2, X).
polynomial(Term1 - Term2, X) :-
polynomial(Term1 - Term2, X) :-
	!, polynomial(Term1, X), polynomial(Term2, X).
	!, polynomial(Term1, X), polynomial(Term2, X).
polynomial(Term1 * Term2, X) :-
polynomial(Term1 * Term2, X) :-
	!, polynomial(Term1, X), polynomial(Term2, X).
	!, polynomial(Term1, X), polynomial(Term2, X).
polynomial(Term1 / Term2, X) :-
polynomial(Term1 / Term2, X) :-
	!, polynomial(Term1, X), constant(Term2).
	!, polynomial(Term1, X), constant(Term2).
polynomial(Term^N, X) :-
polynomial(Term^N, X) :-
	!, integer(N), polynomial(Term, X).
	!, integer(N), polynomial(Term, X).


% sort(Xs, Ys) :- Ys は整数リストXsの順序付けされた順列である.
% sort(Xs, Ys) :- Ys は整数リストXsの順序付けされた順列である.
sort_(Xs, Ys) :-
sort_(Xs, Ys) :-
	append(As, [X,Y|Bs], Xs),
	append(As, [X,Y|Bs], Xs),
	X > Y,
	X > Y,
	!,
	!,
	append(As, [Y,X|Bs], Xs1),
	append(As, [Y,X|Bs], Xs1),
	sort_(Xs1, Ys).
	sort_(Xs1, Ys).
sort_(Xs, Xs) :- ordered(Xs), !.
sort_(Xs, Xs) :- ordered(Xs), !.


ordered([X]).
ordered([X]).
ordered([X,Y|Ys]) :- X =< Y, ordered([Y|Ys]).
ordered([X,Y|Ys]) :- X =< Y, ordered([Y|Ys]).

```
<hr/>


<h2 id="31"> 31. ch11/redcut.pl</h2>

``` prolog
delete([X|Ys], X, Zs) :- !, delete(Ys, X, Zs).
delete([X|Ys], X, Zs) :- !, delete(Ys, X, Zs).
delete([Y|Ys], X, [Y|Zs]) :- !, delete(Ys, X, Zs).
delete([Y|Ys], X, [Y|Zs]) :- !, delete(Ys, X, Zs).
delete([], X, []).
delete([], X, []).


if_then_else(P, Q, R) :- P, !, Q.
if_then_else(P, Q, R) :- P, !, Q.
if_then_else(P, Q, R) :- R.
if_then_else(P, Q, R) :- R.


member_check(X, [X|Xs]) :- !.
member_check(X, [X|Xs]) :- !.
member_check(X, [Y|Xs]) :- member_check(X, Xs).
member_check(X, [Y|Xs]) :- member_check(X, Xs).



```
<hr/>


<h2 id="32"> 32. ch12/editor.pl</h2>

``` prolog
writeIn([X|Xs]) :- write(X), writeIn(Xs).
writeIn([X|Xs]) :- write(X), writeIn(Xs).
writeIn([]) :- nl.
writeIn([]) :- nl.


% echo :- 対話型ループ.
% echo :- 対話型ループ.
echo :- read(X), echo(X).
echo :- read(X), echo(X).
echo(X) :- end_of_file(X), !.
echo(X) :- end_of_file(X), !.
echo(X) :- write(X), nl, read(Y), !, echo(Y).
echo(X) :- write(X), nl, read(Y), !, echo(Y).


edit :- edit(file([],[])).
edit :- edit(file([],[])).
edit(File) :-
edit(File) :-
	write_prompt, read(Command), edit(File, Command).
	write_prompt, read(Command), edit(File, Command).
edit(File, exit) :- !.
edit(File, exit) :- !.
edit(File, Command) :-
edit(File, Command) :-
	apply(Command, File, File1), !, edit(File1).
	apply(Command, File, File1), !, edit(File1).
edit(File, Command) :-
edit(File, Command) :-
	writeIn([Command, 'is not applicable']), !, edit(File).
	writeIn([Command, 'is not applicable']), !, edit(File).


apply(up, file([X|Xs], Ys), file(Xs, [X|Ys])).
apply(up, file([X|Xs], Ys), file(Xs, [X|Ys])).
apply(up(N), file(Xs, Ys), file(Xs1, Ys1)) :-
apply(up(N), file(Xs, Ys), file(Xs1, Ys1)) :-
	N > 0, up(N, Xs, Ys, Xs1, Ys1).
	N > 0, up(N, Xs, Ys, Xs1, Ys1).
apply(down, file(Xs, [Y|Ys]), file([Y|Xs], Ys)).
apply(down, file(Xs, [Y|Ys]), file([Y|Xs], Ys)).
apply(insert(Line), file(Xs, Ys), file(Xs, [Line|Ys])).
apply(insert(Line), file(Xs, Ys), file(Xs, [Line|Ys])).
apply(delete, file(Xs, [Y|Ys]), file(Xs, Ys)).
apply(delete, file(Xs, [Y|Ys]), file(Xs, Ys)).
apply(print, file([X|Xs], Ys), file([X|Xs], Ys)) :-
apply(print, file([X|Xs], Ys), file([X|Xs], Ys)) :-
	write(X), nl.
	write(X), nl.
apply(print(*), file(Xs, Ys), file(Xs, Ys)) :-
apply(print(*), file(Xs, Ys), file(Xs, Ys)) :-
	reverse(Xs, Xs1), write_file(Xs1), write_file(Ys).
	reverse(Xs, Xs1), write_file(Xs1), write_file(Ys).


up(N, [], Ys, [], Ys).
up(N, [], Ys, [], Ys).
up(0, Xs, Ys, Xs, Ys).
up(0, Xs, Ys, Xs, Ys).
up(N, [X|Xs], Ys, Xs1, Ys1) :-
up(N, [X|Xs], Ys, Xs1, Ys1) :-
	N > 0, N1 is N-1, up(N1, Xs, [X|Ys], Xs1, Ys1).
	N > 0, N1 is N-1, up(N1, Xs, [X|Ys], Xs1, Ys1).


write_file([X|Xs]) :-
write_file([X|Xs]) :-
	write(X), nl, write_file(Xs).
	write(X), nl, write_file(Xs).
write_file([]).
write_file([]).
write_prompt :- write('>>'), nl.
write_prompt :- write('>>'), nl.
```
<hr/>


<h2 id="33"> 33. ch12/ex04-02.pl</h2>

``` prolog
writeIn([X|Xs]) :- write(X), writeIn(Xs).
writeIn([X|Xs]) :- write(X), writeIn(Xs).
writeIn([]) :- nl.
writeIn([]) :- nl.


log :- shell_(log('prolog.log')).
log :- shell_(log('prolog.log')).
log(Log) :- shell_(log(Log)).
log(Log) :- shell_(log(Log)).
shell_(Flag) :-
shell_(Flag) :-
	shell_prompt, shell_read(Goal,Flag), shell_(Goal,Flag).
	shell_prompt, shell_read(Goal,Flag), shell_(Goal,Flag).
shell_(exit, Flag) :-
shell_(exit, Flag) :-
	!, close_logging_file(Flag).
	!, close_logging_file(Flag).
shell_(nolog, Flag) :-
shell_(nolog, Flag) :-
	!, shell_(nolog).
	!, shell_(nolog).
shell_(log, Flag) :-
shell_(log, Flag) :-
	!, shell_(log('prolog.log')).
	!, shell_(log('prolog.log')).
shell_(log(Log), Flag) :-
shell_(log(Log), Flag) :-
	!, shell_(log(Log)).
	!, shell_(log(Log)).
shell_(Goal, Flag) :-
shell_(Goal, Flag) :-
	ground(Goal), !, shell_solve_ground(Goal, Flag), shell_(Flag).
	ground(Goal), !, shell_solve_ground(Goal, Flag), shell_(Flag).
shell_(Goal, Flag) :-
shell_(Goal, Flag) :-
	shell_solve(Goal, Flag), shell_(Flag).
	shell_solve(Goal, Flag), shell_(Flag).


shell_solve(Goal,Flag) :-
shell_solve(Goal,Flag) :-
	Goal, shell_write(Goal,Flag), nl, fail.
	Goal, shell_write(Goal,Flag), nl, fail.
shell_solve(Goal,Flag) :-
shell_solve(Goal,Flag) :-
	shell_write('No (more) solutions', Flag), nl.
	shell_write('No (more) solutions', Flag), nl.


shell_solve_ground(Goal,Flag) :-
shell_solve_ground(Goal,Flag) :-
	Goal, !, shell_write('Yes', Flag), nl.
	Goal, !, shell_write('Yes', Flag), nl.
shell_solve_ground(Goal,Flag) :-
shell_solve_ground(Goal,Flag) :-
	shell_write('Noooooo!', Flag), nl.
	shell_write('Noooooo!', Flag), nl.


shell_prompt :- write('next command?  ').
shell_prompt :- write('next command?  ').


shell_read(X, log(Log)) :-
shell_read(X, log(Log)) :-
	read(X),
	read(X),
	file_write(['Next command?  ', X], Log).
	file_write(['Next command?  ', X], Log).
shell_read(X, nolog) :- read(X).
shell_read(X, nolog) :- read(X).


shell_write(X, nolog) :- write(X).
shell_write(X, nolog) :- write(X).
shell_write(X, log(Log)) :- write(X), file_write([X], Log).
shell_write(X, log(Log)) :- write(X), file_write([X], Log).


file_write(X, File) :- telling(Old), tell(File), writeIn(X), tell(Old).
file_write(X, File) :- telling(Old), tell(File), writeIn(X), tell(Old).
close_logging_file(log(Log)) :- tell(Log), told.
close_logging_file(log(Log)) :- tell(Log), told.
```
<hr/>


<h2 id="34"> 34. ch12/ex05.pl</h2>

``` prolog
abolish(F, N) :- repeat, abolish_loop, !
abolish(F, N) :- repeat, abolish_loop, !


abolish_loop(F, N) :-
abolish_loop(F, N) :-
	length(Args, N),
	length(Args, N),
	Term =.. [F|Args],
	Term =.. [F|Args],
	retract((Term :- X)),!, fail.
	retract((Term :- X)),!, fail.
abolish_loop(F,N).
abolish_loop(F,N).
```
<hr/>


<h2 id="35"> 35. ch12/exeditor.pl</h2>

``` prolog
writeIn([X|Xs]) :- write(X), writeIn(Xs).
writeIn([X|Xs]) :- write(X), writeIn(Xs).
writeIn([]) :- nl.
writeIn([]) :- nl.


% echo :- 対話型ループ.
% echo :- 対話型ループ.
echo :- read(X), echo(X).
echo :- read(X), echo(X).
echo(X) :- end_of_file(X), !.
echo(X) :- end_of_file(X), !.
echo(X) :- write(X), nl, read(Y), !, echo(Y).
echo(X) :- write(X), nl, read(Y), !, echo(Y).


edit :- edit(file([],[])).
edit :- edit(file([],[])).
edit(File) :-
edit(File) :-
	write_prompt, read(Command), edit(File, Command).
	write_prompt, read(Command), edit(File, Command).
edit(File, exit) :- !.
edit(File, exit) :- !.
edit(File, Command) :-
edit(File, Command) :-
	apply(Command, File, File1), !, edit(File1).
	apply(Command, File, File1), !, edit(File1).
edit(File, Command) :-
edit(File, Command) :-
	writeIn([Command, 'is not applicable']), !, edit(File).
	writeIn([Command, 'is not applicable']), !, edit(File).


apply(up, file([X|Xs], Ys), file(Xs, [X|Ys])).
apply(up, file([X|Xs], Ys), file(Xs, [X|Ys])).
apply(up(N), file(Xs, Ys), file(Xs1, Ys1)) :-
apply(up(N), file(Xs, Ys), file(Xs1, Ys1)) :-
	N > 0, move(N, Xs, Ys, Xs1, Ys1).
	N > 0, move(N, Xs, Ys, Xs1, Ys1).
apply(down, file(Xs, [Y|Ys]), file([Y|Xs], Ys)).
apply(down, file(Xs, [Y|Ys]), file([Y|Xs], Ys)).
apply(down(N), file(Xs, Ys), file(Xs1, Ys1)) :-
apply(down(N), file(Xs, Ys), file(Xs1, Ys1)) :-
	N > 0, move(-N, Xs, Ys, Xs1, Ys1).
	N > 0, move(-N, Xs, Ys, Xs1, Ys1).


apply(seek(X), file(Xs, Ys), file(Xs2, Ys2)) :-
apply(seek(X), file(Xs, Ys), file(Xs2, Ys2)) :-
	reverse(Xs, Xs1), append(Xs1, Ys, Zs), seek(X, [], Zs, Xs2, Ys2).
	reverse(Xs, Xs1), append(Xs1, Ys, Zs), seek(X, [], Zs, Xs2, Ys2).


apply(insert(Line), file(Xs, Ys), file(Xs, [Line|Ys])).
apply(insert(Line), file(Xs, Ys), file(Xs, [Line|Ys])).
apply(delete, file(Xs, [Y|Ys]), file(Xs, Ys)).
apply(delete, file(Xs, [Y|Ys]), file(Xs, Ys)).
apply(delete(N), file(Xs, Ys), file(Xs1, Ys1)) :-
apply(delete(N), file(Xs, Ys), file(Xs1, Ys1)) :-
	N > 0, delete(N, Xs, Ys, Xs1, Ys1).
	N > 0, delete(N, Xs, Ys, Xs1, Ys1).


apply(replace(Old, New), file(Xs, Ys), file(Xs1, Ys1)) :-
apply(replace(Old, New), file(Xs, Ys), file(Xs1, Ys1)) :-
	replace(Old, New, Xs, Xs1), replace(Old, New, Ys, Ys1).
	replace(Old, New, Xs, Xs1), replace(Old, New, Ys, Ys1).


apply(print, file([X|Xs], Ys), file([X|Xs], Ys)) :-
apply(print, file([X|Xs], Ys), file([X|Xs], Ys)) :-
	write(X), nl.
	write(X), nl.
apply(print(*), file(Xs, Ys), file(Xs, Ys)) :-
apply(print(*), file(Xs, Ys), file(Xs, Ys)) :-
	reverse(Xs, Xs1), write_file(Xs1), write_file(Ys).
	reverse(Xs, Xs1), write_file(Xs1), write_file(Ys).


apply(swap(N,N), File, File) :- !.
apply(swap(N,N), File, File) :- !.
apply(swap(N, M), file(Xs, Ys), file(Xs1, Ys1)) :-
apply(swap(N, M), file(Xs, Ys), file(Xs1, Ys1)) :-
	N > 0, M > 0, N > M, !,
	N > 0, M > 0, N > M, !,
	N1 is N - 1, M1 is M - 1,
	N1 is N - 1, M1 is M - 1,
	swap(M1, N1, Xs, Ys, Xs1, Ys1).
	swap(M1, N1, Xs, Ys, Xs1, Ys1).
apply(swap(N, M), file(Xs, Ys), file(Xs1, Ys1)) :-
apply(swap(N, M), file(Xs, Ys), file(Xs1, Ys1)) :-
	N > 0, M > 0, N =< M, !,
	N > 0, M > 0, N =< M, !,
	N1 is N - 1, M1 is M - 1,
	N1 is N - 1, M1 is M - 1,
	swap(N1, M1, Xs, Ys, Xs1, Ys1).
	swap(N1, M1, Xs, Ys, Xs1, Ys1).


split(N, As, Xs, Ys) :-
split(N, As, Xs, Ys) :-
	append(Xs, Ys, As), length(Xs, N).
	append(Xs, Ys, As), length(Xs, N).


swap(N, M, Xs, Ys, Xs1, Ys1) :-
swap(N, M, Xs, Ys, Xs1, Ys1) :-
	length(Xs, Pos),
	length(Xs, Pos),
	reverse(Xs, Xsr), append(Xsr, Ys, Zs),
	reverse(Xs, Xsr), append(Xsr, Ys, Zs),
	swap(N, M, Zs, Zs1),
	swap(N, M, Zs, Zs1),
	split(Pos, Zs1, Xsr1, Ys1),
	split(Pos, Zs1, Xsr1, Ys1),
	reverse(Xsr1, Xs1).
	reverse(Xsr1, Xs1).


swap(N, M, Xs, Xs1) :-
swap(N, M, Xs, Xs1) :-
	M1 is M - N - 1,
	M1 is M - N - 1,
	split(N, Xs, Fs1, [X|Rs1]),
	split(N, Xs, Fs1, [X|Rs1]),
	split(M1, Rs1, Fs2, [Y|Rs2]),
	split(M1, Rs1, Fs2, [Y|Rs2]),
	append(Fs1, [Y|Fs2], Is),
	append(Fs1, [Y|Fs2], Is),
	append(Is, [X|Rs2], Xs1).
	append(Is, [X|Rs2], Xs1).


move(0, Xs, Ys, Xs, Ys) :- !.
move(0, Xs, Ys, Xs, Ys) :- !.
move(N, [], Ys, [], Ys) :- !.
move(N, [], Ys, [], Ys) :- !.
move(N, [X|Xs], Ys, Xs1, Ys1) :-
move(N, [X|Xs], Ys, Xs1, Ys1) :-
	N > 0, !, N1 is N-1, move(N1, Xs, [X|Ys], Xs1, Ys1).
	N > 0, !, N1 is N-1, move(N1, Xs, [X|Ys], Xs1, Ys1).
move(N, Xs, [Y|Ys], Xs1, Ys1) :-
move(N, Xs, [Y|Ys], Xs1, Ys1) :-
	N < 0, !, N1 is N+1, move(N1, [Y|Xs], Ys, Xs1, Ys1).
	N < 0, !, N1 is N+1, move(N1, [Y|Xs], Ys, Xs1, Ys1).


delete(0, Xs, Ys, Xs, Ys) :- !.
delete(0, Xs, Ys, Xs, Ys) :- !.
delete(N, Xs, [], Xs, []) :- !.
delete(N, Xs, [], Xs, []) :- !.
delete(N, Xs, [Y|Ys], Xs1, Ys1) :-
delete(N, Xs, [Y|Ys], Xs1, Ys1) :-
	N > 0, !, N1 is N-1, delete(N1, Xs, Ys, Xs1, Ys1).
	N > 0, !, N1 is N-1, delete(N1, Xs, Ys, Xs1, Ys1).


seek(X, Xs, [X|Ys], [X|Xs], Ys) :- !.
seek(X, Xs, [X|Ys], [X|Xs], Ys) :- !.
seek(X, Xs, [Y|Ys], Xs1, Ys1) :- seek(X, [Y|Xs], Ys, Xs1, Ys1).
seek(X, Xs, [Y|Ys], Xs1, Ys1) :- seek(X, [Y|Xs], Ys, Xs1, Ys1).


replace(Old, New, [Old|Xs], [New|Zs]) :- !, replace(Old, New, Xs, Zs).
replace(Old, New, [Old|Xs], [New|Zs]) :- !, replace(Old, New, Xs, Zs).
replace(Old, New, [X|Xs], [X|Zs]) :- replace(Old, New, Xs, Zs).
replace(Old, New, [X|Xs], [X|Zs]) :- replace(Old, New, Xs, Zs).
replace(Old, New, [], []) :- !.
replace(Old, New, [], []) :- !.


write_file([X|Xs]) :-
write_file([X|Xs]) :-
	write(X), nl, write_file(Xs).
	write(X), nl, write_file(Xs).
write_file([]).
write_file([]).
write_prompt :- write('>>'), nl.
write_prompt :- write('>>'), nl.
```
<hr/>


<h2 id="36"> 36. ch12/fail.pl</h2>

``` prolog
tab(N) :- between(1, N, I), put(32), fail.
tab(N) :- between(1, N, I), put(32), fail.
echo :- repeat_, read(X), echo(X), !.
echo :- repeat_, read(X), echo(X), !.
echo(X) :- end_of_file(X), !.
echo(X) :- end_of_file(X), !.
echo(X) :- write(X), nl, fail.
echo(X) :- write(X), nl, fail.


repeat_.
repeat_.
repeat_ :- repeat_.
repeat_ :- repeat_.


end_of_file(33).
end_of_file(33).


consult_(File) :- see(File), consult_loop, seen.
consult_(File) :- see(File), consult_loop, seen.
consult_loop :- repeat, read(Clause), process(Clause), !.
consult_loop :- repeat, read(Clause), process(Clause), !.
process(X) :- end_of_file(X).
process(X) :- end_of_file(X).
process(Clause) :- assert(Clause), fail.
process(Clause) :- assert(Clause), fail.


end_of_file(eof).
end_of_file(eof).
```
<hr/>


<h2 id="37"> 37. ch12/io.pl</h2>

``` prolog
% writeIn(Xs) :- 項のリスト Xs は副作用により出力ストリームに書き出される.
% writeIn(Xs) :- 項のリスト Xs は副作用により出力ストリームに書き出される.
writeIn([X|Xs]) :- write(X), writeIn(Xs).
writeIn([X|Xs]) :- write(X), writeIn(Xs).
writeIn([]) :- nl.
writeIn([]) :- nl.


% read_word_list(Ws) :-
% read_word_list(Ws) :-
%	Ws は入力ストリームから副作用により読み込まれた語のリストである.
%	Ws は入力ストリームから副作用により読み込まれた語のリストである.
% 通常， get(X) は空白を無視するので期待通りには決して動かない．
% 通常， get(X) は空白を無視するので期待通りには決して動かない．
read_word_list(Ws) :- get(C), read_word_list(C, Ws).
read_word_list(Ws) :- get(C), read_word_list(C, Ws).
read_word_list(C, [W|Ws]) :-
read_word_list(C, [W|Ws]) :-
	word_char(C),
	word_char(C),
	read_word(C,W,C1),
	read_word(C,W,C1),
	read_word_list(C1, Ws).
	read_word_list(C1, Ws).
read_word_list(C, Ws) :-
read_word_list(C, Ws) :-
	fill_char(C),
	fill_char(C),
	get(C1),
	get(C1),
	read_word_list(C1, Ws).
	read_word_list(C1, Ws).
read_word_list(C, []) :-
read_word_list(C, []) :-
	end_of_words_char(C).
	end_of_words_char(C).


read_word(C, W, C1) :-
read_word(C, W, C1) :-
	word_chars(C, Cs, C1),
	word_chars(C, Cs, C1),
	name(W, Cs).
	name(W, Cs).


word_chars(C, [C|Cs], C0) :-
word_chars(C, [C|Cs], C0) :-
	word_char(C), !,
	word_char(C), !,
	get(C1),
	get(C1),
	word_chars(C1, Cs, C0).
	word_chars(C1, Cs, C0).
word_chars(C, [], C) :-
word_chars(C, [], C) :-
	not(word_char(C)).
	not(word_char(C)).


word_char(C) :- 97 =< C, C =< 122.
word_char(C) :- 97 =< C, C =< 122.
word_char(C) :- 65 =< C, C =< 90.
word_char(C) :- 65 =< C, C =< 90.
word_char(C) :- 48 =< C, C =< 57.
word_char(C) :- 48 =< C, C =< 57.
word_char(39).
word_char(39).
word_char(95).
word_char(95).
fill_char(32).
fill_char(32).
fill_char(44).
fill_char(44).
end_of_words_char(46).
end_of_words_char(46).
end_of_words_char(63).
end_of_words_char(63).
end_of_words_char(33).
end_of_words_char(33).
```
<hr/>


<h2 id="38"> 38. ch12/memoise.pl</h2>

``` prolog
:- op(700, xfx, to).
:- op(700, xfx, to).
:- dynamic hanoi/5.
:- dynamic hanoi/5.
hanoi(1, A, B, C, [A to B]).
hanoi(1, A, B, C, [A to B]).
hanoi(N, A, B, C, Moves) :-
hanoi(N, A, B, C, Moves) :-
	N > 1,
	N > 1,
	M is N -1,
	M is N -1,
	lemma(hanoi(M, A, C, B, Ms1)),
	lemma(hanoi(M, A, C, B, Ms1)),
	hanoi(M, C, B, A, Ms2),
	hanoi(M, C, B, A, Ms2),
	append(Ms1, [A to B|Ms2], Moves).
	append(Ms1, [A to B|Ms2], Moves).
lemma(P) :- P, asserta((P :- !)).
lemma(P) :- P, asserta((P :- !)).


test_hanoi(N, Pegs, Moves) :-
test_hanoi(N, Pegs, Moves) :-
	hanoi(N, A, B, C, Moves), Pegs = [A,B,C].
	hanoi(N, A, B, C, Moves), Pegs = [A,B,C].
```
<hr/>


<h2 id="39"> 39. ch12/shell.pl</h2>

``` prolog
writeIn([X|Xs]) :- write(X), writeIn(Xs).
writeIn([X|Xs]) :- write(X), writeIn(Xs).
writeIn([]) :- nl.
writeIn([]) :- nl.


log :- shell_(log).
log :- shell_(log).
shell_(Flag) :-
shell_(Flag) :-
	shell_prompt, shell_read(Goal,Flag), shell_(Goal,Flag).
	shell_prompt, shell_read(Goal,Flag), shell_(Goal,Flag).
shell_(exit, Flag) :-
shell_(exit, Flag) :-
	!, close_logging_file.
	!, close_logging_file.
shell_(nolog, Flag) :-
shell_(nolog, Flag) :-
	!, shell_(nolog).
	!, shell_(nolog).
shell_(log, Flag) :-
shell_(log, Flag) :-
	!, shell_(log).
	!, shell_(log).
shell_(Goal, Flag) :-
shell_(Goal, Flag) :-
	ground(Goal), !, shell_solve_ground(Goal, Flag), shell_(Flag).
	ground(Goal), !, shell_solve_ground(Goal, Flag), shell_(Flag).
shell_(Goal, Flag) :-
shell_(Goal, Flag) :-
	shell_solve(Goal, Flag), shell_(Flag).
	shell_solve(Goal, Flag), shell_(Flag).


shell_solve(Goal,Flag) :-
shell_solve(Goal,Flag) :-
	Goal, shell_write(Goal,Flag), nl, fail.
	Goal, shell_write(Goal,Flag), nl, fail.
shell_solve(Goal,Flag) :-
shell_solve(Goal,Flag) :-
	shell_write('No (more) solutions', Flag), nl.
	shell_write('No (more) solutions', Flag), nl.


shell_solve_ground(Goal,Flag) :-
shell_solve_ground(Goal,Flag) :-
	Goal, !, shell_write('Yes', Flag), nl.
	Goal, !, shell_write('Yes', Flag), nl.
shell_solve_ground(Goal,Flag) :-
shell_solve_ground(Goal,Flag) :-
	shell_write('Noooooo!', Flag), nl.
	shell_write('Noooooo!', Flag), nl.


shell_prompt :- write('next command?  ').
shell_prompt :- write('next command?  ').


shell_read(X, log) :-
shell_read(X, log) :-
	read(X),
	read(X),
	file_write(['Next command?  ', X], 'prolog.log').
	file_write(['Next command?  ', X], 'prolog.log').
shell_read(X, nolog) :- read(X).
shell_read(X, nolog) :- read(X).


shell_write(X, nolog) :- write(X).
shell_write(X, nolog) :- write(X).
shell_write(X, log) :- write(X), file_write([X], 'prolog.log').
shell_write(X, log) :- write(X), file_write([X], 'prolog.log').


file_write(X, File) :- telling(Old), tell(File), writeIn(X), tell(Old).
file_write(X, File) :- telling(Old), tell(File), writeIn(X), tell(Old).
close_logging_file :- tell('prolog.log'), told.
close_logging_file :- tell('prolog.log'), told.
```
<hr/>


<h2 id="40"> 40. ch13/sect02.pl</h2>

``` prolog
% verify(Goal) :-
% verify(Goal) :-
%	ゴール Goal は，真なる代入例を持つ．この検証は構成的ではないため，変数への代入は行われない.
%	ゴール Goal は，真なる代入例を持つ．この検証は構成的ではないため，変数への代入は行われない.
verify(Goal) :- not( not(Goal) ).
verify(Goal) :- not( not(Goal) ).


numbervars_('$VAR'(N), N, N1) :- N1 is N + 1.
numbervars_('$VAR'(N), N, N1) :- N1 is N + 1.
numbervars_(Term, N1, N2) :-
numbervars_(Term, N1, N2) :-
	nonvar(Term), functor(Term, Name, N),
	nonvar(Term), functor(Term, Name, N),
	numbervars_(0,N,Term,N1,N2).
	numbervars_(0,N,Term,N1,N2).


numbervars_(N,N,Term,N1,N1).
numbervars_(N,N,Term,N1,N1).
numbervars_(I,N,Term,N1,N3) :-
numbervars_(I,N,Term,N1,N3) :-
	I < N,
	I < N,
	I1 is I + 1,
	I1 is I + 1,
	arg(I1, Term, Arg),
	arg(I1, Term, Arg),
	numbervars_(Arg, N1, N2),
	numbervars_(Arg, N1, N2),
	numbervars_(I1, N, Term, N2, N3).
	numbervars_(I1, N, Term, N2, N3).


ground_(Term) :- numbervars(Term, 0, 0).
ground_(Term) :- numbervars(Term, 0, 0).


freeze(X, Term) :- copy_term(X, Term), numbervars(Term, 0, N).
freeze(X, Term) :- copy_term(X, Term), numbervars(Term, 0, N).


occurs_in(X, Term) :-
occurs_in(X, Term) :-
	freeze(X, Xf), freeze(Term, Termf), subterm(Xf, Termf).
	freeze(X, Xf), freeze(Term, Termf), subterm(Xf, Termf).


% subterm(Sub, Term) :- Sub は基底項 Term の部分項である.
% subterm(Sub, Term) :- Sub は基底項 Term の部分項である.
subterm(Term, Term).
subterm(Term, Term).
subterm(Sub, Term) :-
subterm(Sub, Term) :-
	compound(Term), functor(Term, F, N), subterm(N, Sub, Term).
	compound(Term), functor(Term, F, N), subterm(N, Sub, Term).
subterm(N, Sub, Term) :-
subterm(N, Sub, Term) :-
	N > 1, N1 is N - 1, subterm(N1, Sub, Term).
	N > 1, N1 is N - 1, subterm(N1, Sub, Term).
subterm(N,Sub,Term) :-
subterm(N,Sub,Term) :-
	arg(N, Term,  Arg), subterm(Sub, Arg).
	arg(N, Term,  Arg), subterm(Sub, Arg).


% variants(Term1, Term2) :- 項 Term1, Term2 は互いに別形項である.
% variants(Term1, Term2) :- 項 Term1, Term2 は互いに別形項である.
variants(Term1, Term2) :-
variants(Term1, Term2) :-
	verify((numbervars(Term1, 0, N),
	verify((numbervars(Term1, 0, N),
		numbervars(Term2, 0, N),
		numbervars(Term2, 0, N),
		Term1 = Term2)).
		Term1 = Term2)).


% write_vnames(Term) :- あらかじめ定義された変数名を用いて，項 Term を書き出す.
% write_vnames(Term) :- あらかじめ定義された変数名を用いて，項 Term を書き出す.
write_vnames(Term) :- lettervars(Term), write(Term), fail.
write_vnames(Term) :- lettervars(Term), write(Term), fail.
write_vnames(Term).
write_vnames(Term).


lettervars(Term) :-
lettervars(Term) :-
	list_of_variables(Term, Vars),
	list_of_variables(Term, Vars),
	variable_names(Names),
	variable_names(Names),
	unify_variables(Vars, Names).
	unify_variables(Vars, Names).


constant(X) :- atom(X).
constant(X) :- atom(X).
constant(X) :- integer(X).
constant(X) :- integer(X).
constant(X) :- float(X).
constant(X) :- float(X).


list_of_variables(V, [V]) :- var(V), !.
list_of_variables(V, [V]) :- var(V), !.
list_of_variables(V, []) :- constant(V), !.
list_of_variables(V, []) :- constant(V), !.
list_of_variables(Term, Vs) :-
list_of_variables(Term, Vs) :-
	functor(Term, F, N),
	functor(Term, F, N),
	list_of_variables(N, Term, Vs1),
	list_of_variables(N, Term, Vs1),
	flatten(Vs1, Vs).
	flatten(Vs1, Vs).
list_of_variables(N, Term, [VArgs | Vs] ) :-
list_of_variables(N, Term, [VArgs | Vs] ) :-
	N > 0,
	N > 0,
	arg(N, Term, Arg),
	arg(N, Term, Arg),
	list_of_variables(Arg, VArgs),
	list_of_variables(Arg, VArgs),
	N1 is N - 1,
	N1 is N - 1,
	list_of_variables(N1, Term, Vs).
	list_of_variables(N1, Term, Vs).
list_of_variables(0, Term, []).
list_of_variables(0, Term, []).


unify_variables([V|Vs],[V|Ns]) :- !, unify_variables(Vs, Ns).
unify_variables([V|Vs],[V|Ns]) :- !, unify_variables(Vs, Ns).
unify_variables([V|Vs], Ns) :- !, unify_variables(Vs, Ns).
unify_variables([V|Vs], Ns) :- !, unify_variables(Vs, Ns).
unify_variables(Vs, Ns).
unify_variables(Vs, Ns).


variable_names(['X', 'Y', 'Z', 'U', 'V', 'W', 'X1', 'Y1', 'Z1', 'U1', 'V1', 'W1',
variable_names(['X', 'Y', 'Z', 'U', 'V', 'W', 'X1', 'Y1', 'Z1', 'U1', 'V1', 'W1',
		'X2', 'Y2', 'Z2', 'U2', 'V2', 'W2', 'X3', 'Y3', 'Z3', 'U3', 'V3', 'W3']).
		'X2', 'Y2', 'Z2', 'U2', 'V2', 'W2', 'X3', 'Y3', 'Z3', 'U3', 'V3', 'W3']).


:- dynamic flag/2.
:- dynamic flag/2.
% set_flag(Name, Value) :- フラッグ Name の値を Value にする.
% set_flag(Name, Value) :- フラッグ Name の値を Value にする.
set_flag(Name, X) :-
set_flag(Name, X) :-
	nonvar(Name),
	nonvar(Name),
	retract(flag(Name, Val)), !,
	retract(flag(Name, Val)), !,
	asserta(flag(Name, X)).
	asserta(flag(Name, X)).
set_flag(Name, X) :-
set_flag(Name, X) :-
	nonvar(Name), asserta(flag(Name, X)).
	nonvar(Name), asserta(flag(Name, X)).


% gensym(Prefix, Constant) :- Const は Prefix を接頭部にもつ新しい定数名である.
% gensym(Prefix, Constant) :- Const は Prefix を接頭部にもつ新しい定数名である.
gensym(Prefix, Constant) :-
gensym(Prefix, Constant) :-
	var(V),
	var(V),
	atom(Prefix),
	atom(Prefix),
	old_value(Prefix, N),
	old_value(Prefix, N),
	N1 is N + 1,
	N1 is N + 1,
	set_flag(gensym(Prefix), N1),
	set_flag(gensym(Prefix), N1),
	string_concatenate(Prefix, N1, Constant),
	string_concatenate(Prefix, N1, Constant),
	!.
	!.


old_value(Prefix, N) :- flag(gensym(Prefix), N), !.
old_value(Prefix, N) :- flag(gensym(Prefix), N), !.
old_value(Prefix, 0).
old_value(Prefix, 0).


string_concatenate(X,Y,XY) :-
string_concatenate(X,Y,XY) :-
	name(X, Xs), name(Y, Ys), append(Xs, Ys, XYs), name(XY, XYs).
	name(X, Xs), name(Y, Ys), append(Xs, Ys, XYs), name(XY, XYs).
```
<hr/>


<h2 id="41"> 41. course.pl</h2>

``` prolog
course(complexity, monday, 9, 11 , david, harel, feinberg, a).
course(complexity, monday, 9, 11 , david, harel, feinberg, a).
course(complexity, time(monday, 9, 11), lecturer(david, harel),
course(complexity, time(monday, 9, 11), lecturer(david, harel),
       location(feinberg, a)).
       location(feinberg, a)).


lecturer(Lecturer, Course) :- course(Course, Time, Lecturer, Location).
lecturer(Lecturer, Course) :- course(Course, Time, Lecturer, Location).
duration(Course, Length) :-
duration(Course, Length) :-
	course(Course, time(Day, Start, Finish), Lecturer, Location),
	course(Course, time(Day, Start, Finish), Lecturer, Location),
	plus(Start, Duration, Finish).
	plus(Start, Duration, Finish).
teaches(Lecturer, Day) :-
teaches(Lecturer, Day) :-
	course(Course, time(Day, Start, Finish), Lecturer, Location).
	course(Course, time(Day, Start, Finish), Lecturer, Location).
occupied(Room, Day, Time) :-
occupied(Room, Day, Time) :-
	course(Course, time(Day, Start, Finish), Lecturer,
	course(Course, time(Day, Start, Finish), Lecturer,
	       location(Building, Room)),
	       location(Building, Room)),
	Start =< Time, Time =< Room.
	Start =< Time, Time =< Room.


location(Course, Building) :-
location(Course, Building) :-
	course(Course, Time, Lecturer, location(Building, Room)).
	course(Course, Time, Lecturer, location(Building, Room)).
busy(Lecturer, time(Day, Time)) :-
busy(Lecturer, time(Day, Time)) :-
	course(Course, time(Day, Start, Finish), Lecturer, Location),
	course(Course, time(Day, Start, Finish), Lecturer, Location),
	Start =< Time, Time =<  Finish.
	Start =< Time, Time =<  Finish.
cannot_meet(Lecturer1, Lecturer2) :-
cannot_meet(Lecturer1, Lecturer2) :-
	course(C1, Time, Lecturer1, Location1),
	course(C1, Time, Lecturer1, Location1),
	course(C2, Time, Lecturer2, Location2),
	course(C2, Time, Lecturer2, Location2),
	C1 \= C2, Lecturer1 \= Lecturer2, Location1 \= Location2.
	C1 \= C2, Lecturer1 \= Lecturer2, Location1 \= Location2.


schedule_conflict(Time, Place, Course1, Course2) :-
schedule_conflict(Time, Place, Course1, Course2) :-
	course(Course1, Time, Lecturer1, Place),
	course(Course1, Time, Lecturer1, Place),
	course(Course2, Time, Lecturer2, Place),
	course(Course2, Time, Lecturer2, Place),
	Course1 \= Course2.
	Course1 \= Course2.
```
<hr/>


<h2 id="42"> 42. logicgate-stractured.pl</h2>

``` prolog
% resistor(R, Node1, Node2) :- R は Node1 と Node2 の間にある。.
% resistor(R, Node1, Node2) :- R は Node1 と Node2 の間にある。.
resistor(r1, power, n1).
resistor(r1, power, n1).
resistor(r2, power, n2).
resistor(r2, power, n2).


% transistor(T, Gate, Source, Drain) :-
% transistor(T, Gate, Source, Drain) :-
% 	Tは、ゲートがGate, ソースがSource、ドレインがDrain
% 	Tは、ゲートがGate, ソースがSource、ドレインがDrain
% 	であるようなトランジスタである.
% 	であるようなトランジスタである.
transistor(t1, n2, ground, n1).
transistor(t1, n2, ground, n1).
transistor(t2, n3, n4, n2).
transistor(t2, n3, n4, n2).
transistor(t3, n5, ground, n4).
transistor(t3, n5, ground, n4).


% inverter(I, Input, Output) :- I は Input を反転するインバータである.
% inverter(I, Input, Output) :- I は Input を反転するインバータである.
inverter(inv(T,R), Input, Output) :-
inverter(inv(T,R), Input, Output) :-
	transistor(T, Input, ground, Output),
	transistor(T, Input, ground, Output),
	resistor(R, power, Output).
	resistor(R, power, Output).


% nand_gate(Nand, Input1, Input2, Output) :-
% nand_gate(Nand, Input1, Input2, Output) :-
% 	Nand は、Input1 と Input2 の論理 NAND を Output とするゲートである。
% 	Nand は、Input1 と Input2 の論理 NAND を Output とするゲートである。
nand_gate(nand(T1, T2, R), Input1, Input2, Output) :-
nand_gate(nand(T1, T2, R), Input1, Input2, Output) :-
	transistor(T1, Input1, X, Output),
	transistor(T1, Input1, X, Output),
	transistor(T2, Input2, ground, X),
	transistor(T2, Input2, ground, X),
	resistor(R, power, Output).
	resistor(R, power, Output).


% and_gate(And, Input1, Input2, Output) :-
% and_gate(And, Input1, Input2, Output) :-
% 	And は、Input1 と Input2 の論理AND を Output とするゲートである。
% 	And は、Input1 と Input2 の論理AND を Output とするゲートである。
and_gate(and(N, I), Input1, Input2, Output) :-
and_gate(and(N, I), Input1, Input2, Output) :-
	nand_gate(N, Input1, Input2, X),
	nand_gate(N, Input1, Input2, X),
	inverter(I, X, Output).
	inverter(I, X, Output).
```
<hr/>


<h2 id="43"> 43. logicgate.pl</h2>

``` prolog
resistor(power, n1).
resistor(power, n1).
resistor(power, n2).
resistor(power, n2).


transistor(n2, ground, n1).
transistor(n2, ground, n1).
transistor(n3, n4, n2).
transistor(n3, n4, n2).
transistor(n5, ground, n4).
transistor(n5, ground, n4).


inverter(Input, Output) :-
inverter(Input, Output) :-
	transistor(Input, ground, Output),
	transistor(Input, ground, Output),
	resistor(power, Output).
	resistor(power, Output).


nand_gate(Input1, Input2, Output) :-
nand_gate(Input1, Input2, Output) :-
	transistor(Input1, X, Output),
	transistor(Input1, X, Output),
	transistor(Input2, ground, X),
	transistor(Input2, ground, X),
	resistor(power, Output).
	resistor(power, Output).


and_gate(Input1, Input2, Output) :-
and_gate(Input1, Input2, Output) :-
	nand_gate(Input1, Input2, X), inverter(X, Output).
	nand_gate(Input1, Input2, X), inverter(X, Output).

```
<hr/>





[1]:https://github.com/konn/TAOP-Exercises 
[2]:http://jueqingsizhe66.github.io/blog/2015/08/09/allfilesinfoldstoonefile/ 
[3]:/images/prolog/goodTools.png 
[4]:/images/prolog/taop.png 
[5]:http://blog.csdn.net/hansel/article/details/8736775 
[6]:http://lab.madscience.nl/oo.sh.txt 
[7]:http://blog.longwin.com.tw/2011/09/bash-shell-oop-howto-2011/ 
[8]:http://jueqingsizhe66.github.io/blog/categories/linux/ 
