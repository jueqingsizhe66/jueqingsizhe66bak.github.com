---
layout: post
title: "99-problems-with-prolog(Running)"
date: 2016-07-28 21:33:17 +0800
comments: true
categories: prolog
---

The commit is only used for my reading codes in the 99-problems.

<!--more-->

You should watch some information  about the [99-problems][1].

+ [1. 1-Working-with-prolog-lists-P01/p01.pl](#1)
+ [2. 1-Working-with-prolog-lists-P01/p02.pl](#2)
+ [3. 1-Working-with-prolog-lists-P01/p03.pl](#3)
+ [4. 1-Working-with-prolog-lists-P01/p04.pl](#4)
+ [5. 1-Working-with-prolog-lists-P01/p05.pl](#5)
+ [6. 1-Working-with-prolog-lists-P01/p06.pl](#6)
+ [7. 1-Working-with-prolog-lists-P01/p07.pl](#7)
+ [8. 1-Working-with-prolog-lists-P01/p08.pl](#8)
+ [9. 1-Working-with-prolog-lists-P01/p09.pl](#9)
+ [10. 1-Working-with-prolog-lists-P01/p10.pl](#10)
+ [11. 1-Working-with-prolog-lists-P01/p11.pl](#11)
+ [12. 1-Working-with-prolog-lists-P01/p12.pl](#12)
+ [13. 1-Working-with-prolog-lists-P01/p13.pl](#13)
+ [14. 1-Working-with-prolog-lists-P01/p14.pl](#14)
+ [15. 1-Working-with-prolog-lists-P01/p15.pl](#15)
+ [16. 1-Working-with-prolog-lists-P01/p16.pl](#16)
+ [17. 1-Working-with-prolog-lists-P01/p17.pl](#17)
+ [18. 1-Working-with-prolog-lists-P01/p18.pl](#18)
+ [19. 1-Working-with-prolog-lists-P01/p19.pl](#19)
+ [20. 1-Working-with-prolog-lists-P01/p20.pl](#20)
+ [21. 1-Working-with-prolog-lists-P01/p21.pl](#21)
+ [22. 1-Working-with-prolog-lists-P01/p22.pl](#22)
+ [23. 1-Working-with-prolog-lists-P01/p23.pl](#23)
+ [24. 1-Working-with-prolog-lists-P01/p24.pl](#24)
+ [25. 1-Working-with-prolog-lists-P01/p25.pl](#25)
+ [26. 1-Working-with-prolog-lists-P01/p26.pl](#26)
+ [27. 1-Working-with-prolog-lists-P01/p27.pl](#27)
+ [28. 1-Working-with-prolog-lists-P01/p28.pl](#28)
+ [29. 2-Arithmetic-P31/p31.pl](#29)
+ [30. 2-Arithmetic-P31/p32.pl](#30)
+ [31. 2-Arithmetic-P31/p33.pl](#31)
+ [32. 2-Arithmetic-P31/p34.pl](#32)
+ [33. 2-Arithmetic-P31/p35.pl](#33)
+ [34. 2-Arithmetic-P31/p36.pl](#34)
+ [35. 2-Arithmetic-P31/p37.pl](#35)
+ [36. 2-Arithmetic-P31/p38.pl](#36)
+ [37. 2-Arithmetic-P31/p39.pl](#37)
+ [38. 2-Arithmetic-P31/p40.pl](#38)
+ [39. 2-Arithmetic-P31/p41.pl](#39)
+ [40. 3-Logic-and-Codes-P46/p46.pl](#40)
+ [41. 3-Logic-and-Codes-P46/p47.pl](#41)
+ [42. 3-Logic-and-Codes-P46/p48.pl](#42)
+ [43. 3-Logic-and-Codes-P46/p49.pl](#43)
+ [44. 3-Logic-and-Codes-P46/p49_gray.pl](#44)
+ [45. 3-Logic-and-Codes-P46/p50.pl](#45)
+ [46. 4-Binary-Trees-P54/p54.pl](#46)
+ [47. 4-Binary-Trees-P54/p55.pl](#47)
+ [48. 4-Binary-Trees-P54/p56.pl](#48)
+ [49. 4-Binary-Trees-P54/p57.pl](#49)
+ [50. 4-Binary-Trees-P54/p58.pl](#50)
+ [51. 4-Binary-Trees-P54/p59.pl](#51)
+ [52. 4-Binary-Trees-P54/p60.pl](#52)
+ [53. 4-Binary-Trees-P54/p61.pl](#53)
+ [54. 4-Binary-Trees-P54/p61A.pl](#54)
+ [55. 4-Binary-Trees-P54/p62.pl](#55)
+ [56. 4-Binary-Trees-P54/p62B.pl](#56)
+ [57. 4-Binary-Trees-P54/p63.pl](#57)
+ [58. 4-Binary-Trees-P54/p64.pl](#58)
+ [59. 4-Binary-Trees-P54/p65.pl](#59)
+ [60. 4-Binary-Trees-P54/p66.pl](#60)
+ [61. 4-Binary-Trees-P54/p67a.pl](#61)
+ [62. 4-Binary-Trees-P54/p67b.pl](#62)
+ [63. 4-Binary-Trees-P54/p68a.pl](#63)
+ [64. 4-Binary-Trees-P54/p68b.pl](#64)
+ [65. 4-Binary-Trees-P54/p68c.pl](#65)
+ [66. 4-Binary-Trees-P54/p68d.pl](#66)
+ [67. 4-Binary-Trees-P54/p69.pl](#67)
+ [68. 5-MultiwayTrees-P70B/p70.pl](#68)
+ [69. 5-MultiwayTrees-P70B/p70B.pl](#69)
+ [70. 5-MultiwayTrees-P70B/p70C.pl](#70)
+ [71. 5-MultiwayTrees-P70B/p71.pl](#71)
+ [72. 5-MultiwayTrees-P70B/p72.pl](#72)
+ [73. 5-MultiwayTrees-P70B/p73.pl](#73)
+ [74. 6-Graphs-P80/p80.pl](#74)
+ [75. 6-Graphs-P80/p81.pl](#75)
+ [76. 6-Graphs-P80/p82.pl](#76)
+ [77. 6-Graphs-P80/p83.pl](#77)
+ [78. 6-Graphs-P80/p83_minimum.pl](#78)
+ [79. 6-Graphs-P80/p84.pl](#79)
+ [80. 6-Graphs-P80/p85.pl](#80)
+ [81. 6-Graphs-P80/p86.pl](#81)
+ [82. 6-Graphs-P80/p87.pl](#82)
+ [83. 6-Graphs-P80/p87a.pl](#83)
+ [84. 6-Graphs-P80/p88.pl](#84)
+ [85. 6-Graphs-P80/p88a.pl](#85)
+ [86. 6-Graphs-P80/p89.pl](#86)
+ [87. 7-Miscellaneous-Problems-P90/p90.pl](#87)
+ [88. 7-Miscellaneous-Problems-P90/p91.pl](#88)
+ [89. 7-Miscellaneous-Problems-P90/p92.pl](#89)
+ [90. 7-Miscellaneous-Problems-P90/p93.pl](#90)
+ [91. 7-Miscellaneous-Problems-P90/p94.pl](#91)
+ [92. 7-Miscellaneous-Problems-P90/p95.pl](#92)
+ [93. 7-Miscellaneous-Problems-P90/p96.pl](#93)
+ [94. 7-Miscellaneous-Problems-P90/p96a.pl](#94)
+ [95. 7-Miscellaneous-Problems-P90/p97.pl](#95)
+ [96. 7-Miscellaneous-Problems-P90/p97S.pl](#96)
+ [97. 7-Miscellaneous-Problems-P90/p98.pl](#97)
+ [98. 7-Miscellaneous-Problems-P90/p99-readfile.pl](#98)
+ [99. 7-Miscellaneous-Problems-P90/p99.pl](#99)
+ [100. arith.pl/](#100)
+ [101. array.pl/](#101)
+ [102. assert.pl/](#102)
+ [103. binary_search_tree.pl/](#103)
+ [104. cbal_tree.pl/](#104)
+ [105. compare.pl/](#105)
+ [106. complete_binary_tree.pl/](#106)
+ [107. countleaves.pl/](#107)
+ [108. crossword.pl/](#108)
+ [109. doubts.pl/](#109)
+ [110. dropn.pl/](#110)
+ [111. eightqueens.pl/](#111)
+ [112. eliminate_consecutive_duplicates.pl/](#112)
+ [113. functor.pl/](#113)
+ [114. graph.pl/](#114)
+ [115. graph_dfo.pl/](#115)
+ [116. graph_match.pl/](#116)
+ [117. graph_paint.pl/](#117)
+ [118. graph_path.pl/](#118)
+ [119. group.pl/](#119)
+ [120. hbal_tree.pl/](#120)
+ [121. heap.pl/](#121)
+ [122. huffman.pl/](#122)
+ [123. identifier.pl/](#123)
+ [124. incompleteList.pl/](#124)
+ [125. insert_at.pl/](#125)
+ [126. istree.pl/](#126)
+ [127. knapsack.pl/](#127)
+ [128. knight_tour.pl/](#128)
+ [129. layout_binary_tree1.pl/](#129)
+ [130. leaves.pl/](#130)
+ [131. main.pl/](#131)
+ [132. multi_tree.pl/](#132)
+ [133. my-reverse.pl/](#133)
+ [134. mybagof.pl/](#134)
+ [135. mypack.pl/](#135)
+ [136. nonograms.pl/](#136)
+ [137. permute.pl/](#137)
+ [138. permute_pair.pl/](#138)
+ [139. polymorph.pl/](#139)
+ [140. qsort.pl/](#140)
+ [141. record.pl/](#141)
+ [142. regular.pl/](#142)
+ [143. remove_at.pl/](#143)
+ [144. rotate.pl/](#144)
+ [145. runlength.modified.pl/](#145)
+ [146. runlength.pl/](#146)
+ [147. runlengthdecode.pl/](#147)
+ [148. runlengthmodified.pl/](#148)
+ [149. scompress.pl/](#149)
+ [150. slice.pl/](#150)
+ [151. splitn.pl/](#151)
+ [152. sudoku.pl/](#152)
+ [153. sudoku_clpfd.pl/](#153)
+ [154. sunil_flatten.pl/](#154)
+ [155. sunil_reverse.pl/](#155)
+ [156. symmetricbtree.pl/](#156)
+ [157. test_p87.pl/](#157)
+ [158. top95.pl/](#158)
+ [159. treedot.pl/](#159)
+ [160. tree_order.pl/](#160)
+ [161. tree_string.pl/](#161)
+ [162. unify_alternate_elems.pl/](#162)
+ [163. unwind.pl/](#163)
+ [164. von_koch.pl/](#164)
+ [165. while.pl/](#165)
<br/>
<hr/>
<br/>
<h2 id="1"> 1. 1-Working-with-prolog-lists-P01/p01.pl</h2>

``` prolog
% P01 (*): Find the last element of a list

% my_last(X,L) :- X is the last element of the list L
%    (element,list) (?,?)

% Note: last(?Elem, ?List) is predefined

my_last(X,[X]).
my_last(X,[_|L]) :- my_last(X,L).

```


<br/>
<hr/>
<br/>
<h2 id="2"> 2. 1-Working-with-prolog-lists-P01/p02.pl</h2>

``` prolog
% P02 (*): Find the last but one element of a list

% last_but_one(X,L) :- X is the last but one element of the list L
%    (element,list) (?,?)

last_but_one(X,[X,_]).
last_but_one(X,[_,Y|Ys]) :- last_but_one(X,[Y|Ys]).

```


<br/>
<hr/>
<br/>
<h2 id="3"> 3. 1-Working-with-prolog-lists-P01/p03.pl</h2>

``` prolog
% P03 (*): Find the K'th element of a list.
% The first element in the list is number 1.

% element_at(X,L,K) :- X is the K'th element of the list L
%    (element,list,integer) (?,?,+)

% Note: nth1(?Index, ?List, ?Elem) is predefined

element_at(X,[X|_],1).
element_at(X,[_|L],K) :- K > 1, K1 is K - 1, element_at(X,L,K1).

```


<br/>
<hr/>
<br/>
<h2 id="4"> 4. 1-Working-with-prolog-lists-P01/p04.pl</h2>

``` prolog
% P04 (*): Find the number of elements of a list.

% my_length(L,N) :- the list L contains N elements
%    (list,integer) (+,?) 

% Note: length(?List, ?Int) is predefined

my_length([],0).
my_length([_|L],N) :- my_length(L,N1), N is N1 + 1.

```


<br/>
<hr/>
<br/>
<h2 id="5"> 5. 1-Working-with-prolog-lists-P01/p05.pl</h2>

``` prolog
% P05 (*): Reverse a list.

% my_reverse(L1,L2) :- L2 is the list obtained from L1 by reversing 
%    the order of the elements.
%    (list,list) (?,?)

% Note: reverse(+List1, -List2) is predefined

my_reverse(L1,L2) :- my_rev(L1,L2,[]).

my_rev([],L2,L2) :- !.
my_rev([X|Xs],L2,Acc) :- my_rev(Xs,L2,[X|Acc]).


```


<br/>
<hr/>
<br/>
<h2 id="6"> 6. 1-Working-with-prolog-lists-P01/p06.pl</h2>

``` prolog
% P06 (*): Find out whether a list is a palindrome
% A palindrome can be read forward or backward; e.g. [x,a,m,a,x]

% is_palindrome(L) :- L is a palindrome list
%    (list) (?)

is_palindrome(L) :- reverse(L,L).


```


<br/>
<hr/>
<br/>
<h2 id="7"> 7. 1-Working-with-prolog-lists-P01/p07.pl</h2>

``` prolog
% P07 (**): Flatten a nested list structure.

% my_flatten(L1,L2) :- the list L2 is obtained from the list L1 by
%    flattening; i.e. if an element of L1 is a list then it is replaced
%    by its elements, recursively. 
%    (list,list) (+,?)

% Note: flatten(+List1, -List2) is a predefined predicate

my_flatten(X,[X]) :- \+ is_list(X).
my_flatten([],[]).
my_flatten([X|Xs],Zs) :- my_flatten(X,Y), my_flatten(Xs,Ys), append(Y,Ys,Zs).


```


<br/>
<hr/>
<br/>
<h2 id="8"> 8. 1-Working-with-prolog-lists-P01/p08.pl</h2>

``` prolog
% P08 (**): Eliminate consecutive duplicates of list elements.

% compress(L1,L2) :- the list L2 is obtained from the list L1 by
%    compressing repeated occurrences of elements into a single copy
%    of the element.
%    (list,list) (+,?)

compress([],[]).
compress([X],[X]).
compress([X,X|Xs],Zs) :- compress([X|Xs],Zs).
compress([X,Y|Ys],[X|Zs]) :- X \= Y, compress([Y|Ys],Zs).

```


<br/>
<hr/>
<br/>
<h2 id="9"> 9. 1-Working-with-prolog-lists-P01/p09.pl</h2>

``` prolog
% P09 (**):  Pack consecutive duplicates of list elements into sublists.

% pack(L1,L2) :- the list L2 is obtained from the list L1 by packing
%    repeated occurrences of elements into separate sublists.
%    (list,list) (+,?)

pack([],[]).
pack([X|Xs],[Z|Zs]) :- transfer(X,Xs,Ys,Z), pack(Ys,Zs).

% transfer(X,Xs,Ys,Z) Ys is the list that remains from the list Xs
%    when all leading copies of X are removed and transfered to Z

transfer(X,[],[],[X]).
transfer(X,[Y|Ys],[Y|Ys],[X]) :- X \= Y.
transfer(X,[X|Xs],Ys,[X|Zs]) :- transfer(X,Xs,Ys,Zs).


```


<br/>
<hr/>
<br/>
<h2 id="10"> 10. 1-Working-with-prolog-lists-P01/p10.pl</h2>

``` prolog
% P10 (*):  Run-length encoding of a list

% encode(L1,L2) :- the list L2 is obtained from the list L1 by run-length
%    encoding. Consecutive duplicates of elements are encoded as terms [N,E],
%    where N is the number of duplicates of the element E.
%    (list,list) (+,?)

:- ensure_loaded(p09).

encode(L1,L2) :- pack(L1,L), transform(L,L2).

transform([],[]).
transform([[X|Xs]|Ys],[[N,X]|Zs]) :- length([X|Xs],N), transform(Ys,Zs).

```


<br/>
<hr/>
<br/>
<h2 id="11"> 11. 1-Working-with-prolog-lists-P01/p11.pl</h2>

``` prolog
% P11 (*):  Modified run-length encoding

% encode_modified(L1,L2) :- the list L2 is obtained from the list L1 by 
%    run-length encoding. Consecutive duplicates of elements are encoded 
%    as terms [N,E], where N is the number of duplicates of the element E.
%    However, if N equals 1 then the element is simply copied into the 
%    output list.
%    (list,list) (+,?)

:- ensure_loaded(p10).

encode_modified(L1,L2) :- encode(L1,L), strip(L,L2).

strip([],[]).
strip([[1,X]|Ys],[X|Zs]) :- strip(Ys,Zs).
strip([[N,X]|Ys],[[N,X]|Zs]) :- N > 1, strip(Ys,Zs).

```


<br/>
<hr/>
<br/>
<h2 id="12"> 12. 1-Working-with-prolog-lists-P01/p12.pl</h2>

``` prolog
% P12 (**): Decode a run-length compressed list.

% decode(L1,L2) :- L2 is the uncompressed version of the run-length
%    encoded list L1.
%    (list,list) (+,?)

decode([],[]).
decode([X|Ys],[X|Zs]) :- \+ is_list(X), decode(Ys,Zs).
decode([[1,X]|Ys],[X|Zs]) :- decode(Ys,Zs).
decode([[N,X]|Ys],[X|Zs]) :- N > 1, N1 is N - 1, decode([[N1,X]|Ys],Zs).

```


<br/>
<hr/>
<br/>
<h2 id="13"> 13. 1-Working-with-prolog-lists-P01/p13.pl</h2>

``` prolog
% P13 (**): Run-length encoding of a list (direct solution) 

% encode_direct(L1,L2) :- the list L2 is obtained from the list L1 by 
%    run-length encoding. Consecutive duplicates of elements are encoded 
%    as terms [N,E], where N is the number of duplicates of the element E.
%    However, if N equals 1 then the element is simply copied into the 
%    output list.
%    (list,list) (+,?)

encode_direct([],[]).
encode_direct([X|Xs],[Z|Zs]) :- count(X,Xs,Ys,1,Z), encode_direct(Ys,Zs).

% count(X,Xs,Ys,K,T) Ys is the list that remains from the list Xs
%    when all leading copies of X are removed. T is the term [N,X],
%    where N is K plus the number of X's that can be removed from Xs.
%    In the case of N=1, T is X, instead of the term [1,X].

count(X,[],[],1,X).
count(X,[],[],N,[N,X]) :- N > 1.
count(X,[Y|Ys],[Y|Ys],1,X) :- X \= Y.
count(X,[Y|Ys],[Y|Ys],N,[N,X]) :- N > 1, X \= Y.
count(X,[X|Xs],Ys,K,T) :- K1 is K + 1, count(X,Xs,Ys,K1,T).


```


<br/>
<hr/>
<br/>
<h2 id="14"> 14. 1-Working-with-prolog-lists-P01/p14.pl</h2>

``` prolog
% P14 (*): Duplicate the elements of a list

% dupli(L1,L2) :- L2 is obtained from L1 by duplicating all elements.
%    (list,list) (?,?)

dupli([],[]).
dupli([X|Xs],[X,X|Ys]) :- dupli(Xs,Ys).

```


<br/>
<hr/>
<br/>
<h2 id="15"> 15. 1-Working-with-prolog-lists-P01/p15.pl</h2>

``` prolog
% P15 (**): Duplicate the elements of a list agiven number of times

% dupli(L1,N,L2) :- L2 is obtained from L1 by duplicating all elements
%    N times.
%    (list,integer,list) (?,+,?)

dupli(L1,N,L2) :- dupli(L1,N,L2,N).

% dupli(L1,N,L2,K) :- L2 is obtained from L1 by duplicating its leading
%    element K times, all other elements N times.
%    (list,integer,list,integer) (?,+,?,+)

dupli([],_,[],_).
dupli([_|Xs],N,Ys,0) :- dupli(Xs,N,Ys,N).
dupli([X|Xs],N,[X|Ys],K) :- K > 0, K1 is K - 1, dupli([X|Xs],N,Ys,K1).

```


<br/>
<hr/>
<br/>
<h2 id="16"> 16. 1-Working-with-prolog-lists-P01/p16.pl</h2>

``` prolog
% P16 (**):  Drop every N'th element from a list

% drop(L1,N,L2) :- L2 is obtained from L1 by dropping every N'th element.
%    (list,integer,list) (?,+,?)

drop(L1,N,L2) :- drop(L1,N,L2,N).

% drop(L1,N,L2,K) :- L2 is obtained from L1 by first copying K-1 elements
%    and then dropping an element and, from then on, dropping every
%    N'th element.
%    (list,integer,list,integer) (?,+,?,+)

drop([],_,[],_).
drop([_|Xs],N,Ys,1) :- drop(Xs,N,Ys,N).
drop([X|Xs],N,[X|Ys],K) :- K > 1, K1 is K - 1, drop(Xs,N,Ys,K1).


```


<br/>
<hr/>
<br/>
<h2 id="17"> 17. 1-Working-with-prolog-lists-P01/p17.pl</h2>

``` prolog
% P17 (*): Split a list into two parts

% split(L,N,L1,L2) :- the list L1 contains the first N elements
%    of the list L, the list L2 contains the remaining elements.
%    (list,integer,list,list) (?,+,?,?)

split(L,0,[],L).
split([X|Xs],N,[X|Ys],Zs) :- N > 0, N1 is N - 1, split(Xs,N1,Ys,Zs).


```


<br/>
<hr/>
<br/>
<h2 id="18"> 18. 1-Working-with-prolog-lists-P01/p18.pl</h2>

``` prolog
% P18 (**):  Extract a slice from a list

% slice(L1,I,K,L2) :- L2 is the list of the elements of L1 between
%    index I and index K (both included).
%    (list,integer,integer,list) (?,+,+,?)

slice([X|_],1,1,[X]).
slice([X|Xs],1,K,[X|Ys]) :- K > 1, 
   K1 is K - 1, slice(Xs,1,K1,Ys).
slice([_|Xs],I,K,Ys) :- I > 1, 
   I1 is I - 1, K1 is K - 1, slice(Xs,I1,K1,Ys).

```


<br/>
<hr/>
<br/>
<h2 id="19"> 19. 1-Working-with-prolog-lists-P01/p19.pl</h2>

``` prolog
% P19 (**): Rotate a list N places to the left 

% rotate(L1,N,L2) :- the list L2 is obtained from the list L1 by 
%    rotating the elements of L1 N places to the left.
%    Examples: 
%    rotate([a,b,c,d,e,f,g,h],3,[d,e,f,g,h,a,b,c])
%    rotate([a,b,c,d,e,f,g,h],-2,[g,h,a,b,c,d,e,f])
%    (list,integer,list) (+,+,?)

:- ensure_loaded(p17).

rotate(L1,N,L2) :- N >= 0, 
   length(L1,NL1), N1 is N mod NL1, rotate_left(L1,N1,L2).
rotate(L1,N,L2) :- N < 0,
   length(L1,NL1), N1 is NL1 + (N mod NL1), rotate_left(L1,N1,L2).

rotate_left(L,0,L).
rotate_left(L1,N,L2) :- N > 0, split(L1,N,S1,S2), append(S2,S1,L2).





```


<br/>
<hr/>
<br/>
<h2 id="20"> 20. 1-Working-with-prolog-lists-P01/p20.pl</h2>

``` prolog
% P20 (*): Remove the K'th element from a list.
% The first element in the list is number 1.

% remove_at(X,L,K,R) :- X is the K'th element of the list L; R is the
%    list that remains when the K'th element is removed from L.
%    (element,list,integer,list) (?,?,+,?)

remove_at(X,[X|Xs],1,Xs).
remove_at(X,[Y|Xs],K,[Y|Ys]) :- K > 1, 
   K1 is K - 1, remove_at(X,Xs,K1,Ys).

```


<br/>
<hr/>
<br/>
<h2 id="21"> 21. 1-Working-with-prolog-lists-P01/p21.pl</h2>

``` prolog
% P21 (*): Insert an element at a given position into a list
% The first element in the list is number 1.

% insert_at(X,L,K,R) :- X is inserted into the list L such that it
%    occupies position K. The result is the list R.
%    (element,list,integer,list) (?,?,+,?)

:- ensure_loaded(p20).

insert_at(X,L,K,R) :- remove_at(X,R,K,L).


```


<br/>
<hr/>
<br/>
<h2 id="22"> 22. 1-Working-with-prolog-lists-P01/p22.pl</h2>

``` prolog
% P22 (*):  Create a list containing all integers within a given range.

% range(I,K,L) :- I <= K, and L is the list containing all 
%    consecutive integers from I to K.
%    (integer,integer,list) (+,+,?)

range(I,I,[I]).
range(I,K,[I|L]) :- I < K, I1 is I + 1, range(I1,K,L).

```


<br/>
<hr/>
<br/>
<h2 id="23"> 23. 1-Working-with-prolog-lists-P01/p23.pl</h2>

``` prolog
% P23 (**): Extract a given number of randomly selected elements 
%    from a list.

% rnd_select(L,N,R) :- the list R contains N randomly selected 
%    items taken from the list L.
%    (list,integer,list) (+,+,-)

:- ensure_loaded(p20).

rnd_select(_,0,[]).
rnd_select(Xs,N,[X|Zs]) :- N > 0,
    length(Xs,L),
    % L is L +1,
    %I is random(L) + 1,
    %I is random(1,L) + 1,
    %L should below length(Xs).
    random(1,L,I),
    remove_at(X,Xs,I,Ys),
    N1 is N - 1,
    rnd_select(Ys,N1,Zs).

```


<br/>
<hr/>
<br/>
<h2 id="24"> 24. 1-Working-with-prolog-lists-P01/p24.pl</h2>

``` prolog
% P24 (*): Lotto: Draw N different random numbers from the set 1..M

% lotto(N,M,L) :- the list L contains N randomly selected distinct
%    integer numbers from the interval 1..M
%    (integer,integer,number-list) (+,+,-)

:- ensure_loaded(p22).
:- ensure_loaded(p23).

lotto(N,M,L) :- range(1,M,R), rnd_select(R,N,L).

```


<br/>
<hr/>
<br/>
<h2 id="25"> 25. 1-Working-with-prolog-lists-P01/p25.pl</h2>

``` prolog
% P25 (*):  Generate a random permutation of the elements of a list

% rnd_permu(L1,L2) :- the list L2 is a random permutation of the
%    elements of the list L1.
%    (list,list) (+,-)

:- ensure_loaded(p23).

%rnd_permu(L1,L2) :- length(L1,N), N is N+1,rnd_select(L1,N,L2).
rnd_permu(L1,L2) :- length(L1,N),Np is N-1, rnd_select(L1,Np,L2).

```


<br/>
<hr/>
<br/>
<h2 id="26"> 26. 1-Working-with-prolog-lists-P01/p26.pl</h2>

``` prolog
% P26 (**):  Generate the combinations of k distinct objects
%            chosen from the n elements of a list.

% combination(K,L,C) :- C is a list of K distinct elements 
%    chosen from the list L

combination(0,_,[]).
combination(K,L,[X|Xs]) :- el(X,L,R),K1 is K-1, combination(K1,R,Xs).

% Find out what the following predicate el/3 exactly does.

el(X,[X|L],L).
el(X,[_|L],R) :- el(X,L,R).

```


<br/>
<hr/>
<br/>
<h2 id="27"> 27. 1-Working-with-prolog-lists-P01/p27.pl</h2>

``` prolog
% P27 (**) Group the elements of a set into disjoint subsets.

% Problem a)

% group3(G,G1,G2,G3) :- distribute the 9 elements of G into G1, G2, and G3,
%    such that G1, G2 and G3 contain 2,3 and 4 elements respectively

group3(G,G1,G2,G3) :- 
   selectN(2,G,G1),
   subtract(G,G1,R1),
   selectN(3,R1,G2),
   subtract(R1,G2,R2),
   selectN(4,R2,G3),
   subtract(R2,G3,[]).

% selectN(N,L,S) :- select N elements of the list L and put them in 
%    the set S. Via backtracking return all posssible selections, but
%    avoid permutations; i.e. after generating S = [a,b,c] do not return
%    S = [b,a,c], etc.

selectN(0,_,[]) :- !.
selectN(N,L,[X|S]) :- N > 0, 
   e2(X,L,R), 
   N1 is N-1,
   selectN(N1,R,S).

e2(X,[X|L],L).
e2(X,[_|L],R) :- e2(X,L,R).

% subtract/3 is predefined

% Problem b): Generalization

% group(G,Ns,Gs) :- distribute the elements of G into the groups Gs.
%    The group sizes are given in the list Ns.

group([],[],[]).
group(G,[N1|Ns],[G1|Gs]) :- 
   selectN(N1,G,G1),
   subtract(G,G1,R),
   group(R,Ns,Gs).

```


<br/>
<hr/>
<br/>
<h2 id="28"> 28. 1-Working-with-prolog-lists-P01/p28.pl</h2>

``` prolog
% P28 (**) Sorting a list of lists according to length
%
% a) length sort
%
% lsort(InList,OutList) :- it is supposed that the elements of InList 
% are lists themselves. Then OutList is obtained from InList by sorting 
% its elements according to their length. lsort/2 sorts ascendingly,
% lsort/3 allows for ascending or descending sorts.
% (list_of_lists,list_of_lists), (+,?)

lsort(InList,OutList) :- lsort(InList,OutList,asc).

% sorting direction Dir is either asc or desc

lsort(InList,OutList,Dir) :-
	add_key(InList,KList,Dir),
	keysort(KList,SKList),
	rem_key(SKList,OutList).

add_key([],[],_).
add_key([X|Xs],[L-p(X)|Ys],asc) :-!, 
	length(X,L), add_key(Xs,Ys,asc).
add_key([X|Xs],[L-p(X)|Ys],desc) :- 
	length(X,L1), L is -L1, add_key(Xs,Ys,desc).

rem_key([],[]).
rem_key([_-p(X)|Xs],[X|Ys]) :- rem_key(Xs,Ys).

% b) length frequency sort
%
% lfsort (InList,OutList) :- it is supposed that the elements of InList
% are lists themselves. Then OutList is obtained from InList by sorting
% its elements according to their length frequency; i.e. in the default,
% where sorting is done ascendingly, lists with rare lengths are placed
% first, other with more frequent lengths come later.
%
% Example:
% ?- lfsort([[a,b,c],[d,e],[f,g,h],[d,e],[i,j,k,l],[m,n],[o]],L).
% L = [[i, j, k, l], [o], [a, b, c], [f, g, h], [d, e], [d, e], [m, n]]
%
% Note that the first two lists in the Result have length 4 and 1, both
% length appear just once. The third and forth list have length 3 which
% appears, there are two list of this length. And finally, the last
% three lists have length 2. This is the most frequent length.

lfsort(InList,OutList) :- lfsort(InList,OutList,asc).

% sorting direction Dir is either asc or desc

lfsort(InList,OutList,Dir) :-
	add_key(InList,KList,desc),
	keysort(KList,SKList),
	pack1(SKList,PKList),
	lsort(PKList,SPKList,Dir),
	flatten(SPKList,FKList),
	rem_key(FKList,OutList).

pack1([],[]).
pack1([L-X|Xs],[[L-X|Z]|Zs]) :- transf(L-X,Xs,Ys,Z), pack1(Ys,Zs).

% transf(L-X,Xs,Ys,Z) Ys is the list that remains from the list Xs
%    when all leading copies of length L are removed and transfed to Z

transf(_,[],[],[]).
transf(L-_,[K-Y|Ys],[K-Y|Ys],[]) :- L \= K.
transf(L-_,[L-X|Xs],Ys,[L-X|Zs]) :- transf(L-X,Xs,Ys,Zs).

test :-
   L = [[a,b,c],[d,e],[f,g,h],[d,e],[i,j,k,l],[m,n],[o]],
   write('L = '), write(L), nl,
   lsort(L,LS),
   write('LS = '), write(LS), nl,
   lsort(L,LSD,desc),
   write('LSD = '), write(LSD), nl,
   lfsort(L,LFS),
   write('LFS = '), write(LFS), nl.

```


<br/>
<hr/>
<br/>
<h2 id="29"> 29. 2-Arithmetic-P31/p31.pl</h2>

``` prolog
% P31 (**) Determine whether a given integer number is prime. 

% is_prime(P) :- P is a prime number
%    (integer) (+)

is_prime(2).
is_prime(3).
is_prime(P) :- integer(P), P > 3, P mod 2 =\= 0, \+ has_factor(P,3).  

% has_factor(N,L) :- N has an odd factor F >= L.
%    (integer, integer) (+,+)

has_factor(N,L) :- N mod L =:= 0.
has_factor(N,L) :- L * L < N, L2 is L + 2, has_factor(N,L2).

```


<br/>
<hr/>
<br/>
<h2 id="30"> 30. 2-Arithmetic-P31/p32.pl</h2>

``` prolog
% P32 (**) Determine the greatest common divisor of two positive integers.

% gcd(X,Y,G) :- G is the greatest common divisor of X and Y
%    (integer, integer, integer) (+,+,?)


gcd(X,0,X) :- X > 0.
gcd(X,Y,G) :- Y > 0, Z is X mod Y, gcd(Y,Z,G).


% Declare gcd as an arithmetic function; so you can use it
% like this:  ?- G is gcd(36,63).

%:- arithmetic_function(gcd/2).
:- arithmetic_function(gcd/2).

```


<br/>
<hr/>
<br/>
<h2 id="31"> 31. 2-Arithmetic-P31/p33.pl</h2>

``` prolog
% P33 (*) Determine whether two positive integer numbers are coprime. 
%     Two numbers are coprime if their greatest common divisor equals 1.

% coprime(X,Y) :- X and Y are coprime.
%    (integer, integer) (+,+)

:- ensure_loaded(p32).

coprime(X,Y) :- gcd(X,Y,1).

```


<br/>
<hr/>
<br/>
<h2 id="32"> 32. 2-Arithmetic-P31/p34.pl</h2>

``` prolog
% P34 (**) Calculate Euler's totient function phi(m). 
%    Euler's so-called totient function phi(m) is defined as the number 
%    of positive integers r (1 <= r < m) that are coprime to m. 
%    Example: m = 10: r = 1,3,7,9; thus phi(m) = 4. Note: phi(1) = 1.

% totient_phi(M,Phi) :- Phi is the value of the Euler's totient function
%    phi for the argument M.
%    (integer, integer) (+,-)

:- ensure_loaded(p33).
:- arithmetic_function(totient_phi/1).

totient_phi(1,1) :- !.
totient_phi(M,Phi) :- t_phi(M,Phi,1,0).

% t_phi(M,Phi,K,C) :- Phi = C + N, where N is the number of integers R
%    such that K <= R < M and R is coprime to M.
%    (integer,integer,integer,integer) (+,-,+,+)

t_phi(M,Phi,M,Phi) :- !.
t_phi(M,Phi,K,C) :- 
   K < M, coprime(K,M), !, 
   C1 is C + 1, K1 is K + 1,
   t_phi(M,Phi,K1,C1).
t_phi(M,Phi,K,C) :- 
   K < M, K1 is K + 1,
   t_phi(M,Phi,K1,C).

```


<br/>
<hr/>
<br/>
<h2 id="33"> 33. 2-Arithmetic-P31/p35.pl</h2>

``` prolog
% P35 (**) Determine the prime factors of a given positive integer. 

% prime_factors(N, L) :- N is the list of prime factors of N.
%    (integer,list) (+,?)

prime_factors(N,L) :- N > 0,  prime_factors(N,L,2).

% prime_factors(N,L,K) :- L is the list of prime factors of N. It is 
% known that N does not have any prime factors less than K.

prime_factors(1,[],_) :- !.
prime_factors(N,[F|L],F) :-                           % N is multiple of F
   R is N // F, N =:= R * F, !, prime_factors(R,L,F).
prime_factors(N,L,F) :- 
   next_factor(N,F,NF), prime_factors(N,L,NF).        % N is not multiple of F
   

% next_factor(N,F,NF) :- when calculating the prime factors of N
%    and if F does not divide N then NF is the next larger candidate to
%    be a factor of N.

next_factor(_,2,3) :- !.
next_factor(N,F,NF) :- F * F < N, !, NF is F + 2.
next_factor(N,_,N).                                 % F > sqrt(N)

```


<br/>
<hr/>
<br/>
<h2 id="34"> 34. 2-Arithmetic-P31/p36.pl</h2>

``` prolog
% P36 (**) Determine the prime factors of a given positive integer (2). 
% Construct a list containing the prime factors and their multiplicity.
% Example: 
% ?- prime_factors_mult(315, L).
% L = [[3,2],[5,1],[7,1]]

:- ensure_loaded(p35).  % make sure next_factor/3 is loaded

% prime_factors_mult(N, L) :- L is the list of prime factors of N. It is
%    composed of terms [F,M] where F is a prime factor and M its multiplicity.
%    (integer,list) (+,?)

prime_factors_mult(N,L) :- N > 0, prime_factors_mult(N,L,2).

% prime_factors_mult(N,L,K) :- L is the list of prime factors of N. It is 
% known that N does not have any prime factors less than K.

prime_factors_mult(1,[],_) :- !.
prime_factors_mult(N,[[F,M]|L],F) :- divide(N,F,M,R), !, % F divides N
   next_factor(R,F,NF), prime_factors_mult(R,L,NF).
prime_factors_mult(N,L,F) :- !,                          % F does not divide N
   next_factor(N,F,NF), prime_factors_mult(N,L,NF).

% divide(N,F,M,R) :- N = R * F**M, M >= 1, and F is not a factor of R. 
%    (integer,integer,integer,integer) (+,+,-,-)

divide(N,F,M,R) :- divi(N,F,M,R,0), M > 0.

divi(N,F,M,R,K) :- S is N // F, N =:= S * F, !,          % F divides N
   K1 is K + 1, divi(S,F,M,R,K1).
divi(N,_,M,N,M).


```


<br/>
<hr/>
<br/>
<h2 id="35"> 35. 2-Arithmetic-P31/p37.pl</h2>

``` prolog
% P37 (**) Calculate Euler's totient function phi(m) (improved). 
% See problem P34 for the definition of Euler's totient function. 
% If the list of the prime factors of a number m is known in the 
% form of problem P36 then the function phi(m) can be efficiently
% calculated as follows: 
%
% Let [[p1,m1],[p2,m2],[p3,m3],...] be the list of prime factors (and their
% multiplicities) of a given number m. Then phi(m) can be calculated 
% with the following formula:
%
% phi(m) = (p1 - 1) * p1 ** (m1 - 1) * (p2 - 1) * p2 ** (m2 - 1) * 
%          (p3 - 1) * p3 ** (m3 - 1) * ...
%
% Note that a ** b stands for the b'th power of a.

:- ensure_loaded(p36).

% totient_phi_2(N,Phi) :- Phi is the value of Euler's totient function
%    for the argument N.
%    (integer,integer) (+,?)

totient_phi_2(N,Phi) :- prime_factors_mult(N,L), to_phi(L,Phi).

to_phi([],1).
to_phi([[F,1]|L],Phi) :- !,
   to_phi(L,Phi1), Phi is Phi1 * (F - 1).
to_phi([[F,M]|L],Phi) :- M > 1,
   M1 is M - 1, to_phi([[F,M1]|L],Phi1), Phi is Phi1 * F.

```


<br/>
<hr/>
<br/>
<h2 id="36"> 36. 2-Arithmetic-P31/p38.pl</h2>

``` prolog
% P38 (*) Compare the two methods of calculating Euler's totient function. 
% Use the solutions of problems P34 and P37 to compare the algorithms. 
% Take the number of logical inferences as a measure for efficiency.

:- ensure_loaded(p34).
:- ensure_loaded(p37).

totient_test(N) :-
   write('totient_phi (P34):'),
   time(totient_phi(N,Phi1)),
   write('result = '), write(Phi1), nl,
   write('totient_phi_2 (P37):'),
   time(totient_phi_2(N,Phi2)),
   write('result = '), write(Phi2), nl.


```


<br/>
<hr/>
<br/>
<h2 id="37"> 37. 2-Arithmetic-P31/p39.pl</h2>

``` prolog
% P39 (*) A list of prime numbers. 
% Given a range of integers by its lower and upper limit, construct a 
% list of all prime numbers in that range.

:- ensure_loaded(p31).   % make sure is_prime/1 is loaded

% prime_list(A,B,L) :- L is the list of prime number P with A <= P <= B

prime_list(A,B,L) :- A =< 2, !, p_list(2,B,L).
prime_list(A,B,L) :- A1 is (A // 2) * 2 + 1, p_list(A1,B,L).

p_list(A,B,[]) :- A > B, !.
p_list(A,B,[A|L]) :- is_prime(A), !, 
   next(A,A1), p_list(A1,B,L). 
p_list(A,B,L) :- 
   next(A,A1), p_list(A1,B,L).

next(2,3) :- !.
next(A,A1) :- A1 is A + 2.

```


<br/>
<hr/>
<br/>
<h2 id="38"> 38. 2-Arithmetic-P31/p40.pl</h2>

``` prolog
% P40 (**) Goldbach's conjecture. 
% Goldbach's conjecture says that every positive even number greater 
% than 2 is the sum of two prime numbers. Example: 28 = 5 + 23.

:- ensure_loaded(p31).

% goldbach(N,L) :- L is the list of the two prime numbers that
%    sum up to the given N (which must be even).
%    (integer,integer) (+,-)

goldbach(4,[2,2]) :- !.
goldbach(N,L) :- N mod 2 =:= 0, N > 4, goldbach(N,L,3).

goldbach(N,[P,Q],P) :- Q is N - P, is_prime(Q), !.
goldbach(N,L,P) :- P < N, next_prime(P,P1), goldbach(N,L,P1).

next_prime(P,P1) :- P1 is P + 2, is_prime(P1), !.
next_prime(P,P1) :- P2 is P + 2, next_prime(P2,P1).


```


<br/>
<hr/>
<br/>
<h2 id="39"> 39. 2-Arithmetic-P31/p41.pl</h2>

``` prolog
% P41 (*) A list of Goldbach compositions. 
% Given a range of integers by its lower and upper limit, 
% print a list of all even numbers and their Goldbach composition.

:- ensure_loaded(p40).

% goldbach_list(A,B) :- print a list of the Goldbach composition
%    of all even numbers N in the range A <= N <= B
%    (integer,integer) (+,+)

goldbach_list(A,B) :- goldbach_list(A,B,2).

% goldbach_list(A,B,L) :- perform goldbach_list(A,B), but suppress
% all output when the first prime number is less than the limit L.

goldbach_list(A,B,L) :- A =< 4, !, g_list(4,B,L).
goldbach_list(A,B,L) :- A1 is ((A+1) // 2) * 2, g_list(A1,B,L).

g_list(A,B,_) :- A > B, !.
g_list(A,B,L) :- 
   goldbach(A,[P,Q]),
   print_goldbach(A,P,Q,L),
   A2 is A + 2,
   g_list(A2,B,L).

print_goldbach(A,P,Q,L) :- P >= L, !,
   writef('%t = %t + %t',[A,P,Q]), nl.
print_goldbach(_,_,_,_).


```


<br/>
<hr/>
<br/>
<h2 id="40"> 40. 3-Logic-and-Codes-P46/p46.pl</h2>

``` prolog
% P46 (**) Truth tables for logical expressions.
% Define predicates and/2, or/2, nand/2, nor/2, xor/2, impl/2 
% and equ/2 (for logical equivalence) which succeed or
% fail according to the result of their respective operations; e.g.
% and(A,B) will succeed, if and only if both A and B succeed.
% Note that A and B can be Prolog goals (not only the constants
% true and fail).
% A logical expression in two variables can then be written in 
% prefix notation, as in the following example: and(or(A,B),nand(A,B)).
%
% Now, write a predicate table/3 which prints the truth table of a
% given logical expression in two variables.
%
% Example:
% ?- table(A,B,and(A,or(A,B))).
% true  true  true
% true  fail  true
% fail  true  fail
% fail  fail  fail
    
and(A,B) :- A, B.

or(A,_) :- A.
or(_,B) :- B.

equ(A,B) :- or(and(A,B), and(not(A),not(B))).

xor(A,B) :- not(equ(A,B)).

nor(A,B) :- not(or(A,B)).

nand(A,B) :- not(and(A,B)).

impl(A,B) :- or(not(A),B).

% bind(X) :- instantiate X to be true and false successively

bind(true).
bind(fail).

table(A,B,Expr) :-
	bind(A), bind(B),
	write('-----------------------------'),nl,
	mydo(A,B,Expr), fail.

mydo(A,B,Expr):-write(A),write(' '),write(B),write(' '),writeValueOf(Expr),nl.

writeValueOf(Expr):-Expr,write(itsTrue),nl.
writeValueOf(Expr):-not(Expr),write(itsFalse),nl.









```


<br/>
<hr/>
<br/>
<h2 id="41"> 41. 3-Logic-and-Codes-P46/p47.pl</h2>

``` prolog
% P47 (*) Truth tables for logical expressions (2).
% Continue problem P46 by defining and/2, or/2, etc as being
% operators. This allows to write the logical expression in the
% more natural way, as in the example: A and (A or not B).
% Define operator precedence as usual; i.e. as in Java.
%
% Example:
% ?- table(A,B, A and (A or not B)).
% true  true  true
% true  fail  true
% fail  true  fail
% fail  fail  fail
    
:- ensure_loaded(p46).

:- op(900, fy,not).
:- op(910, yfx, and).
:- op(910, yfx, nand).
:- op(920, yfx, or).
:- op(920, yfx, nor).
:- op(930, yfx, impl).
:- op(930, yfx, equ).
:- op(930, yfx, xor).

% I.e. not binds stronger than (and, nand), which bind stronger than
% (or,nor) which in turn bind stronger than implication, equivalence
% and xor.

```


<br/>
<hr/>
<br/>
<h2 id="42"> 42. 3-Logic-and-Codes-P46/p48.pl</h2>

``` prolog
% P48 (**) Truth tables for logical expressions (3).
% Generalize problem P47 in such a way that the logical
% expression may contain any number of logical variables.
%
% Example:
% ?- table([A,B,C], A and (B or C) equ A and B or A and C).
% true  true  true  true
% true  true  fail  true
% true  fail  true  true
% true  fail  fail  true
% fail  true  true  true
% fail  true  fail  true
% fail  fail  true  true
% fail  fail  fail  true

:- ensure_loaded(p47).    

% table(List,Expr) :- print the truth table for the expression Expr,
%   which contains the logical variables enumerated in List.

table(VarList,Expr) :- bindList(VarList), do(VarList,Expr), fail.

bindList([]).
bindList([V|Vs]) :- bind(V), bindList(Vs).

do(VarList,Expr) :- writeVarList(VarList), writeExpr(Expr), nl.

writeVarList([]).
writeVarList([V|Vs]) :- write(V), write('  '), writeVarList(Vs).

writeExpr(Expr) :- Expr, !, write(true).
writeExpr(_) :- write(fail).

```


<br/>
<hr/>
<br/>
<h2 id="43"> 43. 3-Logic-and-Codes-P46/p49.pl</h2>

``` prolog
% (**) P49 Gray codes

% gray(N,C) :- C is the N-bit Gray code

gray(1,['0','1']).
gray(N,C) :- N > 1, N1 is N-1,
   gray(N1,C1), reverse(C1,C2),
   prepend('0',C1,C1P),
   prepend('1',C2,C2P),
   append(C1P,C2P,C).

prepend(_,[],[]) :- !.
prepend(X,[C|Cs],[CP|CPs]) :- atom_concat(X,C,CP), prepend(X,Cs,CPs).


% This gives a nice example for the result caching technique:

:- dynamic gray_c/2.

gray_c(1,['0','1']) :- !.
gray_c(N,C) :- N > 1, N1 is N-1, 
   gray_c(N1,C1), reverse(C1,C2),
   prepend('0',C1,C1P),
   prepend('1',C2,C2P),
   append(C1P,C2P,C),
   asserta((gray_c(N,C) :- !)).

% Try the following goal sequence and see what happens:

% ?- [p49]. 
% ?- listing(gray_c/2).
% ?- gray_c(5,C).
% ?- listing(gray_c/2).


% There is an alternative definition for the gray code construction:

gray_alt(1,['0','1']).
gray_alt(N,C) :- N > 1, N1 is N-1,
   gray_alt(N1,C1), 
   postpend(['0','1'],C1,C).   

postpend(_,[],[]).
postpend(P,[C|Cs],[C1P,C2P|CsP]) :- P = [P1,P2],
   atom_concat(C,P1,C1P), 
   atom_concat(C,P2,C2P),
   reverse(P,PR),
   postpend(PR,Cs,CsP).

```


<br/>
<hr/>
<br/>
<h2 id="44"> 44. 3-Logic-and-Codes-P46/p49_gray.pl</h2>

``` prolog
:-ensure_loaded(permute).
different_by_one_bit(A,B):-I is A xor B,has_exactly_one_bit_on(I).
has_exactly_one_bit_on(I):-I>0,K is I/\1,K>0,I1 is I>>1,I1=:=0.
has_exactly_one_bit_on(I):-I>0,K is I/\1,K =:= 0,I1 is I>>1,has_exactly_one_bit_on(I1).

upto(0,[0]).
upto(N,[N|Xs]):-N>0,N1 is N-1,upto(N1,Xs).

max(NBits,Val):-Val is 2^NBits-1.

graycode(NBits,C):-
	max(NBits,Max),
	upto(Max,P),
	length(P,Length),
	permutation(Length,P,C),
	is_gray(C).
is_gray([]).
is_gray([X1,X2|Xs]):-
	different_by_one_bit(X1,X2),
	is_gray([X2|Xs]).
is_gray([_]).

```


<br/>
<hr/>
<br/>
<h2 id="45"> 45. 3-Logic-and-Codes-P46/p50.pl</h2>

``` prolog
% (***) P50 Huffman code

% We suppose a set of symbols with their frequencies, given as a list 
% of fr(S,F) terms. 
% Example: [fr(a,45),fr(b,13),fr(c,12),fr(d,16),fr(e,9),fr(f,5)]. 
% Our objective is to construct a list hc(S,C) terms, where C is the Huffman
% code word for the symbol S. In our example the result could be 
% [hc(a, '0'), hc(b, '101'), hc(c, '100'), hc(d, '111'), hc(e, '1101'), 
% hc(f, '1100')]  

% The task shall be performed by the predicate huffman/2 defined as follows: 
 
% huffman(Fs,Hs) :- Hs is the Huffman code table for the frequency table Fs
% (list-of-fr/2-terms, list-of-hc/2-terms)  (+,-).

% During the construction process, we need nodes n(F,S) where, at the 
% beginning, F is a frequency and S a symbol. During the process, as n(F,S)
% becomes an internal node, S becomes a term s(L,R) with L and R being 
% again n(F,S) terms. A list of n(F,S) terms, called Ns, is maintained 
% as a sort of priority queue.

huffman(Fs,Cs) :-
   initialize(Fs,Ns),
   make_tree(Ns,T),
   traverse_tree(T,Cs).

initialize(Fs,Ns) :- init(Fs,NsU), sort(NsU,Ns).

init([],[]).
init([fr(S,F)|Fs],[n(F,S)|Ns]) :- init(Fs,Ns).

make_tree([T],T).
make_tree([n(F1,X1),n(F2,X2)|Ns],T) :- 
   F is F1+F2,
   insert3(n(F,s(n(F1,X1),n(F2,X2))),Ns,NsR),
   make_tree(NsR,T).

% insert3(n(F,X),Ns,NsR) :- insert3 the node n(F,X) into Ns such that the
%    resulting list NsR is again sorted with respect to the frequency F.

insert3(N,[],[N]) :- !.
insert3(n(F,X),[n(F0,Y)|Ns],[n(F,X),n(F0,Y)|Ns]) :- F < F0, !.
insert3(n(F,X),[n(F0,Y)|Ns],[n(F0,Y)|Ns1]) :- F >= F0, insert3(n(F,X),Ns,Ns1).

% traverse_tree(T,Cs) :- traverse the tree T and construct the Huffman 
%    code table Cs,

traverse_tree(T,Cs) :- traverse_tree(T,'',Cs1-[]), sort(Cs1,Cs).

traverse_tree(n(_,A),Code,[hc(A,Code)|Cs]-Cs) :- atom(A). % leaf node
traverse_tree(n(_,s(Left,Right)),Code,Cs1-Cs3) :-         % internal node
   atom_concat(Code,'0',CodeLeft), 
   atom_concat(Code,'1',CodeRight),
   traverse_tree(Left,CodeLeft,Cs1-Cs2),
   traverse_tree(Right,CodeRight,Cs2-Cs3).


% The following predicate gives some statistical information.

huffman(Fs) :- huffman(Fs,Hs) , nl, report(Hs,5), stats(Fs,Hs).

report([],_) :- !, nl, nl.
report(Hs,0) :- !, nl, report(Hs,5).
report([hc(S,C)|Hs],N) :- N > 0, N1 is N-1, 
   writef('%w %8l  ',[S,C]), report(Hs,N1).

stats(Fs,Cs) :- sort(Fs,FsS), sort(Cs,CsS), stats(FsS,CsS,0,0).

stats([],[],FreqCodeSum,FreqSum) :- Avg is FreqCodeSum/FreqSum,
   writef('Average code length (weighted) = %w\n',[Avg]). 
stats([fr(S,F)|Fs],[hc(S,C)|Hs],FCS,FS) :- 
   atom_chars(C,CharList), length(CharList,N),
   FCS1 is FCS + F*N, FS1 is FS + F,
   stats(Fs,Hs,FCS1,FS1). 
   

```


<br/>
<hr/>
<br/>
<h2 id="46"> 46. 4-Binary-Trees-P54/p54.pl</h2>

``` prolog
% P54: Write a predicate istree/1 which succeeds if and only if its argument
%      is a Prolog term representing a binary tree.
%
% istree(T) :- T is a term representing a binary tree (i), (o)

istree(nil).
istree(t(_,L,R)) :- istree(L), istree(R).


% Test cases (can be used for other binary tree problems as well)

tree(1,t(a,t(b,t(d,nil,nil),t(e,nil,nil)),t(c,nil,t(f,t(g,nil,nil),nil)))).
tree(2,t(a,nil,nil)).
tree(3,nil).

```


<br/>
<hr/>
<br/>
<h2 id="47"> 47. 4-Binary-Trees-P54/p55.pl</h2>

``` prolog
% P55 (**) Construct completely balanced binary trees for a given 
% number of nodes.

% cbal_tree(N,T) :- T is a completely balanced binary tree with N nodes.
% (integer, tree)  (+,?)

cbal_tree(0,nil) :- !.
cbal_tree(N,t(x,L,R)) :- N > 0,
	N0 is N - 1, 
	N1 is N0//2, N2 is N0 - N1,
	distrib(N1,N2,NL,NR),
	cbal_tree(NL,L), cbal_tree(NR,R).

distrib(N,N,N,N) :- !.
distrib(N1,N2,N1,N2).
distrib(N1,N2,N2,N1).

```


<br/>
<hr/>
<br/>
<h2 id="48"> 48. 4-Binary-Trees-P54/p56.pl</h2>

``` prolog
% P56 (**) Symmetric binary trees 
% Let us call a binary tree symmetric if you can draw a vertical 
% line through the root node and then the right subtree is the mirror
% image of the left subtree.
% Write a predicate symmetric/1 to check whether a given binary
% tree is symmetric. Hint: Write a predicate mirror/2 first to check
% whether one tree is the mirror image of another.

% symmetric(T) :- the binary tree T is symmetric.

symmetric(nil).
symmetric(t(_,L,R)) :- mirror(L,R).

mirror(nil,nil).
mirror(t(_,L1,R1),t(_,L2,R2)) :- mirror(L1,R2), mirror(R1,L2).

```


<br/>
<hr/>
<br/>
<h2 id="49"> 49. 4-Binary-Trees-P54/p57.pl</h2>

``` prolog
% P57 (**) Binary search trees (dictionaries)

% Use the predicate add/3, developed in chapter 4 of the course,
% to write a predicate to construct a binary search tree 
% from a list of integer numbers. Then use this predicate to test 
% the solution of the problem P56

:- ensure_loaded(p56).

% add(X,T1,T2) :- the binary dictionary T2 is obtained by 
% adding the item X to the binary dictionary T1
% (element,binary-dictionary,binary-dictionary) (i,i,o)

add(X,nil,t(X,nil,nil)).
add(X,t(Root,L,R),t(Root,L1,R)) :- X @< Root, add(X,L,L1).
add(X,t(Root,L,R),t(Root,L,R1)) :- X @> Root, add(X,R,R1).

construct(L,T) :- construct(L,T,nil).

construct([],T,T).
construct([N|Ns],T,T0) :- add(N,T0,T1), construct(Ns,T,T1).
 	
test_symmetric(L) :- construct(L,T), symmetric(T).

```


<br/>
<hr/>
<br/>
<h2 id="50"> 50. 4-Binary-Trees-P54/p58.pl</h2>

``` prolog
% P58 (**) Generate-and-test paradigm

% Apply the generate-and-test paradigm to construct all symmetric,
% completely balanced binary trees with a given number of nodes.

:- ensure_loaded(p55).
:- ensure_loaded(p56).


sym_cbal_tree(N,T) :- cbal_tree(N,T), symmetric(T).

sym_cbal_trees(N,Ts) :- setof(T,sym_cbal_tree(N,T),Ts).

investigate(A,B) :-
	between(A,B,N),
	sym_cbal_trees(N,Ts),
	length(Ts,L),
	writef('%w   %w',[N,L]), nl,
    fail.
investigate(_,_).

```


<br/>
<hr/>
<br/>
<h2 id="51"> 51. 4-Binary-Trees-P54/p59.pl</h2>

``` prolog
% P59 (**) Construct height-balanced binary trees
% In a height-balanced binary tree, the following property holds for
% every node: The height of its left subtree and the height of  
% its right subtree are almost equal, which means their
% difference is not greater than one.
% Write a predicate hbal_tree/2 to construct height-balanced
% binary trees for a given height. The predicate should
% generate all solutions via backtracking. Put the letter 'x'
% as information into all nodes of the tree.

% hbal_tree(D,T) :- T is a height-balanced binary tree with depth T

hbal_tree(0,nil) :- !.
hbal_tree(1,t(x,nil,nil)) :- !.
hbal_tree(D,t(x,L,R)) :- D > 1,
	D1 is D - 1, D2 is D - 2,
	distr(D1,D2,DL,DR),
	hbal_tree(DL,L), hbal_tree(DR,R).

distr(D1,_,D1,D1).
distr(D1,D2,D1,D2).
distr(D1,D2,D2,D1).
```


<br/>
<hr/>
<br/>
<h2 id="52"> 52. 4-Binary-Trees-P54/p60.pl</h2>

``` prolog
% P60 (**) Construct height-balanced binary trees with a given number of nodes

:- ensure_loaded(p59).

% minNodes(H,N) :- N is the minimum number of nodes in a height-balanced 
% binary tree of height H
% (integer,integer) (+,?)

minNodes(0,0) :- !.
minNodes(1,1) :- !.
minNodes(H,N) :- H > 1, 
	H1 is H - 1, H2 is H - 2,
	minNodes(H1,N1), minNodes(H2,N2),
	N is 1 + N1 + N2.

% maxNodes(H,N) :- N is the maximum number of nodes in a height-balanced 
% binary tree of height H
% (integer,integer) (+,?)

maxNodes(H,N) :- N is 2**H - 1.

% minHeight(N,H) :- H is the minimum height of a height-balanced 
% binary tree with N nodes
% (integer,integer) (+,?)

minHeight(0,0) :- !.
minHeight(N,H) :- N > 0, N1 is N//2, minHeight(N1,H1), H is H1 + 1.

% maxHeight(N,H) :- H is the maximum height of a height-balanced 
% binary tree with N nodes
% (integer,integer) (+,?)

maxHeight(N,H) :- maxHeight(N,H,1,1).

maxHeight(N,H,H1,N1) :- N1 > N, !, H is H1 - 1.
maxHeight(N,H,H1,N1) :- N1 =< N, 
	H2 is H1 + 1, minNodes(H2,N2), maxHeight(N,H,H2,N2).

% hbal_tree_nodes(N,T) :- T is a height-balanced binary tree with N nodes.

hbal_tree_nodes(N,T) :- 
	minHeight(N,Hmin), maxHeight(N,Hmax),
	between(Hmin,Hmax,H),
	hbal_tree(H,T), nodes(T,N).

% the following predicate is from the course (chapter 4)

%  nodes(T,N) :- the binary tree T has N nodes
% (tree,integer);  (i,*) 
 
nodes(nil,0).
nodes(t(_,Left,Right),N) :-
   nodes(Left,NLeft),
   nodes(Right,NRight),
   N is NLeft + NRight + 1.

% count_hbal_trees(N,C) :- there are C different height-balanced binary
% trees with N nodes.

count_hbal_trees(N,C) :- setof(T,hbal_tree_nodes(N,T),Ts), length(Ts,C).
count_hbal_trees1(N,C) :- bagof(T,hbal_tree_nodes(N,T),Ts), length(Ts,C). 
```


<br/>
<hr/>
<br/>
<h2 id="53"> 53. 4-Binary-Trees-P54/p61.pl</h2>

``` prolog
% P61 (*) Count the leaves of a binary tree

:- ensure_loaded(p54).

% count_leaves(T,N) :- the binary tree T has N leaves

count_leaves(nil,0).
count_leaves(t(_,nil,nil),1).
count_leaves(t(_,L,nil),N) :- L = t(_,_,_), count_leaves(L,N).
count_leaves(t(_,nil,R),N) :- R = t(_,_,_), count_leaves(R,N).
count_leaves(t(_,L,R),N) :- L = t(_,_,_), R = t(_,_,_),
   count_leaves(L,NL), count_leaves(R,NR), N is NL + NR.

% The above solution works in the flow patterns (i,o) and (i,i)
% without cut and produces a single correct result. Using a cut 
% we can obtain the same result in a much shorter program, like this:

count_leaves1(nil,0).
count_leaves1(t(_,nil,nil),1) :- !.
count_leaves1(t(_,L,R),N) :- 
    count_leaves1(L,NL), count_leaves1(R,NR), N is NL+NR.

% For the flow pattern (o,i) see P61A

```


<br/>
<hr/>
<br/>
<h2 id="54"> 54. 4-Binary-Trees-P54/p61A.pl</h2>

``` prolog
% P61A (*) Collect the leaves of a binary tree in a list

:- ensure_loaded(p54).

% leaves(T,S) :- S is the list of the leaves of the binary tree T

leaves(nil,[]).
leaves(t(X,nil,nil),[X]).
leaves(t(_,L,nil),S) :- L = t(_,_,_), leaves(L,S).
leaves(t(_,nil,R),S) :- R = t(_,_,_), leaves(R,S).
leaves(t(_,L,R),S) :- L = t(_,_,_), R = t(_,_,_),
    leaves(L,SL), leaves(R,SR), append(SL,SR,S).

% The above solution works in the flow patterns (i,o) and (i,i)
% without cut and produces a single correct result. Using a cut 
% we can obtain the same result in a much shorter program, like this:

leaves1(nil,[]).
leaves1(t(X,nil,nil),[X]) :- !.
leaves1(t(_,L,R),S) :- 
    leaves1(L,SL), leaves1(R,SR), append(SL,SR,S).

% To write a predicate that works in the flow pattern (o,i)
% is a more difficult problem, because using append/3 in
% the flow pattern (o,o,i) always generates an empty list 
% as first solution and the result is an infinite recursion
% along the left subtree of the generated binary tree.
% A possible solution is the following trick: we successively
% construct binary tree structures for a given number of nodes
% and fill the leaf nodes with the elements of the leaf list.
% We then increment the number of tree nodes successively,
% and so on. 

% nnodes(T,N) :- T is a binary tree with N nodes (o,i)
nnodes(nil,0) :- !.
nnodes(t(_,L,R),N) :- N > 0, N1 is N-1, 
   between(0,N1,NL), NR is N1-NL,
   nnodes(L,NL), nnodes(R,NR).


% leaves2(T,S) :- S is the list of leaves of the tree T (o,i)

leaves2(T,S) :- leaves2(T,S,0).

leaves2(T,S,N) :- nnodes(T,N), leaves1(T,S).
leaves2(T,S,N) :- N1 is N+1, leaves2(T,S,N1).

% OK, this was difficulty (**)



```


<br/>
<hr/>
<br/>
<h2 id="55"> 55. 4-Binary-Trees-P54/p62.pl</h2>

``` prolog
% P62 (*) Collect the internal nodes of a binary tree in a list

:- ensure_loaded(p54).

% internals(T,S) :- S is the list of internal nodes of the binary tree T.

internals(nil,[]).
internals(t(_,nil,nil),[]).
internals(t(X,L,nil),[X|S]) :- L = t(_,_,_), internals(L,S).
internals(t(X,nil,R),[X|S]) :- R = t(_,_,_), internals(R,S).
internals(t(X,L,R),[X|S]) :- L = t(_,_,_), R = t(_,_,_), 
   internals(L,SL), internals(R,SR), append(SL,SR,S).

% The above solution works in the flow patterns (i,o) and (i,i)
% without cut and produces a single correct result. Using a cut 
% we can obtain the same result in a much shorter program, like this:

internals1(nil,[]).
internals1(t(_,nil,nil),[]) :- !.
internals1(t(X,L,R),[X|S]) :- 
    internals1(L,SL), internals1(R,SR), append(SL,SR,S).

% For the flow pattern (o,i) there is the following very
% elegant solution:

internals2(nil,[]).
internals2(t(X,L,R),[X|S]) :- 
   append(SL,SR,S), internals2(L,SL), internals2(R,SR).

```


<br/>
<hr/>
<br/>
<h2 id="56"> 56. 4-Binary-Trees-P54/p62B.pl</h2>

``` prolog
% P62B (*) Collect the nodes of a binary tree at a given level in a list

:- ensure_loaded(p54).

% atlevel(T,D,S) :- S is the list of nodes of the binary tree T at level D
% (i,i,o)

atlevel(nil,_,[]).
atlevel(t(X,_,_),1,[X]).
atlevel(t(_,L,R),D,S) :- D > 1, D1 is D-1,
   atlevel(L,D1,SL), atlevel(R,D1,SR), append(SL,SR,S).


% The following is a quick-and-dirty solution for the
% level-order sequence

levelorder(T,S) :- levelorder(T,S,1).

levelorder(T,[],D) :- atlevel(T,D,[]), !.
levelorder(T,S,D) :- atlevel(T,D,SD),
   D1 is D+1, levelorder(T,S1,D1), append(SD,S1,S).


```


<br/>
<hr/>
<br/>
<h2 id="57"> 57. 4-Binary-Trees-P54/p63.pl</h2>

``` prolog
% P63 (**) Construct a complete binary tree
%
% A complete binary tree with height H is defined as follows: 
% The levels 1,2,3,...,H-1 contain the maximum number of nodes 
% (i.e 2**(i-1) at the level i, note that we start counting the 
% levels from 1 at the root). In level H, which may contain less 
% than the maximum number possible of nodes, all the nodes are 
% "left-adjusted". This means that in a levelorder tree traversal 
% all internal nodes come first, the leaves come second, and
% empty successors (the nils which are not really nodes!) 
% come last. Complete binary trees are used for heaps.

:- ensure_loaded(p57).

% complete_binary_tree(N,T) :- T is a complete binary tree with
% N nodes. (+,?)

complete_binary_tree(N,T) :- complete_binary_tree(N,T,1).

complete_binary_tree(N,nil,A) :- A > N, !.
complete_binary_tree(N,t(_,L,R),A) :- A =< N,
	AL is 2 * A, AR is AL + 1,
	complete_binary_tree(N,L,AL),
	complete_binary_tree(N,R,AR).


% ----------------------------------------------------------------------

% This was the solution of the exercise. What follows is an application
% of this result.

% We define a heap as a term heap(N,T) where N is the number of elements
% and T a complete binary tree (in the sense used above).

% The conservative usage of a heap is first to declare it with a predicate
% declare_heap/2 and then use it with a predicate element_at/3.

% declare_heap(H,N) :- 
%    declare H to be a heap with a fixed number N  of elements

declare_heap(heap(N,T),N) :- complete_binary_tree(N,T).

% element_at(H,K,X) :- X is the element at address K in the heap H. 
%  The first element has address 1.
%  (+,+,?)

element_at(heap(_,T),K,X) :- 
   binary_path(K,[],BP), element_at_path(T,BP,X).

binary_path(1,Bs,Bs) :- !.
binary_path(K,Acc,Bs) :- K > 1, 
   B is K /\ 1, K1 is K >> 1, binary_path(K1,[B|Acc],Bs).

element_at_path(t(X,_,_),[],X) :- !.
element_at_path(t(_,L,_),[0|Bs],X) :- !, element_at_path(L,Bs,X).
element_at_path(t(_,_,R),[1|Bs],X) :- element_at_path(R,Bs,X).


% We can transform lists into heaps and vice versa with the following
% useful predicate:

% list_heap(L,H) :- transform a list into a (limited) heap and vice versa.

list_heap(L,H) :- is_list(L), list_to_heap(L,H).
list_heap(L,heap(N,T)) :- integer(N), fill_list(heap(N,T),N,1,L).

list_to_heap(L,H) :- 
   length(L,N), declare_heap(H,N), fill_heap(H,L,1).

fill_heap(_,[],_).
fill_heap(H,[X|Xs],K) :- element_at(H,K,X), K1 is K+1, fill_heap(H,Xs,K1).

fill_list(_,N,K,[]) :- K > N.
fill_list(H,N,K,[X|Xs]) :- K =< N, 
   element_at(H,K,X), K1 is K+1, fill_list(H,N,K1,Xs).


% However, a more aggressive usage is *not* to define the heap in the
% beginning, but to use it as a partially instantiated data structure.
% Used in this way, the number of elements in the heap is unlimited.
% This is Power-Prolog!

% Try the following and find out exactly what happens.

% ?- element_at(H,5,alfa), element_at(H,2,beta), element(H,5,A).

% -------------------------------------------------------------------------

% Test section. Suppose you have N elements in a list which must be looked
% up M times in a random order.

test1(N,M) :-
   length(List,N), lookup_list(List,N,M).

lookup_list(_,_,0) :- !.
lookup_list(List,N,M) :- 
   K is random(N)+1,       % determine a random address
   nth1(K,List,_),         % look up and throw away
   M1 is M-1,
   lookup_list(List,N,M1).

% ?- time(test1(100,100000)).
% 1,384,597 inferences in 3.98 seconds (347889 Lips)
% ?- time(test1(500,100000)).
% 4,721,902 inferences in 13.82 seconds (341672 Lips)
% ?- time(test1(10000,100000)).
% 84,016,719 inferences in 277.51 seconds (302752 Lips)

test2(N,M) :-
   declare_heap(Heap,N), 
   lookup_heap(Heap,N,M).

lookup_heap(_,_,0) :- !.
lookup_heap(Heap,N,M) :- 
   K is random(N)+1,       % determine a random address
   element_at(Heap,K,_),   % look up and throw away
   M1 is M-1,
   lookup_heap(Heap,N,M1).

% ?- time(test2(100,100000)).
% 3,002,061 inferences in 7.81 seconds (384387 Lips)                          
% ?- time(test2(500,100000)).
% 4,097,961 inferences in 10.75 seconds (381206 Lips)
% ?- time(test2(10000,100000)).
% 6,366,206 inferences in 19.16 seconds (332265 Lips)

% Conclusion: In this scenario, for lists longer than 500 elements 
% it is more efficient to use a heap.

```


<br/>
<hr/>
<br/>
<h2 id="58"> 58. 4-Binary-Trees-P54/p64.pl</h2>

``` prolog
% P64 (**) Layout a binary tree (1)
%
% Given a binary tree as the usual Prolog term t(X,L,R) (or nil).
% As a preparation for drawing the tree, a layout algorithm is
% required to determine the position of each node in a rectangular
% grid. Several layout methods are conceivable, one of them is
% the following:
%
% The position of a node v is obtained by the following two rules:
%   x(v) is equal to the position of the node v in the inorder sequence
%   y(v) is equal to the depth of the node v in the tree
%
% In order to store the position of the nodes, we extend the Prolog 
% term representing a node (and its successors) as follows:
%    nil represents the empty tree (as usual)
%    t(W,X,Y,L,R) represents a (non-empty) binary tree with root
%        W positionned at (X,Y), and subtrees L and R
%
% Write a predicate layout_binary_tree/2:

% layout_binary_tree(T,PT) :- PT is the "positionned" binary
%    tree obtained from the binary tree T. (+,?) or (?,+)

:- ensure_loaded(p57). % for test

layout_binary_tree(T,PT) :- layout_binary_tree(T,PT,1,_,1).

% layout_binary_tree(T,PT,In,Out,D) :- T and PT as in layout_binary_tree/2;
%    In is the position in the inorder sequence where the tree T (or PT)
%    begins, Out is the position after the last node of T (or PT) in the 
%    inorder sequence. D is the depth of the root of T (or PT). 
%    (+,?,+,?,+) or (?,+,+,?,+)
 
layout_binary_tree(nil,nil,I,I,_).
layout_binary_tree(t(W,L,R),t(W,X,Y,PL,PR),Iin,Iout,Y) :- 
   Y1 is Y + 1,
   layout_binary_tree(L,PL,Iin,X,Y1), 
   X1 is X + 1,
   layout_binary_tree(R,PR,X1,Iout,Y1).

% Test (see example given in the problem description):
% ?-  construct([n,k,m,c,a,h,g,e,u,p,s,q],T),layout_binary_tree(T,PT).

```


<br/>
<hr/>
<br/>
<h2 id="59"> 59. 4-Binary-Trees-P54/p65.pl</h2>

``` prolog
% P65 (**) Layout a binary tree (2)
%
% See problem P64 for the conventions.
%
% The position of a node v is obtained by the following rules:
%   (1) y(v) is equal to the depth of the node v in the tree
%   (2) if D denotes the depth of the tree (i.e. the number of
%       populated levels) then the horizontal distance between
%       nodes at level i (counted from the root, beginning with 1)
%       is equal to 2**(D-i+1). The leftmost node of the tree
%       is at position 1.

% layout_binary_tree2(T,PT) :- PT is the "positionned" binary
%    tree obtained from the binary tree T. (+,?)

:- ensure_loaded(p57). % for test

layout_binary_tree2(nil,nil) :- !. 
layout_binary_tree2(T,PT) :- 
   hor_dist(T,D4), D is D4//4, x_pos(T,X,D), 
   layout_binary_tree2(T,PT,X,1,D).

% hor_dist(T,D4) :- D4 is four times the horizontal distance between the 
%    root node of T and its successor(s) (if any).
%    (+,-)

hor_dist(nil,1).
hor_dist(t(_,L,R),D4) :- 
   hor_dist(L,D4L), 
   hor_dist(R,D4R),
   D4 is 2 * max(D4L,D4R).

% x_pos(T,X,D) :- X is the horizontal position of the root node of T
%    with respect to the picture co-ordinate system. D is the horizontal
%    distance between the root node of T and its successor(s) (if any).
%    (+,-,+)

x_pos(t(_,nil,_),1,_) :- !.
x_pos(t(_,L,_),X,D) :- D2 is D//2, x_pos(L,XL,D2), X is XL+D.

% layout_binary_tree2(T,PT,X,Y,D) :- T and PT as in layout_binary_tree/2;
%    D is the the horizontal distance between the root node of T and 
%    its successor(s) (if any). X, Y are the co-ordinates of the root node.
%    (+,-,+,+,+)
 
layout_binary_tree2(nil,nil,_,_,_).
layout_binary_tree2(t(W,L,R),t(W,X,Y,PL,PR),X,Y,D) :- 
   Y1 is Y + 1,
   Xleft is X - D,
   D2 is D//2,
   layout_binary_tree2(L,PL,Xleft,Y1,D2), 
   Xright is X + D,
   layout_binary_tree2(R,PR,Xright,Y1,D2).

% Test (see example given in the problem description):
% ?- construct([n,k,m,c,a,e,d,g,u,p,q],T),layout_binary_tree2(T,PT).

```


<br/>
<hr/>
<br/>
<h2 id="60"> 60. 4-Binary-Trees-P54/p66.pl</h2>

``` prolog
% P66 (***) Layout a binary tree (3)
%
% See problem P64 for the conventions.
%
% The position of a node v is obtained by the following rules:
%   (1) y(v) is equal to the depth of the node v in the tree
%   (2) in order to determine the horizontal positions of the nodes we
%       construct "contours" for each subtree and shift them together 
%       horizontally as close as possible. However, we maintain the
%       symmetry in each node; i.e. the horizontal distance between
%       a node and the root of its left subtree is the same as between
%       it and the root of its right subtree.
%
%       The "contour" of a tree is a list of terms c(Xleft,Xright) which
%       give the horizontal position of the outermost nodes of the tree
%       on each level, relative to the root position. In the example
%       given in the problem description, the "contour" of the tree with
%       root k would be [c(-1,1),c(-2,0),c(-1,1)]. Note that the first
%       element in the "contour" list is derived from the position of
%       the nodes c and m.

% layout_binary_tree3(T,PT) :- PT is the "positionned" binary
%    tree obtained from the binary tree T. (+,?)

:- ensure_loaded(p57). % for test

layout_binary_tree3(nil,nil) :- !. 
layout_binary_tree3(T,PT) :-
   contour_tree(T,CT),      % construct the "contour" tree CT
   CT = t(_,_,_,Contour),
   mincont(Contour,MC,0),   % find the position of the leftmost node
   Xroot is 1-MC,
   layout_binary_tree3(CT,PT,Xroot,1).

contour_tree(nil,nil).
contour_tree(t(X,L,R),t(X,CL,CR,Contour)) :- 
   contour_tree(L,CL),
   contour_tree(R,CR),
   combine(CL,CR,Contour).

combine(nil,nil,[]).
combine(t(_,_,_,CL),nil,[c(-1,-1)|Cs]) :- shift(CL,-1,Cs).
combine(nil,t(_,_,_,CR),[c(1,1)|Cs]) :- shift(CR,1,Cs).
combine(t(_,_,_,CL),t(_,_,_,CR),[c(DL,DR)|Cs]) :-
   maxdiff(CL,CR,MD,0), 
   DR is (MD+2)//2, DL is -DR,
   merge(CL,CR,DL,DR,Cs).

shift([],_,[]).
shift([c(L,R)|Cs],S,[c(LS,RS)|CsS]) :-
   LS is L+S, RS is R+S, shift(Cs,S,CsS).

maxdiff([],_,MD,MD) :- !.
maxdiff(_,[],MD,MD) :- !.
maxdiff([c(_,R1)|Cs1],[c(L2,_)|Cs2],MD,A) :- 
   A1 is max(A,R1-L2),
   maxdiff(Cs1,Cs2,MD,A1).

merge([],CR,_,DR,Cs) :- !, shift(CR,DR,Cs).
merge(CL,[],DL,_,Cs) :- !, shift(CL,DL,Cs).
merge([c(L1,_)|Cs1],[c(_,R2)|Cs2],DL,DR,[c(L,R)|Cs]) :-
   L is L1+DL, R is R2+DR,
   merge(Cs1,Cs2,DL,DR,Cs).

mincont([],MC,MC).
mincont([c(L,_)|Cs],MC,A) :- 
   A1 is min(A,L), mincont(Cs,MC,A1).

layout_binary_tree3(nil,nil,_,_).
layout_binary_tree3(t(W,nil,nil,_),t(W,X,Y,nil,nil),X,Y) :- !. 
layout_binary_tree3(t(W,L,R,[c(DL,DR)|_]),t(W,X,Y,PL,PR),X,Y) :- 
   Y1 is Y + 1,
   Xleft is X + DL,
   layout_binary_tree3(L,PL,Xleft,Y1), 
   Xright is X + DR,
   layout_binary_tree3(R,PR,Xright,Y1).

% Test (see example given in the problem description):
% ?- construct([n,k,m,c,a,e,d,g,u,p,q],T),layout_binary_tree3(T,PT).

```


<br/>
<hr/>
<br/>
<h2 id="61"> 61. 4-Binary-Trees-P54/p67a.pl</h2>

``` prolog
% P67 (**)  A string representation of binary trees

% The string representation has the following syntax:
%
% <tree> ::=  | <letter><subtrees>
%
% <subtrees> ::=  | '(' <tree> ',' <tree> ')'
%
% According to this syntax, a leaf node (with letter x) could
% be represented by x(,) and not only by the single character x.
% However, we will avoid this when generating the string 
% representation.

tree_string(T,S) :- nonvar(T), !, tree_to_string(T,S). 
tree_string(T,S) :- nonvar(S), string_to_tree(S,T). 

tree_to_string(T,S) :- tree_to_list(T,L), atom_chars(S,L).

tree_to_list(nil,[]).
tree_to_list(t(X,nil,nil),[X]) :- !.
tree_to_list(t(X,L,R),[X,'('|List]) :- 
   tree_to_list(L,LsL),
   tree_to_list(R,LsR),
   append(LsL,[','],List1),
   append(List1,LsR,List2),
   append(List2,[')'],List).

string_to_tree(S,T) :- atom_chars(S,L), list_to_tree(L,T).

list_to_tree([],nil).
list_to_tree([X],t(X,nil,nil)) :- char_type(X,alpha).
list_to_tree([X,'('|List],t(X,Left,Right)) :- char_type(X,alpha),
   append(List1,[')'],List),
   append(LeftList,[','|RightList],List1),
   list_to_tree(LeftList,Left),
   list_to_tree(RightList,Right).

```


<br/>
<hr/>
<br/>
<h2 id="62"> 62. 4-Binary-Trees-P54/p67b.pl</h2>

``` prolog
% P67 (**) A string representation of binary trees

% Most elegant solution using difference lists.

tree_string(T,S) :- nonvar(T), tree_dlist(T,L-[]), !, atom_chars(S,L). 
tree_string(T,S) :- nonvar(S), atom_chars(S,L), tree_dlist(T,L-[]).

% tree_dlist/2 does the trick in both directions!

tree_dlist(nil,L-L).
tree_dlist(t(X,nil,nil),L1-L2) :- 
   letter(X,L1-L2).
tree_dlist(t(X,Left,Right),L1-L7) :- 
   letter(X,L1-L2), 
   symbol('(',L2-L3),
   tree_dlist(Left,L3-L4),
   symbol(',',L4-L5),
   tree_dlist(Right,L5-L6),
   symbol(')',L6-L7).

symbol(X,[X|Xs]-Xs).

letter(X,L1-L2) :- symbol(X,L1-L2), char_type(X,alpha).

```


<br/>
<hr/>
<br/>
<h2 id="63"> 63. 4-Binary-Trees-P54/p68a.pl</h2>

``` prolog
% P68 (**) Preorder and inorder sequences of binary trees

% We consider binary trees with nodes that are identified by
% single lower-case letters.

% a) Given a binary tree, construct its preorder sequence

preorder(T,S) :- preorder_tl(T,L), atom_chars(S,L).

preorder_tl(nil,[]).
preorder_tl(t(X,Left,Right),[X|List]) :-
   preorder_tl(Left,ListLeft),
   preorder_tl(Right,ListRight),
   append(ListLeft,ListRight,List).

inorder(T,S) :- inorder_tl(T,L), atom_chars(S,L).

inorder_tl(nil,[]).
inorder_tl(t(X,Left,Right),List) :-
   inorder_tl(Left,ListLeft),
   inorder_tl(Right,ListRight),
   append(ListLeft,[X|ListRight],List).

```


<br/>
<hr/>
<br/>
<h2 id="64"> 64. 4-Binary-Trees-P54/p68b.pl</h2>

``` prolog
% P68b (**) Preorder and inorder sequences of binary trees

% b) Make preorder/2 and inorder/2 reversible.

% Similar to the solution p68a.pl. However, for the flow pattern (-,+) 
% we have to modify the order of the subgoals in the second clauses 
% of preorder_l/2 and inorder_l/2

% preorder(T,S) :- S is the preorder tre traversal sequence of the
%    nodes of the binary tree T. (tree,atom) (+,?) or (?,+)

preorder(T,S) :- nonvar(T), !, preorder_tl(T,L), atom_chars(S,L). 
preorder(T,S) :- atom(S), atom_chars(S,L), preorder_lt(T,L).

preorder_tl(nil,[]).
preorder_tl(t(X,Left,Right),[X|List]) :-
   preorder_tl(Left,ListLeft),
   preorder_tl(Right,ListRight),
   append(ListLeft,ListRight,List).

preorder_lt(nil,[]).
preorder_lt(t(X,Left,Right),[X|List]) :-
   append(ListLeft,ListRight,List),
   preorder_lt(Left,ListLeft),
   preorder_lt(Right,ListRight).

% inorder(T,S) :- S is the inorder tre traversal sequence of the
%    nodes of the binary tree T. (tree,atom) (+,?) or (?,+)

inorder(T,S) :- nonvar(T), !, inorder_tl(T,L), atom_chars(S,L). 
inorder(T,S) :- atom(S), atom_chars(S,L), inorder_lt(T,L).

inorder_tl(nil,[]).
inorder_tl(t(X,Left,Right),List) :-
   inorder_tl(Left,ListLeft),
   inorder_tl(Right,ListRight),
   append(ListLeft,[X|ListRight],List).

inorder_lt(nil,[]).
inorder_lt(t(X,Left,Right),List) :-
   append(ListLeft,[X|ListRight],List),
   inorder_lt(Left,ListLeft),
   inorder_lt(Right,ListRight).

```


<br/>
<hr/>
<br/>
<h2 id="65"> 65. 4-Binary-Trees-P54/p68c.pl</h2>

``` prolog
% P68c (**) Preorder and inorder sequences of binary trees

% If both the preorder sequence and the inorder sequence of
% the nodes of a binary tree are given, then the tree is determined
% unambiguously. 

:- ensure_loaded(p68b).

% pre_in_tree(P,I,T) :- T is the binary tree that has the preorder
%   sequence P and inorder sequence I.
%   (atom,atom,tree) (+,+,?)

pre_in_tree(P,I,T) :- preorder(T,P), inorder(T,I).

% This is a nice application of the generate-and-test method.


% We can push the tester inside the generator in order to get
% a (much) better performance.

pre_in_tree_push(P,I,T) :- 
   atom_chars(P,PL), atom_chars(I,IL), pre_in_tree_pu(PL,IL,T).

pre_in_tree_pu([],[],nil).
pre_in_tree_pu([X|PL],IL,t(X,Left,Right)) :- 
   append(ILeft,[X|IRight],IL),
   append(PLeft,PRight,PL),
   pre_in_tree_pu(PLeft,ILeft,Left),
   pre_in_tree_pu(PRight,IRight,Right).
   
% Nice. But there is a still better solution. See problem d)!
```


<br/>
<hr/>
<br/>
<h2 id="66"> 66. 4-Binary-Trees-P54/p68d.pl</h2>

``` prolog
% P68d (**) Preorder and inorder sequences of binary trees

% Work with difference lists

% pre_in_tree_d(P,I,T) :- T is the binary tree that has the preorder
%   sequence P and inorder sequence I.
%   (atom,atom,tree) (+,+,?)

pre_in_tree_d(P,I,T) :-  
   atom_chars(P,PL), atom_chars(I,IL), pre_in_tree_dl(PL-[],IL-[],T).

pre_in_tree_dl(P-P,I-I,nil).
pre_in_tree_dl(P1-P4,I1-I4,t(X,Left,Right)) :-
   symbol(X,P1-P2), symbol(X,I2-I3),
   pre_in_tree_dl(P2-P3,I1-I2,Left),
   pre_in_tree_dl(P3-P4,I3-I4,Right).

symbol(X,[X|Xs]-Xs).




% Isn't it cool? But the best of it is the performance!

% With the generate-and-test solution (p68c):
% ?- time(pre_in_tree(abdecfg,dbeacgf,_)).
% 9,048 inferences in 0.01 seconds (904800 Lips)  

% With the "pushed" generate-and-test solution (p68c):
% ?- time(pre_in_tree_push(abdecfg,dbeacgf,_)).
% 67 inferences in 0.00 seconds (Infinite Lips)
  
% With the difference list solution (p68d):
% ?- time(pre_in_tree_d(abdecfg,dbeacgf,_)).
% 32 inferences in 0.00 seconds (Infinite Lips)                     

% Note that the predicate pre_in_tree_dl/3 runs in almost any
% flow pattern. Try it out!


```


<br/>
<hr/>
<br/>
<h2 id="67"> 67. 4-Binary-Trees-P54/p69.pl</h2>

``` prolog
%  P69 (**) Dotstring representation of binary trees</B>

% The syntax of the dotstring representation is super simple:
%
% <tree> ::= . | <letter> <tree> <tree>

tree_dotstring(T,S) :- nonvar(T), !, tree_dots_dl(T,L-[]), atom_chars(S,L). 
tree_dotstring(T,S) :- atom(S), atom_chars(S,L), tree_dots_dl(T,L-[]).

tree_dots_dl(nil,L1-L2) :- symbol('.',L1-L2).
tree_dots_dl(t(X,Left,Right),L1-L4) :- 
   letter(X,L1-L2),
   tree_dots_dl(Left,L2-L3),
   tree_dots_dl(Right,L3-L4).

symbol(X,[X|Xs]-Xs).

letter(X,L1-L2) :- symbol(X,L1-L2), char_type(X,alpha).


```


<br/>
<hr/>
<br/>
<h2 id="68"> 68. 5-MultiwayTrees-P70B/p70.pl</h2>

``` prolog
% P70 (**) Multiway tree construction from a node string

% We suppose that the nodes of a multiway tree contain single
% characters. In the depth-first order sequence of its nodes, a
% special character ^ has been inserted whenever, during the
% tree traversal, the move is a backtrack to the previous level.

% Define the syntax of the string and write a predicate tree(String,Tree)
% to construct the Tree when the String is given. Work with atoms (instead
% of strings). Make your predicate work in both directions.
%

% Syntax in BNF:

% <tree> ::= <letter> <forest> '^'

% <forest> ::= | <tree> <forest> 


% First a nice solution using difference lists

tree(TS,T) :- atom(TS), !, atom_chars(TS,TL), tree_d(TL-[],T). % (+,?)
tree(TS,T) :- nonvar(T), tree_d(TL-[],T), atom_chars(TS,TL).   % (?,+)

tree_d([X|F1]-T, t(X,F)) :- forest_d(F1-['^'|T],F).

forest_d(F-F,[]).
forest_d(F1-F3,[T|F]) :- tree_d(F1-F2,T), forest_d(F2-F3,F).


% Another solution, not as elegant as the previous one.

tree_2(TS,T) :- atom(TS), !, atom_chars(TS,TL), tree_a(TL,T). % (+,?)
tree_2(TS,T) :- nonvar(T), tree_a(TL,T), atom_chars(TS,TL).   % (?,+)

tree_a(TL,t(X,F)) :- 
   append([X],FL,L1), append(L1,['^'],TL), forest_a(FL,F).

forest_a([],[]).
forest_a(FL,[T|Ts]) :- append(TL,TsL,FL), 
   tree_a(TL,T), forest_a(TsL,Ts).

```


<br/>
<hr/>
<br/>
<h2 id="69"> 69. 5-MultiwayTrees-P70B/p70B.pl</h2>

``` prolog
% P70B: Write a predicate istree/1 which succeeds if and only if its argument
%       is a Prolog term representing a multiway tree.
%
% istree(T) :- T is a term representing a multiway tree (i), (o)

% the following is a test case:
tree(1,t(a,[t(f,[t(g,[])]),t(c,[]),t(b,[t(d,[]),t(e,[])])])).

istree(t(_,F)) :- isforest(F).

isforest([]).
isforest([T|Ts]) :- istree(T), isforest(Ts).

```


<br/>
<hr/>
<br/>
<h2 id="70"> 70. 5-MultiwayTrees-P70B/p70C.pl</h2>

``` prolog
% P70C: Write a predicate nnodes/2 to count the nodes of a multiway tree.
%
% nnodes(T,N) :- the multiway tree T has N nodes (i,o))

% the following is a test case:
tree(1,t(a,[t(f,[t(g,[])]),t(c,[]),t(b,[t(d,[]),t(e,[])])])).

nnodes(t(_,F),N) :- nnodes(F,NF), N is NF+1.

nnodes([],0).
nnodes([T|Ts],N) :- nnodes(T,NT), nnodes(Ts,NTs), N is NT+NTs.

% Note that nnodes is called for trees and for forests. An early
% form of polymorphism!

% For the flow pattern (o,i) we can write:

nnodes2(t(_,F),N) :- N > 0, NF is N-1, nnodes2F(F,NF).

nnodes2F([],0).
nnodes2F([T|Ts],N) :- N > 0, 
   between(1,N,NT), nnodes2(T,NT), 
   NTs is N-NT, nnodes2F(Ts,NTs).

```


<br/>
<hr/>
<br/>
<h2 id="71"> 71. 5-MultiwayTrees-P70B/p71.pl</h2>

``` prolog
% P71 (*) Determine the internal path length of a tree

% We define the internal path length of a multiway tree as the
% total sum of the path lengths from the root to all nodes of the tree.

% ipl(Tree,L) :- L is the internal path length of the tree Tree
%    (multiway-tree, integer) (+,?)

ipl(T,L) :- ipl(T,0,L).

ipl(t(_,F),D,L) :- D1 is D+1, ipl(F,D1,LF), L is LF+D.

ipl([],_,0).
ipl([T1|Ts],D,L) :- ipl(T1,D,L1), ipl(Ts,D,Ls), L is L1+Ls.

% Notice the polymorphism: ipl is called with trees and with forests
% as first argument.


```


<br/>
<hr/>
<br/>
<h2 id="72"> 72. 5-MultiwayTrees-P70B/p72.pl</h2>

``` prolog
% P72 (*) Construct the bottom-up order sequence of the tree nodes

% bottom_up(Tree,Seq) :- Seq is the bottom-up sequence of the nodes of
%    the multiway tree Tree. (+,?)

bottom_up_f(t(X,F),Seq) :- 
	bottom_up_f(F,SeqF), append(SeqF,[X],Seq).

bottom_up_f([],[]).
bottom_up_f([T|Ts],Seq):-
	bottom_up_f(T,SeqT), bottom_up_f(Ts,SeqTs), append(SeqT,SeqTs,Seq).

% The predicate bottom_up/2 produces a stack overflow when called
% in the (-,+) flow pattern. There are two problems with that.
% First, the polymorphism does not work properly, because during
% decomposing the string, the program cannot guess whether it should
% construct a tree or a forest next. We can fix this using two
% separate predicates bottom_up_tree/2 and bottom_up_forset/2.
% Secondly, if we maintain the order of the subgoals, then
% the interpreter falls into an endless loop after finding the
% first solution. We can fix this by changing the order of the
% goals as follows:

bottom_up_tree(t(X,F),Seq) :-                        % (?,+)
	append(SeqF,[X],Seq), bottom_up_forest(F,SeqF).

bottom_up_forest([],[]).
bottom_up_forest([T|Ts],Seq):-
	append(SeqT,SeqTs,Seq),
	bottom_up_tree(T,SeqT), bottom_up_forest(Ts,SeqTs).

% Unfortunately, this version doesn't run in both directions either.

% In order to have a predicate which runs forward and backward, we
% have to determine the flow pattern and then call one of the above
% predicates, as follows:

bottom_up(T,Seq) :- nonvar(T), !, bottom_up_f(T,Seq).
bottom_up(T,Seq) :- nonvar(Seq), bottom_up_tree(T,Seq).

% This is not very elegant, I agree.
```


<br/>
<hr/>
<br/>
<h2 id="73"> 73. 5-MultiwayTrees-P70B/p73.pl</h2>

``` prolog
% P73 (**)  Lisp-like tree representation

% Here is my most elegant solution: a single predicate for both flow 
% patterns (i,o) and (o,i)

% tree_ltl(T,L) :- L is the "lispy token list" of the multiway tree T

tree_ltl(T,L) :- tree_ltl_d(T,L-[]).

% using difference lists

tree_ltl_d(t(X,[]),[X|L]-L) :- X \= '('.
tree_ltl_d(t(X,[T|Ts]),['(',X|L]-R) :- forest_ltl_d([T|Ts],L-[')'|R]).

forest_ltl_d([],L-L).
forest_ltl_d([T|Ts],L-R) :- tree_ltl_d(T,L-M), forest_ltl_d(Ts,M-R).
 
% some auxiliary predicates

write_ltl([]) :- nl.
write_ltl([X|Xs]) :- write(X), write(' '), write_ltl(Xs).

dotest(T) :- write(T), nl, tree_ltl(T,L),
   write_ltl(L), tree_ltl(T1,L), write(T1), nl.

test(1) :- T = t(a,[t(b,[]),t(c,[])]), dotest(T).
test(2) :- T = t(a,[t(b,[t(c,[])])]), dotest(T).
test(3) :- T = t(a,[t(f,[t(g,[])]),t(c,[]),t(b,[t(d,[]),t(e,[])])]), 
   dotest(T).

```


<br/>
<hr/>
<br/>
<h2 id="74"> 74. 6-Graphs-P80/p80.pl</h2>

``` prolog
% (**) P80 Conversions between graph representations

% We use the following notation:
%
% adjacency-list (alist): [n(b,[c,g,h]), n(c,[b,d,f,h]), n(d,[c,f]), ...]
%
% graph-term (gterm)  graph([b,c,d,f,g,h,k],[e(b,c),e(b,g),e(b,h), ...]) or
%                     digraph([r,s,t,u],[a(r,s),a(r,t),a(s,t), ...])
%
% edge-clause (ecl):  edge(b,g).  (in program database)
% arc-clause (acl):   arc(r,s).   (in program database)
%
% human-friendly (hf): [a-b,c,g-h,d-e]  or [a>b,h>g,c,b>a]
%
% The main conversion predicates are: alist_gterm/3 and human_gterm/2 which
% both (hopefully) work in either direction and for graphs as well as
% for digraphs, labelled or not.

% alist_gterm(Type,AL,GT) :- convert between adjacency-list and graph-term
%    representation. Type is either 'graph' or 'digraph'.
%    (atom,alist,gterm)  (+,+,?) or (?,?,+)

alist_gterm(Type,AL,GT):- nonvar(GT), !, gterm_to_alist(GT,Type,AL).
alist_gterm(Type,AL,GT):- atom(Type), nonvar(AL), alist_to_gterm(Type,AL,GT).

gterm_to_alist(graph(Ns,Es),graph,AL) :- memberchk(e(_,_,_),Es), ! ,
   lgt_al(Ns,Es,AL).
gterm_to_alist(graph(Ns,Es),graph,AL) :- !, 
   gt_al(Ns,Es,AL).
gterm_to_alist(digraph(Ns,As),digraph,AL) :- memberchk(a(_,_,_),As), !,
   ldt_al(Ns,As,AL).
gterm_to_alist(digraph(Ns,As),digraph,AL) :- 
   dt_al(Ns,As,AL).

% labelled graph
lgt_al([],_,[]).
lgt_al([V|Vs],Es,[n(V,L)|Ns]) :-
   findall(T,((member(e(X,V,I),Es) ; member(e(V,X,I),Es)),T = X/I),L),
   lgt_al(Vs,Es,Ns).

% unlabelled graph
gt_al([],_,[]).
gt_al([V|Vs],Es,[n(V,L)|Ns]) :-
   findall(X,(member(e(X,V),Es) ; member(e(V,X),Es)),L), gt_al(Vs,Es,Ns).

% labelled digraph
ldt_al([],_,[]).
ldt_al([V|Vs],As,[n(V,L)|Ns]) :-
   findall(T,(member(a(V,X,I),As), T=X/I),L), ldt_al(Vs,As,Ns).

% unlabelled digraph
dt_al([],_,[]).
dt_al([V|Vs],As,[n(V,L)|Ns]) :-
   findall(X,member(a(V,X),As),L), dt_al(Vs,As,Ns).


alist_to_gterm(graph,AL,graph(Ns,Es)) :- !, al_gt(AL,Ns,EsU,[]), sort(EsU,Es).
alist_to_gterm(digraph,AL,digraph(Ns,As)) :- al_dt(AL,Ns,AsU,[]), sort(AsU,As).

al_gt([],[],Es,Es).
al_gt([n(V,Xs)|Ns],[V|Vs],Es,Acc) :- 
   add_edges(V,Xs,Acc1,Acc), al_gt(Ns,Vs,Es,Acc1). 

add_edges(_,[],Es,Es).
add_edges(V,[X/_|Xs],Es,Acc) :- V @> X, !, add_edges(V,Xs,Es,Acc).
add_edges(V,[X|Xs],Es,Acc) :- V @> X, !, add_edges(V,Xs,Es,Acc).
add_edges(V,[X/I|Xs],Es,Acc) :- V @=< X, !, add_edges(V,Xs,Es,[e(V,X,I)|Acc]).
add_edges(V,[X|Xs],Es,Acc) :- V @=< X, add_edges(V,Xs,Es,[e(V,X)|Acc]).

al_dt([],[],As,As).
al_dt([n(V,Xs)|Ns],[V|Vs],As,Acc) :- 
   add_arcs(V,Xs,Acc1,Acc), al_dt(Ns,Vs,As,Acc1). 

add_arcs(_,[],As,As).
add_arcs(V,[X/I|Xs],As,Acc) :- !, add_arcs(V,Xs,As,[a(V,X,I)|Acc]).
add_arcs(V,[X|Xs],As,Acc) :- add_arcs(V,Xs,As,[a(V,X)|Acc]).

% ---------------------------------------------------------------------------

% ecl_to_gterm(GT) :- construct a graph-term from edge/2 facts in the
%    program database.

ecl_to_gterm(GT) :-
   findall(E,(edge(X,Y),E=X-Y),Es), human_gterm(Es,GT).

% acl_to_gterm(GT) :- construct a graph-term from arc/2 facts in the
%    program database.

acl_to_gterm(GT) :-
   findall(A,(arc(X,Y),A= >(X,Y)),As), human_gterm(As,GT).

% ---------------------------------------------------------------------------

% human_gterm(HF,GT) :- convert between human-friendly and graph-term
%    representation.
%    (list,gterm) (+,?) or (?,+)

human_gterm(HF,GT):- nonvar(GT), !, gterm_to_human(GT,HF).
human_gterm(HF,GT):- nonvar(HF), human_to_gterm(HF,GT).

gterm_to_human(graph(Ns,Es),HF) :-  memberchk(e(_,_,_),Es), !, 
   lgt_hf(Ns,Es,HF).
gterm_to_human(graph(Ns,Es),HF) :-  !, 
   gt_hf(Ns,Es,HF).
gterm_to_human(digraph(Ns,As),HF) :- memberchk(a(_,_,_),As), !, 
   ldt_hf(Ns,As,HF).
gterm_to_human(digraph(Ns,As),HF) :- 
   dt_hf(Ns,As,HF).

% labelled graph
lgt_hf(Ns,[],Ns).
lgt_hf(Ns,[e(X,Y,I)|Es],[X-Y/I|Hs]) :-
   delete(Ns,X,Ns1),
   delete(Ns1,Y,Ns2),
   lgt_hf(Ns2,Es,Hs).

% unlabelled graph
gt_hf(Ns,[],Ns).
gt_hf(Ns,[e(X,Y)|Es],[X-Y|Hs]) :-
   delete(Ns,X,Ns1),
   delete(Ns1,Y,Ns2),
   gt_hf(Ns2,Es,Hs).

% labelled digraph
ldt_hf(Ns,[],Ns).
ldt_hf(Ns,[a(X,Y,I)|As],[X>Y/I|Hs]) :-
   delete(Ns,X,Ns1),
   delete(Ns1,Y,Ns2),
   ldt_hf(Ns2,As,Hs).

% unlabelled digraph
dt_hf(Ns,[],Ns).
dt_hf(Ns,[a(X,Y)|As],[X>Y|Hs]) :-
   delete(Ns,X,Ns1),
   delete(Ns1,Y,Ns2),
   dt_hf(Ns2,As,Hs).

% we guess that if there is a '>' term then it's a digraph, else a graph
human_to_gterm(HF,digraph(Ns,As)) :- memberchk(_>_,HF), !, 
   hf_dt(HF,Ns1,As1), sort(Ns1,Ns), sort(As1,As).
human_to_gterm(HF,graph(Ns,Es)) :- 
   hf_gt(HF,Ns1,Es1), sort(Ns1,Ns), sort(Es1,Es).
% remember: sort/2 removes duplicates!

hf_gt([],[],[]).
hf_gt([X-Y/I|Hs],[X,Y|Ns],[e(U,V,I)|Es]) :- !, 
   sort0([X,Y],[U,V]), hf_gt(Hs,Ns,Es).
hf_gt([X-Y|Hs],[X,Y|Ns],[e(U,V)|Es]) :- !,
   sort0([X,Y],[U,V]), hf_gt(Hs,Ns,Es).
hf_gt([H|Hs],[H|Ns],Es) :- hf_gt(Hs,Ns,Es).

hf_dt([],[],[]).
hf_dt([X>Y/I|Hs],[X,Y|Ns],[a(X,Y,I)|As]) :- !, 
   hf_dt(Hs,Ns,As).
hf_dt([X>Y|Hs],[X,Y|Ns],[a(X,Y)|As]) :- !,
   hf_dt(Hs,Ns,As).
hf_dt([H|Hs],[H|Ns],As) :-  hf_dt(Hs,Ns,As).

sort0([X,Y],[X,Y]) :- X @=< Y, !.
sort0([X,Y],[Y,X]) :- X @> Y.

% tests ------------------------------------------------------------------

testdata([b-c, f-c, g-h, d, f-b, k-f, h-g]).
testdata([s > r, t, u > r, s > u, u > s, v > u]).
testdata([b-c/5, f-c/9, g-h/12, d, f-b/13, k-f/3, h-g/7]).
testdata([p>q/9, m>q/7, k, p>m/5]).
testdata([a,b(4711),c]).
testdata([a-b]).
testdata([]).

test :- 
   testdata(H1),
   write(H1), nl,
   human_gterm(H1,G1),
   alist_gterm(Type,AL,G1), 
   alist_gterm(Type,AL,G2),
   human_gterm(H2,G2),
   human_gterm(H2,G1),
   write(G1), nl, nl,
   fail.
test.
```


<br/>
<hr/>
<br/>
<h2 id="75"> 75. 6-Graphs-P80/p81.pl</h2>

``` prolog
% P81 (**) Path from one node to another one

% path(G,A,B,P) :- P is a (acyclic) path from node A to node B in the graph G.
%   G is given in graph-term form.
%   (+,+,+,?)

:- ensure_loaded(p80).  % conversions

path(G,A,B,P) :- path1(G,A,[B],P).

path1(_,A,[A|P1],[A|P1]).
path1(G,A,[Y|P1],P) :- 
   adjacent(X,Y,G), \+ memberchk(X,[Y|P1]), path1(G,A,[X,Y|P1],P).

% A useful predicate: adjacent/3

adjacent(X,Y,graph(_,Es)) :- member(e(X,Y),Es).
adjacent(X,Y,graph(_,Es)) :- member(e(Y,X),Es).
adjacent(X,Y,graph(_,Es)) :- member(e(X,Y,_),Es).
adjacent(X,Y,graph(_,Es)) :- member(e(Y,X,_),Es).
adjacent(X,Y,digraph(_,As)) :- member(a(X,Y),As).
adjacent(X,Y,digraph(_,As)) :- member(a(X,Y,_),As).


```


<br/>
<hr/>
<br/>
<h2 id="76"> 76. 6-Graphs-P80/p82.pl</h2>

``` prolog
% P82 (*) Cycle from a given node

% cycle(G,A,P) :- P is a closed path starting at node A in the graph G.
%    G is given in graph-term form.
%    (+,+,?)

:- ensure_loaded(p80).  % conversions
:- ensure_loaded(p81).  % adjacent/3 and path/4

cycle(G,A,P) :- 
   adjacent(B,A,G), path(G,A,B,P1), length(P1,L), L > 2, append(P1,[A],P).

```


<br/>
<hr/>
<br/>
<h2 id="77"> 77. 6-Graphs-P80/p83.pl</h2>

``` prolog
% P83 (**) Construct all spanning trees 

% s_tree(G,T) :- T is a spanning tree of the graph G
%    (graph-term graph-term) (+,?)

:- ensure_loaded(p80).  % conversions

s_tree(graph([N|Ns],GraphEdges),graph([N|Ns],TreeEdges)) :- 
   transfer(Ns,GraphEdges,TreeEdgesUnsorted),
   sort(TreeEdgesUnsorted,TreeEdges).

% transfer(Ns,GEs,TEs) :- transfer edges from GEs (graph edges)
%    to TEs (tree edges) until the list NS of still unconnected tree nodes
%    becomes empty. An edge is accepted if and only if one end-point is 
%    already connected to the tree and the other is not.

transfer([],_,[]).
transfer(Ns,GEs,[GE|TEs]) :- 
   select(GE,GEs,GEs1),        % modified 15-May-2001
   incident(GE,X,Y),
   acceptable(X,Y,Ns),
   delete(Ns,X,Ns1),
   delete(Ns1,Y,Ns2),
   transfer(Ns2,GEs1,TEs).

incident(e(X,Y),X,Y).
incident(e(X,Y,_),X,Y).

acceptable(X,Y,Ns) :- memberchk(X,Ns), \+ memberchk(Y,Ns), !.
acceptable(X,Y,Ns) :- memberchk(Y,Ns), \+ memberchk(X,Ns).

% An almost trivial use of the predicate s_tree/2 is the following
% tree tester predicate:
 
% is_tree(G) :- the graph G is a tree
is_tree(G) :- s_tree(G,G), !.


% Another use is the following connectivity tester:

% is_connected(G) :- the graph G is connected
is_connected(G) :- s_tree(G,_), !.

% Example graph p83.dat

test :-  
   see('p83.dat'), read(G), seen,
   human_gterm(H,G),
   write(H), nl, 
   setof(T,s_tree(G,T),Ts), length(Ts,N),
   write(N).

```


<br/>
<hr/>
<br/>
<h2 id="78"> 78. 6-Graphs-P80/p83_minimum.pl</h2>

``` prolog
% P83 (**) Construct all spanning trees 

% s_tree(G,T) :- T is a spanning tree of the graph G
%    (graph-term graph-term) (+,?)

:- ensure_loaded(p80).  % conversions

s_tree(graph([N|Ns],GraphEdges),graph([N|Ns],TreeEdges)) :- 
   transfer(Ns,GraphEdges,TreeEdgesUnsorted),
   sort(TreeEdgesUnsorted,TreeEdges).

% transfer(Ns,GEs,TEs) :- transfer edges from GEs (graph edges)
%    to TEs (tree edges) until the list NS of still unconnected tree nodes
%    becomes empty. An edge is accepted if and only if one end-point is 
%    already connected to the tree and the other is not.

transfer([],_,[]).
transfer(Ns,GEs,[GE|TEs]) :- 
   select(GE,GEs,GEs1),        % modified 15-May-2001
   incident(GE,X,Y),
   acceptable(X,Y,Ns),
   delete(Ns,X,Ns1),
   delete(Ns1,Y,Ns2),
   transfer(Ns2,GEs1,TEs).

incident(e(X,Y),X,Y).
incident(e(X,Y,_),X,Y).

acceptable(X,Y,Ns) :- memberchk(X,Ns), \+ memberchk(Y,Ns), !.
acceptable(X,Y,Ns) :- memberchk(Y,Ns), \+ memberchk(X,Ns).

% An almost trivial use of the predicate s_tree/2 is the following
% tree tester predicate:
 
% is_tree(G) :- the graph G is a tree
is_tree(G) :- s_tree(G,G), !.


% Another use is the following connectivity tester:

% is_connected(G) :- the graph G is connected
is_connected(G) :- s_tree(G,_), !.

% Example graph p83.dat

test :-  
   see('p83.dat'), read(G), seen,
   human_gterm(H,G),
   write(H), nl, 
   setof(T,s_tree(G,T),Ts), length(Ts,N),
   write(N).

```


<br/>
<hr/>
<br/>
<h2 id="79"> 79. 6-Graphs-P80/p84.pl</h2>

``` prolog
% P84 (**) Construct the minimal spanning tree of a labelled graph 

% ms_tree(G,T,S) :- T is a minimal spanning tree of the graph G.
%    S is the sum of the edge values. Prim's algorithm.
%    (graph-term graph-term) (+,?)

:- ensure_loaded(p80).  % conversions
:- ensure_loaded(p83).  % transfer/3, incident/3, and accept/3

ms_tree(graph([N|Ns],GraphEdges),graph([N|Ns],TreeEdges),Sum) :- 
   predsort(compare_edge_values,GraphEdges,GraphEdgesSorted),
   transfer(Ns,GraphEdgesSorted,TreeEdgesUnsorted),
   sort(TreeEdgesUnsorted,TreeEdges),
   edge_sum(TreeEdges,Sum).

compare_edge_values(Order,e(X1,Y1,V1),e(X2,Y2,V2)) :- 
	compare(Order,V1+X1+Y1,V2+X2+Y2).

edge_sum([],0).
edge_sum([e(_,_,V)|Es],S) :- edge_sum(Es,S1), S is S1 + V. 

% Example graph p84.dat

test :-  
   see('p84.dat'), read(G), seen,
   human_gterm(H,G),
   write(H), nl, 
   ms_tree(G,T,S),
	human_gterm(TH,T),
   write(S), nl,
	write(TH).

```


<br/>
<hr/>
<br/>
<h2 id="80"> 80. 6-Graphs-P80/p85.pl</h2>

``` prolog
% P85 (**) Graph isomorphism

:- ensure_loaded(p80).  % conversions

% This is a solution for graphs only. It is not difficult to write the 
% corresponding predicates for digraphs.

% isomorphic(G1,G2) :- the graphs G1 and G2 are isomorphic.

isomorphic(G1,G2) :- isomorphic(G1,G2,_). 

% isomorphic(G1,G2,Iso) :- the graphs G1 and G2 are isomorphic.  
%    Iso is a list representing the bijection between the node 
%    sets of the graphs. It is an open-ended list and contains 
%    a term i(X,Y) for each pair of corresponding nodes 

isomorphic(graph(Ns1,Es1),graph(Ns2,Es2),Iso):-
   append(Es1,Ns1,List1),
   append(Es2,Ns2,List2),
   isomo(List1,List2,Iso).

% isomo(List1,List2,Iso) :- the graphs represented by List1 and 
%    List2 are isomorphic. 

isomo([],[],_) :- !.
isomo([X|Xrest],Ys,Iso) :- 
   select(Y,Ys,Yrest),
   iso(X,Y,Iso),
   isomo(Xrest,Yrest,Iso).

% iso(E1,E2,Iso) :- the edge E1 in one graph corresponds 
%    to the edge E2 in the other. Note that edges are undirected.
% iso(N1,N2,Iso) :- matches isolated vertices.

iso(E1,E2,Iso) :- 
   edge(E1,X1,Y1), edge(E2,X2,Y2), 
   bind(X1,X2,Iso), bind(Y1,Y2,Iso).
iso(E1,E2,Iso) :- 
   edge(E1,X1,Y1), edge(E2,X2,Y2), 
   bind(X1,Y2,Iso), bind(Y1,X2,Iso).
iso(N1,N2,Iso) :-
   \+ edge(N1,_,_),\+ edge(N2,_,_),     % isolated vertices
   bind(N1,N2,Iso).

edge(e(X,Y),X,Y).
edge(e(X,Y,_),X,Y).

% bind(X,Y,Iso) :- it is possible to "bind X to Y" as part of the
%    bijection Iso; i.e. a term i(X,Y) is already in the list Iso,
%    or it can be added to it without violating the rules. Note that
%    bind(X,Y,Iso) makes sure that both X and Y are really "new"
%    if i(X,Y) is added to Iso.

bind(X,Y,Iso) :- memberchk(i(X,Y0),Iso), nonvar(Y0), !, Y = Y0.
bind(X,Y,Iso) :- memberchk(i(X0,Y),Iso), X = X0.

% ----------------------------------------------------------------------

test(1) :-
   human_gterm([f-e,e-d,e-g,c-e,c-b,a-b,c-d,beta],G1),
   human_gterm([6-3,6-4,3-4,alfa,4-5,7-4,6-2,1-2],G2),
   isomorphic(G1,G2,Iso), write(Iso).
test(2) :-
   human_gterm([f-e,e-d,e-g,c-e,c-b,a-b,c-d,beta],G1),
   human_gterm([6-3,6-4,3-4,4-5,7-4,6-2,1],G2),
   isomorphic(G1,G2,Iso), write(Iso).
test(3) :-
   human_gterm([a-b,c-d,e,d-f],G1),
   human_gterm([1-2,1-3,5,4-6],G2),
   isomorphic(G1,G2,Iso), write(Iso).
test(4):-
	human_gterm([a],G1),
	human_gterm([b],G2),
	write(G1),nl,
	write(G2),nl,
	isomorphic(G1,G2,Iso),write(Iso),nl.
test(5):-
	human_gterm([a-c],G1),
	human_gterm([b-d],G2),
	write(G1),nl,
	write(G2),nl,
	isomorphic(G1,G2,Iso),write(Iso),nl.
test(6):-
	human_gterm([a,c],G1),
	human_gterm([b,d],G2),
	write(G1),nl,
	write(G2),nl,
	isomorphic(G1,G2,Iso),write(Iso),nl.
```


<br/>
<hr/>
<br/>
<h2 id="81"> 81. 6-Graphs-P80/p86.pl</h2>

``` prolog
% P86 (**) Node degree and graph coloration

:- ensure_loaded(p80).  % conversions
:- ensure_loaded(p81).  % adjacent/3

% a) Write a predicate degree(Graph,Node,Deg) that determines the degree 
% of a given node. 

% degree(Graph,Node,Deg) :- Deg is the degree of the node Node in the
%    graph Graph.
%    (graph-term, node, integer), (+,+,?).

degree(graph(Ns,Es),Node,Deg) :- 
   alist_gterm(graph,AList,graph(Ns,Es)),
   member(n(Node,AdjList),AList), !,
   length(AdjList,Deg).

% --------------------------------------------------------------------------

% b) Write a predicate that generates a list of all nodes of a graph 
% sorted according to decreasing degree.

% degree_sorted_nodes(Graph,Nodes) :- Nodes is the list of the nodes
%    of the graph Graph, sorted according to decreasing degree.

degree_sorted_nodes(graph(Ns,Es),DSNodes) :- 
   alist_gterm(graph,AList,graph(Ns,Es)),  
   predsort(compare_degree,AList,AListDegreeSorted),
   reduce(AListDegreeSorted,DSNodes).

compare_degree(Order,n(N1,AL1),n(N2,AL2)) :-
   length(AL1,D1), length(AL2,D2),
   compare(Order,D2+N1,D1+N2).

% Note: compare(Order,D2+N1,D1+N2) sorts the nodes according to 
% decreasing degree, but alphabetically if the degrees are equal. Cool!

reduce([],[]).
reduce([n(N,_)|Ns],[N|NsR]) :- reduce(Ns,NsR).

% --------------------------------------------------------------------------

% c) Use Welch-Powell's algorithm to paint the nodes of a graph in such 
% a way that adjacent nodes have different colors.

% Use Welch-Powell's algorithm to paint the nodes of a graph
% in such a way that adjacent nodes have different colors.

paint(Graph,ColoredNodes) :-
   degree_sorted_nodes(Graph,DSNs),
   paint_nodes(Graph,DSNs,[],1,ColoredNodes).

% paint_nodes(Graph,Ns,AccNodes,Color,ColoNodes) :- paint the remaining
%    nodes Ns with a color number Color or higher. AccNodes is the set
%    of nodes already colored. Return the result in ColoNodes.
%    (graph-term,node-list,c-node-list,integer,c-node-list)
%    (+,+,+,+,-) 
paint_nodes(_,[],ColoNodes,_,ColoNodes) :- !.
paint_nodes(Graph,Ns,AccNodes,Color,ColoNodes) :-
   paint_nodes(Graph,Ns,Ns,AccNodes,Color,ColoNodes).
   
% paint_nodes(Graph,DSNs,Ns,AccNodes,Color,ColoNodes) :- paint the
%    nodes in Ns with a fixed color number Color, if possible.
%    If Ns is empty, continue with the next color number.
%    AccNodes is the set of nodes already colored. 
%    Return the result in ColoNodes.
%    (graph-term,node-list,c-node-list,c-node-list,integer,c-node-list)
%    (+,+,+,+,+,-) 
paint_nodes(Graph,Ns,[],AccNodes,Color,ColoNodes) :- !,
   Color1 is Color+1,
   paint_nodes(Graph,Ns,AccNodes,Color1,ColoNodes).
paint_nodes(Graph,DSNs,[N|Ns],AccNodes,Color,ColoNodes) :- 
   \+ has_neighbor(Graph,N,Color,AccNodes), !,
   delete(DSNs,N,DSNs1),
   paint_nodes(Graph,DSNs1,Ns,[c(N,Color)|AccNodes],Color,ColoNodes).
paint_nodes(Graph,DSNs,[_|Ns],AccNodes,Color,ColoNodes) :- 
   paint_nodes(Graph,DSNs,Ns,AccNodes,Color,ColoNodes).
   
has_neighbor(Graph,N,Color,AccNodes) :- 
   adjacent(N,X,Graph),
   memberchk(c(X,Color),AccNodes).

```


<br/>
<hr/>
<br/>
<h2 id="82"> 82. 6-Graphs-P80/p87.pl</h2>

``` prolog
% (**) P87 Depth-first order graph traversal

% Write a predicate that generates a depth-first order graph
% traversal sequence. The starting point should be specified,
% and the output should be a list of nodes that are reachable from
% this starting point (in depth-first order).

% The main problem is that if we traverse the graph recursively,
% we must store the encountered nodes in such a way that they
% do not disappear during the backtrack step.

% In this solution we use the "recorded database" which is a
% more efficient alternative to the well-known assert/retract 
% mechanism. See the SWI-Prolog manuals for details.

:- ensure_loaded(p80).  % conversions
:- ensure_loaded(p81).  % adjacent/3

depth_first_order(Graph,Start,Seq) :- 
   (Graph = graph(Ns,_), !; Graph = digraph(Ns,_)),
   memberchk(Start,Ns),
   clear_rdb(dfo),
   recorda(dfo,Start),
   (dfo(Graph,Start); true),
   bagof(X,recorded(dfo,X),Seq).

dfo(Graph,X) :-
   adjacent(X,Y,Graph), 
   \+ recorded(dfo,Y),
   recordz(dfo,Y),
   dfo(Graph,Y).

clear_rdb(Key) :-
   recorded(Key,_,Ref), erase(Ref), fail.
clear_rdb(_).



```


<br/>
<hr/>
<br/>
<h2 id="83"> 83. 6-Graphs-P80/p87a.pl</h2>

``` prolog
% (**) P87a Depth-first order graph traversal

% Write a predicate that generates a depth-first order graph
% traversal sequence. The starting point should be specified,
% and the output should be a list of nodes that are reachable from
% this starting point (in depth-first order).

% The main problem is that if we traverse the graph recursively,
% we must store the encountered nodes in such a way that they
% do not disappear during the backtrack step.

% In this solution we use the "recorded database" which is a
% more efficient alternative to the well-known assert/retract 
% mechanism. See the SWI-Prolog manuals for details.

% Alternative solution using acjacency list

:- ensure_loaded(p80).  % conversions

depth_first_order(Graph,Start,Seq) :- 
   alist_gterm(_,Alist,Graph),
   clear_rdb(dfo),
   dfo(Alist,Start),
   bagof(X,recorded(dfo,X),Seq).

dfo(_,X) :- recorded(dfo,X).
dfo(Alist,X) :-
   \+ recorded(dfo,X),
   recordz(dfo,X),
   memberchk(n(X,AdjNodes),Alist),
   Pred =.. [dfo,Alist],        % see remark below
   checklist(Pred,AdjNodes).

clear_rdb(Key) :-
   recorded(Key,_,Ref), erase(Ref), fail.
clear_rdb(_).

% The construction of the predicate Pred and the use of the checklist/2
% predefined predicate may seem strange at first. It is equivalent to 
% the following construction:
%
% dfo(_,X) :- recorded(dfo,X).
% dfo(Alist,X) :-
%    \+ recorded(dfo,X),
%    recordz(dfo,X),
%    memberchk(n(X,AdjNodes),Alist),
%    dfo_list(Alist,AdjNodes).
%
% dfo_list(_,[]).
% dfo_list(Alist,[A|As]) :- dfo(Alist,A), dfo_list(Alist,As).

```


<br/>
<hr/>
<br/>
<h2 id="84"> 84. 6-Graphs-P80/p88.pl</h2>

``` prolog
% P88 (**) Connected components

%  Write a predicate that splits a graph into its connected components.

:- ensure_loaded(p80).  % conversions
:- ensure_loaded(p81).  % path/4

% connected_components(G,Gs) :- Gs is the list of the connected components
%    of the graph G (only for graphs, not for digraphs!)
%    (gterm, list-of-gterms), (+,-)

connected_components(graph([],[]),[]) :- !.
connected_components(graph(Ns,Es),[graph(Ns1,Es1)|Gs]) :-
   Ns = [N|_],
   component(graph(Ns,Es),N,graph(Ns1,Es1)),
   subtract(Ns,Ns1,NsR),
   subgraph(graph(Ns,Es),graph(NsR,EsR)),
   connected_components(graph(NsR,EsR),Gs).

component(graph(Ns,Es),N,graph(Ns1,Es1)) :-
   Pred =..[is_path,graph(Ns,Es),N],
   sublist(Pred,Ns,Ns1),
   subgraph(graph(Ns,Es),graph(Ns1,Es1)).

is_path(Graph,A,B) :- path(Graph,A,B,_).

% subgraph(G,G1) :- G1 is a subgraph of G
subgraph(graph(Ns,Es),graph(Ns1,Es1)) :-
   subset(Ns1,Ns),
   Pred =.. [edge_is_compatible,Ns1],
   sublist(Pred,Es,Es1).

edge_is_compatible(Ns1,Z) :- 
   (Z = e(X,Y),!; Z = e(X,Y,_)),
   memberchk(X,Ns1), 
   memberchk(Y,Ns1). 

```


<br/>
<hr/>
<br/>
<h2 id="85"> 85. 6-Graphs-P80/p88a.pl</h2>

``` prolog
% P88 (**) Connected components

%  Write a predicate that splits a graph into its connected components.

:- ensure_loaded(p80).  % conversions
:- ensure_loaded(p87).  % depth_first_order/3

% connected_components(G,Gs) :- Gs is the list of the connected components
%    of the graph G (only for graphs, not for digraphs!)
%    (gterm, list-of-gterms), (+,-)

connected_components(graph([],[]),[]) :- !.
connected_components(graph(Ns,Es),[graph(Ns1,Es1)|Gs]) :-
   Ns = [N|_],
   component(graph(Ns,Es),N,graph(Ns1,Es1)),
   subtract(Ns,Ns1,NsR),
   subgraph(graph(Ns,Es),graph(NsR,EsR)),
   connected_components(graph(NsR,EsR),Gs).

component(graph(Ns,Es),N,graph(Ns1,Es1)) :-
   depth_first_order(graph(Ns,Es),N,Seq),
   sort(Seq,Ns1),
   subgraph(graph(Ns,Es),graph(Ns1,Es1)).

% subgraph(G,G1) :- G1 is a subgraph of G
subgraph(graph(Ns,Es),graph(Ns1,Es1)) :-
   subset(Ns1,Ns),
   Pred =.. [edge_is_compatible,Ns1],
   sublist(Pred,Es,Es1).

edge_is_compatible(Ns1,Z) :- 
   (Z = e(X,Y),!; Z = e(X,Y,_)),
   memberchk(X,Ns1), 
   memberchk(Y,Ns1). 

```


<br/>
<hr/>
<br/>
<h2 id="86"> 86. 6-Graphs-P80/p89.pl</h2>

``` prolog
% P89 (**) Bipartite graphs

%  Write a predicate that finds out whether a given graph is bipartite.

:- ensure_loaded(p80).  % conversions
:- ensure_loaded(p88).  % connected_components/2

% is_bipartite(G) :- the graph G is bipartite

is_bipartite(G) :- 
   connected_components(G,Gs),
   checklist(is_bi,Gs).

is_bi(graph(Ns,Es)) :- Ns = [N|_], 
   alist_gterm(_,Alist,graph(Ns,Es)),
   paint(Alist,[],red,N).

% paint(Alist,ColoredNodes,Color,ActualNode)
% (+,+,+,+)

paint(_,CNs,Color,N) :-  
   memberchk(c(N,Color),CNs), !.
paint(Alist,CNs,Color,N) :- 
   \+ memberchk(c(N,_),CNs),
   other_color(Color,OtherColor),
   memberchk(n(N,AdjNodes),Alist),
   Pred =.. [paint,Alist,[c(N,Color)|CNs],OtherColor],
   checklist(Pred,AdjNodes).

other_color(red,blue).
other_color(blue,red).

```


<br/>
<hr/>
<br/>
<h2 id="87"> 87. 7-Miscellaneous-Problems-P90/p90.pl</h2>

``` prolog
% P90 (**) Eight queens problem

% This is a classical problem in computer science. The objective is to
% place eight queens on a chessboard so that no two queens are attacking 
% each other; i.e., no two queens are in the same row, the same column, 
% or on the same diagonal. We generalize this original problem by 
% allowing for an arbitrary dimension N of the chessboard. 

% We represent the positions of the queens as a list of numbers 1..N.
% Example: [4,2,7,3,6,8,5,1] means that the queen in the first column
% is in row 4, the queen in the second column is in row 2, etc.
% By using the permutations of the numbers 1..N we guarantee that
% no two queens are in the same row. The only test that remains
% to be made is the diagonal test. A queen placed at column X and 
% row Y occupies two diagonals: one of them, with number C = X-Y, goes
% from bottom-left to top-right, the other one, numbered D = X+Y, goes
% from top-left to bottom-right. In the test predicate we keep track
% of the already occupied diagonals in Cs and Ds.   

% The first version is a simple generate-and-test solution.

% queens_1(N,Qs) :- Qs is a solution of the N-queens problem

queens_1(N,Qs) :- range(1,N,Rs), permu(Rs,Qs), test(Qs).

% range(A,B,L) :- L is the list of numbers A..B

range(A,A,[A]).
range(A,B,[A|L]) :- A < B, A1 is A+1, range(A1,B,L).

% permu(Xs,Zs) :- the list Zs is a permutation of the list Xs

permu([],[]).
permu(Qs,[Y|Ys]) :- del(Y,Qs,Rs), permu(Rs,Ys).

del(X,[X|Xs],Xs).
del(X,[Y|Ys],[Y|Zs]) :- del(X,Ys,Zs).

% test(Qs) :- the list Qs represents a non-attacking queens solution

test(Qs) :- test(Qs,1,[],[]).

% test(Qs,X,Cs,Ds) :- the queens in Qs, representing columns X to N,
% are not in conflict with the diagonals Cs and Ds

test([],_,_,_).
test([Y|Ys],X,Cs,Ds) :- 
	C is X-Y, \+ memberchk(C,Cs),
	D is X+Y, \+ memberchk(D,Ds),
	X1 is X + 1,
	test(Ys,X1,[C|Cs],[D|Ds]).

%--------------------------------------------------------------

% Now, in version 2, the tester is pushed completely inside the
% generator permu.

queens_2(N,Qs) :- range(1,N,Rs), permu_test(Rs,Qs,1,[],[]).

permu_test([],[],_,_,_).
permu_test(Qs,[Y|Ys],X,Cs,Ds) :- 
	del(Y,Qs,Rs), 
	C is X-Y, \+ memberchk(C,Cs),
	D is X+Y, \+ memberchk(D,Ds),
	X1 is X+1,
	permu_test(Rs,Ys,X1,[C|Cs],[D|Ds]).

```


<br/>
<hr/>
<br/>
<h2 id="88"> 88. 7-Miscellaneous-Problems-P90/p91.pl</h2>

``` prolog
% P91 (**) Knight's tour
% Another famous problem is this one: How can a knight jump on an
% NxN chessboard in such a way that it visits every square exactly once?

% knights(N,Knights) :- Knights is a knight's tour on a NxN chessboard 

knights(N,Knights) :- M is N*N-1,  knights(N,M,[1/1],Knights).

% closed_knights(N,Knights) :- Knights is a knight's tour on a NxN 
% chessboard which ends at the same square where it begun.

closed_knights(N,Knights) :- 
   knights(N,Knights), Knights = [X/Y|_], jump(N,X/Y,1/1). 

% knights(N,M,Visited,Knights) :- the list of squares Visited must be
% extended by M further squares to give the solution Knights of the
% NxN chessboard knight's tour problem. 

knights(_,0,Knights,Knights).
knights(N,M,Visited,Knights) :-
   Visited = [X/Y|_],
   jump(N,X/Y,U/V),
   \+ memberchk(U/V,Visited),
   M1 is M-1,
   knights(N,M1,[U/V|Visited],Knights).

% jumps on an NxN chessboard from square A/B to C/D
jump(N,A/B,C/D) :- 
   jump_dist(X,Y), 
   C is A+X, C > 0, C =< N,
   D is B+Y, D > 0, D =< N.

% jump distances
jump_dist(1,2).
jump_dist(2,1).
jump_dist(2,-1).
jump_dist(1,-2).
jump_dist(-1,-2).
jump_dist(-2,-1).
jump_dist(-2,1).
jump_dist(-1,2).


% A more user-friendly presentation of the results ------------------------

show_knights(N) :- 
   get_time(Time), convert_time(Time,Tstr),
   write('Start: '), write(Tstr), nl, nl,
   knights(N,Knights), nl, show(N,Knights).

show(N,Knights) :-
   get_time(Time), convert_time(Time,Tstr),
   write(Tstr), nl, nl,
   length(Chessboard,N),
   Pred =.. [invlength,N],
   checklist(Pred,Chessboard),
   fill_chessboard(Knights,Chessboard,1),
   checklist(show_row,Chessboard),
   nl, fail.

invlength(N,L) :- length(L,N).

show_row([]) :- nl.
show_row([S|Ss]) :- writef('%3r',[S]), show_row(Ss). 

fill_chessboard([],_,_).
fill_chessboard([X/Y|Ks],Chessboard,K) :-
   nth1(Y,Chessboard,Row),
   nth1(X,Row,K),
   K1 is K+1,
   fill_chessboard(Ks,Chessboard,K1).

% --------------------------------------------------------------------------



```


<br/>
<hr/>
<br/>
<h2 id="89"> 89. 7-Miscellaneous-Problems-P90/p92.pl</h2>

``` prolog
% P92 (***) Von Koch's conjecture

% Von Koch's Conjecture: Given a tree with N nodes (and hence N-1 edges).
% Find a way to enumerate the nodes from 1 to n and, accordingly, the
% edges from 1 to N-1 in such a way, that for each edge K the difference
% of its node numbers equals to K. The conjecture is that this is always
% possible.

% Example:      *      Solution:     4    Note that the node number 
%              /                    /     differences of adjacent nodes
%        * -- *               3 -- 1      are just the numbers 1,2,3,4
%        |     \              |     \     which can be used to enumerate
%        *      *             2      5    the edges.

:- ensure_loaded(p80).  % conversions
:- ensure_loaded(p83).  % is_tree

% vonkoch(G,Enum) :- the nodes of the graph G can be enumerated
%    as described in Enum. Enum is a list of pairs X/K, where X
%    is a node and K the corresponding number. 

vonkoch(Graph,Enum) :- 
   is_tree(Graph),            % check before doing too much work!
   Graph = graph(Ns,_),
   length(Ns,N),
   human_gterm(Hs,Graph),
   vonkoch(Hs,N,Enum).

vonkoch([IsolatedNode],1,[IsolatedNode/1|_]).  % special case
vonkoch(EdgeList,N,Enum) :-
   range(1,N,NodeNumberList), 
   N1 is N-1,range(1,N1,EdgeNumberList),
   bind(EdgeList,NodeNumberList,EdgeNumberList,Enum).

% The tree is given as an edge list; e.g. [d-a,a-g,b-c,e-f,b-e,a-b].
% Our problem is to find a bijection between the nodes (a,b,c,...) and
% the integer numbers 1..N which is compatible with the condition
% cited above. In order to construct this bijection, we use an open-
% ended list Enum; and we scan the given edge list.

bind([],_,_,_) :- !.
bind([V1-V2|Es],NodeNumbers,EdgeNumbers,Enum) :-
   bind_node(V1,K1,NodeNumbers,NodeNumbers1,Enum), 
   bind_node(V2,K2,NodeNumbers1,NodeNumbers2,Enum), 
   D is abs(K1-K2), select(D,EdgeNumbers,EdgeNumbers1), % modif 15-May-2001
   bind(Es,NodeNumbers2,EdgeNumbers1,Enum).

% bind_node(V,K,NodeNumsIn,NodeNumsOut,Enum) :-  
% V/K is an element of the list Enum, and there is no V1 \= V 
% such that V1/K is in Enum, and there is no K1 \= K such that 
% V =:= K1 is in Enum. In the case V gets a new number, it is
% selected from the set NodeNumsIn; what remains is NodeNumsOut.
% (node,integer,integer-list,integer-list,enumeration)  (+,?,+,-,?)

bind_node(V,K,NodeNumbers,NodeNumbers,Enum) :- 
   memberchk(V/K1,Enum), number(K1), !, K = K1.
bind_node(V,K,NodeNumbers,NodeNumbers1,Enum) :- 
   select(K,NodeNumbers,NodeNumbers1), memberchk(V/K,Enum).

% range(A,B,L) :- L is the list of numbers A..B

range(B,B,[B]) :- !.
range(A,B,[A|L]) :- A < B, A1 is A+1, range(A1,B,L).

% test suite ------------------------------------------------------------

test(K) :-
   test_tree(K,TH),
   write(TH), nl,  
   human_gterm(TH,T),
   vonkoch(T,Enum),
   write(Enum).
      
test_tree(1,[a-b,b-c,c-d,c-e]).
test_tree(2,[d-a,a-g,b-c,e-f,b-e,a-b]).
test_tree(3,[g-a,i-a,a-h,b-a,k-d,c-d,m-q,p-n,q-n,e-q,e-c,f-c,c-a]).
test_tree(4,[a]).

% Solution for the tree given in the problem statement (picture p92b.gif):
%
% ?- test(3).
% [g-a, i-a, a-h, b-a, k-d, c-d, m-q, p-n, q-n, e-q, e-c, f-c, c-a]
% [a/1, b/2, c/12, g/11, h/13, i/14, d/3, e/4, f/5, k/8, q/10, m/6, n/7, p/9|_]
%
% Remark: In most cases, there are many different solutions.

```


<br/>
<hr/>
<br/>
<h2 id="90"> 90. 7-Miscellaneous-Problems-P90/p93.pl</h2>

``` prolog
% P93 (***)  Arithmetic puzzle: Given a list of integer numbers, 
% find a correct way of inserting arithmetic signs such that 
% the result is a correct equation. The idea to the problem
% is from Roland Beuret. Thanx.

% Example: With the list of  numbers [2,3,5,7,11] we can form the
% equations  2-3+5+7 = 11  or  2 = (3*5+7)/11 (and ten others!).

% equation(L,LT,RT) :- L is the list of numbers which are the leaves
%    in the arithmetic terms LT and RT - from left to right. The 
%    arithmetic evaluation yields the same result for LT and RT.

equation(L,LT,RT) :-
   split(L,LL,RL),              % decompose the list L
   term(LL,LT),                 % construct the left term
   term(RL,RT),                 % construct the right term
   LT =:= RT.                   % evaluate and compare the terms

% term(L,T) :- L is the list of numbers which are the leaves in
%    the arithmetic term T - from left to right.

term([X],X).                    % a number is a term in itself
term([X],-X).                   % unary minus
term(L,T) :-                    % general case: binary term
   split(L,LL,RL),              % decompose the list L
   term(LL,LT),                 % construct the left term
   term(RL,RT),                 % construct the right term
   binterm(LT,RT,T).            % construct combined binary term

% binterm(LT,RT,T) :- T is a combined binary term constructed from
%    left-hand term LT and right-hand term RT

binterm(LT,RT,LT+RT).
binterm(LT,RT,LT-RT).
binterm(LT,RT,LT*RT).
binterm(LT,RT,LT/RT) :- RT =\= 0.   % avoid division by zero

% split(L,L1,L2) :- split the list L into non-empty parts L1 and L2
%    such that their concatenation is L

split(L,L1,L2) :- append(L1,L2,L), L1 = [_|_], L2 = [_|_].

% do(L) :- find all solutions to the problem as given by the list of
%    numbers L, and print them out, one solution per line.

do(L) :- 
   equation(L,LT,RT),
      writef('%w = %w\n',[LT,RT]),
   fail.
do(_).


% Try the following goal:   ?- do([2,3,5,7,11]).

```


<br/>
<hr/>
<br/>
<h2 id="91"> 91. 7-Miscellaneous-Problems-P90/p94.pl</h2>

``` prolog
% P94 (**) Generate K-regular simple graphs with N nodes. 
% 
% In a K-regular graph all nodes have a degree of K.

% k_regular(K,N,Graph) :- Graph is a K-regular simple graph with N nodes.
% The graph is in graph-term form. The nodes are identified by numbers 1..N.
% All solutions can be generated via backtracking. 
% (+,+,?)  (int,int,graph(nodes,edges))
%
% Note: The predicate generates the Nodes list and a list of terms u(V,F)
% which indicates, for each node V, the number F of unused (or free) edges. 
% For example: with N=5, K=3 the algorithm starts with Nodes=[1,2,3,4,5]
% and UList=[u(1,3),u(2,3),u(3,3),u(4,3),u(5,3)].

k_regular(K,N,graph(Nodes,Edges)) :-
   range(1,N,Nodes),                         % generate Nodes list
   maplist(mku(K),Nodes,UList),              % generate initial UList
   k_reg(UList,0,Edges).

mku(K,V,u(V,K)).

% k_reg(UList,MinY,Edges) :- Edges is a list of e(X,Y) terms where u(X,UX)
% is the first element in UList and u(Y,UY) is another element of UList,
% with Y > MinY. Both UX and UY, which indicate the number of free edges
% of X and Y, respectively, must be greater than 0. They are both reduced
% by 1 for the recursion if the edge e(X,Y) is chosen. 
% (+,+,-) (ulist,int,elist)

k_reg([],_,[]). 
k_reg([u(_,0)|Us],_,Edges) :- !, k_reg(Us,0,Edges).   % no more unused edges
k_reg([u(1,UX)|Us],MinY,[e(1,Y)|Edges]) :- UX > 0,    % special case X = 1
   pick(Us,Y,MinY,Us1), !,                    % pick a Y
   UX1 is UX - 1,                             % reduce number of unused edges
   k_reg([u(1,UX1)|Us1],Y,Edges).
k_reg([u(X,UX)|Us],MinY,[e(X,Y)|Edges]) :- X > 1, UX > 0,
   pick(Us,Y,MinY,Us1),                       % pick a Y
   UX1 is UX - 1,                             % reduce number of unused edges
   k_reg([u(X,UX1)|Us1],Y,Edges).

% pick(UList_in,Y,MinY,UList_out) :- there is an element u(Y,UY) in UList_in,
% Y is greater than MinY, and UY > 0. UList_out is obtained from UList_in
% by reducing UY by 1 in the term u(Y,_). This predicate delivers all
% possible values of Y via backtracking.
% (+,-,+,-) (ulist,int,int,ulist)

pick([u(Y,UY)|Us],Y,MinY,[u(Y,UY1)|Us]) :- Y > MinY, UY > 0, UY1 is UY - 1.
pick([U|Us],Y,MinY,[U|Us1]) :- pick(Us,Y,MinY,Us1).
   
% range(X,Y,Ls) :- Ls is the list of the integer numbers from X to Y.
% (+,+,?) (int,int,int_list)

range(B,B,[B]).
range(A,B,[A|L]) :- A < B, A1 is A + 1, range(A1,B,L).

:- dynamic solution/1.

% all_k_regular(K,N,Gs) :- Gs is the list of all (non-isomorphic)
% K-regular graphs with N nodes.
% (+,+,-) (int,int,list_of_graphs)
% Note: The predicate prints each new solution as a progress report.
% Use tell('/dev/null') to switch off the printing if you don't like it.

all_k_regular(K,N,_) :-
   retractall(solution(_)),
   k_regular(K,N,Graph),
   no_iso_solution(Graph),
   write(Graph), nl,
   assert(solution(Graph)),
   fail.
all_k_regular(_,_,Graphs) :- findall(G,solution(G),Graphs).

:- ensure_loaded(p85).  % load isomorphic/2

% no_iso_solution(Graph) :- there is no graph G in the solution/1 data base
% predicate which is isomorphic to Graph

no_iso_solution(Graph) :-
   solution(G), isomorphic(Graph,G), !, fail.
no_iso_solution(_).

% The rest of this program constructs a table of K-regular simple graphs
% with N nodes for N up to a maximum N and sensible values of K.
% Example:  ?- table(6).

table(Max) :-  
   nl, write('K-regular simple graphs with N nodes'), nl,
   table(3,Max).

table(N,Max) :- N =< Max, !,
   table(2,N,Max),
   N1 is N + 1,
   table(N1,Max).
table(_,_) :- nl. 

table(K,N,Max) :- K < N, !,
   tell('/dev/null'),
   statistics(inferences,I1),
   all_k_regular(K,N,Gs),
   length(Gs,NSol),    
   statistics(inferences,I2),
   NInf is I2 - I1,
   told,
   plural(NSol,Pl),
   writef('\nN = %w  K = %w   %w solution%w  (%w inferences)\n',[N,K,NSol,Pl,NInf]),
   checklist(print_graph,Gs),
   K1 is K + 1,
   table(K1,N,Max).
table(_,_,_) :- nl.

plural(X,' ') :- X < 2, !.
plural(_,'s').

:- ensure_loaded(p80).  % conversion human_gterm/2

print_graph(G) :- human_gterm(HF,G), write(HF), nl.
   

```


<br/>
<hr/>
<br/>
<h2 id="92"> 92. 7-Miscellaneous-Problems-P90/p95.pl</h2>

``` prolog
% P95 (**) English number words
% On financial documents, like cheques, numbers must sometimes be 
% written in full words. Example: 175 must be written as one-seven-five.
% Write a predicate full_words/1 to print (non-negative) integer numbers
% in full words.

% full_words(N) :- print the number N in full words (English)
% (non-negative integer) (+)

full_words(0) :- !, write(zero), nl.
full_words(N) :- integer(N), N > 0, full_words1(N), nl.

full_words1(0) :- !.
full_words1(N) :- N > 0,
   Q is N // 10, R is N mod 10,
   full_words1(Q), numberword(R,RW), hyphen(Q), write(RW).

hyphen(0) :- !.
hyphen(Q) :- Q > 0, write('-'). 

numberword(0,zero).
numberword(1,one).
numberword(2,two).
numberword(3,three).
numberword(4,four).
numberword(5,five).
numberword(6,six).
numberword(7,seven).
numberword(8,eight).
numberword(9,nine).

```


<br/>
<hr/>
<br/>
<h2 id="93"> 93. 7-Miscellaneous-Problems-P90/p96.pl</h2>

``` prolog
% P96 (**) Syntax checker for Ada identifiers

% A purely recursive syntax is:
%
% <identifier> ::= <letter> <rest>
%
% <rest> ::=  | <optional_underscore> <letter_or_digit> <rest>
%
% <optional_underscore> ::=  | '_'
%
% <letter_or_digit> ::= <letter> | <digit>

% identifier(Str) :- Str is a legal Ada identifier
%    (atom) (+)

identifier(S) :- atom(S), atom_chars(S,L), identifier(L).

identifier([X|L]) :- char_type(X,alpha), rest(L).

rest([]) :- !.
rest(['_',X|L]) :- !, letter_or_digit(X), rest(L).
rest([X|L]) :- letter_or_digit(X), rest(L).

letter_or_digit(X) :- char_type(X,alpha), !.
letter_or_digit(X) :- char_type(X,digit).

% Try also a solution with difference lists!
% See p96a.pl

```


<br/>
<hr/>
<br/>
<h2 id="94"> 94. 7-Miscellaneous-Problems-P90/p96a.pl</h2>

``` prolog
% P96 (**) Syntax checker for Ada identifiers (Difference lists)

% A purely recursive syntax is:
%
% <identifier> ::= <letter> <rest>
%
% <rest> ::=  | <optional_underscore> <letter_or_digit> <rest>
%
% <optional_underscore> ::=  | '_'
%
% <letter_or_digit> ::= <letter> | <digit>

% identifier(Str) :- Str is a legal Ada identifier
%    (atom) (+)

identifier(S) :- atom(S), atom_chars(S,L), identifier(L-[]).

identifier([X|L]-R) :- char_type(X,alpha), rest(L-R).

rest(T-T) :- !.
rest(L-R) :- 
   optional_underscore(L-L1),
   letter_or_digit(L1-L2),
   rest(L2-R).

optional_underscore(['_'|R]-R) :- !.
optional_underscore(T-T).

letter_or_digit([X|R]-R) :- char_type(X,alpha), !.
letter_or_digit([X|R]-R) :- char_type(X,digit).

% This solution with difference lists is not more elegant as the 
% simple solution with ordinary lists (see p96.pl), because the
% parsing is done by always removing just one or two characters
% from the head of the list. Take this as an easy exercise!

```


<br/>
<hr/>
<br/>
<h2 id="95"> 95. 7-Miscellaneous-Problems-P90/p97.pl</h2>

``` prolog
%  P97 (**) Sudoku
%  
%  Sudoku puzzles go like this:  

%   Problem statement                Solution

%    .  .  4 | 8  .  . | .  1  7     9  3  4 | 8  2  5 | 6  1  7	     
%            |         |                     |         |
%    6  7  . | 9  .  . | .  .  .     6  7  2 | 9  1  4 | 8  5  3
%            |         |                     |         |
%    5  .  8 | .  3  . | .  .  4     5  1  8 | 6  3  7 | 9  2  4
%    --------+---------+--------     --------+---------+--------
%    3  .  . | 7  4  . | 1  .  .     3  2  5 | 7  4  8 | 1  6  9
%            |         |                     |         |
%    .  6  9 | .  .  . | 7  8  .     4  6  9 | 1  5  3 | 7  8  2
%            |         |                     |         |
%    .  .  1 | .  6  9 | .  .  5     7  8  1 | 2  6  9 | 4  3  5
%    --------+---------+--------     --------+---------+--------
%    1  .  . | .  8  . | 3  .  6     1  9  7 | 5  8  2 | 3  4  6
%            |         |                     |         |
%    .  .  . | .  .  6 | .  9  1     8  5  3 | 4  7  6 | 2  9  1
%            |         |                     |         |
%    2  4  . | .  .  1 | 5  .  .     2  4  6 | 3  9  1 | 5  7  8

% Every spot in the puzzle belongs to a (horizontal) row and a (vertical)
% column, as well as to one single 3x3 square (which we call "square" 
% for short). At the beginning, some of the spots carry a single-digit
% number between 1 and 9. The problem is to fill the missing spots with
% digits in such a way that every number between 1 and 9 appears exactly
% once in each row, in each column, and in each square.

% We represent the Sudoku puzzle as a simple list of 81 digits. At
% the beginning, the list is partially instantiated. During the 
% process, all the elememts of the list get instantiated with digits.

% We are going to treat each spot as a Prolog term spot(X,R,C,S) where
% X is the number to put into the field, R is the row, C the column, and
% S the square the field belongs to. R, C, and S are lists which represent
% the respective number sets.

% --------------------------------------------------------------------------

% sudoku(Puzzle) :- solve the given Sudoku puzzle and print the
%    problem statement as well as the solution to the standard output
%   (list-of-integers, partially instantiated)

sudoku(Puzzle) :- 
   printPuzzle(Puzzle), nl, 
   connect(Spots),
   flag(counter,_,0),
   init(Puzzle,Spots),
   solve(Spots),
   printPuzzle(Puzzle),
   flag(counter,N,N+1),
   fail.
sudoku(_) :- 
   flag(counter,N,N), nl, 
   printCounter(N).

% ---------------------------------------------------------------

% The most difficult part of the problem solution is to prepare 
% the list of spot/4 terms representing the spots in the puzzle.
% We have to make sure that every spot "knows" its row, column,
% and square. In other words, all the spots in a row access the
% same list in order to check whether a new number can be placed
% in the row. The same is true for the columns and the squares. 

% connect(Spots) :- Spots = [spot(_,R1,C1,S1),spot(_,R1,C2,S1),.....].

connect(Spots) :- 
   length(Spots,81),
   connectRows(Spots),
   connectCols(Spots),
   connectSquares(Spots).

% connectRows(Spots) :- connect the spots of all rows in the list Spot

connectRows([]).
connectRows(Spots) :- 
   connectRow(Spots,_,9),
   skip(Spots,9,Spots1), 
   connectRows(Spots1).

% connectRow(Spots,R,K) :- the first K elements of Spot
% are in the same row R

connectRow(_,_,0).
connectRow([spot(_,R,_,_)|Spots],R,K) :- K > 0,
   K1 is K-1, connectRow(Spots,R,K1).

% connectCols(Spots) :- connect the spots of the same column

connectCols(Spots) :- connectCols(Spots,9).

% connectCols(Spots,K) :- connect K more columns columns

connectCols(_,0) :- !.
connectCols(Spots,K) :- K > 0,
   connectCol(Spots,_),
   skip(Spots,1,Spots1), 
   K1 is K-1, connectCols(Spots1,K1).

% connectCol(Spots,C) :- connect the first spot in Spots with
% the other spots in its column C

connectCol([],_).
connectCol([spot(_,_,C,_)|Spots],C) :-
   skip(Spots,8,Spots1),
   connectCol(Spots1,C).

% connectSquares(Spots) :- connect all three bands
% The nine squares are arranged in three horizontal bands,
% three squares in each band. 
   
connectSquares(Spots) :- 
   connectBand(Spots),
   skip(Spots,27,Spots1),
   connectBand(Spots1),
   skip(Spots1,27,Spots2),
   connectBand(Spots2).

% connectBand(Spots) :- connect the next band (of three squares

connectBand(Spots) :- 
   connectSq(Spots,_),
   skip(Spots,3,Spots1),
   connectSq(Spots1,_),
   skip(Spots1,3,Spots2),
   connectSq(Spots2,_).

% connectSq(Spots,S) :- connect the spots of square S. In the Spots
%    list each square is composed of three spot-triplets which 
%    are separated by 6 spots. 

connectSq([],_).
connectSq(Spots,S) :- 
  connectTriplet(Spots,S),
  skip(Spots,9,Spots1),
  connectTriplet(Spots1,S),
  skip(Spots1,9,Spots2),
  connectTriplet(Spots2,S).

% connectTriplet(Spots,S) :- connect the next three spots in the Spots
%    list with the square S

connectTriplet([spot(_,_,_,S),spot(_,_,_,S),spot(_,_,_,S)|_],S).

% skip(List,N,List1) :- skip the first N elements from a List
%    and return the rest of the list in List1. If the List is
%    too short, then succeed and return [] as rest.

skip([],_,[]) :- !. 
skip(Xs,0,Xs) :- !.
skip([_|Xs],K,Zs) :- K > 0, K1 is K-1, skip(Xs,K1,Zs). 

% -----------------------------------------------------------

% init(Puzzle,Spots) :- initialize the Spots list on the
%    basis of the problem statement (Puzzle) and link the
%    Puzzle list to the Spots list 

init([],[]).
init([X|Xs],[Sp|Spots]) :- initSpot(X,Sp), init(Xs,Spots).

% If X is not instantiated in the given puzzle, create a link
% between the variable in the puzzle and the corresponding 
% variable in the spot. Otherwise copy the given number from 
% the puzzle into the spot and insert it into the spot's 
% correct row, column, and square, if this is legal.

initSpot(X,spot(X,_,_,_)) :- var(X), !.
initSpot(X,spot(X,R,C,S)) :- integer(X),
   insert(X,R),
   insert(X,C),
   insert(X,S).

% ----------------------------------------------------------

% solve(Spots) :- solve the problem using backtrack

solve([]).
solve([Spot|Spots]) :- bind(Spot), solve(Spots).

% bind(Spot) :- bind the data field in Spot to an 
% available non-conflicting digit.

bind(spot(X,_,_,_)) :- nonvar(X), !.
bind(spot(X,R,C,S)) :- var(X),
   between(1,9,X),  % try a digit
   insert(X,R),
   insert(X,C),
   insert(X,S).

% ---------------------------------------------------------

% insert(X,L) :- X can be inserted into the list without 
% conflict, i.e. X is not yet in the list, when insert/2
% is called. Otherwise the predicate fails.

insert(X,L) :- var(L), !, L = [X|_].
insert(X,[Y|Ys]) :- X \= Y, insert(X,Ys).

% ---------------------------------------------------------

printPuzzle([]).
printPuzzle(Xs) :- nl,
   printBand(Xs,Xs1),
   write('--------+---------+--------'), nl,
   printBand(Xs1,Xs2),
   write('--------+---------+--------'), nl,
   printBand(Xs2,_).

printBand(Xs,Xs3) :- 
   printRow(Xs,Xs1), nl,
   write('        |         |'), nl, 
   printRow(Xs1,Xs2), nl,
   write('        |         |'), nl, 
   printRow(Xs2,Xs3), nl.
 
printRow(Xs,Xs3) :-
   printTriplet(Xs,Xs1), write(' | '),
   printTriplet(Xs1,Xs2), write(' | '),
   printTriplet(Xs2,Xs3).

printTriplet(Xs,Xs3) :-
   printElement(Xs,Xs1), write('  '),
   printElement(Xs1,Xs2), write('  '),
   printElement(Xs2,Xs3).

printElement([X|Xs],Xs) :- var(X), !, write('.').
printElement([X|Xs],Xs) :- write(X).

printCounter(0) :- !, write('No solution'), nl.
printCounter(1) :- !, write('1 solution'), nl.
printCounter(K) :- write(K), write(' solutions'), nl.

% ---------------------------------------------------------

test(N) :- puzzle(N,P), sudoku(P).

puzzle(1,P) :- 
   P = [_,_,4,8,_,_,_,1,7, 6,7,_,9,_,_,_,_,_, 5,_,8,_,3,_,_,_,4,
        3,_,_,7,4,_,1,_,_, _,6,9,_,_,_,7,8,_, _,_,1,_,6,9,_,_,5,
	1,_,_,_,8,_,3,_,6, _,_,_,_,_,6,_,9,1, 2,4,_,_,_,1,5,_,_].

puzzle(2,P) :- 
   P = [3,_,_,_,7,1,_,_,_, _,5,_,_,_,_,1,8,_, _,4,_,8,_,_,_,_,_,
	_,_,6,2,_,_,3,_,_, _,_,1,_,5,_,8,_,_, _,_,3,_,_,8,2,_,_,
        _,_,_,_,_,3,_,4,_, _,6,4,_,_,_,_,7,_, _,_,_,9,6,_,_,_,1].

puzzle(3,P) :-
   P = [1,7,_,_,_,9,_,_,4, _,_,_,_,_,_,7,_,_, 5,_,_,3,_,_,2,_,_,
        _,8,_,_,_,_,5,3,6, _,_,_,_,8,_,_,_,_, 6,9,1,_,_,_,_,8,_,
        _,_,7,_,_,4,_,_,2, _,_,2,_,_,_,_,_,_, 3,_,_,5,_,_,_,7,1].

% an example with many solutions

puzzle(4,P) :-
   P = [1,_,_,_,_,9,_,_,4, _,_,_,_,_,_,7,_,_, 5,_,_,3,_,_,2,_,_,
        _,8,_,_,_,_,5,_,6, _,_,_,_,8,_,_,_,_, 6,9,1,_,_,_,_,8,_,
        _,_,7,_,_,4,_,_,2, _,_,2,_,_,_,_,_,_, 3,_,_,5,_,_,_,7,1].

puzzle(5,P) :- 
   P = [_,6,5,_,_,_,7,2,_, 3,_,7,_,_,_,1,_,8, 2,9,_,_,1,_,_,3,4,
        _,_,_,5,_,7,_,_,_, _,_,1,_,_,_,8,_,_, _,_,_,2,_,1,_,_,_,
        8,1,_,_,2,_,_,5,7, 7,_,2,_,_,_,9,_,1, _,5,4,_,_,_,6,8,_].

puzzle(6,P) :- 
   P = [5,_,2,_,_,3,_,_,_, 4,6,_,_,7,_,9,_,_, _,_,3,4,_,_,_,_,_,
        9,5,_,_,6,_,_,_,_, _,4,_,_,_,_,_,9,_, _,_,_,_,9,_,_,1,7,
        _,_,_,_,_,7,2,_,_, _,_,9,_,4,_,_,3,5, _,_,_,3,_,_,7,_,6].

% an example with an error in the problem statement (5 appears
% twice in the top left square)

puzzle(e1,P) :- 
   P = [5,_,2,_,_,3,_,_,_, 4,6,5,_,7,_,9,_,_, _,_,3,4,_,_,_,_,_,
        9,5,_,_,6,_,_,_,_, _,4,_,_,_,_,_,9,_, _,_,_,_,9,_,_,1,7,
        _,_,_,_,_,7,2,_,_, _,_,9,_,4,_,_,3,5, _,_,_,3,_,_,7,_,6].

% another example with an error in the problem statement (garbage
% in the first row

puzzle(e2,P) :- 
   P = [x,_,2,_,_,3,_,_,_, 4,6,_,_,7,_,9,_,_, _,_,3,4,_,_,_,_,_,
        9,5,_,_,6,_,_,_,_, _,4,_,_,_,_,_,9,_, _,_,_,_,9,_,_,1,7,
        _,_,_,_,_,7,2,_,_, _,_,9,_,4,_,_,3,5, _,_,_,3,_,_,7,_,6].

% some more examples from the Sonntagszeitung

puzzle(8,P) :- 
   P = [4,8,_,_,7,_,_,_,_, _,_,9,6,8,_,3,_,7, 3,_,7,4,_,_,_,5,_,
        _,_,_,3,_,_,_,2,_, 9,5,_,7,2,1,_,6,8, _,1,_,_,_,4,_,_,_,
        _,4,_,_,_,2,7,_,1, 8,_,2,_,4,7,5,_,_, _,_,_,_,5,_,_,8,4].

puzzle(9,P) :- 
   P = [_,1,_,_,_,_,_,2,4, 5,_,_,_,4,_,_,8,6, 6,_,4,1,_,_,_,_,_,
        _,_,_,8,_,6,9,_,_, 8,_,_,_,_,_,_,_,2, _,_,6,4,_,3,_,_,_,
        _,_,_,_,_,7,2,_,8, 1,6,_,_,9,_,_,_,5, 7,4,_,_,_,_,_,9,_].

puzzle(10,P) :-
   P = [_,9,7,_,_,5,_,_,4, _,_,_,_,_,9,_,_,_, _,_,5,_,4,_,2,_,7,
        _,8,6,_,_,3,_,_,_, _,_,_,_,2,_,_,_,_, _,_,_,5,_,_,3,4,_,
        5,_,3,_,7,_,6,_,_, _,_,_,6,_,_,_,_,_, 9,_,_,8,_,_,1,7,_].

% a puzzle rated "not fun" by 
% http://dingo.sbs.arizona.edu/~sandiway/sudoku/examples.html 

puzzle(11,P) :-
   P = [_,2,_,_,_,_,_,_,_, _,_,_,6,_,_,_,_,3, _,7,4,_,8,_,_,_,_,
	_,_,_,_,_,3,_,_,2, _,8,_,_,4,_,_,1,_, 6,_,_,5,_,_,_,_,_,
        _,_,_,_,1,_,7,8,_, 5,_,_,_,_,9,_,_,_, _,_,_,_,_,_,_,4,_].

% a "super hard puzzle" by
% http://www.menneske.no/sudoku/eng/showpuzzle.html?number=2155141

puzzle(12,P) :-
   P = [_,_,_,6,_,_,4,_,_, 7,_,_,_,_,3,6,_,_, _,_,_,_,9,1,_,8,_,
        _,_,_,_,_,_,_,_,_, _,5,_,1,8,_,_,_,3, _,_,_,3,_,6,_,4,5,
        _,4,_,2,_,_,_,6,_, 9,_,3,_,_,_,_,_,_, _,2,_,_,_,_,1,_,_].

% some puzzles from Spektrum der Wissenschaft 3/2006, p.100

% leicht
puzzle(13,P) :-
   P = [_,2,6,4,5,8,3,_,_, 1,7,_,_,_,_,_,4,_, _,8,_,_,_,_,_,_,_,
	_,_,_,_,_,_,9,8,_, _,_,_,5,9,_,1,_,4, 7,_,_,2,_,1,_,5,_,
	_,_,_,_,4,_,_,3,_, _,_,_,8,_,_,5,_,_, 6,_,_,_,_,7,_,9,1].

% mittel
puzzle(14,P) :-
   P = [9,_,_,6,3,_,_,_,4, _,1,_,2,5,8,_,_,_, _,_,_,7,_,_,_,_,8,
        6,4,_,_,2,_,5,_,_, _,_,_,_,_,_,_,_,_, 8,2,_,5,_,_,_,9,_,
	_,_,_,_,_,_,8,7,_, 3,_,_,_,_,5,_,4,_, _,_,1,_,7,6,_,_,_].

% schwer
puzzle(15,P) :-
   P = [_,_,_,_,_,_,_,_,7, _,_,_,_,_,_,6,3,4, _,_,_,9,4,_,_,2,_,
        5,_,1,7,_,_,8,6,_, _,_,9,_,_,_,_,_,3, _,_,_,_,8,_,_,_,_,
	4,3,_,5,_,_,_,_,_, _,1,_,_,6,8,_,_,_, _,_,_,_,_,3,1,_,9].

% hoellisch (!)
puzzle(16,P) :-
   P = [_,_,_,_,3,_,_,_,_, _,1,5,_,_,_,6,_,_, 6,_,_,2,_,_,3,4,_,
        _,_,_,6,_,_,_,8,_, _,3,9,_,_,_,5,_,_, 5,_,_,_,_,_,9,_,2,
	_,_,_,_,_,_,_,_,_, _,_,_,9,7,_,2,5,_, 1,_,_,_,5,_,_,7,_].

% Spektrum der Wissenschaft 3/2006 Preisraetsel (angeblich hoellisch !)

puzzle(17,P) :-
   P = [_,1,_,_,6,5,4,_,_, _,_,_,_,8,4,1,_,_, 4,_,_,_,_,_,_,7,_,
        _,5,_,1,9,_,_,_,_, _,_,3,_,_,_,7,_,_, _,_,_,_,3,7,_,5,_,
        _,8,_,_,_,_,_,_,3, _,_,2,6,5,_,_,_,_, _,_,9,8,1,_,_,2,_].

% the (almost) empty grid

puzzle(99,P) :- 
   P = [1,2,3,4,5,6,7,8,9, _,_,_,_,_,_,_,_,_, _,_,_,_,_,_,_,_,_,
        _,_,_,_,_,_,_,_,_, _,_,_,_,_,_,_,_,_, _,_,_,_,_,_,_,_,_,
        _,_,_,_,_,_,_,_,_, _,_,_,_,_,_,_,_,_, _,_,_,_,_,_,_,_,_].

```


<br/>
<hr/>
<br/>
<h2 id="96"> 96. 7-Miscellaneous-Problems-P90/p97S.pl</h2>

``` prolog
%  P97 (**) Sudoku
%  
%  Sudoku puzzles go like this:  

%   Problem statement                Solution

%    .  .  4 | 8  .  . | .  1  7     9  3  4 | 8  2  5 | 6  1  7	     
%            |         |                     |         |
%    6  7  . | 9  .  . | .  .  .     6  7  2 | 9  1  4 | 8  5  3
%            |         |                     |         |
%    5  .  8 | .  3  . | .  .  4     5  1  8 | 6  3  7 | 9  2  4
%    --------+---------+--------     --------+---------+--------
%    3  .  . | 7  4  . | 1  .  .     3  2  5 | 7  4  8 | 1  6  9
%            |         |                     |         |
%    .  6  9 | .  .  . | 7  8  .     4  6  9 | 1  5  3 | 7  8  2
%            |         |                     |         |
%    .  .  1 | .  6  9 | .  .  5     7  8  1 | 2  6  9 | 4  3  5
%    --------+---------+--------     --------+---------+--------
%    1  .  . | .  8  . | 3  .  6     1  9  7 | 5  8  2 | 3  4  6
%            |         |                     |         |
%    .  .  . | .  .  6 | .  9  1     8  5  3 | 4  7  6 | 2  9  1
%            |         |                     |         |
%    2  4  . | .  .  1 | 5  .  .     2  4  6 | 3  9  1 | 5  7  8

% Every spot in the puzzle belongs to a (horizontal) row and a (vertical)
% column, as well as to one single 3x3 square (which we call "square" 
% for short). At the beginning, some of the spots carry a single-digit
% number between 1 and 9. The problem is to fill the missing spots with
% digits in such a way that every number between 1 and 9 appears exactly
% once in each row, in each column, and in each square.

% We represent the Sudoku puzzle as a simple list of 81 digits. At
% the beginning, the list is partially instantiated. During the 
% process, all the elememts of the list get instantiated with digits.

% We are going to treat each spot as a Prolog term spot(X,R,C,S) where
% X is the number to put into the field, R is the row, C the column, and
% S the square the field belongs to. R, C, and S are lists which represent
% the respective number sets.

% --------------------------------------------------------------------------

% sudoku(Puzzle) :- solve the given Sudoku puzzle and print the
%    problem statement as well as the solution to the standard output
%   (list-of-integers, partially instantiated)

sudoku(Puzzle) :- 
   printPuzzle(Puzzle), nl, 
   connect(Spots),
   flag(counter,_,0),
   init(Puzzle,Spots),
   solve(Spots),
   printPuzzle(Puzzle),
   flag(counter,N,N+1),
   fail.
sudoku(_) :- 
   flag(counter,N,N), nl, 
   printCounter(N).

% ---------------------------------------------------------------

% The most difficult part of the problem solution is to prepare 
% the list of spot/4 terms representing the spots in the puzzle.
% We have to make sure that every spot "knows" its row, column,
% and square. In other words, all the spots in a row access the
% same list in order to check whether a new number can be placed
% in the row. The same is true for the columns and the squares. 

% connect(Spots) :- Spots = [spot(_,R1,C1,S1),spot(_,R1,C2,S1),.....].

connect(Spots) :- 
   length(Spots,16),
   connectRows(Spots),
   connectCols(Spots),
   connectSquares(Spots).

% connectRows(Spots) :- connect the spots of all rows in the list Spot

connectRows([]).
connectRows(Spots) :- 
   connectRow(Spots,_,4),
   skip(Spots,4,Spots1), 
   connectRows(Spots1).

% connectRow(Spots,R,K) :- the first K elements of Spot
% are in the same row R

connectRow(_,_,0).
connectRow([spot(_,R,_,_)|Spots],R,K) :- K > 0,
   K1 is K-1, connectRow(Spots,R,K1).

% connectCols(Spots) :- connect the spots of the same column

connectCols(Spots) :- connectCols(Spots,4).

% connectCols(Spots,K) :- connect K more columns columns

connectCols(_,0) :- !.
connectCols(Spots,K) :- K > 0,
   connectCol(Spots,_),
   skip(Spots,1,Spots1), 
   K1 is K-1, connectCols(Spots1,K1).

% connectCol(Spots,C) :- connect the first spot in Spots with
% the other spots in its column C

connectCol([],_).
connectCol([spot(_,_,C,_)|Spots],C) :-
   skip(Spots,3,Spots1),
   connectCol(Spots1,C).

% connectSquares(Spots) :- connect all three bands
% The nine squares are arranged in three horizontal bands,
% three squares in each band. 
   
connectSquares(Spots) :- 
   connectBand(Spots),
   skip(Spots,3,Spots1),
   connectBand(Spots1),
   skip(Spots1,3,Spots2),
   connectBand(Spots2).

% connectBand(Spots) :- connect the next band (of three squares

connectBand(Spots) :- 
   connectSq(Spots,_),
   skip(Spots,3,Spots1),
   connectSq(Spots1,_),
   skip(Spots1,3,Spots2),
   connectSq(Spots2,_).

% connectSq(Spots,S) :- connect the spots of square S. In the Spots
%    list each square is composed of three spot-triplets which 
%    are separated by 6 spots. 

connectSq([],_).
connectSq(Spots,S) :- 
  connectTriplet(Spots,S),
  skip(Spots,4,Spots1),
  connectTriplet(Spots1,S),
  skip(Spots1,4,Spots2),
  connectTriplet(Spots2,S).

% connectTriplet(Spots,S) :- connect the next three spots in the Spots
%    list with the square S

connectTriplet([spot(_,_,_,S),spot(_,_,_,S),spot(_,_,_,S)|_],S).

% skip(List,N,List1) :- skip the first N elements from a List
%    and return the rest of the list in List1. If the List is
%    too short, then succeed and return [] as rest.

skip([],_,[]) :- !. 
skip(Xs,0,Xs) :- !.
skip([_|Xs],K,Zs) :- K > 0, K1 is K-1, skip(Xs,K1,Zs). 

% -----------------------------------------------------------

% init(Puzzle,Spots) :- initialize the Spots list on the
%    basis of the problem statement (Puzzle) and link the
%    Puzzle list to the Spots list 

init([],[]).
init([X|Xs],[Sp|Spots]) :- initSpot(X,Sp), init(Xs,Spots).

% If X is not instantiated in the given puzzle, create a link
% between the variable in the puzzle and the corresponding 
% variable in the spot. Otherwise copy the given number from 
% the puzzle into the spot and insert it into the spot's 
% correct row, column, and square, if this is legal.

initSpot(X,spot(X,_,_,_)) :- var(X), !.
initSpot(X,spot(X,R,C,S)) :- integer(X),
   insert(X,R),
   insert(X,C),
   insert(X,S).

% ----------------------------------------------------------

% solve(Spots) :- solve the problem using backtrack

solve([]).
solve([Spot|Spots]) :- bind(Spot), solve(Spots).

% bind(Spot) :- bind the data field in Spot to an 
% available non-conflicting digit.

bind(spot(X,_,_,_)) :- nonvar(X), !.
bind(spot(X,R,C,S)) :- var(X),
   between(1,9,X),  % try a digit
   insert(X,R),
   insert(X,C),
   insert(X,S).

% ---------------------------------------------------------

% insert(X,L) :- X can be inserted into the list without 
% conflict, i.e. X is not yet in the list, when insert/2
% is called. Otherwise the predicate fails.

insert(X,L) :- var(L), !, L = [X|_].
insert(X,[Y|Ys]) :- X \= Y, insert(X,Ys).

% ---------------------------------------------------------

printPuzzle([]).
printPuzzle(Xs) :- nl,
   printBand(Xs,Xs1),
   write('--------+---------+--------'), nl,
   printBand(Xs1,Xs2),
   write('--------+---------+--------'), nl,
   printBand(Xs2,_).

printBand(Xs,Xs3) :- 
   printRow(Xs,Xs1), nl,
   write('        |         |'), nl, 
   printRow(Xs1,Xs2), nl,
   write('        |         |'), nl, 
   printRow(Xs2,Xs3), nl.
 
printRow(Xs,Xs3) :-
   printTriplet(Xs,Xs1), write(' | '),
   printTriplet(Xs1,Xs2), write(' | '),
   printTriplet(Xs2,Xs3).

printTriplet(Xs,Xs3) :-
   printElement(Xs,Xs1), write('  '),
   printElement(Xs1,Xs2), write('  '),
   printElement(Xs2,Xs3).

printElement([X|Xs],Xs) :- var(X), !, write('.').
printElement([X|Xs],Xs) :- write(X).

printCounter(0) :- !, write('No solution'), nl.
printCounter(1) :- !, write('1 solution'), nl.
printCounter(K) :- write(K), write(' solutions'), nl.

% ---------------------------------------------------------

test(N) :- puzzle(N,P), sudoku(P).

puzzle(1,P) :- 
   P = [_,_,4,8,_,_,_,1,7, 6,7,_,9,_,_,_,_,_, 5,_,8,_,3,_,_,_,4,
        3,_,_,7,4,_,1,_,_, _,6,9,_,_,_,7,8,_, _,_,1,_,6,9,_,_,5,
	1,_,_,_,8,_,3,_,6, _,_,_,_,_,6,_,9,1, 2,4,_,_,_,1,5,_,_].

puzzle(2,P) :- 
   P = [3,_,_,_,7,1,_,_,_, _,5,_,_,_,_,1,8,_, _,4,_,8,_,_,_,_,_,
	_,_,6,2,_,_,3,_,_, _,_,1,_,5,_,8,_,_, _,_,3,_,_,8,2,_,_,
        _,_,_,_,_,3,_,4,_, _,6,4,_,_,_,_,7,_, _,_,_,9,6,_,_,_,1].

puzzle(3,P) :-
   P = [1,7,_,_,_,9,_,_,4, _,_,_,_,_,_,7,_,_, 5,_,_,3,_,_,2,_,_,
        _,8,_,_,_,_,5,3,6, _,_,_,_,8,_,_,_,_, 6,9,1,_,_,_,_,8,_,
        _,_,7,_,_,4,_,_,2, _,_,2,_,_,_,_,_,_, 3,_,_,5,_,_,_,7,1].

% an example with many solutions

puzzle(4,P) :-
   P = [1,_,_,_,_,9,_,_,4, _,_,_,_,_,_,7,_,_, 5,_,_,3,_,_,2,_,_,
        _,8,_,_,_,_,5,_,6, _,_,_,_,8,_,_,_,_, 6,9,1,_,_,_,_,8,_,
        _,_,7,_,_,4,_,_,2, _,_,2,_,_,_,_,_,_, 3,_,_,5,_,_,_,7,1].

puzzle(5,P) :- 
   P = [_,6,5,_,_,_,7,2,_, 3,_,7,_,_,_,1,_,8, 2,9,_,_,1,_,_,3,4,
        _,_,_,5,_,7,_,_,_, _,_,1,_,_,_,8,_,_, _,_,_,2,_,1,_,_,_,
        8,1,_,_,2,_,_,5,7, 7,_,2,_,_,_,9,_,1, _,5,4,_,_,_,6,8,_].

puzzle(6,P) :- 
   P = [5,_,2,_,_,3,_,_,_, 4,6,_,_,7,_,9,_,_, _,_,3,4,_,_,_,_,_,
        9,5,_,_,6,_,_,_,_, _,4,_,_,_,_,_,9,_, _,_,_,_,9,_,_,1,7,
        _,_,_,_,_,7,2,_,_, _,_,9,_,4,_,_,3,5, _,_,_,3,_,_,7,_,6].

% an example with an error in the problem statement (5 appears
% twice in the top left square)

puzzle(e1,P) :- 
   P = [5,_,2,_,_,3,_,_,_, 4,6,5,_,7,_,9,_,_, _,_,3,4,_,_,_,_,_,
        9,5,_,_,6,_,_,_,_, _,4,_,_,_,_,_,9,_, _,_,_,_,9,_,_,1,7,
        _,_,_,_,_,7,2,_,_, _,_,9,_,4,_,_,3,5, _,_,_,3,_,_,7,_,6].

% another example with an error in the problem statement (garbage
% in the first row

puzzle(e2,P) :- 
   P = [x,_,2,_,_,3,_,_,_, 4,6,_,_,7,_,9,_,_, _,_,3,4,_,_,_,_,_,
        9,5,_,_,6,_,_,_,_, _,4,_,_,_,_,_,9,_, _,_,_,_,9,_,_,1,7,
        _,_,_,_,_,7,2,_,_, _,_,9,_,4,_,_,3,5, _,_,_,3,_,_,7,_,6].

% some more examples from the Sonntagszeitung

puzzle(8,P) :- 
   P = [4,8,_,_,7,_,_,_,_, _,_,9,6,8,_,3,_,7, 3,_,7,4,_,_,_,5,_,
        _,_,_,3,_,_,_,2,_, 9,5,_,7,2,1,_,6,8, _,1,_,_,_,4,_,_,_,
        _,4,_,_,_,2,7,_,1, 8,_,2,_,4,7,5,_,_, _,_,_,_,5,_,_,8,4].

puzzle(9,P) :- 
   P = [_,1,_,_,_,_,_,2,4, 5,_,_,_,4,_,_,8,6, 6,_,4,1,_,_,_,_,_,
        _,_,_,8,_,6,9,_,_, 8,_,_,_,_,_,_,_,2, _,_,6,4,_,3,_,_,_,
        _,_,_,_,_,7,2,_,8, 1,6,_,_,9,_,_,_,5, 7,4,_,_,_,_,_,9,_].

puzzle(10,P) :-
   P = [_,9,7,_,_,5,_,_,4, _,_,_,_,_,9,_,_,_, _,_,5,_,4,_,2,_,7,
        _,8,6,_,_,3,_,_,_, _,_,_,_,2,_,_,_,_, _,_,_,5,_,_,3,4,_,
        5,_,3,_,7,_,6,_,_, _,_,_,6,_,_,_,_,_, 9,_,_,8,_,_,1,7,_].

% a puzzle rated "not fun" by 
% http://dingo.sbs.arizona.edu/~sandiway/sudoku/examples.html 

puzzle(11,P) :-
   P = [_,2,_,_,_,_,_,_,_, _,_,_,6,_,_,_,_,3, _,7,4,_,8,_,_,_,_,
	_,_,_,_,_,3,_,_,2, _,8,_,_,4,_,_,1,_, 6,_,_,5,_,_,_,_,_,
        _,_,_,_,1,_,7,8,_, 5,_,_,_,_,9,_,_,_, _,_,_,_,_,_,_,4,_].

% a "super hard puzzle" by
% http://www.menneske.no/sudoku/eng/showpuzzle.html?number=2155141

puzzle(12,P) :-
   P = [_,_,_,6,_,_,4,_,_, 7,_,_,_,_,3,6,_,_, _,_,_,_,9,1,_,8,_,
        _,_,_,_,_,_,_,_,_, _,5,_,1,8,_,_,_,3, _,_,_,3,_,6,_,4,5,
        _,4,_,2,_,_,_,6,_, 9,_,3,_,_,_,_,_,_, _,2,_,_,_,_,1,_,_].

% some puzzles from Spektrum der Wissenschaft 3/2006, p.100

% leicht
puzzle(13,P) :-
   P = [_,2,6,4,5,8,3,_,_, 1,7,_,_,_,_,_,4,_, _,8,_,_,_,_,_,_,_,
	_,_,_,_,_,_,9,8,_, _,_,_,5,9,_,1,_,4, 7,_,_,2,_,1,_,5,_,
	_,_,_,_,4,_,_,3,_, _,_,_,8,_,_,5,_,_, 6,_,_,_,_,7,_,9,1].

% mittel
puzzle(14,P) :-
   P = [9,_,_,6,3,_,_,_,4, _,1,_,2,5,8,_,_,_, _,_,_,7,_,_,_,_,8,
        6,4,_,_,2,_,5,_,_, _,_,_,_,_,_,_,_,_, 8,2,_,5,_,_,_,9,_,
	_,_,_,_,_,_,8,7,_, 3,_,_,_,_,5,_,4,_, _,_,1,_,7,6,_,_,_].

% schwer
puzzle(15,P) :-
   P = [_,_,_,_,_,_,_,_,7, _,_,_,_,_,_,6,3,4, _,_,_,9,4,_,_,2,_,
        5,_,1,7,_,_,8,6,_, _,_,9,_,_,_,_,_,3, _,_,_,_,8,_,_,_,_,
	4,3,_,5,_,_,_,_,_, _,1,_,_,6,8,_,_,_, _,_,_,_,_,3,1,_,9].

% hoellisch (!)
puzzle(16,P) :-
   P = [_,_,_,_,3,_,_,_,_, _,1,5,_,_,_,6,_,_, 6,_,_,2,_,_,3,4,_,
        _,_,_,6,_,_,_,8,_, _,3,9,_,_,_,5,_,_, 5,_,_,_,_,_,9,_,2,
	_,_,_,_,_,_,_,_,_, _,_,_,9,7,_,2,5,_, 1,_,_,_,5,_,_,7,_].

% Spektrum der Wissenschaft 3/2006 Preisraetsel (angeblich hoellisch !)

puzzle(17,P) :-
   P = [_,1,_,_,6,5,4,_,_, _,_,_,_,8,4,1,_,_, 4,_,_,_,_,_,_,7,_,
        _,5,_,1,9,_,_,_,_, _,_,3,_,_,_,7,_,_, _,_,_,_,3,7,_,5,_,
        _,8,_,_,_,_,_,_,3, _,_,2,6,5,_,_,_,_, _,_,9,8,1,_,_,2,_].

% the (almost) empty grid

puzzle(99,P) :- 
   P = [1,2,3,4,5,6,7,8,9, _,_,_,_,_,_,_,_,_, _,_,_,_,_,_,_,_,_,
        _,_,_,_,_,_,_,_,_, _,_,_,_,_,_,_,_,_, _,_,_,_,_,_,_,_,_,
        _,_,_,_,_,_,_,_,_, _,_,_,_,_,_,_,_,_, _,_,_,_,_,_,_,_,_].

```


<br/>
<hr/>
<br/>
<h2 id="97"> 97. 7-Miscellaneous-Problems-P90/p98.pl</h2>

``` prolog
%  P98 (***) Nonograms

%   Around 1994, a certain kind of puzzles was very popular in England.
%   The "Sunday Telegraph" newspaper wrote: "Nonograms are puzzles from 
%   Japan and are currently published each week only in The Sunday 
%   Telegraph.  Simply use your logic and skill to complete the grid 
%   and reveal a picture or diagram." As a Prolog programmer, you are in 
%   a better situation: you can have your computer do the work! Just write
%   a little program ;-).
%   The puzzle goes like this: Essentially, each row and column of a 
%   rectangular bitmap is annotated with the respective lengths of 
%   its distinct strings of occupied cells. The person who solves the puzzle 
%   must complete the bitmap given only these lengths.

%          Problem statement:          Solution:

%          |_|_|_|_|_|_|_|_| 3         |_|X|X|X|_|_|_|_| 3           
%          |_|_|_|_|_|_|_|_| 2 1       |X|X|_|X|_|_|_|_| 2 1         
%          |_|_|_|_|_|_|_|_| 3 2       |_|X|X|X|_|_|X|X| 3 2         
%          |_|_|_|_|_|_|_|_| 2 2       |_|_|X|X|_|_|X|X| 2 2         
%          |_|_|_|_|_|_|_|_| 6         |_|_|X|X|X|X|X|X| 6           
%          |_|_|_|_|_|_|_|_| 1 5       |X|_|X|X|X|X|X|_| 1 5         
%          |_|_|_|_|_|_|_|_| 6         |X|X|X|X|X|X|_|_| 6           
%          |_|_|_|_|_|_|_|_| 1         |_|_|_|_|X|_|_|_| 1           
%          |_|_|_|_|_|_|_|_| 2         |_|_|_|X|X|_|_|_| 2           
%           1 3 1 7 5 3 4 3             1 3 1 7 5 3 4 3              
%           2 1 5 1                     2 1 5 1                      

%   For the example above, the problem can be stated as the two lists
%   [[3],[2,1],[3,2],[2,2],[6],[1,5],[6],[1],[2]] and 
%   [[1,2],[3,1],[1,5],[7,1],[5],[3],[4],[3]] which give the
%   "solid" lengths of the rows and columns, top-to-bottom and
%   left-to-right, respectively. (Published puzzles are larger than this
%   example, e.g. 25 x 20, and apparently always have unique solutions.)

% Basic ideas  -------------------------------------------------------------

% (1) Every square belongs to a (horizontal) row and a (vertical) column.
%     We are going to treat each square as a variable that can be accessed
%     via its row or via its column. The objective is to instantiate each
%     square with either an 'x' or a space character.

% (2) Rows and columns should be processed in a similar way. We are going
%     to collectively call them "lines", and we call the strings of
%     successive 'x's "runs". For every given line, there are, in 
%     general, several possibilities to put 'x's into the squares. 
%     For example, if we have to put a run of length 3 into a line 
%     of length 8 then there are 6 ways to do so.

% (3) In principle, all these possibilities have to be explored for all
%     lines. However, because we are only interested in a single solution,
%     not in all of them, it may be advantageous to first try the lines 
%     with few possibilities.

% --------------------------------------------------------------------------

% nonogram(RowNums,ColNums,Solution,Opt) :- given the specifications for
%    the rows and columns in RowNums and ColNums, respectively, the puzzle
%    is solved by Solution, which is a row-by-row representation of
%    the filled puzzle grid. Opt = 0 is without optimization, Opt = 1
%    optimizes the order of the line tasks (see below). 
%    (list-of-int-lists,list-of-int-lists,list-char-lists)    (+,+,-)

nonogram(RowNums,ColNums,Solution,Opt) :-
	length(RowNums,NRows),
	length(ColNums,NCols),
	make_rectangle(NRows,NCols,Rows,Cols),
	append(Rows,Cols,Lines),
	append(RowNums,ColNums,LineNums),
	maplist(make_runs,LineNums,LineRuns),
	combine(Lines,LineRuns,LineTasks),
	optimize(Opt,LineTasks,OptimizedLineTasks),
	solve(OptimizedLineTasks),
	Solution = Rows.
 
combine([],[],[]).
combine([L1|Ls],[N1|Ns],[task(L1,N1)|Ts]) :- combine(Ls,Ns,Ts).

solve([]).
solve([task(Line,LineRuns)|Tasks]) :- 
   place_runs(LineRuns,Line),
   solve(Tasks).


% (1) The first basic idea is implemented as follows. ----------------------

% make_rectangle(NRows,NCols,Rows,Cols) :- a rectangular array of variables
%    with NRows rows and NCols columns is generated. The variables can
%    be accessed via the Rows or via the Cols list. I.e the variable in 
%    row 1 and column 2 can be addressed in the Rows list as [[_,X|_]|_]
%    or in the Cols list as [_,[X|_]|_]. Cool!
%    (integer,integer,list-of-char-list,list-of-char-list)    (+,+,_,_)

make_rectangle(NRows,NCols,Rows,Cols) :-
	NRows > 0, NCols > 0,
	length(Rows,NRows),
%	Pred1 =.. [inv_length, NCols],
	Pred1 = inv_length(NCols),
	checklist(Pred1,Rows),
	length(Cols,NCols),
	Pred2 =.. [inv_length, NRows],
	checklist(Pred2,Cols),
	write(Rows),nl,
	write(Cols),nl,
	unify_rectangle(Rows,Cols).

help(make_rectangle/4):-
	write('creates a rectangle which can be accessed both column and row wise..'),nl.


inv_length(Len,List) :- length(List,Len).

% unify_rectangle([[]|_],[]).
unify_rectangle(_,[]).
unify_rectangle([],_).
unify_rectangle([[X|Row1]|Rows],[[X|Col1]|Cols]) :-
   unify_row(Row1,Cols,ColsR), 
   unify_rectangle(Rows,[Col1|ColsR]).   

unify_row([],[],[]).
unify_row([X|Row],[[X|Col1]|Cols],[Col1|ColsR]) :- unify_row(Row,Cols,ColsR).


% (2) The second basic idea is implemented as follows -----------------------

% make_runs(RunLens,Runs) :- Runs is a list of character-lists that
%    correspond to the given run lengths RunLens. Actually, each run
%    is a difference list; e.g ['x','x'|T]-T.
%    (integer-list,list-of-runs) (+,-)

make_runs([],[]) :- !.
make_runs([Len1|Lens],[Run1-T|Runs]) :- 
   put_x(Len1,Run1,T),
   make_runs2(Lens,Runs).

% make_runs2(RunLens,Runs) :- same as make_runs, except that the runs
%    start with a space character.
make_runs2([],[]).
make_runs2([Len1|Lens],[[' '|Run1]-T|Runs]) :- 
   put_x(Len1,Run1,T),
   make_runs2(Lens,Runs).

put_x(0,T,T) :- !.
put_x(N,['x'|Xs],T) :- N > 0, N1 is N-1, put_x(N1,Xs,T).

% place_runs(Runs,Line) :- Runs is a list of runs, each of them being
%    a difference list of characters. Line is a list of characters.
%    The runs are placed into the Line, optionally separated by
%    additional space characters. Via backtracking, all possibilities
%    are generated.
%   (run-list,square-list)  (+,?)

place_runs([],[]).
place_runs([Line-Rest|Runs],Line) :- place_runs(Runs,Rest).
place_runs(Runs,[' '|Rest]) :- place_runs(Runs,Rest).
 
% In order to understand what the predicates make_runs/2 make_runs2/2
% put_x/3, and place_runs/2, try the following goal:

% ?-  make_runs([3,1],Runs), Line = [_,_,_,_,_,_,_], place_runs(Runs,Line).

% (3) The third idea is an optimization. It is performed by ordering
%     the line tasks in an advantageous way. This is done by the 
%     predicate optimize.

% optimize(LineTasks,LineTasksOpt)

optimize(0,LineTasks,LineTasks).     

optimize(1,LineTasks,OptimizedLineTasks) :- 
   label(LineTasks,LabelledLineTasks),
   sort(LabelledLineTasks,SortedLineTasks),
	unlabel(SortedLineTasks,OptimizedLineTasks).
   
label([],[]).
label([task(Line,LineRuns)|Tasks],[task(Count,Line,LineRuns)|LTasks]) :- 
   length(Line,N),   
   findall(L,(length(L,N), place_runs(LineRuns,L)),Ls),
   length(Ls,Count),
   label(Tasks,LTasks).

unlabel([],[]).
unlabel([task(_,Line,LineRuns)|LTasks],[task(Line,LineRuns)|Tasks]) :- 
   unlabel(LTasks,Tasks).

% Printing the solution ----------------------------------------------------

% print_nonogram(RowNums,ColNums,Solution) :-

print_nonogram([],ColNums,[]) :- print_colnums(ColNums).
print_nonogram([RowNums1|RowNums],ColNums,[Row1|Rows]) :-
   print_row(Row1),
   print_rownums(RowNums1),
   print_nonogram(RowNums,ColNums,Rows).

print_row([]) :- write('  ').
print_row([X|Xs]) :- print_replace(X,Y), write(' '), write(Y), print_row(Xs).
   
print_replace(' ',' ') :- !.
print_replace(x,'*').

print_rownums([]) :- nl.
print_rownums([N|Ns]) :- write(N), write(' '), print_rownums(Ns).

print_colnums(ColNums) :-
   maxlength(ColNums,M,0),
	print_colnums(ColNums,ColNums,1,M).

maxlength([],M,M).
maxlength([L|Ls],M,A) :- length(L,N), B is max(A,N), maxlength(Ls,M,B). 

print_colnums(_,[],M,M) :- !, nl.
print_colnums(ColNums,[],K,M) :- K < M, !, nl,
   K1 is K+1, print_colnums(ColNums,ColNums,K1,M).
print_colnums(ColNums,[Col1|Cols],K,M) :- K =< M, 
   write_kth(K,Col1), print_colnums(ColNums,Cols,K,M).
   
write_kth(K,List) :- nth1(K,List,X), !, writef('%2r',[X]).
write_kth(_,_) :- write('  ').

% --------------------------------------------------------------------------

% Test with some "real" puzzles from the Sunday Telegraph:

test(Name,Opt) :- 
   specimen_nonogram(Name,Rs,Cs),
   nonogram(Rs,Cs,Solution,Opt), nl,
   print_nonogram(Rs,Cs,Solution).

% Results for the nonogram 'Hen':

% ?- time(test('Hen',0)).     - without optimization
% 16,803,498 inferences in 39.30 seconds (427570 Lips)  

% ?- time(test('Hen',1)).     - with optimization
% 5,428 inferences in 0.02 seconds (271400 Lips)

% specimen_nonogram( Title, Rows, Cols) :-
%	NB  Rows, Cols and the "solid" lengths are enlisted
%	top-to-bottom or left-to-right as appropriate

specimen_nonogram(
	'Hen',
	[[3], [2,1], [3,2], [2,2], [6], [1,5], [6], [1], [2]],
	[[1,2], [3,1], [1,5], [7,1], [5], [3], [4], [3]]
	).

specimen_nonogram(
	'Jack & The Beanstalk',
	[[3,1], [2,4,1], [1,3,3], [2,4], [3,3,1,3], [3,2,2,1,3],
	 [2,2,2,2,2], [2,1,1,2,1,1], [1,2,1,4], [1,1,2,2], [2,2,8],
	 [2,2,2,4], [1,2,2,1,1,1], [3,3,5,1], [1,1,3,1,1,2],
	 [2,3,1,3,3], [1,3,2,8], [4,3,8], [1,4,2,5], [1,4,2,2],
	 [4,2,5], [5,3,5], [4,1,1], [4,2], [3,3]],
	[[2,3], [3,1,3], [3,2,1,2], [2,4,4], [3,4,2,4,5], [2,5,2,4,6],
	 [1,4,3,4,6,1], [4,3,3,6,2], [4,2,3,6,3], [1,2,4,2,1], [2,2,6],
	 [1,1,6], [2,1,4,2], [4,2,6], [1,1,1,1,4], [2,4,7], [3,5,6],
	 [3,2,4,2], [2,2,2], [6,3]]
	).

specimen_nonogram(
	'WATER BUFFALO',
	[[5], [2,3,2], [2,5,1], [2,8], [2,5,11], [1,1,2,1,6], [1,2,1,3],
	 [2,1,1], [2,6,2], [15,4], [10,8], [2,1,4,3,6], [17], [17],
	 [18], [1,14], [1,1,14], [5,9], [8], [7]],
	[[5], [3,2], [2,1,2], [1,1,1], [1,1,1], [1,3], [2,2], [1,3,3],
	 [1,3,3,1], [1,7,2], [1,9,1], [1,10], [1,10], [1,3,5], [1,8],
	 [2,1,6], [3,1,7], [4,1,7], [6,1,8], [6,10], [7,10], [1,4,11],
	 [1,2,11], [2,12], [3,13]]
	).

% Thanks to ------------------------------------------------------------
%  __   __    Paul Singleton (Dr)           JANET: paul@uk.ac.keele.cs
% |__) (__    Computer Science Dept.        other: paul@cs.keele.ac.uk
% |  .  __).  Keele University, Newcastle,    tel: +44 (0)782 583477
%             Staffs ST5 5BG, ENGLAND         fax: +44 (0)782 713082
% for the idea and the examples ----------------------------------------

```


<br/>
<hr/>
<br/>
<h2 id="98"> 98. 7-Miscellaneous-Problems-P90/p99-readfile.pl</h2>

``` prolog
% readfile.pl   werner.hett@hta-bi.bfh.ch  
% Time-stamp: <8-Oct-2000 15:25 hew>

% Auxiliary predicate for reading a text file and splitting the text 
% into lines. Cope with the different end-of-line conventions.
% Should work with UNIX, DOS/Windows, and Mac file system.


% read_lines(File,Lines) :- read the text file File and split the text
% into lines. Lines is a list of char-lists, each of them being a text line.
% (+,-) (atom, list-of-charlists)  

read_lines(File,Lines) :-
   seeing(Old), see(File), 
   get_char(X), read_file(X,CharList),  % read the whole file into a charlist
   parse_charlist(CharList-[],Lines),   % parse lines using difference lists
   see(Old).

read_file(end_of_file,[]) :- !.
read_file(X,[X|Xs]) :- get_char(Y), read_file(Y,Xs).

parse_charlist(T-T,[]) :- !.
parse_charlist(X1-X4,[L|Ls]) :- 
   parse_line(X1-X2,L), 
   parse_eol(X2-X3), !,
   parse_charlist(X3-X4,Ls).

parse_eol([]-[]) :- !.           % no end-of-line at end-of-file
parse_eol(['\r','\n'|R]-R) :- !. % DOS/Windows
parse_eol(['\n','\r'|R]-R) :- !. % Mac (?)
parse_eol(['\r'|R]-R) :- !.      % Mac (?)
parse_eol(['\n'|R]-R).           % UNIX

parse_line([]-[],[]) :- !.       % no end-of-line at end-of-file
parse_line([X|X1]-[X|X1],[]) :- eol_char(X), !.
parse_line([X|X1]-X2,[X|Xs]) :- \+ eol_char(X), parse_line(X1-X2,Xs).

eol_char('\r').
eol_char('\n').

```


<br/>
<hr/>
<br/>
<h2 id="99"> 99. 7-Miscellaneous-Problems-P90/p99.pl</h2>

``` prolog
% Crossword puzzle
%
% Given an empty (or almost empty) framework of a crossword puzzle and 
% a set of words. The problem is to place the words into the framework.
%
% werner.hett@hta-bi.bfh.ch     Time-stamp: <8-Oct-2000 14:46 hew>
% modified argument order in select/3 predicate (SWI 3.3 -> 3.4) 
% 15-May-2001 hew
%
% The particular crossword puzzle is specified in a text file which
% first lists the words (one word per line) in an arbitrary order. Then,
% after an empty line, the crossword framework is defined. In this 
% framework specification, an empty character location is represented
% by a dot (.). In order to make the solution easier, character locations 
% can also contain predefined character values. (See example files p99*.dat;
% note that p99c.dat does not have a solution).
%
% Words are strings (character lists) of at least two characters. 
% A horizontal or vertical sequence of character places in the 
% crossword framework is called a site. Our problem is to find a 
% compatible way of placing words onto sites.

:- ensure_loaded('p99-readfile.pl').  % used to read the data file

% main program section -----------------------------------------------------

crossword :-
	write('usage: crossword(File)'), nl,
   write('or     crossword(File,Opt)         with Opt one of 0,1, or 2'), nl,
   write('or     crossword(File,Opt,debug)   for extra output'), nl.

:- crossword.

% crossword/1 runs without optimization (not recommended for large files)
crossword(FileName) :- crossword(FileName,0).

% crossword/2 runs with a given optimization and no debug output
crossword(FileName,Opt) :- crossword(FileName,Opt,nodebug).

% crossword/3 runs with a given optimization and a given debugging modus
crossword(FileName,Opt,Debug) :-
   read_lines(FileName,Lines),  % from file p99-readfile.pl
                                % read_lines returns a list of character-lists
   separate(Lines,Words,FrameLines),
   length(Words,NWords), 
   construct_squares(FrameLines,Squares,MaxRow,MaxCol),
   debug_write(Debug,Squares),
   construct_sites(Squares,MaxRow,MaxCol,Sites),
	length(Sites,NSites),
   check_lengths(NWords,NSites), 
   solve(Words,Sites,Opt,Debug), % do the real work
   show_result(Squares,MaxRow,MaxCol).

debug_write(debug,X) :- !, write(X), nl, nl.
debug_write(_,_).

check_lengths(N,N) :- !.
check_lengths(NW,NS) :- NW \= NS, 
	write('Number of words does not correspond to number of sites.'), nl,
   fail.

% input preparation ----------------------------------------------------

% parse the data file and separate the word list from the framework 
% description
separate(Lines,Words,FrameLines) :-
   trim_lines(Lines,LinesT),
   parse_non_empty_lines(LinesT-L1,Words),  % difference lists!
   parse_empty_lines(L1-L2),
	parse_non_empty_lines(L2-L3,FrameLines),
   parse_empty_lines(L3-[]).

% remove white space at the end of the lines
trim_lines([],[]).
trim_lines([L|Ls],[LT|LTs]) :- trim_line(L,LT), trim_lines(Ls,LTs).

trim_line(L,LT) :- reverse(L,RL), rm_white_space(RL,RLT), reverse(RLT,LT).

rm_white_space([X|Xs],L) :- char_type(X,white), !, rm_white_space(Xs,L).
rm_white_space(L,L).      

% separate the word lines from the frame lines
parse_non_empty_lines([L|L1]-L2,[L|Ls]) :- L \= [], !, 
   parse_non_empty_lines(L1-L2,Ls).
parse_non_empty_lines(L-L,[]).

parse_empty_lines([[]|L1]-L2) :- !, parse_empty_lines(L1-L2).
parse_empty_lines(L-L).

% A square is a position for a single character. As Prolog term a square
% has the form sq(Row,Col,X), where X denotes the character and Row and
% Col define the position within the puzzle frame. Squares is simply
% the list of all sq/3 terms.

construct_squares(FrameLines,Squares,MaxRow,MaxCol) :-   % (+,-,+,+)
   construct_squares(FrameLines,SquaresList,1),
   flatten(SquaresList,Squares),
   maxima(Squares,0,0,MaxRow,MaxCol).

construct_squares([],[],_).                              % (+,-,+)
construct_squares([FL|FLs],[SL|SLs],Row) :- 
   construct_squares_row(FL,SL,Row,1),
   Row1 is Row+1,
   construct_squares(FLs,SLs,Row1).

construct_squares_row([],[],_,_).                        % (+,-,+,+)
construct_squares_row(['.'|Ps],[sq(Row,Col,_)|Sqs],Row,Col) :- !, 
   Col1 is Col+1, construct_squares_row(Ps,Sqs,Row,Col1).
construct_squares_row([X|Ps],[sq(Row,Col,X)|Sqs],Row,Col) :- 
   char_type(X,alpha), !, 
   Col1 is Col+1, construct_squares_row(Ps,Sqs,Row,Col1).
construct_squares_row([_|Ps],Sqs,Row,Col) :-  
   Col1 is Col+1, construct_squares_row(Ps,Sqs,Row,Col1).

% maxima(Squares,0,0,MaxRow,MaxCol) :- determine maximum dimensions

maxima([],MaxRow,MaxCol,MaxRow,MaxCol).
maxima([sq(Row,Col,_)|Sqs],AccRow,AccCol,MaxRow,MaxCol) :-
   AccRow1 is max(AccRow,Row),
   AccCol1 is max(AccCol,Col),
   maxima(Sqs,AccRow1,AccCol1,MaxRow,MaxCol).

% construction of sites -----------------------------------------------

% construct_sites/4 traverses the framework twice in order to
% collect all the sites in the list Sites

construct_sites(Squares,MaxRow,MaxCol,Sites) :-             % (+,+,+,-)
	construct_sites_h(Squares,MaxRow,MaxCol,1,SitesH,[]),    % horizontal
	construct_sites_v(Squares,MaxRow,MaxCol,1,Sites,SitesH). % vertical

% horizontal sites

construct_sites_h(_,MaxRow,_,Row,Sites,Sites) :- Row > MaxRow, !.
construct_sites_h(Squares,MaxRow,MaxCol,Row,Sites,AccSites) :-
   construct_sites_h(Squares,MaxRow,MaxCol,Row,1,AccSites1,AccSites),
   Row1 is Row+1,
	construct_sites_h(Squares,MaxRow,MaxCol,Row1,Sites,AccSites1).

construct_sites_h(_,_,MaxCol,_,Col,Sites,Sites) :- Col > MaxCol, !.
construct_sites_h(Squares,MaxRow,MaxCol,Row,Col,Sites,AccSites) :-
   construct_site_h(Squares,Row,Col,Site,Incr), !,
   Col1 is Col+Incr, 
   AccSites1 = [Site|AccSites],
   construct_sites_h(Squares,MaxRow,MaxCol,Row,Col1,Sites,AccSites1).
construct_sites_h(Squares,MaxRow,MaxCol,Row,Col,Sites,AccSites) :-
   Col1 is Col+1,
   construct_sites_h(Squares,MaxRow,MaxCol,Row,Col1,Sites,AccSites).

construct_site_h(Squares,Row,Col,[X,Y|Cs],Incr) :-
   memberchk(sq(Row,Col,X),Squares),
   Col1 is Col+1,
   memberchk(sq(Row,Col1,Y),Squares),
   Col2 is Col1+1,
   continue_site_h(Squares,Row,Col2,Cs,3,Incr).

continue_site_h(Squares,Row,Col,[X|Cs],Acc,Incr) :-
   memberchk(sq(Row,Col,X),Squares), !,
   Acc1 is Acc+1,
   Col1 is Col+1,
   continue_site_h(Squares,Row,Col1,Cs,Acc1,Incr).
continue_site_h(_,_,_,[],Incr,Incr).

% vertical sites
   
construct_sites_v(_,_,MaxCol,Col,Sites,Sites) :- Col > MaxCol, !.
construct_sites_v(Squares,MaxRow,MaxCol,Col,Sites,AccSites) :-
   construct_sites_v(Squares,MaxRow,MaxCol,1,Col,AccSites1,AccSites),
   Col1 is Col+1,
	construct_sites_v(Squares,MaxRow,MaxCol,Col1,Sites,AccSites1).

construct_sites_v(_,MaxRow,_,Row,_,Sites,Sites) :- Row > MaxRow, !.
construct_sites_v(Squares,MaxRow,MaxCol,Row,Col,Sites,AccSites) :-
   construct_site_v(Squares,Row,Col,Site,Incr), !,
   Row1 is Row+Incr,
   AccSites1 = [Site|AccSites],
   construct_sites_v(Squares,MaxRow,MaxCol,Row1,Col,Sites,AccSites1).
construct_sites_v(Squares,MaxRow,MaxCol,Row,Col,Sites,AccSites) :-
   Row1 is Row+1,
   construct_sites_v(Squares,MaxRow,MaxCol,Row1,Col,Sites,AccSites).

construct_site_v(Squares,Row,Col,[X,Y|Cs],Incr) :-
   memberchk(sq(Row,Col,X),Squares),
   Row1 is Row+1,
   memberchk(sq(Row1,Col,Y),Squares),
   Row2 is Row1+1,
   continue_site_v(Squares,Row2,Col,Cs,3,Incr).

continue_site_v(Squares,Row,Col,[X|Cs],Acc,Incr) :-
   memberchk(sq(Row,Col,X),Squares), !,
   Acc1 is Acc+1,
   Row1 is Row+1,
   continue_site_v(Squares,Row1,Col,Cs,Acc1,Incr).
continue_site_v(_,_,_,[],Incr,Incr).

% --------------------------------------------------------------------------

:- ensure_loaded('p28.pl').  % lsort and lfsort

% solve/4 does the optimization of the word and site lists

solve(Words,Sites,0,Debug) :- !,   % unsorted 
	solve(Words,Sites,Debug).
solve(Words,Sites,1,Debug) :- !,   % length sorted
	lsort(Words,Words1,desc),
   lsort(Sites,Sites1,desc),
	solve(Words1,Sites1,Debug).
solve(Words,Sites,2,Debug) :-      % length frequency sorted 
	lfsort(Words,Words1),
   lfsort(Sites,Sites1),
	solve(Words1,Sites1,Debug).

% solve/3 does the debug_write of the prepared Words and Sites
% and then calls solve/2 to do the real work

solve(Words,Sites,Debug) :-  
   debug_write(Debug,Words), 
   debug_write(Debug,Sites),
   solve(Words,Sites).        

% solve/2 does the real work: find the right site for every word

solve([],[]).
solve([W|Words],Sites) :- 
	select(W,Sites,SitesR),
	solve(Words,SitesR).

% --------------------------------------------------------------------------

show_result(Squares,MaxRow,MaxCol) :-
   show_result(Squares,MaxRow,MaxCol,1), nl.

show_result(_,MaxRow,_,Row) :- Row > MaxRow, !.
show_result(Squares,MaxRow,MaxCol,Row) :- 
   show_result(Squares,MaxRow,MaxCol,Row,1), nl,
   Row1 is Row+1, show_result(Squares,MaxRow,MaxCol,Row1).

show_result(_,_,MaxCol,_,Col) :- Col > MaxCol, !. 
show_result(Squares,MaxRow,MaxCol,Row,Col) :- 
   (memberchk(sq(Row,Col,X),Squares), !, write(X); write(' ')),
   Col1 is Col+1, show_result(Squares,MaxRow,MaxCol,Row,Col1).  

% -------------------------------------------------------------------------

% Benchmark results <8-Oct-2000 14:45 hew>

% On a 330 MHz Pentium II the following results have been measured
% with SWI-Prolog version 3.3.10 for i686-linux under SuSE Linux 6.3

% ?- time(crossword('p99b.dat',0)).
% 439,743,691 inferences in 1975.34 seconds (222617 Lips)

% ?- time(crossword('p99b.dat',1)).
% 19,644,100 inferences in 76.37 seconds (257223 Lips) 

% ?- time(crossword('p99b.dat',2)).
% 152,880 inferences in 0.94 seconds (162638 Lips)   

```


<br/>
<hr/>
<br/>
<h2 id="100"> 100. arith.pl/</h2>

``` prolog
init:-
	add_db(paren,['(',')']),
	add_db(operator,['+','-','*','/','=']),
	add_db(operator_ne,['+','-','*','/']).

add_db(_,[]).
add_db(X,[V|Vs]):-
	recorda(X,V),
	add_db(X,Vs).

combination_without_paranthesis([N],[N]):-!.
combination_without_paranthesis([N|Ns],[N,Op|Cs]):-
	combination_without_paranthesis(Ns,Cs),
	recorded(operator,Op),
	\+ (memberchk('=',Cs),Op='=').

add_parenthesis(FVC_NP,FVC_NP).

accept(L):-
	append([First,['='],Second],L),!,
	evaluate(First,LVal),
	evaluate(Second,RVal),
	LVal=RVal.

evaluate(L,S):-
	append([First,['+'],Second],L),!,
	evaluate(First,S1),
	evaluate(Second,S2),
	S is S1+S2.

evaluate(L,S):-
	append([First,['-'],Second],L),!,
	evaluate(First,S1),
	evaluate(Second,S2),
	S is S1-S2.

evaluate(L,S):-
	append([First,['*'],Second],L),!,
	evaluate(First,S1),
	evaluate(Second,S2),
	S is S1*S2.

evaluate(L,S):-
	append([First,['/'],Second],L),!,
	evaluate(First,S1),
	evaluate(Second,S2),
	\+ S2 = 0,
	S is S1/S2.

evaluate(L,S):-
	append(['(',L1,')'],L),!,
	evaluate(L1,S).


evaluate([L],L).
		     


		 
find_valid_combination(L,FVC):-
	combination_without_paranthesis(L,FVC_NP),
	memberchk('=',FVC_NP),
	add_parenthesis(FVC_NP,FVC).

solve(L,FVC):-
	find_valid_combination(L,FVC),
	accept(FVC).

data_type(paranthesis).
data_type(operator).

clear_db(X):-
	recorded(X,_,Ref),
	erase(Ref),
	fail.
clear_db(_).


do(L):-
	clear_db(paren),
	clear_db(operator),
	clear_db(operator_ne),
	init,
	solve(L,S),
	write(S),nl,
	fail.

test(1):-
	do([2,3,5,7,11]).

test(2):-
	do([1,2,3]).

teval(1):-
	accept([1,'+',2,'=',4]).

teval1(2,L):-
	evaluate([1,'+',2],L).


```


<br/>
<hr/>
<br/>
<h2 id="101"> 101. array.pl/</h2>

``` prolog
array_atom(A,[X]):-
	format(atom(A),"~w",X).
array_atom(A,[X|Xs]):-
	array_atom(AXs,Xs),
	format(atom(A),"~w_~w",[X,AXs]).
array([],Name):-
	
	
array([NI|NIs],Name):-
	between(1,NI,I),
	array_atom(IthElement,[Name,I]),
	array(NIs,IthElement).

do(1):-
	array([1,2,3,4],s),write
	
```


<br/>
<hr/>
<br/>
<h2 id="102"> 102. assert.pl/</h2>

``` prolog

```


<br/>
<hr/>
<br/>
<h2 id="103"> 103. binary_search_tree.pl/</h2>

``` prolog
add(X,nil,t(X,nil,nil)).
add(X,t(Root,Left,Right),t(Root,LeftNew,Right)):-
	X@<Root,
	add(X,Left,LeftNew).
add(X,t(Root,Left,Right),t(Root,Left,RightNew)):-
	X@>Root,
	add(X,Right,RightNew).

construct(L,T):-construct(L,T,nil).
construct([],T,T).
construct([X|Xs],T,T1):-
	add(X,T1,T2),
	construct(Xs,T,T2).
	

```


<br/>
<hr/>
<br/>
<h2 id="104"> 104. cbal_tree.pl/</h2>

``` prolog
split(N,N1,N2):-N1 is (N-1)//2, N2 is (N-1)-N1.
split(N,N1,N2):-N2 is (N-1)//2, N1 is (N-1)-N2,\+N1=N2.

mybalanced_tree(0,nil):-!.
mybalanced_tree(N,t(x,L,R)):-
	split(N,N1,N2),
	mybalanced_tree(N1,L),
	mybalanced_tree(N2,R).
	


```


<br/>
<hr/>
<br/>
<h2 id="105"> 105. compare.pl/</h2>

``` prolog
tell_order(A,B):-
	compare('<',A,B),
	write(less_than),nl.

tell_order(A,B):-
	compare('=',A,B),
	write(equal_to),nl.

tell_order(A,B):-
	compare('>',A,B),
	write(greater_than),nl.
```


<br/>
<hr/>
<br/>
<h2 id="106"> 106. complete_binary_tree.pl/</h2>

``` prolog
split(0,0,0):-!.
split(N,N1,N2):-
	N1t is N//2,N2t is N-N1t,
	maxFirst(N1t,N2t,N1,N2).

maxFirst(N1,N2,M1,M2):-
	N1<N2,
	M1 is N2,M2 is N1,!.
maxFirst(N1,N2,N1,N2).


complete_binary_tree(0,nil):-!.
complete_binary_tree(N,t(_,L,R)):-
	N1 is N-1,
	split(N1,M1,M2),
	complete_binary_tree(M1,L),
	complete_binary_tree(M2,R).
```


<br/>
<hr/>
<br/>
<h2 id="107"> 107. countleaves.pl/</h2>

``` prolog
count_leaves(nil,0).
count_leaves(t(_,L,R),N):-count_leaves(L,N1),count_leaves(R,N2),N is N1+N2+1.
```


<br/>
<hr/>
<br/>
<h2 id="108"> 108. crossword.pl/</h2>

``` prolog
:-ensure_loaded(p99-readfile).
bind_crossword(1,'p99a.dat').
bind_crossword(2,'p99b.dat').
bind_crossword(3,'p99c.dat').
bind_crossword(4,'p99d.dat').

is_word([X]):-!,char_type(X,alpha).
is_word([X|Xs]):-
	char_type(X,alpha),
	is_word(Xs).

generate_word_list(L,WL):-
	setof(WordLength-Word,
	      (member(Word,L),is_word(Word),length(Word,WordLength)),
	      WL).

is_puzzle_entry('.').
is_puzzle_entry(' ').

is_puzzle_list(S):-
	maplist(is_puzzle_entry,S),
	\+ S=[].

generate_syms([],[]).
generate_syms(['.'|Xs],[_|Syms]):-!,
	generate_syms(Xs,Syms).
generate_syms([' '|Xs],[' '|Syms]):-!,
	generate_syms(Xs,Syms).
generate_puzzle(L,P):-
	bagof(ML,
	      (member(ML,L),is_puzzle_list(ML)),
	      PL),
	maplist(generate_syms,PL,P).

is_valid_puzzle_entry(X):-var(X),!.
is_valid_puzzle_entry(' ').


chk_prow(PR):-
	maplist(is_valid_puzzle_entry,PR).

chk_assum(P):-
	maplist(chk_prow,P).


is_valid_word_entry(X):-var(X),!.
is_valid_word_entry(X):-char_type(X,upper).

get_word([],[],[]).
get_word([X|Xs],[X|RestOfWord],RemainingInputList):-
	is_valid_word_entry(X),!,
	get_word(Xs,RestOfWord,RemainingInputList).
get_word(RemaingInputList,[],RemaingInputList).

is_valid_word([_W1,_W2|_Ws]).

words_list([],[]):-!.
words_list([X|Xs],L):-
	\+ is_valid_word_entry(X),!,
	words_list(Xs,L).
words_list(R,[W|Ws]):-
	get_word(R,W,Rs),
	is_valid_word(W),!,
	words_list(Rs,Ws).
words_list(R,Ws):-
	get_word(R,_,Rs),!,
	words_list(Rs,Ws).

getwords([],[]).
getwords([R|Rs],L):-
	words_list(R,L1),
	getwords(Rs,L2),
	append(L1,L2,L).

get_col([],[],[]).
get_col([[X|Xs]|Rs],[X|Cs],[Xs|RXs]):-
	get_col(Rs,Cs,RXs).

get_cols([[]|_],[]).
get_cols(Rs,[C|Cs]):-
	get_col(Rs,C,RRs),
	get_cols(RRs,Cs).
	
get_puzzle_words(P,Ws):-
	getwords(P,Ws1),
	get_cols(P,PC),
	getwords(PC,Ws2),
	append(Ws1,Ws2,Ws).

add_length_prefix(V,N-V):-
	length(V,N).
tag_permutation_groups_with_length([],[]).
tag_permutation_groups_with_length([_-[X,Y]|Gs],[K-[X,Y]|PGs]):-
	length(X,K),
	tag_permutation_groups_with_length(Gs,PGs).

unify_list([],[]).
unify_list([X|Xs],[X|Ys]):-
	unify_list(Xs,Ys).

unify_permutable_group([],[]).
unify_permutable_group([G1|G1s],G2L):-
	select(G2,G2L,G2s),
	unify_list(G1,G2),
	unify_permutable_group(G1s,G2s).

unify_permutable_groups([]).
unify_permutable_groups([_-[G1,G2]|Gs]):-
	unify_permutable_group(G1,G2),
	unify_permutable_groups(Gs).
		       


print_crossie_row(L):-
	maplist(write,L),nl.

testc(N):-
	bind_crossword(N,FName),
	read_lines(FName,L),
	generate_word_list(L,WL),
	generate_puzzle(L,P),
	get_puzzle_words(P,Ws),
	maplist(add_length_prefix,Ws,Wsp),
	msort(Wsp,Wsp1),
	group_pairs_by_key(Wsp1,GWsp),
	group_pairs_by_key(WL,GWL),
	append(GWsp,GWL,G),
	msort(G,G1),
	group_pairs_by_key(G1,GG),
	tag_permutation_groups_with_length(GG,GG1),
	msort(GG1,GG2),
	unify_permutable_groups(GG2),
	maplist(print_crossie_row,P).

	
	
```


<br/>
<hr/>
<br/>
<h2 id="109"> 109. doubts.pl/</h2>

``` prolog

predicate(A,B):-!.
predicate(A,B).

investigate(A,B) :-
	between(A,B,N),
	sym_cbal_trees(N,Ts),
	length(Ts,L),
	writef('%w   %w',[N,L]), nl,
    fail.
investigate(_,_).

program 60

program 66

program 80

graph_paint algo....
```


<br/>
<hr/>
<br/>
<h2 id="110"> 110. dropn.pl/</h2>

``` prolog
mydrop(L1,N,L2):-drop(L1,N,L2,N).
drop([],_,[],_).
drop([_X|Xs],1,Ys,N):-drop(Xs,N,Ys,N).
drop([X|Xs],N,[X|Ys],N):-N1 is N-1,drop(Xs,N1,Ys,N).
```


<br/>
<hr/>
<br/>
<h2 id="111"> 111. eightqueens.pl/</h2>

``` prolog
no_collision(Q,Qs):-
	\+ (member(Q1,Qs),Q1=Q),
	\+ (nth1(N,Qs,Q1),N =:= abs(Q1-Q)).

solve(N,Qs):-
	place_queen(N,N,Qs).

place_queen(_,0,[]).
place_queen(N,I,[Q|Qs]):-
	I>0,
	I1 is I-1,
	place_queen(N,I1,Qs),
	between(1,N,Q),
	no_collision(Q,Qs).
```


<br/>
<hr/>
<br/>
<h2 id="112"> 112. eliminate_consecutive_duplicates.pl/</h2>

``` prolog
eliminate_consecutive_duplicates(L1,L2):-eliminate_consecutive_duplicates(L1,L2,[]).
eliminate_consecutive_duplicates([],L2,Acc):-reverse(Acc,L2).
eliminate_consecutive_duplicates([X,X|Rx],L2,Acc):-eliminate_consecutive_duplicates([X|Rx],L2,Acc).
eliminate_consecutive_duplicates([X|Rx],L2,Acc):-eliminate_consecutive_duplicates(Rx,L2,[X|Acc]).
```


<br/>
<hr/>
<br/>
<h2 id="113"> 113. functor.pl/</h2>

``` prolog
nple(M,I,O):-O is M*I.
```


<br/>
<hr/>
<br/>
<h2 id="114"> 114. graph.pl/</h2>

``` prolog
bind_graph(human_readable_graph([b-c, f-c, g-h, d, f-b, k-f, h-g])).
bind_graph(human_readable_graph([s > r, t, u > r, s > u, u > s, v > u])).
bind_graph(human_readable_graph([p>q/9, m>q/7, k, p>m/5])).
%bind_graph(adjacency_list([n(b,[c,f]), n(c,[b,f]), n(d,[]),n(f,[b,c,k]),n(k,[f]),n(g,[h]),n(h,[g])])).
%bind_graph(adjacency_list([n(r,[]),n(s,[r,u]),n(t,[]),n(u,[r]),n(v,[u])])).
%bind_graph(adjacency_list([n(k,[]),n(m,[q/7]),n(p,[m/5,q/9]),n(q,[])])).
%bind_graph(graph([k,m,p,q],[a(m,p,7),a(p,m,5),a(p,q,9)])).
%bind_graph(graph([b,c,d,f,g,h,k],[e(b,c),e(b,f),e(c,f),e(f,k),e(g,h)])).
%bind_graph(graph([r,s,t,u,v],[a(s,r),a(s,u),a(u,r),a(u,s),a(v,u)])).


convert(human_readable_graph(X),Y):-
	convert_hrg_g(X,Y).
convert(human_readable_graph(X),Y):-
	convert_hrg_al(X,Y).

edge_end(X-_,X).
edge_end(_-X,X).
edge_end(X>_,X).
edge_end(_>X/_,X):-!.
edge_end(_>X,X):-!.
edge_end(
other_end(X-Y,X,Y).
other_end(X-Y,Y,X).
other_end(X>Y,X,Y).

setof_nodes(human_readable_graph(X),L):-
	setof(Node,Edge^(member(Edge,X),edge_end(Edge,Node)),L).

graph_edge(X-Y,e(X,Y)).
graph_edge(X>Y/N,a(X,Y,N)):-!.
graph_edge(X>Y,a(X,Y)).

setof_graph_edges(human_readable_graph(X),SetOfGraphEdges):-
	setof(GEdge,Edge^(member(Edge,X),graph_edge(Edge,GEdge)),SetOfGraphEdges).

node_neighbours(human_readable_graph(X),Node,NodeNeighbours):-
	setof(OtherEnd,
	      Edge^(member(Edge,X),other_end(Edge,Node,OtherEnd)),
	      NodeNeighbours).

	      
graph_neighbour_nodes(human_readable_graph(X),SetOfNodeNeighbours):-
	setof_nodes(human_readable_graph(X),Nodes),
	setof(n(Node,NodeNeighbours),
	      (member(Node,Nodes),
	       node_neighbours(human_readable_graph(X),
			       Node,
			       NodeNeighbours)),
	      SetOfNodeNeighbours).


convert_hrg_g(X,graph(SetOfNodes,SetOfEdges)):-
	setof_nodes(human_readable_graph(X),SetOfNodes),
	setof_graph_edges(human_readable_graph(X),SetOfEdges).

	
convert_hrg_al(X, adjacency_list(NodeNeighbours)):-
	graph_neighbour_nodes(human_readable_graph(X),NodeNeighbours).
	

convert_al_hrg(X,human_readable_graph(Y)):-


    

    
		   

```


<br/>
<hr/>
<br/>
<h2 id="115"> 115. graph_dfo.pl/</h2>

``` prolog
other_end(e(N,NN,_),N,NN).
other_end(e(NN,N,_),N,NN).
other_end(e(N,NN),N,NN).
other_end(e(NN,N),NN,N).

neighbour(N,Es,NN):-
	member(E,Es),
	other_end(E,N,NN).

neighbours(N,Es,NNs):-
	setof(NN,E^(member(E,Es),other_end(E,N,NN)),NNs).

dfo(graph(Ns,Es),Start,Seq):-
	dfo_dl(Es,
	       [Start],_FrontOut,
	       _,Seq,
	       Ns,_RemainingNodesOut).

symbol(X,[X|L]-L).
dfo(Es,
    [],[],
    SeqIn,SeqIn,
    [],[]):-!.
dfo(Es,
    [],FrontNodesOut,
    SeqIn,SeqOut,
    RemainingNodesIn,RemainingNodesOut):-
	select(N,RemainingNodesIn,RemainingNodesInNew),
	dfo(Es,
	    [N],FrontNodesOut,
	    SeqIn,SeqOut,
	    RemainingNodesInNew,RemainingNodesOut).
addNodeToSequence(	
dfo(Es,
    [FrontNode|FrontNodesIn],FrontNodesOut,
    SeqIn,SeqOut,
    RemainingNodesIn,RemainingNodesOut):-
	addNodeToSequence(FrontNode,SeqIn,SeqInNew),
	neighbours(FrontNode,Es,Neighbours),
	subtract(Neighbours,SeqInNew,NewFrontNodes),
	append(NewFrontNodes,FrontNodesIn,FrontNodesInNew),
	dfo(Es,
	    FrontNodesInNew,FrontNodesOut,
	    SeqInNew,SeqOut,
	    RemainingInNew,RemainingNodesOut),
```


<br/>
<hr/>
<br/>
<h2 id="116"> 116. graph_match.pl/</h2>

``` prolog
bind_graph1(graph([a,b,c],[e(a,b),e(a,c)])).
bind_graph1(graph([a,b,c,d,e,f,g],[e(a,b),e(a,c),e(a,d),e(b,e),e(d,f),e(f,g)])).
bind_graph1(graph([a,b],[e(a,b)])).
bind_graph1(graph([a],[])).
bind_graph1(graph([b,c,d,f,g,h,k],[e(b,c),e(b,f),e(c,f),e(d,c),
				   e(f,k),e(f,g),e(g,h)])).
bind_graph1(graph([k,m,p,q],[e(m,p,7),e(p,m,5),e(p,q,9)])).
bind_graph1(graph([b,c,d,f,g,h,k],[e(b,c),e(b,f),e(c,f),e(f,k),e(g,h)])).
bind_graph1(graph([r,s,t,u,v],[e(s,r),e(s,u),e(u,r),e(u,s),e(v,u)])).

bind_graph2(graph([aa,bb,cc],[e(aa,bb),e(aa,cc)])).
bind_graph2(graph([aa,bb,cc,dd,ee,ff,gg],[e(aa,bb),e(aa,cc),
					  e(aa,dd),e(bb,ee),
					  e(dd,ff),e(ff,gg)])).
bind_graph2(graph([c,d],[e(c,d)])).
bind_graph2(graph([c],[])).
bind_graph2(graph([kk,bb,ff,dd,gg,cc,hh],[e(gg,hh),e(bb,cc),e(bb,ff),
					  e(cc,ff),e(dd,cc),
					  e(ff,kk),e(ff,gg)])).
bind_graph2(graph([m,p,k,q],[e(m,p,7),e(p,q,9),e(p,m,5)])).
bind_graph2(graph([g,k,b,d,f,c,h],[e(b,f),e(f,k),e(b,c),e(g,h),e(c,f)])).
bind_graph2(graph([r,v,t,u,s],[e(s,r),e(u,r),e(u,s),e(v,u),e(s,u)])).

bind_edges([e(a,b),e(a,c),e(b,c),e(d,e),e(b,d),e(a,d)]).
bind_nodes([a,b,c,d,e,f]).
other_end(e(X,Y,_),X,Y):-!.
other_end(e(X,Y,_),Y,X):-!.
other_end(e(X,Y),X,Y):-!.
other_end(e(X,Y),Y,X):-!.

neighbour_nodes(Edges,Node,Neighbours):-
	nonvar(Edges),
	nonvar(Node),
	setof(Neighbour,
	      Edge^(member(Edge,Edges),other_end(Edge,Node,Neighbour)),
	      Neighbours),!.
neighbour_nodes(_,_,[]).

pattern_match(graph(Nodes1,Edges1),graph(Nodes2,Edges2),Match):-
	pattern_match(graph(Nodes1,Edges1),graph(Nodes2,Edges2),
		      [], % front
		      Nodes1,Nodes2, % remaining nodes in the two graphs
		      Match).

pattern_match(_,_,[],[],[],[]).

pattern_match(_,_,
	      [],
	      [TryN1|RestOfRemainingNs1],RemainingNs2,
	      [m(TryN1,TryN2)|RestOfMatch]):-
	select(TryN2,RemainingNs2,RestOfRemainingNs2),
	pattern_match(_,_,
		      [m(TryN1,TryN2)],
		      RestOfRemainingNs1,RestOfRemainingNs2,
		      RestOfMatch).

pattern_match(graph(_,Edges1),graph(_,Edges2),
	      [m(X1,X2)|RestOfFront],
	      RemainingNodes1,RemainingNodes2,
	      Match):-
	neighbour_nodes(Edges1,X1,Neighbours1),
	neighbour_nodes(Edges2,X2,Neighbours2),
	intersection(RemainingNodes1,Neighbours1,NewNodesInFront1),
	intersection(RemainingNodes2,Neighbours2,NewNodesInFront2),
	permute(NewNodesInFront1,NewNodesInFront2,CurrentMatch),
	append(CurrentMatch,RestOfFront,NewFront),
	ord_subtract(RemainingNodes1,NewNodesInFront1,RestOfRemainingNodes1),
	ord_subtract(RemainingNodes2,NewNodesInFront2,RestOfRemainingNodes2),
	pattern_match(graph(_,Edges1),graph(_,Edges2),
		      NewFront,
		      RestOfRemainingNodes1,RestOfRemainingNodes2,
		      RestOfMatch),
	
	append(CurrentMatch,RestOfMatch,Match).

permute([],[],[]).
permute([X1|X1s],L2,[m(X1,X2)|Ms]):-
	select(X2,L2,X2s),
	permute(X1s,X2s,Ms).

test:-
	bind_graph1(X1),bind_graph2(X2),
	write(X1),nl,write(X2),nl,
	pattern_match(X1,X2,M),
	write(M),nl,fail.

```


<br/>
<hr/>
<br/>
<h2 id="117"> 117. graph_paint.pl/</h2>

``` prolog
bind_graph(graph([b,c,d,f,g,h,k],[e(b,c),e(b,f),e(c,f),e(d,c),
				  e(f,k),e(f,g),e(g,h)])).
bind_graph(graph([k,m,p,q],[a(m,p,7),a(p,m,5),a(p,q,9)])).
bind_graph(graph([b,c,d,f,g,h,k],[e(b,c),e(b,f),e(c,f),e(f,k),e(g,h)])).
bind_graph(graph([r,s,t,u,v],[a(s,r),a(s,u),a(u,r),a(u,s),a(v,u)])).

degree(graph(_,Es),Node,Degree):-
	neighbouring_nodes(Es,Node,NeighbouringNodes),
	length(NeighbouringNodes,Degree).

neighbouring_nodes(Es,N,NNs):-
	setof(NN,bind_neighbour(N,Es,NN),NNs),!.
neighbouring_nodes(_,_,[]).

bind_neighbour(N,Es,NN):-
	member(e(N,NN),Es).

bind_neighbour(N,Es,NN):-
	member(e(NN,N),Es).

bind_neighbour(N,Es,NN):-
	member(a(N,NN),Es).

bind_neighbour(N,Es,NN):-
	member(a(N,NN,_),Es).

degree_attached_nodes(_,[],[]).
degree_attached_nodes(Es,[N|Ns],[p(D,N)|DNs]):-
	degree(graph(_,Es),N,D),
	degree_attached_nodes(Es,Ns,DNs).

detach_degree_from_nodes([],[]).
detach_degree_from_nodes([p(_,N)|DNs],[N|Ns]):-
	detach_degree_from_nodes(DNs,Ns).

reorder_nodes_in_decreasing_degree_order(graph(Ns,Es),graph(NsDecreasingOrder,Es)):-
	degree_attached_nodes(Es,Ns,DNs),
	write(DNs),nl,
	sort(DNs,SortedDNs),
	write(SortedDNs),nl,
	detach_degree_from_nodes(SortedDNs,NsIncreasingOrder),
	reverse(NsIncreasingOrder,NsDecreasingOrder).

test:-
	bind_graph(X),
	reorder_nodes_in_decreasing_degree_order(X,NX),
	write(X),nl,
	write(NX),nl,
	fail.
```


<br/>
<hr/>
<br/>
<h2 id="118"> 118. graph_path.pl/</h2>

``` prolog
bind_graph(graph([b,c,d,f,g,h,k],[e(b,c),e(b,f),e(c,f),e(d,c),
				  e(f,k),e(f,g),e(g,h)])).
bind_graph(graph([k,m,p,q],[a(m,p,7),a(p,m,5),a(p,q,9)])).
bind_graph(graph([b,c,d,f,g,h,k],[e(b,c),e(b,f),e(c,f),e(f,k),e(g,h)])).
bind_graph(graph([r,s,t,u,v],[a(s,r),a(s,u),a(u,r),a(u,s),a(v,u)])).

other_end(e(X,Y,_),X,Y).
other_end(e(X,Y,_),Y,X).
other_end(e(X,Y),X,Y).
other_end(e(X,Y),Y,X).
other_end(a(X,Y),X,Y).
other_end(a(X,Y,_),X,Y).

neighbour_nodes(Edges,Node,Neighbours):-
	setof(Neighbour,
	      Edge^(member(Edge,Edges),other_end(Edge,Node,Neighbour)),
	      Neighbours).

graph_path(graph(Nodes,Edges),Start,End,[Start|RestOfPath]):-
	delete(Nodes,Start,Available),
	graph_path(graph(Nodes,Edges),Start,End,RestOfPath,Available).

graph_path(_,Start,Start,[],_).
graph_path(graph(Nodes,Edges),Start,End,[Neighbour|RestOfPath],Available):-
	neighbour_nodes(Edges,Start,Neighbours),
	member(Neighbour,Neighbours),
	member(Neighbour,Available),
	delete(Available,Neighbour,RestOfAvailable),
	graph_path(graph(Nodes,Edges),
		   Neighbour,
		   End,
		   RestOfPath,RestOfAvailable).

test:-bind_graph(graph(Nodes,Edges)),
	write(Nodes),nl,
	write(Edges),nl,
	member(Node1,Nodes),
	member(Node2,Nodes),
	write('------------------------------------------------'),nl,
	write('start : '),write(Node1),nl,
	write('end   : '),write(Node2),nl,
	graph_path(graph(Nodes,Edges),Node1,Node2,Path),
	write('path  : '),write(Path),nl.

% X is adjacent to Y in graph(_,Es).
adjacent(X,Y,graph(_,Es)):-member(e(X,Y),Es).
adjacent(X,Y,graph(_,Es)):-member(e(Y,X),Es).
adjacent(X,Y,graph(_,Es)):-member(e(X,Y,_),Es).
adjacent(X,Y,graph(_,Es)):-member(e(Y,X,_),Es).
adjacent(X,Y,graph(_,Es)):-member(a(Y,X),Es).
adjacent(X,Y,graph(_,Es)):-member(a(Y,X,_),Es).

adjacent_edges(X,Y,Es):-adjacent(X,Y,graph(_,Es)).

graph_cycle(graph(Nodes,Edges),Start,[Start|Xs]):-
	is_graph_path(graph(Nodes,Edges),Xs-[Start]).

is_graph_path(_,L-L).

is_graph_path(Graph,[X1,X2|Xs]-L):-
	adjacent(X2,X1,Graph),
	is_graph_path(Graph,[X2|Xs]-L).
				    	
graph_cycle_new(graph(Nodes,Edges),
		Start,
		[Start|RestOfCycle]):-
	adjacent(Neighbour,Start,graph(Nodes,Edges)),
	graph_path(graph(Nodes,Edges),Neighbour,Start,RestOfCycle).

combination(_,0,[]).
combination(Elements,N,[X|Cs]):-
	N>0,
	append([_,[X],Right],Elements),
	N1 is N-1,
	combination(Right,N1,Cs).

s_tree(graph(Nodes,Edges),graph(Nodes,STreeEdges)):-
	length(Nodes,NNodes),
	NSTreeEdges is NNodes-1,
	combination(Edges,NSTreeEdges,STreeEdges),
	is_tree(graph(Nodes,STreeEdges)).
	
is_connected(graph(Nodes,Edges)):-
	append([Left,[Node],Right],Nodes),
	append([Left,Right],RemainingNodes),
	is_connected(Edges,RemainingNodes,[Node]).

is_connected(_,[],_).
is_connected(Edges,RemainingNodes,[FrontNode|RestOfFrontNodes]):-
	neighbour_nodes(Edges,FrontNode,Neighbours),
	intersection(Neighbours,RemainingNodes,NewFrontNodes),
	ord_subtract(RemainingNodes,NewFrontNodes,NewRemainingNodes),
	append([NewFrontNodes,RestOfFrontNodes],FrontNodes),
	is_connected(Edges,NewRemainingNodes,FrontNodes).
	
is_tree(graph(Nodes,Edges)):-
	is_connected(graph(Nodes,Edges)),
	length(Nodes,NNodes),
	NEdges is NNodes-1,
	length(Edges,NEdges).
	
	
```


<br/>
<hr/>
<br/>
<h2 id="119"> 119. group.pl/</h2>

``` prolog
combination(0,Rest,[],Rest).
combination(N,L,[X|Xs],Rest):-
	append([Left,[X],Right],L),
	N1 is N-1,
	combination(N1,Right,Xs,Rest1),
	append([Left,Rest1],Rest).
group(_,[],[]).
group(L,[N|Ns],[G|Gs]):-
	combination(N,L,G,Rest),
	group(Rest,Ns,Gs).
	
```


<br/>
<hr/>
<br/>
<h2 id="120"> 120. hbal_tree.pl/</h2>

``` prolog
:-ensure_loaded(cbal_tree).

max_min_depth(t(_,R,L),D):-
	max_min

is_height_balanced_tree(t(_,R,L)):-
	max_min_depth(R,DrMin,DrMax),
	max_min_depth(L,DlMin,DlMax),
	\+ abs(min(DrMin,DlMin)-Dl) > 1.

hbal_tree(4,T):-
```


<br/>
<hr/>
<br/>
<h2 id="121"> 121. heap.pl/</h2>

``` prolog
heap_get_min
```


<br/>
<hr/>
<br/>
<h2 id="122"> 122. huffman.pl/</h2>

``` prolog
%bind([fr(a,1),fr(b,2),fr(c,3),fr(d,4)]).
bind([fr(a,45),fr(b,13),fr(c,12),fr(d,16),fr(e,9),fr(f,5)]).

init([],[]).
init([fr(S,F)|Fs],[n(F,S)|Ns]) :- init(Fs,Ns).

find_huffman_code(F,H):-
	init(F,Nus),
	sort(Nus,Ns),
	make_tree(Ns,Tree),
	convert_tree_to_code(Tree,'',H).

convert_tree_to_code(n(_,p(N1,N2)),ICode,Code):-
	atom_concat(ICode,0,ICodeLeft),
	convert_tree_to_code(N1,ICodeLeft,Code1),
	atom_concat(ICode,1,ICodeRight),
	convert_tree_to_code(N2,ICodeRight,Code2),
	append([Code1,Code2],Code).

convert_tree_to_code(n(_,S),ICode,[hc(S,ICode)]):-!.
	
	
make_tree(Ns,Tree):-
	length(Ns,L),
	L1 is L-1,
	merge_the_lowest(Ns,Tree,L1).

merge_the_lowest([X],X,0):-!.
merge_the_lowest(InputTree,OutputTree,L):-
	merge_the_two_lowest(InputTree,Tree1),
	update_the_position_of_the_first_element(Tree1,Tree2),
	L1 is L-1,
	merge_the_lowest(Tree2,OutputTree,L1).


update_the_position_of_the_first_element([n(F1,S1),n(F2,S2)|Xs],
					 [n(F1,S1),n(F2,S2)|Xs]):-F1<F2.
update_the_position_of_the_first_element([n(F1,S1),n(F2,S2)|Xs],
					 [n(F2,S2)|Ys]):-F1>F2,
	update_the_position_of_the_first_element([n(F1,S1)|Xs],Ys).
update_the_position_of_the_first_element([X],[X]):-write(here1),nl,!.


merge_the_two_lowest([n(F1,S1),n(F2,S2)|Ns],[n(F,p(n(F1,S1),n(F2,S2)))|Ns]):-F is F1+F2.
merge_the_two_lowest(_):-!.
```


<br/>
<hr/>
<br/>
<h2 id="123"> 123. identifier.pl/</h2>

``` prolog
identifier([X|Xs]):-
	char_type(X,alpha),
	intermediate(Xs).

is_valid_intermediate_entry('-').
is_valid_intermediate_entry(X):-char_type(X,alnum).

is_valid_last_entry(X):-char_type(X,alnum).


intermediate([]).
intermediate([X,Y]):-!,
	is_valid_intermediate_entry(X),
	is_valid_last_entry(Y).
intermediate([X|Xs]):-
	is_valid_intermediate_entry(X),
	intermediate(Xs).
```


<br/>
<hr/>
<br/>
<h2 id="124"> 124. incompleteList.pl/</h2>

``` prolog
bind_incomplete([]).mypack.plmypack.pl
```


<br/>
<hr/>
<br/>
<h2 id="125"> 125. insert_at.pl/</h2>

``` prolog
:-ensure_loaded(remove_at).
sinsert_at(X,L,K,R):-sremove_at(X,R,K,L).
```


<br/>
<hr/>
<br/>
<h2 id="126"> 126. istree.pl/</h2>

``` prolog
bind(t(a,t(b,nil,nil),nil)).
bind(t(a,t(b,nil,nil))).
is_tree(t(_,nil,nil)):-!.
is_tree(t(_,Y,nil)):-is_tree(Y),!.
is_tree(t(_,nil,Z)):-is_tree(Z),!.       
is_tree(t(_,Y,Z)):-is_tree(Y),is_tree(Z).


%istree(nil).
%istree(t(_,L,R)) :- istree(L), istree(R).

```


<br/>
<hr/>
<br/>
<h2 id="127"> 127. knapsack.pl/</h2>

``` prolog
:-ensure_loaded(library(clpr)).
:-ensure_loaded(library(simplex)).
knapsack_constrain(S) :-
	gen_state(S0),
	constraint([6*x(1), 4*x(2)] =< 8, S0, S1),
	constraint([x(1)] =< 1, S1, S2),
	constraint([x(2)] =< 2, S2, S).

knapsack(S) :-
	knapsack_constrain(S0),
	maximize([7*x(1), 4*x(2)], S0, S).

```


<br/>
<hr/>
<br/>
<h2 id="128"> 128. knight_tour.pl/</h2>

``` prolog
adxdy(1,2).
adxdy(2,1).
plus_or_minus(X,-X).
plus_or_minus(X,X).
dxdy(X,Y):-
	adxdy(I,J),
	plus_or_minus(I,X),
	plus_or_minus(J,Y).
	
next_move(X,Y):-
	var(X),!,
	next_move(Y,X).

next_move(p(I,J),p(I1,J1)):-
	dxdy(DX,DY),
	I1 is  I+DX,
	between(1,8,I1),
	J1 is J+DY,
	between(1,8,J1).

move(1,[p(1,1)]).
move(N,[Qc,Qp|QOldMoves]):-
	N1 is N-1,
	move(N1,[Qp|QOldMoves]),
	next_move(Qp,Qc),
	\+ member(Qc,[Qp|QOldMoves]).

write_moves(Qs):-
	nl,write('------------------------------------------------'),nl,
	write(Qs).
write_moves(_).
solve:-
	move(64,Qs),
	write_moves(Qs).
	
```


<br/>
<hr/>
<br/>
<h2 id="129"> 129. layout_binary_tree1.pl/</h2>

``` prolog
bind_layout_tree(t(a,t(b,nil,nil),t(c,nil,t(d,t(e,nil,nil),nil)))).
bind_layout_tree(t(a,t(b,nil,nil),t(c,nil,t(d,nil,nil)))).
bind_layout_tree(t(a,t(b,nil,nil),t(c,nil,nil))).
bind_layout_tree(t(a,nil,nil)).
bind_layout_tree(t(n,t(k,t(c,t(a,nil,nil),t(h,t(g,t(e,nil,nil),nil),nil)),t(m,nil,nil)),t(u,t(p,nil,t(s,t(q,nil,nil),nil)),nil))).
bind_layout_tree(t(1,t(2,t(3,nil,nil),t(4,nil,nil)),t(5,t(6,nil,nil),t(7,nil,nil)))).

layout_binary_tree1(T,PT):-layout_binary_tree1(T,1,0,PT,_).

layout_binary_tree1(nil,_,PosX_Prev,nil,PosX_Prev).
layout_binary_tree1(t(X,L,R),
		    Depth,
		    PosX_Prev,
		    t(X,PosX,Depth,PT_L,PT_R),
		    PosX_LastUsed_R):-
	DepthChild is Depth+1,
	layout_binary_tree1(L,
			    DepthChild,
			    PosX_Prev,
			    PT_L,
			    PosX_LastUsed_L),
	PosX is PosX_LastUsed_L+1,
	layout_binary_tree1(R,
			    DepthChild,
			    PosX,
			    PT_R,
			    PosX_LastUsed_R).

depth(nil,0).
depth(t(_,L,R),D):-
	depth(L,Dl),
	depth(R,Dr),
	D is max(Dl,Dr)+1.


layout_binary_tree2
```


<br/>
<hr/>
<br/>
<h2 id="130"> 130. leaves.pl/</h2>

``` prolog
leaves(nil,[]).
leaves(t(X,L,R),[X|Xs]):-
	leaves(L,Xs1),
	leaves(R,Xs2),
	append([Xs1,Xs2],Xs).
```


<br/>
<hr/>
<br/>
<h2 id="131"> 131. main.pl/</h2>

``` prolog
%%  mail test file for the 99 problem

file_search_path(workingWithList,'1-Working-with-prolog-lists-P01/').
file_search_path(arithmetic,'./2-Arithmetic-P31').
file_search_path(logicAndCodes,'./3-Logic-and-Codes-P46').
file_search_path(binaryTrees,'./4-Binary-Trees-P54').
file_search_path(multiwaryTrees,'./5-MultiwayTrees-P70B').
file_search_path(graphs,'./6-Graphs-P80').
file_search_path(miscellaneous,'./7-Miscellaneous-Problems-P90').

:- [workingWithList(p01)].
:- [workingWithList(p02)].
:- [workingWithList(p03)].
:- [workingWithList(p04)].
:- [workingWithList(p05)].
:- [workingWithList(p06)].
:- [workingWithList(p07)].
:- [workingWithList(p08)].
:- [workingWithList(p09)].
:- [workingWithList(p10)].
:- [workingWithList(p11)].
:- [workingWithList(p12)].
:- [workingWithList(p13)].
:- [workingWithList(p14)].
:- [workingWithList(p15)].
:- [workingWithList(p16)].
:- [workingWithList(p17)].
:- [workingWithList(p18)].
:- [workingWithList(p19)].
:- [workingWithList(p20)].
:- [workingWithList(p21)].
:- [workingWithList(p22)].
:- [workingWithList(p23)].
:- [workingWithList(p24)].
:- [workingWithList(p25)].
:- [workingWithList(p26)].
:- [workingWithList(p27)].
:- [workingWithList(p28)].


:- [arithmetic(p31)].
:- [arithmetic(p32)].
:- [arithmetic(p33)].
:- [arithmetic(p34)].
:- [arithmetic(p35)].
:- [arithmetic(p36)].
:- [arithmetic(p37)].
:- [arithmetic(p38)].
:- [arithmetic(p39)].
:- [arithmetic(p40)].
:- [arithmetic(p41)].

:- [logicAndCodes(p46)].
:- [logicAndCodes(p47)].
:- [logicAndCodes(p48)].  %% funny
:- [logicAndCodes(p49)].
:- [logicAndCodes(p49_gray)].
:- [logicAndCodes(p50)].


:- [binaryTrees(p54)].
:- [binaryTrees(p55)].
:- [binaryTrees(p56)].
:- [binaryTrees(p57)].
:- [binaryTrees(p58)].
:- [binaryTrees(p59)].
:- [binaryTrees(p60)].
:- [binaryTrees(p61)].
:- [binaryTrees(p62)].
:- [binaryTrees(p63)].
:- [binaryTrees(p64)].
:- [binaryTrees(p65)].
:- [binaryTrees(p66)].
%:- [binaryTrees(p67a)].
:- [binaryTrees(p67b)].
%:- [binaryTrees(p68a)].
%:- [binaryTrees(p68b)].
%:- [binaryTrees(p68c)].
%:- [binaryTrees(p68d)].
:- [binaryTrees(p69)].

```


<br/>
<hr/>
<br/>
<h2 id="132"> 132. multi_tree.pl/</h2>

``` prolog
bind_mult_tree(t(a,[t(f,[t(g,[])]),t(c,[]),t(b,[t(d,[]),t(e,[])])])).
bind_mult_tree(t(a,[t(f,[])])).


bind_mult_tree_string('afg^^c^bd^e^^^').
bind_mult_tree_string('af^^').

bind_mult_tree_bottom_up_atom(gfcdeba).
bind_mult_tree_bottom_up_atom(fa).

is_mult_tree(t(_,List)):-
	is_mult_tree_list(List).

is_mult_tree_list([]).
is_mult_tree_list([X|Xs]):-
	is_mult_tree(X),
	is_mult_tree_list(Xs).

nnodes_multitree_list([],0).
nnodes_multitree_list([X|Xs],N):-
	nnodes_multitree(X,N1),
	nnodes_multitree_list(Xs,N2),
	N is N1+N2.

nnodes_multitree(t(_,L),N):-
	nnodes_multitree_list(L,N1),
	N is N1+1.


carat([^|Xs]-Xs).
symbol(X,[X|Xs]-Xs).
letter(X,[X|Xs]-Xs):-char_type(X,alpha).

multi_tree_forest_string_dl([],['^'|L]-L).
multi_tree_forest_string_dl([T|Ts],L1-L3):-
	multi_tree_string_dl(T,L1-L2),
	multi_tree_forest_string_dl(Ts,L2-L3).

multi_tree_string_dl(t(X,F),L1-L3):-
	letter(X,L1-L2),
	multi_tree_forest_string_dl(F,L2-L3).

multi_tree_string(T,A):-
	var(T),!,
	atom_chars(A,S),
	multi_tree_string_dl(T,S-[]).

multi_tree_string(T,A):-
	var(A),!,
	multi_tree_string_dl(T,S-[]),
	atom_chars(A,S).

ipl_multi_tree(T,L):-ipl_multi_tree(T,0,L).

ipl_multi_tree(t(_,F),D,L):-
	DF is D+1,
	ipl_multi_tree_forest(F,DF,LF),
	L is LF+D.

ipl_multi_tree_forest([],_,0).
ipl_multi_tree_forest([T|Ts],D,LF):-
	ipl_multi_tree(T,D,LT),
	ipl_multi_tree_forest(Ts,D,LTs),
	LF is LT+LTs.

bottom_up(T,A):-
	var(A),!,
	bottom_up_f(T,S-[]),
	atom_chars(A,S).

bottom_up(T,A):-
	var(T),!,
	atom_chars(A,S),
	bottom_up_f(T,S-[]).

bottom_up_f(t(X,F),L1-L3):-
	bottom_up_forest_f(F,L1-L2),
	letter(X,L2-L3).

bottom_up_forest_f([],L-L).
bottom_up_forest_f([T|Ts],L1-L3):-
	bottom_up_f(T,L1-L2),
	bottom_up_forest_f(Ts,L2-L3).


tree_ltl(T,L):-tree_ltl_d(T,L-[]).

tree_ltl_d(t(X,[]),[X|L]-L):-!.
tree_ltl_d(t(X,F),L1-L5):-
	symbol('(',L1-L2),
	letter(X,L2-L3),
	tree_ltl_forest_d(F,L3-L4),
	symbol(')',L4-L5).

tree_ltl_forest_d([],L-L).
tree_ltl_forest_d([T|Ts],L1-L3):-
	tree_ltl_d(T,L1-L2),
	tree_ltl_forest_d(Ts,L2-L3).
```


<br/>
<hr/>
<br/>
<h2 id="133"> 133. my-reverse.pl/</h2>

``` prolog
sunil_reverse(List,ReverseList):-sunil_reverse(List,ReverseList,[]).
sunil_reverse([],ReverseList,ReverseList):-!.
sunil_reverse([X|Xs],ReverseList,Accumulator):-sunil_reverse(Xs,ReverseList,[X|Accumulator]).

```


<br/>
<hr/>
<br/>
<h2 id="134"> 134. mybagof.pl/</h2>

``` prolog
foo(a, b, c).
foo(a, b, d).
foo(b, c, e).
foo(b, c, f).
foo(c, c, g).



```


<br/>
<hr/>
<br/>
<h2 id="135"> 135. mypack.pl/</h2>

``` prolog
stransfer([X|Xs],[X|Ys]):-stransfer(Xs,[X,X|Ys]).
stransfer([X|Xs],[]):-stransfer(Xs,[X]).
stransfer(X,Y).
```


<br/>
<hr/>
<br/>
<h2 id="136"> 136. nonograms.pl/</h2>

``` prolog
:- ensure_loaded(permute).


calc_list(L,C,SplitL):-
	calc_list(L,C,SplitL,0).

calc_list([],[],[],_).
calc_list([X|Xs],[C|Cs],[N1-N2|PNs],K):-
	N1 is K+C,
	N2 is N1+X,
	K1 is K+X,
	calc_list(Xs,Cs,PNs,K1).

nono_combination_row(L,NL,SplitL):-
	sumlist(L,Sum),
	length(L,K),
	N is NL-Sum,
	bagof(I,between(0,N,I),Is),
	combination(K,Is,C),
	calc_list(L,C,SplitL).

is_full([N1-N2|_],X):-
	X<N2,!,
	X>=N1.
is_full([_|PNs],X):-
	is_full(PNs,X).


nono_combination_col_noblank([Row|Rows],ColId,X,RowsRemaining):-
	is_full(Row,ColId),
	nono_combination_col_noblank(Rows,ColId,X1,RowsRemaining),
	X is X1+1.
nono_combination_col_noblank([Row|Rows],ColId,X,Rows):-
	\+ is_full(Row,ColId),
	X is 0.
nono_combination_col_noblank([],_,0,[]).

nono_combination_col([],_,[]).
nono_combination_col([Row|Rows],ColId,[X|Xs]):-
	is_full(Row,ColId),!,
	nono_combination_col_noblank(Rows,ColId,X1,RowsRemaining),
	X is X1+1,
	nono_combination_col(RowsRemaining,ColId,Xs).
nono_combination_col([_|Rows],ColId,Xs):-
	nono_combination_col(Rows,ColId,Xs).


chk_cols(Cs,Rows):-
	chk_cols(Cs,0,Rows).


chk_cols([C|Cs],ColId,Rows):-!,
	nono_combination_col(Rows,ColId,C),
	ColId1 is ColId+1,
	chk_cols(Cs,ColId1,Rows).
chk_cols([],_,_).

gen_rows([],_,[]).
gen_rows([R|Rs],Rmax,[CR|CRs]):-
	nono_combination_row(R,Rmax,CR),
	gen_rows(Rs,Rmax,CRs).

display_nonogram(A,B):-
	write(nonogram),nl,
	place('-',0,B),nl,
	display_nonogram_h(A,B),
	place('-',0,B),nl.

display_nonogram_h([],_).
display_nonogram_h([Row|Rows],Rmax):-
	display_row(Row,Rmax,0),nl,
	display_nonogram_h(Rows,Rmax).

place(X,From,To):-
	From<To,
	write(X),
	From1 is From+1,
	place(X,From1,To).
place(_,To,To).

display_row([],Rmax,Rmax):-!.
display_row([N1-N2|Rs],Rmax,CurColId):-
	place('.',CurColId,N1),
	place('X',N1,N2),
	display_row(Rs,Rmax,N2).
display_row([],Rmax,CurColId):-
	place('.',CurColId,Rmax).

solve_nonogram(Rows,Cols,CRs):-
	length(Cols,Rmax),
	gen_rows(Rows,Rmax,CRs),
	chk_cols(Cols,CRs).

bind_nono(1,
	  [[3],[2,1],[3,2],[2,2],[6],[1,5],[6],[1],[2]],
	  [[1,2],[3,1],[1,5],[7,1],[5],[3],[4],[3]]).
bind_nono(2,
	  [[2],[2]],
	  [[2],[2]]).
bind_nono(3,
	  [[1]],
	  [[1]]).
bind_nono(4,
	  [[3],[1,1],[3]],
	  [[3],[1,1],[3]]).
bind_nono(5,
	  [[3],[1],[3],[1],[3]],
	  [[3,1],[1,1,1],[1,3]]).

bind_nono(6,
	  [[3,1], [2,4,1], [1,3,3], [2,4], [3,3,1,3], [3,2,2,1,3],
	   [2,2,2,2,2], [2,1,1,2,1,1], [1,2,1,4], [1,1,2,2], [2,2,8],
	   [2,2,2,4], [1,2,2,1,1,1], [3,3,5,1], [1,1,3,1,1,2],
	   [2,3,1,3,3], [1,3,2,8], [4,3,8], [1,4,2,5], [1,4,2,2],
	   [4,2,5], [5,3,5], [4,1,1], [4,2], [3,3]],
	  [[2,3], [3,1,3], [3,2,1,2], [2,4,4], [3,4,2,4,5], [2,5,2,4,6],
	   [1,4,3,4,6,1], [4,3,3,6,2], [4,2,3,6,3], [1,2,4,2,1], [2,2,6],
	   [1,1,6], [2,1,4,2], [4,2,6], [1,1,1,1,4], [2,4,7], [3,5,6],
	   [3,2,4,2], [2,2,2], [6,3]]).

bind_nono(7,
	  [[5], [2,3,2], [2,5,1], [2,8], [2,5,11], [1,1,2,1,6], [1,2,1,3],
	   [2,1,1], [2,6,2], [15,4], [10,8], [2,1,4,3,6], [17], [17],
	   [18], [1,14], [1,1,14], [5,9], [8], [7]],
	  [[5], [3,2], [2,1,2], [1,1,1], [1,1,1], [1,3], [2,2], [1,3,3],
	   [1,3,3,1], [1,7,2], [1,9,1], [1,10], [1,10], [1,3,5], [1,8],
	   [2,1,6], [3,1,7], [4,1,7], [6,1,8], [6,10], [7,10], [1,4,11],
	   [1,2,11], [2,12], [3,13]]).


possible(Rows,Cols,Sum):-
	maplist(sumlist,Rows,RL),
	sumlist(RL,Sum),
	maplist(sumlist,Cols,CL),
	sumlist(CL,Sum).

test_gen_row(1):-
	bind_nono(Rows,Cols),
	length(Cols,Rmax),
	gen_rows(Rows,Rmax,CRs),
	L is Rmax+1,
	\+ length(CRs,L),
	length(CRs,ActualR),
	write([rmax,Rmax,actualr,ActualR]),nl,
	write(CRs),nl,
	write(displaying),nl,
	display_nonogram(CRs,Rmax),nl.

test_col(N):-
	bind_nono(N,Rows,Cols),
	length(Cols,Rmax),
	gen_rows(Rows,Rmax,CRs),
	display_nonogram(CRs,Rmax),
	Rmax1 is Rmax -1,
	between(0,Rmax1,CId),
	nono_combination_col(CRs,CId,C),
	write([cid,CId,c,C]),nl.

test_nono(N):-
	bind_nono(N,Rows,Cols),
	write([rows,Rows,cols,Cols]),nl,
	length(Cols,Rmax),
	solve_nonogram(Rows,Cols,CRs),
	display_nonogram(CRs,Rmax).
```


<br/>
<hr/>
<br/>
<h2 id="137"> 137. permute.pl/</h2>

``` prolog
permutation(0,_,[]).
permutation(K,L,[X|Xs]):-
	K>0,
	select(X,L,Rest),
	K1 is K-1,
	permutation(K1,Rest,Xs).

combination(0,_,[]).
combination(K,L,[X|Xs]):-
	K>0,
	append([_,[X],R],L),
	K1 is K-1,
	combination(K1,R,Xs).


```


<br/>
<hr/>
<br/>
<h2 id="138"> 138. permute_pair.pl/</h2>

``` prolog
permute([],[],[]).
permute([X1|X1s],L2,[m(X1,X2)|Ms]):-
	select(X2,L2,X2s),
	permute(X1s,X2s,Ms).

```


<br/>
<hr/>
<br/>
<h2 id="139"> 139. polymorph.pl/</h2>

``` prolog
bind_hello(sunil(a)).
bind_hello(victor(b)).
bind_hello(satish(c)).
hello(sunil(X),satish([X,X])).
hello(sunil(X),victor([X,X,X])).
hello(victor(X),satish([X,X,X,X])).

```


<br/>
<hr/>
<br/>
<h2 id="140"> 140. qsort.pl/</h2>

``` prolog
qsort([],[]).
qsort([X|Xs],O):-
	divide(X,Xs,Left,Right),
	qsort(Left,SortedLeft),
	qsort(Right,SortedRight),
	append([SortedLeft,[X],SortedRight],O).
divide(_,[],[],[]).
divide(X,[Y|Ys],[Y|Ls],Rs):-Y<X,divide(X,Ys,Ls,Rs).
divide(X,[Y|Ys],Ls,[Y|Rs]):-Y>X,divide(X,Ys,Ls,Rs).
```


<br/>
<hr/>
<br/>
<h2 id="141"> 141. record.pl/</h2>

``` prolog
:- record point(x,y).

```


<br/>
<hr/>
<br/>
<h2 id="142"> 142. regular.pl/</h2>

``` prolog
bind_graph1(1,graph([a,b,c],[e(a,b),e(a,c)])).
bind_graph1(2,graph([a,b,c,d,e,f,g],[e(a,b),e(a,c),e(a,d),e(b,e),e(d,f),e(f,g)])).
bind_graph1(3,graph([a,b],[e(a,b)])).
bind_graph1(4,graph([a],[])).
bind_graph1(5,graph([b,c,d,f,g,h,k],[e(b,c),e(b,f),e(c,f),e(d,c),
				     e(f,k),e(f,g),e(g,h)])).
bind_graph1(6,graph([k,m,p,q],[e(m,p,7),e(p,m,5),e(p,q,9)])).
bind_graph1(7,graph([b,c,d,f,g,h,k],[e(b,c),e(b,f),e(c,f),e(f,k),e(g,h)])).
bind_graph1(8,graph([r,s,t,u,v],[e(s,r),e(s,u),e(u,r),e(u,s),e(v,u)])).

bind_graph2(1,graph([aa,bb,cc],[e(aa,bb),e(aa,cc)])).
bind_graph2(2,graph([aa,bb,cc,dd,ee,ff,gg],[e(aa,bb),e(aa,cc),
					    e(aa,dd),e(bb,ee),
					    e(dd,ff),e(ff,gg)])).
bind_graph2(3,graph([c,d],[e(c,d)])).
bind_graph2(4,graph([c],[])).
bind_graph2(5,graph([kk,bb,ff,dd,gg,cc,hh],[e(gg,hh),e(bb,cc),e(bb,ff),
					    e(cc,ff),e(dd,cc),
					    e(ff,kk),e(ff,gg)])).
bind_graph2(6,graph([m,p,k,q],[e(m,p,7),e(p,q,9),e(p,m,5)])).
bind_graph2(7,graph([g,k,b,d,f,c,h],[e(b,f),e(f,k),e(b,c),e(g,h),e(c,f)])).
bind_graph2(8,graph([r,v,t,u,s],[e(s,r),e(u,r),e(u,s),e(v,u),e(s,u)])).

change_edge_rep(e(X,Y),X-Y).
change_edge_rep(e(X,Y,_),X-Y).


		
isomorph(graph(N1,E1),graph(N2,E2),L):-
	maplist(change_edge_rep,E1,E1L),
	maplist(change_edge_rep,E2,E2L),
	append(N1,E1L,G1),
	append(N2,E2L,G2),
	unify_graph(G1,G2,L).

unify_graph([],[],_).
unify_graph([G1|G1s],G2L,L):-
	select(G2,G2L,G2s),
	unify(G1,G2,L),
	unify_graph(G1s,G2s,L).

unify(X1-X2,Y1-Y2,L):-
	bind(X1,Y1,L),bind(X2,Y2,L).
unify(X1-X2,Y1-Y2,L):-
	bind(X1,Y2,L),bind(X2,Y1,L).
unify(X,Y,L):-
	\+ X=_-_,\+ Y=_-_,
	bind(X,Y,L).

bind(X,Y,L):-
	memberchk(X-Y0,L),nonvar(Y0),!,Y = Y0.
bind(X,Y,L):-
	memberchk(X0-Y,L),X = X0.

combination(_,0,[]).
combination(L,K,[C|Cs]):-
	append([_,[C],R],L),
	K1 is K-1,
	combination(R,K1,Cs).

test(N):-
	bind_graph1(N,G1),
	bind_graph2(N,G2),
	isomorph(G1,G2,L),
	maplist(writeln,[G1,G2,L]).

edge_rep([X,Y],e(X,Y)).

all_possible_edges(Nodes,Edges):-
	bagof(E,combination(Nodes,2,E),Es),
	maplist(edge_rep,Es,Edges).
all_possible_edges([_],[]).

power_set([],[]).
power_set([X|Xs],[X|PS]):-
	power_set(Xs,PS).
power_set([_|Xs],PS):-
	power_set(Xs,PS).

gen_graph(Ns,Es,graph(Ns,PEs)):-
	power_set(Es,PEs).

generate_graphs(NNodes,G):-
	length(Nodes,NNodes),
	maplist(gensym(n),Nodes),
	all_possible_edges(Nodes,Edges),
	gen_graph(Nodes,Edges,G).

mneighbours(_,[],[]).
mneighbours(N,[e(N,N2)|Es],[N2|Neighbours]):-!,
	mneighbours(N,Es,Neighbours).
mneighbours(N,[e(N1,N)|Es],[N1|Neighbours]):-!,
	mneighbours(N,Es,Neighbours).
mneighbours(N,[_|Es],Neighbours):-!,
	mneighbours(N,Es,Neighbours).

remove_isomorphic_entries([],[]).
remove_isomorphic_entries([G|Gs],[G|NonIsomorphicGs]):-
	(bagof(NIG,(member(NIG,Gs),\+ isomorph(G,NIG,_)),NIGs),!;NIGs=[]),
	remove_isomorphic_entries(NIGs,NonIsomorphicGs).

generate_non_isomorphic_k_regular_graphs(N,K,NonIsomorphicG):-
	bagof(G,generate_k_regular_graphs(N,K,G),Gs),
	remove_isomorphic_entries(Gs,NonIsomorphicGs),
	member(NonIsomorphicG,NonIsomorphicGs).

valency(graph(_,Es),V,N):-
	mneighbours(N,Es,Neighbours),
	length(Neighbours,V).

is_graph_k_regular(graph(Ns,Es),K):-
	maplist(valency(graph(Ns,Es),K),Ns).

generate_k_regular_graphs(NNodes,K,G):-
	generate_graphs(NNodes,G),
	is_graph_k_regular(G,K).
```


<br/>
<hr/>
<br/>
<h2 id="143"> 143. remove_at.pl/</h2>

``` prolog
sremove_at(X,[X|Xs],1,Xs).
sremove_at(X,[Y|Xs],K,[Y|R]):- K1 is K-1,sremove_at(X,Xs,K1,R).
```


<br/>
<hr/>
<br/>
<h2 id="144"> 144. rotate.pl/</h2>

``` prolog
:-ensure_loaded(splitn).
myrotate(L,N,Lr):-split(L,N,L1,L2),append([L2,L1],Lr).
```


<br/>
<hr/>
<br/>
<h2 id="145"> 145. runlength.modified.pl/</h2>

``` prolog
runlength([],[]).
runlength(L1,[X|L2]):-first_code(L1,1,X,Rest),runlength(Rest,L2).
runlength(L1,[[N,X]|L2]):-first_code(L1,N,X,Rest),runlength(Rest,L2).
first_code([X|Xs],0,Y,[X|Xs]):-X\=Y.
first_code([X],1,X,[]).
first_code([X|Xs],N1,X,Rest):-first_code(Xs,N,X,Rest), N1 is N+1.
```


<br/>
<hr/>
<br/>
<h2 id="146"> 146. runlength.pl/</h2>

``` prolog
runlength([],[]).
runlength(L1,[[N,X]|L2]):-first_code(L1,N,X,Rest),runlength(Rest,L2).
first_code([X|Xs],0,Y,[X|Xs]):-X\=Y.
first_code([X],1,X,[]).
first_code([X|Xs],N1,X,Rest):-first_code(Xs,N,X,Rest), N1 is N+1.
```


<br/>
<hr/>
<br/>
<h2 id="147"> 147. runlengthdecode.pl/</h2>

``` prolog
decode(X,Y):-runlengthdecode(X,Z),append(Z,Y).
runlengthdecode([],[]).
runlengthdecode([C|Cs],[D|Ds]):-decodeelement(C,D),runlengthdecode(Cs,Ds).
decodeelement([1,X],[X]).
decodeelement([N,X],[X|Ds]):- N1 is N-1,decodeelement([N1,X],Ds).
decodeelement(X,[X]).
```


<br/>
<hr/>
<br/>
<h2 id="148"> 148. runlengthmodified.pl/</h2>

``` prolog
runlength([],[]).
runlength(L1,[X|L2]):-first_code(L1,1,X,Rest),runlength(Rest,L2).
runlength(L1,[[N,X]|L2]):-first_code(L1,N,X,Rest),runlength(Rest,L2).
first_code([X|Xs],0,Y,[X|Xs]):-X\=Y.
first_code([X],1,X,[]).
first_code([X|Xs],N1,X,Rest):-first_code(Xs,N,X,Rest), N1 is N+1.
```


<br/>
<hr/>
<br/>
<h2 id="149"> 149. scompress.pl/</h2>

``` prolog
% P08 (**): Eliminate consecutive duplicates of list elements.

% compress(L1,L2) :- the list L2 is obtained from the list L1 by
%    compressing repeated occurrences of elements into a single copy
%    of the element.
%    (list,list) (+,?)

compress([],[]).
compress([X],[X]).
compress([X,X|Xs],Zs) :- compress([X|Xs],Zs).
compress([X|Xs],Zs) :- compress(Xs,Zs).

```


<br/>
<hr/>
<br/>
<h2 id="150"> 150. slice.pl/</h2>

``` prolog
slice([X|_],0,0,[X]).
slice([X|Xs],0,K,[X|Ys]):- K > 0, K1 is K - 1, slice(Xs,0,K1,Ys).
slice([_|Xs],I,K,Ys):- I > 0, I1 is I - 1, K1 is K - 1, slice(Xs,I1,K1,Ys).
```


<br/>
<hr/>
<br/>
<h2 id="151"> 151. splitn.pl/</h2>

``` prolog
split(Xs,0,[],Xs).
split([X|Xs],N,[X|Ys],L2):-N1 is N-1,split(Xs,N1,Ys,L2).
```


<br/>
<hr/>
<br/>
<h2 id="152"> 152. sudoku.pl/</h2>

``` prolog
:-ensure_loaded(top95).

mutually_exclusive_with(N,N1):-
	pos(N,R,_,_),
	pos(N1,R,_,_),
	\+ N=N1.

mutually_exclusive_with(N,N1):-
	pos(N,_,C,_),
	pos(N1,R,C,_),
	\+pos(N,R,_,_).

mutually_exclusive_with(N,N1):-
	pos(N,_,_,S),
	pos(N1,R,C,S),
	\+ pos(N,R,_,_),
	\+ pos(N,_,C,_).

square_id(S,_,R,C):-
	S_CID is C//3,
	S_RID is R//3,
	S is S_RID * 3 + S_CID.

pos(N,R,C,S):-
	nonvar(N),!,
	R is N//9,
	C is N mod 9,
	square_id(S,N,R,C).

pos(N,R,C,S):-
	nonvar(R),
	nonvar(C),!,
	N is R*9+C,
	square_id(S,N,R,C).

pos(N,R,C,S):-
	nonvar(R),!,
	between(0,8,C),
	N is R*9+C,
	square_id(S,N,R,C).

pos(N,R,C,S):-
	nonvar(C),!,
	between(0,8,R),
	N is R*9+C,
	square_id(S,N,R,C).

pos(N,R,C,S):-
	nonvar(S),!,
	R0 is S//3*3,
	C0 is (S mod 3)*3,
	between(0,2,R1),
	R is R0+R1,
	between(0,2,C1),
	C is C0+C1,
	N is R*9+C.
	
addkey([],_,[]).
addkey([X|Xs],K,[K-(X,K,R,C,S)|XKs]):-
	pos(K,R,C,S),
	K1 is K+1,
	addkey(Xs,K1,XKs).

puzzle_to_array([],[]).
puzzle_to_array([X|Xs],[[X]|AXs]):-
	nonvar(X),
	puzzle_to_array(Xs,AXs).
puzzle_to_array([X|Xs],[[1,2,3,4,5,6,7,8,9]|AXs]):-
	var(X),
	puzzle_to_array(Xs,AXs).
	
create_puzzle(S,P):-
	puzzle_to_array(S,AS),
	addkey(AS,0,ASK),
	list_to_assoc(ASK,P).

setNewId(N1,L1,_N2,L2,N,L):-
	L1<L2,!,
	N is N1,
	L is L1.
setNewId(_N1,_L1,N2,L2,N,L):-
	N is N2,
	L is L2.

select_node_with_fewest_possibilities(P,N,L,[N]):-
	get_assoc(N,P,(V,N,_,_,_)),
	length(V,L).
select_node_with_fewest_possibilities(P,NFewest,LFewest,[NodeId,N1|RestOfRemaingNodes]):-
	get_assoc(NodeId,P,(V,NodeId,_,_,_)),
	length(V,L),
	select_node_with_fewest_possibilities(P,NFewest1,LFewest1,[N1|RestOfRemaingNodes]),
	setNewId(NodeId,L,NFewest1,LFewest1,NFewest,LFewest).

%single_element_node(([_],N,_,_,_),N).
%single_element_nodes(P,L):-
%	setof(N,K^V^(gen_assoc(K,P,V),single_element_node(V,N)),L).


single_element_nodes(_P,[],[],[]):-!.
single_element_nodes(P,[N|Ns],[N|RestOfRIn],ROut):-
	get_assoc(N,P,([_],_,_,_,_)),!,
	single_element_nodes(P,Ns,RestOfRIn,ROut).
single_element_nodes(P,Ns,[N|RestOfRIn],[N|RestOfRout]):-
	single_element_nodes(P,Ns,RestOfRIn,RestOfRout).

trim_puzzle(P,P,[],[]):-!.
trim_puzzle(P,TrimP,RemainingIn,RemainingOut):-
	single_element_nodes(P,TrimNodes,RemainingIn,NewRemainingIn),
	\+ TrimNodes = [],!,
	set_nodes_in_list(P,TrimNodes,TrimP1),
	trim_puzzle(TrimP1,TrimP,NewRemainingIn,RemainingOut).
trim_puzzle(P,P,RemainingIn,RemainingIn).

writeSpace(0):-!.
writeSpace(N):-
	write(' '),
	N1 is N-1,
	writeSpace(N1).

writeList([]):-!.
writeList([V|Vs]):-
	write(V),
	writeList(Vs).

printElem((V,_,_,_,_),DW):-
	length(V,L),
	Bf is DW-L,
	FBF is Bf//2,
	BBF is Bf - FBF,
	writeSpace(FBF),
	writeList(V),
	writeSpace(BBF).
	

display_row(P,RowId,DisplayWidth):-
	pos(N,RowId,_ColId,_),
	get_assoc(N,P,V),
	printElem(V,DisplayWidth),
	fail.

display_row(_,_,_):-
	nl.
	
display_puzzle(P):-
	find_length_of_longest_val(P,Lmax),
	DisplayWidth is Lmax+2,
	display_puzzle(P,DisplayWidth).
display_puzzle(P,DisplayWidth):-
	between(0,8,RowId),
	display_row(P,RowId,DisplayWidth),
	fail.
display_puzzle(_,_):-
	nl.
	
find_length_of_longest_val(P,Lmax):-
	assoc_to_list(P,PL),
	find_max_length(PL,Lmax).

find_max_length([],0).
find_max_length([_-(X,_,_,_,_)|Xs],Lmax):-
	length(X,L),
	find_max_length(Xs,LsMax),
	Lmax is max(L,LsMax).

set_nodes_in_list(P,[],P).
set_nodes_in_list(P,[N|Ns],P2):-
	get_assoc(N,P,(Vs,_Id,_RId,_CId,_SId)),
	member(V,Vs),
	set(P,N,V,P1),
	set_nodes_in_list(P1,Ns,P2).

eliminate_from_mutually_exclusive_ents(P,[],_,P).
eliminate_from_mutually_exclusive_ents(P,[NEx|NExs],V,P2):-
	get_assoc(NEx,P,(OldVal,N,R,C,S)),
	delete(OldVal,V,NewVal),
	get_assoc(NEx,P,_,P1,(NewVal,N,R,C,S)),
	eliminate_from_mutually_exclusive_ents(P1,NExs,V,P2).

eliminate(P,N,V,P1):-
	setof(NEx,mutually_exclusive_with(N,NEx),NExs),
	eliminate_from_mutually_exclusive_ents(P,NExs,V,P1).	

set(P,N,V,P1):-
	eliminate(P,N,V,P_RCS),
	pos(N,R,C,S),
	get_assoc(N,P_RCS,_,P1,([V],N,R,C,S)).

solve_sudoku(P,P,[],[]).
solve_sudoku(P,Pout,RemainingIn,RemainingOut):-
	select_node_with_fewest_possibilities(P,N,_L,RemainingIn),
	get_assoc(N,P,(TV,_,_,_,_)),
	member(V,TV),
	set(P,N,V,TP),
	trim_puzzle(TP,TP1,RemainingIn,NewRemainingIn),
%	write(trim),nl,
%	display_puzzle(TP1),
	solve_sudoku(TP1,Pout,NewRemainingIn,RemainingOut).

solve(N):-
	top95(N,S),
	create_puzzle(S,P),
	assoc_to_keys(P,Remaining),
	solve_sudoku(P,Pout,Remaining,[]),
	write(solution),nl,
	display_puzzle(Pout).


%eliminate(P,N,P1):-
%	eliminate_imposibble(P).

%set(Val,N,Sol,NewSol):-
%	pos(N,R,C,S),
%	get_assoc(N,P,OldVal,P1,Val),
%	eliminate_val.
	
	
```


<br/>
<hr/>
<br/>
<h2 id="153"> 153. sudoku_clpfd.pl/</h2>

``` prolog
:- use_module(library(clpfd)).
:- ensure_loaded(top95).

square_id(S,_,R,C):-
	S_CID is C//3,
	S_RID is R//3,
	S is S_RID * 3 + S_CID.

pos(N,R,C,S):-
	nonvar(N),!,
	R is N//9,
	C is N mod 9,
	square_id(S,N,R,C).

pos(N,R,C,S):-
	nonvar(R),
	nonvar(C),!,
	N is R*9+C,
	square_id(S,N,R,C).

pos(N,R,C,S):-
	nonvar(R),!,
	between(0,8,C),
	N is R*9+C,
	square_id(S,N,R,C).

pos(N,R,C,S):-
	nonvar(C),!,
	between(0,8,R),
	N is R*9+C,
	square_id(S,N,R,C).

pos(N,R,C,S):-
	nonvar(S),!,
	R0 is S//3*3,
	C0 is (S mod 3)*3,
	between(0,2,R1),
	R is R0+R1,
	between(0,2,C1),
	C is C0+C1,
	N is R*9+C.

group_all_rows(P):-
	setof(R,between(1,9,R),Rs),
	group_rows(P,Rs).

group_all_cols(P):-
	setof(C,between(1,9,C),Cs),
	group_cols(P,Cs).

group_all_squares(P):-
	setof(S,between(1,9,S),Ss),
	group_squares(P,Ss).

group_rows(_,[]).
group_rows(P,[R|Rs]):-
	group_row(P,R),
	group_rows(P,Rs).

group_row(P,R):-
	setof(X,N^(pos(N,R,_,_),nth0(N,P,X)),Xs),
	all_different(Xs).

group_cols(_,[]).
group_cols(P,[C|Cs]):-
	group_col(P,C),
	group_cols(P,Cs).

group_col(P,C):-
	setof(X,N^(pos(N,_,C,_),nth0(N,P,X)),Xs),
	all_different(Xs).

group_squares(_,[]).
group_squares(P,[S|Ss]):-
	group_square(P,S),
	group_squares(P,Ss).

group_square(P,S):-
	setof(X,N^(pos(N,_,_,S),nth0(N,P,X)),Xs),
	all_different(Xs).

sudoku_solve(P):-
	P ins 1..9,

	group_all_rows(P),
	group_all_cols(P),
	group_all_squares(P),
	label(P).
	
```


<br/>
<hr/>
<br/>
<h2 id="154"> 154. sunil_flatten.pl/</h2>

``` prolog
sunil_flatten(Tree,FlatList):-sunil_flatten(Tree,FlatList,[]).
sunil_flatten([],FlatList,FlatList):-!.

```


<br/>
<hr/>
<br/>
<h2 id="155"> 155. sunil_reverse.pl/</h2>

``` prolog
% P05 (*): Reverse a list.

% my_reverse(L1,L2) :- L2 is the list obtained from L1 by reversing 
%    the order of the elements.
%    (list,list) (?,?)

% Note: reverse(+List1, -List2) is predefined

my_reverse(L1,L2) :- my_rev(L1,L2,[]).

my_rev([],L2,L2).
my_rev([X|Xs],L2,Acc) :- my_rev(Xs,L2,[X|Acc]).


```


<br/>
<hr/>
<br/>
<h2 id="156"> 156. symmetricbtree.pl/</h2>

``` prolog
symmetric(nil).
symmetric(t(_,L,R)):-mirror(L,R).

mirror(nil,nil).
mirror(t(_,L1,R1),t(_,L2,R2)):-
	mirror(L1,R2),
	mirror(L2,R1).
```


<br/>
<hr/>
<br/>
<h2 id="157"> 157. test_p87.pl/</h2>

``` prolog
:- ensure_loaded(p87).
bind_graph(graph([a,b,c,d,e,f,g,h],
		 [e(a,b),e(a,d),e(b,c),e(b,e),
		  e(c,e),e(d,e),e(d,f),e(d,g),
		  e(e,h),e(f,g),e(g,h)])).
bind_graph(graph([a,b,c,d,e,f,g,h],[e(a,b,5),e(a,d,3),e(b,c,2),e(b,e,4),
				    e(c,e,6),e(d,e,7),e(d,f,4),e(d,g,3),
				    e(e,h,5),e(f,g,4),e(g,h,1)])).
bind_graph(graph([b,c,d,f,g,h,k],[e(b,c),e(b,f),e(c,f),e(d,c),
				  e(f,k),e(f,g),e(g,h)])).
bind_graph(graph([k,m,p,q],[a(m,p,7),a(p,m,5),a(p,q,9)])).
bind_graph(graph([b,c,d,f,g,h,k],[e(b,c),e(b,f),e(c,f),e(f,k),e(g,h)])).
bind_graph(graph([r,s,t,u,v],[a(s,r),a(s,u),a(u,r),a(u,s),a(v,u)])).

test:-
	bind_graph(graph(Ns,Es)),
	write('-------------------------------------'),nl,
	write(graph(Ns,Es)),nl,
	member(N,Ns),
	depth_first_order(graph(Ns,Es),N,Seq),
	write(Seq),nl,fail.
```


<br/>
<hr/>
<br/>
<h2 id="158"> 158. top95.pl/</h2>

``` prolog
mt(N,S):-
	top95(N,S),sudoku(S).
top95(1,S):-
	S=[4,_,_,_,_,_,8,_,5,_,3,_,_,_,_,_,_,_,_,_,_,7,_,_,_,_,_,
	   _,2,_,_,_,_,_,6,_,_,_,_,_,8,_,4,_,_,_,_,_,_,1,_,_,_,_,
	   _,_,_,6,_,3,_,7,_,5,_,_,2,_,_,_,_,_,1,_,4,_,_,_,_,_,_].
top95(2,S):-
	S=[5,2,_,_,_,6,_,_,_,_,_,_,_,_,_,7,_,1,3,_,_,_,_,_,_,_,_,
	   _,_,_,4,_,_,8,_,_,6,_,_,_,_,_,_,5,_,_,_,_,_,_,_,_,_,_,
	   _,4,1,8,_,_,_,_,_,_,_,_,_,3,_,_,2,_,_,_,8,7,_,_,_,_,_].
top95(3,S):-
	S=[6,_,_,_,_,_,8,_,3,_,4,_,7,_,_,_,_,_,_,_,_,_,_,_,_,_,_,
	   _,_,_,5,_,4,_,7,_,3,_,_,2,_,_,_,_,_,1,_,6,_,_,_,_,_,_,
	   _,2,_,_,_,_,_,5,_,_,_,_,_,8,_,6,_,_,_,_,_,_,1,_,_,_,_].
top95(4,S):-
	S=[4,8,_,3,_,_,_,_,_,_,_,_,_,_,_,_,7,1,_,2,_,_,_,_,_,_,_,
	   7,_,5,_,_,_,_,6,_,_,_,_,2,_,_,8,_,_,_,_,_,_,_,_,_,_,_,
	   _,_,1,_,7,6,_,_,_,3,_,_,_,_,_,4,_,_,_,_,_,_,5,_,_,_,_].
top95(5,S):-
	S=[_,_,_,_,1,4,_,_,_,_,3,_,_,_,_,2,_,_,_,7,_,_,_,_,_,_,_,
	   _,_,_,9,_,_,_,3,_,6,_,1,_,_,_,_,_,_,_,_,_,_,_,_,_,8,_,
	   2,_,_,_,_,_,1,_,4,_,_,_,_,5,_,6,_,_,_,_,_,7,_,8,_,_,_].
top95(6,S):-
	S=[_,_,_,_,_,_,5,2,_,_,8,_,4,_,_,_,_,_,_,3,_,_,_,9,_,_,_,
	   5,_,1,_,_,_,6,_,_,2,_,_,7,_,_,_,_,_,_,_,_,3,_,_,_,_,_,
	   6,_,_,_,1,_,_,_,_,_,_,_,_,_,_,7,_,4,_,_,_,_,_,_,_,3,_].
top95(7,S):-
	S=[6,_,2,_,5,_,_,_,_,_,_,_,_,_,3,_,4,_,_,_,_,_,_,_,_,_,_,
	   4,3,_,_,_,8,_,_,_,_,1,_,_,_,_,2,_,_,_,_,_,_,_,_,7,_,_,
	   5,_,_,2,7,_,_,_,_,_,_,_,_,_,_,_,8,1,_,_,_,6,_,_,_,_,_].
top95(8,S):-
	S=[_,5,2,4,_,_,_,_,_,_,_,_,_,7,_,1,_,_,_,_,_,_,_,_,_,_,_,
	   _,_,_,8,_,2,_,_,_,3,_,_,_,_,_,6,_,_,_,9,_,5,_,_,_,_,_,
	   1,_,6,_,3,_,_,_,_,_,_,_,_,_,_,_,8,9,7,_,_,_,_,_,_,_,_].
top95(9,S):-
	S=[6,_,2,_,5,_,_,_,_,_,_,_,_,_,4,_,3,_,_,_,_,_,_,_,_,_,_,
	   4,3,_,_,_,8,_,_,_,_,1,_,_,_,_,2,_,_,_,_,_,_,_,_,7,_,_,
	   5,_,_,2,7,_,_,_,_,_,_,_,_,_,_,_,8,1,_,_,_,6,_,_,_,_,_].
top95(10,S):-
	S=[_,9,2,3,_,_,_,_,_,_,_,_,_,8,_,1,_,_,_,_,_,_,_,_,_,_,_,
	   1,_,7,_,4,_,_,_,_,_,_,_,_,_,_,_,6,5,8,_,_,_,_,_,_,_,_,
	   _,6,_,5,_,2,_,_,_,4,_,_,_,_,_,7,_,_,_,_,_,9,_,_,_,_,_].
top95(11,S):-
	S=[6,_,_,3,_,2,_,_,_,_,5,_,_,_,_,_,1,_,_,_,_,_,_,_,_,_,_,
	   7,_,2,6,_,_,_,_,_,_,_,_,_,_,_,_,5,4,3,_,_,_,_,_,_,_,_,
	   _,8,_,1,5,_,_,_,_,_,_,_,_,4,_,2,_,_,_,_,_,_,_,_,7,_,_].
top95(12,S):-
	S=[_,6,_,5,_,1,_,9,_,1,_,_,_,9,_,_,5,3,9,_,_,_,_,7,_,_,_,
	   _,4,_,8,_,_,_,7,_,_,_,_,_,_,_,5,_,8,_,8,1,7,_,5,_,3,_,
	   _,_,_,_,5,_,2,_,_,_,_,_,_,_,_,_,_,_,_,7,6,_,_,8,_,_,_].
top95(13,S):-
	S=[_,_,5,_,_,_,9,8,7,_,4,_,_,5,_,_,_,1,_,_,7,_,_,_,_,_,_,
	   2,_,_,_,4,8,_,_,_,_,9,_,1,_,_,_,_,_,6,_,_,2,_,_,_,_,_,
	   3,_,_,6,_,_,2,_,_,_,_,_,_,_,9,_,7,_,_,_,_,_,_,_,5,_,_].
top95(14,S):-
	S=[3,_,6,_,7,_,_,_,_,_,_,_,_,_,_,_,5,1,8,_,_,_,_,_,_,_,_,
	   _,1,_,4,_,5,_,_,_,7,_,_,_,_,_,6,_,_,_,_,_,2,_,_,_,_,_,
	   _,2,_,_,_,_,_,4,_,_,_,_,_,8,_,3,_,_,_,_,_,5,_,_,_,_,_].
top95(15,S):-
	S=[1,_,_,_,_,_,3,_,8,_,7,_,4,_,_,_,_,_,_,_,_,_,_,_,_,_,_,
	   2,_,3,_,1,_,_,_,_,_,_,_,_,_,_,_,9,5,8,_,_,_,_,_,_,_,_,
	   _,5,_,6,_,_,_,7,_,_,_,_,_,8,_,2,_,_,_,4,_,_,_,_,_,_,_].
top95(16,S):-
	S=[6,_,_,3,_,2,_,_,_,_,4,_,_,_,_,_,1,_,_,_,_,_,_,_,_,_,_,
	   7,_,2,6,_,_,_,_,_,_,_,_,_,_,_,_,5,4,3,_,_,_,_,_,_,_,_,
	   _,8,_,1,5,_,_,_,_,_,_,_,_,4,_,2,_,_,_,_,_,_,_,_,7,_,_].
top95(17,S):-
	S=[_,_,_,_,3,_,_,9,_,_,_,_,2,_,_,_,_,1,_,5,_,9,_,_,_,_,_,
	   _,_,_,_,_,_,_,_,_,1,_,2,_,8,_,4,_,6,_,8,_,5,_,_,_,2,_,
	   _,7,5,_,_,_,_,_,_,4,_,1,_,_,6,_,_,3,_,_,_,_,_,4,_,6,_].
top95(18,S):-
	S=[4,5,_,_,_,_,_,3,_,_,_,_,8,_,1,_,_,_,_,9,_,_,_,_,_,_,_,
	   _,_,_,_,5,_,_,9,_,2,_,_,7,_,_,_,_,_,8,_,_,_,_,_,_,_,_,
	   _,1,_,_,4,_,_,_,_,_,_,_,_,_,_,7,_,2,_,_,_,6,_,_,8,_,_].
top95(19,S):-
	S=[_,2,3,7,_,_,_,_,6,8,_,_,_,6,_,5,9,_,9,_,_,_,_,_,7,_,_,
	   _,_,_,_,4,_,9,7,_,3,_,7,_,9,6,_,_,2,_,_,_,_,_,_,_,_,_,
	   5,_,_,4,7,_,_,_,_,_,_,_,_,_,2,_,_,_,_,8,_,_,_,_,_,_,_].
top95(20,S):-
	S=[_,_,8,4,_,_,_,3,_,_,_,_,3,_,_,_,_,_,9,_,_,_,_,1,5,7,4,
	   7,9,_,_,_,8,_,_,_,_,_,_,_,_,7,_,_,5,1,4,_,_,_,_,_,2,_,
	   _,_,9,_,6,_,_,_,2,_,5,_,_,_,_,4,_,_,_,_,_,_,9,_,_,5,6].
top95(21,S):-
	S=[_,9,8,_,1,_,_,_,_,2,_,_,_,_,_,_,6,_,_,_,_,_,_,_,_,_,_,
	   _,_,_,3,_,2,_,5,_,_,8,4,_,_,_,_,_,_,_,_,_,6,_,_,_,_,_,
	   _,_,_,_,4,_,8,_,9,3,_,_,5,_,_,_,_,_,_,_,_,_,_,_,1,_,_].
top95(22,S):-
	S=[_,_,2,4,7,_,_,5,8,_,_,_,_,_,_,_,_,_,_,_,_,_,_,1,_,4,_,
	   _,_,_,_,2,_,_,_,9,5,2,8,_,9,_,4,_,_,_,_,9,_,_,_,1,_,_,
	   _,_,_,_,_,_,_,3,_,3,_,_,_,_,7,5,_,_,6,8,5,_,_,2,_,_,_].
top95(23,S):-
	S=[4,_,_,_,_,_,8,_,5,_,3,_,_,_,_,_,_,_,_,_,_,7,_,_,_,_,_,
	   _,2,_,_,_,_,_,6,_,_,_,_,_,5,_,4,_,_,_,_,_,_,1,_,_,_,_,
	   _,_,_,6,_,3,_,7,_,5,_,_,2,_,_,_,_,_,1,_,9,_,_,_,_,_,_].
top95(24,S):-
	S=[_,2,_,3,_,_,_,_,_,_,6,3,_,_,_,_,_,5,8,_,_,_,_,_,_,_,1,
	   5,_,_,_,_,9,_,3,_,_,_,_,7,_,_,_,_,_,_,_,_,1,_,_,_,_,8,
	   _,8,7,9,_,_,2,6,_,_,_,_,_,_,6,_,7,_,_,_,6,_,_,7,_,_,4].
top95(25,S):-
	S=[1,_,_,_,_,_,7,_,9,_,4,_,_,_,7,2,_,_,8,_,_,_,_,_,_,_,_,
	   _,7,_,_,1,_,_,6,_,3,_,_,_,_,_,_,_,5,_,6,_,_,4,_,_,2,_,
	   _,_,_,_,_,_,_,_,8,_,_,5,3,_,_,_,7,_,7,_,2,_,_,_,_,4,6].
top95(26,S):-
	S=[4,_,_,_,_,_,3,_,_,_,_,_,8,_,2,_,_,_,_,_,_,7,_,_,_,_,_,
	   _,_,_,1,_,_,_,8,7,3,4,_,_,_,_,_,_,_,6,_,_,_,_,_,_,_,_,
	   5,_,_,_,6,_,_,_,_,_,_,_,_,1,_,4,_,_,_,8,2,_,_,_,_,_,_].
top95(27,S):-
	S=[_,_,_,_,_,_,_,7,1,_,2,_,8,_,_,_,_,_,_,_,_,4,_,3,_,_,_,
	   7,_,_,_,6,_,_,5,_,_,_,_,2,_,_,3,_,_,9,_,_,_,_,_,_,_,_,
	   6,_,_,_,7,_,_,_,_,_,8,_,_,_,_,4,_,_,_,_,_,_,5,_,_,_,_].
top95(28,S):-
	S=[6,_,_,3,_,2,_,_,_,_,4,_,_,_,_,_,8,_,_,_,_,_,_,_,_,_,_,
	   7,_,2,6,_,_,_,_,_,_,_,_,_,_,_,_,5,4,3,_,_,_,_,_,_,_,_,
	   _,8,_,1,5,_,_,_,_,_,_,_,_,8,_,2,_,_,_,_,_,_,_,_,7,_,_].
top95(29,S):-
	S=[_,4,7,_,8,_,_,_,1,_,_,_,_,_,_,_,_,_,_,_,_,6,_,_,7,_,_,
	   6,_,_,_,_,3,5,7,_,_,_,_,_,_,5,_,_,_,_,1,_,_,6,_,_,_,_,
	   2,8,_,_,4,_,_,_,_,_,9,_,1,_,_,_,4,_,_,_,_,_,2,_,6,9,_].
top95(30,S):-
	S=[_,_,_,_,_,_,8,_,1,7,_,_,2,_,_,_,_,_,_,_,_,5,_,6,_,_,_,
	   _,_,_,7,_,_,_,5,_,_,1,_,_,_,_,3,_,_,_,8,_,_,_,_,_,_,_,
	   5,_,_,_,_,_,_,2,_,_,4,_,_,8,_,_,_,_,6,_,_,_,3,_,_,_,_].
top95(31,S):-
	S=[3,8,_,6,_,_,_,_,_,_,_,9,_,_,_,_,_,_,_,2,_,_,3,_,5,1,_,
	   _,_,_,_,_,5,_,_,_,_,3,_,_,1,_,_,6,_,_,_,_,4,_,_,_,_,_,
	   _,1,7,_,5,_,_,8,_,_,_,_,_,_,_,9,_,_,_,_,_,_,_,7,_,3,2].
top95(32,S):-
	S=[_,_,_,5,_,_,_,_,_,_,_,_,_,_,_,5,_,6,9,7,_,_,_,_,_,2,_,
	   _,_,4,8,_,2,_,_,_,2,5,_,1,_,_,_,3,_,_,8,_,_,3,_,_,_,_,
	   _,_,_,_,_,4,_,7,_,_,1,3,_,5,_,_,9,_,_,2,_,_,_,3,1,_,_].
top95(33,S):-
	S=[_,2,_,_,_,_,_,_,_,3,_,5,_,6,2,_,_,9,_,6,8,_,_,_,3,_,_,
	   _,5,_,_,_,_,_,_,_,_,_,_,6,4,_,8,_,2,_,_,4,7,_,_,9,_,_,
	   _,_,3,_,_,_,_,_,1,_,_,_,_,_,6,_,_,_,1,7,_,4,3,_,_,_,_].
top95(34,S):-
	S=[_,8,_,_,4,_,_,_,_,3,_,_,_,_,_,_,1,_,_,_,_,_,_,_,_,2,_,
	   _,_,5,_,_,_,4,_,6,9,_,_,1,_,_,8,_,_,2,_,_,_,_,_,_,_,_,
	   _,_,_,3,_,9,_,_,_,_,6,_,_,_,_,5,_,_,_,_,_,2,_,_,_,_,_].
top95(35,S):-
	S=[_,_,8,_,9,_,1,_,_,_,6,_,5,_,_,_,2,_,_,_,_,_,_,6,_,_,_,
	   _,3,_,1,_,7,_,5,_,_,_,_,_,_,_,_,_,9,_,_,4,_,_,_,3,_,_,
	   _,5,_,_,_,_,2,_,_,_,7,_,_,_,3,_,8,_,2,_,_,7,_,_,_,_,4].
top95(36,S):-
	S=[4,_,_,_,_,_,5,_,8,_,3,_,_,_,_,_,_,_,_,_,_,7,_,_,_,_,_,
	   _,2,_,_,_,_,_,6,_,_,_,_,_,5,_,8,_,_,_,_,_,_,1,_,_,_,_,
	   _,_,_,6,_,3,_,7,_,5,_,_,2,_,_,_,_,_,1,_,8,_,_,_,_,_,_].
top95(37,S):-
	S=[1,_,_,_,_,_,3,_,8,_,6,_,4,_,_,_,_,_,_,_,_,_,_,_,_,_,_,
	   2,_,3,_,1,_,_,_,_,_,_,_,_,_,_,_,9,5,8,_,_,_,_,_,_,_,_,
	   _,5,_,6,_,_,_,7,_,_,_,_,_,8,_,2,_,_,_,4,_,_,_,_,_,_,_].
top95(38,S):-
	S=[1,_,_,_,_,6,_,8,_,_,6,4,_,_,_,_,_,_,_,_,_,_,4,_,_,_,7,
	   _,_,_,_,9,_,6,_,_,_,7,_,4,_,_,5,_,_,5,_,_,_,7,_,1,_,_,
	   _,5,_,_,_,_,3,2,_,3,_,_,_,_,8,_,_,_,4,_,_,_,_,_,_,_,_].
top95(39,S):-
	S=[2,4,9,_,6,_,_,_,3,_,3,_,_,_,_,2,_,_,8,_,_,_,_,_,_,_,5,
	   _,_,_,_,_,6,_,_,_,_,_,_,2,_,_,_,_,_,_,1,_,_,4,_,8,2,_,
	   _,9,_,5,_,_,7,_,_,_,_,4,_,_,_,_,_,1,_,7,_,_,_,3,_,_,_].
top95(40,S):-
	S=[_,_,_,8,_,_,_,_,9,_,8,7,3,_,_,_,4,_,6,_,_,7,_,_,_,_,_,
	   _,_,8,5,_,_,9,7,_,_,_,_,_,_,_,_,_,_,_,4,3,_,_,7,5,_,_,
	   _,_,_,_,_,3,_,_,_,_,3,_,_,_,1,4,5,_,4,_,_,_,_,2,_,_,1].
top95(41,S):-
	S=[_,_,_,5,_,1,_,_,_,_,9,_,_,_,_,8,_,_,_,6,_,_,_,_,_,_,_,
	   4,_,1,_,_,_,_,_,_,_,_,_,_,7,_,_,9,_,_,_,_,_,_,_,_,3,_,
	   8,_,_,_,_,_,1,_,5,_,_,_,2,_,_,4,_,_,_,_,_,3,6,_,_,_,_].
top95(42,S):-
	S=[_,_,_,_,_,_,8,_,1,6,_,_,2,_,_,_,_,_,_,_,_,7,_,5,_,_,_,
	   _,_,_,6,_,_,_,2,_,_,1,_,_,_,_,3,_,_,_,8,_,_,_,_,_,_,_,
	   2,_,_,_,_,_,_,7,_,_,3,_,_,8,_,_,_,_,5,_,_,_,4,_,_,_,_].
top95(43,S):-
	S=[_,4,7,6,_,_,_,5,_,8,_,3,_,_,_,_,_,2,_,_,_,_,_,9,_,_,_,
	   _,_,_,8,_,5,_,_,6,_,_,_,1,_,_,_,_,_,6,_,2,4,_,_,_,_,_,
	   _,7,8,_,_,_,5,1,_,_,_,6,_,_,_,_,4,_,_,9,_,_,_,4,_,_,7].
top95(44,S):-
	S=[_,_,_,_,_,7,_,9,5,_,_,_,_,_,1,_,_,_,8,6,_,_,2,_,_,_,_,
	   _,2,_,_,7,3,_,_,8,5,_,_,_,_,_,_,6,_,_,_,3,_,_,4,9,_,_,
	   3,_,5,_,_,_,4,1,7,2,4,_,_,_,_,_,_,_,_,_,_,_,_,_,_,_,_].
top95(45,S):-
	S=[_,4,_,5,_,_,_,_,_,8,_,_,_,9,_,_,3,_,_,7,6,_,2,_,_,_,_,
	   _,1,4,6,_,_,_,_,_,_,_,_,_,_,9,_,_,7,_,_,_,_,_,3,6,_,_,
	   _,_,1,_,_,4,_,5,_,_,6,_,_,_,_,_,_,3,_,_,7,1,_,_,2,_,_].
top95(46,S):-
	S=[_,8,3,4,_,_,_,_,_,_,_,_,_,7,_,_,5,_,_,_,_,_,_,_,_,_,_,
	   _,4,_,1,_,8,_,_,_,_,_,_,_,_,_,_,2,7,_,_,_,3,_,_,_,_,_,
	   2,_,6,_,5,_,_,_,_,5,_,_,_,_,_,8,_,_,_,_,_,_,_,_,1,_,_].
top95(47,S):-
	S=[_,_,9,_,_,_,_,_,3,_,_,_,_,_,9,_,_,_,7,_,_,_,_,_,5,_,6,
	   _,_,6,5,_,_,4,_,_,_,_,_,3,_,_,_,_,_,_,2,8,_,_,_,_,_,_,
	   3,_,_,7,5,_,6,_,_,6,_,_,_,_,_,_,_,_,_,_,_,1,2,_,3,_,8].
top95(48,S):-
	S=[_,2,6,_,3,9,_,_,_,_,_,_,6,_,_,_,_,1,9,_,_,_,_,_,7,_,_,
	   _,_,_,_,_,4,_,_,9,_,5,_,_,_,_,2,_,_,_,_,8,5,_,_,_,_,_,
	   3,_,_,2,_,_,9,_,_,4,_,_,_,_,7,6,2,_,_,_,_,_,_,_,_,_,4].
top95(49,S):-
	S=[2,_,3,_,8,_,_,_,_,8,_,_,7,_,_,_,_,_,_,_,_,_,_,_,1,_,_,
	   _,6,_,5,_,7,_,_,_,4,_,_,_,_,_,_,3,_,_,_,_,1,_,_,_,_,_,
	   _,_,_,_,_,_,_,8,2,_,5,_,_,_,_,6,_,_,_,1,_,_,_,_,_,_,_].
top95(50,S):-
	S=[6,_,_,3,_,2,_,_,_,_,1,_,_,_,_,_,5,_,_,_,_,_,_,_,_,_,_,
	   7,_,2,6,_,_,_,_,_,_,_,_,_,_,_,_,8,4,3,_,_,_,_,_,_,_,_,
	   _,8,_,1,5,_,_,_,_,_,_,_,_,8,_,2,_,_,_,_,_,_,_,_,7,_,_].
top95(51,S):-
	S=[1,_,_,_,_,_,9,_,_,_,6,4,_,_,1,_,7,_,_,7,_,_,4,_,_,_,_,
	   _,_,_,3,_,_,_,_,_,3,_,8,9,_,_,5,_,_,_,_,7,_,_,_,_,2,_,
	   _,_,_,_,6,_,7,_,9,_,_,_,_,_,4,_,1,_,_,_,_,1,2,9,_,3,_].
top95(52,S):-
	S=[_,_,_,_,_,_,_,_,_,9,_,_,_,_,_,_,8,4,_,6,2,3,_,_,_,5,_,
	   _,_,_,6,_,_,_,4,5,3,_,_,_,1,_,_,_,6,_,_,_,9,_,_,_,7,_,
	   _,_,_,1,_,_,_,_,_,4,_,5,_,_,2,_,_,_,_,3,_,8,_,_,_,_,9].
top95(53,S):-
	S=[_,2,_,_,_,_,5,9,3,8,_,_,5,_,_,4,6,_,9,4,_,_,6,_,_,_,8,
	   _,_,2,_,3,_,_,_,_,_,6,_,_,8,_,7,3,_,7,_,_,2,_,_,_,_,_,
	   _,_,_,_,4,_,3,8,_,_,7,_,_,_,_,6,_,_,_,_,_,_,_,_,_,_,5].
top95(54,S):-
	S=[9,_,4,_,_,5,_,_,_,2,5,_,6,_,_,1,_,_,3,1,_,_,_,_,_,_,8,
	   _,7,_,_,_,9,_,_,_,4,_,_,2,6,_,_,_,_,_,_,1,4,7,_,_,_,_,
	   7,_,_,_,_,_,_,_,2,_,_,_,3,_,_,8,_,6,_,4,_,_,_,_,_,9,_].
top95(55,S):-
	S=[_,_,_,5,2,_,_,_,_,_,9,_,_,_,3,_,_,4,_,_,_,_,_,_,7,_,_,
	   _,1,_,_,_,_,_,4,_,_,8,_,_,4,5,3,_,_,6,_,_,_,1,_,_,_,8,
	   7,_,2,_,_,_,_,_,_,_,_,8,_,_,_,_,3,2,_,4,_,_,8,_,_,1,_].
top95(56,S):-
	S=[5,3,_,_,2,_,9,_,_,_,2,4,_,3,_,_,5,_,_,_,9,_,_,_,_,_,_,
	   _,_,_,_,1,_,8,2,7,_,_,_,7,_,_,_,_,_,_,_,_,_,9,8,1,_,_,
	   _,_,_,_,_,_,_,_,_,_,_,6,4,_,_,_,_,9,1,_,2,_,5,_,4,3,_].
top95(57,S):-
	S=[1,_,_,_,_,7,8,6,_,_,_,7,_,_,8,_,1,_,8,_,_,2,_,_,_,_,9,
	   _,_,_,_,_,_,_,_,2,4,_,_,_,1,_,_,_,_,_,_,9,_,_,5,_,_,_,
	   6,_,8,_,_,_,_,_,_,_,_,_,_,5,_,9,_,_,_,_,_,_,_,9,3,_,4].
top95(58,S):-
	S=[_,_,_,_,5,_,_,_,1,1,_,_,_,_,_,_,7,_,_,6,_,_,_,_,_,8,_,
	   _,_,_,_,_,4,_,_,_,_,_,9,_,1,_,3,_,_,_,_,_,5,9,6,_,2,_,
	   _,8,_,_,6,2,_,_,7,_,_,7,_,_,_,_,_,_,3,_,5,_,7,_,2,_,_].
top95(59,S):-
	S=[_,4,7,_,2,_,_,_,_,8,_,_,_,_,1,_,_,_,_,3,_,_,_,_,9,_,2,
	   _,_,_,_,_,5,_,_,_,6,_,_,8,1,_,_,5,_,_,_,_,_,4,_,_,_,_,
	   _,7,_,_,_,_,3,_,4,_,_,_,9,_,_,_,1,_,4,_,_,2,7,_,8,_,_].
top95(60,S):-
	S=[_,_,_,_,_,_,9,4,_,_,_,_,_,9,_,_,_,5,3,_,_,_,_,5,_,7,_,
	   _,8,_,4,_,_,1,_,_,4,6,3,_,_,_,_,_,_,_,_,_,_,_,7,_,8,_,
	   8,_,_,7,_,_,_,_,_,7,_,_,_,_,_,_,2,8,_,5,_,2,6,_,_,_,_].
top95(61,S):-
	S=[_,2,_,_,_,_,_,_,6,_,_,_,_,4,1,_,_,_,_,_,7,8,_,_,_,_,1,
	   _,_,_,_,_,_,7,_,_,_,_,3,7,_,_,_,_,_,6,_,_,4,1,2,_,_,_,
	   _,1,_,_,7,4,_,_,5,_,_,8,_,5,_,_,7,_,_,_,_,_,_,3,9,_,_].
top95(62,S):-
	S=[1,_,_,_,_,_,3,_,8,_,6,_,4,_,_,_,_,_,_,_,_,_,_,_,_,_,_,
	   2,_,3,_,1,_,_,_,_,_,_,_,_,_,_,_,7,5,8,_,_,_,_,_,_,_,_,
	   _,7,_,5,_,_,_,6,_,_,_,_,_,8,_,2,_,_,_,4,_,_,_,_,_,_,_].
top95(63,S):-
	S=[2,_,_,_,_,1,_,9,_,_,1,_,_,3,_,7,_,_,9,_,_,8,_,_,_,2,_,
	   _,_,_,_,_,_,8,5,_,_,6,_,4,_,_,_,_,_,_,_,_,_,7,_,_,_,3,
	   _,2,_,3,_,_,_,6,_,_,_,_,5,_,_,_,_,_,1,_,9,_,_,_,2,_,5].
top95(64,S):-
	S=[_,_,7,_,_,8,_,_,_,_,_,6,_,2,_,3,_,_,_,3,_,_,_,_,_,_,9,
	   _,1,_,_,5,_,_,6,_,_,_,_,_,1,_,_,_,_,_,7,_,9,_,_,_,_,2,
	   _,_,_,_,_,_,_,_,4,_,8,3,_,_,4,_,_,_,2,6,_,_,_,_,5,1,_].
top95(65,S):-
	S=[_,_,_,3,6,_,_,_,_,8,5,_,_,_,_,_,_,_,9,_,4,_,_,8,_,_,_,
	   _,_,_,_,_,6,8,_,_,_,_,_,_,_,_,_,1,7,_,_,9,_,_,4,5,_,_,
	   _,1,_,5,_,_,_,6,_,4,_,_,_,_,9,_,_,2,_,_,_,_,_,3,_,_,_].
top95(66,S):-
	S=[3,4,_,6,_,_,_,_,_,_,_,7,_,_,_,_,_,_,_,2,_,_,8,_,5,7,_,
	   _,_,_,_,_,5,_,_,_,_,7,_,_,1,_,_,2,_,_,_,_,4,_,_,_,_,_,
	   _,3,6,_,2,_,_,1,_,_,_,_,_,_,_,9,_,_,_,_,_,_,_,7,_,8,2].
top95(67,S):-
	S=[_,_,_,_,_,_,4,_,1,8,_,_,2,_,_,_,_,_,_,_,_,6,_,7,_,_,_,
	   _,_,_,8,_,_,_,6,_,_,4,_,_,_,_,3,_,_,_,1,_,_,_,_,_,_,_,
	   6,_,_,_,_,_,_,2,_,_,5,_,_,1,_,_,_,_,7,_,_,_,3,_,_,_,_].
top95(68,S):-
	S=[_,4,_,_,5,_,_,6,7,_,_,_,1,_,_,_,4,_,_,_,_,2,_,_,_,_,_,
	   1,_,_,8,_,_,3,_,_,_,_,_,_,_,_,2,_,_,_,6,_,_,_,_,_,_,_,
	   _,_,_,_,4,_,_,5,_,3,_,_,_,_,_,8,_,_,2,_,_,_,_,_,_,_,_].
top95(69,S):-
	S=[_,_,_,_,_,_,_,4,_,_,_,2,_,_,4,_,_,1,_,7,_,_,5,_,_,9,_,
	   _,_,3,_,_,7,_,_,_,_,4,_,_,6,_,_,_,_,6,_,_,1,_,_,8,_,_,
	   _,2,_,_,_,_,1,_,_,8,5,_,9,_,_,_,6,_,_,_,_,_,8,_,_,_,3].
top95(70,S):-
	S=[8,_,_,7,_,_,_,_,4,_,5,_,_,_,_,6,_,_,_,_,_,_,_,_,_,_,_,
	   _,3,_,9,7,_,_,_,8,_,_,_,_,4,3,_,_,5,_,_,_,_,2,_,9,_,_,
	   _,_,6,_,_,_,_,_,_,2,_,_,_,6,_,_,_,7,_,7,1,_,_,8,3,_,2].
top95(71,S):-
	S=[_,8,_,_,_,4,_,5,_,_,_,_,7,_,_,3,_,_,_,_,_,_,_,_,_,_,_,
	   _,1,_,_,8,5,_,_,_,6,_,_,_,_,_,2,_,_,_,_,_,_,4,_,_,_,_,
	   3,_,2,6,_,_,_,_,_,_,_,_,_,_,_,_,4,1,7,_,_,_,_,_,_,_,_].
top95(72,S):-
	S=[_,_,_,_,7,_,_,8,_,_,_,6,_,_,_,5,_,_,_,2,_,_,_,3,_,6,1,
	   _,1,_,_,_,7,_,_,2,_,_,8,_,_,5,3,4,_,2,_,_,9,_,_,_,_,_,
	   _,_,2,_,_,_,_,_,_,5,8,_,_,_,6,_,3,_,4,_,_,_,1,_,_,_,_].
top95(73,S):-
	S=[_,_,_,_,_,_,8,_,1,6,_,_,2,_,_,_,_,_,_,_,_,7,_,5,_,_,_,
	   _,_,_,6,_,_,_,2,_,_,1,_,_,_,_,3,_,_,_,8,_,_,_,_,_,_,_,
	   2,_,_,_,_,_,_,7,_,_,4,_,_,8,_,_,_,_,5,_,_,_,3,_,_,_,_].
top95(74,S):-
	S=[_,2,_,_,_,_,_,_,_,_,_,_,6,_,_,_,_,3,_,7,4,_,8,_,_,_,_,
	   _,_,_,_,_,3,_,_,2,_,8,_,_,4,_,_,1,_,6,_,_,5,_,_,_,_,_,
	   _,_,_,_,1,_,7,8,_,5,_,_,_,_,9,_,_,_,_,_,_,_,_,_,_,4,_].
top95(75,S):-
	S=[_,5,2,_,_,6,8,_,_,_,_,_,_,_,7,_,2,_,_,_,_,_,_,_,6,_,_,
	   _,_,4,8,_,_,9,_,_,2,_,_,4,1,_,_,_,_,_,_,1,_,_,_,_,_,8,
	   _,_,6,1,_,_,3,8,_,_,_,_,_,9,_,_,_,6,3,_,_,6,_,_,1,_,9].
top95(76,S):-
	S=[_,_,_,_,1,_,7,8,_,5,_,_,_,_,9,_,_,_,_,_,_,_,_,_,_,4,_,
	   _,2,_,_,_,_,_,_,_,_,_,_,6,_,_,_,_,3,_,7,4,_,8,_,_,_,_,
	   _,_,_,_,_,3,_,_,2,_,8,_,_,4,_,_,1,_,6,_,_,5,_,_,_,_,_].
top95(77,S):-
	S=[1,_,_,_,_,_,_,_,3,_,6,_,3,_,_,7,_,_,_,7,_,_,_,5,_,_,1,
	   2,1,_,7,_,_,_,9,_,_,_,7,_,_,_,_,_,_,_,_,8,_,1,_,_,2,_,
	   _,_,_,8,_,6,4,_,_,_,_,9,_,2,_,_,6,_,_,_,_,4,_,_,_,_,_].
top95(78,S):-
	S=[4,_,_,_,7,_,1,_,_,_,_,1,9,_,4,6,_,5,_,_,_,_,_,1,_,_,_,
	   _,_,_,7,_,_,_,_,2,_,_,2,_,3,_,_,_,_,8,4,7,_,_,6,_,_,_,
	   _,1,4,_,_,_,8,_,6,_,2,_,_,_,_,3,_,_,6,_,_,_,9,_,_,_,_].
top95(79,S):-
	S=[_,_,_,_,_,_,8,_,1,7,_,_,2,_,_,_,_,_,_,_,_,5,_,6,_,_,_,
	   _,_,_,7,_,_,_,5,_,_,1,_,_,_,_,3,_,_,_,8,_,_,_,_,_,_,_,
	   5,_,_,_,_,_,_,2,_,_,3,_,_,8,_,_,_,_,6,_,_,_,4,_,_,_,_].
top95(80,S):-
	S=[9,6,3,_,_,_,_,_,_,1,_,_,_,_,8,_,_,_,_,_,_,2,_,5,_,_,_,
	   _,4,_,8,_,_,_,_,_,_,1,_,_,_,_,7,_,_,_,_,_,_,3,_,_,2,5,
	   7,_,_,_,_,_,_,3,_,_,_,9,_,2,_,4,_,7,_,_,_,_,_,_,9,_,_].
top95(81,S):-
	S=[1,5,_,3,_,_,_,_,_,_,7,_,_,4,_,2,_,_,_,_,4,_,7,2,_,_,_,
	   _,_,8,_,_,_,_,_,_,_,_,_,9,_,_,1,_,8,_,1,_,_,8,_,7,9,_,
	   _,_,_,_,_,3,8,_,_,_,_,_,_,_,_,_,_,_,6,_,_,_,_,7,4,2,3].
top95(82,S):-
	S=[_,_,_,_,_,_,_,_,_,_,5,7,2,4,_,_,_,9,8,_,_,_,_,9,4,7,_,
	   _,_,9,_,_,3,_,_,_,5,_,_,9,_,_,1,2,_,_,_,3,_,1,_,9,_,_,
	   _,6,_,_,_,_,2,5,_,_,_,_,5,6,_,_,_,_,_,7,_,_,_,_,_,_,6].
top95(83,S):-
	S=[_,_,_,_,7,5,_,_,_,_,1,_,_,2,_,_,_,_,_,4,_,_,_,3,_,_,_,
	   5,_,_,_,_,_,3,_,2,_,_,_,8,_,_,_,1,_,_,_,_,_,_,_,6,_,_,
	   _,_,_,1,_,_,4,8,_,2,_,_,_,_,_,_,_,_,7,_,_,_,_,_,_,_,_].
top95(84,S):-
	S=[6,_,_,_,_,_,7,_,3,_,4,_,8,_,_,_,_,_,_,_,_,_,_,_,_,_,_,
	   _,_,_,5,_,4,_,8,_,7,_,_,2,_,_,_,_,_,1,_,3,_,_,_,_,_,_,
	   _,2,_,_,_,_,_,5,_,_,_,_,_,7,_,9,_,_,_,_,_,_,1,_,_,_,_].
top95(85,S):-
	S=[_,_,_,_,6,_,_,_,4,_,_,6,_,3,_,_,_,_,1,_,_,4,_,_,5,_,7,
	   7,_,_,_,_,_,8,_,5,_,_,_,8,_,_,_,_,_,6,_,8,_,_,_,_,9,_,
	   _,_,2,_,9,_,_,_,_,4,_,_,_,_,3,2,_,_,_,_,9,7,_,_,1,_,_].
top95(86,S):-
	S=[_,3,2,_,_,_,_,_,5,8,_,_,3,_,_,_,_,_,9,_,4,2,8,_,_,_,1,
	   _,_,_,4,_,_,_,3,9,_,_,_,6,_,_,_,5,_,_,_,_,_,1,_,_,_,_,
	   _,2,_,_,_,6,7,_,8,_,_,_,_,_,4,_,_,_,_,9,5,_,_,_,_,6,_].
top95(87,S):-
	S=[_,_,_,5,_,3,_,_,_,_,_,_,_,6,_,7,_,_,5,_,8,_,_,_,_,1,6,
	   3,6,_,_,2,_,_,_,_,_,_,_,4,_,1,_,_,_,_,_,_,_,3,_,_,_,5,
	   6,7,_,_,_,_,2,_,8,_,_,4,_,7,_,_,_,_,_,_,_,2,_,_,5,_,_].
top95(88,S):-
	S=[_,5,_,3,_,7,_,4,_,1,_,_,_,_,_,_,_,_,_,3,_,_,_,_,_,_,_,
	   5,_,8,_,3,_,6,1,_,_,_,_,8,_,_,5,_,9,_,6,_,_,1,_,_,_,_,
	   _,_,_,_,4,_,_,_,6,_,_,_,6,9,2,7,_,_,_,_,2,_,_,_,9,_,_].
top95(89,S):-
	S=[_,_,5,_,_,8,_,_,1,8,_,_,_,_,_,_,9,_,_,_,_,_,_,_,7,8,_,
	   _,_,_,4,_,_,_,_,_,6,4,_,_,_,_,9,_,_,_,_,_,_,5,3,_,_,2,
	   _,6,_,_,_,_,_,_,_,_,_,1,3,8,_,_,5,_,_,_,_,9,_,7,1,4,_].
top95(90,S):-
	S=[_,_,_,_,_,_,_,_,_,_,7,2,_,6,_,1,_,_,_,_,5,1,_,_,_,8,2,
	   _,8,_,_,_,1,3,_,_,4,_,_,_,_,_,_,_,_,_,3,7,_,9,_,_,1,_,
	   _,_,_,_,2,3,8,_,_,5,_,4,_,_,9,_,_,_,_,_,_,_,_,_,7,9,_].
top95(91,S):-
	S=[_,_,_,6,5,8,_,_,_,_,_,4,_,_,_,_,_,_,1,2,_,_,_,_,_,_,_,
	   _,_,_,_,_,9,6,_,7,_,_,_,3,_,_,5,_,_,_,_,2,_,8,_,_,_,3,
	   _,_,1,9,_,_,8,_,_,3,_,6,_,_,_,_,_,4,_,_,_,_,4,7,3,_,_].
top95(92,S):-
	S=[_,2,_,3,_,_,_,_,_,_,_,6,_,_,8,_,9,_,8,3,_,5,_,_,_,_,_,
	   _,_,_,2,_,_,_,8,_,7,_,9,_,_,5,_,_,_,_,_,_,_,_,6,_,_,4,
	   _,_,_,_,_,_,_,1,_,_,_,1,_,_,_,4,_,2,2,_,_,7,_,_,8,_,9].
top95(93,S):-
	S=[_,5,_,_,9,_,_,_,_,1,_,_,_,_,_,6,_,_,_,_,_,3,_,8,_,_,_,
	   _,_,8,_,4,_,_,_,9,5,1,4,_,_,_,_,_,_,_,3,_,_,_,_,2,_,_,
	   _,_,_,_,_,_,_,_,4,_,8,_,_,_,6,_,_,7,7,_,_,1,5,_,_,6,_].
top95(94,S):-
	S=[_,_,_,_,_,2,_,_,_,_,_,_,_,7,_,_,_,1,7,_,_,3,_,_,_,9,_,
	   8,_,_,7,_,_,_,_,_,_,2,_,8,9,_,6,_,_,_,1,3,_,_,6,_,_,_,
	   _,9,_,_,5,_,8,2,4,_,_,_,_,_,8,9,1,_,_,_,_,_,_,_,_,_,_].
top95(95,S):-
	S=[3,_,_,_,8,_,_,_,_,_,_,_,7,_,_,_,_,5,1,_,_,_,_,_,_,_,_,
	   _,_,_,_,_,_,3,6,_,_,_,2,_,_,4,_,_,_,_,7,_,_,_,_,_,_,_,
	   _,_,_,_,6,_,1,3,_,_,4,5,2,_,_,_,_,_,_,_,_,_,_,_,8,_,_].
top95(96,S):-
	S=[, ,_,_,8,_,_,_,_,_,_,_,7,_,_,_,_,5,1,_,_,_,_,_,_,_,_,
	   _,_,_,_,_,_,3,6,_,_,_,2,_,_,4,_,_,_,_,7,_,_,_,_,_,_,_,
	   _,_,_,_,6,_,1,3,_,_,4,5,2,_,_,_,_,_,_,_,_,_,_,_,8,_,_].

```


<br/>
<hr/>
<br/>
<h2 id="159"> 159. treedot.pl/</h2>

``` prolog
treedot(".",nil):-!.
treedot([X],t(X,nil,nil)).
treedot([X|Xs],t(X,Left,Right)):-
	append([LeftString,RightString],Xs),
	write('left : '),write(LeftString),nl,
	write('right: '),write(RightString),nl,
	treedot(LeftString,Left),
	treedot(RightString,Right).
```


<br/>
<hr/>
<br/>
<h2 id="160"> 160. tree_order.pl/</h2>

``` prolog
preorder(nil,[]).
preorder(t(A,B,C),[A|Xs]):-
	preorder(B,Left),
	preorder(C,Right),
	append([Left,Right],Xs).

preorder_tree(nil,[]).
preorder_tree(t(X,L,R),[X|Xs]):-
	append([LeftString,RightString],Xs),
	preorder_tree(L,LeftString),
	preorder_tree(R,RightString).

inorder(nil,[]).
inorder(t(A,B,C),Result):-
	inorder(B,Left),
	inorder(C,Right),
	append([Left,[A],Right],Result).

pre_in_order(T,InOrder,PreOrder):-
	preorder_tree(T,PreOrder),
	inorder(T,InOrder).



%add_val(X,[X|Xs]-Xs).


%preorder_diff(nil,L-L).
%preorder_diff(t(A,B,C),L1-L4):-
%	add_val(A,L1-L2),
%	preorder_diff(B,L2-L3),
%	preorder_diff(C,L3-L4).
```


<br/>
<hr/>
<br/>
<h2 id="161"> 161. tree_string.pl/</h2>

``` prolog
bind_char_list("abc(,des)").
bind_char_list("abd(,)").
bind_char_list("a").
bind_char_list("abc").
bind_char_list("abc(ded,feg)").
bind_char_list("a(b,c)").
bind_char_list("a(b,c),d(,e)").
bind_char_list("a(b(c,d),e(f,g))").
bind_char_list("ab(,cd(,de))").
bind_char_list("a(b(d,e),c(,f(g,)))").
bind_char_list("a ( b ( d , e ),c(,f(gfs,)))").
bind_char_list("a ( b ( d, e),c(,f(g f,)))").
bind_char_list("a ( b ( d;s, e;),c(,f(gf,)))").
bind_char_list("a(,b,s)").
bind_char_list("a(c,d,e,g)").
bind_char_list(":a(d,c)").
bind_char_list("a(d,,c)").
bind_string(S):-bind_char_list(L),string_to_list(S,L).
bind_tree(t(a,t(b,nil,nil),t(cd,nil,nil))).
bind_tree(t(abc,t(ade,nil,nil),nil)).

simplify_trivial_node(t(nil,nil,nil),nil):-!.
simplify_trivial_node(X,X).

string_to_node(t(Atom,nil,nil),NodeString):-
	my_string_to_atom(NodeString,Atom).

string_to_node(t(Atom,LeftNode,RightNode),NodeString):-
	append([AtomString,"(",NodeString1,",",NodeString2,")"],NodeString),
	string_to_node(LeftNode1,NodeString1),
	simplify_trivial_node(LeftNode1,LeftNode),
	string_to_node(RightNode1,NodeString2),
	simplify_trivial_node(RightNode1,RightNode),
	my_string_to_atom(AtomString,Atom).

tree_list(Tree,ListOfChars):-
	var(Tree),
	nonvar(ListOfChars),
	string_to_node(ListOfChars,Tree).

tree_list(Tree,String):-
	nonvar(Tree),
	var(ListOfChars),
	node_to_string(Tree,ListOfChars),
	string_to_list(String,ListOfChars).
node_to_string(nil,[]).
node_to_string(t(Atom,LeftNode,RightNode),NodeString):-
	atom_chars(Atom,AtomString),
	node_to_string(LeftNode,LeftNodeString),
	node_to_string(RightNode,RightNodeString),
	append([AtomString,['('],LeftNodeString,[','],RightNodeString,[')']],
	       NodeString).

remove_space(S1,S2):-
	remove_leading_space(S1,S3),
	remove_trailing_space(S3,S2).

remove_leading_space([],[]):-!.
remove_leading_space([X|Xs],Xs1):-
	char_type(X,white),
	remove_leading_space(Xs,Xs1),!.
remove_leading_space(S,S).

remove_trailing_space(S1,S2):-
	reverse(S1,S3),
	remove_leading_space(S3,S4),
	reverse(S4,S2).

is_alphabet(X):-
	char_type(X,alpha).

my_string_to_atom([],nil):-!.
my_string_to_atom(AtomString,Atom):-
	remove_space(AtomString,AtomStringWithOutSpace),
	maplist(is_alphabet,AtomStringWithOutSpace),
	atom_chars(Atom,AtomStringWithOutSpace).



```


<br/>
<hr/>
<br/>
<h2 id="162"> 162. unify_alternate_elems.pl/</h2>

``` prolog
unify([C,_|Xs],[C,_|Ys]):-
	unify(Xs,Ys).
unify([],[]).
unify([C],[C]).


myl(N,L):-length(L,N).

unify_row([],[],_).
unify_row([X|Xs],[Col|Cols],RowId):-
	nth0(RowId,Col,X),
	unify_row(Xs,Cols,RowId).

unify_rows([],_,_).
unify_rows([Row|Rows],Cols,RowId):-
	unify_row(Row,Cols,RowId),
	RowId1 is RowId+1,
	unify_rows(Rows,Cols,RowId1).


make_rectangle(Rows,Cols,NRows,NCols):-
	length(Rows,NRows),
	length(Cols,NCols),
	maplist(myl(NCols),Rows),
	maplist(myl(NRows),Cols),
	unify_rows(Rows,Cols).

```


<br/>
<hr/>
<br/>
<h2 id="163"> 163. unwind.pl/</h2>

``` prolog
bind(true).
bind(fail).
unwind(_):-bind(A),write(A),A,!,write(' here '),\+ A.
```


<br/>
<hr/>
<br/>
<h2 id="164"> 164. von_koch.pl/</h2>

``` prolog
graph_bind(graph([a,b],[e(a,b)])).
graph_bind(graph([a,b,c],[e(a,b),e(a,c)])).
graph_bind(graph([a,b,c,d,e,f,g],
		 [e(a,b),e(a,d),e(a,g),
		  e(b,c),e(b,e),e(e,f)])).
graph_bind(graph([a,b,c,d,e,f,g,h,i,k,m,n,p,q],
		 [e(a,b),e(a,c),e(a,g),e(a,h),e(a,i),
		  e(c,d),e(c,e),e(c,f),
		  e(d,k),
		  e(e,q),
		  e(m,q),
		  e(n,q),e(n,p)])).

check_edge(NodeIdPairs,e(A,B),e(A,B)-N):-
	member(A-Na,NodeIdPairs),
	member(B-Nb,NodeIdPairs),
	N is abs(Na-Nb).

all_different(L):-
	length(L,N),
	all_different(L,N).

all_different(_,0).
all_different(L,N):-
	member(N,L),
	N1 is N-1,
	all_different(L,N1).



	
solve(P,PEs):-
	graph_bind(graph(Ns,Es)),
	length(Ns,N),
	setof(I,between(1,N,I),Is),
	permutation(Is,PermIs),
	pairs_keys_values(P,Ns,PermIs),
	Pred =.. [check_edge , P],
	maplist(Pred,Es,PEs),
	pairs_keys_values(PEs,_,Values),
	all_different(Values).

```


<br/>
<hr/>
<br/>
<h2 id="165"> 165. while.pl/</h2>

``` prolog
while_i_less_than_3(I):-
	I<3,!,
	write([i,I]),nl,
	I1 is I+1,
	while_i_less_than_3(I1).
while_i_less_than_3(_):-
	write('while loop done'),nl.
	       
```



[1]:https://web.ti.bfh.ch/~hew1/informatik3/prolog/p-99/ 
