
## Background Loading
https://docs.godotengine.org/en/stable/tutorials/io/background_loading.html#doc-background-loading



### Example
```gdscript
const ENEMY_SCENE_PATH : String = "Enemy.tscn"

func _ready():
	ResourceLoader.load_threaded_request(ENEMY_SCENE_PATH)	
	self.pressed.connect(_on_button_pressed)

func _on_button_pressed(): 
	#
```

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjgwNDQ3MDQyXX0=
-->