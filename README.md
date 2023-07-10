# OpenFOAM
Sample OpenFOAM Cases

All cases expect the names "inlet", "outlet" and "walls" in the mesh.  

Workflow: 
1. Create a Geometry in FreeCAD or any other CAD Application, export STP file
2. Generate a Mesh in Salome by importing STP file and exporting a UNV file
3. Copy UNV file i a prepared case folder and run in terminal: ideasUnvToFoam meshname.unv
4. Run case in terminal in the root case folder: interFoam
5. Visualize case in a second terminal in the root case folder: paraFoam

OpenFOAM boundary conditions overview:
https://www.openfoam.com/documentation/user-guide/a-reference/a.4-standard-boundary-conditions

OpenFOAM Dimensions Vector in every file: [kg m s K mol A cd]:
MASS 	kilogram 	kg
LENGTH 	metre 	m
TIME 	second 	s
TEMPERATURE 	Kelvin 	K
MOLES 	mole 	mol
CURRENT 	Ampere 	A
LUMINOUS_INTENSITY 	Candela 	cd

Smoother
Smoother is another name for sparse linear solver like Jacobi, Gauss Seidel and all.
It is called smoother because we do not use this to solve the linear system but rather quickly remove some part of error.
Other than Jacobi, openfoam also uses ILU based smoother. 

