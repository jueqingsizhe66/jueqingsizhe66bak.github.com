---
layout: post
title: "fortran宫殿"
date: 2015-08-09 15:06:38 +0800
comments: true
categories: fortran
---

任何语言应该都可以有类似宫殿的想法，在纵横交错的街道中，能过找到进口和出口，
并可以使用很多条道路完成你想去的地方，而不是拘泥于一条道路。Fortran应该也是如此。
诞生于1951年左右的第一个面向对象的高级语言Fortran 语言，也已经65岁左右了，它以其
快速有效的科学数值计算，一直存在与科学研究领域中。不想过多赘述，只想自己谈谈对于
fortran的理解----宫殿式的介绍.
<!--more-->

## 宫殿

  宫殿蕴含着纵横交错，阡陌交通，但是确实零落有序，复合规律。通过宫殿群来认识这门
  科学语言。
  ![宫殿][1]
## fortran

### 进口
   程序在开始设计的时候，需要变量初始化，也就是进行进口的布置。![input][2]
### 数组arrays and allocatable
   有时候我们发现一条街到应该相同的样式，于是我们采用数组把具有相同的
  数据形成放进一个一个的房屋。![arrays][2]
  fortran的dimension是相当常见的形式，因为他就是数组的体现。
  一般房屋的建造都是需要allocatable的属性，也就是政府指定的可用地，利用allocate建房，使用完，废弃了，就deallocate。![zhengdi][8]

### subroutine and function
   房屋一多我们发现没法管理，于是我们就把很多的房屋放在对应的街道中去,也就是
subroutine或者function当中。![街道][7]

   另外有时候有一些已经在外建的subroutine和function一定得把他们的头和参数类型和end部分提交到program（政府）当中的INTERFACE当中，这样才能确定这块地的合法性等。这也是为什么fortran中会有很多的额外的子程序的定义（和主文件不在同一个文件夹当中），为了让主文件能够识别，使用INTERFACE把它嵌入进来。![interface][9]

### 升级版的module
  为了管理和维护房间(arrays)和街道(subs)，于是我们把他们放在一个一个区域当中。![module][4]

### 出口
   当我们绕过了整个宫殿后，还需要布置一个或多个的出口。 ![output][5]

### 然而我们发现我们需要更大的进口说明

   有时候我们进一步组装多个输入文件，而为了方便，我们把他们组装成一个更大的进口装置，放置在
   最前面。![square][6]

## 后续
   无论模块化，还是函数化，亦或者模式化，都是基于原先大的过程式变成的改成，
   只不过是为了减少冗余、重复，同时简化和方便阅读，另外还有就是方便拓展。
   *从现在开始可以建立你自己的fortranlibrary modal*


   fortran2003出来了个type这个数据结构的创建，方便的把多个变量或者数组组合成为一种数据类型，这就有点像3D打印的功能：
   ```
  type creat_a
    real :: a
    integer :: b
    real,allocatable :: c(:)
  end type creat_a
  
  type(creat_a) :: creat_b,creat_c
  
  creat_b%a = 1.0
  creat_b%b = 2
  allocate (creat_b%c(3))
  creat_b%c = 6.0
  
  creat_c = creat_b
  creat_c%b = 8

   ```

   type的作用类似Hashkell的data的作用，没有行为只有数据类型。
   先开个头…… 




[1]: /images/fortran/palace.jpg
[2]: /images/fortran/wumen.jpg
[3]: /images/fortran/subroutine.jpg
[4]: /images/fortran/module.png
[5]: /images/fortran/shenwu.jpg
[6]: /images/fortran/square.jpg
[7]: /images/fortran/street.png
[8]: /images/fortran/zhengdi.jpg
[9]: /images/fortran/zhai.jpg

