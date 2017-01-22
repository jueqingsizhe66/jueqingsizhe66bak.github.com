---
layout: post
title: "Jump Out of Ontology"
date: 2016-09-28 23:21:06 +0800
comments: true
categories: life
---


[工具][11](进一步可以参考[The design of everyday things][12])之内，讲究的是如何设计这个东西(制作者角度)，可能更多讲的是设计的东西，而不是完成的功能，更多用于写技术路线
![inner1][1]
<!--more-->

工具之外，讲究的是从工具的外头看这个东西（眼睛放在工具外面,使用者角度），从工具外看黑夹子的工具，看他在不同的工况下能够实现什么功能？ 所以更多用于写软件说明书。

![outer][2]


[语言][4]之内和语言之外（站在一个本不存在逻辑体之外）

语言之内 说的是语言如何创造出来，如何写一个解释器，怎么设计这门语言，添加那些语言特性[(参考EOPL语言特性)][3]，语言的语法怎么样等等

语言之外是指运用这门语言 我能够解决某个工况问题，某个实际问题，某个数值问题，


[问题][9]之内是指解决这个问题，问题之外是指解决了这个问题之后还可以解决其他什么问题。

内是指内部链接，外是指外部链接

[领域][10]之内和领域之外， 领域之内是指从专业的角度进行介绍，领域之外是平民（不懂专业知识）的角度进行解释。


工具、语言、问题、领域等都可以归结为本体[ontology][5]论的[知识][7](进一步参考[knowledge_graph][6]以及 [knowledge-presentation and reasoning][8])。
[Ontology (information science)][16]

![ontology][14]

其中，亚里斯多德指出本体论

+ what it is (its 'whatness', quidditas or essence)
+ how it is (its 'howness' or qualitativeness)
+ how much it is (quantitativeness)
+ where it is, its relatedness to other beings[3]


1. whatness指的是individuals from class(这边是指类 不是指[贝叶斯概率][15]中的class-predictor键值对，其中class指目标值，而predictor是指属性值，或者叫做表征值,比如表征天气情况：晴天 下雨天 多云)
也就是说whatness解决的是存在的[原因][17](whyness).

经常我们会这样描述`What is A`

```
明确主谓宾(忌讳缺乏主谓宾)

A是一种***技术，解决了***问题，帮助我们实现了***，帮助我们简化了
****。第一***，第二****，第三***；
首先****，其次***,最后***,

总的来说，A****

```

也就是我们需要[说明][31]([声明式语言][30])

1. 确保主谓宾
2. 说明它的功能，解决了什么问题(现实都是问题驱动 problem-driven),好处是什么？
3. 可能的话在细化分析，第一，第二，第三，首先，其次等.

任何一项技术的学习其实都有一个思路，

1. 这门技术是什么？
2. 怎么使用这门技术？
3. 使用过程中需要注意些什么？
4. 这门技术和其他的优缺点？
5. 这门技术的主要场所是在哪里？

最好能够熟练地自己提问自己上面的5个问题。并反复运用说明方式组织好自己的逻辑语言。这大概是whatness该注意的大部分内容。


howness是从定性的角度分析该存在体，而howmuchness则是从定量的角度对存在体进行分析。

由于存在体有很多个，于是需要确定他们的存在关系，于是选用了whereness(也就是空间维度，每加一层维度都可以确定一个y=ax+b的关系，可以参考[SVM][18], 
所有的算法问题都最终可以归结为[降阶][19]和[优化][20]（优化只是一种思想）问题，而[最小二乘法][21]却是贯穿始终，最小二乘法进一步其实original from y=ax+b)

关于最小二乘法的总结如下：

![leastsquare1][22]
![leastsquare2][23]
![leastsquare3][24]
![leastsquare4][25]

最小二乘法另外一些描述

1. [最小二乘法 least square method ][26]
2. [least_squares fitting][27]
3. [最小二乘法fortran实现 from 熊叉叉][28]
4. [最小二乘法介绍][29]

