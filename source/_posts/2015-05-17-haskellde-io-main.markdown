---
layout: post
title: "Haskell的IO main"
date: 2015-05-17 21:48:03 +0800
comments: true
categories: Haskell
---

main在Haskell充当类似c系列语言的exe文件的头函数
main对应的类型是[IO Something](http://learnyoua.haskell.sg/content/zh-tw/ch09/input-and-output.html)
比如下列源码： 

<!--more-->
``` haskell 
main = do 
    putStrLn "Hello Hello" 
    putStrLn "Hello, what's your name?" 
    name <- getLine 
    putStrLn $ "Read this carefully, because this is your future: " ++  name 
``` 

Windows 输出： 

``` sh 
c:\>ghc --make hello.hs -o Hello 
[1 of 1] Compiling Main             ( hello.hs, hello.o ) 
Linking Hello.exe ... 

c:\>Hello.exe 
Hello Hello 
Hello, what's your name? 
yezhaoliang 
Read this carefully, because this is your future: yezhaoliang 
```

Ubuntu 输出:

``` sh
root at javazhao-N53SM [21:52:10ä��午] in /home/happycamp-of-lisp on git:develop+?
$ ghc --make hello12.hs -o hello12
[1 of 1] Compiling Main ( hello12.hs, hello12.o )
Linking hello12 ...

root at javazhao-N53SM [21:52:25ä��午] in /home/happycamp-of-lisp on git:develop+?
$ ./hello12
Hello Hello
Hello, what's your name?
yezhaoliang
Read this carefully, because this is your future: yezhaoliang

```

Haskell天生的具备跨平台的特点
