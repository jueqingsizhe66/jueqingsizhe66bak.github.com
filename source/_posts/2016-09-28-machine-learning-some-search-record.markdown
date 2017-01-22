---
layout: post
title: "Machine Learning some search record"
date: 2016-09-28 17:20:32 +0800
comments: true
categories: matlab
---

学习[stanford Cs229机器学习][20]的一些资料汇总。

<!--more-->

[手把手教你实现SVM算法（一）][1] 

[详解2D-PCA （二维PCA）][2]  

[手把手教你实现SVM算法（二）][3] 

了解了一些机器学习，想到了prolog是一门人工智能算法，所以顺便搜索到了[prolog九宫格][4] .
首先这段prolog代码显然是简短的、简单的（但是不友好 声明式语言一定得写的友好）。我们根本没有告诉计算机该怎么做，我们只是说现在我们有一个九宫格，并要求：

+ （1）每个格都是从1到9中的一个整数；
+ （2）九宫格中的数字不会重复； 
+ （3）每行的和都相等；
+ （4）每列的和都相等；
+ （5）两条对角线上的和也相等

<font color="red">现在运行还有点问题！待以后解决！</font>

[基于PDC_PROLOG自学习机器博弈 ][7]

<font color=green>白马负金羁</font>的[机器学习资料][5] （包含R语言，逻辑规约，HMM,卡尔曼滤波）

[MATLAB中进行基于SVM的数据分析][6] 


[机器学习简史][8] 

[机器学习概念][9] 

[贝叶斯及其他常用机器学习算法 From analyticsvidhya][10]（<font color=red>相当棒</font>的一个机器学习算法介绍 好评不断  通过计算认识了贝叶斯算法） 
并同时介绍了what is predicator?  what is Class Prior Probabilty? 
what is Posterior Probability? what is Predictor Prior Probability?

下面是我的手写笔记(需要2min看懂)
![bayes1][22]

![bayes2][23]

![bayes3][24]


[Analyticsvidhya][11],一个相当不错的计算机学习网站。

[贝叶斯理论 From wikipedia][12]

[平凡而有神奇的贝叶斯 刘未鹏][25]， <font color=red>从这里开始认识到贝叶斯的美</font>

[A Complete Tutorial on Tree Based Modeling from Scratch (in R & Python)][13]，<font color=red>十分清楚,需要一定时间理解</font>

R语言和Python语言比较经常被人们用来做机器学习，其实包括lisp,prolog,java等都是可以的。
[完整的R语言教程][14]

[完整的python语言教程][15]

[ipython机器学习][16].Ipython是一个比较有用的python IDE界面，提供比较友好的输出调试界面(网页上操作),所以顺便也查了一下怎么用。 

尝试Notebook, 还需要下载一些其它咚咚
1) 下载安装 pyzmq, 在这里不建议使用pip, pip对pyzmq支持不太好，装不上。我尝试使用easy_install
      c:>easy_install.exe pyzmq
2) 下载安装 jinja2,
      c:>easy_install.exe jinja2        
3) 下载安装 tornado,
      c:>easy_install.exe tornado       
好了，使用下面命令就可以把Notebook起来：
      c:>ipython.exe notebook

十分友好的ipython notebook，在命令行敲一下会打开一个浏览器窗口，用于编写Python的语句


然而python和R并不是我的选择，我还是熟悉<font color="red">matlab</font>,打算选择matlab作为我机器学习的工具。

关于机器语言的一些常用算法

![algo][17]

+ 雷老师推荐的[LS-SVM matlab][18], <font color="red">结论是选择LS-SVM</font>.
+ [台湾的林智仁(Lin Chih-Jen)教授等编写SVM模式识别和回归的软件包，多语言实现][19]


附带dot语言图片生成

[dot reference][21]

