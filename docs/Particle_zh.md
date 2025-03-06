# Particle.f90

该文件包含 MPM3D 代码中粒子和体的定义和初始化。它还包括计算能量和应用动态松弛阻尼的子程序。

## 目的

`Particle.f90` 文件定义了 MPM3D 代码中与粒子和体相关的数据结构和子程序。它包括以下主要子程序：
- `InitParticle`：初始化粒子列表。
- `InitBody`：初始化体列表。
- `dynamicRelaxationDamping`：应用动态松弛阻尼以获得准静态解。
- `calcEnergy`：计算粒子的动能和内能。

## 参数

### 粒子类型
- `XX(3)`：时间步 t+1 的粒子位置。
- `Xp(3)`：时间步 t 的粒子位置。
- `Xo(3)`：时间步 t=0 的粒子位置。
- `VXp(3)`：粒子速度。
- `FXp(3)`：载荷。
- `VOL`：当前体积。
- `sig_y`：屈服应力。
- `SM`：平均应力。
- `Seqv`：Mises 应力。
- `SDxx`、`SDyy`、`SDzz`、`SDxy`、`SDyz`、`SDxz`：偏应力分量。
- `epeff`：有效塑性应变。
- `celsius_t`：摄氏温度。
- `SkipThis`：用于绘制较少粒子的标志。
- `failure`：粒子失效标志。
- `icell`：单元编号。
- `centericell`：辅助网格中的单元编号。
- `DMG`：损伤。
- `LT`：爆炸模拟的点火时间。
- `ie`：内能。
- `mass`：粒子质量。
- `cp`：声速。
- `q`：体积粘性。

### 体类型
- `mat`：材料集编号（1 ~ nb_mat）。
- `comID`：组件集编号（1 ~ nb_component）。
- `Gravp(3)`：重力。
- `par_begin`：体的粒子索引起始位置。
- `par_end`：体的粒子索引结束位置。

### 其他参数
- `nb_particle`：粒子数量。
- `particle_list`：粒子列表。
- `nb_component`：接触仿真的组件数量。
- `nb_body`：体数量。
- `body_list`：体列表。
- `Version`：MPM3D 版本。
- `Release`：MPM3D 版本发布。
- `Title`：仿真标题。
- `MUSL`：是否使用 MUSL 方法的标志。
- `USL`：是否使用 USL 方法的标志。
- `USF`：是否使用 USF 方法的标志。
- `GIMP`：是否使用 GIMP 方法的标志。
- `Bspline`：是否使用 Bspline 方法的标志。
- `SGMP`：是否使用 SGMP 方法的标志。
- `contact`：是否使用接触方法的标志。
- `Gravity`：是否设置重力的标志。
- `DR_DAMPING`：是否使用动态松弛技术进行准静态解的标志。
- `istep`：当前时间步。
- `DT`：时间步长间隔。
- `CurrentTime`：当前步的时间。
- `EndTime`：解的结束时间。
- `DTScale`：时间步长大小因子（<= 1.0）。
- `EngInternal`：内能。
- `EngKinetic`：当前动能。
- `EngKinetic_prev`：先前的动能。
- `Momentum`：动量。

## 子程序

### InitParticle
通过设置粒子属性的默认值来初始化粒子列表。

### InitBody
通过设置体属性的默认值来初始化体列表。

### dynamicRelaxationDamping
通过检查动能的变化并在动能减少时将粒子速度设置为零来应用动态松弛阻尼以获得准静态解。

### calcEnergy
通过汇总每个粒子的贡献来计算粒子的动能和内能。

## 方程

### 动能
粒子的动能使用以下方程计算：
\[ \text{Kinetic Energy} = \frac{1}{2} m v^2 \]
其中 \( m \) 是粒子的质量，\( v \) 是其速度。

### 内能
通过汇总每个粒子的内能贡献来计算粒子的内能。

### 动态松弛阻尼
通过检查动能的变化来应用动态松弛阻尼：
\[ \Delta \text{Kinetic Energy} = \text{EngKinetic} - \text{EngKinetic_prev} \]
如果动能的变化小于或等于零，则将粒子速度设置为零。
