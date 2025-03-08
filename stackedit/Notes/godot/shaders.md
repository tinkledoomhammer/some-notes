
`COLOR` = vec4 with the output color
`UV` = vec2 with texture coordinates
`SCREEN_UV` = vec2 with screen coordinates in [0,1]
`TIME` = time in seconds since the engine started
### Sampling the screen
```hlsl
uniform sampler2D SCREEN_TEXTURE : hint_screen_texture, filter_linear_mipmap;

void fragment(){
	COLOR=texture(SCREEN_TEXTURE, SCREEN_UV);
}
```

# References
https://docs.godotengine.org/en/stable/tutorials/shaders/index.html

## Intro
https://docs.godotengine.org/en/stable/tutorials/shaders/introduction_to_shaders.html
### Shader Functions and Types
Functions: 
* `vertex()` - used in [`canvas_item`](https://docs.godotengine.org/en/stable/tutorials/shaders/shader_reference/canvas_item_shader.html#doc-canvas-item-shader) and [`spatial`](https://docs.godotengine.org/en/stable/tutorials/shaders/shader_reference/spatial_shader.html#doc-spatial-shader) shaders
	* runs for every vertex in a mesh
* `fragment()` - used in `canvas_item` and `spatial` shaders
	* runs for every pixel in a mesh
	* uses  values interpolated from the output of `vertex()`
* `light()` - used in `canvas_item` and `spatial` 
	* runs for every pixel for every light
	* uses data from `fragment()`
* `start()` - used in [particle shaders](https://docs.godotengine.org/en/stable/tutorials/shaders/shader_reference/particle_shader.html#doc-particle-shader), once per particle when it spawns
* `process()` used in `particle` shaders, per particle per frame
* `sky()` - [sky shaders](https://docs.godotengine.org/en/stable/tutorials/shaders/shader_reference/sky_shader.html#doc-sky-shader) which render [`Skies`](https://docs.godotengine.org/en/stable/classes/class_sky.html#class-sky)
	* per pixel in the radiance cubemep when it needs to be updated
	* also per pixel in the current screen
* `fog()` - [fog shaders](https://docs.godotengine.org/en/stable/tutorials/shaders/shader_reference/fog_shader.html#doc-fog-shader)
	* per froxel in the volumetric fog froxel buffer that intersects with the [`FogVolume`](https://docs.godotengine.org/en/stable/classes/class_fogvolume.html#class-fogvolume)

#### Types:
`shader_type spatial;` at the beginning of the shader file

* `spatial` for 3d rendering
* `canvas_item` for 2D rendering
* `particles` for particle systems
* `sky`
* `fog`

#### Render Modes: 
Specified on the second line:
`render_mode unshaded, cull_dissabled;`
* unshaded will prevent the built in light processor function on this object

Can be applied
## Language Reference
https://docs.godotengine.org/en/stable/tutorials/shaders/shader_reference/shading_language.html

Comments
```c++
// comment to end of line
/* part of a line */
/* 	multi
	line
*/
```
### Data Types
* `void`
* `bool`
* `bvec2` - two component of booleans
	* `bvec3` `bvec4`
* `int` - 32-bit signed scalar integer
* `ivec2` - two component, signed int
	* `ivec3` `ivec4`
* `uint` - 32bit unsigned
	* `uvec2`, `uvec3`, `uvec4`
* `float` 32-bit floating point scalar
	* `vec2` `vec3` `vec4`
* `mat2` 2x2 matrix, column major
	* `mat3`, `mat4`
* `sampler2D` - binds 2d textures, reads as **float**
	* `isampler2D`  `usampler2D` `sampler2DArray` `usampler2DArray` `isampler2dArray`
* `sampler3D` - floats
	* `isampler3D` `usampler3D`
* `samplerCube` -- samples cubemaps as floats
	* `SamplerCubeArray`
* `samplerExternalOES` - only supported in compatibility/android
#### precision
* precision specifiers are 
	* `lowp` - usually 8 bits per component, maps 0-1 for floats
	*  `mediump`- usually 16 bits or half floats
	*  `highp` - full float or int range
	* i.e. `lowp vec4 a = vec4(0.0,1.0,2.0,3.0);`
#### casting
* Implicit casts are not allowed between different types, even if the size is the same
* default `int`s are signed, converting to unsigned requires an explicit cast
* i.e. `uint x = uint(5);`
#### Members
* Vector members can be :
	* `x` `y` `z` `w` or
	* `r` `g` `b` `a`
* Matrices are column-row order
	* `mat2 m2 ... m2[0].x`

Swizzling
```hlsl
vec4  a  =  vec4(0.0,  1.0,  2.0,  3.0);
vec3  b  =  a.rgb;  // Creates a vec3 with vec4 components.
vec3  b  =  a.ggg;  // Also valid; creates a vec3 
		// and fills it with a single vec4 component.
vec3  b  =  a.bgr;  // "b" will be vec3(2.0, 1.0, 0.0).
vec3  b  =  a.xyz;  // Also rgba, xyzw are equivalent.
vec3  b  =  a.stp;  // And stpq (for texture coordinates).
float  c  =  b.w;  // Invalid, because "w" is not present in vec3 b.
vec3  c  =  b.xrt;  // Invalid, mixing different styles is forbidden.
b.rrr  =  a.rgb;  // Invalid, assignment with duplication.
b.bgr  =  a.rgb;  // Valid assignment. 
		// "b"'s "blue" component will be "a"'s "red" and vice versa.
```

#### Construction

Vectors:

```c++
// The required amount of scalars
vec4  a  =  vec4(0.0,  1.0,  2.0,  3.0);
// Complementary vectors and/or scalars
vec4  a  =  vec4(vec2(0.0,  1.0),  vec2(2.0,  3.0));
vec4  a  =  vec4(vec3(0.0,  1.0,  2.0),  3.0);
// A single scalar for the whole vector
vec4  a  =  vec4(0.0);
```

Matrices:

```c++
// with the correct size
mat2  m2  =  mat2(vec2(1.0,  0.0),  vec2(0.0,  1.0));
mat3  m3  =  mat3(vec3(1.0,  0.0,  0.0),
	vec3(0.0,  1.0,  0.0),
	vec3(0.0,  0.0,  1.0));
//With a single value, a diagonal matrix is constructed	
mat4  identity  =  mat4(1.0);

//Different sizes
mat3  basis  =  mat3(MODEL_MATRIX); 
//Expanding fills with 1.0 on diagonals
//and 0 elsewhere
mat4  m4  =  mat4(basis);
//Shrinking crops the bottom, right 
//returns a top,left submatrix
mat2  m2  =  mat2(m4);
```

#### Arrays
* declared with c-style syntax
	* `[const] + [precision] + typename + identifyer + [arraySize]`
* Not valid for samplers
Initialization:

```c
float  float_arr[3]  =  float[3]  (1.0,  0.5,  0.0); //type 1
int  int_arr[3]  =  int[]  (2,  1,  0);	//type 2
vec2  vec2_arr[3]  =  {  vec2(1.0,  1.0),
		vec2(0.5,  0.5),
		vec2(0.0,  0.0)
		};  		// third constructor

//Specifying the size is optional
bool  bool_arr[]  =  {  true,  true,  false  };  // fourth constructor
	//Size is determined by the element count of the initializer
```
* built-in methods
*  `.length()` returns the # of elements
* `clamp()` ??
* `if` ?? -- is this even a function

Global arrays
* can be declared as either `const` or `uniform`
* `const` must have in initializer, `uniform` is not allowed to have an initializer

#### Constants
* must be initialized
* cannot have hints
* cannot be changed
* can be global or local
	* globals can be shared between shader stages
	* they are not accessible from outside the shader
* unsigned `const` must be suffiexed with `u` or constructed with `uint(val)`

### Built-in functions
### Shader preprocessor
### Shader Types
#### Spatial
#### CanvasItem
#### Particle
#### Sky
#### Fog

### Compute Shaders






> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTI2NTI2MDgzLC0xNDYxNzQ1MTAzLC0xND
M0NTM3NzQ3LC0xMzEyNjgwMDYsLTE0OTU4NjE5NzldfQ==
-->