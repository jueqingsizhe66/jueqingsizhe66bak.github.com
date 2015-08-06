---
layout: post
title: "Incompact3d的Makefile及DNS算法"
date: 2015-06-22 16:52:20 +0800
comments: true
categories: incompact3d
---

[Incompact3d](https://code.google.com/p/incompact3d)是一个开源的基于fortran语言编写的DNS求解器，也是我研究生期间使用的开源源代码。
Incompact3d整体框架的介绍可以参考软件的[user-guide](https://code.google.com/p/incompact3d/downloads/detail?name=user_guide_incompact3d_V1-1.pdf&can=2&q=)和文献[High-order-compact schemes for incompressible flows: a simple and effcient method with the quasi-spectral accuary](http://www.imperial.ac.uk/media/imperial-college/research-centres-and-groups/turbulence-mixing-and-flow-control-group/2009_LAIZET_JCP.pdf),还可以搜索[sylvain Laziet](http://www.imperial.ac.uk/tmfc) 相关的文章 ，只不过他并不是成熟的软件，很多的编译和后处理都可能出现问题，下面是我使用过程遇到的一些问题。
当然主要过程是，通过makefile编译，然后运行incompact3d，最后处理计算结果。
<!--more-->

#1 如何模拟？

以周期性槽道流动(当然我也就会这么一种，其他都只是走了一小半）为例。

1. 首先是得到(不得不说，我也有点忘记)一个完全发展的旋转槽道流动(注意旋转源项的添加) (一般设置20000步可以了，只要收敛即可 大概一天)
2. 然后是获得完全发展槽道流动(也就是不加上旋转源项)(估计得三万步以上 大概一天)
3. 最后是获得统计的完全发展槽道流动.因为DNS获得的结果是非定常的，所以你得进行时均处理(当然时均程序得打开时均统计项 umean等)(我统计了20万步，大概花了4天时间。)

步骤就是这样,大概一个较小的流程需要一周左右。


Incompact3d比较特殊的是收敛性判定问题，上面的几个过程都需要使用实时[监控小程序](http://jueqingsizhe66.github.io/blog/2015/05/31/u-plus-y-plus/)
来观看速度散度和质量流量(质量流量是周期性槽道流动所特有的，所以最好进行实时显示)的收敛情况.另外你可以通过python的一个小脚本
来获得实时显示的图片，这样就能看到程序模拟得怎么样了，具体查看[python局域网上传和下载](http://jueqingsizhe66.github.io/blog/2015/06/11/ji-yu-pythonde-ju-yu-wang-wen-jian-gong-xiang-ruan-jian-simplehttpserverwithupload/).
#2 后处理方法
参考我的[pathline处理方法]( http://jueqingsizhe66.github.io/blog/2015/06/22/incompact3dru-he-tong-guo-jie-guo-chu-li-chu-ji-xian )
一定要注意(real 8) 否则得到的结果肯定是错误的，这也是困扰我几星期的问题。

#3 关于Makefile编译

``` makefile
#=======================================================================
# Makefile for Imcompact3D
#=======================================================================

# Choose pre-processing options
#   -DSHM	   - enable shared-memory implementation
#   -DDOUBLE_PREC  - use double-precision
OPTIONS = -DDOUBLE_PREC

# Choose an FFT engine, available options are:
#   essl       - IBM Blue Gene ESSL Library
#   fftw3      - FFTW version 3.x
#   generic    - A general FFT algorithm (no 3rd-party library needed)
#FFT= essl # I ignore
FFT = generic

# Paths to FFTW 3
FFTW3_PATH=   # full path of FFTW installation if using fftw3 engine above
FFTW3_INCLUDE = -I$(FFTW3_PATH)/include
FFTW3_LIB = -L$(FFTW3_PATH)/lib -lfftw3 -lfftw3f

# Paths to ESSL
ESSL_PATH=/bgsys/drivers/ppcfloor/comm/xl
ESSL_INCLUDE =
ESSL_LIB = -L$(ESSL_PATH)/lib -L/opt/ibmmath/lib64 -lesslbg

# Specify Fortran and C compiler names and flags here
# Normally, use MPI wrappers rather than compilers themselves 
# Supply a Fortran pre-processing flag together with optimisation level flags
# Some examples are given below:

#FC =  
#OPTFC = 
#CC = 
#CFLAGS = 

# PGI
#FC = ftn
#OPTFC = -fast -O3 -Mpreprocess
#CC = cc
#CFLAGS = -O3

# PathScale
#FC = ftn
#OPTFC = -Ofast -cpp
#CC = cc
#CFLAGS = -O3

# GNU 选用mpif90进行编译
FC = mpif90
OPTFC = -O0 -g  -fdefault-real-8 -fdefault-double-8 -funroll-loops -ftree-vectorize -fcray-pointer -cpp
CC = mpicc
CFLAGS = -O0
PLATFORM=gnu

#Blue Gene/Q : EDF R&D
#PREP=/bgsys/drivers/ppcfloor/comm/xl/bin/
#FC = $(PREP)mpixlf95_r
#OPTFC= -O3 -qsuffix=cpp=f90 -qinitauto -qautodbl=dbl4
##OPT_LK= -O3 -qinitauto -qautodbl=dbl4
#CFLAGS= -O3 -qinitauto -qautodbl=dbl4
#CC=$(PREP)mpixlc_r
#PLATFORM=bgq_xlf

# Cray
#FC = ftn
#OPTFC = -e Fm
#CC = cc
#CFLAGS = 

#-----------------------------------------------------------------------
# Normally no need to change anything below

# include PATH 
ifeq ($(FFT),generic)
  INC=
else ifeq ($(FFT),fftw3)
  INC=$(FFTW3_INCLUDE)
else ifeq ($(FFT),essl)
  INC=$(ESSL_INCLUDE)
endif

# library path
ifeq ($(FFT),generic)
   LIBFFT=
else ifeq ($(FFT),fftw3)
   LIBFFT=$(FFTW3_LIB)
else ifeq ($(FFT),essl)
   LIBFFT=$(ESSL_LIB)
endif

# List of source files
# 注意这边编译的模块，一般是需要的mod生成放在前面首先编译，如果不放前面会报错，解决办法 就是文件名放在前面即可,具体可以查看关于Makefile Fortran
SRC = decomp_2d.f90 glassman.f90 fft_$(FFT).f90 module_param.f90 io.f90 variables.f90 poisson.f90 schemes.f90 implicit.f90 convdiff.f90 user_module.f90 incompact3d.f90 navier.f90 derive.f90 parameters.f90 tools.f90 visu.f90

#-----------------------------------------------------------------------
# Normally no need to change anything below

ifneq (,$(findstring DSHM,$(OPTIONS)))
SRC := FreeIPC.f90 $(SRC)  
OBJ =	$(SRC:.f90=.o) alloc_shm.o FreeIPC_c.o
else
OBJ =	$(SRC:.f90=.o)
endif	

OPTION=$(OPTIONS)
from:=-D
to:=-WF,-D
TMP=$(subst $(from),$(to),$(OPTIONS))
ifeq ($(PLATFORM),bgp_xlf)
   OPTION=$(TMP)
endif
ifeq ($(PLATFORM),bgq_xlf)
   OPTION=$(TMP)
endif

all: incompact3d

alloc_shm.o: alloc_shm.c
	$(CC) $(CFLAGS) -c $<

FreeIPC_c.o: FreeIPC_c.c
	$(CC) $(CFLAGS) -c $<

incompact3d : $(OBJ)
	$(FC) -O0 -g -o $@ $(OBJ) $(LIBFFT)

%.o : %.f90
	$(FC) $(OPTFC) $(OPTION) $(INC) -c $<

.PHONY: clean 
clean:
	rm -f *~ *.o *.mod incompact3d

.PHONY: realclean
realclean: clean
	rm -f *~ \#*\#
```

运行方式:
我的电脑刚好是8线程，就用8线程运行，你也可以选用4或者更多.

``` sh
mpirun -np 8  incompact3d  > tail.out &
```
#4 The Algorithm of the DNS in Incompact3d

Incompact3d的执行流程基本上是下面几个(当然得仔细阅读，并反复比对).我写得这段英文大体能够对得上.

``` sh
1. Initial the velocity field with noise(init subroutine).
2. Start the iterative process by guessing the pressure field. First we use convdiff subroutine take convection and diffusion of the flow into consideration.And then use pre_correc subroutine to correction the velocity value with the specified boundary condition.
3. Use the values of u,v,and w to get the initialize of the pp3 in the spectral space from subroutine divergence(…pp3,1…)(first sign to turbulent spot).Then we use poisson solver decomp_2d_poisson_stag to get the value of the pp3 in the spectral space.
4. Since they were obtained from the guessed values of u,v,w,the values pp3,when substituted into the divergence equation,will not necessarily satisfy that that equation.Hence ,using the gradp subroutine,get the pressure gradients in the physics space,then using corgp subroutine ,get the velocity correction by the pressure gradient in the physics space.Use subroutine divergence(…dv3,2…) to do another monitor for turbulent spot(second sign to turbulent spot) .At the end of the current step, we go to step2 again until the simulation have been fully developed.
```
