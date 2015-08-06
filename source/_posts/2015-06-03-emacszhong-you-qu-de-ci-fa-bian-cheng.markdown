---
layout: post
title: "emacs中有趣的词法编程"
date: 2015-06-03 14:35:50 +0800
comments: true
categories: Linux
---

词法编程最开始是有1984，Donald Knuth提出的[词法编程](http://comjnl.oxfordjournals.org/content/27/2/97.full.pdf ).
另外可以参考[emacs org-mode 词法编程](http://www.howardism.org/Technical/LP/introduction.html )和 [emacs orgmode的高级词法编程](http://www.howardism.org/Technical/Emacs/literate-devops.html )
我的理解是词法编程，就是在文本和代码中交叉实现，不让代码只是代码，文本只是文本，其实这根函数式编成的思想是相似的，__代码即数据，数据即代码__。
<!--more-->
``` sh
#+begin_src sh
ls
hostname -I
#+end_src

#+results:
| activate.org                   |
| activate.png                   |
| activity.png                   |
| broken.png                     |
| communication.png              |
| data                           |
| digraph2.png                   |
| ditaa.org                      |
| ditta.png                      |
| emacs-chats                    |
| emacsconf2015                  |
| emacs-notes                    |
| gnuplot.org                    |
| gnuplot.png                    |
| #GTD.org#                      |
| gv01.png                       |
| javaTalentTest.org             |
| javaweb5.org                   |
| javaWriteRules.org             |
| latex.org                      |
| Learn.html                     |
| lispfile.org                   |
| literate-programming.org       |
| meeting_states.png             |
| multi.png                      |
| named.png                      |
| newLearningTakecar.el          |
| newLearningTakecar.org         |
| newLearningTakecar.org_archive |
| normal_task_states.png         |
| object.org                     |
| object.png                     |
| orgmode-babel-ditaa3.png       |
| org-mode-doc                   |
| org-mode.org                   |
| orgTest.org                    |
| outcome.sh                     |
| phone_states.png               |
| plantuml_example_states.png    |
| plantuml.org                   |
| scales.png                     |
| schemeToTime.org               |
| sequence.png                   |
| some_filename.png              |
| somefile.png                   |
| state.org                      |
| style                          |
| style.tar.gz                   |
| table.png                      |
| task.org                       |
| teaching.org                   |
| test-act2.png                  |
| test-act2.txt                  |
| test-act3.png                  |
| test-act3.txt                  |
| test-act4.png                  |
| test-act4.txt                  |
| test-act.png                   |
| test-act.txt                   |
| testChines.org                 |
| testgraph.org                  |
| thinking.org                   |
| time.png                       |
| usecase.org                    |
| usecase.png                    |
| vimium                         |
| 172.17.36.2                    |

** You must add the section :properties: and :end: ,or
nothing show.
:PROPERTIES:
:var: name1="hello"
:END:
#+begin_src sh :var hosts="10.0.2.1"
echo $hosts
echo $name1
#+end_src

#+results:
| 10.0.2.1 |
|    hello |


#+header: :var Dir="/home/"
#+begin_src sh
cd $Dir
ls -lsh
#+end_src

#+results:
| 总用量 | 28K        |    |          |          |      |     |    |       |                      |
| 4.0K   | drwx------ | 43 | root     | root     | 4.0K | 5月 | 30 | 15:19 | happycamp-of-fortran |
| 4.0K   | drwx------ |  5 | root     | root     | 4.0K | 5月 | 30 | 15:36 | happycamp-of-gnuplot |
| 4.0K   | drwx------ | 31 | root     | root     | 4.0K | 6月 |  3 | 13:32 | happycamp-of-lisp    |
| 4.0K   | drwxr-xr-x | 50 | javazhao | javazhao | 4.0K | 6月 |  3 | 14:22 | javazhao             |
| 4.0K   | drwxr-xr-x |  7 | javazhao | javazhao | 4.0K | 4月 | 30 | 16:28 | lispbox-0.7          |
| 4.0K   | -rw-r--r-- |  1 | root     | root     | 1    | 5月 | 13 | 23:37 | MyOrgHome            |
| 4.0K   | drwxr-xr-x |  7 | root     | root     | 4.0K | 4月 | 10 | 22:18 | StudyCenter          |


```

当然emacs中也是可以结合gnuplot ,latex, plantuml,graphviz等。
具体可以参看
[http://www.3zso.com/archives/orgmode-babel.html](http://www.3zso.com/archives/orgmode-babel.html)
