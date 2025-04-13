## GDScript: An introduction to dynamic languages

https://docs.godotengine.org/en/stable/tutorials/scripting/gdscript/gdscript_advanced.html

## Custom Iterators
Use `iter_init`, `_iter_next` and `_iter_get`
```
class ForwardIterator:
	var start
	var current
	var end
	var increment

	func _init(start, stop, increment):
		self.start = start
		self.current = start
		self.end = stop
		self.increment = increment

	func should_continue():
		return (current < end)

	func _iter_init(arg):
		current = start
		return should_continue()

	func _iter_next(arg):
		current += increment
		return should_continue()

	func _iter_get(arg):
		return current

#Usage:
var iter = ForwardIterator.new(0,6,2)
for i in iter: print(i) # 0,2,4
```
### Duck typing
use `object.has_method("methodName")`
```
func _on_object_hit(object):
	if object.has_method("smash"):
		object.smash()
```


## GDScript exported properties
https://docs.godotengine.org/en/stable/tutorials/scripting/gdscript/gdscript_exports.html

* Exported properties are saved along with resources (like the scene)
* They are available for editing in the property editor
* Exporting is done using the `@export` annotation
* Export variables must be initialized to a constant expression and/or have a type specifier
#### Basic Use
```
@export var number = 4
@export var other: int

@export var resource: Resource
@export var node: Node
```

#### Grouping export
* `@export_goup("group")` to group all subsequent exports
* end by using a different export group
	* an empty string is equivalent to no group
* `@export_subgroup("subgroup name")` to nest
* there are also export categories
	* `@export_category("Main Category")` 
	* It seems like `@export_category` can change the default category?
	* categories are higher level than groups
	* they aren't collapsible
	* they are presented visually in the same style as parent classes
#### Strings as paths
```
@export_file var f
@export_dir var d
@export_file("*.txt") var f

#Pick a file from the global filesystem
@export_global_file("*.png") var gf
@export_global_dir var gd
```
Also: `@export_multiline var text`
#### Limiting editor input ranges
```
# Basic format : @export(min,max,increment)
@export_range(0,20) var i1
@export_range(-10,20,0.2) var f1: float

#String arguments have special meanings
@export_range(1,100,1,"or_less", "or_greater") var i3: int
@export_range(0,1000,0.01, "exp") var f2: float
@export_range(0,1000,0.01, "hide_slider") var f3: float
etc
```
String args
* `"or_less"`, `"or_greater"` - allow setting outside the specified range by typing or dragging when not clicked on the slider
* `"exp"` uses an exponential scale rather than a linear scale for the slider
* `"hide_slider"` prevents showing the slider at all
* `"suffix:m"` sets a suffix (like for units and whatnot)
* `"radians_as_degrees"` used for angles because built-in functions use radians
	* also sets the degree sign as the suffix: `°`

#### Easing hints
`@export_exp_easing var num`
: exports a floating-poit property with an easing editor widget
* additional hints can be provided as string args
	* `"attenuation"` flips the curve
	* `"positive_only"` limits values to be $>=0$


#### Colors
`@export var col: Color`
* exports with a type of `Color` will use a color picker in the editor
`@export_color_no_alpha ...` is the same but with no alpha

#### Nodes
* When the export is a `Node` type then you can drag a node from the current scene
* A class that inherits `Node` will filter options i.e. `@export var x : BaseButton`
* There is an older `NodePath` type that is similar
	* converting a `Node` to a `NodePath` uses the `get_node(node_path)` function
	* `@export_node_path var nodepath` for unfiltered
	* `@export_node_path("Button")` for filtered 

#### Resources
When the export type is or inherits from 1`Resource` then additional options are available
* It is best to provide a more specific type than `Resource` because the list is long and won't be as type-safe


