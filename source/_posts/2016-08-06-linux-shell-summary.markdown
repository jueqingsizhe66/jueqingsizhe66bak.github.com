---
layout: post
title: "linux shell summary"
date: 2016-08-06 01:11:00 +0800
comments: true
categories: Linux
---

其实之前写过一个[bash的总结][6]，现在只不过是再做一次shell编程总结

首先得明白我们现在讲的shell物理上存在那个层面：

![linux_shell][12]

他只是用户连接kernel的一个中间层，用于操作kernel来和硬件进行交互的软件平台。

<font color="green">Main Frame:</font>

+ [base part](#base)
+ [key kernal part](#key_kernel)
+ [advance part](#advance)


<font color="green">Detailed Frame:</font>

1. [1. apt-get](#apt-get)
2. [2. wget and curl](#wgetAndCurl)
3. [3. Linux常用的系统检测命令](#system)
4. [4. 工作目录切换命令](#changeDir)
5. [5. 文本文件编辑命令](#text)
6. [6. 打包压缩文件命令](#tar)
7. [7. 文件目录管理](#dirManage)
8. [8. 用户与组管理命令](#user)
9. [9. 关键命令总结](#kernel)
10. [10. bash 算数操作和表达式](#arithmetic)
11. [11. 一个工作流命令crontab](#crontab)
12. [12. kill命令删除僵尸进程](#kill)
13. [13. Bash数组 ](#bashArray)
14. [14. iconv解决中文识别的问题](#iconv)


linux教会你很重要的思想就是命令流.Stream thinking is most important in the linux system(不是之一).
正如[think-like-a-git][8]所倡导的图形原理一样，犹如一个一个的[Nodes and edges][9]组成你的求解图
![社交图][10]. 而且linux stream thinking作为现代软件设计中的基石，基本上会被所有的软件设计过程中
所参考，所以不时的你可以在其他的语言中使用得到比如git，matlab，swi-prolog的cd命令等。



<!--more-->


<br/>
<br/>
<br/>
<hr/>
<h2 id="base"><font color="red" size="8">基础部分</font></h2>
<hr/>
<br/>
<br/>
<br/>

My crocodile shell only for thinking about shell programming.
![myeshell][11]
<h2>一些不需要介绍的命令(这些命令就好像吃饭睡觉自然而然)</h2>

1. cd  
2. ls
3. reboot
4. halt -p
5. date
6. echo
7. mount -o loop /dev/sdb4 /mnt (配合fdisk -l 查看U盘所在的/dev的位置，也就是u盘硬件的虚拟化文件,体现的思想是所有设备即文件，更高一层是所有对象即文件，再往上是世界即文件)

cd means change directory, ls means list, echo has some history,you can try [bing][1] or [baidu][2].
`ls -al`可以用来显示系统隐藏命令.

<h2>常用的系统工作命令</h2>

<h3 id="apt-get">1. apt-get </h3>

我进入ubuntu系统最经常做的，可能就是`sudo -s`,然后`apt-get update` 看一下可不可以更新一下系统(特殊版本更新可能apt-get dist-update).

而且你也经常可能需要安装一些相关软件，这时候就需要`apt-get install <软件名字>`;
当然你也可能卸载一些运用，这时候就需要`apt-get remove <软件名字>`.

类似的你也可以通过wget或者curl下载相关的源文件进行安装


<h3 id="wgetAndCurl"> 2. wget and curl</h3>


1. curl是libcurl这个库支持的，wget是一个纯粹的命令行命令。
2. curl支持更多的协议。curl supports FTP, FTPS, HTTP, HTTPS, SCP, SFTP, TFTP, TELNET, DICT, LDAP, LDAPS, FILE, POP3, IMAP, SMTP and RTSP at the time of this writing. Wget supports HTTP, HTTPS and FTP.
3. curl 默认支持HTTP1.1（也支持1.0），而wget仅仅支持HTTP1.0规范。引用wget的man page中的一段话吧，Please be aware that Wget needs to know the size of the POST data in advance. It's not quite clear how to work around this limitation inherent in HTTP/1.0. Although HTTP/1.1 introduces chunked transfer that doesn't require knowing the request length in advance, a client can't use chunked unless it knows it's talking to an HTTP/1.1 server.  And it can't know that until it receives a response, which in turn requires the request to have been completed -- a chicken-and-egg problem.
4. curl在指定要下载的链接时能够支持URL的序列或集合，而wget则不能这样;
5. <font color='red'>wget支持递归下载</font>，而curl则没有这个功能。（这是wget的一个主要好处，wget也是有优势的）


<h4 id="curl"> curl（文件传输工具） </h4>

常用参数可以使用`curl --help`

使用示例：

```
例1：抓取页面到指定文件，如果有乱码可以使用iconv转码
 curl -o baidu.html www.baidu.com
 curl –s –o baidu.html www.baidu.com |iconv -f utf-8  #减少输出信息
例2：模拟浏览器头（user-agent）
 curl -A "Mozilla/4.0 (compatible;MSIE 6.0; Windows NT 5.0)" www.baidu.com
例3：处理重定向页面
 curl –L http://192.168.1.100/301.php  #默认curl是不处理重定向
例4：模拟用户登陆，保存cookie信息到cookies.txt文件，再使用cookie登陆
 curl -c ./cookies.txt -F NAME=user -F PWD=***URL            #NAME和PWD是表单属性不同，每个网站基本都不同
 curl -b ./cookies.txt –o URL
例5：获取HTTP响应头headers
 curl -I http://www.baidu.com
 curl -D ./header.txt http://www.baidu.com  #将headers保存到文件中
例6：访问HTTP认证页面
 curl –u user:pass URL
例7：通过ftp上传和下载文件
 curl -T filename ftp://user:pass@ip/docs  #上传
 curl -O ftp://user:pass@ip/filename  #下载
```

<h4 id="wget">wget（文件下载工具）</h4>

常用参数如可以使用`wget --help` 

```
2.1 启动参数
-V，--version：显示版本号
-h，--help：查看帮助
-b，--background：启动后转入后台执行
2.2 日志记录和输入文件参数
-o，--output-file=file：把记录写到file文件中
-a，--append-output=file：把记录追加到file文件中
-i，--input-file=file：从file读取url来下载
2.3 下载参数
-bind-address=address：指定本地使用地址
-t，-tries=number：设置最大尝试连接次数
-c，-continue：接着下载没有下载完的文件
-O，-output-document=file：将下载内容写入到file文件中
-spider：不下载文件
-T，-timeout=sec：设置响应超时时间
-w，-wait=sec：两次尝试之间间隔时间
--limit-rate=rate：限制下载速率
-progress=type：设置进度条
2.4 目录参数
-P，-directory-prefix=prefix：将文件保存到指定目录
2.5 HTTP参数
-http-user=user：设置http用户名
-http-passwd=pass：设置http密码
-U，--user-agent=agent：伪装代理
-no-http-keep-alive：关闭http活动链接，变成永久链接
-cookies=off：不使用cookies
-load-cookies=file：在开始会话前从file文件加载cookies
-save-cookies=file：在会话结束将cookies保存到file文件
2.6 FTP参数
-passive-ftp：默认值，使用被动模式
-active-ftp：使用主动模式
2.7 递归下载排除参数
-A，--accept=list：分号分割被下载扩展名的列表
-R，--reject=list：分号分割不被下载扩展名的列表
-D，--domains=list：分号分割被下载域的列表
--exclude-domains=list：分号分割不被下载域的列表

```
使用示例：

```
例1：下载单个文件到当前目录下，也可以-P指定下载目录
 wgethttp://nginx.org/download/nginx-1.8.0.tar.gz
例2：对于网络不稳定的用户可以使用-c和--tries参数，保证下载完成
 wget --tries=20 -c http://nginx.org/download/nginx-1.8.0.tar.gz
例3：下载大的文件时，我们可以放到后台去下载，这时会生成wget-log文件来保存下载进度
 wget -b http://nginx.org/download/nginx-1.8.0.tar.gz
例4：可以利用—spider参数判断网址是否有效
 wget --spider http://nginx.org/download/nginx-1.8.0.tar.gz
例5：自动从多个链接下载文件
 cat url_list.txt  #先创建一个URL文件
http://nginx.org/download/nginx-1.8.0.tar.gz
http://nginx.org/download/nginx-1.6.3.tar.gz
 wget -i url_list.txt
例6：限制下载速度
 wget --limit-rate=1m http://nginx.org/download/nginx-1.8.0.tar.gz
例7：登陆ftp下载文件
 wget --ftp-user=user --ftp-password=pass ftp://ip/filename
```

<font color="red" size="8">我常用的下载网站的方式</font>
```
 wget -r -k -L -np -c --execute robots=off http://www.inf.ed.ac.uk/teaching/courses/aipp/#Lectures_
```
-L 递归时不进入其它主机，如wget -c -r www.xxx.org/	
如果网站内有一个这样的链接：www.yyy.org，不加参数-L，就会像大火烧山一样，会递归下载www.yyy.org网站	

-nd 递归下载时不创建一层一层的目录，把所有的文件下载到当前目录,所以<font color="red">注意去掉-nd选项</font>

-np 递归下载时不搜索上层目录，如wget -c -r www.xxx.org/pub/path/	
没有加参数-np，就会同时下载path的上一级目录pub下的其它文件	

-k 将绝对链接转为相对链接，下载整个站点后脱机浏览网页，最好加上这个参数	,

-c即–continue选项，这就是大名鼎鼎的“断点续传”。无论你之前使用哪个下载工具下载了一半的文件

-r   参数是指使用递归下载



<h3 id="system">3. Linux常用的系统检测命令</h3>


1. 内核版本检测:
uname命令用于查看系统内核版本等信息,比如`uname -a`,`uname -s`.
2. 网络检测:ifconfig
3. 系统负载检查
    uptime命令用于查看系统负载情况，输出内容分别为系统当前时间，系统已运行时间，当前在线用户以及平均负载值。
4. 内存检测
    free命令用于显示当前系统中内存的使用量情况,`free -m`
5. 硬盘检测
    fdisk -l 或者 du -sh .
6. 用户检测
    who命令用于查看当前登录主机的用户情况
    last命令用于查看所有系统的登入纪录
7. 历史命令检测
    history命令用户显示历史执行过的命令

<h3 id="changeDir">4. 工作目录切换命令</h3>

比如： cd ,  cd ~, cd .. ,cd ../.. ,cd -(切换到上一次访问目录),pwdm也是经验的积累，多查查即可.
<h3 id="text">5. 文本文件编辑命令</h3>

比如：cat,tea,more,less,head,tail等，用到的时候查一下即可。

`head -n 20 <文件名>` :查看前20行.

`cat tr.txt | tr[a-z][A-Z]` : 文件大小写转换

比较特殊的是diff，可用于比较两个文件的差异
`diff a.txt b.txt`

<h3 id="tar">6. 打包压缩文件命令</h3>

```
打包并压缩文件：
tar -czvf 压缩包名.tar.gz 文件名
解压并展开压缩包
tar -xzvf 压缩包名.tar.gz

```
<h3 id="dirManage">7. 文件目录管理</h3>
```
touch 文件名
mkdir 文件夹名
cp 源文件 目标文件
mv aaa bbb
删除普通文件并提示确认信息
rm 文件名
删除普通文件或目录文件，不提示确认信息
rm -rf 文件或目录名

```

其实，应该要加上文件的<font color='red'>重定向</font>，比如`> ,>>`

<h3 id="user">8. 用户与组管理命令</h3>
我用的比较少，基本上就是useradd,passwd 用户，userdel,groupadd 等,用到的时候再查吧
`chmod -R 777 <文件名或者目录>` ,用的比较多


更多的信息参考[linux常用工作命令总结][3]

<br/>
<br/>
<br/>
<hr/>
<h2 id="key_kernel"><font color="red" size="8">核心部分</font></h2>
<hr/>
<br/>
<br/>
<br/>

<h3 id="kernel">9. 关键命令总结 </h3>

正如linux哲学所倡导的

+ Do just a few things well (it’s not too big to understand) 短小精悍. 
+ use a little extension of standard libraries(BLAS,lapack,MPI etc)

1. [sort](#sort)
2. [cut](#cut)
3. [sed](#sed)
4. [awk](#awk)
5. [uniq](#uniq)
6. [grep](#grep)
7. [find](#find)
8. [seq](#seq)

<hr>
<br/>
<br/>
<br/>

<h4 id="sort">sort命令执行排序</h4>

sort是一个重要的命令，你可以在许多重要的UNIX/LINUX shell中见到它的身影:只要有数据存在，并有检索的需求，往往就有sort的支持。如果给UNIX/LINUX的命令论资排辈，sort绝对可以进入前十名。
上边所做的只是sort命令冰山一角，sort命令的强大之处远不止于此。除了对文本
行和字段进行排序，sort命令和其他命令结合使用〔它本身就被设计成过滤器的模式)将实现强大的功能，例如，可以对文本块进行排序处理等.并且，sort命令的效率是值得称道的:自从它问世起，已经有许多人对它进行研究、优化和调整。它肯定比你写的排序算法工作得更好，相信它，而不要尝试去重复发明轮子〔编写冗余的脆弱的排序算法)。
但是，注意，sort命令是不稳定的口排序算法的稳定性指的是:两条相同的记录输入顺序和输出顺序保持不变。

```
$ sort -n -k 2 -t : facebook.txt
apple:10:2.5
orange:20:3.4
banana:30:5.5
pear:90:2.3

我们使用冒号作为间隔符，并针对第二列来进行数值升序排序，结果很令人满意。

```

-n ：表示数字形式认为-k 2的列
-k: 表示第几列 -k是一个前缀表达式
-t: 指定哪个分隔符， 可以这样说 
<font color="green">所有的选项都是一个前缀表达式，后面跟着一个参数</font>

<h4 id="cut">cut命令通过列来提取文本字符</h4>

查看当前系统中所有用户的名称：
参数作用: -d以":"来做分隔符，-f参数代表只看第一列的内容
```
cut -d: -f1 /etc/passwd
root
bin
daemon
adm
...

```

<h4 id="sed">sed命令用于文本编辑、修改、替换、删除</h4>

sed相比于awk的主要不同是，他处理的最小单位是行，而[awk](#awk)处理的最小单位是字段(当然他也是逐行处理)，[grep](#grep)和[find](#find)处理的最小字段是文件

参考： 

+ [sed基础部分][16]
+ [sed模式空间(pattern space)和保持空间(hold space)][18]，讲保留和暂存都不好，我觉得应该是保持空间。
+ [sed chinaunix模式空间][17]

模式空间，即为处理文件中一行内容的一个临时缓冲区，处理完一行之后就会把模式空间中的内容打印到标准输出，然后自动清空缓存，
保持空间，是sed中的另外一个缓冲区，此缓冲区正如其名，不会自动清空，但也不会主动把此缓冲区中的内容打印到标准输出中(也就是说<font color="red">只有内容放置到
模式空间才能输出</font>。
而是需要以下sed命令进行处理：

```
          d     Delete pattern space.  Start next cycle.    删除pattern space的内容，开始下一个循环.
          h、 H    Copy/append pattern space to hold space.   复制/追加pattern space的内容到hold space.
          g、 G    Copy/append hold space to pattern space.   复制/追加hold space的内容到pattern space.
          x      Exchange the contents of the hold and pattern spaces.    交换hold space和pattern space的内容.
```

具体例子参考[SED的暂存空间和模式空间][19]
```
 seq 5|sed '/3/{x;p;x}'
1
2

3
4
5
$ seq 5|sed '/[34]/{x;p;x}'
1
2

3

4
5
$ seq 5|sed '/[34]/{x;x}'（交换来交换区 等于没做）
1
2
3
4
5
$ seq 5|sed '/[34]/{x;p;x;p}'
1
2

3
3

4
4
5

```

<h4 id="awk">awk命令用于文本编辑、修改、替换、删除</h4>
参考： [awk-Summary][4]

<h4 id="uniq">uniq命令去除重复的搜索行</h4>
可以使用`uniq --help`获得帮助信息。

```
uniq -c : to get some statistical info
uniq -d : show the repetive part(noted: only continual)
uniq -u : show uncontinual repetive part.
```

<h4 id="grep">grep命令用于对文本进行搜索</h4>

搜索某个文件中某个关键词：
`grep 关键词 文本文件`

进一步可以查看[shell的正则表达式][14]用法和[零宽断言][15],
还有一些[拓展性的语法][13],另外一个[繁体字版本的正则表达式讲解][20].

<h4 id="find">find命令用于查找文件</h4>


搜索在/etc/中所有以host开头的文件：
```
find /etc -name "host*" -print 
/etc/hostname
/etc/hosts
/etc/backup/2/hosts
/etc/selinux/targeted/modules/active/modules/hostname.pp

```
find命令也可以进一步参考运用[allfilesIntoOne][5].
```
find . -type f -size -100M +100M 大于100M -100M小于10M 

```

<h4 id="seq">seq 序列输出</h4>

```
$ seq -f "%02g" 1 30
01
02
03
04
05
06
07
08
09
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30


意义：

for i in `seq -f "%02g" 1 30`;do cp ampthb$i/*.rar .;done;

或者
seq -f"ampthb%02g"  1 30;

seq -sw
```

<br/>
<br/>
<br/>
<hr/>
<h2 id="advance"><font color="red" size="8">高级部分</font></h2>
<hr/>
<br/>
<br/>
<br/>

<h3 id="arithmetic">10. bash 算数操作和表达式</h3>

记录一下特殊的符号的使用。

bash shell解析的都是字符，比如3也变成了一个字符，而如果要进行数值运算呢？ 只能利用
类似于slash的反转效果，让其变成数字的效果，
(())专门来做数值运算.
[[]]专门来做逻辑判断,比如[],也就是if test更强一些.

另外地，${}开头的都是针对字符串的操作，可以剪切替换
```
${name:-default} 使用一个默认值（一般是空值）来代替那些空的或者没有赋值的变量name；
${name:=default}使用指定值来代替空的或者没有赋值的变量name；
${name:?message}如果变量为空或者未赋值，那么就会显示出错误信息并中止脚本的执行同时返回退出码1。
${#name} 给出name的长度
${name%word} 从name的尾部开始删除与word匹配的最小部分，然后返回剩余部分
${name%%word} 从name的尾部开始删除与word匹配的最长部分，然后返回剩余部分
${name#word} 从name的头部开始删除与word匹配的最小部分，然后返回剩余部分
${name##word} 从name的头部开始删除与word匹配的最长部分，然后返回剩余部分

```

附加一个子串替换和子串提取:
```
子串替换：
${string/substring/replacement}
使用$replacement来替换第一个匹配的$substring.

${string//substring/replacement}
使用$replacement来替换所有匹配的$substring.

${string/#substring/replacement}
如果$substring匹配$string的开头部分, 那么就用$replacement来替换$substring.

${string/%substring/replacement}
如果$substring匹配$string的结尾部分, 那么就用$replacement来替换$substring.


子串提取的方法主要有：直接到指定位置求子串，字符匹配求子串。
${string:position}
在$string中从位置$position开始提取子串.

如果$string是"*"或者"@", 那么将会提取从位置$position开始的位置参数.[1]

${string:position:length}
在$string中从位置$position开始提取$length长度的子串.
```
更细节信息可以参考[Bash智能分类][6],[Bash Beginer Guide][7].


<h3 id="crontab">11. 一个工作流命令crontab</h3>
下面是我曾经放在`crontab -e` 的一断控制流
```
crontab：
0 23 19 6 * sh /home/simulation2_128-117-128Re4200/main.sh
30 23 19 6 * sh /home/simulation2_128-117-128Re4200/main.sh
0 0 20 6 * sh /home/simulation2_128-117-128Re4200/main.sh
30 0 20 6 * sh /home/simulation2_128-117-128Re4200/main.sh
0 1 20 6 * sh /home/simulation2_128-117-128Re4200/main.sh
30 1 20 6 * sh /home/simulation2_128-117-128Re4200/main.sh
0 2 20 6 * sh /home/simulation2_128-117-128Re4200/main.sh
30 2 20 6 * sh /home/simulation2_128-117-128Re4200/main.sh
0 3 20 6 * sh /home/simulation2_128-117-128Re4200/main.sh
30 3 20 6 * sh /home/simulation2_128-117-128Re4200/main.sh
0 4 20 6 * sh /home/simulation2_128-117-128Re4200/main.sh
30 4 20 6 * sh /home/simulation2_128-117-128Re4200/main.sh
0 5 20 6 * sh /home/simulation2_128-117-128Re4200/main.sh
30 5 20 6 * sh /home/simulation2_128-117-128Re4200/main.sh
0 6 20 6 * sh /home/simulation2_128-117-128Re4200/main.sh
30 6 20 6 * sh /home/simulation2_128-117-128Re4200/main.sh
0 7 20 6 * sh /home/simulation2_128-117-128Re4200/main.sh
30 7 20 6 * sh /home/simulation2_128-117-128Re4200/main.sh
0 8 20 6 * sh /home/simulation2_128-117-128Re4200/main.sh
30 8 20 6 * sh /home/simulation2_128-117-128Re4200/main.sh
0 9 20 6 * sh /home/simulation2_128-117-128Re4200/main.sh
30 9 20 6 * sh /home/simulation2_128-117-128Re4200/main.sh
0 10 20 6 * sh /home/simulation2_128-117-128Re4200/main.sh

```

<h3 id="kill">12. kill命令删除僵尸进程</h3>

也可以先用 `ps -aux |less` 查看一下
```
 ps|grep sed|awk '{print $1}'|xargs kill -9
```

<h3 id="bashArray">13. Bash数组 </h3>

```
1： 定义
bash1=(34,5,6,4)
bash2=("34","5","5","4")

2： 关联数组定义
declare -a bash1
bash1["34"]="53"
bash1["56"]="45"

3: 添加数组数据

bash1+="54"

4. 数组引用

4.1  ${bash1[1]}   第一个数组变量   ----${.[.]}
4.2  ${bash1[@]}
4.3  ${bash1[*]}
4.4  ${bash1[@]:1:2}

```

<h3 id="iconv"> 14. iconv解决中文识别的问题</h3>
```
$ for j in {-10..25}; do for i in `ls *.txt`;do iconv -f gb18030 -t utf-8 $i|awk '{print}' |awk -v B="$i" -v A="攻角：""$j" '$0~A{print A}'; done; done;
攻角：-10
攻角：-10
攻角：-10
攻角：-10
攻角：-9
攻角：-9
攻角：-9
攻角：-1
攻角：-1
攻角：-1
攻角：-1

改进：

$ for j in -10.00 -9.00 -8.00 -7.00 -6.00 -5.00 -4.00 -3.00 -2.00 -1.00 -0.00 1.00 2.00 3.00 4.00 5.00 6.00 7.00 8.00 9.00 10.00 11.00 12.00 13.00 14.00 15.00 16.00 17.00 18.00 19.00 20.00 21.00 22.00 23.00 24.00 25.00; do for i in `ls *.txt`;do iconv -f gb18030 -t utf-8 $i|awk '{print}' |awk -v B="$i" -v A="攻角：$"j" '$0~A{print B,A}'; done; done;
1.txt-10.txt 攻角：-10.00
2.txt-10.txt 攻角：-10.00
3.txt-10.txt 攻角：-10.00
as.txt-10.txt 攻角：-10.00
as.txt-10.txt 攻角：-9.00
实验数据20150425182803.txt 攻角：-9.00
实验数据20150425182804.txt 攻角：-9.00
```



终点(重点)推荐 <div align="center"><font color="red">[ChinaUnix的参考链接][17]</font></div>

[1]:http://cn.bing.com/ 
[2]:https://www.baidu.com/ 
[3]:http://www.jianshu.com/p/bfbcb65170c8 
[4]:http://jueqingsizhe66.github.io/blog/2016/07/25/awk-summary/ 
[5]:http://jueqingsizhe66.github.io/blog/2015/08/09/allfilesinfoldstoonefile/ 
[6]:http://jueqingsizhe66.github.io/blog/2015/05/27/bashzhi-neng-fen-lei-wen-jian-bing-qiu-zhi/[7]:http://jueqingsizhe66.github.io/blog/2015/05/27/bashzhi-neng-fen-lei-wen-jian-bing-qiu-zhi/ 
[7]:http://tldp.org/LDP/Bash-Beginners-Guide/html/index.html 
[8]:http://think-like-a-git.net/ 
[9]:http://think-like-a-git.net/sections/graph-theory/nodes-and-edges.html 
[10]:http://think-like-a-git.net/assets/images2/social_network.jpg 
[11]:/images/linux/eshell.png
[12]:/images/linux/linux_shell.gif
[13]:http://www.linuxjournal.com/content/bash-extended-globbing 
[14]: http://blog.csdn.net/hulihong/article/details/8553549 
[15]:http://www.linuxidc.com/Linux/2013-03/81801.htm 
[16]:http://www.cnblogs.com/dong008259/archive/2011/12/07/2279897.html 
[17]:http://bbs.chinaunix.net/tree/index_136_1/ 
[18]: http://blog.csdn.net/itsenlin/article/details/21129405 
[19]:http://leowzy.iteye.com/blog/1453421 
[20]:http://user.frdm.info/ckhung/b/re/index.php 
[21]:http://www.cnblogs.com/51linux/archive/2012/05/23/2515299.html 
