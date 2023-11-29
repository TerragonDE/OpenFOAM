# OpenFOAM Case Files
Here you find some sample case files for the Series on my YouTube Channel:

https://www.youtube.com/playlist?list=PLeVTllFUlYNCt1H-08fAvwwvi1hJkKhDk

# My Workflow
1. (1) https://www.youtube.com/watch?v=gm6zOZSZLpA : Create a Geometry in FreeCAD -> export .stp file
2. (2) https://www.youtube.com/watch?v=c9gge1AATgk : Generate a Mesh in Salome, import .stp file -> export .unv file
3. (3) https://www.youtube.com/watch?v=jy3ux-mX20c : Copy .unv mesh in case folder and run -> ideasUnvToFoam meshname.unv 
4. Change the type of "walls" from "patch" to "wall" in generated constant/polyMesh/boundary file for turbulent simulations
5. Run the case in terminal in the root case folder: -> interFoam
6. (4) https://www.youtube.com/watch?v=5u7_LPqYiuU : Running parallel on 8+ CPU cores
7. Alternative: -> decomposePar and -> mpirun --allow-run-as-root -np 8 interFoam -parallel and -> reconstructPar
8. Visualize case in a second terminal in the root case folder: -> paraFoam
9. Export Animation in paraFoam as .png Sequence Ani.0001.png
10. Convert .png Sequence to .mp4 in ffmpeg -> ffmpeg -f image2 -i Ani.%4d.png -c:v libx264 -vb 20M output.mp4 (apt install ffmpeg parole xarchiver)

## My Case file Standards
1. Mesh is generated with group-names "inlet", "outlet" and "walls" (define the atmosphere also as "outlet")
2. Gravity is applied in minus z-axis direction
3. Only water and air phases are defined in the cases
4. "turbulenceProperties" is renamed to "momentumTransport" since OpenFOAM v10. content stays the same, so you need to rename the file according to your OpenFOAM Version!
5. Calculate l/s in m/s for a surface: $w = ($Q / 1000 ) / (A); example: 4,5 l/s through  0,9 m2 = 0,005 m/s velocity

## boundary file in constant/polyMesh/ 
After importing the UNV file with ideasUnvToFoam you need to rename the Patch type of "walls" to "wall"!
- patch means: generic patch type containing no geometric or topological information about the mesh, e.g. used for an inlet or an outlet.
- wall means: for a patch that coincides with a solid wall, required for some physical modelling, e.g. wall functions in turbulence modelling.

# Details

## meshSize vs. timeStep
- A stable courant number depends mainly on meshSize vs. timeStep
- 0.002 max. size in Salome
- 0.001 min. size in Salome
- then use controlDict timeStep 0.001 in OpenFoam
- choose a low maxCo and maxDeltaT value together with adjustable timeStep (e.g. CO to 0.1 and maxDeltaT to 1e-4)

## OpenFOAM Dimensions
The Dimensions Vector in every file looks the same and means the following dimensions: [kg m s K mol A cd]:
- MASS 	kilogram 	kg
- LENGTH 	metre 	m
- TIME 	second 	s
- TEMPERATURE 	Kelvin 	K
- MOLES 	mole 	mol
- CURRENT 	Ampere 	A
- LUMINOUS_INTENSITY 	Candela 	cd

## files in the 0/ folder 
Boundary conditions are speciﬁed in ﬁeld ﬁles, e.g. p, U, in time directories like "0"
- ﬁxedValue: value of eqn is speciﬁed by a given fixed value.
- ﬁxedGradient: normal gradient of eqn (eqn) is speciﬁed by gradient.
- zeroGradient: normal gradient of eqn is zero.
- calculated: patch ﬁeld eqn calculated from other patch ﬁelds.
- inletOutlet: switches between zeroGradient when the ﬂuid ﬂows out of the domain, and ﬁxedValue, when the ﬂuid is ﬂowing into the domain.
- Explanation: https://www.openfoam.com/documentation/user-guide/a-reference/a.4-standard-boundary-conditions

## Smoother
Smoother is another name for sparse linear solver like Jacobi, Gauss Seidel and all.
It is called smoother because we do not use this to solve the linear system but rather quickly remove some part of error.
Other than Jacobi, openfoam also uses ILU based smoother. 

## checkMesh errors
Terminal : checkMesh -allTopology -allGeometry

The errors with three stars are the most important. I have played around with the number of non-orthogonal corrector loops and found that 2 are sufficient.
non-orthogonality (nO) is a very important parameter, we can define three ranges of its values:
- nO 41 70 – safe value
- 70 41 nO < 90 – require special treatment of , e.g., nonOrthoCorrectors in fvSolution or numerical schemes in fvSchemes
- nO 42 9O – bad mesh, which can not be used for simulation

More mesh-related functions:

- refineMesh // recalculates the mesh, but does not reduce mesh errors
- renumberMesh   // the mesh is re-written using the write format settings in your controlDict (binary oder ascii)
- transformPoints -translate '(1 0 0)' // moves the mesh in xyz direction, dimension is meter

Right Hand Rule in blockMesh: order of points in a face = Thumb is flow direction (inside or outside)!

## relaxationFactors
(system/fvSolution relaxationFactors)
Under and over relaxation factors control the stability and convergence rate of the iterative process.
The under relaxation factor increases the stability while over relaxation increases the rate of convergence.
Generally start off without RF's then when solution becomes unstable later on bring them in where needed - whichever equations (residual graph) are unstable, 
meaning not a nice smooth line but up and down rapidly. Start off moving down from 1 to 0.8 or 0.8 to 0.6 etc. 
For energy equation start off with 0.9 it takes hundreds or thousands of iterations to converge with relaxation factors in.
