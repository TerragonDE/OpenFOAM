Workflow for this case: 

1. ideasUnvToFoam Mesh.unv
2. decomposePar
3. mpirun -np 8 interFoam -parallel  (adjust decomposePar file first to 8 !)
4. reconstructPar
5. paraFoam (reload files, export animation)
6. ffmpeg -f image2 -i Ani1.%4d.png -c:v libx264 -vb 20M output1.mp4

Tipp: Try first, if the case works without decomposePar by just just typing interFoam in step 2. 
