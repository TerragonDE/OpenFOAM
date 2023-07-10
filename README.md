# OpenFOAM
Sample OpenFOAM Cases

Workflow: 
1. Create a Geometry in FreeCAD or any other CAD Application -> export .STP file
2. Generate a Mesh in Salome by importing STP file and -> export .UNV file
3. Copy UNV file i a prepared case folder and run in terminal: -> ideasUnvToFoam meshname.unv
4. Change the type of "walls" from "patch" to "wall" in generated constant/polyMesh/boundary file (for turbulent simulations)
5. Run case in terminal in the root case folder: -> interFoam
6. Visualize case in a second terminal in the root case folder: -> paraFoam

All cases are built on:
1. Mesh with face-names "inlet", "outlet" and "walls"
2. Gravity in z-axis
3. Only water and air phases
4. "turbulenceProperties" is renamed to "momentumTransport" since OpenFOAM v10. content stays the same!

Dimensions Vector in every file: [kg m s K mol A cd]:
- MASS 	kilogram 	kg
- LENGTH 	metre 	m
- TIME 	second 	s
- TEMPERATURE 	Kelvin 	K
- MOLES 	mole 	mol
- CURRENT 	Ampere 	A
- LUMINOUS_INTENSITY 	Candela 	cd

Patch types
- patch: generic type containing no geometric or topological information about the mesh, e.g. used for an inlet or an outlet.
- wall: for patch that coincides with a solid wall, required for some physical modelling, e.g. wall functions in turbulence modelling.

Boundary conditions are speciﬁed in ﬁeld ﬁles, e.g. p, U, in time directories 
- ﬁxedValue: value of eqn is speciﬁed by value.
- ﬁxedGradient: normal gradient of eqn (eqn) is speciﬁed by gradient.
- zeroGradient: normal gradient of eqn is zero.
- calculated: patch ﬁeld eqn calculated from other patch ﬁelds.
- inletOutlet: switches between zeroGradient when the ﬂuid ﬂows out of the domain, and ﬁxedValue, when the ﬂuid is ﬂowing into the domain.
- https://www.openfoam.com/documentation/user-guide/a-reference/a.4-standard-boundary-conditions


Smoother
Smoother is another name for sparse linear solver like Jacobi, Gauss Seidel and all.
It is called smoother because we do not use this to solve the linear system but rather quickly remove some part of error.
Other than Jacobi, openfoam also uses ILU based smoother. 
