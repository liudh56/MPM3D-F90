# MPM3D.f90

## Purpose
The `MPM3D.f90` file is the main program file for the MPM3D-F90 project. It orchestrates the overall simulation process by calling various subroutines from other modules. The main subroutine in this file is the `MPM3D` program, which controls the flow of the simulation.

## Main Subroutines
1. `MPM3D`
   - Purpose: Main program that controls the flow of the simulation.
   - Inputs: None
   - Outputs: Simulation results

## Parameters
1. `t_begin`: Start time for a time step
2. `t_end`: End time for a time step
3. `t_cpu`: Total CPU time
4. `TPZC`: Time per particle cycle
5. `t_bg`: Start time for preprocessing
6. `t_ed`: End time for preprocessing
7. `t_elapsed`: Elapsed time
8. `t0`: Start time for solving
9. `t1`: End time for solving
10. `t_sec`: Seconds part of the elapsed time
11. `t_s`: Seconds part of the CPU time
12. `t_min`: Minutes part of the elapsed time
13. `t_m`: Minutes part of the CPU time
14. `iplotstep`: Plot step counter
15. `plt`: Plot time
16. `prt`: Report time

## Equations
1. Kinetic energy: `EngKinetic = DOT_PRODUCT(pt%VXp, pt%VXp) * pt%mass * 0.5d0`
2. Internal energy: `EngInternal = EngInternal + pt%ie`
3. Momentum: `Momentum = Momentum + pt%mass * pt%VXp`
4. Time per particle cycle: `TPZC = t_cpu / nb_particle / istep`
