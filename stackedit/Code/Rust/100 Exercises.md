# 100-exercises-to-learn-rust.pdf
https://rust-exercises.com/100-exercises/04_traits/10_assoc_vs_generic.html

Use `wr` to check exercises


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
**Most references are stored internally as a pointer**  `<usize>`
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

```rust
pub trait From<T>: Sized{
	fn from(value: T) -> Self;
}

pub trait Into<T>: Sized{
	fn into(self) -> T;
}
```
The syntax above implies that `From` is a **subtrait** of `Sized`.
* Any type that implements `From` must also implement `Sized` `Sized` is a **supertrait** of `From`

#### Implicit trait bounds
* Every generic type parameter is implicitly `Sized`

Negative trait bounds
```rust
pub struct Foo<T: ?Sized> {...}
// T doesn't have to be 'Sized'
```
`Sized` is the only trait that can be used with negative bound

Dual traits
: Pairs of traits where implementing one implements the other
```rust
impl<T,U> Into<U> for T
where
	U: From<T>,
{
	fn into(self) -> U {
		U::from(self)
	}
}
```

### 4.10 Generics and associated types
Trait examples
```rust
pub trait From<T>{
	fn from(Value: T) -> Self;
} // generic 

pub trait Deref{
	type Target;
	fn deref(&self) -> &Self::Target;
} // associated type ("Target")
```

* In `From<T>` `from` can be implemented any number of times for a single type i.e (see the next code block)
* In `Deref` each implementation for a given `for` type must use the same `Target` type

Implementation examples
```rust
impl From<u32> for WrappingU32{
	fn from(value:u32) -> Self{ ... }
}

impl From<u16> for WrappingU32 {
	fn from(value: u16) -> Self { ... }
}

impl Deref for Asdf
	type target = String;
	fn deref(&self) -> String {...}
	// alternatively
	fn deref(&self) -> Deref::target { ... }
```
#### Combining both
```rust
pub trait Add<RHS = Self> {
	type Output;
	fn add(slef, rhs: RHS) -> Self::Output;
}

impl Add<u32> for u32{
	type Output = u32; 
	// Now whenever Add<T> is implemented for u32
	// the Output type must be the same?
	fn add(self, rhs: u32) -> u32 {
		// ...
		// the return type could also be Self::Output
		// or <Self as Add>::Output
	}
}

impl Add<&u32> for u32{
	type Output = u32;
	fn add(self,rhs: &u32) -> u32{
		// ...
	}
```

### 4.11 `Clone`Trait
Ownership rules:
1. Every value has a single owner at any given time
2. When a function takes ownership of a value ("consumes" it) the caller can't use that value anymore

`Clone` trait
: `pub trait Clone { fn clone(&self) -> Self; }`
* takes a ref to self and returns an **owned** instance of the same type
* Should be a deep copy
* used by calling `.clone();` 

Implementing Clone
* the default implementation will clone each member of a struct
* so you can `#[derive(Clone)]`

### 4.12 `Copy` trait

```rust
pub trait Copy: Clone { }

//implemented with #[derive()]
#[derive(Copy, Clone)]
struct MyStruct{...}
```
Restricted to types that 
1. Don't manage any additional resources
2. Are not mutable references 

Effects
1. Allows automatic/implicit cloning
2. The clones will be **bitwise copies** of the original

### 4.13 `Drop` trait
```rust
pub trait Drop{
	fn drop(&mut self);
}

impl Drop for MyStruct{
	fn drop(&mut self){
		// any cleanup
		// even an empty function will cause deallocation
	}
}
```
**`Drop` and `Copy` cannot be implemented on the same type**
* trying will produce a compiler error

### Traits Wrapup
* Don't make a function eneric if it is always invoked with a single type.
* Don't create a trait if you only have one implementation
* Implement standard traits for custome types (`Debug`, `PartialEq`, etc)
* Implement traits from 3rd party crates to use their functionality
* Beware making generics solely to use mocks. see the [testing masterclass](https://github.com/mainmatter/rust-advanced-testing-workshop) for better options


## 05 Modelling a Ticket, pt 2
Covers
1. `enum` s
2. The `Option` and `Result` type for nullable returns and recoverable errors
3. The `Debug` and `Display` traits for printing
4. The `Error` trait to mark error types
5. The `TryFrom` and `TryInto` traits for fallible conversions
6. Rust's Package system

### 5.1 Enums
```rust
enum Status{
	ToDo,
	InProgress,
	Done,
}

let status: Status = Status::ToDo;
```
#### `match`
```rust
impl Status{
	fn is_done(&self) -> bool {
		match self{
			Status::Done => true,
			Status::InProgress | Status::ToDo => false
		}
	}
}
```
* Like a type-level if
* no pass through, must exhaust all options
The general format
```rust
match <var> {
	pattern1 => result1,
	pattern2 => result2,
	_ => defaultResult,
}
```
* `_` is a wildcard pattern that will catch any unmatched variants
* If it is used, the compiler-driven refactoring won't be an option

Compiler driven refactoring
: using `enum`s and `match` statements so that the adding a new option to the `enum` will generate compiler errors in each place the code must be updated 

#### Variants can hold data








> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTg5OTY4NjE5LC0yMTQwNDMxNTQsLTYwOT
AxMjQwMywtMTg4OTA0Njk3MywtMjExNzU0MDA2OSwtNTMwNDk3
NDg1LDE2NzM3Nzc2MDIsLTUwMDc0Njk4NSwtMTQ5MDczNjQ4OS
wtMTE5Mzg2NDcxMywxODY3ODA3NjkxLDEyODI5NzA5NzUsLTE4
MDcxMjA4NywtMTQyMzQ2MTAxMCwtODY5MjMwMTgsLTc0MTY2Mz
gyNiw5OTQxODA3MzMsMTY0NDAxMTMwOSwyMTQyMzk4NzcxLDQw
MzM2MTMwNF19
-->