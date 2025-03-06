# DataOut.f90

## Purpose
The `DataOut.f90` file is responsible for outputting the results of the simulation to various files. It contains the main subroutines `OutCurve`, `OutAnim`, `OutAnimPV`, and `OutGrid`, which handle different aspects of data output.

## Main Subroutines
1. `OutCurve()`
   - Purpose: Output result data for plotting time-history curves.
   - Inputs: None
   - Outputs: Time-history curve data

2. `OutAnim()`
   - Purpose: Output result data for animation in TecPlot format.
   - Inputs: None
   - Outputs: Animation data in TecPlot format

3. `OutAnimPV(iStepNo)`
   - Purpose: Output result data for animation in ParaView format.
   - Inputs:
     - `iStepNo`: Step number
   - Outputs: Animation data in ParaView format

4. `OutGrid()`
   - Purpose: Write computational grid to output file viewable by ParaView.
   - Inputs: None
   - Outputs: Computational grid data in ParaView format

## Parameters
1. `nCvs`: Number of curves
2. `nAnm`: Number of animations
3. `nVariables`: Number of variables
4. `OutputName`: Array of output variable names
5. `WriteTecPlot`: Logical value indicating whether to write TecPlot data
6. `WriteParaView`: Logical value indicating whether to write ParaView data
7. `OutTime`: Time interval for plot data output
8. `ReportTime`: Time interval for status report
9. `nCurves`: Number of curves defined
10. `nAnimate`: Number of animation variables defined
11. `MaxCurves`: Maximum number of curves
12. `MaxAnim`: Maximum number of animation variables
13. `CurveOption`: Array of curve options
14. `AnimOption`: Array of animation options
15. `CurvePoint`: Array of curve points
16. `plot2d`: Array for 2D plot data
17. `plot2dTrue`: Logical value indicating whether to plot 2D data

## Equations
1. Kinetic energy: `EngKinetic = DOT_PRODUCT(pt%VXp, pt%VXp) * pt%mass * 0.5d0`
2. Internal energy: `EngInternal = EngInternal + pt%ie`
3. Momentum: `Momentum = Momentum + pt%mass * pt%VXp`
4. Time-history curve data: `write(iow2, "(e12.4)", advance='no') CurrentTime`
5. Animation data in TecPlot format: `write(iow1, "(3e12.4)", advance='no') pt%Xp`
6. Animation data in ParaView format: `write(iow11, "(A,e12.4,A)") indent//'<DataSet timestep="', CurrentTime, '" group="" part="0" file="'//trim(FileAnimNext)//'"/>'`
7. Computational grid data in ParaView format: `write(iow10, "(a)", advance='no') indent//indent//indent//indent`
