---
layout: post
title: "Jar包打包成可安装的EXE文件"
date: 2015-05-11 14:58:43 +0800
comments: true
categories: JavaBasic
---
<!--more-->

 学习java的人都知道，java执行class文件是需要在jvm的环境下。但是你写完一个软件难道非得叫人安装一个JRE嘛？！是的，必须。但是这样别人用起来会不爽，于是我们就事先把java的JRE给嵌入到我们的EXE安装包，这样不是很ok！ 于是就调研了一番，最后选择eclipse+exe4j+innosetup这三个软件配合使用
附加的word文件分成三个部分进行，
  第一部分   eclipse生成可运行的jar包
      第二部分 Exe4j产生exe程序   
      第三部分   InnoSetup封装 jre到exe的运行环境下
      exe4j是非破解版，在Word中提供了注册码，主要作用是生成exe文件（运行在有JRE的环境下），由于太大并未上传，提供连接exe4j下载 。 InnoSetup.exe是一个打包程序，编程setup.exe安装版（安装之后，没有JRE也可以运行。）
主要关键点：
   exe4j 的 GUI和console两种情况的生成  
填写JRE最小版本，最大版本可以不用填。删掉InnoSetup默认的Java_home信息，导入正确的JRE文件夹（切勿小心）。 其他的都是傻瓜式安装

 
Jar包打包成可安装的EXE文件.docx
559.84 KB, 下载次数: 0
jar变exe的详细步骤
 
innosetup-5.5.5.rar
1.83 MB, 下载次数: 0
innosetup打包程序


    在word 文档里面，使用的jre不是jdk自带的，而是另外下载，当时不知道为什么直接拷贝到项目文件夹不行,报找不到jvm的错误。今天下午帅锅锅一折腾，才发现下载的jre和jdk自带的jre有些不同，jdk的jre目录的bin目录多了一个server文件夹，而同时还有一个client 文件夹（非java自带的jre只有一个client文件夹），所以其实java自带的jre文件夹有两个jvm（听赵帅说是：为了设计完程序后以用户角度体验程序效果，我表示不解） ，而也正因为这两个jvm的原因导致了程序不知道使用哪个jvm运行了，于是有了下面的方法。
   所以，基于这番调查，提出另一种方法
1.  拷贝jdk底下的jre到项目文件夹下，然后删掉bin目录下的server（删掉多余的jvm) ,然后按照word中的过程照样有效（不需要额外再去下载jre)
   测试，有效。
