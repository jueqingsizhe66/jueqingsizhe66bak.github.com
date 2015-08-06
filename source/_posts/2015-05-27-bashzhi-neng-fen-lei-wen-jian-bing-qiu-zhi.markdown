---
layout: post
title: "Bash智能分类文件并求值"
date: 2015-05-27 10:36:24 +0800
comments: true
categories: Linux
---

Bash是linux底下的一种shell实现，可以方便linux用户进行系统管理。他的核心就是
调用最有效的命令组合形成更加强大完美的管理和应用程序,这也是unix的哲学,通过调用between programs的**关系**来**get its power**而不是通过自己的程序。
shell编成的目的减少unix相关系统的管理员的人物


下面介绍主要是bash语法，以及有bash语法调用awk，sed，grep等linux底下强大的工具实现一个文件智能分类，并对相应文件夹取平均。
<!--more-->


一些基本语法,虽然扎成一团，却静心选择。

``` sh
 在单行模式下可以
echo {userA,userB,userC}-{home,bin,data}
echo {"userA","userB","userC"}-{"home","bin","data"}
echo `{userA,userB,userC}-{home,bin,data}` #error
echo '{userA,userB,userC}-{home,bin,data}'
 分号的作用 挡任连续指令的功能！（其实也可以用换行代替）


 一定要注意的bash的if语法，有空格：
 if [ "$?" != 0 ] 等价于  if test "$?" != 0    一定要注意[]之间有空格

 等价关系有条件：
 `command` 等价于 $(command)
 "" 约等于 ''  但是单引号不转义特殊字符  双引号不转义特殊字符

 另外需要注意这种情况，${var} 和$var的区别：
  基本上一样，$1...$9这个没区别，但是$10呢，${var}的这种就是${10}
而$var却是$10是$1后边带个0


 几个dollar开头的内置参数
 $$ shell脚本号
 $* 以一个单字符显示所有脚本传递的参数 $1 
 $#  传给shell程序的位置参数个数
 $0  脚本名称
 $n  脚本的第几个参数
 $((...))  ==> x=$(($x+1)) 对括号内的表达式求值

 逻辑操作符相关:
 and   statement && statement && .. 
 or statement || statement || .. 

 最常用的分支判断：
 if [ -f file_one ]  && [ -f file_two ]
 then
 echo "in if"
 else
 echo "in else
 fi

 bash也在改进,比如：
 [[]] 是提高版的test  比如\>必须在[]和test中使用  在[[]]直接使用！
   #  所以以后改用[[]]

 下面几个${}开头的都是针对字符串的操作，可以剪切替换
${name:-default} 使用一个默认值（一般是空值）来代替那些空的或者没有赋值的变量name；
${name:=default}使用指定值来代替空的或者没有赋值的变量name；
${name:?message}如果变量为空或者未赋值，那么就会显示出错误信息并中止脚本的执行同时返回退出码1。
${#name} 给出name的长度
${name%word} 从name的尾部开始删除与word匹配的最小部分，然后返回剩余部分
${name%%word} 从name的尾部开始删除与word匹配的最长部分，然后返回剩余部分
${name#word} 从name的头部开始删除与word匹配的最小部分，然后返回剩余部分
${name##word} 从name的头部开始删除与word匹配的最长部分，然后返回剩余部分

（注，name为变量名，word为要匹配的字符串）
case${tao} in
a)      echo “a” ;;
b)      echo “b” ;;
esac

## 一些当行运行的指令，只在prompt运行窗口才有效
1 let
let "sus=2**3"
echo "sus = $sus" 
sus=[[2**3]]
echo "2**3"
echo "sus = $sus",$sus
if [ 2**3 == 8] 
then  echo "hello"
else
    echo "fuck"
fi
 注意在shell #的命令行运行可以写上分号 



##  虽然有人说函数创建可以使用  
 [function] function_name[()]{
 commands
 [return ...]
}
 但是我发现加入function反而不行，于是使用 function_name(){}的风格

```

