# update_step.f90

## 目的
`update_step.f90` 文件负责在每个时间步长中更新网格动量、粒子应力和粒子位置。它包含几个处理更新过程不同方面的子程序。

## 主要子程序
1. `GridMomentumInitial()`
   - 目的：通过将粒子变量映射到网格节点来初始化网格动量。
   - 输入：无
   - 输出：更新的网格节点质量和动量

2. `ParticleStressUpdate()`
   - 目的：计算应变率和旋转张量，并使用本构模型更新应力。
   - 输入：无
   - 输出：更新的粒子应力

3. `GridMomentumUpdate()`
   - 目的：计算网格节点力。
   - 输入：无
   - 输出：更新的网格节点力

4. `IntegrateMomentum()`
   - 目的：在计算网格上积分动量方程并应用边界条件。
   - 输入：无
   - 输出：更新的网格节点动量

5. `Lagr_NodContact()`
   - 目的：建立节点接触标准，校正法向矢量并应用接触力。
   - 输入：无
   - 输出：更新的网格节点力和动量

6. `ParticlePositionUpdate()`
   - 目的：更新粒子位置和速度。
   - 输入：无
   - 输出：更新的粒子位置和速度

7. `GridMomentumMUSL()`
   - 目的：通过映射更新的粒子信息重新计算网格节点动量并应用边界条件。
   - 输入：无
   - 输出：更新的网格节点动量

8. `ApplyBoundaryConditions()`
   - 目的：对网格节点应用边界条件。
   - 输入：无
   - 输出：更新的网格节点动量

## 参数
1. `nb_body`: 体的数量
2. `nb_particle`: 粒子的数量
3. `nb_gridnode`: 网格节点的数量
4. `nb_centernode`: 中心节点的数量
5. `nb_InflNode`: 影响节点的数量
6. `centernb_InflNode`: 中心影响节点的数量
7. `DCell`: 网格单元大小
8. `DT`: 时间步长
9. `CutOff`: 网格质量的截止值
10. `tot_cont_for`: 总接触力
11. `fricfa`: 摩擦因子
12. `normbody`: 接触法线的计算方法
13. `InflNode`: 影响节点数组
14. `CenterInflNode`: 中心影响节点数组
15. `SHP`: 形函数值
16. `DNDX`: x方向的形函数导数
17. `DNDY`: y方向的形函数导数
18. `DNDZ`: z方向的形函数导数
19. `CSHP`: 中心形函数值
20. `CDNDX`: 中心节点x方向的形函数导数
21. `CDNDY`: 中心节点y方向的形函数导数
22. `CDNDZ`: 中心节点z方向的形函数导数

## 方程
1. 应变增量：`de = de * DT`
   - 该方程通过将应变增量乘以时间步长（DT）来更新应变增量。
2. 旋转增量：`vort = vort * DT / 2`
   - 该方程通过将旋转增量乘以时间步长的一半（DT/2）来更新旋转增量。
3. 节点质量：`gd%Mg = gd%Mg + shm`
   - 该方程通过将形函数质量（shm）添加到现有节点质量（gd%Mg）来更新节点质量。
4. 节点动量：`gd%PXg = gd%PXg + pt%VXp*shm`
   - 该方程通过将粒子速度（pt%VXp）和形函数质量（shm）的乘积添加到现有节点动量（gd%PXg）来更新节点动量。
5. 节点力：`gd%FXg = gd%FXg + f_int + f_ext`
   - 该方程通过将内力（f_int）和外力（f_ext）添加到现有节点力（gd%FXg）来更新节点力。
6. 内力：`f_int = - (sxx*DNDXn + sxy*DNDYn + sxz*DNDZn)*vol_`
   - 该方程通过将应力分量（sxx、sxy、sxz）和形函数导数（DNDXn、DNDYn、DNDZn）的乘积相加，并乘以体积（vol_）来计算内力。
7. 外力：`f_ext = fx*SHPn`
   - 该方程通过将外力分量（fx）与形函数值（SHPn）相乘来计算外力。
8. 节点速度：`vgx = gd%PXg / gd%Mg`
   - 该方程通过将节点动量（gd%PXg）除以节点质量（gd%Mg）来计算节点速度。
9. 粒子位置更新：`pt%XX = xx + vx * DT`
   - 该方程通过将粒子速度（vx）和时间步长（DT）的乘积添加到现有粒子位置（xx）来更新粒子位置。
10. 粒子速度更新：`pt%VXp = pt%VXp + ax * DT`
    - 该方程通过将粒子加速度（ax）和时间步长（DT）的乘积添加到现有粒子速度（pt%VXp）来更新粒子速度。
11. 接触力：`nomforce = crit / tt`
    - 该方程通过将临界值（crit）除以时间步长（tt）来计算法向接触力。
12. 切向接触力：`fstick = cforce - nomforce*CP1%ndir`
    - 该方程通过将法向接触力（nomforce）和接触法线方向（CP1%ndir）的乘积从接触力（cforce）中减去来计算切向接触力。
13. 滑动接触力：`val_fslip = fricfa*abs(nomforce)`
    - 该方程通过将摩擦因子（fricfa）与法向接触力（nomforce）的绝对值相乘来计算滑动接触力。
14. 接触力调整：`cforce = nomforce*CP1%ndir + val_fslip*(fstick /val_fstick)`
    - 该方程通过将法向接触力（nomforce）和接触法线方向（CP1%ndir）的乘积与滑动接触力（val_fslip）和切向接触力（fstick）与滑动接触力（val_fstick）之比的乘积相加来调整接触力。