#### Exporting bit flags
https://docs.godotengine.org/en/stable/classes/class_%40gdscript.html#class-gdscript-annotation-export-flags
`@expoort_flags(...) var varname : int`
* the must be `int` `Array[int]` `PackedByteArray` `PackedInt32Array` or `PackedInt64Array`
* The argument must be a list of the flag names
* Flag values can be assigned manually or automatically
* Manual assignment : `@export_flags("Self:4", "Allies:8", "Self and Allies: 12", ...`
* auto asignment will start with `1` and subsequent flags ill be 2x the previous
* If Explicit and auto are mixed, then implicit values will not consider explicit values i.e.
	* `@export_flags("A:16", "B", "C") var x` will have `B:2` and `C:4`
* Values must be in the range $[1..2^{32}-1]$

There are special options for exporting layer selections: `@export_flags_...` where ... can be 
* `2d_physics` `2d_render` `2d_navigation` `3d_physics` `3d_render` `3d_navigation`

#### Enums
https://docs.godotengine.org/en/stable/classes/class_%40gdscript.html#class-gdscript-annotation-export-enum
* Exports with an enum type are represented in the editor with a drop-down box
* `@export_enum(...)` has the same syntax as `@export_flags`
* the type can also be `String`

#### Exporting arrays
* Default values must be constant expressions
* The editor will allow adding and removing items to the array
* `@export var ints: Array[int] = [1,2,3]`
* If no initializer is provided then `null` will be assigned
* `Packed...Array`s can also be used, but can only be initialized empty
* other variants can be used with arrays: `@export_range` `@export_enum` etc


#### `@export_storage`
* exports fields that are not shown in the inspector
* They are still serialized with the scene and will be duplicated by `Node.duplicate()` and `Resource.duplicate()`


#### `@export_custom`
Used to specify a variety of hints. see https://docs.godotengine.org/en/stable/classes/class_%40gdscript.html#class-gdscript-annotation-export-custom


#### `@export_tool_button`
* used in tool scripts
* Exports a callable as a clickable button
* `@export_tool_button("Hello", "Callable") var hello_action = hello`
		* The button is titled "Hello" and has the "Callable" icon
		* The field is "hello_action" and clicking calls the member function `hello()`

#### Setting exported variables from a tool script
If a tool script changes the value, the inspector won't automatically update unless `notify_property_list_changed()` is called

#### Advanced Exports
Object has some important methods:
 `Object._set()` `._get()` and `._get_property_list()` see https://docs.godotengine.org/en/stable/classes/class_object.html#class-object-private-method-get
 see also https://docs.godotengine.org/en/stable/tutorials/best_practices/godot_interfaces.html#doc-accessing-data-or-logic-from-object
and also https://docs.godotengine.org/en/stable/contributing/development/core_and_modules/object_class.html#doc-binding-properties-using-set-get-property-list ( for C++ )



## GDscript documentation comments
* must start with `##` (double hash symbols)
* Must immediately precede:
	* a script member the top of the code (for function and script docs)
	* an exported variable
#### Documenting a script
* Should be divided into 3 parts
	1. A brief description
	2. A detailed description
	3. Tutorials and deprecated/experimental marks
	3. 
#### Documenting Script members
Members that can be documented:
* Signal, enum, enum value, constant, variable, function, inner class
* Documentation of a member must be immediately preceeded by its annotations
* Lines with no `##` will not be considered part of the documentation

#### Tags
Item | Tag example
-|-
Brief Description | No tag, just used at the beginning of the doc section
Description | No tag. Use one blank line to saparate from the brief description
Tutorial | `@tutorial: https://example.com` or `@tutorial(Title Here): https://exmaple.com`
Deprecated | `@deprecated` or `@deprecated: Use [AnotherClass] instead.
Experimental | `@experimental` or `@experimental: This class is unstable.`

#### big Example
```
extends Node2D
## A brief description of the class's role and functionality.
##
## The description of the script, what it can do,
## and any further detail.
##
## @tutorial:             https://example.com/tutorial_1
## @tutorial(Tutorial 2): https://example.com/tutorial_2
## @experimental

## The description of a signal.
signal my_signal