# 无用的分号命令

： 该命令什么都不做，但执行后会返回一个正确的退出代码，即exit 0。比如在if语句中，then后面不想做任何操作，但是又不能空着，这时就可以使用“:”来解决，如下
``` sh
if [ "$i" -ne 1 ];then
    :
else
    echo "$i is not equal 1"
fi
```


# 特殊的点号
点号+ 文件名的作用 和 source+文件名的作用一样，都是在当前的shell执行命令
而 ./filename.sh 则是在子shell执行命令。
所以如果想要设置环境变量得用source+文件名或者. 文件名的方式。

# 关于大括号和小括号的区别
大括号和小括号的作用都一样，都是把几个命令组合在一起，然而他们也有所区别。
()是在产生的子shell下执行，而{}是在当前的shell下执行
与前面讲到是使用".  filename.sh"和"./filename.sh"的区别一样
``` sh

# A=123
# (A=abc;echo $A);echo $A
abc
123
# { A=abc;echo $A; };echo $A
abc
abc
```

由此可见，() 子shell设置完，不会影响到父shell，因为父不会继承子，影响的只是
自身的shell。
*注意：()里面两边可以不使用空格，{}里面两边必须使用空格，且最后一个命令也需要以“；”结尾，表示命令结束。*

# 中括号用于判断

[] 与test命令一样，用于比较值以及检查文件类型。如下

1. [ "$A" = 123 ]：是字符串的测试，以测试 $A 是否为 1、2、3 这三个连续的"文字"。
2. [ "$A" -eq 123 ]：是整数的测试，以测试 $A 是否等于"一百二十三"。
3. [ -e "$A" ]：是关于文件的测试，以测试 123 这份"文件"是否存在
4. [ -f "$A" ] : 判断是否存在文件
5. [ -d "$A" ] : 判断是否存在文件夹

# 关于 单括号和双中括号的区别
[[]]可以说是[]的“增强版”，它能够将多个test命令支持的测试组合起来.
``` sh

# [[ (-d "$HOME") && (-w "$HOME") ]] && echo echo "home is a writable directory"  
home is a writable directory
```

## 两者的主要区别如下：

+ 数字测试： -eq -ne -lt -le -gt -ge，[[ ]]同 [ ]一致
    - test int1 -eq int2  测试整数是否相等 
    - test int1 -ge int2  测试int1是否\>=int2 
    - test int1 -gt int2  测试int1是否\>int2 
    - test int1 -le  int2 测试int1是否<=int2 
    - test int1 -lt int2  测试int1是否\<int2 
    - test int1 -ne int2  测试整数是否不相等 
+ 文件测试： -r、-l、-w、-x、-f、-d、-s、-nt、-ot，[[ ]]同 [ ]一致
    + test -d file 指定文件是否目录 
    + test -f file 指定文件是否常规文件 
    + test -x file 指定文件是否可执行 
    + test -r file 指定文件是否可读 
    + test -w file 指定文件是否可写 
    + test -a file 指定文件是否存在 
    + test -s file 文件的大小是否非0 
+ 字符串测试： > < =(同==) != -n -z，不可使用“<=”和“>=”，[[ ]]同 [ ]一致，但在[]中，>和<必须使用\进行转义，即\>和\<
    + test str1=str2  测试字符串是否相等 
    + test str1!=str2  测试字符串是否不相等 
    + test str1 测试字符串是否不为空 
    + test -n str1 测试字符串是否不为空 
    + test -z str1 测试字符串是否为空
+ 逻辑测试： []为 -a -o ! [[ ]] 为&& || !
+ 数学运算： [] 不可以使用 [[ ]]可以使用+ - * / %
+ 组合： 均可用各自逻辑符号连接的数字（运算）测试、文件测试、字符测试
``` sh

# [ a \> 1 ] && echo ture || echo false
ture
# [[ a > 1 ]] && echo ture || echo false
ture
```

