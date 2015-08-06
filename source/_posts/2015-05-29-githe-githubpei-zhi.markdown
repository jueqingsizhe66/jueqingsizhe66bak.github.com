---
layout: post
title: "git和github配置"
date: 2015-05-29 21:14:39 +0800
comments: true
categories: git
---

git是一个Linus开发的一个版本控制系统，其实就是一个文件系统而已。
下面是我总结的一些操作步骤，最终的目的是你可以提交信息到github，
并了解git和github之间的关系。
<!--more-->

## 1. 打开git 
![打开][open]
## 2. 配置本地git环境
![配置][config]
## 3. Github 注册用户
![注册][register]

账号邮箱最好是你刚才git本地配置！ 密码自己选

## 4. ssh 公匙生成（一路回车 就好，由于我已经配置好了公匙所以显示是否覆盖。。。。。）
![ssh][ssh]
## 5. 拷贝公匙到黏贴板（ubuntu 得安装clip命令： apt-get install clip
![拷贝][copy]
## 6. 登陆你的github用户
![登录][enter]
自动切换到登陆后的界面
![界面1][interface]
然后，你点击配置，如上图所示。
![界面2][interface2]

按照上图，打开添加公匙界面，然后利用下图的Add SSH key添加公匙。Title的内容随便写。。。。。。。主要是把公匙框填好即可
![界面3][interface3]
![取名字][name]


网上说要测试，但是大部分情况，这样就算通过了，你也可以按照https://help.github.com/articles/generating-ssh-keys/
     这个连接的说法进行测试，。。。。。。我现在一般不测试。。。。



## 7. 现在开始下载RupengFanYi/fanyi 的原始版本内容 
我是想把已经翻译的放在Finished文件夹下
![文件夹][folder]

我们现在开始克隆这个项目《-----做正事了！！


Git clone
![复制gtk项目][clone]

记住，你可以切换到任意目录下进行git clone,你也可以把你的这个fanyi文件夹拷贝到任意文件夹
Cd f:/  就表示切换到f盘
![切换目录][cd]


既然已经git clone 了之后，我们现在开始进行第八步

## 8.  把你已经翻译的文件 比如\*.html 复制到Finished 文件夹表示已经翻译的
一般不动fanyi文件夹里面的内容！我们只是把翻译好的文件拷贝到Finished然后上传

然后再切回到主工作区（表示 fanyi文件夹下）
![完成][finished]

下面字符界面操作流程：
![添加][add]

但是也可能push出现问题，
比如Gtk翻译01犯的错误
![一个错误][error]

这个是因为，我实现更新了文件，而他的当前文件夹没有！所以发生了冲突


![报错内容][errorContent]
这就是报错的内容，所以你现在的解决办法办法是：
![解决][solve]
然后就ok了
![可以][ok]




## 总结
以后我们可以这样做
![步骤1][future1]
![步骤2][future2]



Gtk翻译01说有一个小问题：（仅供大家参考）
![注意][note]



[open]:/images/git/1.png
[config]:/images/git/config.png
[register]:/images/git/register.png
[ssh]:/images/git/ssh.png
[copy]:/images/git/copy.png
[enter]:/images/git/enter.png
[interface]:/images/git/interface.png
[interface2]:/images/git/interface2.png
[interface3]:/images/git/interface3.png
[name]:/images/git/name.png
[folder]:/images/git/folder.png
[clone]:/images/git/clone.png
[cd]:/images/git/changedirectory.png
[finished]:/images/git/finished.png
[add]:/images/git/solve.png
[error]:/images/git/oneeroor.png
[solve]:/images/git/solve.png
[ok]:/images/git/ok.png
[errorContent]:/images/git/errorContent.png
[future1]:/images/git/future.png
[future2]:/images/git/future2.png
[note]:/images/git/note.png