## This is a description of the below enum.
enum Direction {
	## Direction up.
	UP = 0,
	## Direction down.
	DOWN = 1,
	# ...
}

## The description of a constant.
const GRAVITY = 9.8

## The description of the variable v1.
var v1

## This is a multiline description of the variable v2.[br]
## The type information below will be extracted for the documentation.
var v2: int

## If the member has any annotation, the annotation should
## immediately precede it.
@export
var v3 := some_func()

## As the following function is documented, even though its name starts with
## an underscore, it will appear in the help window.
func _fn(p1: int, p2: String) -> int:
	return 0

# The below function isn't documented and its name starts with an underscore
# so it will treated as private and will not be shown in the help window.
func _internal() -> void:
	pass

## Documenting an inner class.
##
## The same rules apply here. The documentation must
## immediately precede the class definition.
##
## @tutorial: https://example.com/tutorial
## @experimental
class Inner:
	## Inner class variable v4.
	var v4

	## Inner class function fn.
	func fn(): pass
```
Note: No space is allowed between the tag name and the `:`
Note2: When the description spans multiple lines, they will be joined and trimmed with a single space between. use `[br]` for manual breaks



#### BBCode and class references
Tag | | Example/result
-|-|-
`[Class]` | Link to a class | Move the [Sprite2D].
`[annotation Class.name]` | Link to annotation | See [annotation @GDScript.@rpc].
`[constant Class.name]`` | link to constant| See [constant COlor.RED].
`[enum Class.name]` | |
`[member Class.name]` | |
`[method Class.name]` | |
`[constructor Class.name]` | | Use [constructor Color.Color]
`[operator Class.name]` | | Use [operator Color.operator*].
`[signal Class.name]` | | 
`[theme_item Class.name]` | | see [theme_item Label.font].
`[param name]` | param name, not linked | Takes [param size] for the size.
`[br]` | line break | 
`[lb]` and `[rb]` | escapes `[` and `]` | [lb]asdf[rb] -> [asdf]
`[b]`..`[/b]` | bold text | Do [b]not[/b] call this method.
`[i]`..`[/i]` | italics |
`[u]`..`[/u]` | underline |
`[s]` ..`[/s]` | strikethrough |
`[color]`..`[/color]` | color | [color=red]Error![/color]
`[font]`..`[/font]` | | [font=res://mono.ttf]License[/font]
`[img]`..`[/img]` | image | [img width=32]res://icon.svg[/img]
`[url]`..`[/url` | link type1 | [url]https://example.com[/ur]
_ | link type 2 | [url=https://exmaple.com]Website[/url]
 `[center]`..`[/center]` | centering text | 
 `[kbd]`..`[/kbd]` | keyboard/mouse shortcut | Press [kbd]Ctrl + C[/kbd].
 `[code]`..`[/code]` | code fragment | Returns [code]true[/code]
 `[codeblock]` .. `[/codeblock]` | multiline codeblock

Notes:
1. Currently only [@GDScript](https://docs.godotengine.org/en/stable/classes/class_%40gdscript.html#class-gdscript) has annotations
2. blocks created with `[kbd]` `[code]` and `[codeblock]` disable BBCode until the end of the block
3. Inside codeblocks, always use **four spaces** or indentation. tabs will be deleted
4. `[codeblock]` has an optional `lang=` attribute
	* `lang=gdscript` is the default and provides syntax highlighting
	* `lang=text` disables syntax highlighting
	* `lang=csharp` uses C# syntax


## GDScript Style guide

#### Example
```
class_name StateMachine
extends Node
## Hierarchical State machine for the player.
##
## Initializes states and delegates engine callbacks ([method Node._physics_process],
## [method Node._unhandled_input]) to the state.

signal state_changed(previous, new)

@export var initial_state: Node
var is_active = true:
	set = set_is_active

@onready var _state = initial_state:
	set = set_state
@onready var _state_name = _state.name


func _init():
	add_to_group("state_machine")


func _enter_tree():
	print("this happens before the ready method!")


func _ready():
	state_changed.connect(_on_state_changed)
	_state.enter()


func _unhandled_input(event):
	_state.unhandled_input(event)


func _physics_process(delta):
	_state.physics_process(delta)


func transition_to(target_state_path, msg={}):
	if not has_node(target_state_path):
		return

	var target_state = get_node(target_state_path)
	assert(target_state.is_composite == false)

	_state.exit()
	self._state = target_state
	_state.enter(msg)
	Events.player_state_changed.emit(_state.name)


func set_is_active(value):
	is_active = value
	set_physics_process(value)
	set_process_unhandled_input(value)
	set_block_signals(not value)


func set_state(value):
	_state = value
	_state_name = _state.name


func _on_state_changed(previous, new):
	print("state changed")
	state_changed.emit()


class State:
	var foo = 0

	func _init():
		print("Hello!")
```
#### Formatting
General (editor defaults
: use a single `LF` to terminate lines
: use UTF-8 witout byte order mark
: use **Tabs** for indentation

Indentation
: Code-block indent levels should be a single tab
: line continuations should be 2 tabs
	* except arrays, dictionaries, and enums which use a single tab

Trailing coma
: use a trailing comma in multiline lists
: don't use a trailing coma in single-line lists

Blank lines
: surround functions and class definitions with 2 blank lines

One statement per line
: one statement per line except for the ternary operator

Format multi-line statements for readability
: break on `else` and such if the line would be really long

Avoid unnecessary prentheses
: only use them when necessary for the order of operations

Boolean operators
: prefer the plain english versions (`and` instead of `&&` etc)
: parens can be used to make longer expressions more readable

Comment spacing
: Regular and documentation comments should begin with a space
: commented-out code and regions should not begin with a space
* prefer comments on their own line, expecially if they are long

Whitespace
: use one space around operators and after comas
: Avoid extra spaces in dictionaries and function calls
* include one space for single-line dictionaries 
* **don't** use spaces to align expressions vertically

Quotes
: Don't use single quotes unless that would require escaping fewer characters

Numbers
: Don't omit leading or trailing `0` in floats
: Use lowercase for hex
: Use `_` to break up large strings of digits

#### Naming Conventions

Type | Convention | example
-|-|-
File names | snake_case | `yaml_parser.gd`
Class names | PascalCase | `class_name YAMLParser`
Node names | PascalCase | `Camera3D, Player`
Functions | snake_case | `func load_level():`
Variables | snake_case | `var particle_effect`
Signals | snake_case | `signal door_opened`
Constants | CONST_CASE | `const MAX_SPEED = 200`
Enum names | PascalCase | `enum Element{..`
Enum members | CONST_CASE | `{EARTH, WATER, ...`

File names
: should be the snake-case version of the PascalCase class name

Classes and Nodes
: Use pascal case for the class name and `const`s that hold a `preload`

Functions and variables
: Snake case
* use `_` at the beginning for
	* virtual methods that the user must override
	* private functions
	* private methods

Signals
: **past tense** snake case

Enums and constants
: All caps for the constants
: The enum name should be pascal case
* Each enum member should be all caps on its own line
*

#### Code order
```
01. @tool, @icon, @static_unload
02. class_name
03. extends
04. ## doc comment

05. signals
06. enums
07. constants
08. static variables
09. @export variables
10. remaining regular variables
11. @onready variables

12. _static_init()
13. remaining static methods
14. overridden built-in virtual methods:
	1. _init()
	2. _enter_tree()
	3. _ready()
	4. _process()
	5. _physics_process()
	6. remaining virtual methods
15. overridden custom methods
16. remaining methods
17. subclasses
```
* Methods and variables should have public before private

Rules of Tumb
1. Properties and signals first, followed by methods
2. Public before private
3. Virtual callbacks before the class interface
4. Object construction and initialization (`_init` and `_ready`) before functions that modify the object at runtime

Signals and Properties
: Docstring -> signals ->properties
* Enums should come after signals (to be used as export hints???)
* then constants, exported variables, public, private, and onready variables in that order

Variables
: should be declared where used
* Use locals instead of class members where possible
* declare locals as close to their first use as possible

Methods and static functions
: Right after properties
* Initializers first, in the order they are called
	* `_init()` first
	* then `_ready()`
* Then built-in virtual callbacks
	* `_unhandled_input()` `_physics_process()` etc
* Then other public methods
* then private methods

#### Static typing
**Avoid the following**
```
# Typed as int, but it could be that float was intended.
var health := 0

# The type hint has redundant information.
var direction: Vector3 = Vector3(1, 2, 3)

# What type is this? It's not immediately clear to the reader, so it's bad.
var value := complex_function()
```


## Static typing in GDScript
https://docs.godotengine.org/en/stable/tutorials/scripting/gdscript/static_typing.html
#### Some settings
* Text Editor > Completion > Add Type Hints (editor settings)
* consider using some warnings that will be described below

#### How to use
`var name: type = val`
or 
`var name := val` to infer the type

Constants don't need type hints except for typed arrays:
`const A: Array[int] = [1,2,3]` because arrays are untyped by default

Allowed Type hints
1. `Variant` "any type" - similar to not specifying any type except
	* when used as a return type it forces some value to be returned
2. `void` - only for return types
3. Built-in types 
4. Native classes (`Object` `Node` etc)
5. Global classes
6. Inner classes
7. Global, native, and named custom enums. this is the same as `int`
	* There is no guarantee that the assigned values will be valid
8. Constants that contain a preloaded class or enum

Class names
: Can be specified with `class_name` or `const ClassName=preload(...`

`->`
: specifies a return type
* void for functions that don't return a value
* `Variant` is also allowed, to force that some value must be returned

Covariance and contravariance
: Follow the [Liskov substitution principle](https://en.wikipedia.org/wiki/Liskov_substitution_principle)

Covariance
: when inheriting a method, you can specify a more specific  (subtype) **return** type than the parent

Contravariance
: When inheriting a method **parameters** scan e a less specific type (supertype)

Specify the element type of an Array
: `var varname: Array[type] = [1,2,...]`
* array types are used with
	* `for` loop variables
	* some operators like `[]` `[]=` and `+`
* Variant is still used for
	* array methods (`push_back()`, `map`, etc
	* some other operators such as `==`
* Nested array type arrays are not supported
TODO: statically typed dictionary


#### Type Casting
` var player := body as PlayerController`
* will be `null` when `body is PlayerController` is false

Three ways to do type casting safely 
```
if not (body is PlayerController):
if body is not PlayerController:

let player = body as PlayerController
	if not player:
```
* Casting will allow full autocomplete
* The is keyword is safer
* If the conversion fails for most types, the operation will *silently* return `null`
* If a cast with a built-in type fails, it will throw an error

Safe Lines
: A tool to tell when ambiguous lines of code are type-safe
: Shown with green line numbers in the editor (gray for unsafe lines)


#### Warning system and safe counterparts to unsafe operations
https://docs.godotengine.org/en/stable/tutorials/scripting/gdscript/warning_system.html

To enable: Project Settings -> Debug -> GDScript (with **Advanced settings** enabled

`UNTYPED_DECLARATIONN` - warning is appropriate if static types are desired in all circumstances
`INFERRED_DECLARATION` -  to warn if the less verbose syntax is used (???)

`UNSAFE_*` warnings
: make many unsafe operations more noticeable
: does not include all cases

`UNSAFE_PROPERTY_ACCESS` and `UNSAFE_METHOD_ACCESS`
: Caused by duck-typeing, even after using `in` , `has_method()`, etc
* To avoid it, cast to a script that defines the properties or methods after using `is` to verify that the cast will work
* This can be done explicitely with the `as` keyword, or by assigning the object to a new variable with the specified type
* When using the `as` keyword to cast, the object can be test against `null` instead of using `is`

`UNSAFE_CAST`
: caused by casting from `Variant` to a real type
To avoid both warnings:
```
func _on_body_entered(body: Node2D) -> void:
	var label_variant: Variant = body.get("label")
	if label_variant is Label:
		var label: Label = label_variant
		label.text = name
```

#### Cases where you can't specify types
* Individual elements in an array
* Nested types i.e. typed array of typed arrays




## GDScript warning system
https://docs.godotengine.org/en/stable/tutorials/scripting/gdscript/warning_system.html

## GDScript format strings
https://docs.godotengine.org/en/stable/tutorials/scripting/gdscript/gdscript_format_string.html

See also `String.format()` 

Format strings use `%` as a special char. to include it as a literal, escape it with `%%`

#### Multiple placeholders
```
var  format_string  =  "%s was reluctant to learn %s, but now he enjoys it."
var  actual_string  =  format_string  %  ["Estragon",  "GDScript"]

print(actual_string)
# Output: "Estragon was reluctant to learn GDScript, but now he enjoys it."
```
#### Format Specifiers
Placeholder types
: only one per format specifier, must be the last character

character | meaning
-|-
s | Simple - converts to string by the same method as implicit conversion
c | a single unicode character; expects an unsigned 8-bit integer or single-char string
d | decimal integer; expects an integer or real number (will be rounded down)
o | an octal integer,
x | a lower-case hex int
X | an upper-case hex int
f | a decimal real
v | a vector; can be Vector2..4, and the int variants but all coordinates will be formatted as floats

Placeholder modifiers
: characters between `%` and the type

char | meaning
-|-
`+` | in numbers, show `+` sign if the number is positive
[integer] | set **padding** will be spaces if the int does not start with `0` the leading 0. 
`.`  | before `f` or `v` sets precision to 0. follow with a number to increase precision
`-` | pad to the right will always use space instead of `0`
`*`| **dynamic padding** expects an additional integer paremeter

#### Padding
* by default space-padds on the left
* when using numbers for both padding and precision with `.` 
	* the number before the `.`  sets the minimum overall length
	* the number after the `.` is the number of decimal digits
	* `print("%10.3f" % 10000.5555)` -> " 10000.556"
* Dynamic padding uses `*` with an optional `0` for zero-padding
* `"%*.*f" %[7,3,8.888]` -> `"   8.889"


### `String.format()`
in general the format is `some_string_format(vals [,placeholder)`
* vals can be a dictionary or an array


Placeholders
Type | example | placeholder
-|-|-
Infix (default) | `"Hi, {0} v{1}".format(["Godette",3.0"],"{_}")` | `"{_}"`
Postfix | `"Hi, 0% v1%".format(["Godette", "3.0"],"_%")` | `"_%"`
Prefix | `"Hi, %0 v%1".format(["Godette", "3.0"],"%_")` | `"%_"`
No index | `"Hi, {} v{}".format(["Godette", "3.0"],"{}")` | `"{}"`


Ways to format "aname v3.0"
Type | Style | String | Vals
-|-|-|-
Dictionary | key | "{n} v{v}" | `{"n": "aname","v":"3.0"}`
Dictionary | index | "{0} v{1} | `{"0": "aname", "1":"3.0"}`
Dictionary | mixed | "{0} v{v}" | `{"0":"aname", "v":"3.0"}`
Array | key | "{n} v{v}" | `[["n","aname"], ["v","3.0"]]`
Array | index | "{0} v{1} | `["aname","3.0"]`
Array | mix | "{n} v{0}" | `["3.0", ["n","aname"]]`
Array | no index | "{} v{}" | `["aname","3.0"]`

`String.format()` does not have a way to manipulate number representations
: Use it with the `%` operattor
* `"{0} v{1}".format(["aname", "%0.2f" % 3.0])`



> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE4NzI3OTk2NjYsLTE4NzE5NDc4MDRdfQ
==
-->