# 双中括号和双小括号
(())专门来做数值运算，如果表达式求值为 0，则设置退出状态为 1；如果求值为非 0 值，则设置为 0。不需要对 (( 和 )) 之间的操作符转义。算术只对整数进行。除 0 会产生错误，但不会产生溢出。可以执行 C 语言中常见的算术、逻辑和位操作。

``` sh
((i=1+99));echo $i
100

i=99;((i++));echo $i
100

echo $((2**3))
8

```
*注意：使用 (( )) 时，不需要空格分隔各值和运算符，使用[]和[[ ]] 时需要用空格分隔各值和运算符*

# find跳过指定目录
如果在查找文件时希望忽略某个目录，因为你知道那个目录中没有你所要查找的文件，那么可以使用-prune选项来指出需要忽略的目录。
在使用 -prune选项时要当心，因为如果你同时使用了-depth选项，那么-prune选项就会被find命令忽略。
# [Unix Tutorial for Beginner](www.ee.surrey.ac.uk/Teaching/Unix)
# [常用的bash命令学习](http://vbird.dic.ksu.edu.tw/linux_basic/0320bash.php )
1. wc -l
2. sort -rn -t 2 -k :
3. grep -n "bash" src
4. join a.txt b.txt 两个文件有一列是相同的
5. find /usr -name "hello" -type d -exec ls -l {} \;  使用-ok更为安全。
6. find . -type f -atime -7 -print   7天前被访问的
7. find . -type f -size -100M   +100M 大于100M  -100M小于10M  
    + b-----块（512字节）
    + c-----字节
    + w-----字（2字节）
    + k------千字节（1024个字节）
    + M-----兆字节
    + G------吉字节（1024M）
8. tr 'A-Z' 'a-z'  
9. echo 'this is my passwd' |tr 'abcdefghijklmnopqrstuvwxyz''zyxwvutsrqponmlkjihgfedcba'   tr也可以用于加密
   解密：
echo 'gsrh rh nb kzhhdw' |tr 'zyxwvutsrqponmlkjihgfedcba''abcdefghijklmnopqrstuvwxyz'  
  
tr -d SET1 #用tr删除字符   
echo 'Hello1234 World11111 44' |tr -d '0-9'  
HelloWorld  
10. grep -oP '(?<=\+)fx' *  
    grep -oP '(?<=\+)fx(?=\()' * 
    grep -onP '(?<=\+)fx(?=\()' * 
11. cut -d ":" -f 1,3 /etc/passwd   类似于awk的字域
    cut -d ":" -f 1,3,7 /etc/passwd

# 一个综合性的案例

