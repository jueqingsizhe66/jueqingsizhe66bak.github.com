---
layout: post
title: "Ubuntu15.10-32bit-安装FastV8.15"
date: 2016-04-15 14:46:04 +0800
comments: true
categories: Fast
---

Fast8.15在4-15又发布了新版本，增加了相关功能，具体参考[FastV8.15][1].
下面就32bit ubuntu机子的安装过程做简要记录。
<!--more-->

1. 运行fast源文件下的Compling 的makefile

``` sh
gfortran: error: ../bin/libmap-1.20.10.so: 没有那个文件或目录
makefile:733: recipe for target '../bin/FAST_glin32' failed
make:  [../bin/FAST_glin32] Error 1

```

直接运行make，缺少libmap,可以参考[fast安装][2]

2. 安装LibMap


把libMap-1的Compling底下的driver_makefile修改一下你的FAST本地路径。

``` 
#FAST_DIR     = ../Source
#FAST_DIR     = C:/Users/bjonkman/Documents/DATA/DesignCodes/simulators/FAST/SVNdirectory/branches/BJonkman/Source
FAST_DIR     = /paper/FAST_8.15/ 

```

直接定位到MapDir/src的makefile，进行make一下
```
/usr/bin/ld: cannot find -llapacke
/usr/bin/ld: cannot find -llapacke
collect2: error: ld returned 1 exit status
makefile:81: recipe for target 'all' failed
make: *** [all] Error 1

```

这个原因是因为缺少lapacke开发者库,参考[FAST安装][2]。
```
apt-get install lapacke-dev*

```

然后就可以make，并把生成的libmap-1.2.so放入到fastdir的bin文件夹，没有则创建。
这样就完成了

3. 测试

+ Test02.fst

```
./FAST_glin32 Test02.fst 

 **************************************************************************************************
 FAST (v8.15.00a-bjj, 12-Apr-2016)

 Copyright (C) 2016 National Renewable Energy Laboratory

 This program comes with ABSOLUTELY NO WARRANTY. See the "license.txt" file distributed with this
 software for details.
 **************************************************************************************************

  Running FAST (v8.15.00a-bjj, 12-Apr-2016), compiled as a 32-bit application using single
  precision
  linked with NWTC Subroutine Library (v2.08.00, 5-Apr-2016)

  Heading of the FAST input file:
    FAST Certification Test #02: AWT-27CR2 with many DOFs with startup and shutdown and steady wind

  Running ElastoDyn (v1.03.02a-bjj, 8-Apr-2016).

  Running AeroDyn (v15.02.03, 12-Apr-2016).

  Running AirfoilInfo (v1.01.00a-bjj, 5-Apr-2016).

  Running BEM (v1.01.00a, 12-Apr-2016).

  Running InflowWind (v3.02.00a-bjj, 11-Apr-2016).
  Opening InflowWind input file:  ./AWT27/Test02_InflowWind.dat

  Running ServoDyn (v1.05.00a-bjj, 11-Mar-2016).
  Timestep: 0 of 20 seconds.

 Timestep: 2 of 20 seconds. Estimated final completion at 15:12:15.                               
 Timestep: 4 of 20 seconds. Estimated final completion at 15:12:14.                               
 Timestep: 6 of 20 seconds. Estimated final completion at 15:12:14.                               
 Timestep: 8 of 20 seconds. Estimated final completion at 15:12:14.                               
 Timestep: 10 of 20 seconds. Estimated final completion at 15:12:14.                              
 Timestep: 12 of 20 seconds. Estimated final completion at 15:12:14.                              
 Timestep: 14 of 20 seconds. Estimated final completion at 15:12:14.                              
 Timestep: 16 of 20 seconds. Estimated final completion at 15:12:14.                              
 Timestep: 18 of 20 seconds. Estimated final completion at 15:12:14.                              
 Timestep: 20 of 20 seconds. Estimated final completion at 15:12:14.                              
                                                                                                  
  Total Real Time:       9.788 seconds
  Total CPU Time:        9.708 seconds
  Simulation CPU Time:   9.636 seconds
  Simulated Time:        20 seconds
  Time Ratio (Sim/CPU):  2.0756

  FAST terminated normally.

```

+ Test26.fst

不通过的原因是没有编译DISCON_DLL.

