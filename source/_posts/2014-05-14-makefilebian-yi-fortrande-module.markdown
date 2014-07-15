---
layout: post
title: "makefile编译fortran的module"
date: 2014-05-14 13:09:20 +0800
comments: true
categories: makefile
keyword: makefile module fortran
---

<!--more-->
``` makefile makefile module compile  http://note.youdao.com/web/list?notebook=%2FWPqZjT2&sortMode=0&version=529952&note=%2FWPqZjT2%2Fweb1393566495246  
FC = gfortran
FFLAGS = -g -Wall -O3 --free-form
modFFLAGS =  --free-form

OPTFC = -O3 -funroll-loops -ftree-vectorize -fcray-pointer -cpp
#src = $(wildcard *.f90)
src = secdmo.f90 record.f90
obj = $(src:.f90=.o )
secd : $(obj)
    $(FC) $(FFLAGS) -o $@ $^

%.o : %.f90
    $(FC) $(modFFLAGS) $(OPTFC) -c $<

#secdmo.mod : secdmo.f
#	$(FC) $(modFFLAGS) @ $<

.phony: clean

clean:
    rm -rf secd secdmo.mod funct.plo accu* fort* ./errdi* *.o

```
