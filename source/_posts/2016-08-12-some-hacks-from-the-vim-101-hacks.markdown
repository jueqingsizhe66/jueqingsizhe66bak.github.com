---
layout: post
title: "Some hacks from the Vim 101 Hacks"
date: 2016-08-12 11:31:45 +0800
comments: true
categories: vim
---

Every vimer should have read the introduction document ["learn-vim-progressively"][2].
It is good and simple, very important. And what's more ,there are still some funny tricks
you can learn from [vim.101.hacks][1].

Below are some records from reading [Vim.101.Hacks][1].
<!--more-->


<h2 id="write">visual write</h2>

```
in the visual mode,
you can select some contents,
then :w newfilename

finally you get the new files with select contains.
```

<h2 id="tab multiple windows">tab multiple windows</h2>

```
when you open multiple window with 'vim -f file1 file2 file3'

you can use CTRL-^ to change the different tabs. Also the :tabNext

```

Also you can use `ex`

1. :Sex : open directory folders with  horizontal window
2. :Vex : open directory folders with vertical window
3. :Tex : open directory folders with new window tab

Then you can also modify the filename or Delete filename
such as,vim /etc

+ D  delete the file under the cursor
+ R  rename the file under the cursor
+ X  execute the file under the cursor
+ O  open a horizontal split window 

<h2 id="goodtricks"> good Tricks for programmers</h2>

In the [learn-vim-progressively][2],
we have learn three character which do useful for the programmers:

1. % go to the matching character of the pair. Jump to the matching parenthesis(),or curly braces{} or square []
2. \# go to the next(resp. previous) occurrence of the word under the cursor(what is the definition of the word?)
3. \* go to the previous(resp. next) occurrence of the word under the cursor(what is the definition of the word? Maybe three parts: digits,alphabets,underscore)

You can also learn some tricks for you:

1. `[(` : go to the previous unmatched `(`
2. `[)` : go to the previous unmatched `)`
3. `[{` : go to the previous unmatched `{`
4. `[}` : go to the previous unmatched `}`

<h2 id="record">when you jump</h2>

When you jump the contents in the files, vim will get your jumps  into the :jump register.

Similarly, your copy or yank message will be translated into :reg register(0-10)

<h2 id="mark">When you reading a long file</h2>

You can use <font color='red'>fold</font> and <font color='red'>mark</font>.

When you fold ,you have six methods:

1. set foldmmethod=indent(when see the indent code line,it will fold)
2. set foldmmethod=marker(need some sign such as '{{{ fold text }}}'),it is usually not used for the programmer
3. set foldmmethod=manual
4. set foldmmethod=syntax,maybe good for programmer
5. set foldmmethod=diff
6. set foldmmethod=expr

Advice: you can use the method syntax,indent,manual alternatively,[access link help][3].

Also you can type `:help folding`

Most important,you should install the [vim-signature][5],type `Bundle 'kshenoy/vim-signature'` inside the `bundles.vim`.

then you can watch the effect.A little work for usage

1. m[a-zA-Z] to make marks
2. '] to move alphabets sequence,similarly to the ]\`, the different is the '] put the cursor at the end of the line.
3. '[ to move alphabets inversely,similarly to the [\`

But sometimes you need to delete all the marks, try `dmx to delete x,...`



<h2 id="abbreviation">abbreviation</h2>

you can write the msg with the simple type: 

such as

```
:abbr US United States

```

then you type `:abbr US`,it will auto extend US to the United Stated.

:abbr United States 

if you don't want to auto extension, then you can

```
:noabbr United States
```

So, you can type the `:abbr`  statements in your vimrc, so it will auto extend your spelling,such as your email,your frequently website visited.

<font color="red">In the vim ,you can type many abbreviation, such as 'foldmethod is simplify as fdm'</font>

<h2 id="veryImportant">Important and hack tricks</h2>

such as you have a file in the current directory, then you put the cursor in the filename, then type `gf`
,funny thing vim will let you watch the file you want.

Also, it you have a link in your vimfiles,then move the cursor to the link position, type `gf`, the
same funny thing,vim will download the html file,and let you read it.


Three method to operate:

1. gf  open a file(in the same window) whose name is currently under the cursor
2. <Ctrl-W> f  open a file(in a new window) whose name is currently under the cursor
3. <Ctrl-W> gf open a file(in a new tab) whose name is currently under the cursor

It can be helpful,

+ to verify the filenames given inside configuration files are valid.
+ while editing one file,and want to move another file which filename is place in the current cursion position 


<h2 id="whatIThink">What I think</h2>

Every file you can think contain two parts: 

1. Physical sign
2. logical sign

physical sign

    0,$(dollar sign)
    ^(caret sign),g_
    gg,G
    :50g
    50% : go to the 50th percentage of file. jump to the middle of the file
    75% : 3/4 of the file
    :50 : go to the line 50, or you can use `50gg` and `50G`,also called NG.
    ...

logical sign

    w  : go to next word, the word should contains three type characters:characters,digits and underscore,if not,it will be looked as new word.
    W  : go to next word,while the word is only split by the blank space.
    b  : go to previous word,like 'w'
    B  : go to previous word,like 'W'
    e  : go to the end of the current word(E doesn't work sometimes)

    H  : go to the first line of the current Screen
    L  : go to the middle line of the current Screen
    M  : go to the down line of the current Screen
    f,t:  fy means go to the position after the next 'y' occurance position, while ty go to the position before
    F,T:  Fy means go to the position after the previous y occurance position,while Ty go to the position before

    100l : navigation key iis: 100 followed by l. Go to the 100 characters after the current position(100| ,100 with the vertical bar)
    100<space>:Another way to go to the 100th characters from the current position

    %
    #,*


Actually, you should read the file: `:h usr_03.txt` in the vim window.

Every your operation can be contained three part:

1. modes(under current modes(environments),you can do some positions and modifies)
2. position(go to with some move action),the process of reading(read the file)
3. modify(write in with some modify action),the process of writing(edit the file)

One useful command in the vim script:
```
autocmd FileType c,cpp setl fdm=syntax

```
autocmd means if the FileType is c or cpp ,then execute the command `setl fdm=syntax`,and 
you can use `]s` jump to the next spelling mistake,on contrary,`[`

Sometimes ,you need to do some spell check,because your English is too good. So type `:set spell`,but don't do it in the
chinese text.

Most important place to learn vim: type `:help` in the vim window.
As [a programmer][4], you should read the vim help files ,just type `:h usr_29.txt` in the vim window


[1]: http://ishare.iask.sina.com.cn/f/23515180.html
[2]: http://yannesposito.com/Scratch/en/blog/Learn-Vim-Progressively/
[3]: http://www.cnblogs.com/xuxm2007/archive/2011/11/10/2244418.html
[4]: http://easwy.com/blog/archives/advanced-vim-skills-advanced-more-method/
[5]: https://github.com/kshenoy/vim-signature
