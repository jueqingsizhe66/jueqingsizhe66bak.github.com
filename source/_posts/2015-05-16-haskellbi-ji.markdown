---
layout: post
title: "Haskell笔记"
date: 2015-05-16 14:30:22 +0800
comments: true
categories: Haskell
---

2001年，出生于荷兰的计算机大师、图灵奖获得者EdsgerW.Dijkstra给德州大学预算委员会写信，力劝不要将计算机入门课程改为Java。不幸的是，学校最终还是用Java课程替换了Haskell。Haskell真的不行了？国内有些人的[评论](http://www.vaikan.com/why-haskell-is-worth-learning/)

Haskell:函数式编程语言
Java  : 命令编程语言
尽管Haskell不完美，但仍然比Java好几个数量级，Java就是一个大杂烩(它是通过大范围的广告和销售员夸张的宣传才达到它的商业接受)。

0. 如果别人问你Haskell能够做什么？
一个能够编程的编程语言，高级函数编程语言

1. 如果别人问你Haskell和scheme有什么区别？
比scheme更强调类型理论，但是相对scheme由于对于type theory的过度追求
反而更加难于编程
<!--more-->

2. 如果别人问你Haskell 如何实现冒泡法  直接排序  快速排序？

2.1冒泡排序
``` haskell

sort1 [x] = [x]
sort1 (x:x1:xs)  {-选取了一个x1temp 值  x当做首数，x1:temp-}
    | x>x1 = x1:sort1 (x:xs)
    | otherwise = x:sort1 (x1:xs)
mpsort [] = []
mpsort x = let tmp = sort1 x in
           mpsort (init tmp) ++ [(last tmp)] 
```
2.2直接排序

``` haskell
import Data.List
selectsort::[Int] -> [Int]
selectsort [] = []
selectsort [x] = [x]
selectsort xs = minimum xs : selectsort ( delete ( minimum xs )  xs) 

```
2.3.1快速排序1
``` haskell

quicksort :: (Ord a) => [a] -> [a]   
quicksort [] = []   
quicksort (x:xs) =   
  let smallerSorted = quicksort [a | a <- xs, a <= x]  
      biggerSorted = quicksort [a | a <- xs, a > x]   
  in smallerSorted ++ [x] ++ biggerSorted
```
2.3.2快速排序2
``` haskell
quicksort :: (Ord a) => [a] -> [a]     
quicksort [] = []     
quicksort (x:xs) =      
    let smallerSorted = quicksort (filter (<=x) xs) 
        biggerSorted = quicksort (filter (>x) xs)    
    in  smallerSorted ++ [x] ++ biggerSorted
```
3. 如果别人问你递归是如何实现的？


4. 如果别人问你如何构造Fibonaaci数？

5. 如果别人问你::  => ->是什么意思？
(==) :: Eq a => a -> a -> Bool
可以这样阅读：  相当函数的两个相同的类型值作为参数并返回一个bool值，而这两个参数的类型同在Eq类之中(即类型约束）
eq这一类型 提供了判断相等性的接口，凡是可以比较相等性的类型嗯必属于Eq类
（=>左边的部分叫做类型约束）
a表示人意的类型 ---------泛形 ---多态函数挂钩。。。。

::  读作他的类型为   （类似 !! 从list去除 第几个元素   <- 属于  `elem`是什么的元素   `mod` 可以和什么除。。
6. 如果有人问你如何用haskell编写函数？
他和scheme有很大的不同，scheme用的是(define)
而haskell用的是模式匹配，比如下面的求最大值的程序
```    
maximum' :: (Ord a) => [a] -> a   
maximum' [] = error "maximum of empty list"   
maximum' [x] = x   
maximum' (x:xs)    
    | x > maxTail = x   
    | otherwise = maxTail   
    where maxTail = maximum' xs
```
7. 如果别人叫你简化上面的maximum程序
```
maximum' :: (Ord a) => [a] -> a   
maximum' [] = error "maximum of empty list"   
maximum' [x] = x   
maximum' (x:xs) = max x (maximum' xs) 
```
8. 如果别人问你Haskell到底长成啥子样子？
你会告诉他他涨起来真的是一节一节的，一段一段的，有棱有角

9. 如果有人问你replicate 3 5是如何实现的？
```
replicate' :: (Num i, Ord i) => i -> a -> [a]   
replicate' n x   
| n <= 0    = []   
| otherwise = x:replicate' (n-1) x
```
10. 如果有人问你take是如何实现的呢？ drop?
```
take' :: (Num i, Ord i) => i -> [a] -> [a]   
take' n _   
| n <= 0   = []   
take' _ []     = []   
take' n (x:xs) = x : take' (n-1) xs
```
11. 如果有人问你\_ 下划线表示什么意思？
_ 代表任意东西


12. 如果有人问你reverse如何实现？
```
reverse' :: [a] -> [a]   
reverse' [] = []   
reverse' (x:xs) = reverse' xs ++ [x]
```
13. 如果有人问你repeat如何实现？
```
repeat' :: a -> [a]   
repeat' x = x:repeat' x
```
14. 如果有人做了一个tuple ，想要用zip产生更多tuples？
```
zip' :: [a] -> [b] -> [(a,b)]   
zip' _ [] = []   
zip' [] _ = []   
zip' (x:xs) (y:ys) = (x,y):zip' xs ys
```
15. 如果有人问你mod的类似函数  elem的实现？
```
elem' :: (Eq a) => a -> [a] -> Bool   
elem' a [] = False   
elem' a (x:xs)   
| a == x    = True   
| otherwise = a `elem'` xs
```
16. 如果有人问你typeclass有哪里类？
利用 type 自己查去

17. 如果有人问你Haskell有没有类似scheme的lambda函数？
告诉他有，而且更加直接，虽然刚开始书写起来不方便

scheme版的lambda:
```
    (lambda (x y)
       (+ x y))
```
Haskell版的lambda:
```
    (\y -> y + 3）
    其中\后面跟着参数  ->后面跟着函数体
```
18. 如果有人问haskell有没有存在高阶函数？
有 。 map ,filter
```
map (\y-> y+3) [3,5,6]
```
19. 如果有人问你flip有什么含义？
```
let f x  y = x / y
Prelude> map (f 2) [1,2,3]
[2.0,1.0,0.6666666666666666]
Prelude> map (flip f 2) [1,2,3]
[0.5,1.0,1.5]
```
20. 如何有人问 如何运行 2048
```
$ runhaskell putchar_test.hs 
use runhaskell to run the *.hs programme
```
21. 如果有人问haskell实现递归的自然表达式？
```
quicksort (x:xs) =
elem' a (x:xs) 

zip' (x:xs) (y:ys) = (x,y):zip' xs ys
reverse' (x:xs) = reverse' xs ++ [x]

take' n (x:xs) = x : take' (n-1) xs
maximum' (x:xs) = max x (maximum' xs)
maximum' (x:xs)   
```
你可以看见每一个形式 都有
函数名  函数使用方式   函数的参数表    
这是一种模式,因为Haskell使用函数的时候，先写出函数，然后空格 写出变量 紧接着其他可选变量
所以你也函数匹配模式 也应该是这样来写
基于这种思想，你重新回过头看函数定义，你会发现还真的是这样额？！

977962857@qq.com
从函数的定义知道他的用法，这就是Haskell与众不同的一点。
    
22. 如果有人问如何写出阶乘形式？
告诉他用函数，告诉他用函数匹配，告诉他用递归。。。。。
```
factorial :: (Integral a) => a -> a   
factorial 0 = 1   
factorial n = n * factorial (n - 1) 
```
23. 如果别人问你Haskell如何新建一中类型？
暂时不太懂
但是我知道 Haskell使用 data来定义类别
julia使用type来定义

24. 如果别人问你monads 是什么玩意？
其实这个是更加深层次的运用Haskell,在scheme也有


25. 如果别人问你如何学习haskell？
告诉他有一篇文章叫做 如此有趣的学习Haskell
Haskell趣學指南   http://learnyouahaskell-zh-tw.csie.org/zh-cn/chapters.html 
还不错 ，这个教程

学习Haskell ，要是你能够对于空格特别敏感，会对你有所好处，因为很多地方都是用空格分开
而不是（）  [] 之类的。空格有实际意义，分开不同的字段
26. 如果别人问你 | 有什么意义？
分类说明
```
bmiTell :: (RealFloat a) => a -> a -> String   
bmiTell weight height   
    | weight / height ^ 2 <= 18.5 = "You're underweight, you emo, you!"   
    | weight / height ^ 2 <= 25.0 = "You're supposedly normal. Pffft, I bet you're ugly!"   
    | weight / height ^ 2 <= 30.0 = "You're fat! Lose some weight, fatty!"   
    | otherwise                   = "You're a whale, congratulations!"

```

每一个函数定义的时候，在Haskell看来只是其中的一种模式，一个模式下可以有很多的分类，如果他找不到模式
就说找不到，所以定义的时候，尽可能的完善定义。
比如下面的lucky函数 和sayMe 函数
lucky函数:

```
lucky :: (Integral a) => a -> String   
lucky 7 = "LUCKY NUMBER SEVEN!"   
lucky x = "Sorry, you're out of luck, pal!" 

```
sayme函数:
```
sayMe :: (Integral a) => a -> String   
sayMe 1 = "One!"   
sayMe 2 = "Two!"   
sayMe 3 = "Three!"   
sayMe 4 = "Four!"   
sayMe 5 = "Five!"   
sayMe x = "Not between 1 and 5"
```

27. 如果别人叫你简化bmiTell函数
使用where
```
bmiTell :: (RealFloat a) => a -> a -> String   
bmiTell weight height   
    | bmi <= 18.5 = "You're underweight, you emo, you!"   
    | bmi <= 25.0 = "You're supposedly normal. Pffft, I bet you're ugly!"   
    | bmi <= 30.0 = "You're fat! Lose some weight, fatty!"   
    | otherwise   = "You're a whale, congratulations!"   
    where bmi = weight / height ^ 2
```
28. 如果别人问你 where可以跟多少个变量 ？
跟他说 很多个。
```
bmiTell :: (RealFloat a) => a -> a -> String   
bmiTell weight height   
    | bmi <= skinny = "You're underweight, you emo, you!"   
    | bmi <= normal = "You're supposedly normal. Pffft, I bet you're ugly!"   
    | bmi <= fat    = "You're fat! Lose some weight, fatty!"   
    | otherwise     = "You're a whale, congratulations!"   
    where bmi = weight / height ^ 2   
          skinny = 18.5   
          normal = 25.0   
          fat = 30.0
```
函数在 where 绑定中定义的名字只对本函数可见，因此我们不必担心它会污染其他函数的命名空间。注意，其中的名字
都是一列垂直排开，如果不这样规范，Haskell 就搞不清楚它们在哪个地方了
29. 如果有人问你 where当中可以使用模式匹配吗？

可以。
```
where bmi = weight / height ^ 2   
(skinny, normal, fat) = (18.5, 25.0, 30.0) 

```
30. 如果别人叫你写一个show类型的函数？
```
tell :: (Show a) => [a] -> String   
tell [] = "The list is empty"   
tell (x:[]) = "The list has one element: " ++ show x   
tell (x:y:[]) = "The list has two elements: " ++ show x ++ " and " ++ show y   
tell (x:y:_) = "This list is long. The first two elements are: " ++ show x ++ " and " ++ show y  
```
这个函数顾及了空 List，单元素 List，双元素 List 以及较长的 List，所以这个函数很安全。(x:[]) 与 (x:y:[]) 也可以写作 [x] 和 [x,y] (有了语法糖，我们不必多加括号)。不过 (x:y:_) 这样的模式就不行了，因为它匹配的 List 长度不固定。

31. 如果别人问你Int 和Integer的区别？
Int 有上下线  Integer没有
```
Prelude> minBound :: Int
-9223372036854775808
Prelude> min
min       minBound  minimum
Prelude> minBound  :: Integer 

<interactive>:247:1:
    No instance for (Bounded Integer) arising from a use of `minBound'

```
32. 如果别人问你char and [char] 的区别？
```
Prelude> :t 'a'
'a' :: Char
Prelude> :t "a"
"a" :: [Char]
Prelude> :t "dfasdf"
"dfasdf" :: [Char]
Prelude> 
```

33. 如果别人问你变量有类型，那么函数是不是也有类型？
```
removeNonUppercase :: [Char] -> [Char]   
removeNonUppercase st = [ c | c <- st, c `elem` ['A'..'Z']]
```
removeNonUppercase 的类型为 [Char]->[Char]，从它的参数和回传值的类型上可以看出，
它将一个字串映射为另一个字串。[Char] 与 String 是等价的，但使用 String 会更清晰：
removeNonUppercase :: String -> String。编译器会自动检测出它的类型，我们还是标明了
它的类型声明。要是多个参数的函数该怎样？如下便是一个将三个整数相加的简单函数
参数之间由 -> 分隔，而与回传值之间并无特殊差异
34. 如果别人问你有没有比较好用的编程工具？
告诉他Haskell！

该如何学习？
告诉他安装 ghci,并且在软件中输入 :brower  就能观察到很多的类型
并且支持tab键。 
:brower 会列出内置函数的 类型，以及他的基本使用方法

35. 如果别人问你高阶函数是如何定义？
高阶函数

```
map f lst --将lst按照函数f映射得到一个新的数列
map :: (a->b) -> [a] ->
map f [] = []
map f (x:xs) = f x : map f xs

```
36. 如果别人问你如何使用 :brower 提供的函数类型？
比如：
```
36.1
Prelude> :t product 
product :: Num a => [a] -> a
Prelude> product [3,5,2]
30
36.2
readFile :: FilePath -> IO String

Prelude> readFile "/tmp/a.txt"
"fdsf"
Prelude> let  d=readFile "/tmp/a.txt"
Prelude> d
"fdsf"

```
37.  如果别人问你如何切断一个数组在某个位置？
```
splitAt :: Int -> [a] -> ([a], [a])
Prelude> splitAt 3 [3,5,6,2,3,6,3]
([3,5,6],[2,3,6,3])

```
38.  如果别人问你为什么你这样使用subtract?
告诉他，我是看着定义来使用的。
```
subtract :: Num a => a -> a -> a
Prelude> subtract 20 3
-17
Prelude> subtract [30,3] [3,3]
```

39.  如果别人问你 zip相关的函数？
```
    Prelude> unzip [(3,4)]
    ([3],[4])
    Prelude> unzip [(3,4),(3,7)]
    ([3,3],[4,7])
    Prelude> unzip [(3,4),(3,7),(7,4)]
    ([3,3,7],[4,7,4])
    Prelude> unzip [(3,4),(3,7),(7,4),(8,9)]
    ([3,3,7,8],[4,7,4,9])
    Prelude> unzip [((3,4),(3,7)),((7,4),(8,9))]
    ([(3,4),(7,4)],[(3,7),(8,9)])

    unzip3 :: [(a, b, c)] -> ([a], [b], [c])

    Prelude> unzip3 [(3,6,3)]
    ([3],[6],[3])
    Prelude> unzip3 [(3,6,3),(3,5,9)]
    ([3,3],[6,5],[3,9])
    Prelude> unzip3 [(3,6,3),(3,5,9),(8,9,0)]
    ([3,3,8],[6,5,9],[3,9,0])
    Prelude> unzip3 [(3,6,3,5),(3,5,9,3),(8,9,0,6)]

    <interactive>:172:9:
        Couldn't match expected type `(a0, b0, c0)'
                    with actual type `(t0, t1, t2, t3)'

```
40. 如果别人问你un相关的函数？
```
Prelude> unlines ["ds","dfsf"]
"ds\ndfsf\n"
until :: (a -> Bool) -> (a -> a) -> a -> a
unwords :: [String] -> String
Prelude> unwords ["dsf","fdsf"]
"dsf fdsf"
```

41. 如果比人问你如何使用 sin等三角函数？
转换为弧度制
```
Prelude> sin (45/180*pi)
0.7071067811865475
Prelude> sin (45*pi/180)
0.7071067811865475

```
42. 如果别人问你Haskell针对字符的函数？
Haskell中预定义的针对字符的函数有:
```
   isAscii, isLatin1, isControl, isPrint, isSpace, isUpper, isLower,
   isAlpha, isDigit, isOctDigit, isHexDigit, isAlphaNum,
   digitToInt, intToDigit,
   toUpper, toLower,
   ord, chr,等
   
```
   ord将字母转换为数字, chr反之.

43. 如果别人问你构造数组的几种方式？
数列是通过[]来进行描述的
43.1 数组构造
数列是用[]和(:)构造的, []是一个空的数列, x:xs的含义是元素x附加到数列xs的前面组成一个更长的数列.
比如, 1:[] 等于[1], 2:3:1:[]等于[2,3,1], 运算符(:)是从右向左运算的. 所有的数列都可以看作是从[]开始,
将各元素用(:)附到上面形成的. 在实际编程中有一些简记法可以快速地构造数列. 
43.2 列举法 
将数列的元素一一列举, 比如: [1,2,3], ['A','B','d'], [[1,2], [4,5,6]]等等, 
数列的类型用"[元素类型]"来表示, 这几个例子的类型依次为: [Int], [Char], [[Int]].
43.3 范围法
适用于构造等差数列, 比如: [1..5]等于[1,2,3,4,5], ['a'..'d']等于['a','b','c','d']等于"abcd"因为type String=[Char].
默认的等差为1, 也可以给出前两个元素指定等差, 比如: [2,4..8]等于[2,4,6,8], [2,4..7]等于[2,4,6], [2.5,4..9.5]等于
[2.5,4.0,5.5,7.0,8.5,10.0].

43.3 描述法
描述法给出数列中元素取值的范围以及所满足的条件, 与数学中集合的描述法是一样的. 例如:
[3*x+2| x<-[3,6,9]] --记号"<-"表示属于,x依次取3,6,9,代入3*x+2,得到数列[11,20,29]
[x*x| x<-[1..9], x `rem` 3==1] --给出x的范围,还限定x除3余1
[(x,y)|x<-[1,2,3],y<-[x..3]] --等于 [(1,1),(1,2),(1,3),(2,2),(2,3),(3,3)]

44. 如果别人跟你说Haskell可以write poet?
第一次语文  数学   计算机的结合
```
Prelude>  let nouns = ["he","she","I"]
Prelude> let adjectives=["lazy","handsome","something"]
Prelude> [adjective ++ " " ++ noun | adjective <-adjectives ,noun<-nouns]
["lazy he","lazy she","lazy I","handsome he","handsome she","handsome I","something he","something she","something I"]

*Main> let nouns = ["he","she","I"]
*Main> let adjectives=["lazy","handsome","something"]
*Main> [adjective ++ " " ++ noun | adjective <-adjectives ,noun<-nouns]
["lazy he","lazy she","lazy I","handsome he","handsome she","handsome I","something he","something she","something I"]
```
45. 如果别人跟你说Haskell可以跟几何并轨？
第一次和几何的触碰

```
Prelude>  let triangels=[(a,b,c)|c<-[1..10],b<-[1..c],a<-[1..b],c^2==a^2+b^2,a+b+c==24]
Prelude> triangels 
[(6,8,10)]

```
46. 如果别人问你Haskell的cabal是什么？
类似于ubuntu的apt-get   
scilab  的atoms
python的 easy_install  或者pip
redhat 的yum

47. 如果别人问你有几个基本的TypeClass?
47.1 Eq
包含可判断相等性的类型。提供实现的函数是 == 和 /=。所以，
只要一个函数有Eq类的类型限制，那么它就必定在定义中用到了 == 和 /=
47.2 Ord
包含可比较大小的类型。除了函数以外，我们目前所谈到的所有类型都属于 Ord 类。Ord 包中包含了
<, >, <=, >= 之类用于比较大小的函数。compare 函数取两个
Ord 类中的相同类型的值作参数，回传比较的结果。这个结果是如下三种类型之一：GT, LT, EQ。
47.3 Show
的成员为可用字串表示的类型。目前为止，除函数以外的所有类型都是 Show 的成员。
操作 Show Typeclass，最常用的函数表示 show。它可以取任一Show的成员类型并将其转为字串
47.4 Read
是与 Show 相反的 Typeclass。read 函数可以将一个字串转为 Read 的某成员类型。

47.5 Enum
的成员都是连续的类型 -- 也就是可枚举。Enum 类存在的主要好处就在于我们可以在 Range 中用到它的成员类型：
每个值都有后继子 (successer) 和前置子 (predecesor)，分别可以通过 succ 函数和 pred 函数得到。
该 Typeclass 包含的类型有：(), Bool, Char, Ordering, Int, Integer, Float 和 Double。
47.6 Bounded
的成员都有一个上限和下限。
47.7 Num
Num 是表示数字的 Typeclass，它的成员类型都具有数字的特征。检查一个数字的类型：
47.8 Integral
同样是表示数字的 Typeclass。Num 包含所有的数字：实数和整数。而 Integral 仅包含整数，其中的成员类型有 Int 和 Integer。
47.9 Floating
仅包含浮点类型：Float 和 Double。

48. 我想用Haskell产生一个三角形
```
let triangels=[(a,b,c)|c<-[1..10],b<-[1..10],a<-[1..10]]
let righttriangels=[(a,b,c)|c<-[1..10],b<-[1..c],a<-[1..b],a^2+b^2==c^2]
let righttriangelsc=[(a,b,c)|c<-[1..10],b<-[1..c],a<-[1..b],a^2+b^2==c^2,a+b+c==24]

```
49. 如果别人问你如何表示数学的集合？
```
Prelude> [x*2|x <- [1..10]]
[2,4,6,8,10,12,14,16,18,20]
Prelude> [x*2|x<- [1..10],x*2 >=12 ]
[12,14,16,18,20]
Prelude> [x | x <- [50..100], x `mod` 7 ==3]
[52,59,66,73,80,87,94]

```
50. 如果别人问你!! 代表什么东西？
```
[3, 5,6] !!2
*Main> [3, 5,6] !!2
6
```

51. 几个常用的数组提取函数？
并且是从0开始的
常用几个list函数
```
*Main> head [5,6,4]
5
*Main> last [5,6,4]
4
*Main> tail [5,6,4]
[6,4]
*Main> init [5,6,6,43,6]
[5,6,6,43]


```
length  null  reverse  take等函数  具体说一下take

```
*Main> take 4 [5,6,7,4,3,8]
[5,6,7,4]
*Main> take 3 [5,6,7,4,3,8]
[5,6,7]
*Main> take 2 [5,6,7,4,3,8]
[5,6]
*Main> 
```
同理类似的drop