```
➞ ./FAST_glin32 Test26.fst 

 **************************************************************************************************
 FAST (v8.15.00a-bjj, 12-Apr-2016)

 Copyright (C) 2016 National Renewable Energy Laboratory

 This program comes with ABSOLUTELY NO WARRANTY. See the "license.txt" file distributed with this
 software for details.
 **************************************************************************************************

  Running FAST (v8.15.00a-bjj, 12-Apr-2016), compiled as a 32-bit application using single
  precision
  linked with NWTC Subroutine Library (v2.08.00, 5-Apr-2016)

  Heading of the FAST input file:
    FAST Certification Test #26: NREL 5.0 MW Baseline Wind Turbine (Onshore)

  Running ElastoDyn (v1.03.02a-bjj, 8-Apr-2016).

  Running BeamDyn (v1.01.03, 12-Apr-2016).

  Running BeamDyn (v1.01.03, 12-Apr-2016).

  Running BeamDyn (v1.01.03, 12-Apr-2016).

  Running AeroDyn (v15.02.03, 12-Apr-2016).

  Running AirfoilInfo (v1.01.00a-bjj, 5-Apr-2016).

  Running BEM (v1.01.00a, 12-Apr-2016).
 Warning: Turning off Unsteady Aerodynamics because C_nalpha is 0.  BladeNode = 1, Blade = 1
 Warning: Turning off Unsteady Aerodynamics because C_nalpha is 0.  BladeNode = 2, Blade = 1
 Warning: Turning off Unsteady Aerodynamics because C_nalpha is 0.  BladeNode = 3, Blade = 1
 Warning: Turning off Unsteady Aerodynamics because C_nalpha is 0.  BladeNode = 4, Blade = 1
 Warning: Turning off Unsteady Aerodynamics because C_nalpha is 0.  BladeNode = 1, Blade = 2
 Warning: Turning off Unsteady Aerodynamics because C_nalpha is 0.  BladeNode = 2, Blade = 2
 Warning: Turning off Unsteady Aerodynamics because C_nalpha is 0.  BladeNode = 3, Blade = 2
 Warning: Turning off Unsteady Aerodynamics because C_nalpha is 0.  BladeNode = 4, Blade = 2
 Warning: Turning off Unsteady Aerodynamics because C_nalpha is 0.  BladeNode = 1, Blade = 3
 Warning: Turning off Unsteady Aerodynamics because C_nalpha is 0.  BladeNode = 2, Blade = 3
 Warning: Turning off Unsteady Aerodynamics because C_nalpha is 0.  BladeNode = 3, Blade = 3
 Warning: Turning off Unsteady Aerodynamics because C_nalpha is 0.  BladeNode = 4, Blade = 3

  Running InflowWind (v3.02.00a-bjj, 11-Apr-2016).
  Opening InflowWind input file:  ./5MW_Baseline/NRELOffshrBsline5MW_InflowWind_12mps.dat

    Reading a 31x31 grid (145 m wide, 17.5 m to 162.5 m above ground) with a characteristic wind
    speed of 12 m/s. This full-field file was generated by TurbSim (v1.06.00, 21-Sep-2012) on
    07-Jan-2014 at 12:50:45.

    Processed 1442 time steps of 20-Hz full-field data (72.05 seconds).

  Running ServoDyn (v1.05.00a-bjj, 11-Mar-2016).

  Running ServoDyn Interface for Bladed Controllers (using GNU Fortran for Linux, 14-Oct-2015).

 FAST_InitializeAll:SrvD_Init:BladedInterface_Init:The dynamic library
 ./5MW_Baseline/ServoData/DISCON_win32.dll could not be loaded. Check that the file exists in the
 specified location and that it is compiled for 32-bit applications.

 FAST encountered an error during module initialization.
  Simulation error level: FATAL ERROR

  Aborting FAST.


```

4. 解决Test26无法运行

首先，定位到Fast的Compling目录，基本尚不修改，测试make（默认32bit）,通过。

```
make -f makefile_DISCON_DLL 
gfortran  -O2 -m32 -fbacktrace -ffree-line-length-none -x f95-cpp-input -C -DIMPLICIT_DLLEXPORT -fPIC -c ../CertTest/5MW_Baseline/ServoData/Source/DISCON.f90 -o Obj_lin32/DISCON.obj -J Obj_lin32 -B Obj_lin32
gfortran -shared -O2 -m32 -fbacktrace -fPIC -I Obj_lin32 -o ../CertTest/5MW_Baseline/ServoData/DISCON_glin32.so \
 Obj_lin32/DISCON.obj

```

注意一定得把 生成的DISCON_glin32.so路径写入到对应的NREL5MW Servodata的配置文件，比如

```
Test26.fst文件：

"5MW_Baseline/NRELOffshrBsline5MW_Onshore_ServoDyn.dat"    ServoFile       - Name of file containing control and electrical-drive input parameters (quoted string)

"unused"      HydroFile       - Name of file containing hydrodynamic input parameters (quoted string)

NRELOffshrBsline5MW_Onshore_ServoDyn.dat 更改对应的DLL_FileName 为 ServoData/DISCON_glin32.so

```

