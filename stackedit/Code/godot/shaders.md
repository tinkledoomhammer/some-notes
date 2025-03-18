
`COLOR` = vec4 with the output color
`UV` = vec2 with texture coordinates
`SCREEN_UV` = vec2 with screen coordinates in [0,1]
`TIME` = time in seconds since the engine started
### Sampling the screen
```glsl
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
```glsl
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
	* In GDscript: bitwise packed `int`
* `int` - 32-bit signed scalar integer -- also `int` in gdscript
* `ivec2` - two component, signed int
	* `ivec3` `ivec4`
	* in GDScript `Vector2i`, `Vector3i`, etc
* `uint` - 32bit unsigned
	* `uvec2`, `uvec3`, `uvec4`
	* in GDScript: `int`, `Vector2i`, etc
* `float` 32-bit floating point scalar
	* `vec2` `vec3` `vec4`
	* in GDScript: `float`, `Vector2`, etc
		* `vec3` can also be `Color`
		* `vec4` can also be
			* `Color` - rgba
			* `Rect2` - `(position.x, position.y, size.x, size.y)` 
			* `Plane` - `(normal.x, normal.y, normal.z, d)`
			* `Quaternion`
* `mat2` 2x2 matrix, column major in GDScript: `Transform2D`
	* `mat3` in GDScript: `Basis`
	* `mat4` in GDScript: `Projection`
		* or `Transform3D`, with w Vector set to identity
* `sampler2D` - binds 2d textures, reads as **float**
	* `isampler2D`  `usampler2D` `sampler2DArray` `usampler2DArray` `isampler2dArray`
	* in GDScript `Texture2D` and `Texture2DArray`
* `sampler3D` - floats
	* `isampler3D` `usampler3D`
	* in GDScript `Texture3D`
* `samplerCube` -- samples cubemaps as floats
	* `SamplerCubeArray` - not supported in Compatibility
	* in GDScript `Cubemap`, `CubemapArray`
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
```glsl
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

```glsl
// The required amount of scalars
vec4  a  =  vec4(0.0,  1.0,  2.0,  3.0);
// Complementary vectors and/or scalars
vec4  a  =  vec4(vec2(0.0,  1.0),  vec2(2.0,  3.0));
vec4  a  =  vec4(vec3(0.0,  1.0,  2.0),  3.0);
// A single scalar for the whole vector
vec4  a  =  vec4(0.0);
```

Matrices:

```glsl
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

```glsl
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

```glsl
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

```glsl
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
```glsl
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
```glsl
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
```glsl
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

* There is also `instance_index(n)` for instance uniforms


#### `hint_enum` and `source_color`
```glsl
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
```glsl
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
```glsl
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
uniform int u6;
```
* My Group
	* u2
	* u5
	* sub1
		* u3;
	* sub2
		* u4
* u1
* u6

#### global uniforms
* In project settings, you can set up uniforms that are shared between all shaders
* In individual shaders they must be declared as `global`
* `global uniform vec4 my_color;` after adding a color named `my_color` in project setting >> shader globals
* They can be changed at run time:
	* `RenderingServer.global_shader_parameter_set("my_color", Color(0.3, 0.6, 1.0))`
* And added or removed
```python
RenderingServer.global_shader_parameter_add("my_color",
		RenderingServer.GLOBAL_VAR_TYPE_COLOR,
		Color(0.3,  0.6,  1.0)
	)
RenderingServer.global_shader_parameter_remove("my_color")
```
* Setting uniforms is low cost. Querying, adding and removing them is expensive

#### Per-instance uniforms

