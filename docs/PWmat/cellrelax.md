# 晶格优化
[示例下载](http://39.98.50.106/pwmat-resource/course-download/PWmat/Si-cell-relax.zip)

PWmat进行晶格弛豫计算主要是设置RELAX_DETAIL参数，介绍如下：
``` 
{
  RELAX_DETAIL = IMTH, NSTEP, FORCE_TOL, ISTRESS, TOL_STRESS
  
  IMTH控制弛豫算法，1是CG；2是BFGS；3是最速下降法
  NSTEP控制弛豫的最大步数
  FORCE_TOL控制离子弛豫的力的收敛，单位eV/A
  ISTRESS控制是否进行CELL优化，0不优化晶格；1优化晶格
  TOL_STRESS控制晶格优化的收敛条件，单位eV/A^2
}
```
本例中，etot.input如下：
```
{
   4 1
   job = relax
   in.psp1 = ONCV.PWM.Si.UPF
   in.atom = start.config
   relax_detail = 1 100 0.01 1 0.1
   ecut = 60                在晶格优化的时候截断能一般取比默认值大10Ry左右，截断能的默认值在赝势文件中查询wfc_cutoff
   mp_n123 = 11 11 11 1 1 1
}
```
atom1.config文件如下：
```
{
      2
     LATTICE
           0.00000000     2.80000000     2.80000000
           2.80000000     0.00000000     2.80000000
           2.80000000     2.80000000     0.00000000
     POSITION
       14     0.87500000     0.87500000     0.87500000 1 1 1
       14     0.12500000     0.12500000     0.12500000 1 1 1
}
```
计算结束后晶格优化的信息保存在MOVEMENT文件中，MOVEMENT文件：
```
{
     2 atoms,Iteration =   0, Etot,Ep,Ek =  -0.2141865397E+03  -0.2141865397E+03   0.0000000000E+00, Average Force=  0.00000E+00, Max force=  0.00000E+00
     Lattice vector
        0.0000000000E+00    0.2800000000E+01    0.2800000000E+01     stress:  0.160189E+01  0.000000E+00  0.000000E+00
        0.2800000000E+01    0.0000000000E+00    0.2800000000E+01     stress:  0.000000E+00  0.160189E+01  0.000000E+00
        0.2800000000E+01    0.2800000000E+01    0.0000000000E+00     stress:  0.000000E+00  0.000000E+00  0.160189E+01
     Position, move_x, move_y, move_z
       14    0.875000000    0.875000000    0.875000000     1  1  1
       14    0.125000000    0.125000000    0.125000000     1  1  1
     Force
       14    0.000000000    0.000000000    0.000000000
       14    0.000000000    0.000000000    0.000000000
     -------------------------------------------------
     … … 
     … … 
     -------------------------------------------------
        2 atoms,Iteration =   3, Etot,Ep,Ek =  -0.2142496399E+03  -0.2142496399E+03   0.0000000000E+00, Average Force=  0.00000E+00, Max force=  0.00000E+00
     Lattice vector
        0.0000000000E+00    0.2726977966E+01    0.2726977966E+01     stress: -0.766803E-01  0.000000E+00  0.000000E+00
        0.2726977966E+01    0.0000000000E+00    0.2726977966E+01     stress:  0.000000E+00 -0.766803E-01  0.000000E+00
        0.2726977966E+01    0.2726977966E+01    0.0000000000E+00     stress:  0.000000E+00  0.000000E+00 -0.766803E-01
     Position, move_x, move_y, move_z
       14    0.875000000    0.875000000    0.875000000     1  1  1
       14    0.125000000    0.125000000    0.125000000     1  1  1
     Force
       14    0.000000000    0.000000000    0.000000000
       14    0.000000000    0.000000000    0.000000000
     -------------------------------------------------
}
```

可以从上面的stress看出最后晶格整体所受的stress大小，注意这里晶格弛豫收敛的条件是三个晶格格矢最大的stress三个分量的平均值。

如果只想查看最后的结构，也可以直接查看final.config。

如果想查看晶格的一些信息可以输入
```
{
   atominfo.x final.config
   -------------------------------------------------
     USAGE: atominfo.x YOURFILE
     The LATTICE in Angstrom:
       a1 =       0.0000000      2.7269780      2.7269780
       a2 =       2.7269780      0.0000000      2.7269780
       a3 =       2.7269780      2.7269780      0.0000000
     The Reciprocal LATTICE:
       b1 =      -0.1833531      0.1833531      0.1833531
       b2 =       0.1833531     -0.1833531      0.1833531
       b3 =       0.1833531      0.1833531     -0.1833531
     The Volume(A^3) is:      40.5578460
        a1-length   a2-length   a3-length       alpha        beta       gamma
          3.85653     3.85653     3.85653    60.00000    60.00000    60.00000
}
```
