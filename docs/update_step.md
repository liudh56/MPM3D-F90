# update_step.f90

## Purpose
The `update_step.f90` file is responsible for updating the grid momentum, particle stress, and particle position during each time step of the simulation. It contains several subroutines that handle different aspects of the update process.

## Main Subroutines
1. `GridMomentumInitial()`
   - Purpose: Initialize the grid momentum by mapping particle variables to the grid nodes.
   - Inputs: None
   - Outputs: Updated grid nodal masses and momenta

2. `ParticleStressUpdate()`
   - Purpose: Calculate the strain rate and spin tensor, and update stresses using a constitutive model.
   - Inputs: None
   - Outputs: Updated particle stresses

3. `GridMomentumUpdate()`
   - Purpose: Calculate the grid nodal forces.
   - Inputs: None
   - Outputs: Updated grid nodal forces

4. `IntegrateMomentum()`
   - Purpose: Integrate the momentum equations on the computational grid and apply boundary conditions.
   - Inputs: None
   - Outputs: Updated grid nodal momenta

5. `Lagr_NodContact()`
   - Purpose: Establish nodal contact criteria, correct normal vectors, and apply contact forces.
   - Inputs: None
   - Outputs: Updated grid nodal forces and momenta

6. `ParticlePositionUpdate()`
   - Purpose: Update particle positions and velocities.
   - Inputs: None
   - Outputs: Updated particle positions and velocities

7. `GridMomentumMUSL()`
   - Purpose: Recalculate the grid node momentum by mapping updated particle information and apply boundary conditions.
   - Inputs: None
   - Outputs: Updated grid nodal momenta

8. `ApplyBoundaryConditions()`
   - Purpose: Apply boundary conditions to the grid nodes.
   - Inputs: None
   - Outputs: Updated grid nodal momenta

## Parameters
1. `nb_body`: Number of bodies
2. `nb_particle`: Number of particles
3. `nb_gridnode`: Number of grid nodes
4. `nb_centernode`: Number of center nodes
5. `nb_InflNode`: Number of influence nodes
6. `centernb_InflNode`: Number of center influence nodes
7. `DCell`: Grid cell size
8. `DT`: Time step size
9. `CutOff`: Cutoff value for grid mass
10. `tot_cont_for`: Total contact force
11. `fricfa`: Frictional factor
12. `normbody`: Computational method of contact normal
13. `InflNode`: Array for influence nodes
14. `CenterInflNode`: Array for center influence nodes
15. `SHP`: Shape function values
16. `DNDX`: Shape function derivatives in x direction
17. `DNDY`: Shape function derivatives in y direction
18. `DNDZ`: Shape function derivatives in z direction
19. `CSHP`: Center shape function values
20. `CDNDX`: Center shape function derivatives in x direction
21. `CDNDY`: Center shape function derivatives in y direction
22. `CDNDZ`: Center shape function derivatives in z direction

## Equations
1. Strain increment: `de = de * DT`
   - This equation updates the strain increment by multiplying it with the time step size (DT).
2. Vorticity increment: `vort = vort * DT / 2`
   - This equation updates the vorticity increment by multiplying it with half the time step size (DT/2).
3. Nodal mass: `gd%Mg = gd%Mg + shm`
   - This equation updates the nodal mass by adding the shape function mass (shm) to the existing nodal mass (gd%Mg).
4. Nodal momentum: `gd%PXg = gd%PXg + pt%VXp*shm`
   - This equation updates the nodal momentum by adding the product of particle velocity (pt%VXp) and shape function mass (shm) to the existing nodal momentum (gd%PXg).
5. Nodal force: `gd%FXg = gd%FXg + f_int + f_ext`
   - This equation updates the nodal force by adding the internal force (f_int) and external force (f_ext) to the existing nodal force (gd%FXg).
6. Internal force: `f_int = - (sxx*DNDXn + sxy*DNDYn + sxz*DNDZn)*vol_`
   - This equation calculates the internal force by summing the products of stress components (sxx, sxy, sxz) and shape function derivatives (DNDXn, DNDYn, DNDZn), and multiplying by the volume (vol_).
7. External force: `f_ext = fx*SHPn`
   - This equation calculates the external force by multiplying the external force component (fx) with the shape function value (SHPn).
8. Nodal velocity: `vgx = gd%PXg / gd%Mg`
   - This equation calculates the nodal velocity by dividing the nodal momentum (gd%PXg) by the nodal mass (gd%Mg).
9. Particle position update: `pt%XX = xx + vx * DT`
   - This equation updates the particle position by adding the product of particle velocity (vx) and time step size (DT) to the existing particle position (xx).
10. Particle velocity update: `pt%VXp = pt%VXp + ax * DT`
    - This equation updates the particle velocity by adding the product of particle acceleration (ax) and time step size (DT) to the existing particle velocity (pt%VXp).
11. Contact force: `nomforce = crit / tt`
    - This equation calculates the normal contact force by dividing the critical value (crit) by the time step size (tt).
12. Tangent contact force: `fstick = cforce - nomforce*CP1%ndir`
    - This equation calculates the tangent contact force by subtracting the product of normal contact force (nomforce) and contact normal direction (CP1%ndir) from the contact force (cforce).
13. Slip contact force: `val_fslip = fricfa*abs(nomforce)`
    - This equation calculates the slip contact force by multiplying the frictional factor (fricfa) with the absolute value of the normal contact force (nomforce).
14. Contact force adjustment: `cforce = nomforce*CP1%ndir + val_fslip*(fstick /val_fstick)`
    - This equation adjusts the contact force by adding the product of normal contact force (nomforce) and contact normal direction (CP1%ndir) to the product of slip contact force (val_fslip) and the ratio of tangent contact force (fstick) to the slip contact force (val_fstick).
