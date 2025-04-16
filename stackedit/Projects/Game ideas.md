
## Really basic games for practice

### Tower defense
* Need enemies
* Tower types
* upgrades
* pathing/spawn points
* 



### Tactical combat


### State.io style rtt

### Turn-based RPG

## A good system for balancing and type management

* Exported resources get deleted when variable names change
	* Import/export from file? like a spreadsheet?
	* Put all of the unit data in a single autoload, json style but in gdscript?
		* Can't have constants with custom types. Perhaps there is a way with GDExtension, otherwise 
### Spreadsheet -> resource/gd
* Spread sheets 
* exported to csv or something
* a toolscript that 
	* parses and validates the data
	* saves it as a resource
	* produces one or more .gd files with constants
* Perhaps something better than spreadsheets. to more easily maintain integrity

### Resource -> csv -> resource/gd
* The editor is nice except when it silently deletes data


### Using a toolscript to rename varaibles
https://prvaak.cz/blog/safely-renaming-exported-variables-in-godot/

In the class being changed
```gdscript
@tool
class_name EnemyData
extends Resource

@export var hp: int
@export var hitpoints: int

func __migrate__():
    hitpoints = hp
```

Then have a  toolscript to do the conversion

```gdscript
@tool
extends EditorScript


const MIGRATE_METHOD_NAME := &"__migrate__"


func _run() -> void:
    print("Migrating resources")
    var resource_files := get_all_file_paths("res://", [".tres", ".tscn"])
    for resource_file: String in resource_files:
        var resource := ResourceLoader.load(resource_file)
        migrate_resource(resource)
        ResourceSaver.save(resource, resource_file)
    print("Done")


func migrate_resource(variant: Variant) -> void:
    if variant is not Resource:
        return

    var resource := variant as Resource
    if resource.has_method(MIGRATE_METHOD_NAME):
        prints("Migrating", resource.get_script().get_global_name())
        resource.call(MIGRATE_METHOD_NAME)
    for prop in resource.get_property_list():
        var type := prop[&"type"] as int
        var name := prop[&"name"] as StringName
        match type:
            Variant.Type.TYPE_OBJECT:
                migrate_resource(resource[name])
            Variant.Type.TYPE_ARRAY:
                migrate_array(resource[name])
            Variant.Type.TYPE_DICTIONARY:
                migrate_dictionary(resource[name])


func migrate_array(array: Array) -> void:
    for item in array:
        if item is Resource:
            migrate_resource(item)


func migrate_dictionary(dictionary: Dictionary) -> void:
    for key in dictionary.keys():
        var item = dictionary[key]
        if item is Array:
            migrate_array(item)
        elif item is Dictionary:
            migrate_dictionary(item)
        else:
            migrate_resource(item)


func get_all_file_paths(path: String, extensions: Array[String]) -> Array[String]:
    var file_paths: Array[String] = []
    var directory := DirAccess.open(path)
    directory.list_dir_begin()
    var file_name := directory.get_next()
    while file_name != "":
        var file_path := path + file_name if path.ends_with("/") else path + "/" + file_name
        if directory.current_is_dir():
            file_paths.append_array(get_all_file_paths(file_path, extensions))
        else:
            for extension in extensions:
                if file_name.ends_with(extension):
                    file_paths.append(file_path)
        file_name = directory.get_next()
    return file_paths
```

Finally, change the original file to the new version (and remove the `@tool` and old var.




> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjE0NTk1OTQwNywtNDg1OTMxMTE0LDEwOT
A2OTM1NDFdfQ==
-->