
`COLOR` = vec4 with the output color
`UV` = vec2 with texture coordinates
`SCREEN_UV` = vec2 with screen coordinates in [0,1]

### Sampling the screen
```hlsl
uniform sampler2D SCREEN_TEXTURE : hint_screen_texture, filter_linear_mipmap;

```


> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTQwNTQxMTk5N119
-->