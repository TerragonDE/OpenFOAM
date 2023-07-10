# OpenFOAM
Sample OpenFOAM Cases

OpenFOAM boundary conditions overview:
https://www.openfoam.com/documentation/user-guide/a-reference/a.4-standard-boundary-conditions

OpenFOAM Dimensions Vector in every file: [kg m s K mol A cd]
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
