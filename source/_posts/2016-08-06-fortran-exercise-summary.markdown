---
layout: post
title: "fortran exercise summary"
date: 2016-08-06 15:18:28 +0800
comments: true
categories: fortran
---


how to do it better in the fortran language?

+ [1. checkBox](#millionstone1)
+ [2. decode](#millionstone2)
+ [3. smooth data show](#millionstone3)
+ [4. MeanVariance](#millionstone4)
+ [5. BlankTest](#millionstone5)
+ [6. Class Mark Average](#millionstone6)
+ [7. Combinator functor](#millionstone7)
+ [7. Combinator functor](#millionstone7)
+ [8. Degree Radian Conversion](#millionstone8)
+ [9. Histogram](#millionstone9)
+ [10. NewTon Algorithms](#millionstone10)
+ [11. HeronFomula](#millionstone11)
+ [12. MultipleTable](#millionstone12)
+ [13. Prime Range](#millionstone13)
+ [14. Velocity Projection](#millionstone14)
+ [15. quadratic equation solution](#millionstone15)
+ [16. Test Switch Case](#millionstone16)
+ [17. Two Functions](#millionstone17)
+ [18. YYYYMMDDConversion](#millionstone18)
+ [19. Bisect Method to solve equations](#millionstone19)
+ [20. cmToInch](#millionstone20)
+ [21. ColonTest](#millionstone21)
+ [22. Factorize](#millionstone22)

The codes below are the results of my practice in 2014. I think they will
teach you about the compute process of fortran ,but not the natural compute
process,such as recursion. If you want to deep into the understanding of the 
fortran ,I advice the book [the little scheme][1] & [the season scheme][2]. What's 
more, the main reference material is [the shene fortran90 course][3].

<!--more-->

<br/>
<br/>
<br/>
<hr/>
<br/>
<br/>
<br/>

<h2 id="millionstone1">millionstone1</h2>

checkBox.f90:

``` fortran
!#!-*-author:zhaoliang-*-
!#!-*-coding:utf8-*-
!#Date: 2014年 01月 25日 星期六 20:39:32 CST
!
PROGRAM CheckBox
      IMPLICIT NONE

      CHARACTER(LEN = 1) :: Code
      CHARACTER(LEN = 15) :: Line
      INTEGER             :: Payment , Deposit, Balance, Input
      INTEGER             :: Count
      INTEGER             :: Status
      CHARACTER(LEN=*),PARAMETER :: Filer = "   *******"
      CHARACTER(LEN=*),PARAMETER :: DashLine ="_______________________________________"
      CHARACTER(LEN=*),PARAMETER :: TitleFormat = "(1X,A/ 1X,A//1X,A/1X,A)"
      CHARACTER(LEN=*),PARAMETER :: InputLine = "(A)"
      CHARACTER(LEN=*),PARAMETER :: CheckLine = "(1X,A/1X,I5,SP,3I10/1X,A)"
      CHARACTER(LEN=*),PARAMETER :: PaymentLine = "(1X,I5,SP,I10,A,I10)"
      CHARACTER(LEN=*),PARAMETER :: DepositLine = "(1X,I5,A,SP,2I10)"
      CHARACTER(LEN=*),PARAMETER :: LastLine = "(1X,A/ 1X,A,SP,3I10)"

      WRITE(*,TitleFormat) "Check Book Reference Listing",       &
                           "============================",       &
                           "Count   Payment   Deposit   Balance",&
                           "-----   -------   -------   -------"

      Payment = 0
      Deposit = 0
      Balance = 0
      Count  = 0
      READ(*,*) 
      READ(*,*) 
      Do
          READ(*,InputLine,IOSTAT=Status) Line
          IF(Status < 0) EXIT
          Code  = Line(1:1)
          Count = Count + 1
          SELECT CASE(Code)
            CASE("C","c")
                WRITE(*,CheckLine) DashLine,Count,Payment,Deposit, &
                    Balance,DashLine
            CASE("P","p")
                READ(Line,"(T11,I5)") Input  ! T11 跳到11处开始读取
                Payment = Payment + Input
                Balance = Balance - Input
                WRITE(*,PaymentLine) Count,Input,Filer,Balance
            CASE("D","d")
                READ(Line, "(T6,I5)") Input  ! T6  跳到第6处开始读取
                Deposit = Deposit +Input
                Balance = Balance + Input
                WRITE (*,DepositLine) Count,Filer,Input,Balance
        END SELECT
    END DO
    WRITE(*,LastLine) DashLine ,"Total",Payment,Deposit,Balance
END PROGRAM CheckBox


!这样就可以了！！
!$ tail Checkbill | ./a.out
! Check Book Reference Listing
! ============================
!
! Count    Payment   Deposit   Balance
! -----    -------   -------   -------
!     1   *******      +123      +123
!     2   *******       +78      +201
! ________________________
!     3        +0      +201      +201
! ________________________
!     4       +50   *******      +151
!     5   *******      +100      +251
!     6   *******       +10      +261
!     7      +120   *******      +141
! ________________________
!     8      +170      +311      +141
! ________________________
!     9       +50   *******       +91
!    10   *******      +150      +241
! ________________________
! Total      +220      +461      +241



! Program Input and Output
!If the input data consist of the following:
!
!             1    1
!    ....5....0....5
!    D     +123$$$$$
!    d      +78$$$$$
!    c    $$$$$$$$$$
!    P    $$$$$  +50
!    D     +100$$$$$
!    D      +10$$$$$
!    p    $$$$$ +120
!    C    $$$$$$$$$$
!    P    $$$$$  +50
!    D     +150$$$$$
!
!The output of the program is:
!
!             1    1    2    2    3    3    4
!    ....5....0....5....0....5....0....5....0
!     Check Book Reference Listing
!     ============================
!
!     Count   Payment   Deposit   Balance
!     -----   -------   -------   -------
!         1   *******      +123      +123
!         2   *******       +78      +201
!     -----------------------------------
!         3        +0      +201      +201
!     -----------------------------------
!         4       +50   *******      +151
!         5   *******      +100      +251
!         6   *******       +10      +261
!         7      +120   *******      +141
!     -----------------------------------
!         8      +170      +311      +141
!     -----------------------------------
!         9       +50   *******       +91
!        10   *******      +150      +241
!     -----------------------------------
!     Total      +220      +461      +241
!


!SP 表示 kind 值。
!在大多数编译器，比如 Windows 下的 Visual Fortran 编译器。kind = 4 表示单精度，kind = 8 表示双精度。
!
!也就是说，1.0_8 等同于 1.0D0，表示双精度的 1.0
!
!当然，其他一些编译器，kind 值可能有不同的含义。具体内容，请参阅自己使用的编译器的帮助文档

!  Sp表示数值为正时加上正号
 


!  -------------------------------------------------------------->错误的结果!
!$ cat Checkbill |./a.out 
! Check Book Reference Listing
! ============================
!
! Count   Payment   Deposit   Balance
! -----   -------   -------   -------
!     3   *******      +123      +123
!     4   *******       +78      +201
! _     5        +0      +201      +201
! _______________________________________
!     6       +50   *******      +151
!     7   *******      +100      +251
!     8   *******       +10      +261
!     9      +120   *******      +141
! _    10      +170      +311      +141
! _______________________________________
!    11       +50   *******       +91
!    12   *******      +150      +241
! _______________________________________
! Total      +220      +461      +241


! --------------------------------------------------------------->原因在于
!      CHARACTER(LEN=*),PARAMETER :: TitleFormat = "(1X,A/1X,A//1X,A/1X,A)"
!      CHARACTER(LEN=*),PARAMETER :: InputLine = "(A)"
!      CHARACTER(LEN=*),PARAMETER :: CheckLine = "(1X,A1X,I5,SP,3I10/1X,A)"
!      CHARACTER(LEN=*),PARAMETER :: PaymentLine = "(1X,I5,SP,I10,A,I10)"
!      CHARACTER(LEN=*),PARAMETER :: DepositLine = "(1X,I5,A,SP,2I10)"
!      CHARACTER(LEN=*),PARAMETER :: LastLine = "(1X,A/1X,A,SP,3I10)"
!
!




!-------------------------------------------------------->c错误2

!  无法从1开始计数  那么就先读取两行就可以了！！！！

!Check Book Reference Listing
 !============================

 !Count   Payment   Deposit   Balance
 !-----   -------   -------   -------
 !    3   *******      +123      +123
 !    4   *******       +78      +201
 !_______________________________________
 !    5        +0      +201      +201
 !_______________________________________
 !    6       +50   *******      +151
 !    7   *******      +100      +251
 !    8   *******       +10      +261
 !    9      +120   *******      +141
 !_______________________________________
 !   10      +170      +311      +141
 !_______________________________________
 !   11       +50   *******       +91
 !   12   *******      +150      +241
 !_______________________________________
 !Total      +220      +461      +241

 ! 经过验证之后 只有SP之后的所有的东西！ I10才是具有那种效果
! 已验证过
```

Makefile
``` makefile
FC = gfortran
FFLAGS = -Wall -g
CheckBox : CheckBox.f90
	$(FC) $(FFLAGS) -o $@  $^
```

<br/>
<br/>
<br/>
<hr/>
<br/>
<br/>
<br/>

<h2 id="millionstone2">millionstone2</h2>

decode.f90:
``` fortran
!#!-*-author:zhaoliang-*-
!#!-*-coding:utf8-*-
!#Date: 2014年 01月 26日 星期日 14:57:42 CST

PROGRAM Dedoding
      IMPLICIT NONE
      INTEGER :: SalesNo, Phone,Amount
      INTEGER :: SN_1,SN_2,SN_3
      INTEGER :: Phone_1, Phone_2
      INTEGER :: Number
      INTeger :: Status
      LOGICAL :: Problem

      WRITE(*,"(A,A)") " ", "     Sales Amount Table" 
      WRITE(*,"(A,A)") " ", "     =================="
      WRITE(*,*)
      WRITE(*,"(A,A)") " ", "Sales No.    Phone No.    Amount No."
      WRITE(*,"(A,A)") " ", "---------    ---------    ----------"
      Problem = .FALSE.
      Number = 0
      Do
          READ(*,"(I10,I10,I5)",IOSTAT=Status) SalesNo, Phone ,Amount
          IF(Status > 0) THEN
              WRITE(*,*) "Something wrong in your input data"
              Problem = .TRUE.
              EXIT
          ELSE IF(Status < 0) THEN
              EXIT
          ELSE
              SN_3 = MOD(SalesNo, 1000)
              SN_2 = MOD(SalesNo/1000, 100)
              SN_1 = SalesNo/100000

              Phone_2 = MOD(Phone,10000)
              Phone_1 = Phone/ 10000

              WRITE(*,"(A,I2.2,A,I2.2,A,I3.3,I6.3,A,I4.4,I10)") &
                  " ",SN_1,"-",SN_2,"-",SN_3,Phone_1,"-",Phone_2,Amount
              Number = Number + 1

          END IF
    END DO
    IF (.NOT. Problem) THEN
          WRITE(*,*)
          WRITE(*,"(A,A,I3,A)") " ","Total",Number," person(s) processed."
    END IF

END PROGRAM Dedoding
        
!$ cat salesPhone |./a.out 
!      Sales Amount Table
!      ==================
!
! Sales No.    Phone No.    Amount No.
! ---------    ---------    ----------
! 04-94-094   707-9173      1000
! 03-63-363   634-7862      1120
! 01-01-001   713-0067       750
! 03-23-023   670-0175      1253
! 00-10-810   900-0018       980
!
! Total  5 person(s) processed.


!0----------------------------------------------->上面错误
! 在于   mod(salesNo,1000)  mod(salesno,100)  mod(salesno,10)
!$ tail salesPhone |./a.out 
!      Sales Amount Table
!      ==================
!
! Sales No.    Phone No.    Amount No.
! ---------    ---------    ----------
! 76-45-094   707-9173      1000
! 87-45-363   634-7862      1120
! 87-01-001   713-0067       750
! 40-00-023   670-0175      1253
! 88-01-810   900-0018       980
!
! Total  5 person(s) processed.
```


salesPhone:
```
7645094   7079173   1000
8745363   6347862   1120
8701001   7130067   750
4000023   6700175   1253
8801810   9000018   980
```

run.sh:

``` sh
#!/bin/sh
cat salesPhone|./a.out
```


<br/>
<br/>
<br/>
<hr/>
<br/>
<br/>
<br/>

<h2 id="millionstone3"> millionstone3 </h2>

smooth.f90:

``` fortran
!#!-*-author:zhaoliang-*-
!#!-*-coding:utf8-*-
!#Date: 2014年 01月 26日 星期日 16:17:20 CST
PROGRAM Smoothing
      IMPLICIT NONE
      INTEGER,PARAMETER      :: MAX_SIZE = 20
      REAL,DIMENSION(1:MAX_SIZE)  ::  x, y
      INTEGER                     ::  Number
      INTEGER                     ::  i

      READ(*,"(I5)")  Number
      Read(*,"(5F10.0)") (x(i),i=1,Number)
      Do i = 1, Number -1
          y(i) = (x(i) + x(i+1) )/ 2.0
      END DO

      WRITE(*,"(A)") "              **************************"
      WRITE(*,"(A)") "              *  Data Smoothing Table***"
      WRITE(*,"(A)") "              **************************"
      WRITE(*,*)
      WRITE(*,"(4A)") (" "," No       x        y     ",i=1,2)
      WRITE(*,"(4A)") (" ","---     -------  ------  ",i=1,2)
      WRITE(*,10) 1,x(1),"NA",2,x(2),y(1)
      WRITE(*,11)  (i,x(i),y(i-1),i=3,Number)
10 format (T2,I3,F10.2,A10,I6,2F10.2)
11 format (1X,I3,2F10.2,I6,2F10.2)
!CHARACTER(LEN=50)   :: NA_Line    = "(T2,I3,F10.2,A10,I6,2F10.2)"
!CHARACTER(LEN=50)   :: Data_Line  = "(1X,I3,2F10.2,I6,2F10.2)"
!
!WRITE(*,NA_Line)    1, x(1), "NA", 2, x(2), y(1)
!WRITE(*,Data_Line)  (i, x(i), y(i-1), i = 3, Number)
END PROGRAM Smoothing

```


data:

```
17
13.5      17.23     9.0       20.9      23.0
16.3      15.9      21.5      14.78     18.5
16.54     13.67     10.78     9.5       13.2
19.6      20.0
```

run.sh:

```
#!/bin/sh
cat data|./a.out
```


<br/>
<br/>
<br/>
<hr/>
<br/>
<br/>
<br/>

<h2 id="millionstone4">millionstone4 </h2>

MeanVariance.f90:

``` fortran
!1#!-*-author:zhaoliang-*-
!1#!-*-coding:utf8-*-
!1#Date: 2014年 01月 26日 星期日 20:07:47 CST
! --------------------------------------------------------------------
! PROGRAM  MeanVariance:
!    This program reads in an array and computes the mean, variance
! and standard deviation of the data stored in the array.  Then, it
! displays an analysis table.  If a value is greater than the value
! of (mean + standard deviation), it displays a "good".  If a value
! is less than the value of (mean - standard deviation), it displays
! a "bad".
! --------------------------------------------------------------------
PROGRAM MeanVariance
      IMPLICIT NONE

      INTEGER, PARAMETER  :: MAX_SIZE = 50 !Maximum array size
      REAL ,DIMENSION(1:MAX_SIZE) :: Data  !input array
      REAL               :: Mean,Variance,StdDev! results
      INTEGER        ::  n       ! actual array size
      INTEGER        ::  i       ! running index
      CHARACTER(LEN = 40) :: TitleHeading = '(1X,A//1X,A/1X,A)'
      ! // 表示换行加上空行    /仅仅表示换行
      CHARACTER(LEN = 20) :: For_Data = '(I4,F7.2)'
      CHARACTER(LEN = 50) :: ResultFormat = '(/1X,A,F15.7/1X &
          ,A,F15.7/1X,A,F15.7/)'
      ! 前后的/ 就表示加了两个空行
      CHARACTER(LEN = 20) :: For_Analysis = '(I4,2F7.2,A)'

      READ(*,*) n     !read in input array
      READ(*,*) (Data(i), i = 1,n)
      WRITE(*,TitleHeading) "Input Data: " ,&
          " No.  Data",&
          "=== ======"
      WRITE(*,For_Data) (i,Data(i), i = 1,n)

      Mean = 0.0   ! compute Mean 
      DO i = 1, n
          Mean = Mean + Data(i)
      END DO
      Mean = Mean/ n
    
      Variance = 0.0
      Do i = 1, n
          Variance = Variance + (Data(i) - Mean) ** 2
      END DO
      Variance = Variance/(n-1)
      StdDev = SQRT(Variance)

      WRITE (*,ResultFormat)  "Mean                 :",Mean,&
          "Variance             :", Variance,&
          "Standard Deviation   :", StdDev 
      WRITE(*,TitleHeading) "Analysis Table:",   &
          "No.   Data    Dev ",&
          "===  =====  ====="

      DO i = 1,n
          IF(Data(i) > Mean + StdDev)  THEN
              WRITE(*,For_Analysis) i, Data(i),Data(i)-Mean," <----Good"
          ELSE IF (Data(i) < Mean - StdDev) THEN
              WRITE(*,For_Analysis) i, Data(i),Data(i)-Mean," <----Bad"
          ELSE
              WRITE(*,For_Analysis) i, Data(i),Data(i)-Mean
          END IF
      END DO
END PROGRAM MeanVariance

              
!$ ./run.sh 
! Input Data: 
!
!  No.  Data
! === ======    ------------------------>  titleheading
!   1   6.60
!   2   6.00
!   3   4.00
!   4   9.00
!   5   4.50
!   6   7.30
!   7   9.50
!   8   8.00
!   9   7.00
!  10   5.20  ------------------------------------->For_data
!
! Mean                 :      6.7100000
! Variance             :      3.3498890
! Standard Deviation   :      1.8302702
!             ------------------------------------->Resultformat
! Analysis Table:
!
! No.   Data    Dev 
! ===  =====  =====   ----------------------------->Titleheading
!   1   6.60  -0.11
!   2   6.00  -0.71
!   3   4.00  -2.71 <----Bad
!   4   9.00   2.29 <----Good
!   5   4.50  -2.21 <----Bad
!   6   7.30   0.59
!   7   9.50   2.79 <----Good
!   8   8.00   1.29
!   9   7.00   0.29
!  10   5.20  -1.51   ----------------------------->For_Analysis
```

data:
```
10
6.6  6.0  4.0  9.0
4.5  7.3  9.5  8.0
7.0  5.2
```


run.sh

``` sh
#!/bin/sh
cat data|./a.out
```

<br/>
<br/>
<br/>
<hr/>
<br/>
<br/>
<br/>

<h2 id="millionstone5"> millionstone5</h2>

BlankTest.f90

``` fortran
!#!-*-author:zhaoliang-*-
!#!-*-coding:utf8-*-
!#Date: 2014年 01月 26日 星期日 22:22:58 CST
PROGRAM BlankTest
      IMPLICIT NONE
      INTEGER            :: a,b
      REAL               :: x, y
      INTEGER            :: IO
      CHARACTER(LEN=60)  :: Format
      CHARACTER(LEN =5)  :: Input

      !Format = "(A5,BN,I5,BZ,T1,I5,BN,T1,F5.2,BZ,T1,F5.2)"
      Format = "(A5,BN,T1,I5,BZ,T1,I5,BN,T1,F5.2,BZ,T1,F5.2)"

      WRITE(*,"(1X,A)") "Input     BN     BZ     BN      BZ"
      WRITE(*,"(1X,A)") "-----     ---    ---    ---     ---"
      DO
          READ(*,Format,IOSTAT=IO)  Input ,a,b,x,y
          ! 你如何看到 怎么浏览数据  程序是怎么看数据的，踏实怎么看的？
          IF(IO < 0)  EXIT
          WRITE(*,"(1X,A,2I6,2F8.2)") Input,a,b,x,y
      END DO
END PROGRAM BlankTest

```

data:

```
1 3 5
 2 4
6 8 9
112 
 23 4
5 678
```
run.sh:
``` sh
#!/bin/sh
cat data|./a.out
```


<br/>
<br/>
<br/>
<hr/>
<br/>
<br/>
<br/>

<h2 id="millionstone6"> millionstone6 </h2>

Class marks average:

``` fortran
!#!-*-author:zhaoliang-*-
!#!-*-coding:utf8-*-
!#Date: 2014年 01月 27日 星期一 00:18:09 CST

PROGRAM ClassA
      IMPLICIT NONE
      CHARACTER(LEN = 25) :: Name, ClassAverage
      CHARACTER(LEN = 30) :: FormatIn, FormatOut, FormatAvg
      INTEGER             :: Number, i
      INTEGER             :: Score1,Score2,Score3
      REAL                :: Average
      REAL                :: Score1Avg,Score2Avg,Score3Avg,ClassAvg

      FormatIn = "(A25,3I5)"
      FormatOut = " (A,A25,3I8,3x,F9.2)"
      FormatAvg = " (A,A25,3F8.2, F9.2)"
!      WRITE(*,"(2A)") " ","Name                           Score1    Score2&
!            Score3   Average"
!      WRITE(*,"(2A)") " ","=========================      ======    ======&
!            ======   ======="

      WRITE(*,"(2A)") " ","Name                        Score1    Score2   Score3   Average"
      WRITE(*,"(2A)") " ","=========================   ======    ======   ======   ======="
      ClassAverage = "Average"
      Score1Avg = 0.0
      Score2Avg = 0.0
      Score3Avg = 0.0
      READ(*,*) Number
      Do i = 1,Number
          READ(*,FormatIn) Name,Score1,Score2,Score3
          Average = REAL(Score1+Score2+Score3) /3.0
          WRITE(*,FormatOut) " ",Name,Score1,Score2,Score3,Average
          Score1Avg = Score1Avg + Score1
          Score2Avg = Score2Avg + Score2
          Score3Avg = Score3Avg + Score3
      END DO
      Score1Avg = Score1Avg / Number
      Score2Avg = Score2Avg / Number
      Score3Avg = Score3Avg / Number
      ClassAvg = (Score1Avg + Score2Avg + Score3Avg) /3.0

      Write(*,"(2A)") " ", "===========================  ====== ======  ======   ========"
      WRITE(*,FormatAvg) " ",ClassAverage,Score1Avg,Score2Avg,&
          Score3Avg,ClassAvg
END PROGRAM ClassA
```

classdata:
```
10
Jon Rokne                100  95   98
Mukesh Prasad            78   89   82
Jack Morrison            94   98   95
Ken Musgrave             100  100  96
Douglas Voorhies         87   95   89
Dale Schumacher          76   85   73
John Schlag              56   62   50
James Hall               60   78   84
Alan Lindgren            85   77   86
Robert Bogart            94   100  86
```


run.sh
```
#!/bin/sh
cat classdata|./a.out
```


<br/>
<br/>
<br/>
<hr/>
<br/>
<br/>
<br/>

<h2 id="millionstone7"> millionstone Combinator <h2>

computerFactorial.f90:

``` fortran
!#!-*-author:zhaoliang-*-
!#!-*-coding:utf8-*-
!#Date: 2014年 01月 27日 星期一 12:17:06 CST

! --------------------------------------------------------------------
! PROGRAM  ComputeFactorial:
!    This program uses MODULE FactorialModule for computing factorial
! and combinatorial coefficients.
! --------------------------------------------------------------------

PROGRAM  ComputeFactorial
   USE       FactorialModule            ! use a module

   IMPLICIT  NONE

   INTEGER :: N, R

   WRITE(*,*)  'Two non-negative integers --> '
   READ(*,*)   N, R

   WRITE(*,*)  N,   '! = ', Factorial(N)
   WRITE(*,*)  R,   '! = ', Factorial(R)

   IF (R <= N) THEN                     ! if r <= n, do C(n,r)
      WRITE(*,*)  'C(', N, ',', R, ') = ', Combinatorial(N, R)
   ELSE                                 ! otherwise, do C(r,n)
      WRITE(*,*)  'C(', R, ',', N, ') = ', Combinatorial(R, N)
   END IF

END PROGRAM  ComputeFactorial
```

Factorial.f90:

``` fortran
! --------------------------------------------------------------------
! module  factorialmodule
!    this module contains two procedures: factorial(n) and
! combinatorial(n,r).  the first computes the factorial of an integer
! n and the second computes the combinatorial coefficient of two
! integers n and r.
! --------------------------------------------------------------------

module  factorialmodule
   implicit  none

contains

! --------------------------------------------------------------------
! function  factorial() :
!    this function accepts a non-negative integers and returns its
! factorial.
! --------------------------------------------------------------------

   integer function  factorial(n)
      implicit  none

      integer, intent(in) :: n          ! the argument
      integer             :: fact, i    ! result

      fact = 1                          ! initially, n!=1
      do i = 1, n                       ! this loop multiplies
         fact = fact * i                ! i to n!
      end do
      factorial = fact

   end function  factorial

! --------------------------------------------------------------------
! function  combinarotial():
!    this function computes the combinatorial coefficient c(n,r).
! if 0 <= r <= n, this function returns c(n,r), which is computed as
! c(n,r) = n!/(r!*(n-r)!).  otherwise, it returns 0, indicating an
! error has occurred.
! --------------------------------------------------------------------

   integer function  combinatorial(n, r)
      implicit  none

      integer, intent(in) :: n, r
      integer             :: cnr

      if (0 <= r .and. r <= n) then     ! valid arguments ?
         cnr = factorial(n) / (factorial(r)*factorial(n-r))
      else                              ! no,
         cnr = 0                        ! zero is returned
      end if
      combinatorial = cnr

   end function  combinatorial

end module  factorialmodule
```


Makefile

``` makefile
FC = gfortran
FFLAGS = -Wall -O2
Compute : computerFactorial.o Factorial.o
	$(FC) $(FFLAGS) $^ -o $@
computerFactorial.o : computerFactorial.f90
	$(FC) -c $(FFLAGS) $<
Factorial.o : Factorial.f90
	$(FC) -c $(FFLAGS) $<

```


<br/>
<br/>
<br/>
<hr/>
<br/>
<br/>
<br/>

<h2 id="millionstone8">Degree Radian Conversion </h2>

Main.f90

``` fortran
!#!-*-author:zhaoliang-*-
!#!-*-coding:utf8-*-
!#Date: 2014年 01月 28日 星期二 17:06:46 CST
!
! -----------------------------------------------------------------------
! PROGRAM  TrigonFunctTest:
!    This program tests the functions in module MyTrigonometricFunctions.
! Module MyTrigonometricFunctions is stored in file trigon.f90.
! Functions in that module use degree rather than radian.  This program
! displays the sin(x) and cos(x) values for x=-180, -170, ..., 0, 10, 20,
! 30, ..., 160, 170 and 180.  Note that the sin() and cos() function
! in module MyTrigonometricFunctions are named MySIN(x) and MyCOS(x).
! -----------------------------------------------------------------------
PROGRAM TrigonFunctTest
    USE MyTrigonometricFunctions   ! use a module

    IMPLICIT NONE

    REAL   :: Begin = -180.0   ! initial value
    REAL   :: Final = 180.0    ! final  value
    REAL   :: Step  = 10.0     ! step size

    REAL   :: x

    WRITE (*,*) 'value of PI', PI
    WRITE(*,*) 
    x = Begin
    DO 
        IF(X > Final) EXIT  
        WRITE(*,*) ' x = ', x, 'Deg Sin(x) = ', MySIN(x), ' Cos(x) =',MyCOS(x)
        x = x+Step
    END DO
END PROGRAM TrigonFunctTest
```

module.f90:

``` fortran
!#!-*-author:zhaoliang-*-
!#!-*-coding:utf8-*-
!#Date: 2014年 01月 28日 星期二 16:58:55 CST

! --------------------------------------------------------------------
! MODULE  MyTrigonometricFunctions:
!    This module provides the following functions and constants
!    (1) RadianToDegree()     - converts its argument in radian to
!                               degree
!    (2) DegreeToRadian()     - converts its argument in degree to
!                               radian
!    (3) MySIN()              - compute the sine of its argument in
!                               degree
!    (4) MyCOS()              - compute the cosine of its argument
!                               in degree
! --------------------------------------------------------------------
MODULE MyTrigonometricFunctions
      IMPLICIT NONE

      REAL,PARAMETER   :: PI     =   3.1415926  !some constants
      REAL,PARAMETER   :: Degree180     =   180.0  !some constants
      REAL,PARAMETER   :: R_to_D     =   Degree180/PI  !some constants
      REAL,PARAMETER   :: D_to_R     =   PI/Degree180  !some constants

CONTAINS
! --------------------------------------------------------------------
! FUNCTION  RadianToDegree():
!    This function takes a REAL argument in radian and converts it to
! the equivalent degree.
! --------------------------------------------------------------------
      REAL FUNCTION RadianToDegree(Radian)
          IMPLICIT NONE
          REAL,INTENT(IN)  :: Radian

          RadianToDegree = Radian * R_to_D
      END FUNCTION RadianToDegree

! --------------------------------------------------------------------
! FUNCTION  DegreeToRadian():
!    This function takes a REAL argument in degree and converts it to
! the equivalent radian.
! --------------------------------------------------------------------
      REAL FUNCTION DegreeToRadian(Degree180)
          IMPLICIT NONE
          REAL,INTENT(IN)  :: Degree180

          DegreeToRadian = Degree180 * D_to_R
      END FUNCTION DegreeToRadian


! --------------------------------------------------------------------
! FUNCTION  MySIN():
!    This function takes a REAL argument in degree and computes its
! sine value.  It does the computation by converting its argument to
! radian and uses Fortran's sin().
! --------------------------------------------------------------------
      REAL FUNCTION MySIN(x)
          IMPLICIT NONE
          REAL,INTENT(IN)  ::  x

          MySIN = SIN(DegreeToRadian(x))
          !  从这边我们得知   SIN COS 需要的是弧度的输入
      END FUNCTION MySIN

! --------------------------------------------------------------------
! FUNCTION  MyCos():
!    This function takes a REAL argument in degree and computes its
! cosine value.  It does the computation by converting its argument to
! radian and uses Fortran's cos().
! --------------------------------------------------------------------
      REAL FUNCTION MyCOS(x)
          IMPLICIT nONE
          REAL,INTENT(IN) :: x

          MyCOS = COS(DegreeToRadian(x))
      END FUNCTION MyCOS
END MODULE  MyTrigonometricFunctions

```


makefile:

``` makefile
FC = gfortran
FFLAGS = -Wall -g 

SolveSinCos : Main.o module.f90
	$(FC) -o $@  $(FFLAGS) $^
Main.o : Main.f90
	$(FC) -o $@ $(FFLAGS) -c $<  # -c 不可缺少
#Module.o : Module.f90
#	$(FC) -o $@ $(FFLAGS) $<

```


Some public and private control in the module:

``` fortran
!#!-*-author:zhaoliang-*-
!#!-*-coding:utf8-*-
!#Date: 2014年 01月 28日 星期二 17:28:03 CST
! --------------------------------------------------------------------
! MODULE  MyTrigonometricFunctions:
!    This module provides the following functions and constants
!    (1) RadianToDegree()     - converts its argument in radian to
!                               degree
!    (2) DegreeToRadian()     - converts its argument in degree to
!                               radian
!    (3) MySIN()              - compute the sine of its argument in
!                               degree
!    (4) MyCOS()              - compute the cosine of its argument
!                               in degree
! --------------------------------------------------------------------

MODULE  MyTrigonometricFunctions
   IMPLICIT   NONE

   REAL, PARAMETER :: PI        = 3.1415926       ! some constants
   REAL, PARAMETER :: Degree180 = 180.0
   REAL, PARAMETER :: R_to_D    = Degree180/PI
   REAL, PARAMETER :: D_to_R    = PI/Degree180

   PRIVATE         :: Degree180, R_to_D, D_to_R
   PRIVATE         :: RadianToDegree, DegreeToRadian
   PUBLIC          :: MySIN, MyCOS

CONTAINS

! --------------------------------------------------------------------
! FUNCTION  RadianToDegree():
!    This function takes a REAL argument in radian and converts it to
! the equivalent degree.
! --------------------------------------------------------------------

   REAL FUNCTION  RadianToDegree(Radian)
      IMPLICIT  NONE
      REAL, INTENT(IN) :: Radian

      RadianToDegree = Radian * R_to_D
   END FUNCTION  RadianToDegree

! --------------------------------------------------------------------
! FUNCTION  DegreeToRadian():
!    This function takes a REAL argument in degree and converts it to
! the equivalent radian.
! --------------------------------------------------------------------

   REAL FUNCTION  DegreeToRadian(Degree)
      IMPLICIT  NONE
      REAL, INTENT(IN) :: Degree

      DegreeToRadian = Degree * D_to_R
   END FUNCTION  DegreeToRadian

! --------------------------------------------------------------------
! FUNCTION  MySIN():
!    This function takes a REAL argument in degree and computes its
! sine value.  It does the computation by converting its argument to
! radian and uses Fortran's sin().
! --------------------------------------------------------------------

   REAL FUNCTION  MySIN(x)
      IMPLICIT  NONE
      REAL, INTENT(IN) :: x

      MySIN = SIN(DegreeToRadian(x))
   END FUNCTION  MySIN

! --------------------------------------------------------------------
! FUNCTION  MySIN():
!    This function takes a REAL argument in degree and computes its
! cosine value.  It does the computation by converting its argument to
! radian and uses Fortran's cos().
! --------------------------------------------------------------------

   REAL FUNCTION  MyCOS(x)
      IMPLICIT  NONE
      REAL, INTENT(IN) :: x

      MyCOS = COS(DegreeToRadian(x))
   END FUNCTION  MyCOS

END MODULE  MyTrigonometricFunctions
```


<br/>
<br/>
<br/>
<hr/>
<br/>
<br/>
<br/>

<h2 id="millionstone9"> Histogram </h2>


histogram.f90:

``` fortran
!#!-*-author:zhaoliang-*-
!#!-*-coding:utf8-*-
!#Date: 2014年 01月 26日 星期日 21:13:43 CST
PROGRAM VertialBarChart
      IMPLICIT NONE
      CHARACTER(LEN = *),PARAMETER :: Part1 = "(1X, I5, A,"
      CHARACTER(LEN = *),PARAMETER :: Part2 = "A, A,I2, A)"
      CHARACTER(LEN = 2)           :: Repetition
      CHARACTER(LEN = 10),PARAMETER:: InputFormat= "(I5/(5I5))"
      INTEGER                      :: Number,i , j
      INTEGER,DIMENSION(1:100)     :: Data

      Read(*,InputFormat) Number,(Data(i),i=1,Number)
      DO i = 1,Number
          IF(Data(i) /= 0) THEN
              WRITE(Repetition,"(I2)") Data(i)
              WRITE(*,Part1//Repetition//Part2) Data(i),"|",&
                  ("*",j=1,Data(i))," (",Data(i),")"
                  !("*",i=1,Data(i))," (",Data(i),")"  --->error
          ELSE
              WRITE(*,"(1X,I5,A,I2,A)") Data(i),"| (", Data(i),")"
          END IF
      END DO
END PROGRAM VertialBarChart

```

data:

```
22
8    23   27   24   40
45   43   38   26   16
8    8    6    3    5
4    0    2    0    1
2    1
```


run.sh

``` sh
#!/bin/sh
cat data|./a.out

```


<br/>
<br/>
<br/>
<hr/>
<br/>
<br/>
<br/>

<h2 id="millionstone10">NewTon Algorithm</h2>

```
!#!-*-author:zhaoliang-*-
!#!-*-coding:utf8-*-
!#Date: 2014年 01月 27日 星期一 11:20:35 CST
PROGRAM SquareRoot
      IMPLICIT NONE

      REAL :: Input, X, NewX, Tolerance
      INTEGER  :: Count

      WRITE(*,*) 'Please Enter the initail value and Tolerance'
      READ(*,*)  Input,Tolerance

      Count = 0     !count starts with 0
      X     = Input  ! X starts with the input value
      Do 
          Count =Count +1   ! for each interation
          NewX  = 0.5* (X + Input/X) ! comput a nuew approximation
          IF(ABS(X-NewX) < Tolerance)  EXIT
          !if they are very close ,exit
          X = NewX
      END DO
       WRITE(*,*)  'After ', Count, ' iterations:'
       WRITE(*,*)  '  The estimated square root is ', NewX
       WRITE(*,*)  '  The square root from SQRT() is ', SQRT(Input)
       WRITE(*,*)  '  Absolute error = ', ABS(SQRT(Input) - NewX)

END PROGRAM  SquareRoot

!4 
!0.00004
! After            5  iterations:
!   The estimated square root is    2.00000000    
!   The square root from SQRT() is    2.00000000    
!   Absolute error =    0.00000000
```

NewtonAdvance.f90:

``` fortran
!#!-*-author:zhaoliang-*-
!#!-*-coding:utf8-*-
!#Date: 2014年 01月 27日 星期一 11:27:11 CST
! ---------------------------------------------------------------
! This program contains a function MySqrt() that uses Newton's
! method to find the square root of a positive number.  This is
! an iterative method and the program keeps generating better
! approximation of the square root until two successive
! approximations have a distance less than the specified tolerance.
! ---------------------------------------------------------------
PROGRAM  SquareRoot
      IMPLICIT NONE

      REAL :: Begin,End1 ,Step
      REAL :: x ,SQRTx, MySQRTx,Error
      WRITE(*,*) 'Different Initial Condition from begin to end'
      WRITE(*,*) "Please Enter Begin ,End1,Step"
   READ(*,*)  Begin, End1, Step          ! read in init, final and step
   x = Begin                            ! x starts with the init value
   DO
      IF (x > End1)  EXIT                ! exit if x > the final value
      SQRTx   = SQRT(x)                 ! find square root with SQRT()
      MySQRTx = MySqrt(x)               ! do the same with my sqrt()
      Error   = ABS(SQRTx - MySQRTx)    ! compute the absolute error
      WRITE(*,*)  x, SQRTx, MySQRTx, Error   ! display the results
      x = x + Step                      ! move on to the next value
   END DO

CONTAINS

! ---------------------------------------------------------------
! REAL FUNCTION  MySqrt()
!    This function uses Newton's method to compute an approximate
! of a positive number.  If the input value is zero, then zero is
! returned immediately.  For convenience, the absolute value of
! the input is used rather than kill the program when the input
! is negative.
! ---------------------------------------------------------------

   REAL FUNCTION  MySqrt(Input)
      IMPLICIT  NONE
      REAL, INTENT(IN) :: Input
      REAL             :: X, NewX
      REAL, PARAMETER  :: Tolerance = 0.00001

      IF (Input == 0.0) THEN            ! if the input is zero
         MySqrt = 0.0                   !    returns zero
      ELSE                              ! otherwise,
         X = ABS(Input)                 !    use absolute value
         DO                             !    for each iteration
            NewX  = 0.5*(X + Input/X)   !       compute a new approximation
            IF (ABS(X - NewX) < Tolerance)  EXIT  ! if very close, exit
            X = NewX                    !       otherwise, keep the new one
         END DO
         MySqrt = NewX
      END IF
   END FUNCTION  MySqrt

END PROGRAM  SquareRoot
```

<br/>
<br/>
<br/>
<hr/>
<br/>
<br/>
<br/>

<h2 id="millionstone11"> HeronFomula for triangle surface</h2>

HeronFomula.f90
``` fortran
!#!-*-author:zhaoliang-*-
!#!-*-coding:utf8-*-
!#Date: 2014年 01月 27日 星期一 11:00:45 CST
! --------------------------------------------------------------------
!    This program uses Heron's formula to compute the area of a
! triangle.  It "contains" the following functions;
!    (1)  LOGICAL function TriangleTest() -
!         this function has three real formal arguments and tests
!         to see if they can form a triangle.  If they do form a
!         triangle, this function returns .TRUE.; otherwise, it
!         returns .FALSE.
!    (2)  REAL function TriangleArea() -
!         this functions has three real formal arguments considered
!         as three sides of a triangle and returns the area of this
!         triangle.  ! --------------------------------------------------------------------

PROGRAM HeronFomula
      IMPLICIT NONE

      REAL :: a, b, c , TriangleArea
      DO 
          WRITE(*,*) 'Three sides of a Triangle ----> a, b, c'
          READ(*,*) a,b,c
          WRITE(*,*) 'Input sides are', a, b, c
          IF(TriangleTest(a,b,c)) EXIT   ! Exit if the shape is triangle
          WRITE(*,*) 'Your input cannot form a Triangle'

      END DO
      TriangleArea = Area(a, b,c)
      WRITE(*,*) 'Triangle Area is ',TriangleArea
CONTAINS
! --------------------------------------------------------------------
! LOGICAL FUNCTION  TriangleTest() :
!    This function receives three REAL numbers and tests if they form
! a triangle by testing:
!    (1)  all arguments must be positive, and
!    (2)  the sum of any two is greater than the third
! If the arguments form a triangle, this function returns .TRUE.;
! otherwise, it returns .FALSE.
! --------------------------------------------------------------------
      LOGICAL FUNCTION TriangleTest(a, b ,c)
          IMPLICIT NONE
          
          REAL,INTENT(IN)   :: a, b, c
          LOGICAL           :: test1,test2

          test1 = (a > 0.0) .AND. (b > 0.0) .AND. (c > 0.0)
          test2 = (a + b > c) .AND. (a + c > b) .AND. (b + c > a) 
          TriangleTest = test1 .AND. test2   ! both must be true
      END FUNCTION TriangleTest
! --------------------------------------------------------------------
! REAL FUNCTION  Area() :
!    This function takes three real number that form a triangle, and
! computes and returns the area of this triangle using Heron's formula.
! --------------------------------------------------------------------
      REAL FUNCTION Area(a, b ,c)
          IMPLICIT NONE
          
          REAL,INTENT(IN)   :: a, b, c

          REAL               :: s

          s = (a+b+c)/2.0
          Area = SQRT(s*(s-a)*(s-b)*(s-c))
      END FUNCTION Area

END PROGRAM HeronFomula
```


<br/>
<br/>
<br/>
<hr/>
<br/>
<br/>
<br/>


<h2 id="millionstone12">Multiple Table</h2>
``` fortran

!#!-*-author:zhaoliang-*-
!#!-*-coding:utf8-*-
!#Date: 2014年 01月 26日 星期日 14:21:17 CST
!

PROGRAM Multiplication_Table
      IMPLICIT NONE
      INTEGER ,PARAMETER :: MAX = 9
      INTEGER            :: i,j
      CHARACTER(LEN =80) :: FORMAT

      FORMAT = "(9(2X,I1,A,I1,A,I2))"
      Do i = 1, MAX
          WRITE(*,FORMAT) (i, '*',j,'=',i*j,j=1,MAX)
      END DO
END PROGRAM Multiplication_Table
```

<br/>
<br/>
<br/>
<hr/>
<br/>
<br/>
<br/>


<h2 id="millionstone13">Prime Range </h2>


``` fortran
!#!-*-author:zhaoliang-*-
!#!-*-coding:utf8-*-
!#Date: 2014年 01月 29日 星期三 12:12:46 CST
! --------------------------------------------------------------------
! This program finds all prime numbers in the range of 2 and an
! input integer.
! --------------------------------------------------------------------

PROGRAM PrimesRange
      IMPLICIT NONE

      INTEGER :: Range ,Number, Count

      Range =GetNumber()

      count =1          !input is correct .start counting
      WRITE(*,*)
      WRITE(*,*) 'Prime number #',Count,': ',2
    DO Number = 3, Range, 2              ! try all odd numbers 3, 5, 7, ...
       IF (Prime(Number)) THEN
           Count = Count + 1              ! yes, this Number is a prime
           WRITE(*,*)  'Prime number #', Count, ': ', Number
       END IF
    END DO

   WRITE(*,*)
   WRITE(*,*)  'There are ', Count, ' primes in the range of 2 and ', Range

CONTAINS

! --------------------------------------------------------------------
! INTEGER FUNCTION  GetNumber()
!    This function does not require any formal argument.  It keeps
! asking the reader for an integer until the input value is greater
! than or equal to 2.
! --------------------------------------------------------------------
   INTEGER FUNCTION  GetNumber()
      IMPLICIT  NONE

      INTEGER :: Input

      WRITE(*,*)  'What is the range ? '
      DO                                ! keep trying to read a good input
         READ(*,*)  Input               ! ask for an input integer
         IF (Input >= 2)  EXIT          ! if it is GOOD, exit
         WRITE(*,*)  'The range value must be >= 2.  Your input = ', Input
         WRITE(*,*)  'Please try again:'     ! otherwise, bug the user
      END DO
      GetNumber = Input
   END FUNCTION  GetNumber

! --------------------------------------------------------------------
! LOGICAL FUNCTION  Prime()
!    This function receives an INTEGER formal argument Number.  If it
! is a prime number, .TRUE. is returned; otherwise, this function
! returns .FALSE.
! --------------------------------------------------------------------
      LOGICAL FUNCTION Prime(Number)
          IMPLICIT nONE

          INTEger,INTENT(IN)  :: Number
          INTEGER             :: Divisor

          IF(Number < 2) THEN
              Prime = .False.
          ELSE IF(Number == 2) THEN
              Prime = .TRUE.
          ELSE IF(MOD(Number,2) == 0) THEN
              Prime = .False.
          ELSE
              Divisor = 3
              DO 
                  IF(Divisor*Divisor>Number .OR. MOD(Number,Divisor)==0) EXIT
                  !divisor*divisor is much better than sqrt(number)
                  Divisor = Divisor + 2
              END DO
              Prime = Divisor* Divisor > Number
        END IF
      END FUNCTION Prime
END PROGRAM PrimesRange
                  

```


<br/>
<br/>
<br/>
<hr/>
<br/>
<br/>
<br/>


<h2 id="millionstone14"> Velocity Projection</h2>


Projection Method.f90

```
!#!-*-author:zhaoliang-*-
!#!-*-coding:utf8-*-
!#Date: 2014年 01月 25日 星期六 10:44:45 CST
! --------------------------------------------------------------------
! Given t, the time since launch, u, the launch velocity, a, the
! initial angle of launch (in degree), and g, the acceleration due to
! gravity, this program computes the position (x and y coordinates)
! and the velocity (magnitude and direction) of a projectile.
! --------------------------------------------------------------------
PROGRAM Projectile
      implicit none

      REAL,PARAMETER :: g =9.8  ! acceleration due to gravity
      REAL,PARAMETER :: PI = 3.1415926 

      REAL :: Angle !lanch angle in degree
      REAL :: Time  ! time to flight
      REAL :: Theta ! direction at time in degree
      REAL :: U     ! launch velocity
      REAL :: V     ! resultant velocity
      REAL :: Vx    ! horizontal velocity
      REAL :: Vy    ! vertical  velocity
      REAL :: X     ! horizontal displacement
      REAL :: Y     ! vertical displacement

      WRITE(*,*)  "Please Input Angle,Time,U"
      READ(*,*) Angle,Time,U

      Angle = Angle * PI / 180.0   ! convert to radian

      X = U * COS(Angle) * Time
      Y = U * SIN(Angle) * Time - g * Time * Time /2.0
      Vx = U * COS(Angle)
      Vy = U * SIN(Angle) - g * Time
      V = SQRT(Vx*Vx + Vy*Vy)
      Theta = ATAN(Vy/Vx) * 180.0 /PI


      WRITE (*,*) 'Horizontal  displacement: ',X 
      WRITE (*,*) 'Vertical  displacement: ',Y 
      WRITE (*,*) 'Resultant  velocity: ',V
      WRITE (*,*) 'Direction (in degree): ',Theta

END PROGRAM Projectile

!$ ./a.out 
 !Please Input Angle,Time,U
!45 6 60
 !Horizontal  displacement:    254.558456    
 !Vertical  displacement:    78.1584320    
 !Resultant  velocity:    45.4763107    
 !Direction (in degree):   -21.1030636    
! 
 !Discussion

 !   The program uses Angle for the angle a, Time for t, and U for u. The READ statement reads the input.
 !   The first assignment statement converts the angle in degree to radian. This is necessary since all intrinsic trigonometric functions use radian rather than degree.
 !   Variables X and Y, which are computed in the second and third assignments, hold the displacements.
 !   The next two assignments compute the components of the velocity vector.
 !   The velocity itself is computed in the sixth assignment.
 !   Finally, the angle with ground, Theta, is computed with the last assignment. Note that ithe result is converted back to degree, since ATAN(x) returns the arc tangent value of x in radian.
```


<br/>
<br/>
<br/>
<hr/>
<br/>
<br/>
<br/>


<h2 id="millionstone15"> quadratic equation solution </h2>

Solve.f90:

``` fortran
!#!-*-author:zhaoliang-*-
!#!-*-coding:utf8-*-
!#Date: 2014年 01月 25日 星期六 16:41:44 CST
! ---------------------------------------------------
!   Solve  Ax^2 + Bx + C = 0 given B*B-4*A*C >= 0
!   Now, we are able to detect the following:
!    (1) unsolvable equation
!    (2) linear equation
!    (3) quadratic equation
!        (a) distinct real roots
!        (b) repeated root
!        (c) no real roots
! ---------------------------------------------------
PROGRAM QuadraticEquation
      IMPLICIT NONE

      REAL :: a, b, c
      REAL :: d
      REAL :: root1 , root2

      !read  in the coefficients a,b and c

      WRITE(*,*) 'Please Enter a,b,c : '
      READ(*,*) a, b, c
      WRITE(*,*) 'a= ', a
      WRITE(*,*) 'b= ', b
      WRITE(*,*) 'c= ', c
      WRITE(*,*)

      IF(a == 0.0) THEN    ! could be a linear equation
          IF(b == 0.0) THEN ! the input becomes c =0
              IF(c == 0) THEN ! all numbers are roots
                  WRITE(*,*) 'ALL numbers are roots'
              ELSE
                  WRITE(*,*) 'Unsolvable equation'
              END IF
          ELSE
              WRITE(*,*) 'This is linear equation, root = ',&
                  -c/b
          END IF
      ELSE                    ! ok we have a quadraticequation
          d = b*b -4.0*a*c  
          IF(d > 0.0) THEN
              d = SQRT(d)
              root1 = (-b + d)/(2.0*a) !first root
              root2 = (-b - d)/(2.0*a) !second root
              WRITE(*,*) 'Roots  are',root1,' and ',root2
          ELSE IF(d == 0) THEN ! repeated roots!
              WRITE(*,*) 'The repeated root is ' , -b/(2.0*a)
          ELSE         ! complex roots
              WRiTe(*,*) 'There is no real roots!'
              WRITE(*,*) 'Discriminant = ',d
          END IF
      END IF
END PROGRAM QuadraticEquation

! Please Enter a,b,c : 
!0 2 1
! a=    0.00000000    
! b=    2.00000000    
! c=    1.00000000    
!
! This is linear equation, root =  -0.500000000 


!$ ./a.out 
! Please Enter a,b,c : 
!3 2 3
! a=    3.00000000    
! b=    2.00000000    
! c=    3.00000000    
!
! There is no real roots!
! Discriminant =   -32.0000000    
!
!root at yezhaoliang-N53SM in /home/happycamp-of-fortran/sheneTurtorilForFortran90
!$ ./a.out 
! Please Enter a,b,c : 
!0 0 0
! a=    0.00000000    
! b=    0.00000000    
! c=    0.00000000    
!
! ALL numbers are roots
!
!root at yezhaoliang-N53SM in /home/happycamp-of-fortran/sheneTurtorilForFortran90
!$ ./a.out 
! Please Enter a,b,c : 
!0 0 3
! a=    0.00000000    
! b=    0.00000000    
! c=    3.00000000    
!
! Unsolvable equation
```

<br/>
<br/>
<br/>
<hr/>
<br/>
<br/>
<br/>


<h2 id="millionstone16">Test Switch Case</h2>


``` fortran
!#!-*-author:zhaoliang-*-
!#!-*-coding:utf8-*-
!#Date: 2014年 01月 25日 星期六 17:18:17 CST
!! ------------------------------------------------------------
! This program reads in a single character and determines if
! it is a vowel, a consonant, a digit, one of the four
! arithmetic operators (+, -, * and /), a space, or something
! else.  You can do it with IF-THEN-ELSE-END IF statement; but
! SELECT CASE statement provides a cleaner solution.
!
! For character input, you could use the quote characters like
!         'G'
! Or, just type the character.  In this case, the first
! character you type will be read.
! ------------------------------------------------------------

PROGRAM CharacterTesting
      IMPLICIT NONE

      CHARACTER(LEN =1) :: INPUT

      WRITE (*,*) 'Please ENter a CHARACTER'
      READ(*,*) INPUT

      SELECT CASE (Input)
      CASE ('A' : 'Z', 'a' : 'z')       ! rule out letters
         WRITE(*,*)  'A letter is found : "', Input, '"'
         SELECT CASE (Input)            ! a vowel ?
            CASE ('A', 'E', 'I', 'O', 'U', 'a', 'e', 'i', 'o','u')
               WRITE(*,*)  'It is a vowel'
            CASE DEFAULT                ! it must be a consonant
               WRITE(*,*)  'It is a consonant'
         END SELECT
      CASE ('0' : '9')                  ! a digit   用冒号表示连续的数字
         WRITE(*,*)  'A digit is found : "', Input, '"'
      CASE ('+', '-', '*', '/')         ! an operator    !
      !    用逗号表示独立的几个状态
         WRITE(*,*)  'An operator is found : "', Input, '"'
      CASE (' ')                        ! space
         WRITE(*,*)  'A space is found : "', Input, '"'
      CASE DEFAULT                      ! something else
         WRITE(*,*)  'Something else found : "', Input, '"'
   END SELECT

END PROGRAM  CharacterTesting
```


<br/>
<br/>
<br/>
<hr/>
<br/>
<br/>
<br/>


<h2 id="millionstone17">Two Functions</h2>

twoFunctions.f90
``` fortran
!#!-*-author:zhaoliang-*-
!#!-*-coding:utf8-*-
!#Date: 2014年 01月 27日 星期一 10:25:53 CST
PROGRAM TWOFUNCTION
      IMPLICIT NONE
      INTEGER :: a,b,BiggerOne

      real :: GeoMetricMean

      WRITE(*,*) 'Please Enter two variables : a, b'
      read(*,*) a,b
      BiggerOne = Large(a,b)
      GeoMetricMean = GeoMean(a,b)
      WRITE(*,*) 'Input = ', a, b
      WRITE(*,*) 'Large one = ',BiggerOne 
      WRITE(*,*) 'GeoMetric Mean = ',GeoMetricMean 

CONTAINS
      INTEGER FUNCTION Large(a,b)
          IMPLICIT NONE
          INTEGER,INTENT(IN) :: a, b
          IF(a >= b) THEN
              Large = a
          ELSE
              Large = b
          END IF
      END FUNCTION Large

      REAL FUNCTION GeoMean(a,b)
          IMPLICIT NONE
          INTEGER,INTENT(IN)  :: a,b
          GeoMean = SQRT(REAL(a*b))
      END FUNCTION GeoMean
END PROGRAM TWOFUNCTION



```


<br/>
<br/>
<br/>
<hr/>
<br/>
<br/>
<br/>


<h2 id="millionstone18">YYYYMMDDConversion</h2>

YYYYMMDDCONVERSION.f90:

``` fortran
!#!-*-author:zhaoliang-*-
!#!-*-coding:utf8-*-
!#Date: 2014年 01月 28日 星期二 17:35:15 CST
! --------------------------------------------------------------------
! PROGRAM  YYYYMMDDConversion:
!    This program uses an external subroutine Conversion() to convert
! an integer value in the form of YYYYMMDD to Year, Month and Day.
! --------------------------------------------------------------------

PROGRAM  YYYYMMDDConversion
   IMPLICIT  NONE

   INTERFACE                            ! interface block
      SUBROUTINE  Conversion(Number, Year, Month, Day)
         INTEGER, INTENT(IN)  :: Number
         INTEGER, INTENT(OUT) :: Year, Month, Day
      END SUBROUTINE  Conversion

      LOGICAL FUNCTION Test(Number)
          IMPLICIT NONE
          INTEGER, INTENT(IN)  :: Number
      END FUNCTION Test

   END INTERFACE

   INTEGER :: YYYYMMDD, Y, M, D

   DO                                   ! loop until a zero is seen
      WRITE(*,*)  "A YYYYMMDD (e.g., 19971027) please (0 to stop) -> "
      READ(*,*)   YYYYMMDD              ! read in the value
      IF (YYYYMMDD == 0)  EXIT          ! if 0, then bail out
      IF(.NOT. Test(YYYYMMDD)) THEN
          WRITE(*,*) 'Enter Error,REPEAT!'
          CYCLE
      END IF
! 还有一个缺陷所在  没有实现字符串接口，如果输入EXIT怎么获取！
! 这是一个问题！！
      CALL  Conversion(YYYYMMDD, Y, M, D)    ! do conversation

      WRITE(*,*)  "Year  = ", Y         ! display results

      ! 缺陷  无法判断输入的是否正确，可以加上一个判断的子程序
      WRITE(*,*)  "Month = ", M
      WRITE(*,*)  "Day   = ", D
      WRITE(*,*)
   END DO
END PROGRAM  YYYYMMDDConversion

! --------------------------------------------------------------------
! SUBROUTINE  Conversion():
!    This external subroutine takes an integer input Number in the
! form of YYYYMMDD and convert it to Year, Month and Day.
! --------------------------------------------------------------------

SUBROUTINE  Conversion(Number, Year, Month, Day)
   IMPLICIT  NONE

   INTEGER, INTENT(IN)  :: Number
   INTEGER, INTENT(OUT) :: Year, Month, Day

   Year  = Number / 10000
   Month = MOD(Number, 10000) / 100
   Day   = MOD(Number, 100)
END SUBROUTINE  Conversion
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
!    Subrotine Test(i)
!    to test whether the input is valid?
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
LOGICAL FUNCTION Test(Number)
    IMPLICIT NONE
    INTEGER,INTENT(IN) :: Number
    INTEGER :: Year,Month,Day 

    Test = .FALSE.
    call Conversion(Number,Year,Month,Day)
    if(Year/1000 <9) THEN
        if(Month <= 12) THEN
            SELECT CASE (Month)
                CASE(1,3,5,7,8,10,12)
                    if(Day <= 31)  Test = .True.
                CASE(2)
                    !if() 少了润年的判断
                    if(day <=28)  Test = .True.
                case(4,6,9,11)
                    if(day <= 30) Test = .True.
            END SELECT
        END IF
    END IF

END FUNCTION Test
```



<br/>
<br/>
<br/>
<hr/>
<br/>
<br/>
<br/>


<h2 id="millionstone19">Bisect Method to solve equations </h2>

``` fortran

! --------------------------------------------------------------------
!    This program solves equations with the Bisection Method.  Given
! a function f(x) = 0.  The bisection method starts with two values,
! a and b such that f(a) and f(b) have opposite signs.  That is,
! f(a)*f(b) < 0.  Then, it is guaranteed that f(x)=0 has a root in
! the range of a and b.  This program reads in a and b (Left and Right
! in this program) and find the root in [a,b].
!    In the following, function f() is REAL FUNCTION Funct() and
! solve() is the function for solving the equation.
! --------------------------------------------------------------------
PROGRAM  Bisection
   IMPLICIT  NONE

   REAL, PARAMETER :: Tolerance = 0.00001
   REAL            :: Left,  fLeft
   REAL            :: Right, fRight
   REAL            :: Root

   WRITE(*,*)  'This program can solves equation F(x) = 0'
   WRITE(*,*)  'Please enter two values Left and Right such that '
   WRITE(*,*)  'F(Left) and F(Right) have opposite signs.'
   WRITE(*,*)
   WRITE(*,*)  'Left and Right please --> '
   READ(*,*)   Left, Right              ! read in Left and Right

   fLeft  = Funct(Left)                 ! compute their function values
   fRight = Funct(Right)
   WRITE(*,*)
   WRITE(*,*)  'Left = ', Left, '    f(Left) = ', fLeft
   WRITE(*,*)  'Right = ', Right, '   f(Right) = ', fRight
   WRITE(*,*)
   IF (fLeft*fRight > 0.0)  THEN
      WRITE(*,*)  '*** ERROR: f(Left)*f(Right) must be negative ***'
   ELSE
      Root = Solve(Left, Right, Tolerance)
      WRITE(*,*)  'A root is ', Root
   END IF

CONTAINS

! --------------------------------------------------------------------
! REAL FUNCTION  Funct()
!    This is for function f(x).  It takes a REAL formal argument and
! returns the value of f() at x.  The following is sample function
! with a root in the range of -10.0 and 0.0.  You can change the
! expression with your own function.
! --------------------------------------------------------------------

   REAL FUNCTION  Funct(x)
      IMPLICIT  NONE
      REAL, INTENT(IN) :: x
      REAL, PARAMETER  :: PI = 3.1415926
      REAL, PARAMETER  :: a  = 0.8475

      Funct = SQRT(PI/2.0)*EXP(a*x) + x/(a*a + x*x)

   END FUNCTION  Funct

! --------------------------------------------------------------------
! REAL FUNCTION  Solve()
!    This function takes Left - the left end, Right - the right end,
! and Tolerance - a tolerance value such that f(Left)*f(Right) < 0
! and find a root in the range of Left and Right.
!    This function works as follows.  Because of INTENT(IN), this
! function cannot change the values of Left and Right and therefore
! the values of Left and Right are saved to a and b.
!    Then, the middle point c=(a+b)/2 and its function value f(c)
! is computed.  If f(a)*f(c) < 0, then a root is in [a,c]; otherwise,
! a root is in [c,b].  In the former case, replacing b and f(b) with
! c and f(c), we still maintain that a root in [a,b].  In the latter,
! replacing a and f(a) with c and f(c) will keep a root in [a,b].
! This process will continue until |f(c)| is less than Tolerance and
! hence c can be considered as a root.
! --------------------------------------------------------------------

   REAL FUNCTION  Solve(Left, Right, Tolerance)
      IMPLICIT  NONE
      REAL, INTENT(IN) :: Left, Right, Tolerance
      REAL             :: a, Fa, b, Fb, c, Fc

      a = Left                          ! save Left and Right
      b = Right

      Fa = Funct(a)                     ! compute the function values
      Fb = Funct(b)
      IF (ABS(Fa) < Tolerance) THEN     ! if f(a) is already small
         Solve = a                      ! then a is a root
      ELSE IF (ABS(Fb) < Tolerance) THEN     ! is f(b) is small
         Solve = b                      ! then b is a root
      ELSE                              ! otherwise,
         DO                             ! iterate ....
            c  = (a + b)/2.0            !   compute the middle point
            Fc = Funct(c)               !   and its function value
            IF (ABS(Fc) < Tolerance) THEN    ! is it very small?
               Solve = c                ! yes, c is a root
               EXIT
            ELSE IF (Fa*Fc < 0.0) THEN  ! do f(a)*f(c) < 0 ?
               b  = c                   ! replace b with c
               Fb = Fc                  ! and f(b) with f(c)
            ELSE                        ! then f(c)*f(b) < 0 holds
               a  = c                   ! replace a with c
               Fa = Fc                  ! and f(a) with f(c)
            END IF
         END DO                         ! go back and do it again
      END IF
   END FUNCTION  Solve

END PROGRAM  Bisection
```


<br/>
<br/>
<br/>
<hr/>
<br/>
<br/>
<br/>


<h2 id="millionstone20">cmToInch</h2>

cmToInch2.f90:

``` fortran
!#!-*-author:zhaoliang-*-
!#!-*-coding:utf8-*-
!#Date: 2014年 01月 27日 星期一 10:45:56 CST
!! ---------------------------------------------------------------
!    This program "contains" two REAL functions:
!         (1)  Cm_to_Inch() takes a real inch unit and converts
!              it to cm unit, and
!         (2)  Inch_to_cm() takes a real cm unit and converts it
!              to inch unit.
! The main program uses these functions to convert 0, 0.5, 1, 1.5,
! 2.0, 2.5, ..., 8.0, 8.5, 9.0, 9.5 and 10.0 inch (resp., cm) to
! cm (resp., inch).
! ---------------------------------------------------------------
PROGRAM Conversion
      IMPLICIT NONE

      REAL, PARAMETER :: Initial = 0.0, Final = 10.0 , Step =0.5
      REAL            :: x

      x = Initial
      DO
          IF(x > Final ) EXIT
          WRITE(*,"(F3.1,1X,A3,F5.2,1X,A10,5X,F3.1,1X,A7,F5.2,1X,A2)") x, 'cm=' , Cm_to_Inch(x),'inch   and', &
              x, 'inch = ',Inch_to_Cm(x), 'cm'
          x  = x + step
      END DO

CONTAINS
! ---------------------------------------------------------------
! REAL FUNCTION  Cm_to_Inch()
!    This function converts its real input in cm to inch.
! ---------------------------------------------------------------
      REAL FUNCTION Cm_to_Inch(cm)
          IMPLICIT NONE

          REAL,INTENT(IN) :: cm
          REAL,PARAMETER  :: To_Inch = 0.3937  ! conversion Factor

          Cm_to_Inch = To_Inch * cm
      END FUNCTION Cm_to_Inch
! 0.3937 and 2,54 是 最重要的两个数
      REAL FUNCTION Inch_to_Cm(inch)
          IMPLICIT NONE

          REAL,INTENT(IN) :: inch
          REAL,PARAMETER  :: To_Cm = 2.54  ! conversion Factor

          Inch_to_Cm = To_Cm * inch
      END FUNCTION Inch_to_Cm
END PROGRAM Conversion


!------------------------------------------------------------不断变化的输出控制----------------------------------------<
!root at yezhaoliang-N53SM in /home/happycamp-of-fortran/sheneTurtorilForFortran90
!$ ./a.out 
!   0.00000000     cm=   0.00000000     inch   and   0.00000000     inch =    0.00000000     cm
!  0.500000000     cm=  0.196850002     inch   and  0.500000000     inch =    1.26999998     cm
!   1.00000000     cm=  0.393700004     inch   and   1.00000000     inch =    2.53999996     cm
!   1.50000000     cm=  0.590550005     inch   and   1.50000000     inch =    3.80999994     cm
!   2.00000000     cm=  0.787400007     inch   and   2.00000000     inch =    5.07999992     cm
!   2.50000000     cm=  0.984250009     inch   and   2.50000000     inch =    6.34999990     cm
!   3.00000000     cm=   1.18110001     inch   and   3.00000000     inch =    7.61999989     cm
!   3.50000000     cm=   1.37794995     inch   and   3.50000000     inch =    8.88999939     cm
!   4.00000000     cm=   1.57480001     inch   and   4.00000000     inch =    10.1599998     cm
!   4.50000000     cm=   1.77165008     inch   and   4.50000000     inch =    11.4300003     cm
!   5.00000000     cm=   1.96850002     inch   and   5.00000000     inch =    12.6999998     cm
!   5.50000000     cm=   2.16534996     inch   and   5.50000000     inch =    13.9699993     cm
!   6.00000000     cm=   2.36220002     inch   and   6.00000000     inch =    15.2399998     cm
!   6.50000000     cm=   2.55905008     inch   and   6.50000000     inch =    16.5100002     cm
!   7.00000000     cm=   2.75589991     inch   and   7.00000000     inch =    17.7799988     cm
!   7.50000000     cm=   2.95274997     inch   and   7.50000000     inch =    19.0499992     cm
!   8.00000000     cm=   3.14960003     inch   and   8.00000000     inch =    20.3199997     cm
!   8.50000000     cm=   3.34645009     inch   and   8.50000000     inch =    21.5900002     cm
!   9.00000000     cm=   3.54330015     inch   and   9.00000000     inch =    22.8600006     cm
!   9.50000000     cm=   3.74014997     inch   and   9.50000000     inch =    24.1299992     cm
!   10.0000000     cm=   3.93700004     inch   and   10.0000000     inch =    25.3999996     cm
!
!root at yezhaoliang-N53SM in /home/happycamp-of-fortran/sheneTurtorilForFortran90
!$ vim cmToinch.f90 
!
!root at yezhaoliang-N53SM in /home/happycamp-of-fortran/sheneTurtorilForFortran90
!$ gfortran cmToinch.f90 
!
!root at yezhaoliang-N53SM in /home/happycamp-of-fortran/sheneTurtorilForFortran90
!$ ./a.out 
!0.0 cm=    0.00 inch   and     0.0 inch =     0.00 cm
!0.5 cm=    0.20 inch   and     0.5 inch =     1.27 cm
!1.0 cm=    0.39 inch   and     1.0 inch =     2.54 cm
!1.5 cm=    0.59 inch   and     1.5 inch =     3.81 cm
!2.0 cm=    0.79 inch   and     2.0 inch =     5.08 cm
!2.5 cm=    0.98 inch   and     2.5 inch =     6.35 cm
!3.0 cm=    1.18 inch   and     3.0 inch =     7.62 cm
!3.5 cm=    1.38 inch   and     3.5 inch =     8.89 cm
!4.0 cm=    1.57 inch   and     4.0 inch =    10.16 cm
!4.5 cm=    1.77 inch   and     4.5 inch =    11.43 cm
!5.0 cm=    1.97 inch   and     5.0 inch =    12.70 cm
!5.5 cm=    2.17 inch   and     5.5 inch =    13.97 cm
!6.0 cm=    2.36 inch   and     6.0 inch =    15.24 cm
!6.5 cm=    2.56 inch   and     6.5 inch =    16.51 cm
!7.0 cm=    2.76 inch   and     7.0 inch =    17.78 cm
!7.5 cm=    2.95 inch   and     7.5 inch =    19.05 cm
!8.0 cm=    3.15 inch   and     8.0 inch =    20.32 cm
!8.5 cm=    3.35 inch   and     8.5 inch =    21.59 cm
!9.0 cm=    3.54 inch   and     9.0 inch =    22.86 cm
!9.5 cm=    3.74 inch   and     9.5 inch =    24.13 cm
!*** cm=    3.94 inch   and     *** inch =    25.40 cm
!
!root at yezhaoliang-N53SM in /home/happycamp-of-fortran/sheneTurtorilForFortran90
!$ vim cmToinch.f90 
!
!root at yezhaoliang-N53SM in /home/happycamp-of-fortran/sheneTurtorilForFortran90
!$ vim cmToinch.f90 
!
!root at yezhaoliang-N53SM in /home/happycamp-of-fortran/sheneTurtorilForFortran90
!$ gfortran cmToinch.f90 
!
!root at yezhaoliang-N53SM in /home/happycamp-of-fortran/sheneTurtorilForFortran90
!$ ./a.out 
!0.0 cm= 0.00 inch   and     0.0 inch =  0.00 cm
!0.5 cm= 0.20 inch   and     0.5 inch =  1.27 cm
!1.0 cm= 0.39 inch   and     1.0 inch =  2.54 cm
!1.5 cm= 0.59 inch   and     1.5 inch =  3.81 cm
!2.0 cm= 0.79 inch   and     2.0 inch =  5.08 cm
!2.5 cm= 0.98 inch   and     2.5 inch =  6.35 cm
!3.0 cm= 1.18 inch   and     3.0 inch =  7.62 cm
!3.5 cm= 1.38 inch   and     3.5 inch =  8.89 cm
!4.0 cm= 1.57 inch   and     4.0 inch = 10.16 cm
!4.5 cm= 1.77 inch   and     4.5 inch = 11.43 cm
!5.0 cm= 1.97 inch   and     5.0 inch = 12.70 cm
!5.5 cm= 2.17 inch   and     5.5 inch = 13.97 cm
!6.0 cm= 2.36 inch   and     6.0 inch = 15.24 cm
!6.5 cm= 2.56 inch   and     6.5 inch = 16.51 cm
!7.0 cm= 2.76 inch   and     7.0 inch = 17.78 cm
!7.5 cm= 2.95 inch   and     7.5 inch = 19.05 cm
!8.0 cm= 3.15 inch   and     8.0 inch = 20.32 cm
!8.5 cm= 3.35 inch   and     8.5 inch = 21.59 cm
!9.0 cm= 3.54 inch   and     9.0 inch = 22.86 cm
!9.5 cm= 3.74 inch   and     9.5 inch = 24.13 cm
!*** cm= 3.94 inch   and     *** inch = 25.40 cm
```

<br/>
<br/>
<br/>
<hr/>
<br/>
<br/>
<br/>


<h2 id="millionstone21">Colon Test</h2>

``` fortran
!#!-*-author:zhaoliang-*-
!#!-*-coding:utf8-*-
!#Date: 2014年 01月 26日 星期日 21:38:10 CST
PROGRAM  ColonTest
   IMPLICIT  NONE
   INTEGER                      :: i, n
   CHARACTER(LEN=15), PARAMETER :: DashLine = "---------------"

    WRITE(*,*) "Please Enter one integer:"
   READ(*,*)  n
   WRITE(*,"(1X,5I3/)")   (i, i = 1, n)
   WRITE(*,"(1X,a)")      DashLine
   WRITE(*,"(1X,5I3:/)")  (i, i = 1, n)
   WRITE(*,"(1X,a)")      DashLine
END PROGRAM  ColonTest

```


<br/>
<br/>
<br/>
<hr/>
<br/>
<br/>
<br/>


<h2 id="millionstone22">Factorize</h2>

``` fortran
!#!-*-author:zhaoliang-*-
!#!-*-coding:utf8-*-
!#Date: 2014年 01月 29日 星期三 12:20:05 CST

PROGRAM Factorize
      IMPLICIT NONE

      INTEGER :: Input
      INTEGER :: Divisor
      INTEGER :: Count

      WRITE (*,*) 'This Program Factorize any integer >= 2 --->'
      WRITE(*,*) 'Please Enter an integer:'
      READ(*,*) Input
      Count = 0
      WRITE(*,*) 'The most outstanding version!'
      DO 
          IF(Mod(Input,2) /= 0 .OR. Input == 1) EXIT!removes all factor
         ! of 2 
          Count = Count + 1  !increase count
          Write(*,*) 'Factor # ',Count,": ",2
          Input = Input / 2  ! remove this factor 2
      END DO
    ! 3 , 5 ,7 Now we only care about the odd number
      Divisor = 3
      DO 
          IF(Divisor > Input) Exit
          IF(.NOT. Prime(Divisor)) THEN
              Divisor = Divisor + 2
              CYCLE
          END IF
          DO 
              IF(MOD(Input,Divisor) /= 0 .OR. Input == 1) EXIT
              !直到所有的当前divisor被去除掉，才中止除操作
              Count = Count + 1
              WRITE(*,*) 'Factor # ',Count,": ",Divisor
              Input = Input /Divisor  ! remove this factor of Divisor
          END DO
          Divisor = Divisor + 2
      END DO

CONTAINS
! --------------------------------------------------------------------
! LOGICAL FUNCTION  Prime()
!    This function receives an INTEGER formal argument Number.  If it
! is a prime number, .TRUE. is returned; otherwise, this function
! returns .FALSE.
! --------------------------------------------------------------------
      LOGICAL FUNCTION Prime(Number)
          !判断一个数是不是素数
          IMPLICIT nONE

          INTEger,INTENT(IN)  :: Number
          INTEGER             :: Divisor

          IF(Number < 2) THEN
              Prime = .False.
          ELSE IF(Number == 2) THEN
              Prime = .TRUE.
          ELSE IF(MOD(Number,2) == 0) THEN
              Prime = .False.
          ELSE
              Divisor = 3
              DO 
                  IF(Divisor*Divisor>Number .OR. MOD(Number,Divisor)==0) EXIT
                  !divisor*divisor is much better than sqrt(number)
                  Divisor = Divisor + 2
              END DO
              Prime = Divisor* Divisor > Number
        END IF
      END FUNCTION Prime


END PROGRAM Factorize
```

[1]: http://www.ccs.neu.edu/home/matthias/BTLS/
[2]: http://www.ccs.neu.edu/home/matthias/BTSS/
[3]: http://www.cs.mtu.edu/~shene/COURSES/cs201/NOTES/fortran.html

