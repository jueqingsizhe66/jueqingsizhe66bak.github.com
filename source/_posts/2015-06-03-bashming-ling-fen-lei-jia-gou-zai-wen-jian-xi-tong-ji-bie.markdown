---
layout: post
title: "bash命令分类--架构在文件系统级别"
date: 2015-06-03 20:18:18 +0800
comments: true
categories: 
---


I have many years experience in the shell command, so I categoried some commands into 
many categories,which let me understand the commands more clearly.
You can first practice [how to play with bash shell](http://blog.chinaunix.net/uid-25932176-id-2973818.html),then you can catch the topic afterward
[cfaj network](http://cfajohnson.com/)
<!--more-->

## bit

+ programming language : 
    - c ,c++ c#,java
    - lisp,scheme,haskell etc
    - bash,python,perl,ruby,etc
    - javascript


## word

+ programming language : 
    - c ,c++ c#,java
    - lisp,scheme,haskell etc
    - [bash](http://www.vaikan.com/bash-scripting),python,perl,ruby,etc
        + $() == () == `` means in the __subshell__ closure the commands; $((  )) == ((  )): mainly some algorithm operations
        + ${} == {} means in the __current shell__ closure the commands;  $\{\{  \}\} == \{\{  \}\}
            - ${#f}  calculate the variable f's length
            - ${f:6}  get the string from 6 to end of the string
            - ${f:6:4} get the string from 6 to (6+4) of the string. ${f:${pos}:${len}}
            - ${f: -8} get the reverse from end to end+8's string.
            - ${f/path?/x}  --single replace
            - ${f//path?/x} --globle replace
            - ${f#*/}  delete the characters until get across a  / from left to right(__From means start point. To means end point__)
                + ${var#*o}  remove the shortest string that matches PATTERN (the character 'o') from the begining of the expanded value
            - ${f##*/} delete the caaracters until all the / symbols have been got across from left to right.
                + ${0##*/} extract the name of a script from the $0 parameter,which contains the full path to the script
                + similarly, you can use "${1%/*}","${2%/*}"  ....
            - ${f%*/}  delete the characters until get across a  /  from right to left
            - ${f%%*/} delete the caaracters until all the / symbols have been got across from right to left. 
            - variable (__- is used to set the unset value; + is used to set the set value__)
                + ${var:-default} checks to see whether a variable is __unset or empty__,if yes,then expands to a default string to var(if omit the colon? see below)`unset var or var=`
                    - ${var:=default} 和${var:-default}很像，只不过他还多做了一个额外的功能，__assign the default value to the variable__.而在${var:-|+default}的情况下，并没有指派给变量值。
                + ${var-default} checks to see whether a variable is __unset__,if yes,then expands to a default string to var, `unset var`,almost the same with colon
                + ${var:+alternative} use alternative value if $var __is set and  not empty__ 
                + ${var+alternative} use alternative value if $var __is set__,it does matter whether $var is  empty or not
            - ${var^[a-m]}
            - ${var^^[a-m]} ## matches all characters from a to m inclusive! if match change to uppercase.NOTE:__Only in Bash >=4.0__
            - ${var^^}  uppercase the string,such as toronto change to TORONTO
            - ${var^} Captitalize the string, such as toronto change to Toronto
            - ${var,,}  such as var=TORONTO change to toronto
            - ${var,,[N-Q]} such as var=TORONTO changes to ToRonTo
            - ARRAY(use ${} to get the value of the array) Note: __not POSIX standard ,but fit for bash shell__
                + printf "%s\n" "${BASH_VERSINFO[0]}"    print the first element of the array BASH_VERSINFO bulit in the shell bash
                + printf "%s\n" "${BASH_VERSINFO[1]}"    print the second element of the array BASH_VERSINFO bulit in the shell bash
                + printf "%s\n" "${BASH_VERSINFO[*]}"    print the all element of the array BASH_VERSINFO bulit in the shell bash in one line as string 
                + printf "%s\n" "${BASH_VERSINFO[@]}"    print the all element of the array BASH_VERSINFO bulit in the shell bash in different line 
                + printf "%s\n" "${BASH_VERSINFO[@]:1:2}"    print the secondth and thirdth line element of the array BASH_VERSINFO bulit in the shell bash 
                + printf "%s\n" "${#BASH_VERSINFO[*]}"    print the length of the array BASH_VERSINFO bulit in the shell bash 
                + construct an array 1
                    - unset a
                    - a[${#a[@]}]="1 $RANDOM" ## #{#a[@]} is 0
                    - a[${#a[@]}]="2 $RANDOM" ## #{#a[@]} is 1
                    - a[${#a[@]}]="3 $RANDOM" ## #{#a[@]} is 2
                    - a[${#a[@]}]="4 $RANDOM" ## #{#a[@]} is 3  ---> finally we have a fourth element array a
                    - printf "%s\n" "${a[@]}"
                + construct an array 2
                    - province=(Beijing Fujian Liaoning)
                    - printf "%s\n" "${province[@]}"
                + construct an array (Associative Arrays must be declared before)
                    - declare -A array1  ## __if not declare -A array1 only keep the last one assignment operation__.
                    - for subsricpt in a b c d e
                    - do 
                    -     array[$subscript]="$subscript $RANDOM"
                    - done
                    - printf ":%s:\n" "${array["c"]}" ## print one value
                    - printf ":%s:\n" "${array[@]}" ## print one value

        + $[[  ]] == [[  ]] --> [[ =~ ]] (you can use variable match the regex expressions inside the  [[]])
        + some special remarks:
            - *(pattern [|pattern] ...)  the match occurence >=0
            - +(pattern [|pattern] ...)  the match occurence >=1
            - @(pattern [|pattern] ...)  the match occurence ==1
            - ?(pattern [|pattern] ...)  the match occurence ? I am a little confuse!
            - &   let the command execute backward
            - ``  redirect an command output to the argument of another command
            - ; command separator
            - | pipe symbol, put a command output redirect to another command's input
            - $# the numbers of the parameters while execute the shell scripts
            - $@ the parameters  while execute the shell scripts(know every parameters) .Note use "$@"
            - $* the parameters as a string while execute the shell scripts
            - $! the PID number
                + ${!var} 表示的是间接调用变量var,比如x=yes,var=x,如果直接使用echo $x得到yes,如果直接echo $var 得到的是x，如果使用echo ${!var}则是yes
                + __${!}给你的感觉会好象是函数式变量，不断的变化中,类似的比如set --具有更换positional parameters的作用__。
            - case中*[!0-9]*) 表示 非数字 ，在正则表达式中一般是使用[^0-9]
            - "" 字符串中加上双引号的作用是"$a" ，[ "$a" == "abc str"] 避免太多参数的报错！""起着连接的作用,变量的值包起来，防止中间有空格，一般字符串反而不需要.这边又扯到[另一个问题](http://blog.jobbole.com/46191)，比如[[ $foo == bar ]]不会展开foo的值,也就是把空格也当作字符串字面值的整体，而[ $foo == bar ] 则会展开foo的值,也就空格会分隔原来的foo字符串，这样可能会出现多参数错误。所以加上双引号有好处，暂时无坏(针对变量)
    - javascript
+ tr
    - tr -d 'abc'
    - tr '[a-z]' '[A-Z]'
+ seq

类似的可以参考[shell符号](http://blog.useasp.net/archive/2014/06/02/summary-of-the-special-characters-in-shell-on-linux.aspx),并且注意shell当中一个
比较大的书写问题是：等号两边不能有空格，括号和中括号两边最好有空格(比如最多的if判断）。
## line

+ head
+ tail
+ sed
+ grep(egrep)
    - grep -nR "neededWord" *
+ awk(actually it can be categoried into word manipulation)
+ sort
    - sort -r -t :  -k 2
+ cut -d":"  -f5
+ read
+ echo
+ uniq
+ join
+ paste  (much simpler than join , just separte two parts with Tab)
+ shift
    - shift 3    To remove the first three parameters(__To means going to do,it is a target__)
    - shift "$#"  To remove all the parameters,$# contains the number of all the positional parameters
    - shift $(( $# - 2 ))   Remove all but the last two positional parameters.Remember there are blank between (( and $  also between 2 and  )).
+ set --  Newparameters ## which is just replace the parameters with Newparameters
    -   ``` sh \_max3(){ 
        [ $# -ne 3 ] && return 5
        [ $1 -gt $2 ] && { set -- $2 $1 $3; } ## line a   if $1>$2    replace $2 with $1  ,$1 with $2, the bigger in the righter
        [ $2 -gt $3 ] && { set -- $1 $3 $2; } ## line b:  $1 is the last time's $1 ,but $3 is the last time's $3(in line a)
        [ $1 -gt $2 ] && { set -- $2 $1 $3; } ## line c
        _MAX3=$3
        _MID3=$2
        _MIN3=$1
        } ```


## content

+ cat

## file

+ ls
+ find -name "" src/
+ locate
+ touch
+ wc -l
+ rm -f
+ cp
+ mv
+ source == .  ==> sourch \*.sh  == . \*.sh     source .bashrc == . bashrc
+ \>  == 1>
+ \>\> == 1>> 
+ <  == <1
+ split 
    + split -b 300k ...
    + split -l 300   (line=300 as an file)
+ vim
+ gnuplot
+ graghviz

## fildfolder

+ cd
+ mkdir
+ rmdir
+ cp -r
+ mv
+ tar 
    - tar -cvf  src.tar.gz src/
    - tar -xvf  src.tar.gz


## system

+ alias (make the command become convinient)
+ unalias
+ ulimit -a (try to think if there are ten person enter the Linux at the same time, and open th 1G file at the same time, the main machine means 10G. So the machine wil become busy.)
+ set
+ unset
+ env
+ history
    + !!  the last command(you can use the up and down arrow)
    + !66 the 66th command  !line-number
    + !al run the command begin with 'al' nearest.!command
+ type
+ kill -9
+ crontab -e
+ which
+ who
+ dmesg
+ du -sh .
+ make  (for compiling the system)
+ fsck -c
+ System Transplant
    - wget -R [website]
    - curl -l
    - git clone
    - hg clone
    - svn clone
+ system enter
    - ssh
    - telnet
    - ftp
+ ifconfig
+ all files with map relationship can form an ecosystem.

## Network topology

+ node (one computer one system)
+ server (only supply with services)
+ NIC (network interface card)  RJ-45
    - network interface port   which abstracted by the NIC . 
+ route -n


******

bit --> word --> line --> content --> file --> system.  ++Actually, you can use the function to abstraction several command. 
What's more ,you can use module or filefolder to do another bigger abstraction++.

__you can find that from bit manipulation to file manipulation,there are many commands used in the Linux system__

