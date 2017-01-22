---
layout: post
title: "graphviz notebook"
date: 2016-09-28 17:14:53 +0800
comments: true
categories: graphviz
---

[graphviz][1]可用于帮助你绘制工具之外和工具之内的想法。关于绘图的历史可以参看[A short note of the graph drawing][7].

<!--more-->

[graphviz下载链接][14], 安装很方便，一步一步进行即可，一般是在[命令行使用][13]。

下面是我记录学习的一些graphviz的笔记。只用pdf输出`dot -Tpdf *.dot -o *.pdf`，
因为质量较好(矢量化输出)，但是特殊情况可以参考[output format][13],
因为不同的输出会在[Node, Edge and Graph Attributes][9] 节点、边、图中表现出不同的样式出来。

![main1][4]

![main2][5]

## CFD

![CFD][17]
``` gnuplot
digraph G{
    compound=true; // 这样lhead ltail指向cluster才有效
    edge [fontname="SimSun"];
    graph[fontname="SimHei"]
    node [shape=box, fontname="KaiTi" size="5,5", style=filld, color=""];
    node[style=filled,color="lightblue"]

    rankdir=LR;
    {
    rank=same;
    before[label="前处理"];   
    compute[label="计算"];   
    after[label="后处理"];   
    }
    
    subgraph cluster1{
        geometry[label="几何建模"];
        mesh[label="网格生成"];
        geometry->mesh;
        }

    subgraph cluster2{
        initial[label="初始化"];
        setting[label="设置"];
        model[label="湍流模型"];
        output[label="输出"];
        initial->setting->model->output;
        }

    subgraph cluster3{
       plane[label="截面"];
       polyline[label="多边形线"];
       chart[label="表格"];
       pile[label="批处理"];
       contour[label="云图"];
        plane->polyline->chart;
        plane->contour;
        }
    before->compute->after;
    before->geometry[lhead=cluster1];
    compute->initial[lhead=cluster2];
    after->plane[lhead=cluster3];
    
}


```

## inner 图

注意使用circo生成，需要有一个回环圆。

compound用于子图的lhead属性，不设置没效果。

concentrate这里没什么作用。

![inner][15]

``` gnuplot
//circo -Tpdf inner.dot -o inner.pdf
digraph G{
    fontname="SimSun";
    fontsize=10;
    compound=true; // 这样lhead ltail指向cluster才有效
    edge[fontname="SimSun",arrowhead=vee,color=blue];
    graph[fontname="SimHei",color=lightgray];
    node[shape=box, fontname="KaiTi"];
    //concentrate=true;
    //node[style=filled,color="lightblue"];
    arrowsize=.5;

    concentrate=true;
    nodesep=0.5;
    ranksep=0.5;
    rankdir=BU;
    //rank=sink;
    //task[shape=polygon,sides=5,peripheries=2,label="图像处理",fillcolor="#F1C40F"];

/*
    subgraph cluster_0{
        
    innerpoint[shape=point,width=0,height=0];
    innerpoint1[shape=point,width=0,height=0];
    innerpoint2[shape=point,width=0,height=0];
    innerpoint3[shape=point,width=0,height=0];
    innerpoint4[shape=point,width=0,height=0];
    innerpoint->innerpoint1->innerpoint2->innerpoint3->innerpoint4->innerpoint[arrowsize=0.1];
    }

*/
    subgraph cluster_1{
        //rankdir=TB;
        label="内";
        style=filled;
        fillcolor=lightgray;
        //shape=ellpse;
        //edge[fontname="SimSun",arrowhead=vee,color=blue,style=dashed];
        //style=filled;
        //fillcolor=lightgray;
        //fillcolor=mintcream;
        //style=filled;

        {
            //node[style=filled,color="chartreuse"];
            //node[style=filled,color="palegreen"];
            //rank=same;
            node[color=red,shape=circle];
            edge[color=green,style=dashed]
            subgoal1[label="内1"];
            step1[label="内2"];
            step2[label="内3"];
            step3[label="内4"]
            step4[label="内n"]
            subgoal1-> step1->step2->step3->step4->subgoal1;
        }
       
    }
 //           innerpoint->{subgoal1 step1 step2 step3 step4}[color=red,style=dotted,ltail=cluster_0];

    outer1[label="外1"];
    outer2[label="外2"];
    outer3[label="外3"];
    outer4[label="外4"];
    outern[label="外n"];

    subgoal1->outern[ltail=cluster_1];
    step1->outer1[ltail=cluster_1];
    step2->outer2[ltail=cluster_1];
    step3->outer3[ltail=cluster_1];
    step4->outer4[ltail=cluster_1];
    
    outerf1[shape=point,width=0,height=0];
    outerf2[shape=point,width=0,height=0];
    outerf3[shape=point,width=0,height=0];
    outerf4[shape=point,width=0,height=0];
    outerfn[shape=point,width=0,height=0];

    outer1->outerf1;
    outer2->outerf2;
    outer3->outerf3;
    outer4->outerf4;
    outern->outerfn;

    //{rank=same; innerpoint step4 outer4};

   
   

}

```

