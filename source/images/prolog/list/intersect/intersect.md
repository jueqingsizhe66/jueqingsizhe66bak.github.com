
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
