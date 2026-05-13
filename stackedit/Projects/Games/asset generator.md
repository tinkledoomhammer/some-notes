Types
Construct
* has parameters
* a dict or something
* method; render
* output format

Render targets / output formats
* points 2d/3d
* path 3d/3d
* surface 2d/3d
* mesh

Fixed vs parameter values
need to encode formulas

Parameter formats
* angle
* scalar
* transformation (?)
* 

Partial constructs: precalc what can be precalculated, track the effect of changes to unbound parameters?

To research: animation of godot's ArrayMesh class
`

## Hierarchical csg
* use anchors and parameters
* compute transforms based on anchors
* Perhaps they are treated like parameters
* i.e. 
	* baseEdge(lod, base, tip) -> path
		* interpolates between base and tip with a # of steps depending on lod
	* edges(lod, base, tip) -> surface
		* creates a surface that 
			* generates a baseEdge with lod



> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTk1MTQ0NTgzNywxNzg0MDEwOTg5LC03MT
A0Nzc3MDldfQ==
-->