## outer图

![outer][16]


``` gnuplot
//circo -Tpdf inner.dot -o inner.pdf
digraph G{
    fontname="SimSun";
    fontsize=10;
    compound=true; // 这样lhead ltail指向cluster才有效
    edge[fontname="SimSun",arrowhead=vee,color=blue];
    graph[fontname="SimHei",color=lightgray];
    node[shape=box, fontname="KaiTi"];
    //concentrate=true;
    //node[style=filled,color="lightblue"];
    arrowsize=.5;

    nodesep=0.5;
    ranksep=0.5;
    rankdir=BU;
    //center=true;
    //rank=sink;
    //task[shape=polygon,sides=5,peripheries=2,label="图像处理",fillcolor="#F1C40F"];




    subgraph cluster_1{
        //rankdir=TB;
        label="内";
        style=filled;
        fillcolor=chartreuse;
        //shape=ellpse;
        //edge[fontname="SimSun",arrowhead=vee,color=blue,style=dashed];
        //style=filled;
        //fillcolor=lightgray;
        //fillcolor=mintcream;
        //style=filled;

        {
            //node[style=filled,color="chartreuse"];
            //node[style=filled,color="palegreen"];
            //rank=same;
            //innerpoint[shape=point,width=0,height=0];
            node[color=red,shape=circle];
            edge[color="green",style=dashed]
            subgoal1[label="内1"];
            step1[label="内2"];
            step2[label="内3"];
            step3[label="内4"]
            step4[label="内n"]
            subgoal1-> step1->step2->step3->step4->subgoal1;

           // {subgoal1 step1 step2 step3 step4}->innerpoint[color="red"];
        }
       
    }
subgraph cluster_0{
    outer1[label="外1"];
    outer2[label="外2"];
    outer3[label="外3"];
    outer4[label="外4"];
    outern[label="外n"];
    //outer1->outer2->outer3->outer4->outern->outer1;
}
    outern->step3;
    outer1->step4;
    outer2->subgoal1;
    outer3->step1;
    outer4->step2;
    
    outerf1[shape=point,width=0,height=0];
    outerf2[shape=point,width=0,height=0];
    outerf3[shape=point,width=0,height=0];
    outerf4[shape=point,width=0,height=0];
    outerfn[shape=point,width=0,height=0];
    //outerf1->outerf2->outerf3->outerf4->outerfn->outerf1;

    outerf1->outer3;
    outerf2->outer2;
    outerf3->outer1;
    outerf4->outern;
    outerfn->outer4;

   
   // outer1->outer2->outer3->outer4->outern->outer1;
   

}

```

## 算法绘制

![algo][18]

``` gnuplot
digraph ast{
    size="4,4";
fontname="Microsoft YaHei";
fontsize=10;
node [shape=circle, fontname="Microsoft YaHei", fontsize=10];
edge [fontname="Microsoft YaHei", fontsize=10];
node [shape="plaintext"];
mul [label="mul(*)"];
add [label="add(+)"];
add -> 3
add -> 4;
mul -> add;
mul -> 5;
}

```

## 时序表制作

![sequence][19]

聪明的做法 是从图中隐藏了所有stepi（其实图像对象中是存在的，只不过打印不出来
并同时使用了plaintext的风格。


