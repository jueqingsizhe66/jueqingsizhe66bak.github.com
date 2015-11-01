---
layout: post
title: "TurbSim 8个湍流模型的对比"
date: 2015-08-10 01:00:47 +0800
comments: true
categories: Fast
---

[TurbSim][1],是NREL提供的一个小的开源工具， 用于产生湍流风，设置的时候可能对于不同模型会有不同的参数，基于范本进行了一番分析。当然测试文件在TurbSim的Test文件下的inp文件夹下。

如果对那些湍流模型做一个对比，那样是否可以更加清晰的看出不同呢？
于是就有了下面的shell分析脚本。

TurbSim产生的风力机一圈的入流截面模型（仅仅一个截面，全场是因为捕捉了很多个截面）

![Turbsim][3]


<!--more-->

## Shell解析脚本

``` sh
#!/bin/bash - 
#===============================================================================
#
#          FILE: a.sh
# 
#         USAGE: ./a.sh 
# 
#   DESCRIPTION: 
# 
#       OPTIONS: ---
#  REQUIREMENTS: ---
#          BUGS: ---
#         NOTES: ---
#        AUTHOR: Ye Zhaoliang (), zhaoturkkey@163.com
#  ORGANIZATION: YZL
#       CREATED: 2015年08月10日 00:26
#      REVISION:  ---
#===============================================================================

#---  FUNCTION  ----------------------------------------------------------------
#          NAME:  _repeat1
#   DESCRIPTION:  优化版本
#    PARAMETERS:  
#       RETURNS:  
#-------------------------------------------------------------------------------
_repeat1()
{
    #@ 使用方式： _repeat 要重复的字符串  重复次数
    #@  Usage : _repeat string number
    _REPEAT1=$1  #set and not empty, or unlimit recycle
    while [[ ${#_REPEAT1} -lt $2 ]]
    do
        _REPEAT1=$_REPEAT1$_REPEAT1$_REPEAT1 ## 3次优化
    done
    _REPEAT1=${_REPEAT1:0:$2} ##Trim到我们需要的长度
}

alert() #@ Description    产生一个方框的注释
{       #@ Usage    alert "话语"
    _repeat1 "${2:-#}" $(( ${#1} + 8 )) ## ${2:-#} 如果$2未定义则使用#
    printf '%s\n' "$_REPEAT1" ## \a=BEL
    printf '%2.2s %s %2.2s\n' "$_REPEAT1" "$1" "$_REPEAT1" ## \a=BEL
    printf '%s\n' "$_REPEAT1"
}


#alert "Do you really want to delete all the files?"


## RandSeed1 在不同湍流模型配置的不同
{
alert  "RandSeed1" 
grep -n "RandSeed1" *.inp|awk -F: '{print $1,$3}'|awk '{print substr($1,0,length($1)-4),"\t\t",$2}'|column -t
alert  "RandSeed2" 
grep -n "RandSeed2" *.inp|awk -F: '{print $1,$3}'|awk '{print substr($1,0,length($1)-4),"\t\t",$2}'|column -t
alert  "WrBHHTP" 
grep -n "WrBHHTP" *.inp|awk -F: '{print $1,$3}'|awk '{print substr($1,0,length($1)-4),"\t\t",$2}'|column -t
alert  "WrFHHTP" 
grep -n "WrFHHTP" *.inp|awk -F: '{print $1,$3}'|awk '{print substr($1,0,length($1)-4),"\t\t",$2}'|column -t
alert  "WrADHH" 
grep -n "WrADHH" *.inp|awk -F: '{print $1,$3}'|awk '{print substr($1,0,length($1)-4),"\t\t",$2}'|column -t
alert  "WrADFF" 
grep -n "WrADFF" *.inp|awk -F: '{print $1,$3}'|awk '{print substr($1,0,length($1)-4),"\t\t",$2}'|column -t
alert  "WrBLFF" 
grep -n "WrBLFF" *.inp|awk -F: '{print $1,$3}'|awk '{print substr($1,0,length($1)-4),"\t\t",$2}'|column -t
alert  "WrADTWR" 
grep -n "WrADTWR" *.inp|awk -F: '{print $1,$3}'|awk '{print substr($1,0,length($1)-4),"\t\t",$2}'|column -t
alert  "WrFMTFF" 
grep -n "WrFMTFF" *.inp|awk -F: '{print $1,$3}'|awk '{print substr($1,0,length($1)-4),"\t\t",$2}'|column -t
alert  "WrACT" 
grep -n "WrACT" *.inp|awk -F: '{print $1,$3}'|awk '{print substr($1,0,length($1)-4),"\t\t",$2}'|column -t
alert  "NumGrid_Z" 
grep -n "NumGrid_Z" *.inp|awk -F: '{print $1,$3}'|awk '{print substr($1,0,length($1)-4),"\t\t",$2}'|column -t
alert  "NumGrid_Y" 
grep -n "NumGrid_Y" *.inp|awk -F: '{print $1,$3}'|awk '{print substr($1,0,length($1)-4),"\t\t",$2}'|column -t
alert  "HubHt" 
grep -n "HubHt" *.inp|awk -F: '{print $1,$3}'|awk '{print substr($1,0,length($1)-4),"\t\t",$2}'|column -t
alert  "GridHeight" 
grep -n "GridHeight" *.inp|awk -F: '{print $1,$3}'|sed '/HubHt/d'|awk '{print substr($1,0,length($1)-4),"\t\t",$2}'|column -t
alert  "GridWidth" 
grep -n "GridWidth" *.inp|awk -F: '{print $1,$3}'|sed '/HubHt/d'|awk '{print substr($1,0,length($1)-4),"\t\t",$2}'|column -t
alert  "VFlowAng" 
grep -n "VFlowAng" *.inp|awk -F: '{print $1,$3}'|awk '{print substr($1,0,length($1)-4),"\t\t",$2}'|column -t
alert  "HFlowAng" 
grep -n "HFlowAng" *.inp|awk -F: '{print $1,$3}'|awk '{print substr($1,0,length($1)-4),"\t\t",$2}'|column -t
alert  "TurbModel" 
grep -n "TurbModel" *.inp|awk -F: '{print $1,$3}'|awk '{print substr($1,0,length($1)-4),"\t\t",$2}'|column -t
alert  "IECstandard" 
grep -n "IECstandard" *.inp|awk -F: '{print $1,$3}'|awk '{print substr($1,0,length($1)-4),"\t\t",$2}'|column -t
alert  "IECturbc" 
grep -n "IECturbc" *.inp|awk -F: '{print $1,$3}'|awk '{print substr($1,0,length($1)-4),"\t\t",$2}'|column -t
alert  "IEC_WindType" 
grep -n "IEC_WindType" *.inp|awk -F: '{print $1,$3}'|awk '{print substr($1,0,length($1)-4),"\t\t",$2}'|column -t
alert  "ETMc" 
grep -n "ETMc" *.inp|awk -F: '{print $1,$3}'|awk '{print substr($1,0,length($1)-4),"\t\t",$2}'|column -t
alert  "WindProfileType" 
grep -n "WindProfileType" *.inp|awk -F: '{print $1,$3}'|awk '{print substr($1,0,length($1)-4),"\t\t",$2}'|column -t
alert  "RefHt" 
grep -n "RefHt" *.inp|awk -F: '{print $1,$3}'|awk '{print substr($1,0,length($1)-4),"\t\t",$2}'|column -t
alert  "URef" 
grep -n "URef" *.inp|awk -F: '{print $1,$3}'|awk '{print substr($1,0,length($1)-4),"\t\t",$2}'|column -t
alert  "ZJetMax" 
grep -n "ZJetMax" *.inp|awk -F: '{print $1,$3}'|awk '{print substr($1,0,length($1)-4),"\t\t",$2}'|column -t
alert  "PLExp" 
grep -n "PLExp" *.inp|awk -F: '{print $1,$3}'|awk '{print substr($1,0,length($1)-4),"\t\t",$2}'|column -t
alert  "Z0" 
grep -n "Z0" *.inp|awk -F: '{print $1,$3}'|awk '{print substr($1,0,length($1)-4),"\t\t",$2}'|column -t
alert  "RICH_NO" 
grep -n "RICH_NO" *.inp|awk -F: '{print $1,$3}'|awk '{print substr($1,0,length($1)-4),"\t\t",$2}'|column -t
alert  "ZI" 
grep -n "ZI" *.inp|awk -F: '{print $1,$3}'|awk '{print substr($1,0,length($1)-4),"\t\t",$2}'|column -t
alert  "PC_UW" 
grep -n "PC_UW" *.inp|awk -F: '{print $1,$3}'|awk '{print substr($1,0,length($1)-4),"\t\t",$2}'|column -t
alert  "PC_UV" 
grep -n "PC_UV" *.inp|awk -F: '{print $1,$3}'|awk '{print substr($1,0,length($1)-4),"\t\t",$2}'|column -t
alert  "PC_VW" 
grep -n "PC_VW" *.inp|awk -F: '{print $1,$3}'|awk '{print substr($1,0,length($1)-4),"\t\t",$2}'|column -t
alert  "CohExp" 
grep -n "CohExp" *.inp|awk -F: '{print $1,$3}'|awk '{print substr($1,0,length($1)-4),"\t\t",$2}'|column -t
alert  "CTEventPath" 
grep -n "CTEventPath" *.inp|awk -F: '{print $1,$3}'|awk '{print substr($1,0,length($1)-4),"\t\t",$2}'|column -t
alert  "CTEventFile" 
grep -n "CTEventFile" *.inp|awk -F: '{print $1,$3}'|awk '{print substr($1,0,length($1)-4),"\t\t",$2}'|column -t
alert  "Randomize" 
grep -n "Randomize" *.inp|awk -F: '{print $1,$3}'|awk '{print substr($1,0,length($1)-4),"\t\t",$2}'|column -t
alert  "DistScl" 
grep -n "DistScl" *.inp|awk -F: '{print $1,$3}'|awk '{print substr($1,0,length($1)-4),"\t\t",$2}'|column -t
alert  "CTLy" 
grep -n "CTLy" *.inp|awk -F: '{print $1,$3}'|awk '{print substr($1,0,length($1)-4),"\t\t",$2}'|column -t
alert  "CTLz" 
grep -n "CTLz" *.inp|awk -F: '{print $1,$3}'|awk '{print substr($1,0,length($1)-4),"\t\t",$2}'|column -t
alert  "CTStartTime" 
grep -n "CTStartTime" *.inp|awk -F: '{print $1,$3}'|awk '{print substr($1,0,length($1)-4),"\t\t",$2}'|column -t
} > summary.out

```
### 改进

