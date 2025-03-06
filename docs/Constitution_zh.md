# Constitution.f90

## 目的
`Constitution.f90` 文件负责使用各种本构模型更新材料点的应力。它包含主要子程序 `Constitution` 以及处理不同材料模型及其对应状态方程（EOS）的几个其他子程序。

## 主要子程序
1. `Constitution(de, vort, b, p)`
   - 目的：使用本构模型更新应力。
   - 输入：
     - `de`：应变增量 (D11, D22, D33, 2D23, 2D13, 2D12) * DT
     - `vort`：旋度 * DT (W32, W13, W21) * DT
     - `b`：体索引
     - `p`：粒子索引
   - 输出：应力分量

2. `sigrot(vort, sig, sm, sd)`
   - 目的：旋转应力并更新平均应力和偏应力。
   - 输入：
     - `vort`：旋度增量 (W32, W13, W21) * DT
     - `sig`：时间步 t 的 Cauchy 应力 (S11, S22, S33, S23, S13, S12)
   - 输出：旋转后的 Cauchy 应力、偏应力和平均应力

3. `elastic_devi()`
   - 目的：使用弹性关系更新偏应力。
   - 输出：偏应力分量

4. `elastic_p()`
   - 目的：使用弹性关系更新压力。
   - 输出：平均应力（压力）

5. `lieupd()`
   - 目的：更新没有 EOS 的材料模型的能量。
   - 输出：内能

6. `hieupd(failure)`
   - 目的：更新具有 EOS 的材料模型的能量。
   - 输入：`failure` - 粒子的失效状态
   - 输出：内能

7. `EquivalentStress()`
   - 目的：计算等效应力 sqrt(3*J2)。
   - 输出：等效应力

8. `M3DM1()`
   - 目的：弹性材料模型。
   - 输出：更新的应力分量

9. `M3DM2()`
   - 目的：弹性完全塑性材料模型。
   - 输出：更新的应力分量

10. `M3DM3()`
    - 目的：各向同性硬化塑性材料模型。
    - 输出：更新的应力分量

11. `M3DM4(mat, DT, tmprt)`
    - 目的：Johnson-Cook 塑性材料模型。
    - 输入：
      - `mat`：材料属性
      - `DT`：时间步长
      - `tmprt`：温度
    - 输出：更新的应力分量

12. `M3DM5(mat, DT)`
    - 目的：简化的 Johnson-Cook 塑性材料模型。
    - 输入：
      - `mat`：材料属性
      - `DT`：时间步长
    - 输出：更新的应力分量

13. `M3DM6(ltim, ctim, dvelo)`
    - 目的：高爆炸药燃烧材料模型。
    - 输入：
      - `ltim`：点火时间
      - `ctim`：当前时间
      - `dvelo`：爆轰速度
    - 输出：燃烧分数

14. `M3DM7()`
    - 目的：空材料模型。
    - 输出：零偏应力

15. `M3DM8(mat, DT, tmprt, failure)`
    - 目的：具有失效的 Johnson-Cook 塑性材料模型。
    - 输入：
      - `mat`：材料属性
      - `DT`：时间步长
      - `tmprt`：温度
      - `failure`：粒子的失效状态
    - 输出：更新的应力分量

16. `M3DM9(mat, DT)`
    - 目的：土壤的完全弹塑性 Drucker-Prager 模型。
    - 输入：
      - `mat`：材料属性
      - `DT`：时间步长
    - 输出：更新的应力分量

17. `seleos(failure)`
    - 目的：使用适当的 EOS 更新压力和能量。
    - 输入：`failure` - 粒子的失效状态
    - 输出：更新的压力和内能

18. `eos1(failure)`
    - 目的：线性状态方程。
    - 输入：`failure` - 粒子的失效状态
    - 输出：更新的压力和内能

19. `eos2(failure)`
    - 目的：Mie-Gruneisen 状态方程。
    - 输入：`failure` - 粒子的失效状态
    - 输出：更新的压力和内能

20. `eos3(failure)`
    - 目的：JWL 状态方程。
    - 输入：`failure` - 粒子的失效状态
    - 输出：更新的压力和内能

21. `bulkq()`
    - 目的：计算体积粘性。
    - 输出：体积粘性力

## 参数
1. `mid`：材料集 ID
2. `etype_`：EOS（状态方程）类型
3. `mtype_`：材料模型类型
4. `young_`：杨氏模量
5. `poisson_`：泊松比
6. `yield0_`：初始屈服应力
7. `tangmod_`：切线模量
8. `den0_`：初始密度
9. `den_`：当前密度
10. `vold`：时间步长 n 的体积
11. `vol_`：当前体积
12. `vol0_`：初始体积
13. `dvol`：0.5 * 体积增量
14. `dinc`：应变增量
15. `sm`：平均应力
16. `smold`：上一步的平均应力
17. `sd`：偏应力
18. `sig`：应力分量
19. `sold`：时间步长 n 的偏应力
20. `dsm`：平均应力增量
21. `bqf`：体积粘性力
22. `seqv`：等效应力
23. `epeff_`：有效塑性应变
24. `sig_y_`：当前屈服应力
25. `depeff`：等效塑性应变增量
26. `ratio`：硬化计算的比率
27. `G2`：2 * 剪切模量
28. `K3`：3 * 体积模量
29. `PlaMod`：塑性硬化模量
30. `specheat_`：比热
31. `tmprt`：温度
32. `iener`：内能
33. `specen`：每单位初始体积的内能
34. `ieinc`：内能增量
35. `mu`： (den_ / den0_) - 1
36. `rv`：相对体积 (vol_ / vol0_)
37. `bfac`：燃烧分数
38. `cp`：声速
39. `DMG`：损伤
40. `oldDMG`：上一步的损伤

## 方程
1. 应变增量：`dinc = de`
2. 时间步长 t 的体积：`vold = particle_list(p)%VOL`
3. 当前体积：`particle_list(p)%VOL = vold * (1 + de(1) + de(2) + de(3))`
4. 偏应力更新（弹性）：`sd(i) = sd(i) + G2 * (dinc(i) - dem)`
5. 平均应力更新（弹性）：`dsm = K3 * dem`
6. 等效应力：`EquivalentStress = sqrt(3 * J2)`
7. 内能更新（无 EOS）：`iener = iener + 0.25 * ieinc * vavg`
8. 内能更新（有 EOS）：`iener = iener + ieinc`
9. 线性 EOS：`pnew = (A + B * specen) / (1 + B * dvol / vol0_)`
10. Mie-Gruneisen EOS：`pnew = (A + B * specen) / (1 + B * dvol / vol0_)`
11. JWL EOS：`pnew = (A + B * specen) / (1 + B * dvol / vol0_)`
12. 体积粘性：`bqf = den_ * DCell * DCell * bq1 * dd * dd - bq2 * den_ * DCell * cp * dd`
