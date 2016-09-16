---
layout: post
title: "Stack overflow as prolog learning zone"
date: 2016-09-17 01:37:17 +0800
comments: true
categories: prolog

---


十分好的学习prolog网站：
[Stack Overflow for prolog][6]

![stackoverflow][9]

[Bob Kowalski][11] firstly put out
`Algorithm = Logic + Control`, short as `A=L+C`, L states what it does(logic component),while
C states how it is done(the control part), we should memory him.
![Bob][12]

[Leon Sterling][13] said "Declarative programming(prolog) clears the mind. Prolog is indeed, a tool for the thinking ".Declarative 
statement enables us to concentrate(focus) on the essentials of the problem without getting bogged down in too much operational
detail(避免淹没在处理的细节当中,leaving the execute model to process the control part). We should memory him.
![Leon Sterling][14]

We should be grateful for so many people who contribute to the development of the prolog.Also we should memory them and give them appauses, thank you for
giving us opportunity for learning about the logic programming.

而logic始于scientific thinking,也就是logic programming 要求我们做到以下几点:

1. name your goals.
2. name your knowledges.
3. name your premises or assumptions.

if all three statements are true, so we can use the execute model(such as prolog) to deduce from the premise, to analysis the truth and
falsity of every statement in your knowledge representation systems, to verify the validation of arguments in your goal, finally to
create the consistency of your claims(or your knowledge systems).


<!--more-->
Advice for learning prolog from [stackoverflow][1]

1. Stick to writing pure, monotonic code only. You can only judge to choose one or the other side if you know both. I assume you have some previous experience producing side effects in some command-oriented language, but none with pure code. As a consequence this will mean that you will refrain from writing inherently non-monotonic code.
2. Play with the toplevel. Imagine, the toplevel is the only way to access your programs. How would you formulate a problem such that it fits into this format? The SWI toplevel has been specifically designed to permit such light-weight interaction.
3. Use [clpfd][3] for arithmetics. Don't use (is)/2, it makes your code much too moded.
4. Enjoy the algebraic properties of pure, monotonic code. Think of it: You add a goal, no matter where, and still you can predict that this goal will specialize your program (and at best leaves it as is). You can - blindly - remove a goal, and still you know (part of) its effect.
5. Study the notion of a [failure-slice][2] to master non-termination.
6. Do not use a step-by-step tracer/debugger, as it is offered in many Prologs. It only shows you the precise steps Prolog takes. It does not show you anything directly related to the meaning of the program. It reinforces a step-by-step thinking.
7. Watch your language. The way how you talk about a program influences the way you think about it. So, if you use a lot of operationalizing language (like: This does this etc), chances are you reinforce the command-oriented view. There is a cleaner way of talking about things, but you need to find it. This is probably the hardest part.
8. Learn [some annotations for prolog predicate definition][15], and what's most important, "Keep it as you used to be"

[clpfd][4] stands for Constraint Logic Programming over Finite Domains , almost implemented by all modern prolog interpreter,such as
the library implemented by swi-prolog,SICSTus prolog ,YAP etc and as integral part of the system implemented by gnu-prolog and B-prolog. Especially, in swi-prolog, you can use [the clpfd ][5]right below,

``` prolog

:- use_module(library(clpfd))
```


+ [最短路径][8]
+ [jekejekech prolog][10]


I suddenly think the prolog as a doctor(my insight) for your programming , it can [diganose][16] what's wrong with your problem

