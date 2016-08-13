---
layout: post
title: "ubuntu wifi connection in  your writing system"
date: 2016-07-21 20:51:43 +0800
comments: true
categories: life
---
 Ubuntu 下连接无线网络，大体有两种办法

1. [ 第一种是用 iwconfig 命令（经常无效）](#1)
2. [ 第二种是使用 wpa_cli(有效) ](#2) 
3. [ window encoding transform into ubuntu](#3)

<!--more-->
******

搜索无线网 iwlist wlan0 scan |grep "ESSID"|less

记下你要的wifi名字 essid

******

<h2 id="1">1. 第一种是用 iwconfig 命令</h2>

但是这个命令在连接时经常报错，然后无法连接，这里便不做说明了。

<h3> a. 命令行配置wifi</h3>

```
连接无密码的无线网 iwconfig wlan0 essid ChinaNet　其中ChinaNet是搜索到的无线网essid
连接有密码的无线网 iwconfig wlan0 essid ChinaNet key xxxx　其中xxxx是密码
启用无线网卡 ifconfig wlan0 up

```
<h3>b. 动态获取IP</h3>

```
通过dhcp获取IP　dhclient wlan0　或 dhcpcd wlan0
```

******
<h2 id="2">2. 第二种是使用 wpa_cli </h2>

（注：我碰到的情况是，这个命令只可以在恢复模式下的shell中使用，在gnome桌面中是不管用的）

操作步骤：

<h3>a. 进入交互模式 （类似于方法一的wifi名和密码设置）</h3>
 
```
shell 中输入 wpa_cli 进入交互，然后输入

    add_network

    # 执行后会返回一个数字（一般是0），下面会用到，暂且称为 NID

    set_network NID ssid “WIFI SSID，根据你的网络环境填写”

    set_network NID psk “无线密码”

    bssid NID "BSSID", 设置bssid。

    enable_network NID

    # 启用网络，成功后会返回 CONNECT TO XXXX的

    然后就可以输入q退出了
```

<h3>b. 进入dhclient获取wifi(和方法1一样)</h3>

退出后使用 dhclient wlan0 获取一下网络地址就可以使用了

******

<h2 id="3">3. windows system encoding transform into ubuntu system</h2>

```sh

iconv -f gbk -t  utf8 /mnt/ubuntuwifi.txt > /mnt/ubuntu1.txt

```

