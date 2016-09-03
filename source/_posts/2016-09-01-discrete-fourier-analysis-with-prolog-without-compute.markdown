---
layout: post
title: "discrete fourier analysis with prolog(without compute)"
date: 2016-09-01 05:19:15 +0800
comments: true
categories: prolog
---

傅里叶定律的prolog解析。仅仅抛砖，还得进一步引玉。

![outcome][89]

<!--more-->
## a good tool to create image sequences in the matlab for the markdown

matlab code for generating the repetitive part of your blog

``` matlab
countLength=87; %assign the picture number.
prefix='/images/prolog';
topic='dft';
fid =fopen('a.txt','w');

for i=1:countLength
    fprintf(fid,'%s\n','<hr/>')
    fprintf(fid,'%s\n','<br/>');
    fprintf(fid,'%s\n','<br/>');
    fprintf(fid,'<h3 id="%d"><font color="red">Fig%d:</font></h3>\n',i);
    fprintf(fid,'![%s%d][%d]\n',topic,i,i);
    
end

for i=1:countLength
    fprintf(fid,'[%d]:%s%d.png\n',i,strcat(prefix,'/',topic,'/',topic),i);
end
fclose(fid);
```

结果如下:

![codeResult][88]

<hr/>

## 进入正题，分析离散傅里叶的执行过程

when we use the dft, such as 8th dft ,we should input eight coeffiency ,then you should 
calculate the [8th root of unity][91]

just like the figure below:

![dftcoef][90]

你应该仔细阅读 [clauses and effect][92].

下面是实现的dft表达式代码:

<hr/>
``` prolog
%% p([i0,i1,...,in], w^k)


alternate([], [], []).
alternate([A,B|T], [A|T1], [B|T2]) :- 
    alternate(T, T1, T2).

%% the recursion terminates when only one index
%% is encountered p([I],w^K) = a(I), so you can get the I

% [0,1,2,3,4,5,6,7]
generateList(N,N,[]).
generateList(X,N,[X|L]):-
    N1 is X + 1,
    generateList(N1, N , L).

% [7,6,5,4,3,2,1,0]
generateList1(0,0,[]).
generateList1(0,N,[N1|L]):-
    N1 is N - 1,
    generateList1(0, N1 , L).


%% improve generateList
%generateList1(0,N,Result) :-
%    generateListdiff(0, N, Result).
%generateListdiff(0, 0, Hole).
%generateListdiff(0, N, Hole):-
%    N1 is N - 1,
%    generateListdiff(0, N1 , [N1|Hole]).


%% dft(p(8,3,W),X).
dft(p(N, K, W),X):-
    generateList(0, N, L), 
    dft(p(L,W^K), X, N).
    
dft(p([I],V),a(I), _).

dft(p(L, V^P), A1+V^P*A2, N) :-
    alternate(L, L1, L2),
    P1 is (P*2) mod N,
    dft(p(L1, V^P1), A1, N),
    dft(p(L2, V^P1), A2, N).


% dft(p([0,1,2,3,4,5,6,7],W^0),X0,8). 
% dft(p([0,1,2,3,4,5,6,7],W^1),X1,8). 
% dft(p([0,1,2,3,4,5,6,7],W^2),X2,8). 
% dft(p([0,1,2,3,4,5,6,7],W^3),X3,8). 
% dft(p([0,1,2,3,4,5,6,7],W^4),X4,8). 
% dft(p([0,1,2,3,4,5,6,7],W^5),X5,8). 
% dft(p([0,1,2,3,4,5,6,7],W^6),X6,8). 
% dft(p([0,1,2,3,4,5,6,7],W^7),X7,8). 
% dft(p([0,1,2,3,4,5,6,7],W^8),X8,8). 

```

