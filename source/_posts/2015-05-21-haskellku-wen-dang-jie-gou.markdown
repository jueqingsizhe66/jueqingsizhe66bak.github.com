---
layout: post
title: "Haskell库文档结构"
date: 2015-05-21 12:27:42 +0800
comments: true
categories: Haskell
---

```
/src
Folder for source-code
Setup.hs
Haskell build script
app.cabal
Cabal build script
README
Informative text file
LICENCE
Software licence statement
```
下面是我的建立过程:
<!--more-->


## 1新建一个HELLO文件夹,在HELLO文件夹新建一个src
``` sh
mkdir HELLO
cd HELLO
mdkir src
```



## 2 main.hs.src内新建一个main.hs
``` haskell
module Main where

import Control.Monad

main=do
  putStrLn "Quit? y/n"
  ans <- getLine
  when (ans /= "y") $ do
       putStrLn "not quiting"
       main
```


## 3 cabal init

Return to the* top* directory, and run cabal init.
It helps you fill out the following information:
+ Package name
+ Package version
+ License
+ Author name
+ Maintainer email
+ Project homepage URL
+ Project synopsis
+ Build type
``` sh
cabal init
```

[一个填写模板](https://github.com/CGenie/haskell-snake/blob/master/snake.cabal )


至此现在你的文件结构应该是
``` sh
$ tree
.
|-- HELLO.cabal
|-- LICENSE
|-- Setup.hs
`-- src
    `-- Main.hs

```


现在你就可以分享你的程序，而如果你要安装，则可以继续看下面简单操作

Run +cabal install+ at the project directory
然后就会把你的执行程序放在~/.cabal/bin中了
``` sh
cabal install
```
一定要注意 修改HELLO.cabal中的main-is,不要注释他，默认是注释
并且填入src的主文件名 Main.hs
``` sh


executable HELLO
  -- .hs or .lhs file containing the Main module.
   main-is: Main.hs            一定要补好，否则cabal install 失败
  
  -- Modules included in this executable, other than Main.
  -- other-modules:       
  
  -- Other library packages from which modules are imported.
  build-depends:       base ==4.6.*
  
  -- Directories containing source files.
  hs-source-dirs:      src
  
```

结果是：
``` sh
$ cabal install
Resolving dependencies...
Configuring HELLO-1.0.0...
Building HELLO-1.0.0...
Preprocessing executable 'HELLO' for HELLO-1.0.0...
[1 of 1] Compiling Main             ( src/Main.hs, dist/build/HELLO/HELLO-tmp/Main.o )
Linking dist/build/HELLO/HELLO ...
Installing executable(s) in /home/javazhao/.cabal/bin
Installed HELLO-1.0.0

```
此时的文档结构如下:
``` haskell
$ tree
.
|-- dist
|   |-- build
|   |   |-- autogen
|   |   |   |-- cabal_macros.h
|   |   |   `-- Paths_HELLO.hs
|   |   `-- HELLO
|   |       |-- HELLO
|   |       `-- HELLO-tmp
|   |           |-- Main.hi
|   |           `-- Main.o
|   |-- package.conf.inplace
|   `-- setup-config
|-- HELLO.cabal
|-- LICENSE
|-- Setup.hs
`-- src
    `-- Main.hs
```


一些额外知识：

Open up Main.hs and add some documentation
-- | is the syntax for documenting function type signature, type declaration, or class declaration,记住--和|中间有一个空格
``` haskell
module Main where

-- |This is the main function
main :: IO()
main = do
  putStrLn "What's your name?"
  name <- getLine
  putStrLn ("Hi " ++ name)
```

然后就可以使用Haddock(前提是你运行了cabal install,这样你的文件夹下才有dist)
Haddock is a documentation engine

首先需要安装alex：
``` sh
cabal install alex
Linking dist/build/alex/alex ...
Installing executable(s) in /home/javazhao/.cabal/bin
Installed alex-3.1.4

```
然后安装happy
``` sh
cabal install happy

Linking dist/build/happy/happy ...
Installing executable(s) in /home/javazhao/.cabal/bin
Installed happy-1.19.5

```
最后安装haddock：
``` sh
cabal install haddock
Linking dist/build/haddock/haddock ...
Installing library in /home/javazhao/.cabal/lib/haddock-2.13.2.1/ghc-7.6.3
Installing executable(s) in /home/javazhao/.cabal/bin
Registering haddock-2.13.2.1...
Installed haddock-2.13.2.1
```

最后运行结果
``` sh
 cabal haddock --executables
$ cabal haddock --executables 
Running Haddock for HELLO-1.0.0...
Preprocessing executable 'HELLO' for HELLO-1.0.0...
Warning: The documentation for the following packages are not installed. No
links will be generated to these packages: base-4.6.0.1, rts-1.0,
ghc-prim-0.3.0.0, integer-gmp-0.5.0.0
Haddock coverage:
  50% (  1 /  2) in 'Main'
Warning: Main: could not find link destinations for:
    GHC.Types.IO
Documentation created: dist/doc/html/HELLO/HELLO/index.html

```
然后你的文档就会存在dist/doc/html/HELLO/HELLO/index.html
这其实就是官网的那些类库网页的的产生方式.

最终的文档结构：
``` sh
$ tree
.
|-- dist
|   |-- build
|   |   |-- autogen
|   |   |   |-- cabal_macros.h
|   |   |   `-- Paths_HELLO.hs
|   |   `-- HELLO
|   |       |-- HELLO
|   |       `-- HELLO-tmp
|   |           |-- Main.hi
|   |           `-- Main.o
|   |-- doc
|   |   `-- html
|   |       `-- HELLO
|   |           `-- HELLO
|   |               |-- doc-index.html
|   |               |-- frames.html
|   |               |-- haddock-util.js
|   |               |-- HELLO.haddock
|   |               |-- hslogo-16.png
|   |               |-- index-frames.html
|   |               |-- index.html
|   |               |-- Main.html
|   |               |-- mini_Main.html
|   |               |-- minus.gif
|   |               |-- ocean.css
|   |               |-- plus.gif
|   |               `-- synopsis.png
|   |-- package.conf.inplace
|   `-- setup-config
|-- HELLO.cabal
|-- LICENSE
|-- Setup.hs
`-- src
    `-- Main.hs

```

## [一个提醒](https://www.haskell.org/haddock/doc/html/markup.html#id564988 )
以后写代码的时候，一定要注意标注

+ 使用-- |
+ 使用{- -} 针对快注释
[一个链接关于haddock的用户指南](https://www.haskell.org/haddock/doc/html/index.html )
