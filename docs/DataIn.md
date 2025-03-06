# DataIn.f90

## Purpose
The `DataIn.f90` file is responsible for reading input data and initializing the simulation parameters. It contains the main subroutine `InputPara` and several other subroutines that handle different aspects of data input and initialization.

## Main Subroutines
1. `InputPara()`
   - Purpose: Input data both from file and by program.
   - Inputs: None
   - Outputs: Initialized simulation parameters

2. `GetData()`
   - Purpose: Get input data using the FFI module.
   - Inputs: None
   - Outputs: Data stored in the `ParticleData` module

3. `SetMaterial()`
   - Purpose: Get material data using the FFI module.
   - Inputs: None
   - Outputs: Data stored in the `mat_list`

4. `SetParticle()`
   - Purpose: Get particle data using the FFI module.
   - Inputs: None
   - Outputs: Data stored in the `particle_list`

5. `SetLoad()`
   - Purpose: Get load data by component or by node.
   - Inputs: None
   - Outputs: Load data stored in the `particle_list`

6. `SetVelocity()`
   - Purpose: Get initial velocity by component or by node.
   - Inputs: None
   - Outputs: Velocity data stored in the `particle_list`

7. `SetOnOff()`
   - Purpose: Get On/Off switch.
   - Inputs: None
   - Outputs: Logical value indicating the switch state

8. `SetResOption()`
   - Purpose: Get result option.
   - Inputs: None
   - Outputs: Integer value indicating the result option

9. `SetEos()`
   - Purpose: Set EOS parameter.
   - Inputs: None
   - Outputs: Data stored in the `mat_list`

10. `SetCurX()`
    - Purpose: Find particle ID according to coordinates.
    - Inputs: None
    - Outputs: Curve point data stored in the `CurvePoint`

11. `ErrorPt()`
    - Purpose: Output error message and line number.
    - Inputs: None
    - Outputs: Error message

12. `Initial()`
    - Purpose: Initialize specific variables used after reading all particle data.
    - Inputs: None
    - Outputs: Initialized variables

13. `SetDT()`
    - Purpose: Set computational time step size.
    - Inputs: None
    - Outputs: Time step size

14. `statinfo()`
    - Purpose: Output statistical data to message file.
    - Inputs: None
    - Outputs: Statistical data

15. `Setcontact()`
    - Purpose: Get contact data using the FFI module.
    - Inputs: None
    - Outputs: Contact data

## Parameters
1. `bodyCounter`: Body counter
2. `comCounter`: Component counter
3. `parCounter`: Particle counter
4. `jobname`: Job name
5. `fName`: File name
6. `FileInp`: Input data file
7. `FileOut`: Message file
8. `FileCurv`: Curve data file (TecPlot)
9. `FileAnim`: Animation data file (TecPlot)
10. `FileAnimColl`: Animation data file (ParaView)
11. `iow2`: Output unit for curve data
12. `iow1`: Output unit for animation data
13. `iow12`: Output unit for animation collection data
14. `iow03`: Output unit for energy plot data
15. `iow04`: Output unit for momentum plot data
16. `iow05`: Output unit for contact force plot data
17. `nbkw`: Number of keywords
18. `kw`: Keyword array
19. `Title`: Title of the simulation
20. `nb_particle`: Number of particles
21. `nb_mat`: Number of material sets
22. `nb_body`: Number of bodies
23. `nb_component`: Number of components
24. `nb_word`: Number of words in a line
25. `nb_read`: Number of words read in a line
26. `sss`: String buffer
27. `cmd_line`: Command line buffer
28. `CutOff`: Cutoff value for grid mass
29. `plot2dTrue`: Logical value indicating whether to plot 2D data
30. `plot2d`: Array for 2D plot data
31. `CurveOption`: Array for curve options
32. `CurvePoint`: Array for curve points
33. `nCurves`: Number of curves
34. `nAnimate`: Number of animation variables
35. `AnimOption`: Array for animation options
36. `WriteTecPlot`: Logical value indicating whether to write TecPlot data
37. `WriteParaView`: Logical value indicating whether to write ParaView data
38. `OutTime`: Time interval for plot data output
39. `ReportTime`: Time interval for status report
40. `MUSL`: Logical value indicating whether to use MUSL method
41. `USL`: Logical value indicating whether to use USL method
42. `USF`: Logical value indicating whether to use USF method
43. `GIMP`: Logical value indicating whether to use GIMP method
44. `Bspline`: Logical value indicating whether to use Bspline method
45. `SGMP`: Logical value indicating whether to use SGMP method
46. `contact`: Logical value indicating whether to use contact method
47. `Gravity`: Logical value indicating whether to set gravity
48. `DR_DAMPING`: Logical value indicating whether to use dynamic relaxation techniques for quasi-static solution
49. `istep`: Current time step
50. `DT`: Time step interval
51. `CurrentTime`: Time for current step
52. `EndTime`: Time for end of solution
53. `DTScale`: Time step size factor
54. `EngInternal`: Internal energy
55. `EngKinetic`: Current kinetic energy
56. `EngKinetic_prev`: Previous kinetic energy
57. `Momentum`: Array for momentum
58. `nb_component`: Number of components for contact simulation
59. `nb_body`: Number of bodies
60. `body_list`: Array for bodies
61. `particle_list`: Array for particles
62. `mat_list`: Array for materials
63. `DetoX`: Array for detonation points in X direction
64. `DetoY`: Array for detonation points in Y direction
65. `DetoZ`: Array for detonation points in Z direction
66. `nDeto`: Number of detonation points
67. `bq1`: Bulk viscosity coefficient 1
68. `bq2`: Bulk viscosity coefficient 2
69. `contact_type`: Contact type
70. `fricfa`: Frictional factor
71. `normbody`: Computational method of contact normal
72. `FileAnimNext`: Next animation data file
73. `stepc`: Step counter
74. `gridtype`: Grid type
75. `indent`: Indentation string
76. `iElemType`: Element type
77. `nb_elemnode`: Number of element nodes
78. `NumCell`: Number of cells
79. `CellsNode`: Array for cell nodes
80. `node_list`: Array for grid nodes
81. `nb_gridnode`: Number of grid nodes

## Equations
1. Volume at time step t: `vold = particle_list(p)%VOL`
2. Current volume: `particle_list(p)%VOL = vold * (1 + de(1) + de(2) + de(3))`
3. Strain increment: `dinc = de`
4. Bulk viscosity: `bqf = den_ * DCell * DCell * bq1 * dd * dd - bq2 * den_ * DCell * cp * dd`
5. Internal energy update (without EOS): `iener = iener + 0.25 * ieinc * vavg`
6. Internal energy update (with EOS): `iener = iener + ieinc`
7. Linear EOS: `pnew = (A + B * specen) / (1 + B * dvol / vol0_)`
8. Mie-Gruneisen EOS: `pnew = (A + B * specen) / (1 + B * dvol / vol0_)`
9. JWL EOS: `pnew = (A + B * specen) / (1 + B * dvol / vol0_)`
10. Equivalent stress: `EquivalentStress = sqrt(3 * J2)`
11. Deviatoric stress update (elastic): `sd(i) = sd(i) + G2 * (dinc(i) - dem)`
12. Mean stress update (elastic): `dsm = K3 * dem`
