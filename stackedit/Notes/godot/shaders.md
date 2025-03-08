
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
### precision, casts, members, construction
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
Swizzling
```hlsl
vec4  a  =  vec4(0.0,  1.0,  2.0,  3.0);
vec3  b  =  a.rgb;  // Creates a vec3 with vec4 components.
vec3  b  =  a.ggg;  // Also valid; creates a vec3 and fills it with a single vec4 component.
vec3  b  =  a.bgr;  // "b" will be vec3(2.0, 1.0, 0.0).
vec3  b  =  a.xyz;  // Also rgba, xyzw are equivalent.
vec3  b  =  a.stp;  // And stpq (for texture coordinates).
float  c  =  b.w;  // Invalid, because "w" is not present in vec3 b.
vec3  c  =  b.xrt;  // Invalid, mixing different styles is forbidden.
b.rrr  =  a.rgb;  // Invalid, assignment with duplication.
b.bgr  =  a.rgb;  // Valid assignment. "b"'s "blue" component will be "a"'s "red" and vice versa.
```

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
eyJoaXN0b3J5IjpbMTg5NDk0NjQwMiwtMTQzNDUzNzc0NywtMT
MxMjY4MDA2LC0xNDk1ODYxOTc5XX0=
-->