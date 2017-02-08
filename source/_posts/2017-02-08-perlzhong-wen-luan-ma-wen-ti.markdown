---
layout: post
title: "perl中文乱码问题"
date: 2017-02-08 17:23:58 +0800
comments: true
categories: perl
---


binmode可以很好的解决perl中问题，采用[Encode1][1],[Encode][2]等方式不管用

<!--more-->

PerlFile.pl测试源码：

``` perl
#!/usr/bin/env perl 
#===============================================================================
#
#         FILE: perlFile.pl
#
#        USAGE: ./perlFile.pl  
#
#  DESCRIPTION: 
#
#      OPTIONS: ---
# REQUIREMENTS: ---
#         BUGS: ---
#        NOTES: ---
#       AUTHOR: YOUR NAME (), 
# ORGANIZATION: 
#      VERSION: 1.0
#      CREATED: 2017/2/8 9:49:51
#     REVISION: ---
#===============================================================================

use strict;
use warnings;
use utf8;



binmode(STDIN,":encoding(gb2312)");
binmode(STDOUT,":encoding(gb2312)");

while ( <> ) {
    chomp;
    print $_,"\n";
    last if $_ =~ m/q/xm;
}

open(FEIJI,"E:\\feiji.txt") or die "can't open the file \n";
my $car ="尾翼";

	• binmode(FEIJI,":encoding(gb2312)");
# binmode( STDIN,  ':encoding(gbk2312)' );
 #binmode( FEIJI,  ':encoding(utf8)' );
 #binmode( STDOUT, ':encoding(gbk2312)' );   
 #binmode( STDERR, ':encoding(gbk2312)' ); 
#while( my $line = <FEIJI>){
#my $re = Encode::decode('GB2312','汽车');
 my $count=1;
 while( <FEIJI>){
     #print "$_ \n" ;
    #     print "$_ \n" if $_ =~ /.*$re.*/;
#    Encode::_utf8_on($_);
    #    Encode::decode_utf8($_);
    #print "$_\n" ;#if $_ =~ m/$car/xm;
    #print "$count: $_\n" if $_ =~ m/$car/xm;
    #$count +=1;

     s/汽车/风力机/g;
    print;
    }

```

[1]:http://blog.chinaunix.net/uid-20735106-id-3438892.html 
[2]:http://www.jb51.net/article/16041.htm 
