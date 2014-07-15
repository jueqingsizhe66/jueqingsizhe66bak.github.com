---
layout: post
title: "模块分割--工厂机制"
date: 2014-05-15 22:27:07 +0800
comments: true
categories:  makefile
keyword: makefile  cmd   split  map  reduce
---

<!--more-->

1. map
2. reduce
3. deploy(by makefile)

#map---do   or map--action#
``` makefile
FC=gfortran
FFLAGS=-O3
split : ./f90split.f90 ./output/cmd.sh
    $(FC) $(FFLAGS) $^ -o $@
    mv $@ ./output/
    /bin/sh ./output/cmd.sh

.PHONY: clean
```
----------
不是不适合，是超级适合，只不过你不会而已
----------


#makefile:(负责让对应的文件夹去做对应的事情)#
``` makefile
FC=gfortran
FFLAGS=-O3
split : ./f90split.f90 ./output/cmd.sh
    $(FC) $(FFLAGS) $< -o $@                # 造出一个工人出来！
    cp $@ ./output/          # 把一个高级技术工人调到现场 去干活去！  最好用cp 这样就可以派到其他工厂去了
    cd ./output/ &&sh cmd.sh#切换到对应目录下 去做他该做的事情就可以了（而不是原先你想的

    # sh .output/cmd.sh 这样是在当前的目录下产生所有的东西 而不是对应的目录）
.PHONY: clean

```

#在我的output目录下新建了一个#
##cmd.sh:(他只做本目录下的内容)##

``` 
#!/bin/sh
# sh cmd.sh

for i in `ls -l /home/incompact3dNew/channel|grep f90|awk '{print $NF}'`;
do
    file=/home/incompact3dNew/channel/$i;
#    echo $file;
     ./split $file
done;
```

