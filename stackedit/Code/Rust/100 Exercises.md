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

#### 3.5 Encapsulation






> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTk5MDE3Njk5MiwtNjU1MDI1NzkyLC0xMT
QyNDczODA4LC00NTc5Mzg1MjldfQ==
-->