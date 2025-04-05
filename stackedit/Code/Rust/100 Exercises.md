# 100-exercises-to-learn-rust.pdf

## 3 Modeling a Ticket
### 3.3 Modules
Module
: A way to group related code together under a common namespace

Inline Modules
: The module declaration and the module contents (inside `{ ... }` are next to eachother

Crate Root
: Aka Root module
: usually `src/lib.rs`
: the namespace name is `crate`

Modules form a tree structure

#### External modules and the filesystem
`mod dog;` declares the existence of a submodule
* If it is declared in the root of the crate (`src/lib.rs` or `src/main.rs`) then cargo expects to find the file containing the submodule to boe in either:
	* `src/<module_name>.rs` or
	* `src/<module_name>/mod.rs`
* If it is a submodule of another (non-root) module then it should be named:
	* `[..]/<parent_module>/<module_name>.rs`
	* `[..]/<parent_module>/<module_name>/mod.rs`

#### Item Paths and `use` statements

* Items defined in the smae module don't require any special syntax
* For items in other modules the path can be composed in 3 ways
	* from the root of the current crate `crate::module_1::MyStruct`
	* from the parent module `super::my_function`
	* from the current module `sub_module::MyStruct`
* both `crate` and `super` are **keywords**

`use` statements
: bring the entity into scope
: `use crate::module_1::module_2::MyStruct;`
: `//MyStruct is now accessible in the current module`

`use crate::module_1::module_2::*;`
: imports all the items in the entire namespace
: This is discouraged in most cases because it clutters the namespace

### 3.4 Visibility

Private by default
: can only be accessed  in the module where its defined or
: in one of its submodules

Visibility Modifiers

`pub`
: Makes the entity accessible from outside the module, potentially outside the crate

`pub(crate)`
: Makes the entity public within the crate

`pub(super)`
: makes the entity public to the parent module

`pub(in path::to::module)`
: makes the entity public to the specified module

### 3.5 Encapsulation
* Private fields prevent the use of the constructor syntax
```rust
let ticket = Ticket {
	title: "Build a ticket system".into(),
	//...
};
```
* this means that a constructor must be provided
```rust
impl Ticket{
	pub fn new(title: String, ...) ->Ticket{
		//...
		Ticket{title, ...}
	}
	//or ... ?
	pub fn new(title: String, ...) ->Self ...
}
```
Accessor methods
: public methods that allow reading values of private fields

### 3.6 Ownership
The ownership system ensures that :
1. Data is never mutated while it is being read
2. Data is never read while it's being mutated
3. Data is never accessed after it has been destroyed

Owner
: Each value has a single owner at any given time
: statically determined at compile-time

**Each function has to declare in its signature how it wants to interact with arguments**

Move Semantics
: Ownership can be transferred
: Non-ref parameters **consume** their arguments
: ownership is *moved* to the function and the caller can't use it any more

Borrowing
: allows reading a value without taking ownership
: done by obtaining a reference

`&`
: Immutable references

`&mut`
: Allow reading and mutating the value

Restrictions that prevent simultaneous reading and mutating data
1. Can't have 




> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjAzODQxNDA4OCwyMDAzOTIwNzc4LC0zNz
Y0NTYxODIsLTE4NTk1NDM2NDgsMTk5MDE3Njk5MiwtNjU1MDI1
NzkyLC0xMTQyNDczODA4LC00NTc5Mzg1MjldfQ==
-->