* Only available in `spatial` shaders
* Used to share materials between non-identical instances
* They are set on a `GeometryInstance3D` rather than the material
* `instance uniform vec4 my_color : source_color = vec4(...);`
* They can be set in the inspector for each instance
* They can be set in gdscript with the `set_instance_shader_paramtter(..)` method of [`GeometryInstance3D`](https://docs.godotengine.org/en/stable/classes/class_geometryinstance3d.html#class-geometryinstance3d)
* Limitations:
	* No textures, only scalar and vector types
	* As a workaround, a texture array can be passed as a regular uniform, and the index as an instance uniform
	* Each shader can have at most 16 instance uniforms
	* If the shader has multiple materials, the earlier uniforms can shadow the later uniforms unless they have the same name, index, and type
		* This can be guaranteed by specifying an instancce_index hint:
		* `instance uniform vec4 my_color : source_color, instance_index(5);`

# Setting uniforms in GDScript
`material.set_shader_parameter("some_Value", <value>);`
* the first argument must match the shader script exactly
* If there is a type mismatch, then no error will be thrown
* Shaders are limited to 64KiB in size, or 16KiB on mobile
	* bool is padded
	* the contents do not count -- only use the size of the sampler

### Built-in variables
Depend on the type of shader and which function they are used in
*   [Spatial shaders](https://docs.godotengine.org/en/stable/tutorials/shaders/shader_reference/spatial_shader.html#doc-spatial-shader)
*   [Canvas item shaders](https://docs.godotengine.org/en/stable/tutorials/shaders/shader_reference/canvas_item_shader.html#doc-canvas-item-shader)
*   [Particle shaders](https://docs.godotengine.org/en/stable/tutorials/shaders/shader_reference/particle_shader.html#doc-particle-shader)
*   [Sky shaders](https://docs.godotengine.org/en/stable/tutorials/shaders/shader_reference/sky_shader.html#doc-sky-shader)
*   [Fog shaders](https://docs.godotengine.org/en/stable/tutorials/shaders/shader_reference/fog_shader.html#doc-fog-shader)

#### Built-in functions

[Built-in functions](https://docs.godotengine.org/en/stable/tutorials/shaders/shader_reference/shader_functions.html#doc-shader-functions)
https://docs.godotengine.org/en/stable/tutorials/shaders/shader_reference/shader_functions.html#doc-shader-functions

## Built-in functions
https://docs.godotengine.org/en/stable/tutorials/shaders/shader_reference/shader_functions.html

Godot's built-in functions conform roughly with GLSL ES 3.0
* Unless otherwise specified, vector and matrix operations are component-wise
	* i.e. `sqrt(vec2(4,64))` is the same as `vec2(sqt(4), sqrt(64))`

### Type aliases
* Only used in documentation to reduce repetitive declarations
* If they are specified for multiple parameters, then in most cases each instance of the alias must refer to the same type

Godot Alias | Actual types | glsl documentation alias
--|--|--
`vec_type` | `float` `vec2`, vec3, vec4 | `genType`
`vec_int_type` | int, ivec2, ivec3, ivec3 | `genIType`
`vec_uint_type` | uint, uvec2, uvec3, uv3c4 | `genUType`
`vec_bool_type` | bool, bvec2, bvec3, bvec4 | `genBType`
`mat_type` | mat2, mat3, mat4 | `genBType` 
`gvec4_type` | vec4, ivec4, or uvec4 | `gvec4`
`gsampler2D` | sampler2D, isampler2D, uSampler2D | `gsampler2D`
`gasmpler2DArray` | sampler2DArray, isampler2DArray, or uSampler2DArray | `gsampler2DArray`
`gsampler3D` | sampler3D, isampler3D, uSampler3D | `gsampler3D`

* It looks like `uSampler3D` etc are typos?

### Trig functions

* `vec_type radians(vec_type degrees)`
* `vec_type degrees(vec_type radians)`
* `vec_type sin(vec_type x)` // in radians
* `vec_type cos(vec_type x)`
* `vec_type tan(vec_type x)`
* `vec_type asin(vec_type x)`
* `vec_type acos(vec_type x)`
* `vec_type atan(vec_type y_over_x)`
* `vec_type atan(vec_type y, vec_type x)`
* `vec_type sinh(vec_type x)`
* `vec_type cosh(vec_type x)`
* `vec_type tanh(vec_type x)`
* `vec_type asinh(vec_type x)`
* `vec_type acosh(vec_type x)`
* `vec_type atanh(vec_type x)`
* These functions take and return radians for angle
* Returned angles are in [$-\pi , \pi$]

#### Exponential and Math functions
* `vec_type pow(vec_type x, vec_type y)` x^y
* `vec_type exp(vec_type x)` e^x
* `vec_type exp2(vec_typex)` 2^x
* `vec_type log(vec_type x)` $log_e x$
* `vec_type log2(vec_type x)` $log_2 x$
* `vec_type sqrt(vec_type x)` $\sqrt x$
* `vec_type inversesqrt(vec_type x)` $\frac{1}{\sqrt x}$
* `vec_type abs(vec_type x)` or `vec_int_type abs(vec_int_type)` $|x|$
* `vec_type sign(vec_type x)` or `vec_int_type sign(vec_int_type)`
	*	returns `1.0` or `1` if `>0`, `0` or `0.0` if `x==0`, or `-1.0`, etc
*	`vec_type floor(vec_type x)` rounds down to the nearest integer
*	`vec_type round(vec_type x)` rounds to the nearest integer
*	`vec_type roundEven(vec_type x)` rounds down to the nearest even integer
*	`vec_type trunc(vec_type x)` truncation
*	`vec_type ceil(vec_type x)` rounds up to the nearest integer
*	`vec_type fract(vec_type x)` returns `x-floor(x)`
*	`vec_type mod(vec_type x, vec_type y)` or `vec_type mod(vec_type x, float y`
	*	returns `x%y`
*	`vec_type modf(vec_type x, out vec_type i)` returns `frac(x)` and `i :=` the integer part
*	`min(a,b)` and `max(a,b)`
	*	works with float, uint, and int types, and vectors thereof
	*	When the first argument is a vector, the second argument can be a single number
	*	otherwise, the return type and both argument must both be the same type
*	`clamp(x, min, max)`
	*	Like above, and the last two arguments must be the same type
*	`vec_type mix(vec_type a, vec_type b, vec_type c)` 
	*	 `c` may be a float
	*	linear interpolation of c from a to b : `a * (1-c) + b*c` 
	*	also `vec_type mix(vec_type a, vec_type b, vec_bool_type c)`
		*	`return b ? c : a`
*	`vec_type fma(vec_type a, vec_type b, vec_type c)` `return (a* b + c)`
*	`vec_type step(vec_type a, vec_type b)` or `vec_type step(float a, vec_type b)`
	*	`return b<a ? 0.0 : 1.0`
*	`vec_type smoothstep(vec_type a vec_type b, vec_type c)`
	*	or `vec_type smoothstep(float a, float b, vec_type c)`
	*	"Hermite interpolation between `a` and `b` by `c` when `a<c<b`
	*	`vec+type t = clamp((c-a) / (b-a), 0.0, 1.0); return t*t*(3.0 -2.0*t);`
	*	a = lower edge ; b = upper edge ; c=value to be interpolated
*	`vec_bool_type isnan(vec_type x)` `return x==NaN`
*	`vec_bool_type isinf(vec_type x)` `return x==INF`
*	`vec_int_type floatBitsToInt(vec_type x)` copies bits to an int, no conversion
	*	also `floatBitsToUint` , `intBitsToFloat` , `uintBitsToFloat`
	 
### Geometric functions
* float `length`(vec_type x) = $\sqrt{\sum x_n^2}$
* float `distance`(vec_type a, vec_type b) = `return length(a-b)`
* float `dot`(vec_type a, vec_type b) = $\sum{a_n b_n}$ = $|a| |b| \cos \theta$
* float `cross`(vec3a, vec3 b) = $a \times b$
	```glsl 
		vec3( a.y * b.z - b.y * a.z,
		  a.z * b.x - b.z * a.x,
		  a.x * b.z - b.x * a.y)
	```
	https://www.khronos.org/registry/OpenGL-Refpages/gl4/html/cross.xhtml
* vec_type `normalize`(vec_type x) $x / |x|$ 
* vec3 `refelct`(vec3 I, vec3 N) `I - 2.0 * dot(N, I) *N`
	* `N` should be normalized and is the surface normal
	* `I` is the incident vector
* vec3 `refract`(vec3 I, vec3 N, float eta) 
		```glsl
		k  =  1.0  -  eta  *  eta  *  (1.0  -  dot(N,  I)  *  dot(N,  I));
		if  (k  <  0.0)
			R  =  genType(0.0);  //  or  genDType(0.0)
		else
			R  =  eta  *  I  -  (eta  *  dot(N,  I)  +  sqrt(k))  *  N;
	```
	* https://www.khronos.org/registry/OpenGL-Refpages/gl4/html/refract.xhtml

* vec_type `faceforward`(vec_type N, vec_type I, vec_type Nref)
	* https://www.khronos.org/registry/OpenGL-Refpages/gl4/html/faceforward.xhtml
* mat_type `matrixCompMult(mat_type x, mat_type y)` Matrix component multiplication
	* Just multiplies each component
* mat_type`outerProduct(vec_type column, vec_type row)` matrix outer product
	* The output has one row per member of `column`
	* and as many columns as `row` has members
* mat_type `transpose(mat_type m)` $m^T$
* float `determinant(mat_type m)`??? Matrix determinant
	* https://www.khronos.org/registry/OpenGL-Refpages/gl4/html/inverse.xhtml
* mat_type `inverse(mat_type m)` Inverse matrix
	* https://www.khronos.org/registry/OpenGL-Refpages/gl4/html/inverse.xhtml

### Comparison Functions
* vec_bool_type `lessThan`(vec_type x, vec_type y)
* vec_bool_type `greaterThan`(vec_type x, vec_type y)
* vec_bool_type `lessThanEqual`(vec_type x, vec_type y)
* vec_bool_type `greaterThanEqual`(vec_type x, vec_type y)
* vec_bool_type `notEqual`(...)
* bool `any`(vec_bool_type x)
* bool `all`(vec_bool_type x)
* vec_bool_type `not`(vec_bool_type)

### Texture Functions
* `textureSize` i.e. `ivec2 textureSize(gsampler2D s, int lod)`
	* can return `ivec2` or `ivec3`
	* the `s` parameter can be `gsampler2D` `samplerCube` `gsampler3D`, or arrays of the first 2
	* the return order is [width, height, depth]
	* The `lod` parameter is optional and specifies which lod/mipmap to use
* vec2 `textureQueryLod`(gsampler2D s, vec2 p) 
	* The first arg can also be `gsampler2DArray`
	* The first arg can also be `gsampler3D` or `samplerCube`, and the 2nd arg is a `vec3`
	* https://www.khronos.org/registry/OpenGL-Refpages/gl4/html/textureQueryLod.xhtml
* int `textureQueryLevels`(gsampler2D s) or `gasmpler2DArray` or `gsampler3D` or `samplerCube`
	* just returns the number of lod/mipmap levels
* vec4 `texture`(texture, vec p [, float bias])
	* can return `vec4` or `gvec4_type`
	* the first arg `s` can be any sampler type
	* the second arg `p` is a vector with the appropriate dimension
	* the optional `bias` parameter is influences the lod calculation
	* https://www.khronos.org/registry/OpenGL-Refpages/gl4/html/texture.xhtml
	* There is some weirdness around shadows that idk
* gvec4_type `textureProj` (gsampler2D s , vec3 p[, float bias])
	* `p` can be a `vec4`
	* `s` can be a `gsampler3D` but only if `p` is `vec4`
	* https://www.khronos.org/registry/OpenGL-Refpages/gl4/html/textureProj.xhtml
* gvec4_type `textureLod(`gsampler2D s, vec2 p, float lod)
	* Performs a texture lookup at coordinate `p` with an explicit level of detail
	* can also return a `vec4` when used with `samplerCube` or `samplerCubeArray`
* gvec4_type `textureProjLod(`gsampler2D s, vec3 p, float lod
	* `p` can be a `vec4`
	* `s` can be `sampler3D` and requires `vec4 p`
	* https://www.khronos.org/registry/OpenGL-Refpages/gl4/html/textureProjLod.xhtml
* gvec4_type `textureGrad(` gsampler2D s, vec2 p, vec2 dPdx, vec2 dPdy)
	* other types similar to `textureProjLod`
	* looks up a texture with specified explicit texture gradients
	* https://www.khronos.org/registry/OpenGL-Refpages/gl4/html/textureGrad.xhtml
* gvec4_type `textureProjGrad(` ...
	* https://www.khronos.org/registry/OpenGL-Refpages/gl4/html/textureProjGrad.xhtml
* gvec4_type `texelFetch(`gsampler2D s, ivec2 p, int lod)
	* if the sampler is an array or 3D, then `ivec3 p`
* vec4 `textureGather(` gsampler2D s, vec2 p [, int comps])
	* Can also use an array or cube sampler (both require `vec3 p`
	* The optional parameter `comps` is 0 for x (default), ... 3 for w
* vec_type `dFdx(`vec_type p)
	* Fragment shaders only
	* also `dFdxCoarse(` and `dFdxFine(`
	* Returns the partial dirivative of `p` with respect to the window x coordinate using local differencing
	* There are also `dFdy`...
	* Expressions with higher order derivatives and mixed-order derivatives produce undefined result
	* and slow `fwidth(`vectype p available in coarse and fine 
	* that return`abs(dFdx(p)+abs(dFdy(p))`

### Packing and unpacking functions
The convert multiple floats to a uint32 (packing) or the other way around
* uint `packHalf2x16(` vec2 v)
	*	vec2 `unpackHalf2x16(`uint v)
*	uint `packUnorm2x16(`vec2 v)
	*	vec2 `unpackUnorm2x16(` uint v)
	*	two normalized (range 0-1) 32 bit floats ->16-bit floats
