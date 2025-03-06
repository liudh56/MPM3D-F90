# Constitution.f90

## Purpose
The `Constitution.f90` file is responsible for updating the stresses in the material points using various constitutive models. It contains the main subroutine `Constitution` and several other subroutines that handle different material models and their corresponding equations of state (EOS).

## Main Subroutines
1. `Constitution(de, vort, b, p)`
   - Purpose: Update stresses using a constitutive model.
   - Inputs:
     - `de`: Strain increment (D11, D22, D33, 2D23, 2D13, 2D12) * DT
     - `vort`: Vorticity * DT (W32, W13, W21) * DT
     - `b`: Body index
     - `p`: Particle index
   - Outputs: Stress components

2. `sigrot(vort, sig, sm, sd)`
   - Purpose: Rotate stresses and update mean stress and deviatoric stresses.
   - Inputs:
     - `vort`: Vorticity increments (W32, W13, W21) * DT
     - `sig`: Cauchy stresses at time step t (S11, S22, S33, S23, S13, S12)
   - Outputs: Rotated Cauchy stresses, deviatoric stress, and mean stress

3. `elastic_devi()`
   - Purpose: Update deviatoric stress using elastic relation.
   - Outputs: Deviatoric stress components

4. `elastic_p()`
   - Purpose: Update pressure using elastic relation.
   - Outputs: Mean stress (pressure)

5. `lieupd()`
   - Purpose: Update energy for material models without EOS.
   - Outputs: Internal energy

6. `hieupd(failure)`
   - Purpose: Update energy for material models with EOS.
   - Inputs: `failure` - Failure state of the particle
   - Outputs: Internal energy

7. `EquivalentStress()`
   - Purpose: Calculate the equivalent stress sqrt(3*J2).
   - Outputs: Equivalent stress

8. `M3DM1()`
   - Purpose: Elastic material model.
   - Outputs: Updated stress components

9. `M3DM2()`
   - Purpose: Elastic-perfectly plastic material model.
   - Outputs: Updated stress components

10. `M3DM3()`
    - Purpose: Isotropic hardening plastic material model.
    - Outputs: Updated stress components

11. `M3DM4(mat, DT, tmprt)`
    - Purpose: Johnson-Cook plastic material model.
    - Inputs:
      - `mat`: Material properties
      - `DT`: Time step size
      - `tmprt`: Temperature
    - Outputs: Updated stress components

12. `M3DM5(mat, DT)`
    - Purpose: Simplified Johnson-Cook plastic material model.
    - Inputs:
      - `mat`: Material properties
      - `DT`: Time step size
    - Outputs: Updated stress components

13. `M3DM6(ltim, ctim, dvelo)`
    - Purpose: High explosive burn material model.
    - Inputs:
      - `ltim`: Lighting time
      - `ctim`: Current time
      - `dvelo`: Detonation velocity
    - Outputs: Burn fraction

14. `M3DM7()`
    - Purpose: Null material model.
    - Outputs: Zero deviatoric stress

15. `M3DM8(mat, DT, tmprt, failure)`
    - Purpose: Johnson-Cook plastic material model with failure.
    - Inputs:
      - `mat`: Material properties
      - `DT`: Time step size
      - `tmprt`: Temperature
      - `failure`: Failure state of the particle
    - Outputs: Updated stress components

16. `M3DM9(mat, DT)`
    - Purpose: Perfect elastic-plastic Drucker-Prager model for soil.
    - Inputs:
      - `mat`: Material properties
      - `DT`: Time step size
    - Outputs: Updated stress components

17. `seleos(failure)`
    - Purpose: Update pressure and energy using appropriate EOS.
    - Inputs: `failure` - Failure state of the particle
    - Outputs: Updated pressure and internal energy

18. `eos1(failure)`
    - Purpose: Linear Equation of State.
    - Inputs: `failure` - Failure state of the particle
    - Outputs: Updated pressure and internal energy

19. `eos2(failure)`
    - Purpose: Mie-Gruneisen Equation of State.
    - Inputs: `failure` - Failure state of the particle
    - Outputs: Updated pressure and internal energy

20. `eos3(failure)`
    - Purpose: JWL Equation of State.
    - Inputs: `failure` - Failure state of the particle
    - Outputs: Updated pressure and internal energy

21. `bulkq()`
    - Purpose: Calculate bulk viscosity.
    - Outputs: Bulk viscosity force

## Parameters
1. `mid`: Material set ID
2. `etype_`: Type of EOS (Equation of State)
3. `mtype_`: Type of material model
4. `young_`: Young's modulus
5. `poisson_`: Poisson's ratio
6. `yield0_`: Initial yield stress
7. `tangmod_`: Tangential modulus
8. `den0_`: Initial density
9. `den_`: Current density
10. `vold`: Volume at step n
11. `vol_`: Current volume
12. `vol0_`: Initial volume
13. `dvol`: 0.5 * Volume increment
14. `dinc`: Strain increment
15. `sm`: Mean stress
16. `smold`: Mean stress of last step
17. `sd`: Deviatoric stress
18. `sig`: Stress components
19. `sold`: Deviatoric stress of step n
20. `dsm`: Increment of mean stress
21. `bqf`: Bulk viscosity force
22. `seqv`: Equivalent stress
23. `epeff_`: Effective plastic strain
24. `sig_y_`: Current yield stress
25. `depeff`: Increment of equivalent plastic strain
26. `ratio`: Ratio for hardening calculation
27. `G2`: 2 * Shear modulus
28. `K3`: 3 * Bulk modulus
29. `PlaMod`: Plastic hardening modulus
30. `specheat_`: Specific heat
31. `tmprt`: Temperature
32. `iener`: Internal energy
33. `specen`: Internal energy per initial volume
34. `ieinc`: Increment of internal energy
35. `mu`: (den_ / den0_) - 1
36. `rv`: Relative volume (vol_ / vol0_)
37. `bfac`: Burn fraction
38. `cp`: Sound speed
39. `DMG`: Damage
40. `oldDMG`: Damage of last step

## Equations
1. Strain increment: `dinc = de`
2. Volume at time step t: `vold = particle_list(p)%VOL`
3. Current volume: `particle_list(p)%VOL = vold * (1 + de(1) + de(2) + de(3))`
4. Deviatoric stress update (elastic): `sd(i) = sd(i) + G2 * (dinc(i) - dem)`
5. Mean stress update (elastic): `dsm = K3 * dem`
6. Equivalent stress: `EquivalentStress = sqrt(3 * J2)`
7. Internal energy update (without EOS): `iener = iener + 0.25 * ieinc * vavg`
8. Internal energy update (with EOS): `iener = iener + ieinc`
9. Linear EOS: `pnew = (A + B * specen) / (1 + B * dvol / vol0_)`
10. Mie-Gruneisen EOS: `pnew = (A + B * specen) / (1 + B * dvol / vol0_)`
11. JWL EOS: `pnew = (A + B * specen) / (1 + B * dvol / vol0_)`
12. Bulk viscosity: `bqf = den_ * DCell * DCell * bq1 * dd * dd - bq2 * den_ * DCell * cp * dd`
