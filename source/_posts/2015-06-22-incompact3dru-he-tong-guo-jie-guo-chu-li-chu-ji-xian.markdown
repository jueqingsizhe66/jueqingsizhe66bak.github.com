---
layout: post
title: "Incompact3d如何通过结果处理出迹线"
date: 2015-06-22 16:20:48 +0800
comments: true
categories: incompact3d
---

Incompact3D获得的数据都是二进制的数据文件，为了获得内部的文件
需要进一步通过编程获得，下面是一个获得pathline的源代码。
<!--more-->

说明：

1. 通过 module.f90文件读取 nx ny nz的值
2. 把下面的源代码编译并放在ux uy uz所在的文件夹当中
3. 进一步的相关信息，可以参考注释。

``` fortran
PROGRAM b
IMPLICIT NONE
Integer,parameter :: nx=128,ny=129,nz=84 
INTEGER :: I,J,K,COUNT,LN=128,COL=129,VOL=84
REAL(8),DIMENSION(nx,ny,nz) :: ux,uy,uz
CHARACTER(len=12)::NAME1="Incompact3d",NAME2="ux",NAME3="uy",NAME4="uz"
character(len=15) :: temp,temp1,temp2,temp3
CHARACTER(len=20) :: CFILEux
integer :: num

real,dimension(nx):: y1
real,dimension(ny):: y2
real,dimension(nz):: y3

!generation of the mesh
do i=1,nx
   y1(i)=(i-1)*0.098174770425 !0.8 is DX ! incompact3d.prm的配置长度除以网格尺度即可。
enddo


do j=1,ny
   y2(j)=(j-1)*0.015503875968992248 !0.8 is DY
enddo


do k=1,nz
   y3(k)=(k-1)*0.04986655005702381!0.8 is DZ
enddo


! 下面只是对于读取文件进行的一个后处理，只是为了方便批处理而已
22 format(I1)
23 format(I2)
DO num=1,30
    if(num .lt. 10) then
        write(temp,22) num
        temp1 =trim(NAME2)//trim('00')//trim(temp)
        temp2 =trim(NAME3)//trim('00')//trim(temp)
        temp3 =trim(NAME4)//trim('00')//trim(temp)
    else 
        write(temp,23) num
        temp1 =trim(NAME2)//trim('0')//trim(temp)
        temp2 =trim(NAME3)//trim('0')//trim(temp)
        temp3 =trim(NAME4)//trim('0')//trim(temp)
    end if
    
! 产生实际的文件名
CFILEux=trim('./pathchange/')//trim(temp1)//'.dat'
!read the ux 读取ux00*的数据
OPEN(10,FILE=temp1,FORM='UNFORMATTED',&
ACCESS='DIRECT', RECL=8, STATUS='OLD')

! read the uy
OPEN(11,FILE=temp2,FORM='UNFORMATTED',&
ACCESS='DIRECT', RECL=8, STATUS='OLD')
!read the uz
OPEN(12,FILE=temp3,FORM='UNFORMATTED',&
ACCESS='DIRECT', RECL=8, STATUS='OLD')

! 新建一个结果文件，并添加上tecplot的数据头

OPEN(20,FILE=CFILEux,FORM='FORMATTED')
WRITE (20,'(A6,A12)')               'TITLE=',TRIM(ADJUSTL(NAME1))
 WRITE (20,'(A36)')    'VARIABLES="X","Y","Z","VX","VY","VZ"'
      WRITE (20,'(A7,I4,A1,A2,I4,A1,A2,I4,A1,A7)') 'ZONE I=',LN,',','J=',COL,',','K=',VOL,',','F=POINT'

COUNT = 1
DO K=1,nz
    DO J=1,ny
        DO I=1,nx
            READ(10,REC=COUNT) ux(I,J,K)
            READ(11,REC=COUNT) uy(I,J,K)
            READ(12,REC=COUNT) uz(I,J,K)
        !    WRITE(20,30) I,J,K,ux(I,J,K),uy(I,J,K),uz(I,J,K)
             WRITE(20,30) y1(i),y2(j),y3(k),ux(I,J,K),uy(I,J,K),uz(I,J,K)
            30 format(I3,1X,I3,1X,I3,1X,E11.4,1x,E11.4,1x,E11.4)
            COUNT = COUNT + 1
        ENDDO
    ENDDO
ENDDO
ENDDO
CLOSE(10)
CLOSE(11)
CLOSE(10)
CLOSE(20)
END PROGRAM b
```
