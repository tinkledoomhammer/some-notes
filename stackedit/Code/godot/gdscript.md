# GD Script docs
https://docs.godotengine.org/en/stable/tutorials/scripting/gdscript/index.html

also godot scripting in general 
https://docs.godotengine.org/en/stable/tutorials/scripting/index.html

How to read the Godot API
: https://docs.godotengine.org/en/stable/tutorials/scripting/how_to_read_the_godot_api.html

Overridable functions
: https://docs.godotengine.org/en/stable/tutorials/scripting/overridable_functions.html

GDScript Reference
: https://docs.godotengine.org/en/stable/tutorials/scripting/gdscript/gdscript_basics.html

## GDScript Reference (Basics)
https://docs.godotengine.org/en/stable/tutorials/scripting/gdscript/gdscript_basics.html

### History

### Example

```python
# (optional) icon to show in the editor dialogs:
@icon("res://path/to/optional/icon.svg")
```

### Identifiers
* Restricted to alphabetic + digits + `_` underscore
* Can not start with a digit
* case ssensitive

### Keywords
```python
if elif else
for while
match when
break continue pass return
class class_name extends
is in as
self super
signal
static const enum var
breakpoint
preload
await yield
assert
void
PI TAU INF NAN
```


### Operators

Operators | Description
-|-
`()` | Grouping
`x[index]` | Subscription
`x.attribute` | attribute reference
`foo()` | function call
`await x` | awaiting signals or coroutines
`x is node` `x is not node` | Type checking, see also `is_instance_of()`
`x**y` | power, similar to the `pow()` function
`~x` | bitwise not
`+x` , `-x` | Identity / Negation
`*` , `/`, `%` | Multiplication/Division/remainder
`+`, `-` | addition/concatenation and subtraction
`<<` , `>>` | Bit shifting
`&` | bitwise AND
`^` | bitwise XOR
[pipe] | bitwise OR
`==` `!=` `<` `>` `<=` `>=` | Comparison
`x in y` `x not in y` | Inclusion checking
`not x` `!x` | boolean NOT
`&&` `and` | boolean AND
`or ` [double pipe] | boolean OR
`x if y else z` | ternary if/else
`x as Node` | Type casting
`=` `+=` `-=` `*=` `/=` `**=` `%=` `&=` [pipe=] `^=` `<<=` `>>-` | Assignment

Caveats
1. If both operands of a division are ints, then integer division is performed
2. The `%` operator is only available for ints. `fmod()` is for floats
3. For negative `%` truncates instead of rounding down. See also `posmod()` and `fposmod()`
4. `==` and `!=` sometimes works with mixed types, but other times it is a runtime error
	* `is_same()` performs a stricter check and does not generate runtime errors
	* `is_equal_approx()` and `is_zero_approx()` work with floats to handle rounding errors

### Lieterals

