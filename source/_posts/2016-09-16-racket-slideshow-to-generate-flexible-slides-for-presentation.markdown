---
layout: post
title: "racket slideshow to generate flexible slides for presentation"
date: 2016-09-16 20:52:01 +0800
comments: true
categories: scheme
---


本不了解slideshow, 后通过 `prolog site:github.io `找到一些slideshow资料。

+ [高校人才和企业需求脱离][1]
+ [高效演示Racket slideshow][2], and [some tipsi][3] ,Slideshow的魅力在于让你可以把关于制作幻灯片的<font color="red">经验、模式</font>，通过代码固化下来，并且降低<strong>重用和分享</strong>的难度 .Slideshow可以帮你把<font color="red">内容和样式</font>进行干净的分离，随着使用经验的增加，你会积累更多的<strong>样式函数</strong>，那时就可以把精力放在幻灯片的<u>内容上</u>，从而提高生产率。 

<!--more-->

+ [prolog and graph][4]
+ [John Wickerson][5]
+ [aspect-orient programming with prolog][6]
+ [programming-notes: 不错的风格][7]
+ [asm汇编版8皇后问题][8]
+ [octopress-workflow 不错][9]
+ [CS130:what is prolog good for(wolf goat cabbige)][10]
+ [Using Prolog to Solve Logic Puzzles ][11]
+ [Real world usage of prolog][12]
+ [the practical-application-of-prolog][13]
+ [scheme:不确定性(amb)--scheme语言简明教程][14]
+ [prolog-java just a little][15]
+ [日本人写的prolog也挺有意思][16]
+ [浏览器背后的加密算法(和搜索prolog主题不相关)][17]
+ [Two Prolog solutions to TSP][18]
+ [Logic programming][19]
+ [相当不错的blog关于slideshow prolog maching leanring 等][20]
+ [8皇后 8种语言][21]

底下开始是一些关于slideshow使用过程的备注

所有的slideshow都是由

+ slide对象
+ picts组合起来

一个show包含着很多的slides，每一个slide(也就是一个幻灯片)包含着很多picts对象

pict对象，常见的有:

+ 文本pict
    - text
    - t
    - tt
    - colorize
    - highlinetext
    - bt
    - it
+ 布局pict
    - 平面排布
        + vc-append
        + vl-append
        + vr-append
        + hc-append
        + ht-append
        + hd-append
        + htd-append
    - 纵深排布
        + c-superimpose
        + cc-superimpose
        + b-superimpose
        + bl-superimpose
        + t-superimpose
        + tl-superimpose
+ 图像pict
    - bitmap
+ 表格pict
    - table
+ 列表pict
    - item
    - subitem
+ 轮廓pict
    - make-outline
    - sub-para
+ 播放控制pict
    - next
    - alts(用于a list of lists)
    - alts!
+ 代码段pict
    - code
+ 空pict
    - (blank)

<hr/>
<hr/>
<hr/>
<hr/>

一些常用的代码片段:

