---
layout: post
title: "HaskellModule"
date: 2015-05-20 13:49:57 +0800
comments: true
categories: Haskell
---

Reference [Here](www.shuklan.com/haskell/lec05.html#/0/21)

模块化，这样才能方便构建较大型的项目，解决更加复杂的问题。
其中MyData.hs是模块文件
              注意，模块中添加类型时候使用(..);
                          添加函数时候直接使用函数名
    TestMyData.hs是测试模块文件

##基本语法结构：
module MyData
(  类型(..) ....
   函数名 ...)
where
类型声明 ...
函数声明 ...

注意 MyData.hs一定是和module后面的MyData一样的！
## 调用方式
import MyData
******
<!--more-->

MyData.hs
``` haskell
module MyData
(MetricUnit(..),
    ImperialUnit(..),
    Measurement(..),
    convert
) where
-- How to use this module
-- new File Test.hs  
--    import MyData
--    reportMeasurement :: Measurement->String
--    reportMeasurement (MyData.MetricMeasurement x u)
--    = (show  x) ++ " " ++ (show u)
--    reportMesuarement m
--    =reportMeasurement (convert m)
-- N.B. If there are another file use Meter type,it will fail
-- 1
data MetricUnit = Meter|Liter|KiloGram deriving (Show,Eq)
-- 2
data ImperialUnit = Yard
                    | Gallon
                    | Pound 
                    deriving (Show,Eq)
-- 3
data Measurement = MetricMeasurement Double MetricUnit
                    | ImperialMeasurement Double ImperialUnit
                   deriving (Show)   --If not,it will not show,Only
                            --in the type defition,but not function

-- 4
convert :: Measurement-> Measurement
-- Take attention for the () brace
convert (MetricMeasurement x u)
        | u == Meter  = ImperialMeasurement (1.0936*x) Yard
        | u == Liter  = ImperialMeasurement (0.2642*x) Gallon
        | u == KiloGram = ImperialMeasurement (2.2046*x) Pound
--wrong!
--        | u == "Meter"  = ImperialMeasurement (1.0936*x) Yard
--        | u == "Liter"  = ImperialMeasurement (0.2642*x) Gallon
--        | u == "KiloGram" = ImperialMeasurement (2.2046*x) Pound
convert (ImperialMeasurement x u)
        | u == Yard  = MetricMeasurement (0.9144*x) Meter
        | u == Gallon  = MetricMeasurement (3.7854*x) Liter
        | u == Pound = MetricMeasurement (0.4536*x) KiloGram


```


TestMyData.hs
  *注意，一定不要把Measurement-写成了  Mesuarement等错误的文字*
``` haskell
import MyData
reportMeasurement :: Measurement->String
reportMeasurement (MyData.MetricMeasurement x u)
                = (show x) ++ " " ++ (show u)
reportMeasurement m =reportMeasurement (convert m)
--reportMeasurement (MyData.ImperialMeasurement x u)
--                = (show x) ++ " " ++ (show u)
---if imperial    let m = MetricMeasuremnt 2 Meter ...



```

## 运行结果
``` sh
--Main> reportMeasurement (convert (ImperialMeasurement 5 Pound))
--"2.268 KiloGram"
 
*Main> let m = ImperialMeasurement 5 Pound
*Main> reportMeasurement m
"2.268 KiloGram"

*Main> reportMeasurement (MetricMeasurement 5 Meter)
"5.0 Meter"

```
