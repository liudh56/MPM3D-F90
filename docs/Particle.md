# Particle.f90

This file contains the definition and initialization of particles and bodies used in the MPM3D code. It also includes subroutines for calculating energy and applying dynamic relaxation damping.

## Purpose

The `Particle.f90` file defines the data structures and subroutines related to particles and bodies in the MPM3D code. It includes the following main subroutines:
- `InitParticle`: Initializes the particle list.
- `InitBody`: Initializes the body list.
- `dynamicRelaxationDamping`: Applies dynamic relaxation damping for quasi-static solutions.
- `calcEnergy`: Calculates the kinetic and internal energy of the particles.

## Parameters

### Particle Type
- `XX(3)`: Particle position at time step t+1.
- `Xp(3)`: Particle position at time step t.
- `Xo(3)`: Particle position at time step t=0.
- `VXp(3)`: Particle velocity.
- `FXp(3)`: Load.
- `VOL`: Current volume.
- `sig_y`: Yield stress.
- `SM`: Mean stress.
- `Seqv`: Mises stress.
- `SDxx`, `SDyy`, `SDzz`, `SDxy`, `SDyz`, `SDxz`: Deviatoric stress components.
- `epeff`: Effective plastic strain.
- `celsius_t`: Celsius temperature.
- `SkipThis`: Flag for plotting less particles.
- `failure`: Flag for particle failure.
- `icell`: Cell number.
- `centericell`: Cell number in the auxiliary grid.
- `DMG`: Damage.
- `LT`: Lighting time for explosive simulation.
- `ie`: Internal energy.
- `mass`: Particle mass.
- `cp`: Sound speed.
- `q`: Bulk viscosity.

### Body Type
- `mat`: Material set number (1 ~ nb_mat).
- `comID`: Component set number (1 ~ nb_component).
- `Gravp(3)`: Gravity.
- `par_begin`: Beginning of particle index for the body.
- `par_end`: End of particle index for the body.

### Other Parameters
- `nb_particle`: Number of particles.
- `particle_list`: List of particles.
- `nb_component`: Number of components for contact simulation.
- `nb_body`: Number of bodies.
- `body_list`: List of bodies.
- `Version`: Version of MPM3D.
- `Release`: Release of MPM3D version.
- `Title`: Title of the simulation.
- `MUSL`: Flag for using MUSL method.
- `USL`: Flag for using USL method.
- `USF`: Flag for using USF method.
- `GIMP`: Flag for using GIMP method.
- `Bspline`: Flag for using Bspline method.
- `SGMP`: Flag for using SGMP method.
- `contact`: Flag for using contact method.
- `Gravity`: Flag for setting gravity.
- `DR_DAMPING`: Flag for using dynamic relaxation techniques for quasi-static solutions.
- `istep`: Current time step.
- `DT`: Time step interval.
- `CurrentTime`: Time for the current step.
- `EndTime`: Time for the end of the solution.
- `DTScale`: Time step size factor (<= 1.0).
- `EngInternal`: Internal energy.
- `EngKinetic`: Current kinetic energy.
- `EngKinetic_prev`: Previous kinetic energy.
- `Momentum`: Momentum.

## Subroutines

### InitParticle
Initializes the particle list by setting default values for particle properties.

### InitBody
Initializes the body list by setting default values for body properties.

### dynamicRelaxationDamping
Applies dynamic relaxation damping for quasi-static solutions by checking the change in kinetic energy and setting particle velocities to zero if the kinetic energy decreases.

### calcEnergy
Calculates the kinetic and internal energy of the particles by summing the contributions from each particle.

## Equations

### Kinetic Energy
The kinetic energy of a particle is calculated using the equation:
\[ \text{Kinetic Energy} = \frac{1}{2} m v^2 \]
where \( m \) is the mass of the particle and \( v \) is its velocity.

### Internal Energy
The internal energy of a particle is calculated by summing the internal energy contributions from each particle.

### Dynamic Relaxation Damping
The dynamic relaxation damping is applied by checking the change in kinetic energy:
\[ \Delta \text{Kinetic Energy} = \text{EngKinetic} - \text{EngKinetic_prev} \]
If the change in kinetic energy is less than or equal to zero, the particle velocities are set to zero.