**注意还是得按照FAST 非windows平台的编译顺序把register编译好,然后一次MAP++ 和FAST，以及DISCONDLL.**

```
─➞ ../bin/FAST_glin32 Test26.fst 

 **************************************************************************************************
 FAST (v8.15.00a-bjj, 12-Apr-2016)

 Copyright (C) 2016 National Renewable Energy Laboratory

 This program comes with ABSOLUTELY NO WARRANTY. See the "license.txt" file distributed with this
 software for details.
 **************************************************************************************************

  Running FAST (v8.15.00a-bjj, 12-Apr-2016), compiled as a 32-bit application using single
  precision
  linked with NWTC Subroutine Library (v2.08.00, 5-Apr-2016)

  Heading of the FAST input file:
    FAST Certification Test #26: NREL 5.0 MW Baseline Wind Turbine (Onshore)

  Running ElastoDyn (v1.03.02a-bjj, 8-Apr-2016).

  Running BeamDyn (v1.01.03, 12-Apr-2016).

  Running BeamDyn (v1.01.03, 12-Apr-2016).

  Running BeamDyn (v1.01.03, 12-Apr-2016).

  Running AeroDyn (v15.02.03, 12-Apr-2016).

  Running AirfoilInfo (v1.01.00a-bjj, 5-Apr-2016).

  Running BEM (v1.01.00a, 12-Apr-2016).
 Warning: Turning off Unsteady Aerodynamics because C_nalpha is 0.  BladeNode = 1, Blade = 1
 Warning: Turning off Unsteady Aerodynamics because C_nalpha is 0.  BladeNode = 2, Blade = 1
 Warning: Turning off Unsteady Aerodynamics because C_nalpha is 0.  BladeNode = 3, Blade = 1
 Warning: Turning off Unsteady Aerodynamics because C_nalpha is 0.  BladeNode = 4, Blade = 1
 Warning: Turning off Unsteady Aerodynamics because C_nalpha is 0.  BladeNode = 1, Blade = 2
 Warning: Turning off Unsteady Aerodynamics because C_nalpha is 0.  BladeNode = 2, Blade = 2
 Warning: Turning off Unsteady Aerodynamics because C_nalpha is 0.  BladeNode = 3, Blade = 2
 Warning: Turning off Unsteady Aerodynamics because C_nalpha is 0.  BladeNode = 4, Blade = 2
 Warning: Turning off Unsteady Aerodynamics because C_nalpha is 0.  BladeNode = 1, Blade = 3
 Warning: Turning off Unsteady Aerodynamics because C_nalpha is 0.  BladeNode = 2, Blade = 3
 Warning: Turning off Unsteady Aerodynamics because C_nalpha is 0.  BladeNode = 3, Blade = 3
 Warning: Turning off Unsteady Aerodynamics because C_nalpha is 0.  BladeNode = 4, Blade = 3

  Running InflowWind (v3.02.00a-bjj, 11-Apr-2016).
  Opening InflowWind input file:  ./5MW_Baseline/NRELOffshrBsline5MW_InflowWind_12mps.dat

    Reading a 31x31 grid (145 m wide, 17.5 m to 162.5 m above ground) with a characteristic wind
    speed of 12 m/s. This full-field file was generated by TurbSim (v1.06.00, 21-Sep-2012) on
    07-Jan-2014 at 12:50:45.

    Processed 1442 time steps of 20-Hz full-field data (72.05 seconds).

  Running ServoDyn (v1.05.00a-bjj, 11-Mar-2016).

  Running ServoDyn Interface for Bladed Controllers (using GNU Fortran for Linux, 14-Oct-2015).
  Timestep: 0 of 20 seconds.

 FAST_Solution0:CalcOutputs_And_SolveForInputs:SolveOption2:SrvD_CalcOutput:Running with torque
 and pitch control of the NREL offshore 5MW baseline wind turbine from DISCON.dll as written by J.
 Jonkman of NREL/NWTC for use in the IEA Annex XXIII OC3 studies.


 Timestep: 1 of 20 seconds. Estimated final completion at 16:45:58.                               
 Timestep: 2 of 20 seconds. Estimated final completion at 16:45:53.                               
 Timestep: 3 of 20 seconds. Estimated final completion at 16:45:58.                               
 Timestep: 4 of 20 seconds. Estimated final completion at 16:45:57.                               
 Timestep: 5 of 20 seconds. Estimated final completion at 16:45:57.                               
 Timestep: 6 of 20 seconds. Estimated final completion at 16:45:53.                               

```

[1]: https://nwtc.nrel.gov/FAST8 
[2]: http://jueqingsizhe66.github.io/blog/2015/10/22/fast-in-ubuntu64bit-bian-yi-zhu-yi-shi-xiang/ 
