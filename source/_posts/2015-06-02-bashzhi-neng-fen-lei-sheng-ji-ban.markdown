---
layout: post
title: "bash智能分类升级版"
date: 2015-06-02 14:35:57 +0800
comments: true
categories: Linux
---

1. 加入了 seq 1 8  智能产生序列的方法
    使得30度攻角的实验值也可以处理。
2. 判断文件夹内是否存在文件
    [ -e "实验*.txt" ] 无法执行正则表达式
    因为有可能没有对应攻角的
<!--more-->

``` sh
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
        #echo $i
        # 之所以加入 iconv是因为 字符编码问题  怕找不到 密度 和来流动压行
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
#for j in -10.00 -9.00 -8.00 -7.00 -6.00 -5.00 -4.00 -3.00 -2.00 -1.00 0.00 1.00 2.00 3.00 4.00 5.00 6.00 7.00 8.00 9.00 10.00 11.00 12.00 13.00 14.00 15.00 16.00 17.00 18.00 19.00 20.00 21.00 22.00 23.00 24.00 25.00 26.00 27.00 28.00 29.00 30.00
for j in `seq -10.00 30.00`
    do 
        if ! [ -d "a"$j ]
        then    
            mkdir "a"$j
        fi
    done
}

# 导入对应攻角文件到文件夹内

#---  FUNCTION  ----------------------------------------------------------------
#          NAME:  getFileIntoDir
#   DESCRIPTION:  使用Awk按照不同的攻角值 利用 print的功能调用shell，把对应文件拷贝到对应攻角文件夹下
#                注意-v在awk表示定义变量的间隔符
#                 已废弃
#    PARAMETERS:  
#       RETURNS:  
#-------------------------------------------------------------------------------

getFileIntoDir(){
#cd modified
#for j in -10.00 -9.00 -8.00 -7.00 -6.00 -5.00 -4.00 -3.00 -2.00 -1.00 0.00 1.00 2.00 3.00 4.00 5.00 6.00 7.00 8.00 9.00 10.00 11.00 12.00 13.00 14.00 15.00 16.00 17.00 18.00 19.00 20.00 21.00 22.00 23.00 24.00 25.00 26.00 27.00 28.00 29.00 30.00
for j in `seq -10.00 30.00`
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
#for j in -10.00 -9.00 -8.00 -7.00 -6.00 -5.00 -4.00 -3.00 -2.00 -1.00 0.00 1.00 2.00 3.00 4.00 5.00 6.00 7.00 8.00 9.00 10.00 11.00 12.00 13.00 14.00 15.00 16.00 17.00 18.00 19.00 20.00 21.00 22.00 23.00 24.00 25.00 26.00 27.00 28.00 29.00 30.00
for j in `seq -10.00 30.00`
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
        #if [ -e $i/"实验*.txt" ] #由于不存在任何*.txt相关的东西 所以报错！ 但是不影响程序的进行 所以忽略
        #numBlank=`ls $i/*.txt|wc -l`  # 如果加上.txt反而不断报错
        numBlank=`ls $i/|wc -l`  # 这样就可以忽略掉 a26.00 --a30.00为空的情况
        echo $i"--Blank--"$numBlank
        if [ $numBlank -gt 0 ]
        then 
            sed -i '/^\s*$/d' $i/*.txt
        fi
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
        #if [ -e $i/"实验*.txt" ]
        #numCal=`ls $i/*.txt|wc -l`
        numCal=`ls $i/|wc -l`
        echo $i"--Cal--"$numCal
        if [ $numCal -gt 0 ]
        then 
            #echo "$i ok"
            awk -F"[ ：]" 'BEGIN{num=0;}{if($0 ~/攻角/){f[FNR]=$1;g[FNR]=$4;num+=1;}else if($0 ~/升力系数/){f[fNR]=$1;g[FNR]+=$4;a[FNR]=$5;b[FNR]+=$9;}else if($0 ~/压力系数/){f[FNR]=$1;g[FNR]=$2}else{f[FNR]+=$1;g[FNR]+=$2;if(nbr<FNR) {nbr=FNR;}}}END{for(i=0;i<=nbr;i++){print f[i]/num,g[i]/num,a[i]/num,b[i]/num;} print num;};' $i/*.txt>$i/modified.txt

        fi
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
 for i in `ls -d */` ## 用法思想和cp的处理一样
    do 
        #if [ -e $i/"实验数据*.txt" ] #  不支持正则写法
        #if [ -e $i/"实验数据*.txt" ]
        #numPlot=`ls $i/"*.txt"|wc -l`  # 错误写法 不要用双引号
        #numPlot=`ls $i/*.txt|wc -l` 
        numPlot=`ls $i/|wc -l` 
        #echo $i"--cl-cd--"$numPlot
        if [ $numPlot -gt 0 ]
        then
            num=`ls $i|wc -l`
            num=$(($num-1))
            #print $i,$num
            sed -n '2p' $i/"modified.txt"|awk -v A=$i -v B=$num '{print $2*B}' >> shengli.txt
            sed -n '3p' $i/"modified.txt"|awk '{print $2}' >> shengli.txt
            sed -n '2p' $i/"modified.txt"|awk -v A=$i -v B=$num '{print $2*B}' >> zuli.txt
            sed -n '3p' $i/"modified.txt"|awk '{print $4}' >> zuli.txt
        fi
    done
# 这是一个相当棒的技巧 把两行合并为一行
sed -i 'N;s/\n/ /' shengli.txt
sort -n shengli.txt > shenglimodified.txt
sed -i 'N;s/\n/ /' zuli.txt
sort -n zuli.txt > zulimodified.txt

# 两张图 第一张是cl  第二章图是cd
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
    # 之所以添加上 -d */ 的作用是 不要处理上一步产生的shengli.png 等文件，而只处理文件夹即可!
 for i in `ls -d */` # 注意一定是 */ 单独使用 ls -d没有效果
    do 
        #if [ -d $i ] && [ -e $i/"实验数据*txt" ]  ;; 不支持正则写法
        # 新改进的地方 想则会采用 引进ls工具 并调用wc
        #
#IF [···] -a [···] 这样不行。 得改成 if [ ... -a ...] 或者 If [...] && [..]
#这样吧，IF [···] && [···] 
        numPlotCp=`ls $i/|wc -l` 
        #numPlotCp=`ls $i/*.txt|wc -l` 
        #echo $i"--Cp--"$numPlotCp
        if [ $numPlotCp -gt 0 ]
        then 
            sed -n '6,47p' $i/"modified.txt" >> $i/"cpUp.txt";sed -n '49,93p' $i/"modified.txt" |sort -k 2 -nr >> $i/"cpUp.txt";cpGrid
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
cd $1
pwd
deleteDensity
fenLei
getFileIntoDirNew
blankEmptyLine
cal
plot
plotCp



```
