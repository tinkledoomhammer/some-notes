# 100-exercises-to-learn-rust.pdf
https://rust-exercises.com/100-exercises/04_traits/10_assoc_vs_generic.html

Use `wr` to check exercises


## 3 Modeling a Ticket
### 3.03 Modules
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

### 3.04 Visibility

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

### 3.05 Encapsulation
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

### 3.06 Ownership
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


### 3.07 Mutable references
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

### 3.08 Memory Layout
`std::mem::size_of::<T>()`
: Returns the size in bytes the type takes on the stack

String's Memory Layout (24 bytes on 64-bit)
* The *pointer* to the reserved memory
* the *length* of the string
* The *capacity* of the string (reservation)

### 3.09 References
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

### 4.01 Traits

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

### 4.02 Implementing Traits
* When a type is defined in a different crate, then trying to define new methods for it will generate a compiler error

Extension Trait
: a trait whose primary purpose is to attach new methods to foreign types (such as `u32`

Limitations on traits
1. You can't implement the same trait for the same type in the same crate twice

Orphan Rule
: Traits when multiple crates are involved must meet one of 2 rules
: 1. The trait is defined in the current trait or
: 2. The implementor type is defined in the current crate

### 4.03 Operator overloading

Operator Overloading
: The ability to define custom behavior for operators like `+`, `-`, `*`, `/`, `==`, `!=`, etc

* Operatos are traits

`+ - * / %` are  `Add Sub Mul Div Rem` all from `std::ops`
`==` and `!=` are `PartialEq` from `std::cmp`
`<` `>` `<=` `>=` are `PartialOrd` from `std::cmp`

Default Implementations
* specified in the trait definition by providing a body for the method
* for example `std::cmp::PartialEq` defines `.ne(...)`

### 4.04 Derive Macros
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

### 4.05 Trait Bounds
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

### 4.06 String Slices

`&str`
: A view of string data
* stores a pointer and length on the stack
* does not own the data it points to
* preferred over `&String`
* `String` has and owns exactly one heap allocated section of memory


### 4.07  `Deref` trait
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

### 4.08 `Sized`
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

### 4.09 `From` and `Into`
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


## 05 Enums, packages
1. `enum` s
2. The `Option` and `Result` type for nullable returns and recoverable errors
3. The `Debug` and `Display` traits for printing
4. The `Error` trait to mark error types
5. The `TryFrom` and `TryInto` traits for fallible conversions
6. Rust's Package system

### 5.01 Enums
```rust
enum Status{
	ToDo,
	InProgress,
	Done,
}

let status: Status = Status::ToDo;
```
### 5.02 `match`
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

### 5.03 Variants can hold data
C-style enum
: don't have data, basically named constants

struct-like variant
: a variant that has member variables
* They are inlined as a struct inside the enum
```rust
//Declaration
enum Status {
	ToDo,
	InProgress {assigned_to: String,},
	Done,
{

//Access
match status{
	Status::InProgress { assigned_to} => {
	// alternatively Status::InProgress {assigned_to: person} => ...
		println!("Assigned to: {}", assigned_to);
	},
	Status::ToDo | Status::Done => {
		println!("ToDo or Done");
	}
}

```

**Need to get a better understanding of refs as arguments for trait methods **
```rust
 pub fn assigned_to(&self) -> &str {
        match &self.status{
            Status::ToDo | Status::Done => {
                panic!("Only `In-Progress` tickets can be assigned to someone")
            }
            Status::InProgress{assigned_to} => return assigned_to
        }
    }
```

### 5.04 branching `let`
`if let`
```rust
impl Ticket{
	pub fn assigned_to(&self) -> &str {
		if let Status::InProgress {assigned_to} = &self.status {
			assigned_to
		} else {
			panic!("Only `In-Progress` tickets...");
		}
	}
}
```
`let else`
```rust
impl Ticket{
	pub fn assigned_to(&self) -> &str {
		let Status::InProgress {assigned_to} = &self.status else {
			panic!("...");
		}
		assigned_to
	}
}
```
`let else` is used to avoid increasing indentation levels

**
```rust
let Status::InProgress{assigned_to} = &self.status
else{ panic!("...") };
```
**

### 5.05 Nullability

Tuples
: like structs but without naming members
```rust
let first : (i32, i32) = (3, 4);
println!("{}, {}", first.0, first.1);
```

Tuple-like Structs
: Structs with `()` that have only **unnamed fields**
```rust
struct Point(i32, i32);
let point = Point(10,20);
println("({},{})",point.0, point.1);
```

Tuple-like Variants
: a variant (within an enum) that holds unnamed fields
```rust
enum Option<T> {
	Some(T),
	None,
}
```

Exercise
```rust
pub fn assigned_to(&self) -> Option<&String>{
	let Status::InProgres{assigned_to} = &self.status
	else { return None;};
	return Some(assigned_to);
	// or Option::Some(assigned_to);
	// or Option::<&String>::Some(assigned_to);
}
```

### 5.06 Fallibility

`Result`
: an `enum` that can return a result or an error
```rust
enum Result<T,E> {
	Ok(T),
	Err(E),
}
```

Recoverable errors
: represented as values in rust
* No exceptions

Unrecoverable errors
: `panic!("Message")`
* should be used sparingly

Exercise
```rust
fn new(title: String) -> Result<Ticket,String>{
	if title.is_empty() {
		return Err("Title cannot be empty".to_string());
	}
	Ok(Ticket{title})
```

### 5.07 Unwrapping

`result.unwrap()`
: returns the `Ok` value or panics on `Err`

`result.expect("error message")`
: returns the `Ok` value or panics with the specified message on `Err`

```rust
// Panics
let number = parse_int("34").unwrap();
//again but with an error message
let number = parse_int("42").expect("Failed to parse integerr");

//or match
let number = match parse_int("43"){
	Ok(number) => number,
	Err(err) => panic!("{}",err),
}
```

### 5.08 Error enums
```rust
enum U32ParseError {
	NotANumber,
	TooLarge,
	Negative,
}

match s.parse_u32() {
	Ok(n) => n,
	Err(U32ParseError::Negative) => 0,
	Err(U32ParseError::TooLarge) => u32::MAX,
	Err(U32ParseError::NotANumberr) => {
		panic!("Not a number: {}", s);
	}
}
```
### 5.09 `Error` `Debug` and `Display` traits
`pub trait std::error::Error: Debug + Display {}`
: A marker for error objects requires both debug and display

```rust
pub trait Debug {
	fn fmt(&self, f: &mut Formatter<`_>) -> Result<(), Error>;
}
pub trait Display {
	// same as Debug
```
1. Debug is intended for developers and has a default implementation
2. Display is intended for end users and must be implemented

```rust
use std::fmt::{Display, Formatter, Debug};
use std::error::Error;

#[derive(Debug)]
enum TicketNewError {
    TitleError(String),
    DescriptionError(String),
}

impl Display for TicketNewError{
    fn fmt(&self, f: &mut Formatter<'_>) -> std::fmt::Result{
        match &self{
            TicketNewError::TitleError(string) | 
            TicketNewError::DescriptionError(string) => {
                write!(f,"{}",string)
            }
        }
    }
}

impl Error for TicketNewError {}
```

### 5.10 Packages

Package
: Defined by the `[package]` section in `Cargo.toml`

Crate
: components of a package, also known as targets
* can be **binary** or **library** crates

Binaries
: a program compiled into an executable file
* must in clude a `main` function

Libraries
: a group of code (functions, types, etc) that can be used by other packages as a **dependency**

Conventions
1. The packages source code is in its `src` directory
2. if there's a `src/lib.rs` file, cargo will infer a library crate
3. `src/main.rs` for binary crates
4. This can all be changed by editing `Cargo.toml` see https://doc.rust-lang.org/cargo/reference/cargo-targets.html#cargo-targets

* You can just add a `lib.rs` file to add a library to an existing project with a binary crate

### 5.11 Dependencies
```ini
[dependencies]
thiserror = "1"
```
`[dependencies]` section in `Cargo.toml`
: defines the dependencies and version requirements for the package
* Will be downloaded from `crates.io`
* cargo build goes through stages
	1. Dependency resolution
	2. Downloading dependencies
	3. Compiling the project (including dependencies)
* Dependency resolution is skipped if there is a `Cargo.lock` file and `Cargo.toml` has not changed
	* `Cargo.lock` file lists the **exact** versions of dependencies
	* It should be stored in version control
* `cargo update` will update the `Cargo.lock` file with the latest compatible versions of all dependencies

Path dependencies
: dependencies can also be specified using a path in `Cargo.toml`
```ini
[dependencies]
my-library = { path = "../my-library" }
```
* The path is relative to the `Cargo.toml` file that declares the dependency

Other sources
: https://doc.rust-lang.org/cargo/reference/specifying-dependencies.html

`[dev-dependencies]` section of `Cargo.toml`
: specifies that are only used for development
* they only get pulled in when running `cargo test`
```ini
[dev-dependencies]
static_assertions = "1.1.0"
```
**use `cargo search <packagename>` to search for packages on `crates.io`**

### 5.12 `thiserror`
* a package with a procedural macro to help define custom error messages
```rust
#[derive(thiserror::Error, Debug)]
enum TicketNewError {
	#[error("{0}")]
	TitleError(String), ...
```
`#[derive(thiserror::Error...`
: derives the `Error` trait for the custom type

`#[error("{0}")]`
: specifies which field of the variant will be returned.
* in this case the 0^th^ field of the tuple 

Exercise
```rust
#[derive(thiserror::Error, Debug)]
enum TicketNewError {
    #[error("Title cannot be empty")]
    TitleCannotBeEmpty,
    #[error("Title cannot be longer than 50 bytes")]
    TitleTooLong,
    #[error("Description cannot be empty")]
    DescriptionCannotBeEmpty,
    #[error("Description cannot be longer than 500 bytes")]
    DescriptionTooLong,
}
```

### 5.13 `TryFrom` and `TryInto`

`TryFrom` and `TryInto`
: fallible versions of `From` and `Into`
* defined in `std::convert` like `From` and `Into`

```rust
pub trait TryFrom<T> : Sized {
	type Error;
	fn try_from(value: T) -> Result<Self, Self::Error>;
}

pub trait TryInto<T>: Sized {
	type Error
	fn try_into(self) -> Result<T, Self::Error>;
}
```
**Implementing `TryFrom` gives a matching `TryInto` for free**

Exercise
```rust
#[derive(Debug, PartialEq, Clone)]
enum Status {
    ToDo,
    InProgress,
    Done,
}
impl TryFrom<String> for Status{
    type Error = String;
    fn try_from(value: String) -> Result<Status, Self::Error> {
        <Self as TryFrom<&str>>::try_from(&value[..])
    } 
}

impl TryFrom<&str> for Status{
    type Error = String;
    fn try_from(value: &str) -> Result<Status, Self::Error> {
        if value.to_ascii_lowercase() == "todo"{
            return Ok(Status::ToDo);
        }
        if value.to_ascii_lowercase() == "inprogress"{
            return Ok(Status::InProgress)
        }
        if value.to_ascii_lowercase() == "done" {
            return Ok(Status::Done)
        }

        Err("Invalid".into())
    }
}
```

### 5.14 Error::source
The full definition of `std::error::Error`
```rust
pub trait Error: Debug + Display {
	fn source(&self) -> Option<&(dyn Error + 'static)>{
		None
	}
}
```
`Error:source`
: a method that tracks the error that caused the current error
```rust
use std::error::Error

#[derive(Debug)]
struct DatabaseError{
	source: std::io::Error
}

impl std::fmt::Display for DatabaseError{
	...
}
impl std::error::Error for DatabaseError {
	fn source(&self) -> Option<&(dyn Error + 'static)>{
		Some(&self.srouce)
	}																				]
```

`&(dyn Error + 'static)`
: The `Some` returned by `source` ("A reference to a trait object that implements the Error trait")
* `dyn Error` is a **trait object**. it can refer to any type that implements `Error`
* `'static` is a **lifetime specifier** meaning until the program exits

#### Implementing `source` using `thiserror`
```rust
//A field named source 
#[derive(Error, Debug)]
pub enum MyError[
	#[error("Failed to connect")]
	DatabaseError{ source: std::io::Error}
}

//A field with the `#[source]` attribute
#[derive(Error, Debug)]
pub enum MyError[
	#[error("Failed to connect")]
	DatabaseError{ 
		[#source]
		inner: std::io::Error}
}

//a field annotated with #[from] will implement `source`
// and also `From<converrtedType`
#[derive(Error, Debug)]
pub enum MyError[
	#[error("Failed to connect")]
	DatabaseError{
		#[from]
		inner: std::io::Error
	}
}

#### The `?` Operator
`?`
: propigates errors

```rust
use std::fs::File
fn read_file() -> Result<String, std::io::Error> {
	let mut file = File::open("file.txt")?;
	// ...
	Ok(contents)
}

fn read_file() -> Result<String, std::io::Error> {
	let mut file = match File::open("file.txt"){
		Ok(file) => file,
		Err(e) => return Err(e);
	};
	// ...
	Ok(contents)
```
* It will automatically convert the error type of the result to the error type of the function is possible
```rust
let status = match Status::try_from(status){
	Ok(status) => status,
    Err(error) => 
	    return Err(error.into())
};

let status = Status::try_from(status)?;
```

## 06 "Ticket Management"
Covers
* stack-allocated arrays
* `Vec` - growable arrays
* `Iterator` and `IntoIterator`
* Slices
* Lifetimes
* `HashMap` `BTreeMap` key-value structure
* `Ord` and `PartialOrd` required for `BTreeMap`
* `Index` and `IndexMut` to access elements in a collection

### 06.01 Arrays
Arrays
: fixed-size collections of elements of the same type

```rust
let numbbers : [u32;3] = [1,2,3];
// [<type>;<count>]
let numbers : [u32;3] = [1;3];
// [type;count] = [<init>,<number>]

//access
let first = numbers[0];
let second = numbers.get(1); // won't panic
```
**Zero Indexed**

Out-of-bounds access
: Panics by default
* checks can be optimized away by the compiler in many cases, particularly with iterators

`array.get(ind)`
: returns an `Option`, avoids panic

Exercise
`<enum> as usize` allows it to be used as an index
* doesn't work for enums that hold data
* https://doc.rust-lang.org/reference/items/enumerations.html#custom-discriminant-values-for-fieldless-enumerations

### 6.02 Vectors
`vec`
: A growable array type in the standard library
* created by `Vec::new()`
* or with the `vec!` macro `let numbers = vec![1,2,3];`
* Elements are accessed in the same way as arrays
* `Vec::with_capacity(n);` sets a minimum starting capacity
* `Vec::capacity()` - returns the current capacity

Memory Layout
: A pointer, length, and capacity are stack allocated
* just like `String`

### 6.04 Iterators
For loop expansion
```rust
let mut iter = IntoIterator::into_iter(v);
loop{
	match iter.next(){
		Some(n) => {
			println!("{}",n);
		}
		None => break,
	}
}
//equivalent for loop:
for n in v {
	println!("{}",n);
}
```
```rust
trait Iterator {
	type Item;
	fn next(&mut self) -> Option<Self::Item>;
}

trait IntoIterator {
	type Item;
	type IntoIter: Iterator<Item = Self::Item>;
	fn into_iter(self) -> Self::IntoIter;
}

// Implementing IntoIter for a vector
// From the exercise (06/05)
// The IntoIter type from IntoIterator for TicketStore
type IntoIter = std::vec::IntoIter<Self::Item>;
// or 
type IntoIter = std::vec::IntoIter<Self::Item>;
```
**`Iterator`s automatically implement `IntoIterator` by returning themselves**
**An iterator might not be done when it returns `None`**
* the `FusedIterator` trait does that

#### Bounds check
* Using iterators allows some bounds checks to be removed in optimization
```rust
let v = vec![1,2,3]
//slower
for n in 0..v.len() { ... }
//faster
for n in v {...}
```
### 6.05 `.iter()`

**`IntoIterator` consumes `self` to create an iterator**
* this consumes the collection, but returns owned items

Most collections expose a `.iter()` method which returns an iterator over references
* it does not consume the collection and returns references to the items
* The pattern can be simplified by implementing `IntoIterator` for a reference to the collection 
* i.e. `impl IntoIterator for &TicketStore`
* the standard library does this for most collections, i.e. `&Vec<Ticket>`
```rust
let numberrs: Vec<u32> = vec![1,2];

for n in numbers.iter() {
	// n is &u32
}
for n in &numbers{
	//same as above
	// It is idiomatic to provide both
}
```

Exercise
```rust
pub fn iter(&self) -> std::slice::Iter<Ticket> {...
//or
pub fn iter(&self) -> <&Vec<Ticket> as IntoIterator>::IntoIter {...

//Now things that don't work
pub fn iter(&self) -> Iter<'_, Self::Item> { ...
	// Iter is not in scope
pub fn iter(&self) -> std::slice::Iter<'_, Self::Item> {...
	// Self::Item is ambiguous
	//Dropping the lifetime (or whatever that is) and 
	//	usig `Ticket` instead of `Self::Item` seems to work

```
I was supposed to figure this out based on the docs again but 
in https://doc.rust-lang.org/std/vec/struct.Vec.html there is `pub fn iter(&self) -> Iter<'_, T>`
* which is not what the exercise says


### 6.06 Lifetimes
```rust
impl IntoIterator for &TicketStore{
	type Item = &Ticket;
	type IntoIter = std::slice::Iter<Ticket>;
	fn into_iter(self) -> Self::IntoIter {
		self.tickets.iter()
	}
}
```

Lifetime Parameters
: labels used by the compiler to track how long a reference is valid
```rust
impl <T> Vec<T>{
	//this is simplified
	pub fn iter<'a>(&'a self) -> Iter<'a, T>{ ...}
}
```
The above means that the returned `Iter` object cannot outlive the `Vec`.

Lifetime elision
: omission of explicit lifetime annotations in specific circumstances according to **lifetime elision rules**

`'_`
: a placeholder used for lifetimes in methods, makes the lifetime the same as `self`
```rust
fn iter<'a>(&'a self) -> Iter<'a,T>
// is the same as 
fn iter(&self) -> Iter<'_,T>
```

[Lifetime elision rules](https://doc.rust-lang.org/reference/lifetime-elision.html)


Questions from the exercise:
* Why do associated types need a lifetime? shouldn't that be done on a method-by-method basis?

```rust
impl<'a> IntoIterator for &'a TicketStore{
    type Item = &'a Ticket;
    type IntoIter = std::slice::Iter<'a, Ticket>;
    fn into_iter(self) -> Self::IntoIter{
        //self.iter()
        //(*self).tickets.iter()
        self.tickets.iter()
    }
}
```

### 06.07 Combinators
Useful `Iterator` functions called **combinators** that return iterators
* `map` applies a function to each element of the iterator
* `filter` keeps only elements that satisfy a predicate
* `filter_map` combines `filter` and `map` in one step
* `cloned` converts an iterator of references to an iterator of cloned values
* `skip` skips the first `n` elements of the iterator
* `take` stops the iterator after `n` elements
* `chain` combines two iterators into one


#### closures
closures
: anonymous functions
```rust
let add_one = |x| x +1;
let add_one = |x| {x + 1};

//Types can be specified
// Just the input type  let
add_one = |x: i32| x + 1;
// Or both input and output types, using the `fn` syntax  
let add_one: fn(i32) -> i32 = |x| x + 1;
```
* They capture their environment

#### `collect`
`.collect()`
: consumes the iterator and collects its elements into a collection
* it is a generic over its return type
* using a type hint
```rust
let numbers = vec![1, 2, 3, 4, 5];
let squares_of_evens: Vec<u32> = numbers.iter()
    .filter(|&n| n % 2 == 0)
    .map(|&n| n * n)
    .collect();
```
turbofish syntax
: `collect::<Vec<u32>>();`
* `method::<return_type>();`
* the `::` is to resolve ambiguity with comparison operators
```rust
let square_of_evens = numbers.iter()
	.filter(|&n| n # 2 == 0)
	.map(|&n| n * n)
	.collect::<Vec<u32>>();
```
#### See also
* [`Iterator`'s docs](https://doc.rust-lang.org/std/iter/trait.Iterator.html)
* The [`itertools` crate](https://docs.rs/itertools/)
	* for more combinators


### 6.08 `impl` Trait
`impl Trait` (return)
: A feature that allows returning types that don't have names
* Aka **opaque return type**

```rust
impl TicketStore {
	pub fn to_dos(&self) -> impl Iterator<Item = &Ticket> {
		self.tickets.iter().filter(...)
	}
}
// the return type of `.filter(...)` is 
// std::iter::Filter
// pub struct Filter<I,P>{...}
// I is the std::slice::Iter<'_, Ticket>
// P is a closure, which has no named type
```
* This is not a generic. Only one implementation is generated

RPIT
: "Return position Imp Trait"
* found in RFCs and such, and refers to the use of `impl Trait` in the return position

`impl Trait` as argument type
: Similar to a generic
* is usually not used because it does not allow turbofish syntax
* No upside?

### 6.10 Slices
`&[T]`
: a borrowed reference to a contiguous block of `T`s

Memory Layout
: A pointer and a length (both `usize`)
```rust
let numbers = vec![1, 2, 3];
//via index syntax
	let slice: *[i32] = &numbers[..];
//via method
	let slice: &[i32] = numbers.as_slice();
//or indexing a subset
	let slice: &[i32] = &numbers[1..];
//Also with arrays instead of vecs
let numbers = [1,2,3];
let slice = &numbers[..];
```

* prefer immutable slices because they can be backed by arrays or vectors


`&mut [T]`
: a mutable slice
* allows changing the elements of the `T`
* they are limited in that they cannot access the backing type
* Allows mutation of the items, not the slice itself
```rust
let mut numbers = vec![1,2,3];
let mut slice: &mut[i32] = &mut numbers;
//!!
slice.push(1); // won't compile because slices have no `push`
```
* For mutable slices, `Vec` is preferable in some circumstances

### 6.12 Two States
```rust
//The problem
pub struct Ticket{pub id: TicketId, ... }
or 
pub struct Ticket{
	pub id: Option<TicketId>,
	...
}
```
* Neither is desirable because an Id is not available at construction time but should be available thereafter

```rust
// the solution
pub struct TicketDraft { ...}
// Ticket draft has no `id` or `status`
pub struct Ticket {pub id: TicketId, ... }
```

### 6.13 Indexing
```rust
// Simplified
pub trait Index<Idx> {
	type Output;
	//required method index
	fn index(&self, index: Idx) -> &Self::Output;
}

impl std::ops::Index<Idx> ...
```
* Should panic if the index is invalid

#### 6.14 Mutable indexing
```rust
// simplified
pub trait IndexMut<Idx> : Index<Idx> {
	// extends Index
	fn index_mut(&mut self, index: Idx)
		-> &mut Self::Output;
}

impl std::ops::IndexMut<Idx>
```
* can only be implemented if `Index` is already implemented

### 6.15 HashMap
`HashMap<K,V>`
* O(1) for insert, retrieve, and remove
#### Requirements
Key must implement `Hash` and `Eq`
* The trait bounds are listed in the methods rather than the trait
	* Why? Can custom implementations bypass those restrictions?

#### `Hash`
```rust
pub trait Hash{
	// required method
	fn hash<H>(&self, state: &mut H)
		where H: Hasher
}
//to Use
#[derive(Hash)]
struct ...
```
#### `Eq`
`pub trait Eq: PartialEq { /*no additional methods*/}
: Indicates that the implemented `PartialEq` is **reflexive**

**`Eq` and `Hash` are linked. If two keys are `==` then their hashes must be `==` also.


### 6.16 `BTreeMap`
* like `HashMap` but **the elements are stored in order**

#### Requirements
#### Keys must implement `Ord`
`Ord`
: Defined in `std::cmp`
* has a `cmp` method that returns an `Ordering` `enum`
* Is a stricter version of `PartialOrd` which must return a value instead of an `Option`
```rust
pub trait Ord: Eq + PartialOrd {
	fn cmp(&self, other: &Self) -> Ordering;
}
enum Ordering {
	Less,
	Equal,
	Greater,
}
```
* float types do not implement `Eq` or `Ord` because `NaN` values are not comparable.
* `Ord` and `PartialOrd` must be consistent with `Eq` and `PartialEq`
* `Ord` and `PartialOrd` must be consistent with eachother



## 07 Threads
* Threads useing `std::thread` module
* Message passing using channels
* Shared state using `Arc` `Mutex` and `RwLock`
* `Send` and `Sync` - the traits that encode concurrency guarantees

### 7.01 `std::thread`
Thread
: an execution context with its own stack and instruction pointer

Process
: A memory space tht can be shared between threads


`main` thread
: The first thread created when the program starts

`std::thread`
: the module for creating and managing threads
* `std::thread::spawn(||)`
	* the function to create new threads (using closures
	* returns a `JoinHandle`
* Process termination
	* when the main thread ends, the process will terminate
	* spawned threads will continue running until the main thread finishes
* `JoinHandle::join()` will cause the calling thread to block until the thread referred to ends
	* returns an `Option` of the return value(?)

### 7.02 `'static`
```rust
pub fn spawn<F, T>(f:F) ->JoinHandle<T>
where
	f: FnOnce() -> T + Send + 'static,
	T: Send + 'static
{...}
```
`'static`
: A lifetime specifier that can be satisfied with (1) an owned value or (2) a ref that will last until the program finishes execution

Detached threads
: Threads launched via `thread::spawn` can outlive the thread that spawned it

### 7.03 Leaking memory
`Box::leak`
: A method that will return a `&'static ` or `&'static mut` to the contents of a box
```rust
let x = Box::new(41u32);
let static_ref: &'static mut u32 = Box::leak(x)
```
Data leakage is process scoped.

### 7.04 Scoped threads
```rust
let v = vec![1,2,3];
let midpoint = v.len()/2;
std::thread::scope(|scope| {
	scope.spawn(|| {
		let first = &v[..midpoint];
		println!("First half: {first:?}");
	});
	scope.spawn(|| {
		let second = &v[midpoint..];
		println!("Second half: {second:?}");
	});
});
println!("Whole: {v:?}");

// The same thing without std::thread::scope

let v = vec![1,2,3];
let midpoint = v.len()/2;
let handle1 = std::thread::spawn(||{
	let first = &v[..midpoint];
	println!("First half: {first:?");
});
let handle2 = ...
	...
});
handl1.join().unwrap();
handle2.join().unwrap();
println("While: {v:?}");

// But the last version won't copile
//because `v` does not have a static lifetime
```

`std::thread::scope`
:  can safely borrow from the environment
 * takes a closure as an argument that takes a `Scope` object as its single parameter
 	* the object has a `spawn` method that takes a closure
 	* It is of type `ScopedJoinHandle<'_, T>` where T is the return value of the closure it is passed
* will automatically join all of its contained threads before exiting
* can return a value

Exercise
```rust
pub fn sum(v:Vec<i32>) -> i32 {
	let mid = v.len()/2;
	//the one on github
	let(left,right) = v.split_at(mid);
    std::thread::scope(|s|{
        let left = s.spawn(|| left.iter().sum::<i32>());
        let right=s.spawn(|| right.iter().sum::<i32>());
        left.join().unwrap() + right.join().unwrap()
    })
    // my first attempt
    let mut sum1 = 0;
    let mut sum2 = 0;
    std::thread::scope(|scope| {
        scope.spawn(|| sum1=v[..mid].iter().sum::<i32>());
        scope.spawn(|| sum2=v[mid..].iter().sum::<i32>());
    });
    sum1+sum2
    // both pass all tests
```

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTQzMjIzNjQ4NSwxOTc1MDY3ODYxLC0xNT
A5NDgwMjE5LC0xNzkyMDE1MTkxLC0xNzE1NzUxNTc1LDE4NDA2
OTAzODYsLTc1NDk1NjA2LDIxMDc5ODQyMDYsMTgwOTEyMzIyNC
w4NDk0NjY4MDQsLTM1NzEwODE1NSwxNjA0ODYzNDc5LC03MDU0
MTYyMTAsLTE1NjUzMzQ3ODksNjIxNTQzNjc3LDg3NjYxOTYwNC
w5NjE1ODE2NDEsMTIyMTM5MDc4OCwtMTU1MDE3MzAzNCwtMjAz
NDY4OTgxNF19
-->