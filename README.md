# OpenFOAM Case Files
Here you find some sample case files for the Series on my YouTube Channel:

https://www.youtube.com/playlist?list=PLeVTllFUlYNCt1H-08fAvwwvi1hJkKhDk

# Workflow
1. (1) https://www.youtube.com/watch?v=yDRQf7fcAlg : Create a Geometry in FreeCAD or any other CAD Application -> export .STP file
2. (2) https://www.youtube.com/watch?v=mnj0UcPItu8 : Generate a Mesh in Salome by importing STP file and -> export .UNV file
3. (3) https://www.youtube.com/watch?v=E0AzNABWrow : Change the type of "walls" from "patch" to "wall" in generated constant/polyMesh/boundary file for turbulent simulations
4. Copy UNV file i a prepared case folder and run in terminal: -> ideasUnvToFoam meshname.unv 
5. Run case in terminal in the root case folder: -> interFoam
6. Visualize case in a second terminal in the root case folder: -> paraFoam

## Case file Standards
1. Mesh is generated with group-names "inlet", "outlet" and "walls"
2. Gravity is applied in minus z-axis direction
3. Only water and air phases are defined in the cases
4. "turbulenceProperties" is renamed to "momentumTransport" since OpenFOAM v10. content stays the same, so you need to rename the file according to your OpenFOAM Version!

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
- choose a low maxCo value together with adjustable timeStep

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