``` prolog
disease(hiv,[sore_throat,headache,fever,rash]).
disease(pregnancy,[fatigue,vomiting,light_headedness,increased_waistline]).
disease(flu,[fatigue,fever,tiredness,nasal_discharge]).

%% 第一种诊断方式：有点庸医的感觉,输出结果太多
diagnose([], []).
diagnose(Name, [H|T]) :-
    disease(The_Disease, Symptoms),
    member2(H, Symptoms),
    write(Name), write(' has/is '), writeln(The_Disease),
    diagnose(Name, T).

member2(X,[X|_]).
member2(X,[_|T]):-
    member2(X,T).

%diagnose(kevin,[sore_throat,fatigue,tiredness,rash]). 

%% 改进第一种诊断方式 输出太多不必要的结果
symptoms_diagnosis(Symptoms, Diagnosis) :-
   member(Symptom, Symptoms),
   disease(Diagnosis, Indications),
   member(Symptom, Indications).

%setof(Diagnosis,symptoms_diagnosis([sore_throat,fatigue,tiredness,rash],Diagnosis),Diagnoses).
%setof(Diagnose, symptoms_diagnosis([sort_throat,fatigue,tiredness],Diagnose),Diagnose)%.
%Diagnose = [flu, pregnancy]. 


%% 第二种 诊断方式(函数式风格，比较不好看懂)
diagnose2(Name, Symptoms) :-
    findall(D, (disease(D, S), intersection(S, Symptoms, I), I \== []), MayGot),
    atomic_concat(Name, ' has/is ', Start),
    maplist(atomic_concat(Start), MayGot, Temp),
    maplist(writeln, Temp).

%116 ?- diagnose2(kevin,[sore_throat,fatigue]).
%kevin has/is hiv
%kevin has/is pregnancy
%kevin has/is flu
%true. 


%% 采用different_list的风格, 第三种诊断方式
diagnose3(Name, Symptoms) :- diagnose0(Name, Symptoms, []).

diagnose0(Name, [], Diseases) :-
    print_diseases(Name, Diseases).
diagnose0(Name, [H|T], DIn) :-
    disease(Disease, Symptoms),
    member(H, Symptoms),
    % you may want to add a cut here to avoid choicepoints
    (
        member(Disease, DIn)
    ->
        diagnose0(Name, T, DIn)
    ;
        DOut = [Disease|DIn],
        diagnose0(Name, T, DOut)
    ).

print_diseases(_Name, []).
print_diseases(Name, [H|T]) :-
    write(Name), write(' has/is '), writeln(H),
    print_diseases(Name, T).

%diagnose3(kevin,[sore_throat,fatigue]). 
%kevin has/is pregnancy
%kevin has/is hiv
%true .
%kevin has/is flu
%kevin has/is hiv
%true ;
%false.
% 第一组结果 hiv 和flv都有fatigue,所以flv被忽略,未作进一步分析

```

正如上面所说，不要老是debug/tracer, 否则思维就陷入了控制流当中，下面还是列出了调试的结果(不完整，只想表达prolog是你问题的医生)

<hr/>
<br/>
<br/>
<h3 id="diagnose0"><font color="red">diagnose0:</font></h3>
![diagnose0][17]
<hr/>
<br/>
<br/>
<h3 id="diagnose1"><font color="red">diagnose1:</font></h3>
![diagnose1][18]
<hr/>
<br/>
<br/>
<h3 id="diagnose2"><font color="red">diagnose2:</font></h3>
![diagnose2][19]
<hr/>
<br/>
<br/>
<h3 id="diagnose3"><font color="red">diagnose3:</font></h3>
![diagnose3][20]
<hr/>
<br/>
<br/>
<h3 id="diagnose4"><font color="red">diagnose4:</font></h3>
![diagnose4][21]
<hr/>
<br/>
<br/>
<h3 id="diagnose5"><font color="red">diagnose5:</font></h3>
![diagnose5][22]
<hr/>
<br/>
<br/>
<h3 id="diagnose6"><font color="red">diagnose6:</font></h3>
![diagnose6][23]
<hr/>
<br/>
<br/>
<h3 id="diagnose7"><font color="red">diagnose7:</font></h3>
![diagnose7][24]
<hr/>
<br/>
<br/>
<h3 id="diagnose8"><font color="red">diagnose8:</font></h3>
![diagnose8][25]
<hr/>
<br/>
<br/>
<h3 id="diagnose9"><font color="red">diagnose9:</font></h3>
![diagnose9][26]
<hr/>
<br/>
<br/>
<h3 id="diagnose10"><font color="red">diagnose10:</font></h3>
![diagnose10][27]
<hr/>
<br/>
<br/>
<h3 id="diagnose11"><font color="red">diagnose11:</font></h3>
![diagnose11][28]
<hr/>
<br/>
<br/>
<h3 id="diagnose12"><font color="red">diagnose12:</font></h3>
![diagnose12][29]
<hr/>
<br/>
<br/>
<h3 id="diagnose13"><font color="red">diagnose13:</font></h3>
![diagnose13][30]
<hr/>
<br/>
<br/>
<h3 id="diagnose14"><font color="red">diagnose14:</font></h3>
![diagnose14][31]
<hr/>
<br/>
<br/>
<h3 id="diagnose15"><font color="red">diagnose15:</font></h3>
![diagnose15][32]
<hr/>
<br/>
<br/>
<h3 id="diagnose16"><font color="red">diagnose16:</font></h3>
![diagnose16][33]
<hr/>
<br/>
<br/>
<h3 id="diagnose17"><font color="red">diagnose17:</font></h3>
![diagnose17][34]
<hr/>
<br/>
<br/>
<h3 id="diagnose18"><font color="red">diagnose18:</font></h3>
![diagnose18][35]
<hr/>
<br/>
<br/>
<h3 id="diagnose19"><font color="red">diagnose19:</font></h3>
![diagnose19][36]
<hr/>
<br/>
<br/>
<h3 id="diagnose20"><font color="red">diagnose20:</font></h3>
![diagnose20][37]
<hr/>
<br/>
<br/>
<h3 id="diagnose21"><font color="red">diagnose21:</font></h3>
![diagnose21][38]
<hr/>
<br/>
<br/>
<h3 id="diagnose22"><font color="red">diagnose22:</font></h3>
![diagnose22][39]
<hr/>
<br/>
<br/>
<h3 id="diagnose23"><font color="red">diagnose23:</font></h3>
![diagnose23][40]
<hr/>
<br/>
<br/>
<h3 id="diagnose24"><font color="red">diagnose24:</font></h3>
![diagnose24][41]
<hr/>
<br/>
<br/>
<h3 id="diagnose25"><font color="red">diagnose25:</font></h3>
![diagnose25][42]
<hr/>
<br/>
<br/>
<h3 id="diagnose26"><font color="red">diagnose26:</font></h3>
![diagnose26][43]
<hr/>
<br/>
<br/>
<h3 id="diagnose27"><font color="red">diagnose27:</font></h3>
![diagnose27][44]
<hr/>
<br/>
<br/>
<h3 id="diagnose28"><font color="red">diagnose28:</font></h3>
![diagnose28][45]
<hr/>
<br/>
<br/>
<h3 id="diagnose29"><font color="red">diagnose29:</font></h3>
![diagnose29][46]


