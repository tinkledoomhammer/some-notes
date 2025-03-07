
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


### Language Reference
https://docs.godotengine.org/en/stable/tutorials/shaders/shader_reference/shading_language.html






> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTI0NTMzNzc2NCwtMTMxMjY4MDA2LC0xND
k1ODYxOTc5XX0=
-->