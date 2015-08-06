---
layout: post
title: "把时间和键盘写进script"
date: 2015-06-13 20:00:58 +0800
comments: true
categories: Linux
---

在Linux地下，脚本(script)编程是一个快捷的方式和内核(kernel)进行交流。
通常包含以下几种脚本编程:

+ bash shell(其他shell也类似)
+ perl
+ python
+ ruby
+ 等等

我现在发现我们既可以处理文件中的信息，也同时可以处理硬件的东西，这些其实我就清楚，比如linux把很多的设备都虚拟为文件，只不过之前不知道怎么用。
接触到shell之后，我才发现比如键盘的东西都可以被写进脚本的，也就是电脑上的外接设备也是可以被写进程序当中的，当作计算机里面的一种概念。
<!--more-->


## vim的脚本化操作
把下段的代码保存为c.sh,主要功能是把双个除号改为井号，当作注释。
这样就可以把vim的action行为录为脚本。
``` sh

vim ./cData <<VIM > /dev/null 2>&1 
:%s#//#\##
:wq 
VIM 
```
>/dev/null 2>&1  拆为四部分来分析下:

1. 首先 0> 表示stdin标准输入; 1> 表示stdout标准输出; 2> 表示stderr错误输出; 
2. 符号 >  等价于 1> (系统默认为1,省略了先); 所以">/dev/null"等同于 "1>/dev/null"
3. /dev/null 代表空设备文件
4. & 可以理解为是"等同于"的意思，2>&1，即表示2的输出重定向等同于1
因此，>/dev/null 2>&1 也可以写成“1> /dev/null 2> &1”

那么本文标题的语句执行过程为：
1>/dev/null ：首先表示标准输出重定向到空设备文件，也就是不输出任何信息到终端，说白了就是不显示任何信息。
2>&1 ：接着，将标准错误输出重定向 到 标准输出，因为之前标准输出已经重定向到了空设备文件，所以标准错误输出也重定向到空设备文件。

类似还有cat

``` sh
cat >> /etc/man.config <<EOF
MANPATH  /srv/httpd/man/
EOF

cat >> /etc/profile.d/http.sh <<EOF
export PATH=/srv/httpd/bin:$PATH
EOF
```

## mysql的脚本化操作

``` sh
mysql -u$user -p$password << EOF  
FLUSH TABLES WITH READ LOCK;  
system lvcreate --snapshot -n $snap -L$snapsize /dev/$vg/$lv;  
UNLOCK TABLES;  
quit  
EOF 
```
## gnuplot的脚本化操作

``` sh
gnuplot<<EOF   
set ter 'png'   
set out '$i/cpUp.png'  
plot "$i/cpUp.txt" t 'cp' w lp  
EOF 
```



## 获得键盘输入的两种方式

### 使用read
```sh

read -p "input a val:"  val
echo $val

```

### 使用stty

``` sh

get_char(){
   SAVEDSTTY=`stty -g`
   stty -echo
   stty raw
   dd if=/dev/tty bs=1 count=1 2> /dev/null
   stty -raw
   stty echo
   stty $SAVEDSTTY
}
echo "Press any key to continue..."
char=`get_char`
read k
echo $k
```

## 不错的一个菜单输入

``` sh

#字颜色：30—–37  
#字背景颜色范围：40—–47 

while :
do
    echo "\033[5m 1) china \033[0m" 
    echo "\033[32m 2) America \033[0m" 
    echo "\033[33m 3) Japan \033[0m" 
    echo "\033[34m 4) England \033[0m" 
    echo "\033[35m 5) France \033[0m" 
    echo "\033[36m 6) Spain \033[0m" 
    echo "\033[37m 7) Brazil \033[0m" 
    echo "\033[40;37m 8) Turkey \033[0m" 
    echo "\033[41;37m 9) Italy \033[0m" 
    echo "\033[42;37m 10) Germany \033[0m" 
    echo "\033[43;37m 11) Arabi \033[0m" 

    read -p "Please Enter the number [1-11]" val
    case $val in
        1) echo "\033[5m 1) You input the --> china\n \033[0m" 
            ;;
        2) echo "\033[32m 2) You input the --> America \n\033[0m" 
            ;;
        3) echo "\033[33m 3) You input the --> Japan\n \033[0m" 
            ;;
        4) echo "\033[34m 4) You input the --> England \n\033[0m" 
            ;;
        5) echo "\033[35m 5) You input the --> France \n \033[0m" 
            ;;
        6) echo "\033[36m 6) You input the --> Spain\n \033[0m" 
            ;;
        7) echo "\033[37m 7) You input the --> Brazil\n \033[0m" 
            ;;
        8) echo "\033[40;37m 8) You input the --> Turkey\n \033[0m" 
            ;;
        9) echo "\033[41;37m 9) You input the --> Italy\n \033[0m" 
            ;;
        10) echo "\033[42;37m 10) You input the --> Germany\n \033[0m" 
            ;;
        11) echo "\033[43;37m 11) You input the --> Arabi\n \033[0m" 
            ;;
        *) exit 0
            ;;
    esac
    echo ""
done

```

![结果](/images/menu.png)

持续更新本文的script的内容....

