# Core features
https://docs.godotengine.org/en/stable/tutorials/scripting/index.html#core-features

## How to read the Godot API

## Debug

## Idle and Physics processing

## Groups

## Nodes and scene instances

## Overridable functions

## Cross-language scripting

## Creating script templates

## Evaluating expressions

## change scenes manually

## Instancing with signals


## Pausing games and process mode

## File system
Example:
```
/project.godot
/enemy/enemy.tscn
/enemy/enemy.gd
/enemy/enemysprite.png
/player/player.gd
```
project.godot
: Defines the project root
* in `win.ini` format

Path delimeter
: `/` on all platforms

Resource path
: `res://` will always point to the project root
* r/w when running the project locally from the editor
* is read-only for exported projects

User path
: `user://` is always r/w on all platforms
* resolves to different host file systems depending on the os

Host file system
: technically accessible but not recommended

### Drawbacks
* moving assets will break existing references to those assets
	* moving, renaming, etc in the Godot editor will result in fewer broken references
* in windows and macOS path names are case insinsitive
	* using all lower-case seems like the best resolution
* 

## Resources

## Singletons (Autoload)

## Using SceneTree

## Scene Unique Nodes


> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTM3MTM3OTQxMywtMTAwMDkwNTY4OSw3NT
cwMDcxMjVdfQ==
-->