``` gnuplot
digraph G1{
    
    compound=true;
    graph[fontname=Kaiti];
    edge[fontname=SimSun]; 
    node[fontname=SimSun]; 
    MachineLearning[shape=box,style=rounded,fillcolor="#1f8842"]; 
subgraph cluster_1{
        
//    SL[shape=ellipse,label="Supervised Learning"];
    label="Supervised Learning(This algorithm consist of a target / outcome 
    \nvariable (or dependent variable) which is to be predicted 
    \nfrom a given set of predictors (independent variables))";
    node[style=filled,fillcolor=chartreuse];

    Reg[label="1. Linear Regression"];
    LR[label="2. logistic Regression"];
    DT[label="3. Decision Tree"];
    SVM[label="4. Supported Vector Machine"];
    RF[label="5. Random Forest"];
    KNN[label="6. KNN"];
    NB[label="7. Naive Bayes"];

    Reg->LR->DT->SVM->RF->KNN->NB->Reg;
    }
    subgraph cluster_2{
        
    node[style=filled,fillcolor=chartreuse];
    //USL[shape=ellipse,label="UnSupervised Learning"];
    label="UnSupervised Learning(do not have any target or outcome variable to
    \npredict / estimate.  It is used for clustering population
    \nin different groups, which is widely used for 
    \nsegmenting customers in different groups for specific intervention )";
    AA[label="Apriori Alogirithm"];
    KM[label="K-means"];
    }
    subgraph cluster_3{
        
    node[style=filled,fillcolor=chartreuse];
    label="Reinforcement Learning(It works this way: the machine is 
    \nexposed to an environment where it 
    \ntrains itself continually using trial and error. 
    \nThis machine learns from past experience 
    \nand tries to capture the best possible knowledge 
    \nto make accurate business decisions )";
    MDP[label="Markov Decision Process"]
    }


    //MachineLearning->{SL USL RL};
    MachineLearning->Reg[lhead=cluster_1];
    MachineLearning->AA[lhead=cluster_2];
    MachineLearning->MDP[lhead=cluster_3];

}

```



[1]: http://blog.csdn.net/alvine008/article/details/9097105
[2]: http://blog.csdn.net/alvine008/article/details/9097109
[3]: http://blog.csdn.net/alvine008/article/details/9097111
[4]: http://blog.csdn.net/baimafujinji/article/details/49745389
[5]: http://blog.csdn.net/baimafujinji/article/category/6048259
[6]: http://blog.csdn.net/baimafujinji/article/details/6472337
[7]: http://wenku.baidu.com/link?url=H44D9f3eHZB-RqPf2tbICEIHtn3L5ZvCkLS6IcOg_uEhW-nmZocKYCoUGYBjQ6O_YXRbYptH2Yr4vsTtkwjB-36xpFnCAf1Ygyjo3zscoky
[8]: http://blog.csdn.net/stark_summer/article/details/50364666
[9]: http://blog.csdn.net/stark_summer/article/details/50249943
[10]: https://www.analyticsvidhya.com/blog/2015/08/common-machine-learning-algorithms/#rd?sukey=e74171513d3453dd223d8ac2deeb49e1eac3a7c7511b955187120c332b0e4df30837b86c6940bf1967a7c57107306d2c
[11]: https://www.analyticsvidhya.com/
[12]: https://en.wikipedia.org/wiki/Bayes%27_theorem
[13]: https://www.analyticsvidhya.com/blog/2016/04/complete-tutorial-tree-based-modeling-scratch-in-python/
[14]: https://www.analyticsvidhya.com/blog/2016/02/complete-tutorial-learn-data-science-scratch/
[15]: https://www.analyticsvidhya.com/blog/2016/01/complete-tutorial-learn-data-science-python-scratch-2/
[16]: http://blog.csdn.net/u013731339/article/details/40025373
[17]: /images/MachineLearning/algo.jpg
[18]:http://www.esat.kuleuven.be/sista/lssvmlab/ 
[19]:http://www.csie.ntu.edu.tw/~cjlin/libsvm/ 
[20]:http://cs229.stanford.edu/ 
[21]: http://jueqingsizhe66.github.io/blog/2016/09/28/graphviz-notebook/
[22]: /images/MachineLearning/beya1.jpg
[23]: /images/MachineLearning/beya2.jpg
[24]: /images/MachineLearning/beya3.jpg
[25]:http://mindhacks.cn/2008/09/21/the-magical-bayesian-method/ 
