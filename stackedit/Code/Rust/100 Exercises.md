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
1. Can't have a mutable and an immutable reference at the same time
2. can't have more than one mutable reference at the same time
3. The owner can't mutate a value while it is borrowed
4. Can have many mutable references at the same time, as long as there are no mutable reference


### 3.7 Mutable references
Methods with `mut self`
* should return `self`
```rust
impl Ticket {
	pub fn set_title(mut self, new_title: String) -> Self{
		self.title = new_title;
		self
	}
}
...
let ticket = Ticket::new(...);
let ticket = ticket
	.set_title("New title".into())
	.set_description(...)
	.set_status(...);
```
* allows chaining like above
* Takes ownership

Methods with `&mut self`
* doesn't take ownership
* cannot chain calls

### 3.8 Memory Layout
`std::mem::size_of::<T>()`
: Returns the size in bytes the type takes on the stack

String's Memory Layout (24 bytes on 64-bit)
* The *pointer* to the reserved memory
* the *length* of the string
* The *capacity* of the string (reservation)

### 3.9 References
* references put a 	`usize` on the stack (8 bytes)

#### Heap
* allocation and deallocation are much slower

`usize`
: a 32- or 64- bit unsigned int
: the type used for pointers, capacities, and lengths

* `std::mem::size_of::<T>()` doesn't count memory on the heap
* There is no standard mechanism for tracking the heap usage of particular objects

#### 3.10 References
**Most references are stored internally as a pointer ** `<usize>`
* Pointers don't always point to the heap

### 3.11 Destructors
* The user is responsible for memory management
* Features like the borrow checker mean that the user rarely has to manage Memory

Scope
: The region of Rust code where a variable is valid or **alive**
: Starts with the declaration and ends when either
: 1. The block where it was declared ends or
: 2. Ownership is transferred

Destructors
: invoked automatically when a value goes out of scope
: can be called manually by passing it to `std::mem::drop`

* Transferring ownership also transfers responsibility  to destruct



## 04 Traits

Some types of traits in the standard library:
1. Operator Traits (`Add`, `Sub`, `PartialEq`, etc)
2. `From` and `Into` for infallible conversions
3. `Clone` and `Copy` for copying values
4. `Deref` and `deref` coercion
5. `Sized` to mark types with a known size
6. `Drop` for custom cleanup logic

## 04.01 Traits

Traits
: Rust's way of defining **interfaces**

```rust
//Defining a trait
trait <TraitName>{
	fn <method_name>(<params>) -> <return_type>;
}

//Implementing a trait
impl <TraitName> for <TypeName> {
	fn <method_name>(<parameters>) -> <return_type>{
		//method body
	}
}
```

Traits are used with the `.` operator, just like methods
To invoke a trait, 2 things must be true:
1. The type must implement the trait
2. The trait must be in scope

A `use` statement may be required to get the trait in scope
	* unless the trait is defined in the same module
	* or it is defined in the standard library's **prelude**

Prelude
: A set of traits and types that are automtically imported into every Rust program
: `use std::prelude::*;` duplicates the effect of the auto-import

Inherent method
: A method defined directly on a type, without using a trait

### 4.2 Implementing Traits
* When a type is defined in a different crate, then trying to define new methods for it will generate a compiler error

Extension Trait
: a trait whose primary purpose is to attach new methods to foreign types (such as `u32`

Limitations on traits
1. You can't implement the same trait for the same type in the same crate twice

Orphan Rule
: Traits when multiple crates are involved must meet one of 2 rules
: 1. The trait is defined in the current trait or
: 2. The implementor type is defined in the current crate

### 4.3 Operator overloading

Operator Overloading
: The ability to define custom behavior for operators like `+`, `-`, `*`, `/`, `==`, `!=`, etc

* Operatos are traits

`+ - * / %` are  `Add Sub Mul Div Rem` all from `std::ops`
`==` and `!=` are `PartialEq` from `std::cmp`
`<` `>` `<=` `>=` are `PartialOrd` from `std::cmp`

Default Implementations
* specified in the trait definition by providing a body for the method
* for example `std::cmp::PartialEq` defines `.ne(...)`

### 4.4 Derive Macros
Destructuring
```rust
let Ticket {
	title, description, status
	} = self;
// equivalent to
let title=self.title; let description=self.desc....
```

Macros
: code generators
: often look like functions but their name ends with `!`

Derive macro
: A particular flavour of rust macro specified as an **attribute** on top of a struct

```rust
#[defive(PartialEq, Debug)]
struct Ticket{...}
//now PartialEq and Debug are implemented for Ticket
```

### 4.5 Trait Bounds
Traits can also be used for generic programming

Generics
: Code that works with a **Type parameter** instead of a concrete type

```rust
fn print_if_even<T>(n: T)
where
	T: IsEven + Debug
{
	fi n.is_even(){
		println!("{n:?} is even");
	}
}
```
Now `print_if_even` will work for types that implement both `IsEven` and `Debug`

Trait bounds can be inlined:
```rust
fn print_if_even<T: IsEven + Debug> (n: T{...}
```

Type parameter names should be in *upper camel case*

### 4.6 String Slices

`&str`
: A view of string data
* stores a pointer and length on the stack
* does not own the data it points to
* preferred over `&String`
* `String` has and owns exactly one heap allocated section of memory


### 4.7  `Deref` trait
aka **deref coercion**
* defined in `std::ops`
```rust
pub trait Deref{
	type Target;
	fn deref(&self)-> &Self::Target;
}
```
`Target` is called an associated type. It is a placeholder for a concrete type
```rust
impl Deref for String{
	type Target = str;
	fn deref(&self) -> &str{...}
}
```

### 4.8 `Sized`
* the `Sized` trait is implemented for fixed-size types
* it is an **auto** trait and a **marker** trait

Dynamically sized type (DST)
: a type whose size is not known at compile time
: references are **fat pointers**

Fat pointer
: a pointer with metadata

Marker Traits
: doesn't require any methods to be implemented
: doesn't define any behavior
: used by the compiler to enable certain behaviors or optimizations

Auto Traits
: does not be need to be implemented explicitly; the compiler does so automatically

`Sized` is an auto/marker trait that applies to all previously discussed types except `str`

Dynamically sized types like `str`

This allows for implicit conversion

### 4.9 `From` and `Into`
`std::convert` defines 2 traits for **infallible conversions** : `From` and `Into`




> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTc3MzY2NzM5LC0xNDIzNDYxMDEwLC04Nj
kyMzAxOCwtNzQxNjYzODI2LDk5NDE4MDczMywxNjQ0MDExMzA5
LDIxNDIzOTg3NzEsNDAzMzYxMzA0LDExMzg0MDQ0MDMsMTcxOT
Q2OTgwNywxOTE4NDY0NjUwLC0xNTI0NjU5NTA4LDQ3ODU2OTg0
OCwtMTAyMTU3Njg4MiwxNjE1ODk2NTksLTIxNDMxNDU0NDEsLT
E1OTEzNDUyOTQsMTk4MDM3NjYyOCwxMTM4MTA0MTA1LDEwMDMx
NzkyMjhdfQ==
-->