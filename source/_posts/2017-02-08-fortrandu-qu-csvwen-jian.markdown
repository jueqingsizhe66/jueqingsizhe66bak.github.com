---
layout: post
title: "fortran读取csv文件"
date: 2017-02-08 17:29:29 +0800
comments: true
categories: fortran
---

本文只是简单对一个文件读取模块DFile\_mod的一个运用，并读取逗号分隔的csv文件。
<!--more-->

![csvread][1]

FileMod.f90:

``` fortran
Module DFile_Mod
  Implicit None
  !!**************************************
  !*  识别文件的空格和逗号作为分隔符
  !!*************************************** 
contains 

subroutine binaryStreamType
implicit none
integer :: File_Unit
integer :: File_UnitOutput
   !!**************************************
    !* DFile_Mod 变量定义  100%
    !!*************************************** 
    Character(len=512) :: cLine
    integer :: nRow, nCol
    character(len=20) :: name="yezhaoliang",addr="zhangzhou" 
    INTEGER :: I
type :: data_head
integer(kind=2) :: column
end type 

type(data_head) :: FileHead
!real,allocatable :: g(:,:)
real,allocatable :: firstColumn(:)
real,allocatable :: secondColumn(:)
real,allocatable :: thirdColumn(:)

 Open( NewUnit=File_Unit , File = './data/myfile.csv' )
 Open( NewUnit=File_UnitOutput , File = './data/myfile12.csv' )

  nRow = GetFileN( File_Unit )
  write( * , * ) '文件共',nRow,'行！'
  read(File_Unit,*) FileHead
  write(*,*) 'm=',FileHead%column

  allocate(firstColumn(nRow-1))
  allocate(secondColumn(nRow-1))
  allocate(thirdColumn(nRow-1))
  
  !! 流方式读取
!  READ(File_Unit) g

250 format(3f10.4)  
251 format(I4,I4,f10.4)  
252 format (f10.4,A,f10.4,A,f10.4)
253 format (I4,A,I4,A,f10.4)
Do i = 1,nRow-1
    read(File_Unit,251) firstColumn(i),secondColumn(i),thirdColumn(i)
 end Do
  DO i = 1 ,nRow-1
   !write(*,'(f10.4,A,f10.4,A,f10.4)')  firstColumn(i),',',secondColumn(i),',',thirdColumn(i)
   write(File_UnitOutput,253)  firstColumn(i)*2,',--',secondColumn(i)*2,',--',thirdColumn(i)
  end do

deallocate(firstColumn)
deallocate(secondColumn)
deallocate(thirdColumn)
close(File_Unit)
close(File_UnitOutput)

  !Do i = 1, nRow
  !  Do j = 1, FileHead%column
  !
  !  end do
  !end do
  
end subroutine binaryStreamType


subroutine testType
implicit none
integer :: File_Unit
integer :: File_UnitOutput
   !!**************************************
    !* DFile_Mod 变量定义  100%
    !!*************************************** 
    Character(len=512) :: cLine
    integer :: nRow, nCol
    character(len=20) :: name="yezhaoliang",addr="zhangzhou" 
    INTEGER :: I
type :: data_head
integer(kind=2) :: column
end type 

type(data_head) :: FileHead
!real,allocatable :: g(:,:)
real,allocatable :: firstColumn(:)
real,allocatable :: secondColumn(:)
real,allocatable :: thirdColumn(:)

 Open( NewUnit=File_Unit , File = './data/A001.csv' )
 Open( NewUnit=File_UnitOutput , File = './data/A0012.csv' )

  nRow = GetFileN( File_Unit )
  write( * , * ) '文件共',nRow,'行！'
  read(File_Unit,*) FileHead
  write(*,*) 'm=',FileHead%column

  allocate(firstColumn(nRow-1))
  allocate(secondColumn(nRow-1))
  allocate(thirdColumn(nRow-1))
  
  !! 流方式读取
!  READ(File_Unit) g

250 format(3f10.4)  
251 format(I4,I4,f10.4)  
252 format (f10.4,A,f10.4,A,f10.4)
253 format (I4,A,I4,A,f10.4)
Do i = 1,nRow-1
    read(File_Unit,251) firstColumn(i),secondColumn(i),thirdColumn(i)
 end Do
  DO i = 1 ,nRow-1
   !write(*,'(f10.4,A,f10.4,A,f10.4)')  firstColumn(i),',',secondColumn(i),',',thirdColumn(i)
   write(File_UnitOutput,253)  firstColumn(i)*2,',--',secondColumn(i)*2,',--',thirdColumn(i)
  end do

deallocate(firstColumn)
deallocate(secondColumn)
deallocate(thirdColumn)
close(File_Unit)
close(File_UnitOutput)

  !Do i = 1, nRow
  !  Do j = 1, FileHead%column
  !
  !  end do
  !end do
  
end subroutine testType


  Integer Function GetDataN( cStr )
    Character( Len = * ) , Intent( IN ) :: cStr
    Integer :: i
    Logical :: bIsSeparator , bIsQuote
    GetDataN = 0
    bIsSeparator = .TRUE.
    bIsQuote = .FALSE.
    Do i = 1 , Len_Trim( cStr )
      Select Case( cStr(i:i) )
      Case( '"' , "'" ) !// 如果遇到引号
        If ( .Not.bIsQuote ) GetDataN = GetDataN + 1  !//如果不在引号中，则增加一个数据
        bIsQuote = .Not.bIsQuote !// 引号结束或开始
        bIsSeparator = .FALSE.
      Case( " " , "," , char(9) ) !// 如果遇到分隔符
        If ( .Not.bIsQuote ) then  !// 分隔符如果不在引号中
          bIsSeparator = .TRUE.
        End If
      Case Default      
        If ( bIsSeparator ) then
          GetDataN = GetDataN + 1
        End If
        bIsSeparator = .FALSE.
      End Select
    End Do
  End Function GetDataN
  
  Function f_numbervars(vars) result(numvars)
    character(len=*), intent(in) :: vars
    integer :: numvars
    character(len=len(vars)) :: tmpvars
    character(len=256) :: tmpvar
    tmpvars = trim(adjustl(vars))
    numvars = 0
    do while (len_trim(tmpvars) > 0)
      read(tmpvars, *) tmpvar
      numvars = numvars + 1
      tmpvars = tmpvars(index(tmpvars, trim(tmpvar))+len_trim(tmpvar):)
    end do
  End Function f_numbervars
  
  Integer Function GetFileN( iFileUnit )
  !// 此函数应在打开文件后立即调用。调用后读取位置返回文件起始位置
    Implicit None
    Integer , Intent( IN ) :: iFileUnit
    character( Len = 1 ) :: cDummy
    integer :: ierr
    GetFileN = 0
    Rewind( iFileUnit )
    Do
      Read( iFileUnit , * , ioStat = ierr ) cDummy
      If( ierr /= 0 ) Exit
      GetFileN = GetFileN + 1
    End Do
    Rewind( iFileUnit )
  End Function GetFileN 

End Module DFile_Mod


```


主函数main.f90:

``` fortran
program Main

use DFile_Mod
Implicit none

   INTEGER :: I
   !!**************************************
    !* DFile_Mod 变量定义  100%
    !!*************************************** 
    !Character(len=512) :: cLine
    !integer :: nRow, nCol
    !character(len=20) :: name="yezhaoliang",addr="zhangzhou" 
    !**************************************
    !*  测试DFile_Mod   100%
    !*************************************** 
    
 !Open( 53 , File = './data/in.txt' )
  !Open( 53 , File = './data/myfile.csv' )
  !
  !nRow = GetFileN( 53 )
  !write( * , * ) '文件共',nRow,'行！'
  !Do i = 1 , nRow
  !  read( 53 , '(a512)' ) cLine
  !  nCol = GetDataN( cLine )
  !  !nCol = f_numbervars( cLine )
  !  write( * , * ) i,'行有',nCol,'个数据'
  !End Do
  call binaryStreamType

  Close( 53 )
  end program Main

```

配置文件in.txt:

```
1 2 3 , fsd
4 5 6 7 : asd
8 9 10 11 12
13 f sadf ! gad
14 15

```

载入visual studio基本上就能运行。

[1]:/images/fortrandebug/csv/csv1.png
