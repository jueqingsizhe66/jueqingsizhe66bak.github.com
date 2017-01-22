---
layout: post
title: "a little java note"
date: 2016-10-09 01:58:39 +0800
comments: true
categories: JavaBasic
---

<div align="center">目录</div>

+ [1. 基础部分](#1)
+ [2. 通过构造函数，构造出Natural recursion](#2)
+ [3. 重新设计RemAV RemFishV RemIntV](#3)
+ [4. 下一步把PieD的字段放入到对应的参数当中](#4)
+ [5. 引入 this 关键字，指代访问者本身，同步修改对应的访问者类](#5)
+ [6. 需要进一步提取出visitor部分的函数	](#6)
+ [7. 统一bTreeVisitorI itreevisitori ttreevisitori](#7)
+ [8. 重新设计RemAV RemFishV  RemIntV 为RemV(用Object替换）	](#8)
+ [9. 然后现在把Remv 和Subst重新放入PieD](#9)
+ [10. 紧接着 我们还想着把Remv  SubstV放入参数的位置](#10)
+ [11. 紧接着我们可以进行下一步抽象	](#11)
+ [12. 紧接着我们发现rem和subst代码类似	](#12)
+ [13. 然后再实现Set集合类型](#13)
+ [14. SetEvalV直接继承IntEvalV不合理?	](#14)
+ [15. IntEvalD 和SetEvalD很多相似之处	](#15)
+ [16. 根据extends使用Override增加函数的丰富性	](#16)


视角没想到可以分成不同的classes，并在此基础上进行extends和implement，最终new通过constructors创造不同的value。两级在于基础元件和功能元件两部分。两级也可以分为extends和implements。集于一个class。


<!--more-->

学完[《a little java》][2]的心得,

1.	构造函数：new通过构造函数，产生了datatype的值；构造函数由此进行了natural recursion， 并通过一个基类退出循环（详见PieD).new create values.
2.	This,一般指向当前对象，但是在函数式编程中，this可能指代的是当前对象之后的所有剩余对象
"this"指代的是自指对象 也就是函数某某的实例化对象，并且是一直不变的。Just self-referential，because this is a RemV, and it is exactly what we need to complete the job. 所以this指代函数莫某的对象
3.	Top某某函数询问某种来自函数某某的forTop功能 ，并传递构造函数的fields值(properties）以及该某某函数comsumes的objects（arguments)
4.	  升级版本 the method accepts a visitor(该visitor接口包含了forTop forBot抽象方法，该接口统一了不同的函数某某，所以所有函数某某统称为accept函数某某）and ask for its services， so we call it accept， 而该接口的通常实例化对象也被叫做ask， ask for services.
5.	Object类型可以抽象int boolean 其他类型
6.	函数的抽象大体表现为参数数据类型，返回值数据类型和函数body的具体实现。
    a.	参数数据类型
    b.	返回值数据类型
    c.	body具体实现
7.	简化（简洁）、拓展性、方便
8.	当我们的参数值从method变到fields，我们就没必要在反复调用对象的方法的时候 需要不断地comsume，而同时又保证了this对象，也就是不断地自指（期间不产生新的fields）所以也就是使得计算加快了。这也是this存在的原因（并且该this对象只用在访问者对象中）
9.	We know that a visitorI contains one method each for the Circle, Square, and Trans variants,. And each of these methods
Consumes the fields of the respective kinds of objects(objects of types)
某某对象的某某函数，（携带函数某某和字段） 询问函数某某的for对象某某功能，（携带对象某某）
某某对象的accept携带ask并内部由 ask 的for对象某某携带对象某某组成，而for对象某某携带对象某某并内部由对象某某的accept携带当前ask对象构成。
某某对象的accept携带ask并内部由 ask 的for对象某某组成，而for对象某某携带对象某某并内部由对象某某的accept组成。
 
在对象内部，某某对象的accept携带ask并内部由 ask 的for对象某某携带对象某某组成，而在visitor内部，for对象某某携带对象某某并内部由对象某某（也就是consumes the fields of the respective kinds of objects 占据对象类型的字段属性）的accept携带当前ask对象构成。
We don't specify fields in interfaces. And in 7 W hatever.
any case, we don't want anybody else to
see p.
This and that在对象间传递，而consume在函数间传递（Hangs over back and forth(fields 在对象间传递，而consume在函数间传递)
10.	该书包含着TLS TSS 和prolog编程的思想(因为你要理解new Top new Bot等需要有fact and rule的意识)（仔细去品尝 go to absorb) 还有就是skeleton and techniques技术
11.	Extends interface,this extension produces an interface that contains all the obligations(i.e. names of methods and what they consume and produce) of shapeVisitorI and the additional one named forUnion(所以换个角度说，one class can implement many interfaces.
12.	重载override 相同的名字带着不同的输入类型
13.	点号可以读作 from   点号之前叫做from who  点号之后叫做service
14.	解释的重点是什么？？？ 要点是什么

![duichen][3]
 

什么是思考力的三要素，讲这个问题之前我们先来了解一下什么是思考思维，首先思考是思维的一种探索活动，而在思维过程中产生的一种具有积极性和创造性的作用力这种就是思考力。据物理学理解，思考力具有三个基本要素：分别为大小、方向、作用点。思考力同样也离不开三个基本要素：分别为大小、方向、作用点。

1. 大小 ——思考力首先取决于思考者掌握的关于思考对象的知识和信息量（大小），如果没有相关的知识和信息，就不可能产生相关的思考活动。一般情况下，知识量和信息量越大，思考就越加具体、全面和完整，从而决定了思考的维度。
2. 方向 ——我们这里所说的思考有别于妄想和幻想，而是一种有目的性和有计划性的思维活动，因此，这种思考需要有一定的价值导向，也就是思路——体现为目的性、方向性和一致性。漫无目的地思考难以发挥强有力的思考力，常常会把思考引进死胡同，导致思路夭折和无果而终。目的性、方向性、一致性和价值导向，决定着思考的角度和向度。
3. 作用点 ——必须把思考集中在特定的对象上，并把握其中的关键点，这样的思考就会势如破竹。如果找不准思考的着力点，就会精力分散、思维紊乱、胡思乱想，出现东一榔头西一棒的现象。思考就会停留在事物的表面上浮光掠影，无法深刻认识事物的本质。思考在作用点上的集中性程度，决定着思考的强度和力度(广度和深度）。
 

<h3 id="1">基础部分</h3>

理解分层。
 
```  java
KebabD(chapter2) 烤肉
    isVeggie();
    whatHolder();
        Holder 烤肉摆放工具
            Object;
            Holder(Object);
            isVeggie();
            whatHolder();
        Shallot  葱
            KebabD;
            Shallot(KebabD);
            isVeggie();
            whatHolder();
         
        Shrimp 虾
            KebabD;
            Shrimp (KebabD);
            isVeggie();
            whatHolder();
             
        Radish 萝卜
            KebabD;
            Radish (KebabD);
            isVeggie();
            whatHolder();
         
        Pepper 胡椒粉
            KebabD;
            Pepper (KebabD);
            isVeggie();
            whatHolder();
             
        Zucchini 西葫芦
            KebabD;
            Zucchini (KebabD);
            isVeggie();
            whatHolder();
         
 
RodD(chapter2) 杆  将烤肉串起来工具(烤肉摆放方式1）
    Dagger 匕首
    Sabre 军刀
    Sword 剑
     
PlateD(chapter2) 盘子  （烤肉摆放方式2)
    Gold 金盘子
    Silver
    Brass 黄铜盘子
    Copper 镀铜盘子
    Wood   木盘子
 
PointD(chapter1,2) 点
    distanceTo0(int,int); //可以把它们移入到构造函数中
    closerTo0();
        CartesianPt 笛卡尔坐标
            Int
            Int
            CartesianPt(int,int);
            distanceTo0();
            Closeto0();
        ManhattenPt 曼哈顿坐标
            Int
            Int
            ManhattenPt(int,int);
            distanceTo0();
            Closeto0();
         
        圆柱坐标系
        球坐标系
 
PieD
 
 
PizzaD（chapter3,4）
    remA 去除比萨饼的凤尾鱼订料（防止过咸）
    topAwC() 在凤尾鱼顶料加上奶酪顶料（盖住咸味）
    subAbC() 将所有的凤尾鱼顶料换成奶酪顶料
        Crust 面包皮
            remA();
            topAwC();
            subAbC();
        Cheese  奶酪
            pizzaD
            Cheese(PizzaD)
            remA();
            topAwC();
            subAbC();
         
        Olive   橄榄
            pizzaD
            Olive(PizzaD)
            remA();
            topAwC();
            subAbC();
             
        Anchovy 凤尾鱼
            pizzaD
            Anchovy(PizzaD)
            remA();
            topAwC();
            subAbC();
             
        Sausage 香肠
            pizzaD
            Sausage(PizzaD)
            remA();
            topAwC();
            subAbC();
        Spinach 菠菜
            pizzaD
            Spinach(PizzaD)
            remA();
            topAwC();
            subAbC();
         
 
 
 
Shape
 
Shish(chapter2，4) 羊肉串
    onlyOnions(); 
    isVegetarian();
        Skew 架子 烤肉叉子
        Onion 洋葱
        Lamb  羔羊肉
        Tomato 西红柿
         
 
Tree


SeasoningD (chapter1) 调味品
    Sage(鼠尾草)
    Pepper
    Salt
    Thyme(百里香)
 
NumD (chapter1)
    Zero
    OneMoreThan
 
LayerD
    Base
    Slice
 
FruitD

```

**第二条建议**
    When writing a function over a datatype,
    place a method in each of the variants that make up the datatype.
    If a field of a variant belongs to the same datatype,
    the method may call the corresponding method of the field in
    computing the function.（疑问子类 每继承一次父类 都得重写三个父类的抽象函数 有点费事。。 ----那么访问者模式到底是如何解决的？）
**第八条建议**
    When extending a class, use overriding
    to enrich its functionality.

根据以上建议， LtdSubstV 可以直接在 SubstV 类上进行继承和扩展。
``` java
Shish(chapter2，4) 羊肉串
   OnlyOnionsV
   IsVegetarianV
   onlyOnions(); 
   isVegetarian();
        Skew 架子 烤肉叉子
            onlyOnions();
            isVegetarian();
        Onion 洋葱
            ShishD
            Onion(ShishD);
            onlyOnions();
            isVegetarian();
        Lamb  羔羊肉
            ShishD
            Lamb(ShishD);
            onlyOnions();
            isVegetarian();
             
        Tomato 西红柿
            ShishD
            Tomato(ShishD);
            onlyOnions();
            isVegetarian();


OnlyOnionsV 
    forSkew();
    forOnion(ShishD);
    forLamb(ShishD);
    forTomato(ShishD);
 
isVegetatianV
    forSkew();
    forOnion(ShishD);
    forLamb(ShishD);
    forTomato(ShishD);
     
 
ShishD
   OnlyOionsV ooFn
   IsVegeterian ivFn
   onlyOnions()a;
   isVegetarian()a;
        Skew
            onlyOnions();
            isVegetarian();
        Onion
            onlyOnions();
            isVegetarian();
        Lamb
            onlyOnions();
            isVegetarian();
        Tomato
            onlyOnions();
            isVegetarian();
         
 
```
 
<h3 id="2">通过构造函数，构造出Natural recursion部分，形成递归，递归出口为skew</h3>

``` java
RemAV
    forCurst();
    forCheese(PizzaD);
    forOlive(PizzaD);
    forAnchovy(PizzaD);
    forSausage(PizzaD);

TopAwCV
    forCurst();
    forCheese(PizzaD);
    forOlive(PizzaD);
    forAnchovy(PizzaD);
    forSausage(PizzaD);

SubAbCV
    forCurst();
    forCheese(PizzaD);
    forOlive(PizzaD);
    forAnchovy(PizzaD);
    forSausage(PizzaD);
 
 
PizzaD 
   RemAV remFn
   TopAwCV topFn
   SubAbCV  subFn
   remA()a;
   topAwC()a;
   subAbC()a;
        Crust
            remA();
            topAwC();
            subAwC();
        Cheese
            PizzaD
            Cheese(PizzaD)
            remA();
            topAwC();
            subAwC();
        Olive
            PizzaD
            Olive(PizzaD)
            remA();
            topAwC();
            subAwC();
        Anchovy
            PizzaD
            Anchovy(PizzaD)
            remA();
            topAwC();
            subAwC();
        Sausage
            PizzaD
            Sausage(PizzaD
            remA();
            topAwC();
            subAwC();


PieD
    RemAV raFn
    RemFish rfFn
    remA()a;
    remFish(FishD)a;
        Bot
            remA();
            remFish(FishD);
        Top
            Object
            PieD
            Top(Object, PieD)
            remA();
            remFish(FishD);
             
FishD
    Salmon 鲑鱼
    Equals(Object);
    Anchovy 凤尾鱼
    Equals(Object);
    Tuna  金枪鱼
    Equals(Object);
 
RemAV
    forBot
    fotTop(Object, PieD)
RemFishV
    forBot(FishD)
    forTop(Object, PieD,  FishD)
 
RemIntV
    forBot(int)
    forTop(Object, pieD, Integer)
     
``` 
<h3 id="3">重新设计RemAV RemFishV  RemIntV 为RemV(用Object替换）</h3>

``` java
RemV
    forBot(Object)
    forTop(Object, pieD, Object)

```


<h3 id="4">下一步把PieD的字段放入到对应的参数当中</h3>

``` java

abstract class PieD {
    abstract PieD rem(RemV remFn, Object o);
    abstract PieD subst(SubstV substFn, Object n, Object o);
}
 
PieD 
    Rem()a;
    Subst()a;
        Top
            Top(Object,Object)
            Rem();
            Subst();
        Bot
            Bot(Object,Object)
            Rem();
            Subst();

```
<h3 id="5">引入 this 关键字，指代访问者本身，同步修改对应的访问者类。</h3>

``` java
PieD 
    Rem(Remv)a;
    Subst(Subst)a;
        Top
            Top
            Rem
            Subst
        Bot
            Bot
            Rem
            Subst

```
<h3 id="6">需要进一步提取出visitor部分的函数</h3>
 
这样就可以把所有

``` java
PieVisitorI 
    forBot();
    forTop();
        Remv
            forBot();
            forTop();
        SubstV
            forBot();
            forTop();
     
pieD
    Accept(PieVisitorI)
        Bot
            Accept(pieVisitorI);
        Top
            Accept(pieVisitorI);
 
 
 
 
FruitD
    Peach
    Apple
    Pear
    Lemon
    Fig 无花果
 
TreeD
    Accept(bTreeVisitorI)
    Accept(iTreeVisitorI)
    Accept(tTreeVisitorI)
        Bub 芽
            Accept(bTreeVisitorI)
            Accept(iTreeVisitorI)
            Accept(tTreeVisitorI)
        Flat 平顶
            FruitD
            TreeD
            Flat(FruitD, TreeD)
            Accept(bTreeVisitorI)
            Accept(iTreeVisitorI)
            Accept(tTreeVisitorI)
        Split 分枝
            TreeD
            TreeD
            Split(TreeD, TreeD)
            Accept(bTreeVisitorI)
            Accept(iTreeVisitorI)
            Accept(tTreeVisitorI)
 
bTreeVisitorI
    forBud();
    forFlat(FruitD, TreeD);
    forSplit(TreeD, TreeD);
        bIsFlatV implements bTreeVisitorI
            forBud();
            forFlat(FruitD, TreeD);
            forSplit(TreeD, TreeD);
        bIsSplitV implements bTreeVisitorI
            forBud();
            forFlat(FruitD, TreeD);
            forSplit(TreeD, TreeD);
        bHasFruitV implements bTreeVisitorI
            forBud();
            forFlat(FruitD, TreeD);
            forSplit(TreeD, TreeD);

 
iTreeVisitorI
    forBud();
    forFlat(FruitD, TreeD);
    forSplit(TreeD, TreeD);
        iHeightV implements iTreeVisitorI
            forBud();
            forFlat(FruitD, TreeD);
            forSplit(TreeD, TreeD);
        iOccursV implements iTreeVisitorI
            FruitD
            iOccursV(FruitD)
            forBud();
            forFlat(FruitD, TreeD);
            forSplit(TreeD, TreeD);

       
 
tTreeVisitorI
    forBud();
    forFlat(FruitD, TreeD);
    forSplit(TreeD, TreeD);
        tSubstV implements tTreeVisitorI
            FruitD
            FruitD
            tSubstV(FruitD, FruitD)
            forBud();
            forFlat(FruitD, TreeD);
            forSplit(TreeD, TreeD);
         iOccursV implements tTreeVisitorI
            forBud();
            forFlat(FruitD, TreeD);
            forSplit(TreeD, TreeD);
  
 
```
 
 
<h3 id="7">为了统一bTreeVisitorI 和 iTreeVisitorI   tTreeVisitorI</h3>
（三个的不同就在于返回值分别为 boolean   int   treeD)

``` java
TreeVisitorI
    forBud();
    forFlat(FruitD, TreeD);
    forSplit(TreeD, TreeD);
         IsFlatV implements TreeVisitorI
            forBud();
            forFlat(FruitD, TreeD)
            forSplit(TreeD, TreeD)
         
         
        OccursV implements TreeVisitorI
            FruitD
            OccursV(FruitD)
            forBud();
            forFlat(FruitD, TreeD)
            forSplit(TreeD, TreeD)
         
         
        class OccursV implements TreeVisitorI {
            FruitD a;
            OccursV(FruitD _a) {
                a = _a;
            }
            public Object forBud() {
                return new Integer(0);
            }
            public Object forFlat(FruitD f, TreeD t) {
                if (f.equals(a))
                    return new Integer(((Integer)(t.accept(this))).intValue() + 1);
                else
                    return t.accept(this);
            }
            public int forSplit(TreeD l, TreeD r) {
                return new Integer(((Integer)(l.accept(this))).intValue()
                                   +
                                   ((Integer)(r.accept(this))).intValue());
            }
        }


TreeD
    Accept(TreeVisitorI)
        Bub 芽
            Accept(TreeVisitorI)
        Flat 平顶
            FruitD
            TreeD
            Flat(FruitD, TreeD)
            Accept(TreeVisitorI)
        Split 分枝
            TreeD
            TreeD
            Split(TreeD, TreeD)
            Accept(tTreeVisitorI)
         


PieD
    RemAV raFn
    RemFish rfFn
    remA()a;
    remFish(FishD)a;
        Bot
            remA();
            remFish(FishD);
        Top
            Object
            PieD
            Top(Object, PieD)
            remA();
            remFish(FishD);
 
FishD
    Salmon 鲑鱼
    Equals(Object);
    Anchovy 凤尾鱼
    Equals(Object);
    Tuna  金枪鱼
    Equals(Object);
 
RemAV
    forBot
    fotTop(Object, PieD)
RemFishV
    forBot(FishD)
    forTop(Object, PieD,  FishD)
     
RemIntV
    forBot(int)
    forTop(Object, pieD, Integer)
     
```
 
<h3 id="8">重新设计RemAV RemFishV  RemIntV 为RemV(用Object替换）</h3>

``` java
RemV
    forBot(Object)
    forTop(Object, pieD, Object)
PieD
    RemV
    Rem(Object)
        Bot
            Rem(Object)
        Top
            Object
            PieD
            Top(Object, PieD)
            Rem(Object)
 
SubstFishV
    forBot(FishD, FishD)
    forTop(Object , PieD, FishD, FishO)
 
SubstIntV
    forBot(Int, Int)
    forTop(Object , PieD, Int, Int)
 
SubstV
    forBot(Object, Object)
    forTop(Object , PieD, Object, Object)
     
 
``` 
 
 
<h3 id="9">然后现在把Remv 和Subst重新放入PieD</h3>
 
``` java
PieD
    Remv
    SubstV
    Rem(Object)
    Subst(Object, Object)
        Bot
            Rem(Object)
            Subst(Object,Object)
        Top
            Object
            PieD
            Top(Object,PieD)
            Rem(Object)
            Subst(Object,Object)
 

```

<h3 id="9">紧接着 我们还想着把Remv  SubstV放入参数的位置</h3>

``` java
PieD
    Rem(Remv,Object)
    Subst(SubstV,Object, Object)
        Bot
            Rem(Renv,Object)
            Subst(Substv,Object,Object)
        Top
            Object
            PieD
            Top(Object,PieD)
            Rem(RemV,Object)
            Subst(Substv,Object,Object)
```

<h3 id="10">紧接着我们进一步比较RemV SubstV的实现(我们就得重新修改rem和subst了)</h3>


``` java

class RemV {
    Object o;
    RemV(Object _o) {
        o = _o;
    }
    PieD forBot(Object o){
        return new Bot();
    }
    PieD forTop(Object t, PieD r){
        if (o.equals(t))
            return r.rem(this);
        else
            return new Top(t, r.rem(this));
    }


class RemV {
    PieD forBot(Object o) {
        return new Bot();
    }
    PieD forTop(Object t, PieD r, Object o) {
        if (o.equals(t))
            return r.rem(o);
        else
            return new Top(t, r.rem(o));
    }
}

class SubstV {
    PieD forBot(Object n, Object o) {
        return new Bot();
    }
    PieD forTop (Object t, PieD r, Object n, Object o) {
        if (o.equals(t))
            return new Top(n, r.subst(n, o));
        else
            return new Top(t, r.subst(n, 0));
    }
}
class SubstV {
    Object n;
    Object o;
    SubstV(Object _n, Object _o){
        n = _n;
        o = _o;
    }
    PieD forBot(Object n, Object o){
            return new Bot();
        }
    PieD forTop(Object t, PieD r){
        if (o.equals(t))
            return new Top(n, r.subst(this));
        else
            return new Top(t, r.subst(this));
    }
}

 
``` 
 
 
<h3 id="11">紧接着我们可以进行下一步抽象</h3>
 
 
``` java
PieD
    Rem(RemV)
    Subst(SubstV)
        Top
            Object
            PieD
            Top(Object, PieD)
            Rem(RemV)
            Subst(SubstV)
        Bot
            Object
            PieD
            Bot(Object, PieD)
            Rem(RemV)
            Subst(SubstV)
 
 
``` 
 
<h3 id="12">紧接着我们发现rem和subst代码类似</h3>
 
``` java
PieVisitorI
    forBot
    forTop
        RemV implement PieVisitorI
            Object
            RemV(Object)
            forBot();
            forTop(Object,PieD）
        SubstV implement PieVisitorI
            Object
            Object
            SubstV(Object, Object)
            forBot();
            forTop(Object,PieD）
        LtdSubstV implement PieVisitorI
            int
            Object
            Object
            LtdSubstV(int, Object, Object)
            forBot();
            forTop(Object,PieD）
         
PieD
    Accept(PieVisitorI)
        Bob
            Accept(pieVisitorI)
        Top
            Object
            PieD
            Top(Object,PieD)
            Accept(PieVisitorI)
 


ExprVisitorI

ExprVisitorI
    forPlus(ExprD, ExprD);
    forDiff(ExprD, ExprD);
    forProd(ExprD, ExprD);
    forConst(ExprD, ExprD);
        IntEvalV implements  ExprVisitorI
            forPlus(ExprD, ExprD);
            forDiff(ExprD, ExprD);
            forProd(ExprD, ExprD);
            forConst(Object);
            Plus(Object, Object)
            Diff(Object, Object)
            Prod(Object, Object)
 

ExprD
    Accept(ExprVisitorI);
        Plus
            ExprD
            ExprD
            Plus(ExprD, ExprD);
            Accept(ExprVisitorI);
        Diff
            ExprD
            ExprD
            Diff(ExprD, ExprD);
            Accept(ExprVisitorI);
        Prod
            ExprD
            ExprD
            Prod(ExprD, ExprD);
            Accept(ExprVisitorI);
        ConstD
            Object
            ConstD(Object);
            Accept(ExprVisitorI);
 
```
<h3 id="13">然后再实现Set集合的类型</h3>

``` java
SetD 
    Add(integer i);
    Mem(integer i);
    Plus(SetD);
    Diff(SetD);
    Prod(SetD);
        Empty
            Mem(integer i);
            Plus(SetD);
            Diff(SetD);
            Prod(SetD);
         
        Add
            Integer;
            SetD;
            Add(Integer, Integer)
            Mem(integer i);
            Plus(SetD);
            Diff(SetD);
            Prod(SetD);
 
ExprVisitorI
    forPlus(ExprD, ExprD);
    forDiff(ExprD, ExprD);
    forProd(ExprD, ExprD);
    forConst(ExprD, ExprD);
        IntEvalV implements  ExprVisitorI
            forPlus(ExprD, ExprD);
            forDiff(ExprD, ExprD);
            forProd(ExprD, ExprD);
            forConst(Object);
            Plus(Object, Object)
            Diff(Object, Object)
            Prod(Object, Object)
                SetEvalV implements IntEvalV
                    Plus(Object, Object)
                    Diff(Object, Object)
                    Prod(Object, Object)
                 
``` 
<h3 id="14">SetEvalV 直接集成IntEvalV不合理？</h3>

从SetEvalV 和IntEvalV抽取出一个基类
 
``` java
EvalD implements ExprVisitorI
    forPlus(ExprD, ExprD);
    forDiff(ExprD, ExprD);
    forProd(ExprD, ExprD);
    forConst(ExprD, ExprD);
    Plus(Object, Object)
    Diff(Object, Object)
    Prod(Object, Object)
        IntEvalD
            Plus(Object, 	Object)
            Diff(Object, Object)
            Prod(Object, Object)
            SetEvalD
            Plus(Object, Object)
            Diff(Object, Object)
            Prod(Object, Object)
         
```
 
<h3 id="15">IntEvalD 和SetEvalD很多相似之处</h3>
于是我们进一步提取

``` java
SubstD implements pieVisitorI
    Object
    Object
    SubstD(Object, Object)
    forBot();
    forTop(Object,PieD)
SubstV
    SubstV(Object, Object)
    forTop(Object, Object)
LtdSubstV
    int
    LtdSubstV(int,Object, Object)
    forTop(Object, Object)
``` 
 
 
<h3 id="16">根据extends使用override增加函数的丰富性</h3>
 
``` java
SubstV implements PieVisitorI
    Object
    Object
    SubstV(Object, Object)
    forBot()
    forTop(Object, PieD)
 
LtdSubstV
    Int
    Object
    Object
    LtdSubstV(int, Object, Object)
    forTop(Object, PieD)

```

原来，抽象类可以有构造方法.抽象类只要有一个abstract函数就可以叫做抽象类
抽象类可以有构造方法，构造方法不可继承，但是可以供子类用super（）或者super（参数，参数。。。。）调用。
构造函数是对象的基本，没有构造函数就没有对象。 
若果在父类中（这里就是你的抽象类）中显示的写了又参数的构造函数，在子类继承是就必须写一个构造函数来调用父类的构造函数。 


``` java
PointD
PointD(chapter1,2) 点
    distanceTo0(int,int); //可以把它们移入到构造函数中
    closerTo0();
    PointD minus(PointD)
    int moveBy(int ,int)
        CartesianPt 笛卡尔坐标
            Int
            Int
            CartesianPt(int,int);
            distanceTo0();
            Closeto0();
        ManhattenPt 曼哈顿坐标
            Int
            Int
            ManhattenPt(int,int);
            distanceTo0();
            Closeto0();
        圆柱坐标系
        球坐标系



ShadowedCartesia 
    Int
    Int
    ShadowedCartesia(int, int, int, int)
    distanceTo0()
 
shapeVisitorI
    forCircle(int)
    forSquare(int)
    forTrans(pointD, ShapeD)
    UnionVisitorI
    forUnion(ShapeD, ShapeD)
        HasPtV implements ShapeVisitorI
            PointD
            HasPt(PointD)
            forCircle(int)
            forSquare(int)
            forTrans(PointD, ShapeD)
        UnionHasPtV implements ShapeVisitorI
            UnionHasPtV(PointD)
            forUnion(ShapeD, ShapeD)
            return s.accept(new HasPtV(p.minus(q)));
        HasPtV implements ShapeVisitorI
            PointD
            HasPt(PointD)
            ShapeVisitorI newHasPt(PointD)
            forCircle(int)
            forSquare(int)
            forTrans(PointD, ShapeD)
        UnionHasPtV implements unionVisitorI
            UnionHasPtV(PointD)
            ShapeVisitorI newHasPt(PointD)_o
            forUnion(ShapeD, ShapeD)
            return s.accept(newHasPtV(p.minus(q)));
 
 
ShapeD
    Accept(ShapeVisitorI)
        Circle
            Int
            Circle(int)
            Accept(ShapeVisitorI)_o
        Square
            Int
            Square(int)
            Accept(ShapeVisitorI)_o
        Trans
            PointD
            ShapeD
            Trans(int)
            Accept(ShapeVisitorI)_o
        Union
            ShapeD
            ShapeD
            Union(ShapeD, ShapeD)
            Accetp(ShapeVisitorI)_o
         
``` 
 
<h3 id="16">newHasPt和HasPtV are eta reduction</h3>

``` java
PieManI
    Int addTop(Object)
    Int remTop(Object)
    Int substTop(Object,Object)
    Int occTop(Object)
 
PieManM implements PieManI
    PieD
    addTop(Object) _o
    remTop(Object)_o
    substTop(Object,Object)_o
    occTop(Object)
 
 
PieVisitorI
    forBot();
    forTop(Object, Object)
        OccursV
            Object
            OccursV(Object)
            forBot()_o
            forTop(Object,PieD)_o
        SubstV
            Object
            Object
            SubstV(Object, Object)
            forBot();
            forTop(Object,PieD)_o
        RemV
            Object
            RemV(Object)
            forBot(Object)
            forTop(Object,PieD)
     
 
PieD
    Accept(PieVisitorI)
        Bot
            Accept(PieVisitorI)_o
        Top
            Object
            PieD
            Top(Object, PieD)
            Accept(PieVisitorI)_o
 
PieVisitorI
    forBot(Bot)
    foTop(Top)
        OccursV
            Object
            OccursV(Object)
            forBot(Bot that)
            forTop(Top that)
        SubstV
            Object
            Object
            SubstV(Object,Object)
            forBot(Bot that)
            forTop(Top That)
        RemV
            Object
            RemV(Object)
            forBot(Bot that)
            fotTop(Top that)
 
PieD
    Accept(PieVisitorI)
        Bot
            Accept(PieVisitorI)_o
        Top
            Object
            Object
            Top(Object, Objct)
            Accept(PieVisitor)_o
 
``` 
 
**每天你只能不断去思考 事情的要点和第一步和第二步 才能取得进步.**

What’s the point of the visitor pattern? What’s the point of the software design?

+ 亮点
+ 突破点，并按照一定的方向。。
+ 关键点
 
![closure][4]

**Closure(Closure(this))**

具体java源代码参考[the little java][1]

[1]: https://github.com/jueqingsizhe66/ALittleJava
[2]: http://www.ccs.neu.edu/home/matthias/BALJ/ 
[3]: /images/alittlejava/duichen.png
[4]: /images/alittlejava/closure.png