[1]:http://stackoverflow.com/questions/15709808/features-of-good-prolog-code 
[2]:http://stackoverflow.com/questions/tagged/failure-slice 
[3]:http://stackoverflow.com/questions/tagged/clpfd 
[4]:http://stackoverflow.com/tags/clpfd/info 
[5]:http://www.pathwayslms.com/swipltuts/clpfd/clpfd.html 
[6]:http://stackoverflow.com/search?tab=newest&q=prolog 
[7]:http://stackoverflow.com/ 
[8]:http://stackoverflow.com/questions/39504265/find-the-shortest-path-in-dlv/39525015#39525015 
[9]:/images/prolog/stack/stackoverflow.png
[10]:https://plus.google.com/+JekejekeCh  
[11]:https://en.wikipedia.org/wiki/Robert_Kowalski 
[12]:https://upload.wikimedia.org/wikipedia/commons/thumb/6/6f/Robert_Kowalski.jpg/220px-Robert_Kowalski.jpg 
[13]:https://www.researchgate.net/profile/Leon_Sterling/publications 
[14]:https://i1.rgstatic.net/ii/profile.image/AS%3A272697389219850%401442027532347_l/Leon_Sterling.png 
[15]:http://jueqingsizhe66.github.io/blog/2016/08/15/prolog-programming-coding-convention/ 
[16]:http://stackoverflow.com/questions/8305811/prolog-recursion-skipping-same-results/8309945#8309945 
[17]:/images/prolog/stack/diagnose/diagnose0.png
[18]:/images/prolog/stack/diagnose/diagnose1.png
[19]:/images/prolog/stack/diagnose/diagnose2.png
[20]:/images/prolog/stack/diagnose/diagnose3.png
[21]:/images/prolog/stack/diagnose/diagnose4.png
[22]:/images/prolog/stack/diagnose/diagnose5.png
[23]:/images/prolog/stack/diagnose/diagnose6.png
[24]:/images/prolog/stack/diagnose/diagnose7.png
[25]:/images/prolog/stack/diagnose/diagnose8.png
[26]:/images/prolog/stack/diagnose/diagnose9.png
[27]:/images/prolog/stack/diagnose/diagnose10.png
[28]:/images/prolog/stack/diagnose/diagnose11.png
[29]:/images/prolog/stack/diagnose/diagnose12.png
[30]:/images/prolog/stack/diagnose/diagnose13.png
[31]:/images/prolog/stack/diagnose/diagnose14.png
[32]:/images/prolog/stack/diagnose/diagnose15.png
[33]:/images/prolog/stack/diagnose/diagnose16.png
[34]:/images/prolog/stack/diagnose/diagnose17.png
[35]:/images/prolog/stack/diagnose/diagnose18.png
[36]:/images/prolog/stack/diagnose/diagnose19.png
[37]:/images/prolog/stack/diagnose/diagnose20.png
[38]:/images/prolog/stack/diagnose/diagnose21.png
[39]:/images/prolog/stack/diagnose/diagnose22.png
[40]:/images/prolog/stack/diagnose/diagnose23.png
[41]:/images/prolog/stack/diagnose/diagnose24.png
[42]:/images/prolog/stack/diagnose/diagnose25.png
[43]:/images/prolog/stack/diagnose/diagnose26.png
[44]:/images/prolog/stack/diagnose/diagnose27.png
[45]:/images/prolog/stack/diagnose/diagnose28.png
[46]:/images/prolog/stack/diagnose/diagnose29.png
