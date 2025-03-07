
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
* `vertex()` - used in [`canvas_item`](https://docs.godotengine.org/en/stable/tutorials/shaders/shader_reference/canvas_item_shader.html#doc-canvas-item-shader) and [`spatial`](https://docs.godotengine.org/en/stable/tutorials/shaders/shader_reference/spatial_shader.html#doc-spatial-shader) shaders
	* runs for every vertex in a mesh
* `fragment()` - used in `canvas_item` and `spatial` shaders
	* runs for every pixel in a mesh
	* uses  values interpolated from the output of `vertex()`
* `light()` - used in `canvas_item` and `spatial` 
	* runs for every pixel for every light
	* uses data from `fragment()`
* `start()` - used in [particle shaders](https://docs.godotengine.org/en/stable/tutorials/shaders/shader_reference/particle_shader.html#doc-particle-shader), once per particle when it spawns
* `process()` used in particle shaders, per particle per frame
* `sky()` - [sky shaders](https://docs.godotengine.org/en/stable/tutorials/shaders/shader_reference/sky_shader.html#doc-sky-shader)
	* per pixel in the radiance cubemep when it needs to be updated
	* also per pixel in the current screen
* `fog()` - [fog shaders](https://docs.godotengine.org/en/stable/tutorials/shaders/shader_reference/fog_shader.html#doc-fog-shader)
	* per froxel in the volumetric fog froxel buffer that intersects with the [`FogVolume`](https://docs.godotengine.org/en/stable/classes/class_fogvolume.html#class-fogvolume)
### Language Reference
https://docs.godotengine.org/en/stable/tutorials/shaders/shader_reference/shading_language.html






> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbOTkyMTc4MTcxLC0xMzEyNjgwMDYsLTE0OT
U4NjE5NzldfQ==
-->