*	uint `packSnorm2x16` ... as above but normalized to -1 .. 1
*	also `Unorm4x8(` and `Snorm4x8` 

#### Bitwise functions
* vec_[u]int_type `bitfieldExtract(` vec_[u]int_type value, int offset, it bits)
	* in the unsigned version, the most significant bits will be 0
	* the signed version sign-extends the msb of the extracted bits 
* vec_[u]int_type `bitfieldInsert(`vec_[u]int_type base, vec_[u]int_type insert, int offset, int bits)
	* returns the combined value
* vec_[u]int_type `bitfieldReverse(`vec_[u]int_type value)
	* returns a vector of bit-reversed integers (not a reversed vector...)
* vec_[u]int_type `bitCount(`vec_[u]int_type value)
	* returns the number of 1 bits
* vec_[u]int_type `findLSB(`vec_[u]int_type value)
	* vec_[u]int_type `findMSB(`vec_[u]int_type value)
	* return index of the most/least significant bit
	* will return -1 if value is 0
	* MSB will be -1 if value is -1
* void `imulExtended(`vec_int_type x, vec_int_type y, out vec_int_type msb, out vec_int_type lsb)
	* the unsigned version is `umulExtended(`
* vec_uint_type `uaddCarry(`vec_uint_type x, vec_yibt_type y, out vec_uit_type carry)
	* also `usubBorrow(`
	* the out param `carry` will be 1 if carry/borrow occurred, otherwise 0
