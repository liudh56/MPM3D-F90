# Grid.f90

## Purpose
The `Grid.f90` file is responsible for handling the background grid procedures in the Material Point Method (MPM) simulation. It defines the computational grid, determines the cell in which a particle is located, and evaluates the shape functions and their derivatives. The main subroutines include `InWhichCell`, `SetGridData`, `SetContact_GridNodeData`, `NShape`, `NShape_GIMP`, `FindInflNode`, and `NShape_Bspline`.

## Main Subroutines
1. `InWhichCell(xx)`
   - Purpose: Determine which cell the point (xx, yy, zz) is located in.
   - Inputs:
     - `xx`: Coordinates of a point
   - Outputs: Cell number in which the point is located

2. `SetGridData()`
   - Purpose: Create computational grid.
   - Inputs: None
   - Outputs: Grid node information, cell node matrix, shape function parameters

3. `SetContact_GridNodeData()`
   - Purpose: Create computational grid for simulating contact problems.
   - Inputs: None
   - Outputs: Contact grid node information

4. `NShape(node1, p, ider)`
   - Purpose: Evaluate the shape functions and/or their derivatives at particle p associated with nodes of the cell in which the particle p is located.
   - Inputs:
     - `node1`: Number of the first node of the cell in which the particle is located
     - `p`: Particle number
     - `ider`: Flag for shape function and derivative calculation (0 - shape functions only, 1 - derivatives only, 2 - both shape functions and their derivatives)
   - Outputs: Shape functions and their derivatives

5. `NShape_GIMP(p)`
   - Purpose: Evaluate the shape functions and/or their derivatives at particle p associated with p's influence nodes (GIMP).
   - Inputs:
     - `p`: Particle number
   - Outputs: Shape functions and their derivatives

6. `FindInflNode(p, icell)`
   - Purpose: Find influence nodes for particle p.
   - Inputs:
     - `p`: Particle number
     - `icell`: Cell ID containing particle p
   - Outputs: Influence nodes and distances between particle and node

7. `NShape_Bspline(p)`
   - Purpose: Evaluate the shape functions and/or their derivatives at particle p associated with p's influence nodes (B-spline method).
   - Inputs:
     - `p`: Particle number
   - Outputs: Shape functions and their derivatives

## Parameters
1. `SpanX`: Computational region in the X direction
2. `SpanY`: Computational region in the Y direction
3. `SpanZ`: Computational region in the Z direction
4. `DCell`: Grid node interval
5. `CutOff`: Grid mass cutoff value
6. `nb_gridnode`: Number of grid nodes
7. `nb_centernode`: Number of center nodes of the cell
8. `FixS`: Fixed boundary conditions
9. `NumCell`: Number of cells
10. `NumCellx`: Number of cells in the X direction
11. `NumCelly`: Number of cells in the Y direction
12. `NumCellz`: Number of cells in the Z direction
13. `NumCellxy`: Number of cells in the XY plane
14. `CenterNumCell`: Number of center cells
15. `CenterNumCellx`: Number of center cells in the X direction
16. `CenterNumCelly`: Number of center cells in the Y direction
17. `CenterNumCellz`: Number of center cells in the Z direction
18. `CenterNumCellxy`: Number of center cells in the XY plane
19. `NGx`: Number of grid nodes in the X direction
20. `NGy`: Number of grid nodes in the Y direction
21. `NGz`: Number of grid nodes in the Z direction
22. `NGxy`: Number of grid nodes in the XY plane
23. `CellsNode`: Array for cell nodes
24. `CenterCellNode`: Array for center cell nodes
25. `nb_InflNode`: Number of influence nodes
26. `centernb_InflNode`: Number of center influence nodes
27. `InflNode`: Influence node list for each particle
28. `CenterInflNode`: Influence node list for the auxiliary grid
29. `rpg`: Distance between particle and node
30. `iJacobi`: Inverse of the Jacobi determinant
31. `iJacobi4`: Inverse of the Jacobi determinant divided by 8
32. `iDCell`: Inverse of the grid node interval
33. `SHP`: Shape functions
34. `DNDX`: Derivative of shape functions with respect to X
35. `DNDY`: Derivative of shape functions with respect to Y
36. `DNDZ`: Derivative of shape functions with respect to Z
37. `CSHP`: Shape functions for the center node
38. `CDNDX`: Derivative of shape functions with respect to X for the center node
39. `CDNDY`: Derivative of shape functions with respect to Y for the center node
40. `CDNDZ`: Derivative of shape functions with respect to Z for the center node
41. `SNX`: Sign constant for the X direction
42. `SNY`: Sign constant for the Y direction
43. `SNZ`: Sign constant for the Z direction

## Equations
1. Nature coordinate: `x = (pt%Xp - node%Xg) * iJacobi - 1d0`
2. Shape function: `SHP = sx * sy * sz * 0.125d0`
3. Derivative of shape function with respect to X: `DNDX = SNX * sy * sz * iJacobi4`
4. Derivative of shape function with respect to Y: `DNDY = SNY * sx * sz * iJacobi4`
5. Derivative of shape function with respect to Z: `DNDZ = SNZ * sx * sy * iJacobi4`
6. Distance between particle and node: `rpg(i,:) = (pt%Xp - node_list(InflNode(i))%Xg) * iDCell`
