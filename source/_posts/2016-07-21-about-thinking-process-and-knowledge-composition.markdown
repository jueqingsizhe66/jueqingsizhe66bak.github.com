---
layout: post
title: "about thinking process and knowledge composition"
date: 2016-07-21 17:17:43 +0800
comments: true
categories: prolog
---

We can classify what we see and think into three class:

1. thinking logic(don't care about the domain specific)
2. knowlege(domain specific language)
3. amusement

And the most important think you shold keep it in your brain

<div align="center""> Persistence! </div>
<div align="center""> My things? </div>
<!--more-->

<h1 >长期不变的知识，更需要时间的学习和提炼</h1>

1. Base(knowledge).	基础知识是长时间不变
2. Algorithm(knowledge).	
3. DataStructure(knowledge).	
4. GoodCoding(knowledge).	良好的编程习惯
5. AnalysisProblem(knowledge).	
6. SolveProblem(knowledge).	分析问题解决问题的能力
7. Learning_Temptation(knowledge).	强大学习欲望
8. Thingking_process(knowledge).	思维过程
9. add http protocol, tcpip protocol etc.
	

******

<h1> Varient Part(need to be learned)</h1>

+ [你的问题](#1)
+ [你遇到的问题是什么？](#2)
+ [你的知识](#3)
+ [你的理解](#4)
+ [你的方法：](#5)
+ [your vision](#6)
+ [你的演讲](#7)



******

<h2 id="1">你的问题</h2>

Problem(T,E) :- 
	problem(T),
	judgeAndDetermine(T),
	Focus(T,F), % 

	Solution(F,S),
	Example(S,E)
	

Focus(T,F):- 
    thingkingLength(T), thinkingDepth(T,F).


******

 <h2 id="2">你遇到的问题是什么？</h2>

ThinkingProblem(P,G,D):- 
    Problem(P), %% what is problem?
    Gain(P,G),   %你得到了什么
    Who_did_it(G,P), %%谁做了
    Say(G), %说出你的问题
    SpeakToUnknowPerson(G,D).%%选择一个人说

gain(P) :- 
    conclusion(P),
	Chew(P),
	Sortout(P).

******

 <h2 id="3">你的知识</h2>

Knowledge(K,S):-
	judgeAndDetermine(K),
	becomeKnowledge(K),
	DifferenceAndRelationship(K,S),%% the first principle(the base knowledge which will keep constant in a long period)
	ArchitectSystemOfKnowleget([K,S])



******


 <h2 id="4">你的理解</h2>

Understand(P,F):- 
	judgeAndDetermine(P),
	understand(P,T),   %你理解？
	understandProblem(T,F),  %%你理解问题？
	Essential(F,E),       %%问题的本质是什么
	YourUnderstand(E,P),    %你的理解是什么
	ContentWithYourUnderstanding(P,C),   %对你的理解满意？
	Construction(C,F).  %%你的理解有建设性？
******

 <h2 id="5">你的方法：</h2>
Method(P,M,S):
	judgeAndDetermine(M),
	Why(M), %为什么是M方法
	Whynot(S),
	BetterMethod(M),
	Best(M) , %M是最好方法？
	DifferenceAndRelation(M,S),
	Essential(M,P), %问题本质是什么
	Essential(M),%该方法的本质是什么
	Reasoning(M) .%%这个方法是如何推理的

******

 <h2 id="6">Your vision</h2>

Watch(R,E):-
	watchAndCompare(R,T),
	Phonomenon(T),
	Explain(T,S),
	Cause(S,R),   %%你这样解释的理由是什么
	Example(R,E).


******

 <h2 id="7">你的演讲</h2>

presentation(P,T):-
	problem(P),
	whatYouDid(T),
	Important(T),
	Result(T).


******

<hr/>
<br/>
<br/>
<br/>
<div align="center"><font color='red' fontsize='16'>Did you feel content with what you write above?  A little.</font></div>
<br/>
<br/>
<div align="center"><font color='red' fontsize='16'>Just a little.</font></div>
<br/>
<br/>
<br/>


<br/>
<br/>
<br/>
<br/>
<br/>


<div align="center"><font color='red' fontsize='16'> Everybody need some push! Push your goal into life</font></div>

You can check the books ["The design of EveryDay things"][10] to specify the vision points below.

![know your path][1]

Know your goal path.
<br/>
<hr/>
<br/>
![know your path][2]

know your goal state.
<br/>
<hr/>
<br/>

![know your path][3]

know how to get there with multistage methods.
<br/>
<hr/>
<br/>

![know your path][4]

know the layers of your problem. Use layers of language to figure it out.

+ reflective layer
    - plan(before)
    - compare(after)
+ behavior layer
    - specify(before)
    - interpret(after)
+ visual layer
    - perform(before)
    - perceive

<br/>
<hr/>
<br/>

![know your path][5]

Create the subgoals to finish the goal.
<br/>
<hr/>
<br/>

![know your path][6]

compare with the language design process

+ parser
+ semantic
+ need
+ define
+ abstract(should modify to interface)
+ implement
<br/>
<hr/>
<br/>

![know your path][7]

+ picture mind
+ graph mind
+ image mind

Can they reduce your thinking about problem? Possibly not, but thinking it again and again.


Compare with login proof process. We are all live with assume.

+ assume
+ implement
+ assume
+ implement
+ assume
+ implement


<br/>
<hr/>
<br/>

![know your path][8]

+ Determine your goal
+ do
+ then check
+ once again.

<br/>
<hr/>
<br/>

![know your path][9]

Need some deadline.

<hr/>
<br/>
<br/>
<br/>
<div align="center"><font color='red' fontsize='16'>Did you feel content with what you write above? </font></div>
<br/>
<br/>
<div align="center"><font color='red' fontsize='16'>Enough.</font></div>
<br/>
<br/>
<br/>


[1]: /images/prolog/goalPic/0.png
[2]: /images/prolog/goalPic/1.png
[3]: /images/prolog/goalPic/3.png
[4]: /images/prolog/goalPic/4.png
[5]: /images/prolog/goalPic/5.png
[6]: /images/prolog/goalPic/6.png
[7]: /images/prolog/goalPic/7.png
[8]: /images/prolog/goalPic/8.png
[9]: /images/prolog/goalPic/9.png
[10]: http://jueqingsizhe66.github.io/blog/2015/10/24/zhong-guo-ban-de-the-design-of-everyday-things/
