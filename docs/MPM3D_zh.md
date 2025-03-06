# MPM3D.f90

## 目的
`MPM3D.f90` 文件是 MPM3D-F90 项目的主程序文件。它通过调用其他模块的各种子程序来协调整个模拟过程。该文件中的主要子程序是 `MPM3D` 程序，它控制模拟的流程。

## 主要子程序
1. `MPM3D`
   - 目的：控制模拟流程的主程序。
   - 输入：无
   - 输出：模拟结果

## 参数
1. `t_begin`：时间步开始时间
2. `t_end`：时间步结束时间
3. `t_cpu`：总 CPU 时间
4. `TPZC`：每个粒子循环的时间
5. `t_bg`：预处理开始时间
6. `t_ed`：预处理结束时间
7. `t_elapsed`：经过的时间
8. `t0`：求解开始时间
9. `t1`：求解结束时间
10. `t_sec`：经过时间的秒部分
11. `t_s`：CPU 时间的秒部分
12. `t_min`：经过时间的分钟部分
13. `t_m`：CPU 时间的分钟部分
14. `iplotstep`：绘图步计数器
15. `plt`：绘图时间
16. `prt`：报告时间

## 方程
1. 动能：`EngKinetic = DOT_PRODUCT(pt%VXp, pt%VXp) * pt%mass * 0.5d0`
2. 内能：`EngInternal = EngInternal + pt%ie`
3. 动量：`Momentum = Momentum + pt%mass * pt%VXp`
4. 每个粒子循环的时间：`TPZC = t_cpu / nb_particle / istep`
