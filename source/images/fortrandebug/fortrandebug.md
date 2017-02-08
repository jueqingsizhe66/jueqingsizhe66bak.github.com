[NREL FAST][3]的调试一直存在问题，关键是不知道怎么设置选项，查阅了Visual Studio和一些基本知识整理了
NREL项目的调试方法。

<!--more-->

## fortran工作路径问题

<font color="green">__工作路径对于任何的可执行文件都很重要。__</font>
    当然事先可能得了解一下软件编译链接的过程（即编写源代码--调用编译器编译源代码--
    >调用链接器连接相关代码块生成可执行文件-->运行可执行文件）(IDE隐藏了上述过程)


+ 如果不带任何路径，则被认为是当前路径(<font color="green">**working directory**</font>)。
+ 如果带相对路径，则以当前路径为基准。
+ 通过命令行执行，当前路径一般在提示符上(或<font color="red">pwd</font>命令查看)
+ 直接<font color="red">**双击**</font>,当前路径在exe所在的文件夹。
+ 通过<font color="red">**IDE**</font>方式运行，当前路径一般在<font color="red">**工程所在文件夹**</font>
+ 一些IDE还允许设置当前路径(只对IDE方式运行有效)。
+ 右键exe名字也是可以指定运行路径的（这样就可以加载对应的配置文件)

比如:<br/>

![IDE working directory][1]<br/>

## 代码层级改变文件路径

在程序中可以通过一些手段，动态的更改当前路径。

+ IVF: changeDirQQ
+ gfortran: ChDir

也可以通过一些手段，获得当前路径，

+ IVF/gforran: GetCWD

``` fortran

program testDir
    Implicit none
    CHARACTER(len=512) ::  working_path
    call GET_COMMAND_ARGUMENT(0 , working_path);
    write(*,'(a,/,a)') 'exe path: ', trim(working_path);
    call GETCWD(working_path)
    write(*,'(a,/,a)') 'Working Directory: ', trim(working_path);
end program testDir

```

输出:<br\>
```
exe path:

D:\testCWD\Debug\test_cwd.exe

working directory:
D:\testCWD
```

## 调试FAST

![fast][4]
最关键的是一个对应两个设置

+ 一个对应指的是第二步的Linker的output File的exe名字和第三步的General底下的TargetName对应上
+ 两个设置是指在Debuging对Command Arguments和Working Directory的设置。Command Argument指定文件名字
Working Directory指定文件路径。
[1]: /images/fortrandebug/idewd.png
[2]: /images/fortrandebug/fortrandebug.jpg
[3]: https://nwtc.nrel.gov/FAST8 
[4]: /images/fortrandebug/fast.jpg


