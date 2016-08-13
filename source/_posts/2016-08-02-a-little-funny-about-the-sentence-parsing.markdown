---
layout: post
title: "A little funny about the sentence parsing"
date: 2016-08-02 04:25:45 +0800
comments: true
categories: prolog
---


![result][1]

Prolog's one advantage is that he can be professional in parsing the natural 
language sentence or maybe expert system(a little not strict).

Below is only an example about that.
<!--more-->



<h2 id="main">the main running file</h2>

the main running file which is used to link the [dcg.pl](#dcg) and [pretty.pl](#pretty) into the kernel.

Maybe you can also consult the [materials natural language][2]

``` prolog

:- ensure_loaded([dcg,pretty]).

test(InFile,OutFile) :-
    open(InFile,read,In),
    open(OutFile,write,Out),
    read(In,S),
    parse_sentence(S,In,Out),
    close(In),
    close(Out).

parse_sentence(end_of_file,_,_) :- !.
parse_sentence(S,In,Out) :-
    write(Out,S), nl(Out),
    nl(Out),
    s(Tree,S,[]),
    pptree(Out,Tree), nl(Out),
    nl(Out), nl(Out),
    read(In,NewS),
    parse_sentence(NewS,In,Out).
parse_sentence(_,In,Out) :-
    write(Out,'no'), nl(Out),
    nl(Out), nl(Out),
    read(In,S),
    parse_sentence(S,In,Out).
```


<h2 id="dcg">the dcg file</h2>
the dcg file which is the core of the parse sentense program.


``` prolog

:- module(dcg,[s/3]).

s(s(NP,VP)) --> np(NP,X,Y,subject), vp(VP,X,Y).

np(np(DET,NBAR,PP),X,Y,_) --> det(DET,X), nbar(NBAR,X,Y), pp(PP).
np(np(DET,NBAR),X,Y,_) --> det(DET,X), nbar(NBAR,X,Y).
np(np(PRO),X,Y,Z) --> pro(PRO,X,Y,Z).

vp(vp(V),X,Y) --> v(V,X,Y).
vp(vp(V,NP),X,Y) --> v(V,X,Y), np(NP,_,_,object).

nbar(nbar(JP),X,3) --> jp(JP,X).

pp(pp(PREP,NP)) --> prep(PREP), np(NP,_,_,object).

jp(N,X) --> n(N,X).
jp(jp(ADJ,JP),X) --> adj(ADJ), jp(JP,X).

det(det(DET),X) --> [DET], {lex(DET,det,X)}.

pro(pro(PRO),X,Y,Z) --> [PRO], {lex(PRO,pro,X,Y,Z)}.

v(v(V),X,Y) --> [V], {lex(V,v,X,Y)}.

prep(prep(PREP)) --> [PREP], {lex(PREP,prep)}.

n(n(N),X) --> [N], {lex(N,n,X)}.

adj(adj(ADJ)) --> [ADJ], {lex(ADJ,adj)}.

lex(i,pro,singular,1,subject).
lex(you,pro,singular,2,subject).
lex(he,pro,singular,3,subject).
lex(she,pro,singular,3,subject).
lex(it,pro,singular,3,subject).

lex(we,pro,plural,1,subject).
lex(you,pro,plural,2,subject).
lex(they,pro,plural,3,subject).

lex(me,pro,singular,1,object).
lex(you,pro,singular,2,object).
lex(him,pro,singular,3,object).
lex(her,pro,singular,3,object).
lex(it,pro,singular,3,object).

lex(us,pro,plural,1,object).
lex(you,pro,plural,2,object).
lex(them,pro,plural,3,object).

lex(shoot,v,singular,1).
lex(shoot,v,singular,2).
lex(shoots,v,singular,3).

lex(shoot,v,plural,_).

lex(the,det,_).
lex(a,det,singular).

lex(man,n,singular).
lex(woman,n,singular).
lex(table,n,singular).
lex(cow,n,singular).
lex(shower,n,singular).

lex(men,n,plural).
lex(women,n,plural).

lex(on,prep).
lex(under,prep).

lex(small,adj).
lex(frightened,adj).
lex(big,adj).
lex(fat,adj).
```



<h2 id="pretty">the pretty file</h2> 
the pretty file which is used to decorate the output sentences.

```
:- module(pretty,[pptree/2]).

pptree(OS,s(NP,VP)) :- 
    write(OS,'s('), nl(OS),
    tab(OS,4), pptree(OS,NP,4), nl(OS),
    tab(OS,4), pptree(OS,VP,4),
    write(OS,')').

pptree(OS,det(DET)) :-
    write(OS,'det('),
    write(OS,DET),
    write(OS,')').

pptree(OS,pro(PRO)) :-
    write(OS,'pro('),
    write(OS,PRO),
    write(OS,')').

pptree(OS,v(V)) :-
    write(OS,'v('),
    write(OS,V),
    write(OS,')').

pptree(OS,prep(PREP)) :-
    write(OS,'prep('),
    write(OS,PREP),
    write(OS,')').

pptree(OS,adj(ADJ)) :-
    write(OS,'adj('),
    write(OS,ADJ),
    write(OS,')').

pptree(OS,n(N),_) :-
    write(OS,'n('),
    write(OS,N),
    write(OS,')').

pptree(OS,np(DET,NBAR,PP),I) :-
    Indent is I+4,
    write(OS,'np('), nl(OS),
    tab(OS,Indent), pptree(OS,DET), nl(OS), 
    tab(OS,Indent), pptree(OS,NBAR,Indent), nl(OS),
    tab(OS,Indent), pptree(OS,PP,Indent),
    write(OS,')').

pptree(OS,np(DET,NBAR),I) :-
    Indent is I+4,
    write(OS,'np('), nl(OS),
    tab(OS,Indent), pptree(OS,DET), nl(OS), 
    tab(OS,Indent), pptree(OS,NBAR,Indent),
    write(OS,')').

pptree(OS,np(PRO),I) :-
    Indent is I+4,
    write(OS,'np('), nl(OS),
    tab(OS,Indent), pptree(OS,PRO),
    write(OS,')').

pptree(OS,vp(V),I) :-
    Indent is I+4,
    write(OS,'vp('), nl(OS),
    tab(OS,Indent), pptree(OS,V),
    write(OS,')').

pptree(OS,vp(V,NP),I) :-
    Indent is I+4,
    write(OS,'vp('), nl(OS),
    tab(OS,Indent), pptree(OS,V), nl(OS),
    tab(OS,Indent), pptree(OS,NP,Indent),
    write(OS,')').

pptree(OS,nbar(JP),I) :-
    Indent is I+4,
    write(OS,'nbar('), nl(OS),
    tab(OS,Indent), pptree(OS,JP,Indent),
    write(OS,')').

pptree(OS,pp(PREP,NP),I) :-
    Indent is I+4,
    write(OS,'pp('), nl(OS),
    tab(OS,Indent), pptree(OS,PREP), nl(OS),
    tab(OS,Indent), pptree(OS,NP,Indent),
    write(OS,')').

pptree(OS,jp(ADJ,JP),I) :-
    Indent is I+4,
    write(OS,'jp('), nl(OS),
    tab(OS,Indent), pptree(OS,ADJ), nl(OS),
    tab(OS,Indent), pptree(OS,JP,Indent),
    write(OS,')').

```



[1]: /images/prolog/result.png
[2]: http://jueqingsizhe66.github.io/blog/2016/07/28/prolog-some-exercises/#8 
