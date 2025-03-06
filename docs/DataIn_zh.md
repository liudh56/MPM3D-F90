# DataIn.f90

## 目的
`DataIn.f90` 文件负责读取输入数据并初始化仿真参数。它包含主子程序 `InputPara` 和其他几个处理数据输入和初始化不同方面的子程序。

## 主要子程序
1. `InputPara()`
   - 目的：从文件和程序中输入数据。
   - 输入：无
   - 输出：初始化的仿真参数

2. `GetData()`
   - 目的：使用 FFI 模块获取输入数据。
   - 输入：无
   - 输出：存储在 `ParticleData` 模块中的数据

3. `SetMaterial()`
   - 目的：使用 FFI 模块获取材料数据。
   - 输入：无
   - 输出：存储在 `mat_list` 中的数据

4. `SetParticle()`
   - 目的：使用 FFI 模块获取粒子数据。
   - 输入：无
   - 输出：存储在 `particle_list` 中的数据

5. `SetLoad()`
   - 目的：按组件或节点获取负载数据。
   - 输入：无
   - 输出：存储在 `particle_list` 中的负载数据

6. `SetVelocity()`
   - 目的：按组件或节点获取初始速度。
   - 输入：无
   - 输出：存储在 `particle_list` 中的速度数据

7. `SetOnOff()`
   - 目的：获取开/关开关。
   - 输入：无
   - 输出：指示开关状态的逻辑值

8. `SetResOption()`
   - 目的：获取结果选项。
   - 输入：无
   - 输出：指示结果选项的整数值

9. `SetEos()`
   - 目的：设置 EOS 参数。
   - 输入：无
   - 输出：存储在 `mat_list` 中的数据

10. `SetCurX()`
    - 目的：根据坐标找到粒子 ID。
    - 输入：无
    - 输出：存储在 `CurvePoint` 中的曲线点数据

11. `ErrorPt()`
    - 目的：输出错误消息和行号。
    - 输入：无
    - 输出：错误消息

12. `Initial()`
    - 目的：在读取所有粒子数据后初始化特定变量。
    - 输入：无
    - 输出：初始化的变量

13. `SetDT()`
    - 目的：设置计算时间步长。
    - 输入：无
    - 输出：时间步长

14. `statinfo()`
    - 目的：将统计数据输出到消息文件。
    - 输入：无
    - 输出：统计数据

15. `Setcontact()`
    - 目的：使用 FFI 模块获取接触数据。
    - 输入：无
    - 输出：接触数据

## 参数
1. `bodyCounter`: 体计数器
2. `comCounter`: 组件计数器
3. `parCounter`: 粒子计数器
4. `jobname`: 作业名称
5. `fName`: 文件名
6. `FileInp`: 输入数据文件
7. `FileOut`: 消息文件
8. `FileCurv`: 曲线数据文件（TecPlot）
9. `FileAnim`: 动画数据文件（TecPlot）
10. `FileAnimColl`: 动画数据文件（ParaView）
11. `iow2`: 曲线数据的输出单元
12. `iow1`: 动画数据的输出单元
13. `iow12`: 动画集合数据的输出单元
14. `iow03`: 能量图数据的输出单元
15. `iow04`: 动量图数据的输出单元
16. `iow05`: 接触力图数据的输出单元
17. `nbkw`: 关键字数量
18. `kw`: 关键字数组
19. `Title`: 仿真标题
20. `nb_particle`: 粒子数量
21. `nb_mat`: 材料集数量
22. `nb_body`: 体数量
23. `nb_component`: 组件数量
24. `nb_word`: 行中的单词数量
25. `nb_read`: 行中读取的单词数量
26. `sss`: 字符串缓冲区
27. `cmd_line`: 命令行缓冲区
28. `CutOff`: 网格质量的截止值
29. `plot2dTrue`: 指示是否绘制 2D 数据的逻辑值
30. `plot2d`: 2D 绘图数据数组
31. `CurveOption`: 曲线选项数组
32. `CurvePoint`: 曲线点数组
33. `nCurves`: 曲线数量
34. `nAnimate`: 动画变量数量
35. `AnimOption`: 动画选项数组
36. `WriteTecPlot`: 指示是否写入 TecPlot 数据的逻辑值
37. `WriteParaView`: 指示是否写入 ParaView 数据的逻辑值
38. `OutTime`: 绘图数据输出的时间间隔
39. `ReportTime`: 状态报告的时间间隔
40. `MUSL`: 指示是否使用 MUSL 方法的逻辑值
41. `USL`: 指示是否使用 USL 方法的逻辑值
42. `USF`: 指示是否使用 USF 方法的逻辑值
43. `GIMP`: 指示是否使用 GIMP 方法的逻辑值
44. `Bspline`: 指示是否使用 Bspline 方法的逻辑值
45. `SGMP`: 指示是否使用 SGMP 方法的逻辑值
46. `contact`: 指示是否使用接触方法的逻辑值
47. `Gravity`: 指示是否设置重力的逻辑值
48. `DR_DAMPING`: 指示是否使用动态松弛技术进行准静态解的逻辑值
49. `istep`: 当前时间步
50. `DT`: 时间步长间隔
51. `CurrentTime`: 当前步的时间
52. `EndTime`: 解的结束时间
53. `DTScale`: 时间步长大小因子
54. `EngInternal`: 内能
55. `EngKinetic`: 当前动能
56. `EngKinetic_prev`: 先前的动能
57. `Momentum`: 动量数组
58. `nb_component`: 接触仿真的组件数量
59. `nb_body`: 体数量
60. `body_list`: 体数组
61. `particle_list`: 粒子数组
62. `mat_list`: 材料数组
63. `DetoX`: X 方向的爆轰点数组
64. `DetoY`: Y 方向的爆轰点数组
65. `DetoZ`: Z 方向的爆轰点数组
66. `nDeto`: 爆轰点数量
67. `bq1`: 体积粘性系数 1
68. `bq2`: 体积粘性系数 2
69. `contact_type`: 接触类型
70. `fricfa`: 摩擦因子
71. `normbody`: 接触法线的计算方法
72. `FileAnimNext`: 下一个动画数据文件
73. `stepc`: 步计数器
74. `gridtype`: 网格类型
75. `indent`: 缩进字符串
76. `iElemType`: 元素类型
77. `nb_elemnode`: 元素节点数量
78. `NumCell`: 单元数量
79. `CellsNode`: 单元节点数组
80. `node_list`: 网格节点数组
81. `nb_gridnode`: 网格节点数量

## 方程
1. 时间步 t 的体积：`vold = particle_list(p)%VOL`
2. 当前体积：`particle_list(p)%VOL = vold * (1 + de(1) + de(2) + de(3))`
3. 应变增量：`dinc = de`
4. 体积粘性：`bqf = den_ * DCell * DCell * bq1 * dd * dd - bq2 * den_ * DCell * cp * dd`
5. 内能更新（无 EOS）：`iener = iener + 0.25 * ieinc * vavg`
6. 内能更新（有 EOS）：`iener = iener + ieinc`
7. 线性 EOS：`pnew = (A + B * specen) / (1 + B * dvol / vol0_)`
8. Mie-Gruneisen EOS：`pnew = (A + B * specen) / (1 + B * dvol / vol0_)`
9. JWL EOS：`pnew = (A + B * specen) / (1 + B * dvol / vol0_)`
10. 等效应力：`EquivalentStress = sqrt(3 * J2)`
11. 偏应力更新（弹性）：`sd(i) = sd(i) + G2 * (dinc(i) - dem)`
12. 平均应力更新（弹性）：`dsm = K3 * dem`
