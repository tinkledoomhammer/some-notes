
## Background Loading
https://docs.godotengine.org/en/stable/tutorials/io/background_loading.html#doc-background-loading



### Example
```python
const ENEMY_SCENE_PATH : String = "Enemy.tscn"

func _ready():
	ResourceLoader.load_threaded_request(ENEMY_SCENE_PATH)	
	self.pressed.connect(_on_button_pressed)

func _on_button_pressed(): 
	# Obtain the resource now that it is needed
	var enemy_scene = ResourceLoader.load_threaded_get(ENEMY_SCENE_PATH)
	# Instantiate the scene
	var enemy = enemy_scene.instantiate()
	add_child(enemy)

```

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTYwODExMDk0MV19
-->