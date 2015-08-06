---
layout: post
title: "搭建gitlab服务器及使用"
date: 2015-06-09 19:22:07 +0800
comments: true
categories: git
---

现在有很多的公开的代码服务器
比如最为出名的是[github](http://github.com)
Haskell开发的[Darcs](http://darcs.net/Binaries)
国内比较出名的是

+ [开源中国](http://git.oschina.net)
+ [京东](https://code.jd.com)

然而，很多时候我们并不想把所有的东西都公开，于是想有一个私有的云平台，这样
我们就可以让很多内部的人员(局域网)使用了，gitlab正是为一堆比较懒的人设计的基于git的平台搭建。

<!--more-->
## 参考链接

+ [CentOS下一键安装GitLab](http://blog.csdn.net/yanjiangbo/article/details/39154573)
+ [gitlab一键安装笔记](http://www.2cto.com/os/201411/353292.html)
+ [使用Gitlab一键安装包后的日常备份恢复与迁移](http://segmentfault.com/a/1190000002439923)
+ [一键安装 gitlab7 on rhel6.4 并设置邮件发送](http://www.2cto.com/os/201408/323795.html)

### 开始安装

官网下载(我现在下载的版本是7.11.4.0  之前用的是7.1.1.0)
[https://bitnami.com/stack/gitlab/installer](https://bitnami.com/stack/gitlab/installer)


```
gitlab(更新版本的采用的是图形界面，之前的版本采用的是命令流,全面的图形化更方便些。现在是连启动都是图形界面了,比如gitlabci 也直接不需要用./ctlscript.sh start)
EmaikAddress : -977962857@qq.com
Login :xinran
passowrd:+++++++

配置gmail（省的自己配置)
```

使用的过程既可以是在网页中直接使用，也可以直接使用类似github的命令流操作，
但是前提是必须配置好服务器，类似于[github的配置过程](https://help.github.com/articles/set-up-git/)

## 安装完毕，进行ssh配置的注意点

### 问题是rakegemes.rb里的一个block 有问题，提醒rake is not the part of the gem

我分享一下解决方法，我的登录用户假如是fluid,那么我们现在使用

``` sh
ssh-keygen -t rsa -C "你的登录邮箱gitlab账户"

```
然后你就可以复制在/home/fluid/.ssh/id_rsa.pub，粘帖到你的gitlab的网页管理中添加ssh keys ,
以前我犯的错误是以为提取gitlab在安装的时候新建的git 用户的authority_keys,现在看来还真是跟github的配置有点像，这样之后还有一个小问题，
你得是在登录用户fluid才是可以git push等基本操作，而在root用户则是有权限限制，但是紧接着我在root用户下再次ssh-keygen…
这样在/root/.ssh/id_rsa.pub又有一个新的key，你只要也把他添加到gitlab管理界面的profile setting里的ssh keys 添加一下就可以了.
所以变成和github一样可以用root用户，其实罗嗦那么久还是ssh-keygen问题，还有记得对应用户的key问题


## 开始使用
### 我在网页上创建了一个项目

得到了如下信息:

Global Setting
``` sh
git config --global user.name "Administrator"
git config --global user.email "977962857@qq.com"

```

Create a new repository
```sh
git clone git@127.0.1.1:xinran/f708OpenFoam.git cd f708OpenFoam
touch README.md
git add README.md
git commit -m "add README"
git push -u origin master
```

Existing folder or Git repository
``` sh
cd existing_folder
git init
git remote add origin git@127.0.1.1:xinran/f708OpenFoam.git git push -u origin master
```

然后我cd /root/.ssh/
因为我之前已经设置好了git配置，不懂的人可以参考[git和github的配置](http://jueqingsizhe66.github.io/blog/2015/05/29/githe-githubpei-zhi)
然后拷贝id.pub的内容

### 测试是否走通了

我们很想知道到底我们已经配置好了服务器?也很想知道局域网内的客户端是否可以访问服务器？

测试方法：

``` sh
root at fluidman-OptiPlex-990 in /opt/gitlab-7.1.1-0[21:17:36下午] 
$ ssh -T git@127.0.1.1
Welcome to GitLab, Anonymous!
```

局域网人员测试:
``` sh
比如我的127.195.172.64别人只要在浏览器输入 127.195.172.64:80然后就可以了！！
```

### reate Repository（创建仓库）

``` sh
mkdir common-util
cd common-util
git init
touch README
git add README
git commit -m 'first commit'
git remote add origin git@127.0.0.1:devteam/common-util.git
git push -u origin master
```
### 对于已存在Git项目：

```sh
cd existing_git_repo 
git remote add origin git@127.0.0.1:devteam/common-util.git 
git push -u origin master

```

开发完之后，你进行

+ 测试
+ 检查
+ 再测试等过程 

还需要进行提交.

## 多用户问题
### 注意验证用户

会被弄到垃圾邮件当中，
并且验证链接需要注意的是一定得改一下IP，
比如：

``` sh
http://127.0.1.1/users/confirmation?confirmation_token=AEHLjx2WR21sb3zULW5h
```
我的内网IP是(ubuntu :ifconfig    windows: cmd--> ipconfig 进行查看)
121.195.172.217
那么就变为
``` sh
http://121.195.172.217/users/confirmation?confirmation_token=AEHLjx2WR21sb3zULW5h
```
__这步的一个完美解决方案是在装gitlab的时候domain不要填127.0.1.1而应该填你的固有IP（动态获取的IP的话，最好保持不断网，基本上也能够维持IP不变）这样以后就不许要修改了__

### 我的目录分门别类(目的是一个项目一个文件夹)
```sh
/
├── dns-of-incompact3d
├── dns-of-semtex
├── fortran-learning-and-some-tools
├── lisper
├── nreal-fast

```

### 我的一次使用提交

``` sh
root at fluidman-OptiPlex-990 in /latex-chines-english[10:05:08上午]  on git:develop running make, make, make, and make
$ git push -u origin develop
Counting objects: 208, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (205/205), done.
Writing objects: 100% (208/208), 23.27 MiB | 7.67 MiB/s, done.
Total 208 (delta 40), reused 0 (delta 0)
To git@127.0.1.1:xinran/latex-chines-english.git
 * [new branch]      develop -> develop
分支 develop 设置为跟踪来自 origin 的远程分支 develop。
```

很多事情关键在于__坚持__,代码服务器现在有了，关键是几个人协力合作的问题,比如一起往openfoam的风电(疯癫)方向发展。



