---
layout: post
title: "pandoc markdown注意点"
date: 2015-06-11 12:30:13 +0800
comments: true
categories: markdown
---

pandoc是markdown升级版工具，用于制作幻灯片相当好用，然而使用过程需要注意以下几点。

<!--more-->

1. markdown文件  # 大标题不能跟上文本   ## 二标题才可以跟文本，且是一张幻灯
片。
2. org文件使用 ---------来强制执行创建新的幻灯片

##  markdown
``` sh
% Ye Zhaoliang
% Openfoam Introduction
% 11th June,2015

# Openfoam简介

## 1求解器(Solver 数值结算)

  openfoam采用c++编写的类库，开源免费。所以可以把它当作一种类似于String的基本类来使用。
 openfoam框架从使用级别来看主要分为两类：

    - 流体计算
    - 化学反应
    - 换热
    - 结构动力学
    - 电磁场
    - 金融评估等

## 2工具(Utility)

    - 前处理
      + 建模
      + 网格
      + 边界条件
    - 后处理
      + 计算结果显示

## cavity case

cavity case的文件夹简介（等价于fluent的.cas,只不过fluent集成为一个文件，openfoam是一个文件夹)

# 这便使用代码快包裹
├── 0 (初始条件文件夹)
│   ├── p
│   └── U
├── constant
│   ├── polyMesh(网格文件夹)
│   │   └── blockMeshDict
│   └── transportProperties(流体物性参数 传热物性参数)
└── system
    ├── controlDict(控制字典)
    ├── fvSchemes(离散格式 更高级的运用)
    └── fvSolution(求解方法)

# 运用的三个级别[%]

## 三个级别

- [ ] 直接利用解释器(替代商业求解器 初始点)
- [ ] 自定义求解器(关注点)
      + 根据自己的求解流程组合求解器(类库调用)
      + 不需要关心对流项和扩散项是如何离散的
      + 也不需要关心湍流模型
- [ ] 更高级的运用:编写离散格式

## 自定义求解器

    按照自己的_求解流程_编写求解。

##不关心

    + 离散格式(时间项和对流项)
    + 大型代数方程的求解，如何加速等。

## 传热求解

这边使用代码快包裹
    开始
    |
    初始值
    |
    -----------------<------------------------------
    |                                              |
    用CFD进行室内温度计算                          |
    |                   |                          ^
    辐射传热计算Qri  导热计算Qcdi                  |
    |                   |                          |
    由热平衡求新的对流传热量Qcvi^(m+1)             |
    |                                              |
    收敛条件 -------->-----------------------------|
    |
    结束

## 自定义离散格式

   针对于研究离散格式和代数求解器的用户
   目的： 创造更高效更精确的离散方法
   操作： 修改finiteVolumn...
          尤其是对流项。openfoam 提供了NVD和TVD的模板（40以上）
          对流项离散依然是一种研究方向。

## cavity源码文件的主要构成部分

   +  location
     - "system"
     - "constant"
     - "0"
   + class
     - dictionary
     - volVectorField
     - volScalarField

##

   + object
     - dictionary
       + fvschemes对象
       + fvsolution对象
       + controldict对象
       + blockmeshdict
       + transportproperties
     - volVectorField
       + U对象
     - volScalarField
       + p对象



```

## org-mode


``` sh
* Openfoam简介
  openfoam采用c++编写的类库，开源免费。所以可以把它当作一种类似于String的基本类来使用。

 openfoam框架从使用级别来看主要分为两类：
----------
** 1求解器(Solver 数值结算)
    - 流体计算
    - 化学反应
    - 换热
    - 结构动力学
    - 电磁场
    - 金融评估等
** 2工具(Utility)
    - 前处理
      + 建模
      + 网格
      + 边界条件
    - 后处理
      + 计算结果显示

----------------------
** cavity case
cavity case的文件夹简介（等价于fluent的.cas,只不过fluent集成为一个文件，openfoam是一个文件夹)

#+begin_src sh
├── 0 (初始条件文件夹)
│   ├── p
│   └── U
├── constant
│   ├── polyMesh(网格文件夹)
│   │   └── blockMeshDict
│   └── transportProperties(流体物性参数 传热物性参数)
└── system
    ├── controlDict(控制字典)
    ├── fvSchemes(离散格式 更高级的运用)
    └── fvSolution(求解方法)
#+end_src 


* 运用的三个级别[%]

- [ ] 直接利用解释器(替代商业求解器 初始点)
- [ ] 自定义求解器(关注点)

      + 根据自己的求解流程组合求解器(类库调用)
      + 不需要关心对流项和扩散项是如何离散的
      + 也不需要关心湍流模型

- [ ] 更高级的运用:编写离散格式

---------
** 自定义求解器
    按照自己的_求解流程_编写求解。

** 不关心
    + 离散格式(时间项和对流项)
    + 大型代数方程的求解，如何加速等。

----------
** 传热求解
#+begin_src sh
    开始
    |
    初始值
    |
    -----------------<------------------------------
    |                                              |
    用CFD进行室内温度计算                          |
    |                   |                          ^
    辐射传热计算Qri  导热计算Qcdi                  |
    |                   |                          |
    由热平衡求新的对流传热量Qcvi^(m+1)             |
    |                                              |
    收敛条件 -------->-----------------------------|
    |
    结束
#+end_src
--------------------------------
** 自定义离散格式

   针对于研究离散格式和代数求解器的用户
   目的： 创造更高效更精确的离散方法
   操作： 修改finiteVolumn...
          尤其是对流项。openfoam 提供了NVD和TVD的模板（40以上）
          对流项离散依然是一种研究方向。

-------
** cavity源码文件的主要组成部分

   +  location
     - "system"
     - "constant"
     - "0"
  
   + class
     - dictionary
     - volVectorField
     - volScalarField
-----------
   + object
     - dictionary
       + fvschemes对象
       + fvsolution对象
       + controldict对象
       + blockmeshdict
       + transportproperties
     - volVectorField
       + U对象
     - volScalarField
       + p对象


```