紧接着whereness，我觉得还需要引入一个时间维度也就是whenness，我觉得他很重要，因为机器学习的很多算法都是需要通过时间不断的训练，需要明确whenness,
现实当中约会、上课、上班、吃饭、睡觉等都有一个whenness，我们都生活在时间的单项维度当中，放眼在[历史][33]的时间尺度来说，我们一个人真的是微不足道，而放眼到我们自己的局部时间尺度来说，我们却需要细细的
品味时光的间隙，体味其中的变化，记录可能需要表征的一类变化量，进一步可能会影响到其他的量的变化（所以任何东西是不是都有着[因果论][32],也就是说在历史的长河中，我们是渺小的一粒沙子，跌跌撞撞的前行。


某某美国总统[罗斯福][13]曾说:"Joint and separate ,which is the problem". 本体论就是用于解决个体之间的分类和具体关系。


所以最终，本体之外和本体之内？

那是你对自己的定位(在历史长河微不足道，在局部时间，精打细算。秉着科学的精神，踏实生活；挣点小钱，养家糊口; 钱不多，只求闲的时候可以写写岁月中的一些趣事)和取舍(保留需要的东西，舍弃糟粕), 以及你对[世界][35]或者[宇宙][34]的定位(要不然，不想了，想点当下)。


## Appendix

``` gnuplot
//fdp -Tpdf ontology.dot -o ontology.pdf 

digraph G{
    ontology[label="Ontology", shape=box,style=rounded] ;   

    node[style="rounded,filled",fillcolor=chartreuse];

    whatness[label="whatness"];
    howness[label="howness"];
    howmuchness[label="howmuchness"];
    whereness[label="whereness"];
    whenness[label="whenness"];

    ontology{ whatness howness howmuchness whereness whenness};
}

```
[graphviz reference][36]

[1]: /images/ontology/inner.png
[2]: /images/ontology/outer.png
[3]:http://jueqingsizhe66.github.io/blog/2016/02/27/the-fourth-interpreter-about-the-traceproc/ 
[4]:https://en.wikipedia.org/wiki/Language 
[5]:https://en.wikipedia.org/wiki/Ontology 
[6]:https://en.wikipedia.org/wiki/Knowledge_Graph 
[7]:https://en.wikipedia.org/wiki/Knowledge 
[8]:https://en.wikipedia.org/wiki/Knowledge_representation_and_reasoning 
[9]:https://en.wikipedia.org/wiki/Problem_solving 
[10]:https://en.wikipedia.org/wiki/Domain 
[11]:https://en.wikipedia.org/wiki/Tool 
[12]:https://zhidao.baidu.com/share/4ee9bd642cdd8cc40d654d3e6096b35d.html 
[13]:https://en.wikipedia.org/wiki/Franklin_D._Roosevelt 
[14]: /images/ontology/ontology.jpg
[15]:http://jueqingsizhe66.github.io/blog/2016/09/28/machine-learning-some-search-record/ 
[16]:https://en.wikipedia.org/wiki/Ontology_(information_science) 
[17]: https://en.wikipedia.org/wiki/Reason
[18]:https://en.wikipedia.org/wiki/Support_vector_machine 
[19]:https://en.wikipedia.org/wiki/Reduction_(mathematics) 
[20]:https://en.wikipedia.org/wiki/Mathematical_optimization 
[21]:https://en.wikipedia.org/wiki/Least_squares 
[22]: /images/ontology/leastSquare1.jpg 
[23]: /images/ontology/leastSquare2.jpg 
[24]: /images/ontology/leastSquare3.jpg 
[25]: /images/ontology/leastSquare4.jpg 
[26]:http://www.cnblogs.com/emanlee/archive/2011/08/03/2125712.html 
[27]:http://mathworld.wolfram.com/LeastSquaresFitting.html 
[28]:http://jean-pierre.moreau.pagesperso-orange.fr/f_lstsqr.html 
[29]:http://www.cnblogs.com/monoSLAM/p/5252917.html 
[30]:https://en.wikipedia.org/wiki/Declarative_programming 
[31]:https://en.wikipedia.org/wiki/Statement_(logic) 
[32]:https://en.wikipedia.org/wiki/Causality 
[33]:https://en.wikipedia.org/wiki/History 
[34]:https://en.wikipedia.org/wiki/Cosmos 
[35]:https://en.wikipedia.org/wiki/World 
[36]:http://jueqingsizhe66.github.io/blog/2016/09/28/graphviz-notebook/
