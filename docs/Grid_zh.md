# Grid.f90

## 目的
`Grid.f90` 文件负责处理材料点法 (MPM) 仿真中的背景网格程序。它定义了计算网格，确定粒子所在的单元，并评估形函数及其导数。主要子程序包括 `InWhichCell`、`SetGridData`、`SetContact_GridNodeData`、`NShape`、`NShape_GIMP`、`FindInflNode` 和 `NShape_Bspline`。

## 主要子程序
1. `InWhichCell(xx)`
   - 目的：确定点 (xx, yy, zz) 所在的单元。
   - 输入：
     - `xx`：点的坐标
   - 输出：点所在的单元编号

2. `SetGridData()`
   - 目的：创建计算网格。
   - 输入：无
   - 输出：网格节点信息、单元节点矩阵、形函数参数

3. `SetContact_GridNodeData()`
   - 目的：创建用于模拟接触问题的计算网格。
   - 输入：无
   - 输出：接触网格节点信息

4. `NShape(node1, p, ider)`
   - 目的：评估与粒子 p 所在单元的节点相关的形函数及其导数。
   - 输入：
     - `node1`：粒子所在单元的第一个节点编号
     - `p`：粒子编号
     - `ider`：形函数和导数计算的标志 (0 - 仅形函数，1 - 仅导数，2 - 形函数及其导数)
   - 输出：形函数及其导数

5. `NShape_GIMP(p)`
   - 目的：评估与粒子 p 的影响节点相关的形函数及其导数 (GIMP)。
   - 输入：
     - `p`：粒子编号
   - 输出：形函数及其导数

6. `FindInflNode(p, icell)`
   - 目的：找到粒子 p 的影响节点。
   - 输入：
     - `p`：粒子编号
     - `icell`：包含粒子 p 的单元 ID
   - 输出：影响节点及粒子与节点之间的距离

7. `NShape_Bspline(p)`
   - 目的：评估与粒子 p 的影响节点相关的形函数及其导数 (B-spline 方法)。
   - 输入：
     - `p`：粒子编号
   - 输出：形函数及其导数

## 参数
1. `SpanX`：X 方向的计算区域
2. `SpanY`：Y 方向的计算区域
3. `SpanZ`：Z 方向的计算区域
4. `DCell`：网格节点间隔
5. `CutOff`：网格质量截止值
6. `nb_gridnode`：网格节点数量
7. `nb_centernode`：单元中心节点数量
8. `FixS`：固定边界条件
9. `NumCell`：单元数量
10. `NumCellx`：X 方向的单元数量
11. `NumCelly`：Y 方向的单元数量
12. `NumCellz`：Z 方向的单元数量
13. `NumCellxy`：XY 平面的单元数量
14. `CenterNumCell`：中心单元数量
15. `CenterNumCellx`：X 方向的中心单元数量
16. `CenterNumCelly`：Y 方向的中心单元数量
17. `CenterNumCellz`：Z 方向的中心单元数量
18. `CenterNumCellxy`：XY 平面的中心单元数量
19. `NGx`：X 方向的网格节点数量
20. `NGy`：Y 方向的网格节点数量
21. `NGz`：Z 方向的网格节点数量
22. `NGxy`：XY 平面的网格节点数量
23. `CellsNode`：单元节点数组
24. `CenterCellNode`：中心单元节点数组
25. `nb_InflNode`：影响节点数量
26. `centernb_InflNode`：中心影响节点数量
27. `InflNode`：每个粒子的影响节点列表
28. `CenterInflNode`：辅助网格的影响节点列表
29. `rpg`：粒子与节点之间的距离
30. `iJacobi`：Jacobi 行列式的逆
31. `iJacobi4`：Jacobi 行列式的逆除以 8
32. `iDCell`：网格节点间隔的逆
33. `SHP`：形函数
34. `DNDX`：形函数对 X 的导数
35. `DNDY`：形函数对 Y 的导数
36. `DNDZ`：形函数对 Z 的导数
37. `CSHP`：中心节点的形函数
38. `CDNDX`：中心节点的形函数对 X 的导数
39. `CDNDY`：中心节点的形函数对 Y 的导数
40. `CDNDZ`：中心节点的形函数对 Z 的导数
41. `SNX`：X 方向的符号常数
42. `SNY`：Y 方向的符号常数
43. `SNZ`：Z 方向的符号常数

## 方程
1. 自然坐标：`x = (pt%Xp - node%Xg) * iJacobi - 1d0`
2. 形函数：`SHP = sx * sy * sz * 0.125d0`
3. 形函数对 X 的导数：`DNDX = SNX * sy * sz * iJacobi4`
4. 形函数对 Y 的导数：`DNDY = SNY * sx * sz * iJacobi4`
5. 形函数对 Z 的导数：`DNDZ = SNZ * sx * sy * iJacobi4`
6. 粒子与节点之间的距离：`rpg(i,:) = (pt%Xp - node_list(InflNode(i))%Xg) * iDCell`
