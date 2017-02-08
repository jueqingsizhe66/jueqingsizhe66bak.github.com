
一张来自[fcode][2]的有趣的静态和动态链接的比喻图，
![staticdy][1]

静态库是需要直接和exe文件打包在一起的，而动态库则是需要运行的时候才执行dll文件，
dll文件也是一个exe文件，只不过只能由对应的exe文件执行，无法人工执行。

+ [1. 静态链接](#static)
+ [2. 动态链接](#dynamic)
+ [3. 两者比较](#compare)
+ [4. fortran与c混编](#hun)

<!--more-->


<h2 id="static">静态链接</h2>

静态库lib,实际上是obj文件的集合，可认为是打包在一起的若干obj，

因此他的编译过程为：

1.  <font color="red">编译子程序(f90)源代码，得到若干obj文件</font>
2.  <font color="red">打包这些obj文件，成为**lib静态库**</font>

他的使用比较简单：

<font color="red">编译主程序(或其他子程序),链接时，**带上lib**文件即可</font>。

<hr/>

<font color="red">**注意**</font><br/>
在上面的f90源代码中可能包含了module片段。若干静态库中使用了module,那么编译还会产生mod内件。
<font color="green">每一个module对应一个同名的mod文件。</font>

mod是什么？
mod文件可认为是module的"概述",与c语言的"头文件"的做哟个相似，但与C语言需要自己书写头文件不同，
fortran的mod文件由编译器<font color="red">**自动生成**</font>。

也就是说，当`use Module`时，编译器需**能够**找到对应的mod文件。因此有必要把静态库里产生的mod文件
保存起来，以便使用静态库时加载(<font color="red">**mod文件一定得在lib目录下,这点很容易被遗忘。**</font>。

<hr/>

<font color="green">提前准备公用的源代码:</font>

sub.90:
``` fortran
Subroutine sub()
  write(*,*) 'Xueqiu is a good man!'
End Subroutine sub

```

func.f90:

```
MODULE FUNCMOD
  IMPLICIT NONE

CONTAINS

  INTEGER FUNCTION FUNC()
    WRITE(*,*) 'THIS IS A FUNC!'
    FUNC = 1
  END FUNCTION FUNC
END MODULE FUNCMOD

```

main.f90:

```
Program testLib
  use funcmod
  Implicit None
  integer :: i
  i = func()
  call sub()
End Program testLib

```
<hr/>
<hr/>
<hr/>
<h3> gfortran技术解决方案</h3>

+ 第一步: 编译生成obj[和mod] 文件,

```
gfortran -c sub.f90 func.90
```

生成文件列表如下:
```
funcmod.mod
func.o
sub.o
```

+ 第二步: 然后再打包obj文件到lib文件

```
ar rv my.lib func.o sub.o
```
然后你现在就可以删掉func.o sub.o了(`rm -rf func.o sub.o`),
因为他们包含在my.lib中.

+ 第三步: 编译主文件，

```
gfortran -c main.f90
gfortran main.o my.lib -o main.exe
```
这样就会生成对应的main.exe了。

<hr/>
<hr/>
<hr/>

<h3> vs2015+ivf2017的技术解决方案:</h3>

+ 第一步:

新建一个static library项目,并添加sub.f90,func.f90到source文件夹下。

![static][3] 

+ 第二步: 生成解决方案

![static2][4]
这样就在debug目录下得到\*.lib文件

注意：链接需要二个文件\*.obj
和\*.mod文件，而myfirstLib.lib仅仅是把\*.obj打包了一下，所以
还得再ivf的<font color="red">**Additional Include**</font>中添加mod文件路径

+ 第三步:

新建一个testLib  console(/subsystem:console )的项目 用于测试myfirstlib

![test][5]
注意由于在一个解决方案中存在多个项目所以得把test(包含主程序)设置为<font color="red">启动项目</font>。
![启动][6]

+ 第四步:

编译，生成解决方案。<font color="red">**报错了**</font><br>。
![error][7]
原因是没有找到\*.o(func.o)包含的mod文件，解决方法是<font color="red">添加mod路径到additional include directories </font>
![correct][8]

还有一个问题，我一般还需要<font color="red">生成的lib文件放在source目录下</font>，才可以编译成功，这点也需要小心。

<font color="red">**总结**</font>
设置lib路径的时候，在存在mod文件时候，一定得一同存在，否则未可能报符号丢失的错误。

<hr/>
<hr/>
<hr/>

<h2 id="dynamic"> 动态链接</h2>

动态库DLL,实际上也是可执行文件，与exe本质上是一样的。只不过dll通常
没有主程序而已（自己不能运行，只能由对应的主程序运行),他必须由exe在运行以后调用。

DLL的好处?
具有良好的可维护性，节约内存等特点。在windows(dll)，linux(so),mac等操作系统上，都
大量使用了DLL动态库。

动态链接库dll的编译过程是:

1. 编译子程序源代码，得到若干obj文件。
2. 链接这些obj文件，得到dll文件（有可能带lib文件，如果是静态加载则无lib，如果是动态加载则有lib）
3. 部分平台的编译器会得到导入库lib

他的使用由两种方法(显示调用(动态),隐式调用(静态)):

1. 编译主程序(或其他子程序),链接时，带上导入库lib文件。
2. 动态的利用系统函数加载DLL,如windows下的LoadLibrary和GetProcAddress。

<hr/>
<hr/>

提前准备好公用的fortran源代码：

<font color="red">sub.f90:</font>
``` fortran
Subroutine sub()
  !DEC$ ATTRIBUTES DLLEXPORT,ALIAS:"sub"::sub
  write(*,*) 'Xueqiu is a good man!'
End Subroutine sub

```
__注意，!DEC$ ATTRIBUTES DLLEXPORT, ALIAS:"sub"::sub，是有用的exe间的符号对接。__

<font color="red">func.f90:</font>

``` fortran

Module funcmod
  Implicit none

contains

  Integer Function func()
    !DEC$ ATTRIBUTES DLLEXPORT,ALIAS:"func"::func
    write(*,*) 'This is a func!'
    func = 1
  End Function func
End Module funcmod

```

__注意，!DEC$ ATTRIBUTES DLLEXPORT, ALIAS:"func"::func，是有用的exe间的符号对接。__

<font color="red">main.f90:</font>>

``` fortran
Program testDll
  use funcmod
  Implicit None
  integer :: i
  i = func()
  call sub()
End Program testDll

```

<h3> gfortran的技术解决方案:</h3>

+ 第一步: 生成so文件(linux下面)

```
gfotran sub.f90 func.f90 -shared -fPIC -o libmy.so
```
+ 第二步: 直接引用即可
```
gfortran main.f90 libmy.so -o main.exe
```

+ 第三步:直接运行main.exe
有可能报错，在linux下可能需要 `export LD_LIBRARY_PATH=./`
也就是把当前目录也当作查找so文件的一个地方。

<h3> vs2015+ivf2017的技术解决方案:</h3>

+ 第一步：新建一个Dll project
然后类似静态的方法，把func.f90,sub.f90当作一个项目，再新建一个testDll项目，并添加lib等，也要include进来目录
比如，
![main][11]再次基础上，在添加include路径（把mod文件添加进来）
![include][12]

在运行的时候有可能出现运行错误![error][13],这时候需要在Working Directory添加
运行dll路径，
![dll][14],


<h2 id="compare"> 动态链接的静态加载和动态加载</h2>

<font color="blue">总之，静态库lib和动态库dll，是很多第三方库的主要形式。因此他们是学习第三方
函数库的基础知识，也是学习多种语言混编的基础。</font>


+ 静态加载和动态加载的区别:

- 静态加载:不依赖ifort的运行库（可以到没有安装ifort的机器 上运行dll文件）  dll文件比较大
- dll加载 :则会依赖ifort的运行库（dll比较小，但是如果没有ifort运行库 则会报dll错误（所以一般拷贝到别的机子的dll都是静态加载的
可采用depends walker这个软件，查看内部的dll信息。

而我们可以用一张十分perfect的比较图，看出动态链接库的两种加载方式的不同，以及动态和静态的区别(10元)
![dynamicstatic][15]

注意动态函数(subroutine)比静态多了一行`!DEC$ ATTRIBUTE DLLEXPORT,ALIAS:...`

+ 动态加载例子
![动态加载][16]

```  fortran
Module LoadDLL
  Implicit None
  Interface
    FUNCTION GetProcAddress( hModule, lpProcName)
      !DEC$ ATTRIBUTES DEFAULT, STDCALL, DECORATE, ALIAS:'GetProcAddress' :: GetProcAddress
      use ISO_C_Binding
      type( C_FUNPTR ) :: GetProcAddress
      integer hModule
      character(C_CHAR) , dimension(*) :: lpProcName
    END FUNCTION GetProcAddress
    
    Integer Function F_func()
    End Function F_func
    
    Subroutine F_Sub()
    End Subroutine F_Sub
  End Interface
  
End Module LoadDLL
  
Program www_fcode_cn
  use LoadDLL
  use ISO_C_Binding
  use Kernel32 , only : LoadLibrary
  Implicit None
  Procedure(F_func) , pointer :: func
  Procedure(F_sub ) , pointer :: sub
  type( C_FUNPTR ) :: c_pointer
  integer :: hDLL , i
  character(len=32) :: dllname , funcname
  Do
    write(*,*) '您要调用哪个DLL？'
    read(*,*) dllname
    hDLL = LoadLibrary( trim(dllname)//c_null_char )
    if ( hDLL <= 0 ) then
      write(*,*) '对不起，我好像无法找到 '//trim(dllname)//' 哦~~！^_^'
    else
      exit
    end if
  End Do
  Do
    write(*,*) '您要调用哪个函数？'
    read(*,*) funcname
    if ( funcname=="quit" ) exit
    c_pointer = GetProcAddress( hDLL , trim(funcname)//c_null_char )
    if ( .not.C_ASSOCIATED(c_pointer) ) then
      write(*,*) trim(dllname)//' 里面好像没有 '//trim(funcname)//' 函数哦~~！^_^'
      cycle
    end if
    If ( funcname=="func" ) then
      call C_F_ProcPointer( c_pointer , func )
      i = func()
    Else if ( funcname=="sub" ) then
      call C_F_ProcPointer( c_pointer , sub )
      call sub()
    End If
  End Do  
End Program www_fcode_cn

```

<h2 id="hun"> fortran与c混编</h2>

源于此:[http://bbs.fcode.cn/thread-1117-1-1.html][10] 

![混编主界面][17]
混编时候注意两点:

1. c 和 fortran 混编不需要 include
2. c/c++ 里面没有 module 

另外注意vc项目也得添加引用目录
![引用目录][18]

你也可能运行错误
![errorV][19]
注意上面红色的部分。
第一处：如果你强调必须 stdcall 协定，那么你需要在 fortran 中声明。（这个声明不是规范的，是IVF扩展的写法）
第二处：你的 C++ 里的 lens 是通过 strlen 计算出来的，而不是指针。所以 fortran 中必须加 value 修饰符，表示它是传值，而不是传址。（这是导致cpp_main.exe 中的 0x5d7005a9 (vlhm.dll) 处最可能的异常: 0xC0000005: 读取位置 0x00000021 时发生访问冲突）的原因。

最终正确结果如下:
![correctV][20]

源码信息如下:

TestFortranCpp.cpp

``` c++
// TestFortranCpp.cpp : 定义控制台应用程序的入口点。
//

#include "stdafx.h"
#include <stdio.h>
#include <string.h>

extern "C" {void _stdcall   vlhm(char *, int); }
//extern "C" {void vlhm(char *,int); }
int _tmain(int argc, _TCHAR* argv[])
{
	char *inputfile = "E:\\phase6-power.opj";
	//char *inputfile;
	//char str[] = "C:\\Users\\www\\Desktop\\\\31005700.tem";
	//inputfile=&str[0];
	vlhm(inputfile, strlen(inputfile));
	return 0;
}

```

fileNameCheck.f90:

``` fortran
subroutine vlhm(pfilename, lens) Bind(C,Name="vlhm")
!DEC$ ATTRIBUTES STDCALL,DLLEXPORT::vlhm

  use,  Intrinsic :: ISO_C_Binding
!character(len=*)::filename            !c++主程序中传递进来的“Inputfile”变量
!write(*,*)"dll里的filename值是："
!write(*,*)filename

  type(C_PTR) , value :: pfilename !c++主程序中传递进来的“Inputfile”变量，是C语言的指针
  integer, value::lens                                      !传入的字符串长度
  character(len=lens), target :: filename_a
  !character(len=lens) :: filename_a !pointer & target
  character(len=lens),pointer::filename                     !这是Fortran的字符串

  call c_f_pointer( pfilename , filename )                   !把c语言的指针转换成fortran字符串

  filename_a = filename
  write(*,*)filename
  write(*,*)filename_a

  
  write(*,*) 'fine ok'

end subroutine vlhm

```

进一步的细节资料可参考[fcode视频官网][9]。

[1]: /images/fortrandebug/usb.png
[2]:http://fcode.cn/ 
[3]: /images/fortrandebug/static/1.png
[4]: /images/fortrandebug/static/2.png
[5]: /images/fortrandebug/static/3.png
[6]: /images/fortrandebug/static/4.png
[7]: /images/fortrandebug/static/5.png
[8]: /images/fortrandebug/static/6.png
[9]:http://v.fcode.cn/video-library_dll.html 
[10]:http://bbs.fcode.cn/thread-1117-1-1.html 
[11]: /images/fortrandebug/dll/main.png
[12]: /images/fortrandebug/dll/include.png
[13]: /images/fortrandebug/dll/error.png
[14]: /images/fortrandebug/dll/dll.png
[15]: /images/fortrandebug/dynamicstatic.png
[16]: /images/fortrandebug/dll/load.png
[17]: /images/fortrandebug/hunbian/main.png
[18]: /images/fortrandebug/hunbian/vcinclude.png
[19]: /images/fortrandebug/hunbian/error.png
[20]: /images/fortrandebug/hunbian/final.png


