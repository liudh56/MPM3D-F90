# DataOut.f90

## 目的
`DataOut.f90` 文件负责将仿真结果输出到各种文件中。它包含主要子程序 `OutCurve`、`OutAnim`、`OutAnimPV` 和 `OutGrid`，这些子程序处理数据输出的不同方面。

## 主要子程序
1. `OutCurve()`
   - 目的：输出用于绘制时间历史曲线的结果数据。
   - 输入：无
   - 输出：时间历史曲线数据

2. `OutAnim()`
   - 目的：输出用于 TecPlot 格式动画的结果数据。
   - 输入：无
   - 输出：TecPlot 格式的动画数据

3. `OutAnimPV(iStepNo)`
   - 目的：输出用于 ParaView 格式动画的结果数据。
   - 输入：
     - `iStepNo`：步数
   - 输出：ParaView 格式的动画数据

4. `OutGrid()`
   - 目的：将计算网格写入输出文件，以便 ParaView 查看。
   - 输入：无
   - 输出：ParaView 格式的计算网格数据

## 参数
1. `nCvs`：曲线数量
2. `nAnm`：动画数量
3. `nVariables`：变量数量
4. `OutputName`：输出变量名称数组
5. `WriteTecPlot`：指示是否写入 TecPlot 数据的逻辑值
6. `WriteParaView`：指示是否写入 ParaView 数据的逻辑值
7. `OutTime`：绘图数据输出的时间间隔
8. `ReportTime`：状态报告的时间间隔
9. `nCurves`：定义的曲线数量
10. `nAnimate`：定义的动画变量数量
11. `MaxCurves`：最大曲线数量
12. `MaxAnim`：最大动画变量数量
13. `CurveOption`：曲线选项数组
14. `AnimOption`：动画选项数组
15. `CurvePoint`：曲线点数组
16. `plot2d`：2D 绘图数据数组
17. `plot2dTrue`：指示是否绘制 2D 数据的逻辑值

## 方程
1. 动能：`EngKinetic = DOT_PRODUCT(pt%VXp, pt%VXp) * pt%mass * 0.5d0`
2. 内能：`EngInternal = EngInternal + pt%ie`
3. 动量：`Momentum = Momentum + pt%mass * pt%VXp`
4. 时间历史曲线数据：`write(iow2, "(e12.4)", advance='no') CurrentTime`
5. TecPlot 格式的动画数据：`write(iow1, "(3e12.4)", advance='no') pt%Xp`
6. ParaView 格式的动画数据：`write(iow11, "(A,e12.4,A)") indent//'<DataSet timestep="', CurrentTime, '" group="" part="0" file="'//trim(FileAnimNext)//'"/>'`
7. ParaView 格式的计算网格数据：`write(iow10, "(a)", advance='no') indent//indent//indent//indent`
