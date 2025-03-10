
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
```c++
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
vec2  vec2_arr[3]  =  {  vec2(1.0,  1.0), //type 3
		vec2(0.5,  0.5),
		vec2(0.0,  0.0)
		};

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

#### Structs
* mostly like c

```c++
struct PointLight{
	vec3 position;
	vec3 color;
	float intensity;
};

struct TwoLights{
	PointLight lights[2];
};

void someFunc(PointLight light){/**/}
void fragment(){
	PointLight light1;
	PointLight light2= PointLight(
		vec3(0.0), vec3(1.0,0.0,0.0), 0.5 );
	TwoLights lights;
	someFunc(TwoLights.lights[0]);
}
```


### Operators
* The same operators as GLSL ES 3.0

Precedence | name | operators
--|--|--
1(highest) | parenthetical grouping | `( )`
2 | unary | `+` `-` `!` `~`
3 | multiplicative | `/` `*` `%`
4 | additive | `+` `-`
5 | bit-wise shift | `<<` `>>`
6 | relational | `<` `>` `<=` `>=`
7 | equality | `==` `!=`
8 | bit-wise AND | `&`
9 | bit-wise XOR | `^`
10 | bit-wise OR | <code>|</code>
11 | logical AND | `&&`
12 (lowest) | logical OR | <code>||</code>

* Most vector and matrix operations are component-wise **except**:
	* mat * vec
	* vec * mat, and 
	* mat * mat



### Flow control
* It seems to have c- style `if else`, `while`, `do while`, `switch case` and `? : `

```c++
// `if`, `else if` and `else`.
if  (cond)  {}  else  if  (other_cond)  {
}  else  {}

// Ternary operator.
// This is an expression that behaves like `if`/`else` and returns the value.
// If `cond` evaluates to `true`, `result` will be `9`.
// Otherwise, `result` will be `5`.
int  result  =  cond  ?  9  :  5;

// `switch`.
switch  (i)  {  // `i` should be a signed integer expression.
  case  -1:	  break;
  case  0:
	  return;  // `break` or `return` to avoid running the next `case`.
  case  1:  // Fallthrough (no `break` or `return`): will run the next `case`.
  case  2:	break;
  default:  // Only run if no `case` above matches. Optional.
	  break;
}

// `for` loop. Best used when the number of elements to iterate on
// is known in advance.
for  (int  i  =  0;  i  <  10;  i++) {}

// `while` loop. Best used when the number of elements to iterate on
// is not known in advance.
while  (cond)  {}

// `do while`. Like `while`, but always runs at least once even if `cond`
// never evaluates to `true`.
do  {}  while  (cond);
```

#### Testing floats for equality
```
const  float  EPSILON  =  0.0001;
if  (value  >=  0.3  -  EPSILON  &&  value  <=  0.3  +  EPSILON){}

//below is wrong
if(value == 0.3) {} // rounding errors
```

### Discarding and functions and such
the `discard` keyword when used in a light or fragment shader, or custom function, will prevent the fragment from being written
* It has a performance cost

Functions are c-style
* there are special qualifiers though
	* `in` - only for reading
	* `out` - only for writing
	* `inout` - passed via reference
	* `const` - can be combined with `in`
* Functions can be overloaded
	* There are no implicit conversions of arguments

### Varyings
* Data sent from `vertex()` to `fragment()`
	* or from `fragment()` to `light()`
* Computed for every primitive vertex in the vertex processor
* interpolated for every fragment
* They can**not** be used in **assigned** in custom functions or `light()`

#### Interpolation qualifiers
```c
shader_type  spatial;

varying  flat  vec3  flat_color;
//`flat` means no interpolation
varying smooth vec3 smooth_color;
//smooth means perspective-correct interpolation
//smooth is the default

void  vertex()  {
  our_color  =  COLOR.rgb;
  other_color = COLOR.bgr;
}

void  fragment()  {
  //`flat` means no interpolation
  ALBEDO  =  flat_color;
}
```

### Uniforms

* `uniform` are variables that are pased to the shader once 
* they are editable as material properties after a shader has been assigned
* they can be any data type except for `void`
* They can also have default values and hints

#### Uniform hints
Examples:
```c
uniform  vec4  color  :  source_color;
uniform  float  amount  :  hint_range(0,  1);
uniform  vec4  other_color  :  source_color  =  vec4(1.0);
	// Default values go after the hint.
uniform  sampler2D  image  :  source_color;
```

Hint Table
Type | Hint | Description
--| -- | --
`vec3` `vec4` | `source_color` | used as a color
`int` | `hint_enum("string1","string2"...)` | displays a dropdown
`int` `float` | `hint_range(min, max [,step])` | for a slider
`sampler2D` | `source_color` | Used as albedo color
.. | `hint_normal` | normal map
.. | `hint_default_white` | like `source_color` but defaults to white
.. | `hint_default_black`|
.. | `hint_default_transparent` | 
.. | `hint_anisotropy` | as flowmap, default to right
.. | `hint_roughness[_r, _g, _b, _a, _normal, _gray] | Used for roughness limiter
.. |`repeat[_enable, _dissable]` | enable texture repeating
.. | `hint_screen_texture` | Samples the screen
.. | `hint_depth_texture` | Texture is the depth texture
.. | hint_normal_roughness_texture | Texture is the normal roughness texture

#### `hint_enum` and `source_color`
```
uniform int anEnum : hint_enum("a","b"...)=0;
//a=>0, b=>1, etc
uniform int anotherEnum : hint_enum("A:10", "b:110"...)=30;
//similar to other exports
```

`vec3 aColor : source_color`
* used for sRGB color data, because godot renders in linear color space
	* not using it will result in washed out colors
* albedo color textures should use `source_color` others should not
* source color is required when
	* in Forward+ and Mobile renderers
	* In canvas items when [HDR 2D](https://docs.godotengine.org/en/stable/classes/class_projectsettings.html#class-projectsettings-property-rendering-viewport-hdr-2d) is enabled
* It is optional for
	* compatibility renderer
	* `canvas_item` shaders if HDR 2D is disabled
* It should always be used for colors

#### uniform groups
```
group_uniforms MyGroup;
uniform int anInt = 0;
group_uniform MyGroup.MySubgroup
	//The parent group is optional
group_uniforms;
	//closes both MyGroup and MySubgroup
group_uniforms MyGroup;
	uniform int b = 0; //anInt and b are together in the inspector
group_uniforms;
	// the above is only required if there are more 
	// 	ungrouped uniforms below
```

Example
```c++
uniform int u1;
group_uniforms MyGroup;
	uniform u2;
group_uniforms MyGroup.sub1;
	uniform int u3;
group_uniforms Mygroup.sub2;
	uniform int u4;
group_uniforms MyGroup;
	uniform int u5;
group_uniforms;
uniform int u2;
```
* My Group
	* u2
	* u5
	* sub1
		* u3;
	* sub2
		* u4
* u1
* u2


### Built-in variables
#### Built-in functions

## Built-in functions
## Shader preprocessor
## Shader Types
### Spatial
### CanvasItem
### Particle
### Sky
### Fog

## Compute Shaders






> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE5NzkzMDkwMzFdfQ==
-->