1. 一定要注意递归中止的条件就是指当下标只存在一个的时候（唯一一个），也就是`p[i](W^k) =a(i)`,
对应的图片就是[第79张图](#79)


<hr/>
<br/>
<br/>
<h3 id="1"><font color="red">Fig1:</font></h3>
![dft1][1]
<hr/>
<br/>
<br/>
<h3 id="2"><font color="red">Fig2:</font></h3>
![dft2][2]
<hr/>
<br/>
<br/>
<h3 id="3"><font color="red">Fig3:</font></h3>
![dft3][3]
<hr/>
<br/>
<br/>
<h3 id="4"><font color="red">Fig4:</font></h3>
![dft4][4]
<hr/>
<br/>
<br/>
<h3 id="5"><font color="red">Fig5:</font></h3>
![dft5][5]
<hr/>
<br/>
<br/>
<h3 id="6"><font color="red">Fig6:</font></h3>
![dft6][6]
<hr/>
<br/>
<br/>
<h3 id="7"><font color="red">Fig7:</font></h3>
![dft7][7]
<hr/>
<br/>
<br/>
<h3 id="8"><font color="red">Fig8:</font></h3>
![dft8][8]
<hr/>
<br/>
<br/>
<h3 id="9"><font color="red">Fig9:</font></h3>
![dft9][9]
<hr/>
<br/>
<br/>
<h3 id="10"><font color="red">Fig10:</font></h3>
![dft10][10]
<hr/>
<br/>
<br/>
<h3 id="11"><font color="red">Fig11:</font></h3>
![dft11][11]
<hr/>
<br/>
<br/>
<h3 id="12"><font color="red">Fig12:</font></h3>
![dft12][12]
<hr/>
<br/>
<br/>
<h3 id="13"><font color="red">Fig13:</font></h3>
![dft13][13]
<hr/>
<br/>
<br/>
<h3 id="14"><font color="red">Fig14:</font></h3>
![dft14][14]
<hr/>
<br/>
<br/>
<h3 id="15"><font color="red">Fig15:</font></h3>
![dft15][15]
<hr/>
<br/>
<br/>
<h3 id="16"><font color="red">Fig16:</font></h3>
![dft16][16]
<hr/>
<br/>
<br/>
<h3 id="17"><font color="red">Fig17:</font></h3>
![dft17][17]
<hr/>
<br/>
<br/>
<h3 id="18"><font color="red">Fig18:</font></h3>
![dft18][18]
<hr/>
<br/>
<br/>
<h3 id="19"><font color="red">Fig19:</font></h3>
![dft19][19]
<hr/>
<br/>
<br/>
<h3 id="20"><font color="red">Fig20:</font></h3>
![dft20][20]
<hr/>
<br/>
<br/>
<h3 id="21"><font color="red">Fig21:</font></h3>
![dft21][21]
<hr/>
<br/>
<br/>
<h3 id="22"><font color="red">Fig22:</font></h3>
![dft22][22]
<hr/>
<br/>
<br/>
<h3 id="23"><font color="red">Fig23:</font></h3>
![dft23][23]
<hr/>
<br/>
<br/>
<h3 id="24"><font color="red">Fig24:</font></h3>
![dft24][24]
<hr/>
<br/>
<br/>
<h3 id="25"><font color="red">Fig25:</font></h3>
![dft25][25]
<hr/>
<br/>
<br/>
<h3 id="26"><font color="red">Fig26:</font></h3>
![dft26][26]
<hr/>
<br/>
<br/>
<h3 id="27"><font color="red">Fig27:</font></h3>
![dft27][27]
<hr/>
<br/>
<br/>
<h3 id="28"><font color="red">Fig28:</font></h3>
![dft28][28]
<hr/>
<br/>
<br/>
<h3 id="29"><font color="red">Fig29:</font></h3>
![dft29][29]
<hr/>
<br/>
<br/>
<h3 id="30"><font color="red">Fig30:</font></h3>
![dft30][30]
<hr/>
<br/>
<br/>
<h3 id="31"><font color="red">Fig31:</font></h3>
![dft31][31]
<hr/>
<br/>
<br/>
<h3 id="32"><font color="red">Fig32:</font></h3>
![dft32][32]
<hr/>
<br/>
<br/>
<h3 id="33"><font color="red">Fig33:</font></h3>
![dft33][33]
<hr/>
<br/>
<br/>
<h3 id="34"><font color="red">Fig34:</font></h3>
![dft34][34]
<hr/>
<br/>
<br/>
<h3 id="35"><font color="red">Fig35:</font></h3>
![dft35][35]
<hr/>
<br/>
<br/>
<h3 id="36"><font color="red">Fig36:</font></h3>
![dft36][36]
<hr/>
<br/>
<br/>
<h3 id="37"><font color="red">Fig37:</font></h3>
![dft37][37]
<hr/>
<br/>
<br/>
<h3 id="38"><font color="red">Fig38:</font></h3>
![dft38][38]
<hr/>
<br/>
<br/>
<h3 id="39"><font color="red">Fig39:</font></h3>
![dft39][39]
<hr/>
<br/>
<br/>
<h3 id="40"><font color="red">Fig40:</font></h3>
![dft40][40]
<hr/>
<br/>
<br/>
<h3 id="41"><font color="red">Fig41:</font></h3>
![dft41][41]
<hr/>
<br/>
<br/>
<h3 id="42"><font color="red">Fig42:</font></h3>
![dft42][42]
<hr/>
<br/>
<br/>
<h3 id="43"><font color="red">Fig43:</font></h3>
![dft43][43]
<hr/>
<br/>
<br/>
<h3 id="44"><font color="red">Fig44:</font></h3>
![dft44][44]
<hr/>
<br/>
<br/>
<h3 id="45"><font color="red">Fig45:</font></h3>
![dft45][45]
<hr/>
<br/>
<br/>
<h3 id="46"><font color="red">Fig46:</font></h3>
![dft46][46]
<hr/>
<br/>
<br/>
<h3 id="47"><font color="red">Fig47:</font></h3>
![dft47][47]
<hr/>
<br/>
<br/>
<h3 id="48"><font color="red">Fig48:</font></h3>
![dft48][48]
<hr/>
<br/>
<br/>
<h3 id="49"><font color="red">Fig49:</font></h3>
![dft49][49]
<hr/>
<br/>
<br/>
<h3 id="50"><font color="red">Fig50:</font></h3>
![dft50][50]
<hr/>
<br/>
<br/>
<h3 id="51"><font color="red">Fig51:</font></h3>
![dft51][51]
<hr/>
<br/>
<br/>
<h3 id="52"><font color="red">Fig52:</font></h3>
![dft52][52]
<hr/>
<br/>
<br/>
<h3 id="53"><font color="red">Fig53:</font></h3>
![dft53][53]
<hr/>
<br/>
<br/>
<h3 id="54"><font color="red">Fig54:</font></h3>
![dft54][54]
<hr/>
<br/>
<br/>
<h3 id="55"><font color="red">Fig55:</font></h3>
![dft55][55]
<hr/>
<br/>
<br/>
<h3 id="56"><font color="red">Fig56:</font></h3>
![dft56][56]
<hr/>
<br/>
<br/>
<h3 id="57"><font color="red">Fig57:</font></h3>
![dft57][57]
<hr/>
<br/>
<br/>
<h3 id="58"><font color="red">Fig58:</font></h3>
![dft58][58]
<hr/>
<br/>
<br/>
<h3 id="59"><font color="red">Fig59:</font></h3>
![dft59][59]
<hr/>
<br/>
<br/>
<h3 id="60"><font color="red">Fig60:</font></h3>
![dft60][60]
<hr/>
<br/>
<br/>
<h3 id="61"><font color="red">Fig61:</font></h3>
![dft61][61]
<hr/>
<br/>
<br/>
<h3 id="62"><font color="red">Fig62:</font></h3>
![dft62][62]
<hr/>
<br/>
<br/>
<h3 id="63"><font color="red">Fig63:</font></h3>
![dft63][63]
<hr/>
<br/>
<br/>
<h3 id="64"><font color="red">Fig64:</font></h3>
![dft64][64]
<hr/>
<br/>
<br/>
<h3 id="65"><font color="red">Fig65:</font></h3>
![dft65][65]
<hr/>
<br/>
<br/>
<h3 id="66"><font color="red">Fig66:</font></h3>
![dft66][66]
<hr/>
<br/>
<br/>
<h3 id="67"><font color="red">Fig67:</font></h3>
![dft67][67]
<hr/>
<br/>
<br/>
<h3 id="68"><font color="red">Fig68:</font></h3>
![dft68][68]
<hr/>
<br/>
<br/>
<h3 id="69"><font color="red">Fig69:</font></h3>
![dft69][69]
<hr/>
<br/>
<br/>
<h3 id="70"><font color="red">Fig70:</font></h3>
![dft70][70]
<hr/>
<br/>
<br/>
<h3 id="71"><font color="red">Fig71:</font></h3>
![dft71][71]
<hr/>
<br/>
<br/>
<h3 id="72"><font color="red">Fig72:</font></h3>
![dft72][72]
<hr/>
<br/>
<br/>
<h3 id="73"><font color="red">Fig73:</font></h3>
![dft73][73]
<hr/>
<br/>
<br/>
<h3 id="74"><font color="red">Fig74:</font></h3>
![dft74][74]
<hr/>
<br/>
<br/>
<h3 id="75"><font color="red">Fig75:</font></h3>
![dft75][75]
<hr/>
<br/>
<br/>
<h3 id="76"><font color="red">Fig76:</font></h3>
![dft76][76]
<hr/>
<br/>
<br/>
<h3 id="77"><font color="red">Fig77:</font></h3>
![dft77][77]
<hr/>
<br/>
<br/>
<h3 id="78"><font color="red">Fig78:</font></h3>
![dft78][78]
<hr/>
<br/>
<br/>
<h3 id="79"><font color="red">Fig79:</font></h3>

这里是需要特别注意的，根据prolog的depth-first的原则，这边是已经在最底层，也就是达到
一条线上的base case.所以会出现反向的过程。


![dft79][79]
<hr/>
<br/>
<br/>
<h3 id="80"><font color="red">Fig80:</font></h3>
![dft80][80]
<hr/>
<br/>
<br/>
<h3 id="81"><font color="red">Fig81:</font></h3>
![dft81][81]
<hr/>
<br/>
<br/>
<h3 id="82"><font color="red">Fig82:</font></h3>

第一次反向：
![dft82][82]
<hr/>
<br/>
<br/>
<h3 id="83"><font color="red">Fig83:</font></h3>
![dft83][83]
<hr/>
<br/>
<br/>
<h3 id="84"><font color="red">Fig84:</font></h3>
![dft84][84]
<hr/>
<br/>
<br/>
<h3 id="85"><font color="red">Fig85:</font></h3>
![dft85][85]
<hr/>
<br/>
<br/>
<h3 id="86"><font color="red">Fig86:</font></h3>
![dft86][86]
<hr/>
<br/>
<br/>
<h3 id="87"><font color="red">Fig87:</font></h3>

开始不断地重复

![dft87][87]


[1]:/images/prolog/dft/dft1.png
[2]:/images/prolog/dft/dft2.png
[3]:/images/prolog/dft/dft3.png
[4]:/images/prolog/dft/dft4.png
[5]:/images/prolog/dft/dft5.png
[6]:/images/prolog/dft/dft6.png
[7]:/images/prolog/dft/dft7.png
[8]:/images/prolog/dft/dft8.png
[9]:/images/prolog/dft/dft9.png
[10]:/images/prolog/dft/dft10.png
[11]:/images/prolog/dft/dft11.png
[12]:/images/prolog/dft/dft12.png
[13]:/images/prolog/dft/dft13.png
[14]:/images/prolog/dft/dft14.png
[15]:/images/prolog/dft/dft15.png
[16]:/images/prolog/dft/dft16.png
[17]:/images/prolog/dft/dft17.png
[18]:/images/prolog/dft/dft18.png
[19]:/images/prolog/dft/dft19.png
[20]:/images/prolog/dft/dft20.png
[21]:/images/prolog/dft/dft21.png
[22]:/images/prolog/dft/dft22.png
[23]:/images/prolog/dft/dft23.png
[24]:/images/prolog/dft/dft24.png
[25]:/images/prolog/dft/dft25.png
[26]:/images/prolog/dft/dft26.png
[27]:/images/prolog/dft/dft27.png
[28]:/images/prolog/dft/dft28.png
[29]:/images/prolog/dft/dft29.png
[30]:/images/prolog/dft/dft30.png
[31]:/images/prolog/dft/dft31.png
[32]:/images/prolog/dft/dft32.png
[33]:/images/prolog/dft/dft33.png
[34]:/images/prolog/dft/dft34.png
[35]:/images/prolog/dft/dft35.png
[36]:/images/prolog/dft/dft36.png
[37]:/images/prolog/dft/dft37.png
[38]:/images/prolog/dft/dft38.png
[39]:/images/prolog/dft/dft39.png
[40]:/images/prolog/dft/dft40.png
[41]:/images/prolog/dft/dft41.png
[42]:/images/prolog/dft/dft42.png
[43]:/images/prolog/dft/dft43.png
[44]:/images/prolog/dft/dft44.png
[45]:/images/prolog/dft/dft45.png
[46]:/images/prolog/dft/dft46.png
[47]:/images/prolog/dft/dft47.png
[48]:/images/prolog/dft/dft48.png
[49]:/images/prolog/dft/dft49.png
[50]:/images/prolog/dft/dft50.png
[51]:/images/prolog/dft/dft51.png
[52]:/images/prolog/dft/dft52.png
[53]:/images/prolog/dft/dft53.png
[54]:/images/prolog/dft/dft54.png
[55]:/images/prolog/dft/dft55.png
[56]:/images/prolog/dft/dft56.png
[57]:/images/prolog/dft/dft57.png
[58]:/images/prolog/dft/dft58.png
[59]:/images/prolog/dft/dft59.png
[60]:/images/prolog/dft/dft60.png
[61]:/images/prolog/dft/dft61.png
[62]:/images/prolog/dft/dft62.png
[63]:/images/prolog/dft/dft63.png
[64]:/images/prolog/dft/dft64.png
[65]:/images/prolog/dft/dft65.png
[66]:/images/prolog/dft/dft66.png
[67]:/images/prolog/dft/dft67.png
[68]:/images/prolog/dft/dft68.png
[69]:/images/prolog/dft/dft69.png
[70]:/images/prolog/dft/dft70.png
[71]:/images/prolog/dft/dft71.png
[72]:/images/prolog/dft/dft72.png
[73]:/images/prolog/dft/dft73.png
[74]:/images/prolog/dft/dft74.png
[75]:/images/prolog/dft/dft75.png
[76]:/images/prolog/dft/dft76.png
[77]:/images/prolog/dft/dft77.png
[78]:/images/prolog/dft/dft78.png
[79]:/images/prolog/dft/dft79special.png
[80]:/images/prolog/dft/dft80.png
[81]:/images/prolog/dft/dft81special.png
[82]:/images/prolog/dft/dft82special.png
[83]:/images/prolog/dft/dft83.png
[84]:/images/prolog/dft/dft84.png
[85]:/images/prolog/dft/dft85.png
[86]:/images/prolog/dft/dft86.png
[87]:/images/prolog/dft/dft87repetive.png
[88]:/images/prolog/dft/result.png
[89]:/images/prolog/dft/outcome.png
[90]:/images/prolog/dft/dftcoef.png
[91]:https://en.wikipedia.org/wiki/Root_of_unity 
[92]:http://www.springer.com/cn/book/9783540629719 