``` gnuplot
digraph G {
rankdir="LR";
node[shape="point", width=0, height=0];
edge[arrowhead="none", style="dashed"]
{
rank="same";
edge[style="solid"];
LC[shape="plaintext"];
LC -> step00 -> step01 -> step02 -> step03 -> step04 -> step05;
}
{
rank="same";
edge[style="solid"];
Agency[shape="plaintext"];
Agency -> step10 -> step11 -> step12 -> step13 -> step14 -> step15;
}
{
rank="same";
edge[style="solid"];
Agent[shape="plaintext"];
Agent -> step20 -> step21 -> step22 -> step23 -> step24 -> step25;
}
step00 -> step10 [label="sends email new custumer", arrowhead="normal"];
step11 -> step01 [label="declines", arrowhead="normal"];
step12 -> step02 [label="accepts", arrowhead="normal"];
step13 -> step23 [label="forward to", arrowhead="normal"];
step24 -> step14;
step14 -> step04 [arrowhead="normal"];


//xinran[shape="none",image="xinran2.jpg",label=""]
betterline[shape="record",style=filled,fillcolor=chartreuse];
betteredge[shape="record",style=filled,fillcolor=forestgreen];
}

```

## reference

1. graphviz的[在线文档][3],

2. dot语言的[在线文档][2],


3. [drawing Graph using dot][6]介绍了一些图表中分层的概念。

4. [使用Graphviz绘制流程图和关系图][8]教会了关于图、节点、边的属性设置(attribute property predictor)，
其实也可以参考[Node, Edge and Graph Attributes][9]关于默认属性的设置，相关的还可以参考[node shapes][10],
[Arrow Shapes][11], [colors][12] 这也是比较会经常查看的参数属性设置。


注意Mrecord不支持中文，为了实现[圆角][22]你可以使用`style=rounded`来实现(可以和filled叠加起来使用) `style="rounded,filled"`。
还可以参考[Creating Straight Edges in Graphviz][23],以及[官网属性文档style section][24] 

``` sh
node与edge公用样式："dashed"虚线, "dotted"点, "solid"固体框, "invis"隐藏 and "bold" 加粗
edge 特有样式："tapered" 锥形
node 特有样式："filled"填充, "diagonals"对角线 与 "rounded" 圆角
cluster可使用样式："filled"与"rounded"
 "radial"径向样式可被nodes, clusters 与graphs使用，如果使用需要指出一个径向渐变填充风格
```

graphviz也是支持插图的,

``` gnuplot
digraph G {
rankdir="LR";
xinran[shape="none",image="xinran2.jpg",label=""]
betterline[shape="record",style=filled,fillcolor=chartreuse];
betteredge[shape="record",style=filled,fillcolor=forestgreen];
}

```
5. [使用 Graphviz 生成自动化系统图 ][20].
6. [中文乱码][21].
7. [Using graph drawing ][21] <font color=red>很详细，推荐</font>

[1]:http://www.graphviz.org
[2]:http://www.graphviz.org/content/dot-language 
[3]:http://www.graphviz.org/Documentation.php 
[4]:/images/graphviz/main1.jpg
[5]:/images/graphviz/main2.jpg
[6]:http://www.tonyballantyne.com/graphs.html 
[7]:http://www.merl.com/publications/TR2001-49 
[8]:http://www.tuicool.com/articles/qeqeuyb 
[9]:http://graphviz.org/content/attrs 
[10]:http://graphviz.org/content/node-shapes 
[11]: http://graphviz.org/content/arrow-shapes
[12]:http://graphviz.org/content/color-names 
[13]:http://graphviz.org/content/command-line-invocation 
[14]:http://graphviz.org/Download.php 

[15]:/images/graphviz/inner.jpg
[16]:/images/graphviz/outer.jpg
[17]:/images/graphviz/CFD.jpg
[18]:/images/graphviz/binary.jpg
[19]:/images/graphviz/sequence.jpg
[20]:http://www.ibm.com/developerworks/cn/aix/library/au-aix-graphviz/index.html 
[21]:http://lockriver.blog.163.com/blog/static/487232242010101761749383/ 
[22]:http://www.graphviz.org/content/global-subgraph-style-statements 
[23]:http://stackoverflow.com/questions/7115870/creating-straight-edges-in-graphviz 
[24]:http://www.graphviz.org/content/attrs 
