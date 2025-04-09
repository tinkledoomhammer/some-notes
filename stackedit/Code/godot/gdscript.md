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
`\n`
`\t`
`\r`
`\a` | Alert (beep)
`\b` | backspace
`\f` | form-feed / page break
`\v` | vertical tab
`\'` `\"` | single and double quote
`\\` | `\`
`\uXXXX` | UTF16 codepoint, hex, case insensitive
`\UXXXXXXX` | Utf32 codepoint (hex, case



### Annotations


### Comments

### Code Regions


### Line continuation

### Built-in types

### Variables

### Constants

### Functions


### Statements and control flow


### Classes


### Exports


### Properties (setters and getters

### Tool mode


### Memory Management


### Signals


### Assert keyword

## GDScript: An introduction to dynamic languages

## GDScript exported properties

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
eyJoaXN0b3J5IjpbLTEyMzk0NTEzNjUsLTE3NTAyNDM0NTIsLT
k0NDAwODc3NiwtNzAwNzM0NzA0XX0=
-->