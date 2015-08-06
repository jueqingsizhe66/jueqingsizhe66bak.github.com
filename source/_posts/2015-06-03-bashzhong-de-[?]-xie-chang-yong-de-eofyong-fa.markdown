---
layout: post
title: "Bash中的一些常用的EOF用法"
date: 2015-06-03 14:14:22 +0800
comments: true
categories: Linux
---

问题：我如何在shell当中批处理产生图片
解决方案：使用gnuplot即可，但是我如何获得shell变量，于是我使用
         gnuplot<<EOF
         ...<command>
         EOF
的方式导入。即可，后来不断地接触到`<<VIM  ..VIM`也是可以的
<!--more-->


``` sh
vim /etc/nginx/nginx.conf <<VIM > /dev/null 2>&1 
:%s/#gzip/server_tokens off;\r    gzip/ 
:%s/#gzip/gzip/ 
:wq 
VIM 


mysql -u$user -p$password << EOF  
FLUSH TABLES WITH READ LOCK;  
system lvcreate --snapshot -n $snap -L$snapsize /dev/$vg/$lv;  
UNLOCK TABLES;  
quit  
EOF 

gnuplot<<EOF   
set ter 'png'   
set out '$i/cpUp.png'  
plot "$i/cpUp.txt" t 'cp' w lp  
EOF 


cat > /etc/yum.repos.d/nginx.repo <<EOF 
[nginx] 
name=nginx repo 
baseurl=http://nginx.org/packages/centos/${VERSION}/${ARCH}/ 
gpgcheck=0 
enabled=1 
EOF 

```

命令的结果可以通过 %> 的形式来定义输出,其中 %> 代表文件描述符
我们将这个命令组合：“>/dev/null 2>&1”  拆为四部分来分析下:
1. 首先 0> 表示stdin标准输入; 1> 表示stdout标准输出; 2> 表示stderr错误输出; 
2. 符号 >  等价于 1> (系统默认为1,省略了先); 所以">/dev/null"等同于 "1>/dev/null"
3. /dev/null 代表空设备文件
4. & 可以理解为是"等同于"的意思，2>&1，即表示2的输出重定向等同于1
因此，>/dev/null 2>&1 也可以写成“1> /dev/null 2> &1”

那么本文标题的语句执行过程为：
1>/dev/null ：首先表示标准输出重定向到空设备文件，也就是不输出任何信息到终端，说白了就是不显示任何信息。
2>&1 ：接着，将标准错误输出重定向 到 标准输出，因为之前标准输出已经重定向到了空设备文件，所以标准错误输出也重定向到空设备文件。


下面是一些来自[Linux-shell-administration](https://github.com/Linux-Admins/Shell)的脚本。

``` sh
来自Shell/shellall/web/httpd：
cp /srv/httpd-2.4.10/conf/httpd.conf{,.original}
vim /srv/httpd-2.4.10/conf/httpd.conf <<VIM > /dev/null 2>&1
:171,171s/^$/ServerName localhost:80/
:453,453s/#//
:wq
VIM

cat >> /etc/man.config <<EOF
MANPATH  /srv/httpd/man/
EOF


cat >> /etc/profile.d/http.sh <<EOF
export PATH=/srv/httpd/bin:$PATH
EOF
```
