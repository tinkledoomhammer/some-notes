
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

## References
### Intro
https://docs.godotengine.org/en/stable/tutorials/shaders/introduction_to_shaders.html
#### Shader Functions
* `vertex()` - used in `canvas_item` and `spatial` shaders
	* runs for every vertex in a mesh
* `fragment()` - used in `canvas_item` and `spatial` shaders
	* runs for every pixel in a mesh
	* uses  values interpolated from the output of `vertex()`
* `light()` - used in `canvas_item` and `spatial` 
	* runs for every pixel for every light
	* uses data from `fragment()`
* `start()` - used in particle shaders, once per particle when it spawns
* `process()` used in particle shaders, per particle per frame
* `sky()` - sky shaders
	* per pixel in the radiance cubemep when it needss to be updated
	* also per pixel in the current screen
* `fog()` - fog shaders - per froxel in the volumetric fog froxel buffer that intersects with the `FogVolume`
### Language Reference
https://docs.godotengine.org/en/stable/tutorials/shaders/shader_reference/shading_language.html






> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIyODA4MzgxNCwtMTMxMjY4MDA2LC0xND
k1ODYxOTc5XX0=
-->