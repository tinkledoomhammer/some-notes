
# Generating assets including sprites and animations

## Goals
1. Procedurally generate sprites and animations
2. Use readable source code
3. Skeleton-first design
4. Export as vector and raster
5. Integration with Godot

### Features
* Pallet swapping
* Skeleton-based design
* Parameterized morphing color manipulation

### Design Ideas
* Skeleton-first design of anmiation
* Parameterized color and motion
* Design nested 2D or 3D point clouds
	* the geometry should all be defined in terms of a relatively small number of points
	* The point clouds are arranged in a tree
	* Each node has its own local transform
		* which is defined by animation rules
		* so the transform is function of all of the animatable parameters

## Blender/Python

## Webby

## Rust

## Applications
### Aesprite
### Graphite
### Blender
### Inkscape



> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTI3MjQzNDddfQ==
-->