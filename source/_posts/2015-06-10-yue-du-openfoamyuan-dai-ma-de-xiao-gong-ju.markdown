---
layout: post
title: "阅读openfoam源代码的小工具"
date: 2015-06-10 23:19:42 +0800
comments: true
categories: openfoam
---

+ 当你阅读源代码你是否厌倦了cd来cd去，于是想把他们堆在一个文件内
+ 而堆起来又不好看，于是想要打扮一下

于是写了下面的小程序，估计对于阅读有帮助，主要用到

+ shell变量处理
+ shell变量长度
+ find显示相对全路经
+ ==两边__不能有__空格，然而在赋值的时候必须没有空格
+ 利用函数思想，封装小寒数
+ 美观，把一个文件当作一个夹子进行显示
<!--more-->


__注意针对每一个版本都需要进行适当的修改__.在getFile中有一个sed命令一定要注意，那是删除行首的7行，针对不同文件，需要适当修改.

``` sh
#!/bin/bash - 
#===============================================================================
#
#          FILE: browser.sh
# 
#         USAGE: ./browser.sh 
# 
#   DESCRIPTION:  注意针对每一个版本都需要进行适当的修改
# 
#       OPTIONS: ---
#  REQUIREMENTS: ---
#          BUGS: ---
#         NOTES: ---
#        AUTHOR: Ye Zhaoliang (), zhaoturkkey@163.com
#  ORGANIZATION: YZL
#       CREATED: 2015年06月10日 21:50
#      REVISION:  ---
#===============================================================================
#---  FUNCTION  ----------------------------------------------------------------
#          NAME:  generateChapter
#   DESCRIPTION:  针对不同额文件采用不同的处理方法，判断目录 和判断脚本是一个重要的操作
#                 但是更为重要的是 find获得文集拿的绝对路径(相对于当前文件)
#    PARAMETERS:  
#       RETURNS:  
#-------------------------------------------------------------------------------

generateChapter()
{
    # 妙用find 得到当前目录的相对路径 不需要不断的进入目录
    for var in `find . -name "*"`
    do
        if [[ -d  $var ]] # < cannot . Error
        then
            echo "${currentDir}${var#.}是一个目录" # 使用#号来删除之前的点号
        elif [[ "${var##*/}" == "${0}" ]] # 一定要注意等式两边有空格
        then
            echo "${currentDir}${var#.} ${0} 脚本文件不处理"
        else
            getFile ${currentDir}${var#.}
        fi

    done
}


#---  FUNCTION  ----------------------------------------------------------------
#          NAME:  getFile
#   DESCRIPTION:  在每个文件的头部添加一些信息
#    PARAMETERS:  
#       RETURNS:  
#-------------------------------------------------------------------------------
getFile()
{
    startString="# -------------------<<<<<<<<<"$1" started>>>>>>>>>-------------------#"
    endString="# --------------------<<<<<<<<<"$1" ended >>>>>>>>>-------------------#"
       #echo "" >>summary.sh
    #echo -e "\033[44;37m -------------------<<<<<<<<<"$1" started>>>>>>>>>-------------------\033[0m" >>summary.sh
    #echo  "# -------------------<<<<<<<<<"$1" started>>>>>>>>>-------------------" >>summary.sh
    echo $startString >> summary.sh
    echo "#`printf "%$(( ${#startString}-2 ))s" " "`#" >>summary.sh
    echo "#`printf "%$(( ${#startString}-2 ))s" " "`#" >>summary.sh
    echo "#`printf "%$(( ${#startString}-2 ))s" " "`#" >>summary.sh ### 不加上$反而不行
   # echo "#" >>summary.sh
   # echo "#" >>summary.sh
    sed '1,7d' $1 >> summary.sh
    echo "#`printf "%$(( ${#endString}-2 ))s" " "`#" >>summary.sh
    echo "#`printf "%$(( ${#endString}-2 ))s" " "`#" >>summary.sh
    echo "#`printf "%$(( ${#endString}-2 ))s" " "`#" >>summary.sh
    #echo "#" >>summary.sh
    #echo "#" >>summary.sh
    #echo -e "\033[43;37m --------------------<<<<<<<<<"$1" ended >>>>>>>>>-------------------\033[0m" >> summary.sh
    #echo "# --------------------<<<<<<<<<"$1" ended >>>>>>>>>-------------------" >> summary.sh
    echo $endString >> summary.sh
    echo "" >>summary.sh
    echo "" >>summary.sh
    echo "" >>summary.sh
    
}

currentDir=`pwd`

# 如果存在 summary.sh 证明已经存在了，就先把删掉， 因为这里是
# 采用追加的方式，而不是写入。
if [[ -e ./summary.sh ]]
then 
    rm -rf ./summary.sh
fi
# 调用产生所有的文件夹内的数据到一个文件中

generateChapter

#sed -i '/C++/d' summary |sed '/=========/d'|sed '/OpenFOAM/d' | sed '/Version/d' | sed '/Web/d'|sed '/anipulation/d'
```


## 结果显示

```  sh
# -------------------<<<<<<<<</openfoamF708/tutorials/incompressible/icoFoam/cavity/system/fvSolution started>>>>>>>>>-------------------#
#                                                                                                                                        #
#                                                                                                                                        #
#                                                                                                                                        #
FoamFile
{
    version     2.0;
    format      ascii;
    class       dictionary;
    location    "system";
    object      fvSolution;
}
// * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * //

solvers
{
    p
    {
        solver          PCG;
        preconditioner  DIC;
        tolerance       1e-06;
        relTol          0;
    }

    U
    {
        solver          smoothSolver;
        smoother        symGaussSeidel;
        tolerance       1e-05;
        relTol          0;
    }
}

PISO
{
    nCorrectors     2;
    nNonOrthogonalCorrectors 0;
    pRefCell        0;
    pRefValue       0;
}


#                                                                                                                                        #
#                                                                                                                                        #
#                                                                                                                                        #
# --------------------<<<<<<<<</openfoamF708/tutorials/incompressible/icoFoam/cavity/system/fvSolution ended >>>>>>>>>-------------------#



# -------------------<<<<<<<<</openfoamF708/tutorials/incompressible/icoFoam/cavity/system/fvSchemes started>>>>>>>>>-------------------#
```