1.  去除无用的Turbulence and Boundary
2.  增加了对GridWidth and GridHeight的处理

```
grep -n "GridHeight" *.inp|awk -F: '{print $1,$3}'|sed '/HubHt/d'|awk '{print substr($1,0,length($1)-4),"\t\t",$2}'|column -t
grep -n "GridWidth" *.inp|awk -F: '{print $1,$3}'|sed '/HubHt/d'|awk '{print substr($1,0,length($1)-4),"\t\t",$2}'|column -t

```
3.
## shell产生的结果如下：

```
#################
## RandSeed1 ##
#################
GPLLJ        234516
HYDRO_TIDAL  2318573
Kaimal_15    123456
Kaimal       123456
KHtest       123456
smooth       4433456
vonKarm_15   123456
vonKarm      123456
#################
## RandSeed2 ##
#################
GPLLJ        789012
HYDRO_TIDAL  RANLUX
Kaimal_15    789012
Kaimal       789012
KHtest       "RanLux"
smooth       "RanLux"
vonKarm_15   789012
vonKarm      789012
###############
## WrBHHTP ##
###############
GPLLJ        False
HYDRO_TIDAL  False
Kaimal_15    False
Kaimal       False
KHtest       False
smooth       True
vonKarm_15   False
vonKarm      False
###############
## WrFHHTP ##
###############
GPLLJ        True
HYDRO_TIDAL  True
Kaimal_15    False
Kaimal       False
KHtest       False
smooth       False
vonKarm_15   True
vonKarm      True
##############
## WrADHH ##
##############
GPLLJ        False
HYDRO_TIDAL  True
Kaimal_15    False
Kaimal       False
KHtest       False
smooth       False
vonKarm_15   True
vonKarm      True
##############
## WrADFF ##
##############
GPLLJ        True
HYDRO_TIDAL  False
Kaimal_15    False
Kaimal       False
KHtest       False
smooth       False
vonKarm_15   False
vonKarm      False
##############
## WrBLFF ##
##############
GPLLJ        False
HYDRO_TIDAL  True
Kaimal_15    False
Kaimal       False
KHtest       True
smooth       True
vonKarm_15   False
vonKarm      False
###############
## WrADTWR ##
###############
GPLLJ        False
HYDRO_TIDAL  False
Kaimal_15    False
Kaimal       False
KHtest       False
smooth       False
vonKarm_15   False
vonKarm      False
###############
## WrFMTFF ##
###############
GPLLJ        False
HYDRO_TIDAL  False
Kaimal_15    True
Kaimal       True
KHtest       False
smooth       False
vonKarm_15   False
vonKarm      False
#############
## WrACT ##
#############
GPLLJ        True
HYDRO_TIDAL  False
Kaimal_15    False
Kaimal       False
KHtest       True
smooth       True
vonKarm_15   False
vonKarm      False
#################
## NumGrid_Z ##
#################
GPLLJ        5
HYDRO_TIDAL  5
Kaimal_15    5
Kaimal       5
KHtest       5
smooth       5
vonKarm_15   5
vonKarm      5
#################
## NumGrid_Y ##
#################
GPLLJ        5
HYDRO_TIDAL  5
Kaimal_15    5
Kaimal       5
KHtest       5
smooth       5
vonKarm_15   5
vonKarm      5
#############
## HubHt ##
#############
GPLLJ        90.00
HYDRO_TIDAL  10
Kaimal_15    37.00
Kaimal       37.00
KHtest       37.00
smooth       84.00
vonKarm_15   37.00
vonKarm      37.00
##################
## GridHeight ##
##################
GPLLJ        80.00
HYDRO_TIDAL  9.00
Kaimal_15    30.00
Kaimal       30.00
KHtest       50.00
smooth       70.00
vonKarm_15   30.00
vonKarm      30.00
#################
## GridWidth ##
#################
GPLLJ        600.0
GPLLJ        90.0
GPLLJ        80.00
HYDRO_TIDAL  600
HYDRO_TIDAL  40
HYDRO_TIDAL  9.00
Kaimal_15    600.0
Kaimal_15    55.8
Kaimal_15    30.00
Kaimal       600.0
Kaimal       55.8
Kaimal       30.00
KHtest       600.0
KHtest       100.0
KHtest       50.00
smooth       600.0
smooth       100.0
smooth       70.00
vonKarm_15   600.0
vonKarm_15   55.8
vonKarm_15   30.00
vonKarm      600.0
vonKarm      55.8
vonKarm      30.00
################
## VFlowAng ##
################
GPLLJ        0
HYDRO_TIDAL  0
Kaimal_15    5
Kaimal       5
KHtest       0
smooth       0
vonKarm_15   5
vonKarm      0
################
## HFlowAng ##
################
GPLLJ        0
HYDRO_TIDAL  0
Kaimal_15    10
Kaimal       10
KHtest       0
smooth       0
vonKarm_15   10
vonKarm      0
#################
## TurbModel ##
#################
GPLLJ        "GP_LLJ"
HYDRO_TIDAL  "TIDAL"
Kaimal_15    "IECKAI"
Kaimal       "IECKAI"
KHtest       "NWTCUP"
smooth       "SMOOTH"
vonKarm_15   "IECVKM"
vonKarm      "IECVKM"
###################
## IECstandard ##
###################
GPLLJ        1
HYDRO_TIDAL  "1-ED3"
Kaimal_15    "1-ED2"
Kaimal       1
KHtest       1
smooth       1
vonKarm_15   1
vonKarm      1
################
## IECturbc ##
################
GPLLJ        "A"
HYDRO_TIDAL  "A"
Kaimal_15    "18.0"
Kaimal       "C"
KHtest       "KHtest"
smooth       "A"
vonKarm_15   16.0
vonKarm      "A"
####################
## IEC_WindType ##
####################
GPLLJ        NTM
HYDRO_TIDAL  "NTM"
Kaimal_15    NTM
Kaimal       NTM
KHtest       NTM
smooth       NTM
vonKarm_15   NTM
vonKarm      NTM
############
## ETMc ##
############
GPLLJ        default
HYDRO_TIDAL  default
Kaimal_15    default
Kaimal       default
KHtest       default
smooth       default
vonKarm_15   default
vonKarm      default
#######################
## WindProfileType ##
#######################
GPLLJ        default
HYDRO_TIDAL  "H2L"
Kaimal_15    default
Kaimal       default
KHtest       default
smooth       default
vonKarm_15   default
vonKarm      default
#############
## RefHt ##
#############
GPLLJ        85.00
HYDRO_TIDAL  18
Kaimal_15    10.00
Kaimal       10.00
KHtest       37.00
smooth       84.00
vonKarm_15   10.00
vonKarm      10.00
############
## URef ##
############
GPLLJ        12.0
HYDRO_TIDAL  18
HYDRO_TIDAL  1.2
Kaimal_15    15.0
Kaimal       6.0
KHtest       13.0
smooth       17.0
vonKarm_15   15.0
vonKarm      6.0
###############
## ZJetMax ##
###############
GPLLJ        350
HYDRO_TIDAL  default
Kaimal_15    350
Kaimal       350
KHtest       350
smooth       350
vonKarm_15   350
vonKarm      260
#############
## PLExp ##
#############
GPLLJ        default
HYDRO_TIDAL  default
Kaimal_15    default
Kaimal       default
KHtest       default
smooth       1.527
vonKarm_15   default
vonKarm      default
##########
## Z0 ##
##########
GPLLJ        default
HYDRO_TIDAL  default
Kaimal_15    default
Kaimal       default
KHtest       default
smooth       default
vonKarm_15   default
vonKarm      default
###############
## RICH_NO ##
###############
GPLLJ        0.05
HYDRO_TIDAL  0.05
Kaimal_15    0.05
Kaimal       0.05
KHtest       0.02
smooth       0.05
vonKarm_15   0.05
vonKarm      0.05
##########
## ZI ##
##########
GPLLJ        default
HYDRO_TIDAL  default
Kaimal_15    default
Kaimal       default
KHtest       default
smooth       default
vonKarm_15   default
vonKarm      default
#############
## PC_UW ##
#############
GPLLJ        default
HYDRO_TIDAL  default
Kaimal_15    default
Kaimal       default
KHtest       default
smooth       default
vonKarm_15   default
vonKarm      default
#############
## PC_UV ##
#############
GPLLJ        default
HYDRO_TIDAL  default
Kaimal_15    default
Kaimal       default
KHtest       default
smooth       default
vonKarm_15   default
vonKarm      default
#############
## PC_VW ##
#############
GPLLJ        default
HYDRO_TIDAL  default
Kaimal_15    default
Kaimal       default
KHtest       default
smooth       default
vonKarm_15   default
vonKarm      default
##############
## CohExp ##
##############
GPLLJ        default
HYDRO_TIDAL  default
Kaimal_15    default
Kaimal       default
KHtest       default
smooth       default
vonKarm_15   default
vonKarm      default
###################
## CTEventPath ##
###################
GPLLJ        ".\EventData"
HYDRO_TIDAL  "/home/lkilcher/work/nwtc/turbsim/trunk/Test/EventData/"
Kaimal_15    ".\EventData"
Kaimal       ".\EventData"
KHtest       ".\EventData"
smooth       ".\EventData"
vonKarm_15   ".\EventData"
vonKarm      ".\EventData"
###################
## CTEventFile ##
###################
GPLLJ        LES
HYDRO_TIDAL  "Random"
Kaimal_15    random
Kaimal       random
KHtest       "les"
smooth       "les"
vonKarm_15   random
vonKarm      random
#################
## Randomize ##
#################
GPLLJ        true
HYDRO_TIDAL  true
HYDRO_TIDAL  1.0
HYDRO_TIDAL  0.5
HYDRO_TIDAL  0.5
Kaimal_15    true
Kaimal       true
KHtest       true
smooth       true
vonKarm_15   true
vonKarm      true
###############
## DistScl ##
###############
GPLLJ        1.0
HYDRO_TIDAL  1.0
Kaimal_15    1.0
Kaimal       1.0
KHtest       1.0
smooth       1.0
vonKarm_15   1.0
vonKarm      1.0
############
## CTLy ##
############
GPLLJ        0.5
HYDRO_TIDAL  0.5
Kaimal_15    0.5
Kaimal       0.5
KHtest       0.5
smooth       0.5
vonKarm_15   0.5
vonKarm      0.5
############
## CTLz ##
############
GPLLJ        0.5
HYDRO_TIDAL  0.5
Kaimal_15    0.5
Kaimal       0.5
KHtest       0.5
smooth       0.5
vonKarm_15   0.5
vonKarm      0.5
###################
## CTStartTime ##
###################
GPLLJ        30.0
HYDRO_TIDAL  30.0
Kaimal_15    30.0
Kaimal       30.0
KHtest       0.0
smooth       10.0
vonKarm_15   30.0
vonKarm      30.0
```

 进一步的细节可以参考[Turbsim UserGuide][2]

[1]: https://nwtc.nrel.gov/TurbSim 
[2]: https://nwtc.nrel.gov/system/files/TurbSim.pdf 
[3]: /images/fast/turbsim.png
