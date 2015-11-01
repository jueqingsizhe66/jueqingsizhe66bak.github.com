---
layout: post
title: "FAST In Ubuntu64Bit 编译注意事项"
date: 2015-10-22 23:38:33 +0800
comments: true
categories: Fast
---

[FAST(Fatigue,Aerodynamic,Struture,Turbulence)][1],是一款风力机的气动、水动力、
励磁、结构等几个部分的模拟[工具][3]

风力机系统的整体带入流风物理流动现象:

![Physical phenomena affecting a floating wind turbine system][4]


而本文主要讲的是如何在ubuntu64bit系统编译FAST使其可以使用。
<!--more-->
## 1 操作系统的位数
    找到Compiling文件夹下的makefile，然后将bits=32 改为64
## 2.register.exe编译
  首先编译Source/dependecy/Registry 的makefile，然后生成一个register.exe 供给fast makefile使用
## 3. MAP++生成libmap.so
  从官网https://nwtc.nrel.gov/MAP 下载map源文件，然后编译 生成一个libmap.so
 注意： 必须是 lapack developer版本才可以， 

```
 apt-get install lapack*
```
  并把so文件放到FAST目录下的bin文件夹（没有则创建），然后make fast的makefile即可。

## 4 DISCON_DLL编译使得NREL 5MW也能运行
   
   之前的三个步骤,在bin目录生成的FAST_glint64只能运行Test1-18.fst对应的机型.

如果想运行Nrel-5MW（Test18-26.fst)必须是编译makefile_DISCON_DLL,并修改
  CertTest/5MW_Baseline/ServDyn 的CertTest/5MW_Baseline/NRELOffshrBsline5MW_Onshore_ServoDyn.dat 更改对应的**DLL_FileName** 为 *ServoData/DISCON_glin64.so* 

### 4.1 编译方式

```
    make -f makefile_DISCON_DLL
```

### 4.2注意有可能编译不通过：
   必须在makefile_DISCON_DLL中的  FFLAGS加入  -fPIC

```
FFLAGS  = -O2 -m$(BITS) -fbacktrace -ffree-line-length-none -x f95-cpp-input -fPIC -C

```
针对与Fast开发者可以参考[FAST-Developers][2]


[1]: https://nwtc.nrel.gov/FAST8 
[2]: https://nwtc.nrel.gov/FAST-Developers 
[3]: http://www.nrel.gov/docs/fy13osti/57228.pdf 
[4]: /images/fast/WTSystem.png