* vec_type `ldexp(`vec_type x, out vec_int_type exp)
	* returns $x * 2^{exp}$
* vec_type `frexp(`vec_type x, out vec_int_type exp)
	* returns floats in $[0.5, 1.0)$ 
	* the out param `exp` takes the corresponding exponents


## Shader preprocessor
https://docs.godotengine.org/en/stable/tutorials/shaders/shader_reference/shader_preprocessor.html

Directives start with `#` spans to the end of line, multiple lines if they end with `\`
* The first line break that is not featuring a backslash terminates the directive

### #Define
`#define <identifier> [replacement_code]`
* replaces `identifier` with `replacement_code` on a whole-word basis
* `replacement_code` is optional but the macro can only be used with `#ifdef` and `#ifndef`
* `#define <identifier>([args]) [replacement_code]`
	* arguments are substituted on a whole-word basis
* `##` concatenates
	* It is removed along with any space surrounding it, allowing the creation of new symbols
	 ```glsl
	 #define SAMPLE(N) vec4 tex##N = texture(material##N, UV)
	 ...
	 SAMPLE(0) // Declares and initializes tex0
	 ALBEDO = TEX0.rgb // 
	 ```
### `undef <identifier>`
### `#if <condition>`
	

## Shader Types

### Spatial

### CanvasItem

### Particle

### Sky

### Fog

## Compute Shaders






> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTUxMDk1NzM2MCwxMjI1NzA5Njc0LDIwOT
cxODk1NTksLTIwNDU5Mzk0NDcsLTExMzE3MjcwOSwtNzU0NDgy
NjMyLDEyOTY2MjQwNCwzODY0Njg4NjUsLTM0NTI4MjExLC0xOT
M5NDI3MzA5LC04NTQyMTg4NTQsLTEwMjc0ODU4OTAsNDkyMjYz
OTUyLC0zMTM1NTE3MzcsLTE5NzkzMDkwMzFdfQ==
-->