Examples | Dscription
`null` | null value
`false` `true` | boolean values
`0x8f51` | base 16 integer
`0b1010` | binary int
`3.14` `58.1e-10` | floating point number
`"Hello"` `'Hello'` | Regular strings
`"""Hello"""` `'''Hello'''` | triple-quoted strings
`r"Hello"` `r'Hi'` | Raw strings
`r"""Hello"""``r'''hi'''` | tripple-quoted raw strings
`&"name"` | [StringName](https://docs.godotengine.org/en/stable/classes/class_stringname.html#class-stringname)
`^Node/Label` [NodePath](https://docs.godotengine.org/en/stable/classes/class_nodepath.html#class-nodepath)

Some constructs that look like literals but are not
`$NodePath` | Shorthand for `get_node("NodePath")`
`%UniqueNode` | Shorthand for `get_node("%UniqueNode")`

Integers and floats can be separated with underscores `_` to make the more readable

Regular string literals can contain escape sequences
Escape sequence | Expands to
--|--
`\n` |
`\t`  |
`\r` |
`\a` | Alert (beep)
`\b` | backspace
`\f` | form-feed / page break
`\v` | vertical tab
`\'` `\"` | single and double quote
`\\` | `\`
`\uXXXX` | UTF16 codepoint (hex, case insensitive)
`\UXXXXXXX` | Utf32 codepoint (hex, case-insensitive)

Unicode characters above `0xFFFF` can be represented 2 wats
1. as a UTF-16 surrogate paor `\uXXXX\uXXXX`
2. As a single UTF-32 codepoint `\UXXXXXX`

* Using a `\` followed by a newline inside a string will continue the string on the next line without inserting a newline char into the string

* Single quoted strings can contain double quotes and vice-versa
* Triple quoted strings can contain either type of quote unless there are more than 2 consecutively or it is near the beginning or the end (i.e. when it is ambiguous where the string begins or ends)

#### Raw string literals
* always encode the string as it appears in the source code
> A raw string literal doesn't process escape sequences, however it does recognize `\\` and `\"` (`\'`) and replaces them with themselves.. Thus a string can have a quote that matches the opening one, but only if it's preceded by a backslash.
```python
print(\tchar=\"\\t\"") # Prints:`	char="\t"'
print(r"\tchar=\"\\t\"")# prints:'\tchar=\"\\t\"`
```
Some strings cannot be represented with raw string literals:
	1. those with an odd number of backslashes at the end of a string, or
	2.  an unescaped opening quote inside the string


GDScript also supports format strings
https://docs.godotengine.org/en/stable/tutorials/scripting/gdscript/gdscript_format_string.html#doc-gdscript-printf

### Annotations

Annotations
: Special tokens in GDScript that act as modifiers to a script or its code and may affect how the script is treated by the engine or editor
: They start with the `@` special char and are specified by name

See the GDScript class reference 
https://docs.godotengine.org/en/stable/classes/class_%40gdscript.html#class-gdscript

for exports, 

* Annotations will modify the next statement that is not an annotation. ]
* Multiple annotations can be on the same line or different lines
* They can have arguments sent between parens `()` and separated by commas `,`

`@export` annotation
see the exports article
https://docs.godotengine.org/en/stable/tutorials/scripting/gdscript/gdscript_exports.html

`@onready` annotation
* Defers initialization of a member variable until `_ready()` is called
```python
var my_label
func _ready():
	my_label=get_node("MyLabel")

# Is equivalent to
@onready var my_label = get_node("MyLabel")
```

WARNING
specifying `@onready` and `@export` on the same member will result in the `@onready`  overriding the export


### Comments

anything from `#` to the end of the line is considered a comment

Some special keywords will be highlighted by the editor when they are in comments
Type(color) | Keywords
--|--
Critical (red) | `ALERT` `ATTENTION` `CAUTION` `CRITICAL` `DANGER` `SECURITY`
Warning (yellow) | `BUG` `DEPRECATED` `FIXME` `HACK` `TASK` `TPD` `TODO` `WARNING`
Notice (green) | `INFO` `NOTE` `NOTICE` `TEST` `TESTING`
These keywords are all case sensitive

Documentation Comments
: Start with `##`
: must appear above the item they are documenting or at the beginning of the file
: See https://docs.godotengine.org/en/stable/tutorials/scripting/gdscript/gdscript_documentation_comments.html

#### Code Regions
Collapsible sections of code in addition to the normal collapsible regions
```python
# This comment is outside the code region. It will be visible when collapsed.
#region Terrain generation
# This comment is inside the code region. It won't be visible when collapsed.
func  generate_lakes():
	pass
func  generate_hills():
	pass
#endregion

#region Terrain population
func  place_vegetation():
	pass

func  place_roads():
	pass
#endregion
```
Code regions can be nested


### Line continuation
Ending a line with a `\` backslash will continue the current line onto the next line

### Built-in types
With some exceptions
* Stack-allocated
* Passed as values
Those exceptions are `Object` `Array` `Dictionary` and packed arrays

#### Basic built-in types
`null`
: An empty data type
: Only types that can inherit from Object can have a `null` value
: `Variant` types must have a valid value at all times

`bool`
: `true` or `false`

`int`
: integers, equivalent to `int64_t` in c++

`float`
: `double` Note: `Vector2`, `Vector3` and `PackedFloat32Array` store 32-bit floating point values

`String`
: a sequence of chars in Unicode format

`StringName`
: An immutable unique string. Slower to create, especially when multithreading, but fast to compare. suitable for dictionary keys

#### Vector built-in types
`Vector2`
: Contains `x` and `y` fields

`Vector2i`
: int `x` and `y`

Rect2
: `position` and `size` and also an `end` field which is `position+size`

`Vector3` Vector3i`
: `x` `y` and `z`

`Transform2D`
: a 3x2 matrix used for 2D transforms

`Plane`
: a 3d plane in normalized form 
: contains a `normal` vector and a `d` scalar distance

`Quaternion`
: Used to represent 3D rotation

`AABB`
: Axis aligned bounding box -- like `Rect2` but its fields are `Vector3`s

`Transform3D`
: Contains a `basis` and a `Vector3` `origin`

#### Engine built-in types
`Array`
: a sequence of arbitrary objects
: passed by **reference**
```python
var  arr  =  []
arr  =  [1,  2,  3]
var  b  =  arr[1]  # This is 2.
var  c  =  arr[arr.size()  -  1]  # This is 3.
var  d  =  arr[-1]  # Same as the previous line, but shorter.
arr[0]  =  "Hi!"  # Replacing value 1 with "Hi!".
arr.append(4)  # Array is now ["Hi!", 2, 3, 4].
```
`TypedArrays`
: Arrays that have a specific type
: Their methods still use `Variant` types
: `var  a:  Array[int]`
: Types cannot be mixed, even if one type is a subtype of the other
* Typed arrays can be converted with the `Array.assign()` method https://docs.godotengine.org/en/stable/classes/class_array.html#class-array-method-assign
#### Packed Arrays
`Packed<pseudotype>Array` (i.e. `PackedInt32Array`)
Allowed "types" : `Byte` `Int32` `Int64` `Float32` `Float64` `String` `Vector2` `Vector3` `Vector4` and `Color`

* usually faster than other arrays, but in the worst case they are a slow as untyped arrays
* They lack convenience methods like `Array.map(...)`
* Usually much faster to iterate
* Typed arrays are fairly fast on their own. The packed arrays are useful to reduce memory fragmentation for very large collections

`Dictionary`
: Associative arrays which contain values referenced by unique keys
```python
# style 1 (json)
var d = {4: 5, "A key": "A value", 28: [1, 2, 3]}
# Assign to an empty key
d["Hi!"] = 0
#alternative 'Lua-style` syntax
d = {
	22: "value",
	"some_key": 2,
	"other_key": [2, 3, 4],
	"more_key": "Hello"
}
```
TODO: Typed dicts

`Signal`
: a message that can be emitted by an object to those who listen to it

`Callable`
: contains an object and a function which is usefull for passing functions a values
* Getting a method as a member variable returns a callable `object.methodName` (no parens) 
* Can be called by invoking the call method. `object.methodName.call(x,y)` is the same as `object.methodName(x,y)`

### Variables
* can be class members or local to functions
* created with the `var` keyword
* may be assigned upon or ofter initialization
* They can have an optional type specification i.e. `var a : int = 0`
* Types can be inferred `var a :=0`
	* Type inferencing is not allowed for a `Variant` type
* If no type is specified or inferred then the var is a `variant`
`var a` by with no initialization creates an untyped var and initializes it to `null`

Valid types are:
1. Built-in types (`Array`, `int` , etc)
2. Engine classes (`Node`, `Resource`, etc
3. Constant names if they contain a script resource i.e. `const MyScript=preload("...`
4. Other classes in the same script, respecting scope
	* `OuterClass.NestedClass`
5. script classes declared with the `class_name` keyword
6. Autoloads registered as singletons

#### Initialization Order
1. Either `null` or  a type-specific value such as `0` `false` etc
2. Assigned in the order specified in the script, from top to bottom
	* For `Node`-derived classes, the `@onready` annotation moves initialization to step 5
3. if it is defined, `_init()` method
4. Export values (when instantiating scenes only)
5. (`Node`-derived only) - `@onready` vars
6. (`Node`-derived only) - if it is defined, the `_ready()` method

#### Static variables
`static var a:int=0`
* belong to the class, not the instances, and share values between multiple instances
* `static` variables cannot be `@export` `@onready`, or local to a function
* they can have type hints, getters, and setters
* Within the class they can be accessed without a prefix
* Elsewhere they can be accessed by prefixing with the class name or an instance: `Class.stat` or `inst.stat`
* They can also be accessed via a child class

`@static_unload` annotation
* must be placed at the top of the script (before `extends` and `class_name`)
* in theory the annotation allows the static variables to be freed when the script is no longer used
* there is currently a bug that prevents scripts from ever being unloaded
#### Casting
* Values can only be assigned to variables with compatible types
* Type coercion is done with the `as` operator
* Casting between `object` types results in the same object if the value is the same type of a subtype of the cast type
	* otherwise it is `null`
* For built-in types, a forcible conversion is performed if possible, otherwise an error is raised
* casting is recommended when working with the scene tree

### Constants

`const A = 5` 

#### Enums
* shorthand for a collection of integer constants
```python
# Automatic numbering 
enum {TILE_1,TILE_2 ,...}

#Is the same as
const TILE_1 = 0
CONST TILE_2 = 1
...

# Named Enums - give the enum a name to put the consts in a dictionary
enum State {STATE_1, STATE_2, ...}
# Is the same as 
const State = {STATE_1, ...}

#The keys are not registered as global constants. They are accessed like:
State.STATE_1
```
** There is also an array of the names? Prehaps it will be in the Dictionary section

### Functions
* Always belong to a class
* The priority for variable lookup is local -> class member -> global
* The `self` variable is always available and can be used to access class members
* A function can return at any time
* The default return value is `null`
* If a function body is only one line long, then it can be all on one line
```python
func square(a): return a*a
func cube(a): 
	return a*a*a
```
* The return value and parameters can also have a type specification
	* If the argument has a default value then the type can be inferred
	* ` func asdf(x:=0)->void`
* the return type can be expressed after the argument list with the `->` arrow token
* `void` may be used as a return type
	* `void` functions cannot return values
* if a return type is specified then the function **must** return a value of that type
#### Referencing and lambdas
* Accessing a function without calling it (i.e. no arguments) returns a `Callable` object
* Lambdas are created by assigning a function definition to a variable:
* `var lambda = func(x): print(x)`
* They are called with the `call()` method
* They can be named for debugging `var lam = func my_func(x): print(x)`
* Type specifiers can be used just like regular functions
* a `return` must be explicit
* Lambda functions capture the local environment
	* They are only captured once, so 
		* they cannot assign to outer variables
		* they will not detect when the outer function assigns its variables
		* mutable reference types are mutable
#### Static functions
* declared as static: `static func add(x,y): return x+y`
* They do not have access to `self` or any instance members
* static variables and static constructor are covered elsewhere in this doc

### Statements and control flow
`;` is optional as a statement separator
* statements can be assignments, function calls, flow control structures, etc.

Expressions
: sequences of operators and their operands i an orderly fashion
: they can also be statements
* Return values that can be assigned to valid targets
* Operands to some operators can also be expression
* Assignment is not an expression and does not return any value :<

self
: Refers to the current instance, often equivalent to accessing symbols directly
* can also access properties, methods, etc that are defined dynamically
	* i.e. in subtype of the current class (bad practice)
	* or using `_set()` and `_get()`

#### `if` `else` `elif`
```python
if (expression):
	statement(s)
elif (expression):
	statement(s)
else:
	statement(s)
# Short form used when the statement(s) block is a single short statement
if (expression): statement
# The ternary operator is the same but with no `:`
var x = val1 if expression else val2

# Ternary expressions can be nested but should be spread across lines:
var  fruit_alt  = \
		"apple"  if  count  ==  2 \
		else  "pear"  if  count  ==  1 \
		else  "banana"  if  count  ==  0 \
		else  "orange"		
```

#### While
```python
while(expression):
	statement(s)
```
#### for
```python
for x in [5,7,11]
	...
var names = ["a", "b"]
for name:String in names:
	...
for i in names.size():

for i in range(3): # 0,1,2
for i in range(1,3): #1,2
for i in range(2,8,2): # 2,4,6
for i in range(8,2,-2): # 8,6,4
for c in "Hello": #H,e,l,l,o
for i in 3: #similar to range(3) 0,1,2
for i in 2.2: # similar to range(ceil(2.2)) 0.0, 1.0, 2.0
```
Assigning to the local loop variable will not change the value in the array

#### Match
```
match  <test  value>:
	<pattern(s)>:
		<block>
	<pattern(s)>  when  <pattern  guard>:
		<block>
	<...>
```
* no fallthrough
* no break or case
* the pattern to match defaults is `_`
* The first match is the only one that will be executed
* Available patterns
	1. literals
	2. Expressions i.e. `match typeof(x):` ... `TYPE_FLOAT: `...
	3. Wildcard pattern: `_` will match anything
	4. Binding pattern, like `_` but with a variable name: `var new_var:`
	5. Array pattern: matches an array
		* every element of the pattern array is itself a pattern
		* the length is tested first
		* to match longer arrays use `..` i.e. `[42, ..]:` will match any array starting with `42`
		* wildcard patterns can be used as subpatterns
		* the sub-patterns are comma-separated
		* each pattern is enclosed in `[]`
	6. Dictionary pattern: every key has to be a constant pattern.
		* the size is tested first
		* the pattern must be in key:value pairs
		* if no value is specified in the pattern, then only the key is matched and any value will match
		* the match can be open ended with `..` at the end
		* each pattern is enclosed in `{}`
		* sub-patterns are comma-delimited 
		* i.e. `{"name": "Dennis", "age": var age, ..}:`
	7. Multiple patterns
		* comma separated
		* no bindings are allowed
		* eg `1,2,3:` to match 1 or 2 or 3

Pattern guard
: an optional condition that follows the pattern list and allows additional checks before chosing a branch
: can be an arbitrary expression, specified with the `when` keyword
* eg `[var x, var y] when y==x:`
* it will not be evaluated unless the pattern matches
* if the base pattern matches then the guard pattern is evaluated
* only if the guard pattern is true will the branch match


### Classes
> **By default all script files are unnamed classes.**
* the extends keyword can take an absolute or relative path
#### Registering named classes
* names are registered with the `class_name` keyword
* an icon can be associated with the `@icon` annotation
```
@icon(res://interface/icons/item/png)
class_name Item
extends Node
...
```
* `class_name` and `extends` can be on the same line
* Named classes are available globally without `load()` or `preload()`

* Non-static variables are initizlized every time an instance is created, including arrays and dictionaries
* The editor will hade custom classes that begin with the prefix "Editor" in the "Create new Node" and "Create new scene" dialog windows

A class that is stored as a file can inherit fro
* a global calss
* another class file
* an inner class inside another file
* inheritance uses the `extends` keyword
* if no inheritance is specified, then `extends RefCounted` is the default
* use the `is` keyword to determine if an instance inherits from a given class
* functions in a super class, use the `super` keyword
* if there is no name-shadowing, then the name of the parent-class member can be used without prefix
* most functions are virtual except some engine specific functions:
	* `get_class()` and `queue_free()` for example
* engine functions that can be overrided will be marked as `virtual` in the docs
	* such as `_ready()` and `_process()`
#### class constructor: `_init(arg)`
* can call the base class constructor with `super`
* Every class has an implicit constructor that is always called first (which initializes variables with the values specified outside of any function)
* `super` will only call the explicit constructor. the automatic one is already called
Some rules:
1. If the inherited class defines an `_init` that takes arguments, the nthe inheriting class *must* define `_init` and pass the appropriate parameters to `super`
2. The inheriting class can have a different number of arguments thatn the base class

Static Constructor
: `_static_init` is called automatically when the class is loaded, after static variables have been automatically initialized

Inner classes
: Additional classes inside the file declared with the `class keyword`
* `class SomeInnerClass:`
* These are accessible as `ParentClass.SomeInnerClass`

Classes as resources
* Classes at runtime are resource objects that can be loaded with the `load` or `preload` functions
* The object has a `new` method used to instantiate instances of the class





### Properties (setters and getters)
```
var milliseconds: int = 0
var seconds: int:
	get:
		return milliseconds / 1000
	set(value):
		milliseconds = value * 1000
# or methods can be used
var  my_prop:
	get  =  get_my_prop,  set  =  set_my_pro

# it can all be on one line
var my_prop: get = get_my_prop, set = set_my_prop
```
* Setters and getters are not called from inside themselves
* Setters can be called from inside getters and vice versa 
* Functions called from inside the setters and getters will still access the property via the setters and getters though

```
# infinite recursion
var my_prop:
	set(value):
		set_my_prop(value)
func set_my_prop(value)
	my_prop=value # This will call the setter which will call this function
```


### Tool mode
`@tool`
: the annotationn allows scripts to be run inside the editor instead of only in the game
https://docs.godotengine.org/en/stable/tutorials/plugins/running_code_in_the_editor.html


### Memory Management
* There is no garbage collector
* The `RefCounted` type use reference counting 
	* `Resource` inherits from `RefCounted`
* `Object` and its descendants are freed manually with the `.free()` method
* for the `Node` branch of object's descendants use `queue_free()` instead
	* This will also free all of that node's children recursively
* The `WeakRef` function is used to create weak references to avoid circular references
* the `is_instance_valid(instance)` function will return false if the `instance` has been freed

```python
extends Node
var my_file_ref
func _ready():
	var f = FileAccess.open("User://example.json", FileAccess.READ)
	my_file_ref = weakref(f)
	other_node.use_file(f)
func _this_is_called_later():
	var my_file = my_file_ref.get_ref()
	if my_file:
		my_file.close()
```

* Use `weakref(obj)` to get a weakref
* the `weakref` object has a `.get_ref()` method that will return an actual reference if the object is still valid

### Signals
Signals
: A tool to emit messages from an object to other listeners
* Custom signals are created with the `signal` keyword, as class members
* They have a `.connect(callable)` method that registers listeners
* and a `.emit()` method to call all the registered callbacks
* they can also be `await`ed
* `.emit()` can take arguments which are then passed to each callback
* forwarded arguments can be specified in the `signal` declaration 
	* `signal my_signal(a_val, b_val)`
	* then the callbacks should take the same args
	* the args should be passed to `emit()` but this is not enforced at compile time
	* the editor "Node" dock can generate callback functions automatically 
* The registered callbacks can be created with `Callable.bind()` to send additional information to specific callbacks
	* https://docs.godotengine.org/en/stable/classes/class_callable.html#class-callable-method-bind

#### Awaiting signals or coroutines
`await`
: keyword used to create coroutines
: causes execution to wait until the signal is emitted before coninuing
* usig the await keyword with a signal or a call to a function that is also a coroutine will immediately return control to the caller.
	* when the signal is emitted or the coroutine finishes, it will resume execution from the point where it stopped
* using a return value from a coroutine without using `await` will generate an error
* If the return value is unneeded then `await` is optional
* if `await` is used with an expression that is not a signal or coroutine then the value is returned immediately and the function will continue




### Assert keyword
* the assert keyword is only used in debug builds
* `assert(condition, message)` message is optional
* If the condition is false then an error with the specified message is generated
* 



### GDScript: An introduction to dynamic languages
https://docs.godotengine.org/en/stable/tutorials/scripting/gdscript/gdscript_advanced.html
### Custom Iterators
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
#### Duck typing
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


#### Arrays

#### `@export_storage`

#### `@export_custom`

#### `@export_tool_button`

#### Setting exported variables from a ool script

#### Advanced Exports


## GDscript documentation comments

## GDScript Style guide

## Static typing in GDScript

## GDScript warning system

## GDScript format strings

## Core features

### How to read the Godot API

### Debug

### Idle and Physics processing

### Groups

### Nodes and scene instances

### Overridable functions

### Cross-language scripting

### Creating script templates

### Evaluating expressions

### change scenes manually

### Instancing with signals


### Pausing games and process mode

### File system

### Resources

### Singletons (Autoload)

### Using SceneTree

### Scene Unique Nodes


> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE4MzQyMzIzNDQsLTQyNTUyMjIyLDEyMT
MyNzcwNjAsMTUzOTM4NDI3NSwtMTIzMTY5NjgwMSwzNDYwNzI3
NTQsLTgwMDI5ODI2NiwxNTYxMjg0NjExLC00ODA5NjgxMTcsLT
E0NTEyODY0MDMsNzk5MzYwOTg4LC0xNzUwMjQzNDUyLC05NDQw
MDg3NzYsLTcwMDczNDcwNF19
-->