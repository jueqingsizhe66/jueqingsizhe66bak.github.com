---
layout: post
title: "Haskell更高一级抽象模块化"
date: 2015-05-17 16:40:56 +0800
comments: true
categories: Haskell
---

一类抽象是把所有函数都放在module中 比如下面的方式:
(注意一点：默认的查找路径会在当前目录下查找)

<!--more-->
## 当前文件夹新建一个Geometry.hs
*注意Haskell默认Type都是大写开头 模块也是一样*

``` haskell
module Geometry   
( sphereVolume
,sphereArea   
,cubeVolume   
,cubeArea   
,cuboidArea   
,cuboidVolume   
) where   
 
sphereVolume :: Float -> Float   
sphereVolume radius = (4.0 / 3.0) * pi * (radius ^ 3)   
 
sphereArea :: Float -> Float   
sphereArea radius = 4 * pi * (radius ^ 2)   
 
cubeVolume :: Float -> Float   
cubeVolume side = cuboidVolume side side side   
 
cubeArea :: Float -> Float   
cubeArea side = cuboidArea side side side   
 
cuboidVolume :: Float -> Float -> Float -> Float   
cuboidVolume a b c = rectangleArea a b * c   
 
cuboidArea :: Float -> Float -> Float -> Float   
cuboidArea a b c = rectangleArea a b * 2 + rectangleArea a c * 2 + rectangleArea c b * 2   
 
rectangleArea :: Float -> Float -> Float   
rectangleArea a b = a * b

```

然后我们就可以
``` sh
Prelude> :l Geometry.hs 
[1 of 1] Compiling Geometry         ( Geometry.hs, interpreted )
Ok, modules loaded: Geometry.

*Geometry> sphereVolume 3  --->*Geometry表示切换在Geometry模块下
113.097336

```


如果工程项目变大，我们也许需要更好的控制模块的粒度。

## 使用分层目录构建模块

1. Create Geo directory
``` sh
$ ls |grep Geo
Geo

```
2. cd Geo

*注意一点 文件名大写 最好大写*

### 2.1 new Sphere.hs_
增加下面的文件内容到当前文件中_
``` haskell
module Geo.Sphere   
( volume   
,area   
) where   
 
volume :: Float -> Float   
volume radius = (4.0 / 3.0) * pi * (radius ^ 3)   
 
area :: Float -> Float   
area radius = 4 * pi * (radius ^ 2)

```

### 2.2 new Cubi.hs

增加下面的文件内容到当前文件中_
``` haskell
module Geo.Cubi
( volume   
,area   
) where   
 
volume :: Float -> Float -> Float -> Float   
volume a b c = rectangleArea a b * c   
 
area :: Float -> Float -> Float -> Float   
area a b c = rectangleArea a b * 2 + rectangleArea a c * 2 + rectangleArea c b * 2   
 
rectangleArea :: Float -> Float -> Float   
rectangleArea a b = a * b

```
### 2.3 new Cube.hs
增加下面的文件内容到当前文件中_

``` haskell
module Geo.Cube   
( volume   
,area   
) where   
 
import qualified Geo.Cubi as Cuboid   
 
volume :: Float -> Float   
volume side = Cuboid.volume side side side   
 
area :: Float -> Float   
area side = Cuboid.area side side side
```

### 2.4 new Main.hs 和Geo文件夹在同一目录下o

增加下面的文件内容到当前文件中_

``` haskell

import qualified Geo.Sphere as Sphere   
import qualified Geo.Cubi as Cuboid   
import qualified Geo.Cube as Cube
```

这样我们的文件结构如下

``` sh
root at javazhao-N53SM [17:00:24ä��午] in /home/happycamp-of-lisp on git:develop+?
$ ls main.hs Geo
main.hs

Geo:
Cube.hs  Cubi.hs  Sphere.hs

```

这样我们只要在/home/happycamp-of-lisp 运行ghci
并加载main.hs文件即可！(*注意一点 调用模块的函数和python有点像*)
``` sh
Prelude> :l main.hs 
[1 of 4] Compiling Geo.Cubi         ( Geo/Cubi.hs, interpreted )
[2 of 4] Compiling Geo.Cube         ( Geo/Cube.hs, interpreted )
[3 of 4] Compiling Geo.Sphere       ( Geo/Sphere.hs, interpreted )
[4 of 4] Compiling Main             ( main.hs, interpreted )
Ok, modules loaded: Geo.Sphere, Geo.Cubi, Geo.Cube, Main.

*Main> Sphere.area 3
113.097336
*Main> a
abs         all         appendFile  asinh       atanh
acos        and         asTypeOf    atan
acosh       any         asin        atan2
*Main> a
abs         all         appendFile  asinh       atanh
acos        and         asTypeOf    atan
acosh       any         asin        atan2
*Main> area 3

<interactive>:4:1:
    Not in scope: `area'
    Perhaps you meant one of these:
      `Sphere.area' (imported from Geo.Sphere),
      `Cube.area' (imported from Geo.Cube),
      `Cuboid.area' (imported from Geo.Cubi)
```