1. [common.el](#common)
2. [outline](#outline)
3. [item](#item)
4. [table](#table)
5. [size](#size)
6. [ghost](#ghost)
7. [used to](#customer)
8. [three-column show](#three)
9. [item and ghost](#ident)
10. [if control show](#if)

<h3 id="common">1. 常用的工具指令</h3>
    + title
    + s
    + topic
    + on-lt
    + left-right-panel
    + top-bottom-panel
    + tbp

``` racket
;
; TODO
; Unify title and c-title
;

#lang slideshow

(provide title)
(provide s)
(provide topic)
(provide on-lt)
(provide left-right-panel)
(provide top-bottom-panel)
(provide tbp)

(define (adapt content)
    (if (= 1 (length content))
	         (if (string? (car content))
		   (s (car content) 64)
		   (car content)
		 )
                 (apply vc-append content)
	       )
)

(define title
  (lambda content
    (slide (vr-append (* gap-size 5)
             (vc-append 
               (* gap-size 3)
               (adapt content)            
             )
             (vr-append (* gap-size 0.5)
                (text "Ye Zhaoliang" (cons 'bold (current-main-font)) 32)
                (text "http://jueqingsizhe66.github.com" (current-main-font) 24)
		(hc-append 
                  (text "Fluid Lab " (cons 'bold (current-main-font)) 24)
                  (text "@NCEPU" (current-main-font) 24)
                )
                (text "@yzl" (cons 'bold (current-main-font)) 32)
             )
	   )
    )
  )
)


(define (s content size) (text content (current-main-font) size))

;
; One-Topic-One-Slide pattern.
;
(define topic
  (lambda contents
    (cond
      ((= 1 (length contents)) (slide (s (car contents) 70)))
      ((= 2 (length contents))
           (slide 
             (vl-append (* gap-size 1.5)
               (s (car contents) 70)
               (s (car (cdr contents)) 30)
             )
           )
      )
    )
  )
)

; Helper function
(define (adpot-picts content) 
  (if (list? content)
    (apply vc-append content)
    (car content)
  )
)

;
; Locate content on left top corner.
;
(define on-lt 
  (lambda content
    (slide (lt-superimpose (blank client-w client-h) (adpot-picts content)))
  )
)

;
; Split panel into left and right
;
(define left-right-panel
  (lambda (left right [left-superimpose cc-superimpose] [right-superimpose cc-superimpose])
    (slide
      (ht-append 
        (left-superimpose (blank (/ client-w 2) client-h) left) 
        (right-superimpose (blank (/ client-w 2) client-h) right)
      )
    )
  )
)

;
; Split panel into top and bottom
;
(define top-bottom-panel
  (lambda (top bottom [top-superimpose cc-superimpose] [bottom-superimpose cc-superimpose])
    (slide
      (tbp top bottom top-superimpose bottom-superimpose)
    )
  )
)

(define tbp
  (lambda (top bottom [top-superimpose cc-superimpose] [bottom-superimpose cc-superimpose])
    (vc-append
      (top-superimpose (blank client-w (/ client-h 2)) top)
      (bottom-superimpose (blank client-w (/ client-h 2)) bottom)
    )
  )
)

(define sperator-hl (colorize (hline 400 1) "blue"))
(define sperator-vl (colorize (vline 700 1) "red"))

;:w
;(set-margin! 0)

(define black-bg
  (filled-rectangle client-w client-h)
)

(current-slide-assembler
  (lambda (s v-sep c)
    (ct-superimpose
      black-bg
      (if s
        (vl-append v-sep (para (titlet s)) (colorize c "white"))
        (colorize c "white")
      )
    )
  )
)

;(current-title-color "white")
(current-title-color "black")
;current-slide-assembler函数接受一个lambda类型的参数，这个lambda接受三个参数。black-bg函数会画一个和用户可见区域一样大小的黑色矩形——这就是“黑底”了。接下来将判断当前幻灯片是否有title，如果有，则将title和内容垂直左对齐；如果没有，则直接显示内容。注意，无论是否有Title，都会将文字的颜色改为白色。最后调用(current-title-color “white”)，目的是把Title文字的颜色也改为白色。这就是“白字”了。下面的图4和图5是同样的幻灯片，在改变Master前后的效果，


```


<h3 id="outline">2. outline制作</h3>

主要用到了make-outline函数。

```
;; ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; Part II starts here

(define outline 
  (let ([sub-para (lambda l
                    (para #:width (* 3/4 (current-para-width)) l))])
    (make-outline
     'one "Part I: Basic Concepts" 
     #f
     
     'two "Part II: Practical Slides" 
     (lambda (tag)
       (sub-para "Using" (code make-outline) "and more..."))
     
     'three "Part III: Fancy Picts" 
     (lambda (tag)
       (sub-para "Creating interesting graphics"))
     
     'four "Part IV: Advanced Slides" 
     (lambda (tag)
       (sub-para "Beyond" (code 'next) "and" (code 'alts)))
     
     'background "Part V: Controlling the Background"
     (lambda (tag)
       (sub-para "Changing the overall look of your talk"))

     'printing "Part VI: Printing"
     (lambda (tag)
       (sub-para "Exporting slides as PostScript"))

     'end "Conclusion" 
     (lambda (tag)
       (sub-para "This is the end")))))


(outline 'two)


```

<h3 id="item">3. item的bullet自定义</h3>

主要用到了item的#:bullet选项。
``` racket


  (slide
   #:title "Slides and Picts"
   (para "The body of a Slideshow program")
   (item #:bullet (bt " 1.")
         "Makes and combines" (hbl-append (bit "pict") (t "s")))
   (sub-para "For example,")
   (code (t "Hello"))
   (sub-para "creates a pict like this:")
   (colorize (t "Hello") "black")
   (item #:bullet (bt " 2.") "Registers certain picts as slides")
   (sub-para "For example,")
   (code (slide (t "Hello")))
   (sub-para "registers a slide containing only" (colorize (t "Hello") "black")))

```
<h3 id="table">4. table使用</h3>

注意配合上frame和inset(用于增加frame的margin)

``` racket
(slide
 #:title "Tables"
 (para "The" (code table) "function makes rows and columns")
 (frame
  (inset
   (table 3 ; three columns
          (list (t "First") (standard-fish (* 2 gap-size) gap-size) (code cons)
                (t "Second") (jack-o-lantern (* 4 gap-size)) (code car)
                (t "Third") (cloud (* 3 gap-size) gap-size) (code cdr)
                (t "Fourth") (file-icon (* 2 gap-size) (* 3 gap-size) #t) (code null?))
          (list* lc-superimpose  ; left-align first column
                 cc-superimpose) ; h-center the rest
          cc-superimpose ; v-center all rows
          gap-size  ; separate all columns by gap-size
          gap-size) ; separate all rows by gap-size
   gap-size))
 (note "The above also uses" (code standard-fish) ","
       (code jack-o-lantern) "," (code cloud) ", and"
       (code file-icon)))

```

<h3 id="size">5. 几个常见的尺寸 </h3>

superimpose其实就是使得picts对象们具有深度的感觉
相当于幻灯片的置于底层的作用

Cc-superimpose 默认是强制施加在空frame上面，否则无法显示vc-append信息。

``` racket
(slide
  (cc-superimpose 
 (frame (blank client-w client-h))
 (text "几个常用的尺寸")
 (vc-append
  (hc-append (code pict-width ) (t "获得任意pict对象的宽度:") (t (number->string (pict-width (t "hello world")))))
  (hc-append (code pict-height ) (t "获得任意pict对象的宽度:") (t (number->string (pict-height (t "hello world")))))
  (hc-append (code gap-size) (t "当前gap-size 的大小:") (t (number->string gap-size)))
  (hc-append (code (current-para-width)) (t "当前的paragraph的宽度:") (t (number->string (current-para-width))))
  (hc-append (code (current-font-size)) (t "当前的字体的大小是:") (t (number->string (current-font-size))))
  (hc-append (code client-h) (t "当前窗口的高度:") (t (number->string client-h)))
  (hc-append (code client-w) (t "当前窗口的宽度:") (t (number->string client-w)))
  
  )))

```

<h3 id="ghost">6. ghost遮盖效果</h3>

ghost主要用于动画演示时候。

``` racket

#lang slideshow
(define v vl-append)
(define h hc-append)
(define (s-ps str size)
  (text str 'modern size))
(define (ps str)
  (s-ps str (current-font-size)))
(define (ident x) x)
(define (function-invocations show-group-1 show-group-2 show-group-3)
  (slide #:title "Function Invocation"
    (v (* gap-size 1.5)
       (show-group-1 (ps "> Hello (\"World\")"))
       (show-group-1 (ps "> Hello \"World\""))
       (show-group-2 (ps "> Hello World"))
       (show-group-3 (ps "> Hello -name World"))
       (show-group-3 (ps "> Hello -name \"World\"")))
    (blank)
    ((if (and (equal? show-group-1 ident)
              (equal? show-group-2 ident)
              (equal? show-group-3 ident)) ident ghost)
         (colorize (h (text "All" '(bold . modern) (current-font-size))
                      (t " commands are in ")
                      (text "\"COMMAND + ARGS\"" '(italic bold . modern) (current-font-size)) (t " style."))
                   "blue"))))

(function-invocations ident ghost ghost)
(function-invocations ident ident ghost)
(function-invocations ident ident ident)
```

<h3 id="customer">7. slideshow的一些习惯定义</h3>

``` racket
(define (category c)
  (text c (cons 'bold (current-main-font)) 24)
)

(define (name text)
  (s text 24)
)

(define (address text)
 (s text 20))

(define (position text)
 (s text 20))

(define (s-t content size) (text content (current-main-font) size))
(define (ident x) x)

(define v vl-append)
(define h hc-append)

(define (ps str) (s-ps str (current-font-size)))
(define (highlight str) (s-highlight str (current-font-size)))
(define (keyword str) (s-keyword str (current-font-size)))

(define (s-ps str size) (text str 'modern size))
(define (s-highlight str size) (colorize (text str '(bold . modern) size) "red"))
(define (s-keyword str size) (colorize (text str '(bold . modern) size) "blue"))


```

<h3 id="three">8. 三列杆显示</h3>

``` racket
(define three-column-panel
  (lambda (c1 c2 c3 [c1-superimpose ct-superimpose] [c2-superimpose ct-superimpose] [c3-superimpose ct-superimpose])
    (slide
      (ht-append 
        (c1-superimpose (blank (/ client-w 3) client-h) c1) 
        (c2-superimpose (blank (/ client-w 3) client-h) c2)
        (c3-superimpose (blank (/ client-w 3) client-h) c3)
      )
    )
  )
)

(define (category c)
  (text c (cons 'bold (current-main-font)) 24)
)

(define (name text)
  (s text 24)
)

(three-column-panelr
  (vc-append (* gap-size 2)
    (vc-append
      (category "Availablity")
      (name "Hystrix (2666 stars)")
      (name "Simianarmy (2327 stars)")
      (name "Turbine")
    )
    (vc-append
      (category "Cloud Management")
      (name "Ice (1123 stars)")
      (name "Asgard (1574 stars)")
      (name "Frigga")
      (name "Glisten")
    )
    (vc-append
      (category "Persistence Systems")
      (name "Astyanax")
      (name "Cassjmeter")
      (name "Curator")
      (name "Evcache")
      (name "Exhibitor")
      (name "Priam")
      (name "Staash")
      (name "Dynomite")
      (name "Dyno")
      (name "Raigad")
    )
  )
  (vc-append (* gap-size 1)
    (vc-append
      (category "In-Memory Data Management")
      (name "Netflix-graph")
      (name "Zeno")
    )
    (vc-append
      (category "Platform Libraries")
      (name "Archaius")
      (name "Denominator")
      (name "Feign")
      (name "Karyon")
      (name "Ribbon")
      (name "Servo")
      (name "Blitz4j")
      (name "Governator")
    )
    (vc-append
      (category "Infrastructure Services")
      (name "Atlas")
      (name "Edda")
      (name "Suro")
      (name "Eureka")
      (name "Zuul")
      (name "Prana")
    )
    (vc-append
      (category "Developer Productivity")
      (name "Gcviz")
      (name "Pytheas")
    )
    (vc-append
      (category "Sample Applications and recipes")
      (name "Recipes-rss")
    )
  )
  (vc-append (* gap-size 2)
    (vc-append
      (category "Big Data Tools")
      (name "Aegisthus")
      (name "Genie")
      (name "Inviso")
      (name "Lipstick")
      (name "Pigpen")
      (name "S3mper")
    )
    (vc-append
      (category "Build and Deploy Tools")
      (name "Aminator")
    )
    (vc-append
      (category "Security")
      (name "Msl")
      (name "Scumblr")
      (name "Security_monkey")
      (name "Sketchy")
    )
    (vc-append
      (category "Others")
      (name "Nfwebcrypto")
      (name "Brutal")
    )
  )
)

```

<h3 id="ident">9. ident和ghost结合控制显示</h3>

``` racket
(define func-keyword (keyword "Function "))
(define func-name (ps "Hello"))
(define arg-list (ps "($name) "))
(define func-body (ps "    Write-Host Hello $name"))

(define source-snap
   (inset (vl-append (* gap-size 1.5) (para func-keyword func-name arg-list (t " {"))
                  (para func-body)
                  (para (t "}"))) 200 0 0 0))

(define (highlight-function-example show-func-keyword show-func-name show-arg-list show-func-body)
  (slide #:title "PowerShell Function"
   (let* ([sence source-snap]
          [w/func-keyword (pin-balloon (wrap-balloon (t "Keyword") 'e 30 0)
                                  (ghost source-snap)
                                  func-keyword
                                  lc-find)]
          [w/func-name (pin-balloon (wrap-balloon (t "Function Name") 's 10 10)
                               (ghost w/func-keyword)
                               func-name
                               ct-find)]
          [w/arg-list (pin-balloon (wrap-balloon (t "Arg List") 'sw -30 10)
                               (ghost w/func-name)
                               arg-list
                               ct-find)]
          [w/func-body (pin-balloon (wrap-balloon (t "Function Body") 'n 10 -10)
                               (ghost w/arg-list)
                               func-body
                               cb-find)])

   (cc-superimpose sence
                   (show-func-keyword w/func-keyword)
                   (show-func-name w/func-name)
                   (show-arg-list w/arg-list)
                   (show-func-body w/func-body)))))

(highlight-function-example ghost ghost ghost ghost)
(highlight-function-example ident ghost ghost ghost)
(highlight-function-example ident ident ghost ghost)
(highlight-function-example ident ident ident ident)



```

<h3 id="if">10. if逐步控制</h3>

``` racket
;
; Slide for PowerShell invocation
;
(define (function-invocations show-group-1 show-group-2 show-group-3)
  (slide #:title "Function Invocation"
    (v (* gap-size 1.5)
       (show-group-1 (ps "> Hello (\"World\")"))
       (show-group-1 (ps "> Hello \"World\""))
       (show-group-2 (ps "> Hello World"))
       (show-group-3 (ps "> Hello -name World"))
       (show-group-3 (ps "> Hello -name \"World\"")))
    (blank)
    ((if (and (equal? show-group-1 ident)
              (equal? show-group-2 ident)
              (equal? show-group-3 ident)) ident ghost)
         (colorize (h (text "All" '(bold . modern) (current-font-size))
                      (t " commands are in ")
                      (text "\"COMMAND + ARGS\"" '(italic bold . modern) (current-font-size)) (t " style."))
                   "blue"))))

(function-invocations ident ghost ghost)
(function-invocations ident ident ghost)

(function-invocations ident ident ident)
```

下面链接也有用:

1. [笑脸][22]
2. [pin-balloon][23]

[1]: http://www.iamhukai.com/?p=149
[2]: http://isaachan.github.io/blog/2013/08/17/slide-textuality/
[3]: http://isaachan.github.io/blog/2013/08/18/slide-textuality-tips/
[4]:http://rlgomes.github.io/work/prolog/2012/05/22/19.00-prolog-and-graphs.html 
[5]: http://johnwickerson.github.io/
[6]:http://bigzaphod.github.io/Whirl/dma/docs/aspects/aspects-man.html 
[7]: http://tobytripp.github.io/programming-notes/alchemy.html 
[8]:http://davidad.github.io/blog/2014/02/25/overkilling-the-8-queens-problem/ 
[9]:http://davidad.github.io/blog/2014/02/11/octopress-workflow/ 
[10]: http://ucsd-progsys.github.io/cse130/static/goat_etc.html
[11]:http://bennycheung.github.io/using-prolog-to-solve-logic-puzzles 
[12]:http://stackoverflow.com/questions/130097/real-world-prolog-usage 
[13]:http://www.drdobbs.com/parallel/the-practical-application-of-prolog/184405220 
[14]:http://songjinghe.github.io/TYS-zh-translation/140-nondeterminism.html 
[15]: http://java-prolog-connectivity.github.io/short_paper.html
[16]:http://muratamuu.github.io/blog/2015/05/24/prolog-samegame-2/ 
[17]:http://isaachan.github.io/blog/2014/07/20/cipher-behind-https/ 
[18]:http://nerddan.github.io/2015/01/07/prolog-tsp.html 
[19]:http://mozart.github.io/mozart-v1/doc-1.4.0/tutorial/node12.html 
[20]:https://github.com/isaachan/on-the-way 
[21]:https://github.com/isaachan/on-the-way/tree/master/eight-queens 
[22]:http://docs.racket-lang.org/pict/More_Pict_Constructors.html?q=pin-ball#%28def._%28%28lib._pict%2Fface..rkt%29._face%29%29 
[23]:http://docs.racket-lang.org/pict/More_Pict_Constructors.html?q=pin-ball#%28def._%28%28lib._pict%2Fballoon..rkt%29._pin-balloon%29%29 