``` sh

#---  FUNCTION  ----------------------------------------------------------------
#          NAME:  deleteDensity
#   DESCRIPTION:  去除密度和来流经验等行  采用  awk和sed组合删除！ 并把有用信息导入modified文件夹下
#                 iconv主要是考虑到windows下的中文字符无法被识别的问题
#                  这个步骤是复制，而第三个fileintoDirNew是通过mv来操作的
#    PARAMETERS:  
#       RETURNS:  
#-------------------------------------------------------------------------------
deleteDensity()
{
echo  "删除密度和来流"
if [ -d "modified" ]
then echo "modified文件夹已存在"
else 
mkdir modified
fi
for i in `ls *.txt`
    do 
        iconv -f gb18030 -t utf-8 $i|awk '{print}'|awk '{if ($0 ~ /攻角/) {print $3;}else if($0 ~ /升力/){print $1,$4,$5}else if($0 ~ /^$/){print ''}else {print $3,$5;}}'   |sed '/密度/d'|sed '/来流动压/d'|sed '/^$/d' >modified/$i 
    done
}

# 创建文件夹 从-10攻角到25度攻角

#---  FUNCTION  ----------------------------------------------------------------
#          NAME:  fenLei
#   DESCRIPTION:  对攻角实行分类，按照不同的攻角创建不同的文件夹
#    PARAMETERS:  
#       RETURNS:  
#-------------------------------------------------------------------------------
fenLei(){
cd modified  # 注意下面的所有函数 都运行在modified文件夹下
for j in -10.00 -9.00 -8.00 -7.00 -6.00 -5.00 -4.00 -3.00 -2.00 -1.00 0.00 1.00 2.00 3.00 4.00 5.00 6.00 7.00 8.00 9.00 10.00 11.00 12.00 13.00 14.00 15.00 16.00 17.00 18.00 19.00 20.00 21.00 22.00 23.00 24.00 25.00
    do 
        if ! [ -d "a"$j ]
        then    mkdir "a"$j
        fi
    done
}

# 导入对应攻角文件到文件夹内

#---  FUNCTION  ----------------------------------------------------------------
#          NAME:  getFileIntoDir
#   DESCRIPTION:  使用Awk按照不同的攻角值 利用 print的功能调用shell，把对应文件拷贝到对应攻角文件夹下
#                注意-v在awk表示定义变量的间隔符
#    PARAMETERS:  
#       RETURNS:  
#-------------------------------------------------------------------------------

getFileIntoDir(){
#cd modified
for j in -10.00 -9.00 -8.00 -7.00 -6.00 -5.00 -4.00 -3.00 -2.00 -1.00 0.00 1.00 2.00 3.00 4.00 5.00 6.00 7.00 8.00 9.00 10.00 11.00 12.00 13.00 14.00 15.00 16.00 17.00 18.00 19.00 20.00 21.00 22.00 23.00 24.00 25.00
do 
    for i in `ls *.txt`
        do 
            awk '{print}' $i |awk -v B="$i" -v A="攻角：""$j" -v C="a""$j" '$0~A{print "cp ",B," "C"/"B|"/bin/bash"}'
        done
done
}

#---  FUNCTION  ----------------------------------------------------------------
#          NAME:  getFileIntoDirNew
#   DESCRIPTION:  新版本的getFileIntoDir 方式
#    PARAMETERS:  
#       RETURNS:  
#-------------------------------------------------------------------------------
#grep "攻角：25.00" *.txt|awk -F[:] '{print $1}'

getFileIntoDirNew(){
for j in -10.00 -9.00 -8.00 -7.00 -6.00 -5.00 -4.00 -3.00 -2.00 -1.00 0.00 1.00 2.00 3.00 4.00 5.00 6.00 7.00 8.00 9.00 10.00 11.00 12.00 13.00 14.00 15.00 16.00 17.00 18.00 19.00 20.00 21.00 22.00 23.00 24.00 25.00
do 
        grep "攻角：$j" *.txt|awk -F[:] '{print $1}'|xargs -i mv {} "a""$j"
done

}
## 清除空行

#---  FUNCTION  ----------------------------------------------------------------
#          NAME:  blankEmptyLine
#   DESCRIPTION:  利用sed -i命令直接处理文件中多余的空行
#    PARAMETERS:  
#       RETURNS:  
#-------------------------------------------------------------------------------
blankEmptyLine(){
# 在每个文件夹运行这些命令 清除空行 
#cd modified
for i in `ls -d */`
    do 
        sed -i '/^\s*$/d' $i/*.txt
    done
}

#---  FUNCTION  ----------------------------------------------------------------
#          NAME:  cal
#   DESCRIPTION:  把每个攻角下的文件，利用awk执行攻角平均  升力系数平均  和压力系数平均
#    PARAMETERS:  
#       RETURNS:  
#-------------------------------------------------------------------------------
cal(){
# 在每个文件夹下都运行此计算平均值 程序
#cd modified

for i in `ls -d */`
    do 
        awk -F"[ ：]" 'BEGIN{num=0;}{if($0 ~/攻角/){f[FNR]=$1;g[FNR]=$4;num+=1;}else if($0 ~/升力系数/){f[fNR]=$1;g[FNR]+=$4;a[FNR]=$5;b[FNR]+=$9;}else if($0 ~/压力系数/){f[FNR]=$1;g[FNR]=$2}else{f[FNR]+=$1;g[FNR]+=$2;if(nbr<FNR) {nbr=FNR;}}}END{for(i=0;i<=nbr;i++){print f[i]/num,g[i]/num,a[i]/num,b[i]/num;} print num;};' $i/*.txt>$i/modified.txt
    done
}


#---  FUNCTION  ----------------------------------------------------------------
#          NAME:  plot
#   DESCRIPTION:  函数用于画图 调用gnuplot
#                 使用下面函数之前得安装gnuplot
#    PARAMETERS:  
#       RETURNS:  
#-------------------------------------------------------------------------------
plot(){

 #for i in `ls`;do num=`ls $i|wc -l`;let num=$[$num-1];sed -n '2p' $i/"modified.txt"|awk -v A=$i -v B=$num-2 '{print A,$2*B}' >> a.txt;sed -n '3p' $i/"modified.txt"|awk '{print $2}' >> a.txt;done; 

 #   sed 'N;s/\n/ :/' a.txt


# for i in `ls`;do num=`ls $i|wc -l`;let num=$[$num-1];sed -n '2p' $i/"modified.txt"|awk -v A=$i -v B=$num-2 '{print $2*B}' >> a.txt;sed -n '3p' $i/"modified.txt"|awk '{print $2}' >> a.txt;done; 
 for i in `ls`
    do 
        num=`ls $i|wc -l`
        num=$(($num-1))
        #print $i,$num
        sed -n '2p' $i/"modified.txt"|awk -v A=$i -v B=$num '{print $2*B}' >> shengli.txt
        sed -n '3p' $i/"modified.txt"|awk '{print $2}' >> shengli.txt
        sed -n '2p' $i/"modified.txt"|awk -v A=$i -v B=$num '{print $2*B}' >> zuli.txt
        sed -n '3p' $i/"modified.txt"|awk '{print $4}' >> zuli.txt
    done

sed -i 'N;s/\n/ /' shengli.txt
sort -n shengli.txt > shenglimodified.txt
sed -i 'N;s/\n/ /' zuli.txt
sort -n zuli.txt > zulimodified.txt

gnuplot<<EOF 
set ter 'png' 
set out 'shengli.png' 
plot "shenglimodified.txt" t 'cl' w lp
EOF
gnuplot<<EOF 
set ter 'png' 
set out 'zuli.png' 
plot "zulimodified.txt" t 'cd' w lp
EOF
rm -rf zuli.txt
rm -rf shengli.txt
}


#---  FUNCTION  ----------------------------------------------------------------
#          NAME:  plotCp
#   DESCRIPTION:  用于打印cp曲线
#    PARAMETERS:  
#       RETURNS:  
#-------------------------------------------------------------------------------
plotCp(){
 for i in `ls`
    do 
        if [ -d $i ]
            then sed -n '6,47p' $i/"modified.txt" >> $i/"cpUp.txt";sed -n '49,93p' $i/"modified.txt" |sort -k 2 -nr >> $i/"cpUp.txt";cpGrid
            fi
    done

}

#---  FUNCTION  ----------------------------------------------------------------
#          NAME:  cpGrid
#   DESCRIPTION:  注意  gnuplot必须放在句首
#    PARAMETERS:  
#       RETURNS:  
#-------------------------------------------------------------------------------
cpGrid(){
gnuplot<<EOF 
set ter 'png' 
set out '$i/cpUp.png'
set xtics rotate by -45
set ytics rotate by -45
plot "$i/cpUp.txt" t 'cp' w lp
EOF
}
# 执行函数
deleteDensity
fenLei
getFileIntoDirNew
blankEmptyLine
cal
plot
plotCp


```


[shell脚本集合1](http://dngood.blog.51cto.com/446195/703347)
[shell脚本集合2](http://dngood.blog.51cto.com/446195/764563)
[shell脚本集合3](http://dngood.blog.51cto.com/446195/1107458)
