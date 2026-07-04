# The Rust Book part 2
"The Rust Programming Language"
https://rust-book.cs.brown.edu/

# 08 Common Collections

**Dropping a vector drops its elements**

## 08.01 `Vec`tors
* `let v: Vec<i32> = Vec::new();`
* `Vec`s are generics, with the type being stored as the generic parameter
* `let v = vec![1, 2, 3];`

`Vec<T>.push(val)`
: adds `val` to the end of the vector

#### Reading elements
```rust
let v = vec![1,2,3,4,5];
//using [] to index
let third: &i32 = &v[2];
// will panic if the index is out of bounds

// with Vec<T>.get(index) -> Option<&T>
let third: Option<&i32> = v.get(2);
// will not panic, ruturns None if out of bounds
```
**Note:** The borrow checker considers the entire vec to be one item

### Iterating over values in a `vec`
```rust
let v = vec![100,32,57];
for i in &v {
	println!("{i}");
}
let mut v = vec![100,32,57];
for i in &mut v {
	*i +=50;
}
```

### Safely using `iter`ators
More details in [13.2 Processing a series...](https://rust-book.cs.brown.edu/ch13-02-iterators.html)

 `Vec::iter` and `Iterator::next`

```rust
fn main() {
use std::slice::Iter;  
let mut v: Vec<i32>         = vec![1, 2];
let mut iter: Iter<'_, i32> = v.iter();
let n1: &i32                = iter.next().unwrap();
let n2: &i32                = iter.next().unwrap();
let end: Option<&i32>       = iter.next();
}
```
Values can**not** be added or removed while the iterator is alive.
Instead, iterate over a range:
```rust
let mut v: Vec<i32>			= vec![1,2];
let mut iter: Range<usize>	= 0..v.len();
let i1: usize				= iter.next().unwrap();
let n1: &i32				= &v[i1];
```
### Vecs of Enums
```rust
enum SpreadsheetCell {
    Int(i32),
    Float(f64),
    Text(String),
}

let row = vec![
    SpreadsheetCell::Int(3),
    SpreadsheetCell::Text(String::from("blue")),
    SpreadsheetCell::Float(10.12),
];
```


**Dropping a vector drops its elements**



## 08.02 UTF-8  encoded `String`s

string slices
: `str` type, usually used as `&str`
: The native type for string literals
: the only string type in the core language

### Creating a new String
```rust
let mut s = String::new();
let data: str = "initial contents";
let s2 = data.to_string();
let s3 = "initial contents".to_string();
```
**UTF-8 Encoding allows strings in a variety of languages**

### Updating a String
* can grow in size
* the `+` operator or the `format!` macro can concatenate strings

**push_str**
* `s.push_str("bar");` => `"foobar"`
* `fn push_str(&mut self, s: &str) ;`
* takes a `&str` argument and appends it to the end of the string

**push**
* `s.push('a')` -- adds a single char to the end of the string

`+` operator
: `fn add(self, s: &str) -> String {...`
: Modifies/consumes the self, and returns the resultant combination
: `s1 + &s2` where both are `String` coerces `&s2` to `&str`

`format!` macro
: uses `println!` like syntax to make combining strings less cumbersome 
: can avoid many re-allocations
```rust
let s1 = String::from("tic");
let s2 = String::from("tac");
let s3 = String::from("toe");
let s = format!("{s1}-{s2}-{s3}");
//"tic-tac-toe", one allocation
//equivalent to
let s = s1 + "-" + &s2 + "-" + &s3;
//same result but up to 7 reallocations
```

### Indexing into `String`s
* regular indexing is invalid in rust due to the variable-length of UTF-8 chars
* And also indexing cannot be guaranteed to be O(1) (constant time) 

**Internal Representation**
* `String` is a wrapper over a `Vec<u8>`
* The elements of that `Vec` are u8, not chars
* Rust's chars are Unicode scalars

**Bytes vs Scalars vs Grapheme Clusters**
* bytes are bytes
* scalars are Unicode scalar values, which can be characters or diacritics
* Grapheme clusters are one or more scalar values

### Slicing and Iterating over Strings

Slicing
: Technically allowed with ranges
: Indexes are in bytes
: Will cause a panic if it includes part of a char
: `let s =&hello[0..4]`

Iterating
: `.chars()` or `.bytes()`
: The standard library does not provide a way to iterate over grapheme clusters
```rust
for c in "Зд".chars() {
    println!("{c}");
}
for b in  "Зд".bytes() {
 qprintln!("{b}");
}
```
>З
>д
>208
>151
>208
>180


## 08.03 `HashMap`s

### Creating and using `HashMap`s

`HashMap<K, V>`
: stores mapping of `K` type keys to `V` type values
: uses a *hashing function* to determine how keys and values are stored in memory

```rust
use std::collection::HashMap;
let mut scores = HashMap::new();
scores.insert(String::from("Blue"), 10);
scores.insert(String::fom("Yellow"),50);

let team_name = String::from("Blue");
let score = scores.get(&team_name).copied().unwrap_or(0);

for (key, value) in &scores {
	println("{key}: {value}");
}

scores[somekey] // will panic if `somekey` is not present
```

### Ownership and Updating
* Values with the `Copy` trait are copied into the hm
* other values are moved
* References can be added, and will not move the target of the ref
	* but the ref must be valid for at least as long as the hm is valid
	
**Each key can only have one associated value**

```rust
map.insert(key1, val1);
map.insert(key1, val2);
// key1 => val2
// insert will replace a value if the key exists

map.entry(key2).or_insert(val2);
//the entry() method returns an Entry enum
//the Entry.or_isert(val) method
//	will return a mutable referrence to the value, assigning one if none is present
```

```rust
use std::collections::HashMap;
let text = "hello world wonderful world";
let mut map = HashMap::new();
for word in text.split_whitespace(){
	let count = map.entry(word).or_insert(0);
	*count +=1;
}
println!("{map:?}");
//{"world": 2, "hello: 1, "wonderful:: 1}
```

### See Also [Chapter 10](https://rust-book.cs.brown.edu/ch10-02-traits.html) for changing the hash function
* The built in one is fairly slow for security reasons

## 08.04 Ownership quiz 2

# 09 Error Handling
`panic!`
: macro causes the program to exit immediately 

`Result<T,E>`
: type used to represent possible errors

## 09.01 Unrecoverable errors with `panic!`

### Unwinding vs Aborting
* By default panics cause the stack to unwind, deallocating memory as it goes
* alternatively the binary can be compiled to abort
	* `panic = abort` in the appropriate `[profile]` section i.e. `[profile.release]`
	* this will force the os to clean up memory, but is a faster and results in a smaller binary

```rust
fn main() {
	panic!("crash and burn");
}
```

```bash
$ cargo run
   Compiling panic v0.1.0 (file:///projects/panic)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.25s
     Running `target/debug/panic`

thread 'main' panicked at src/main.rs:2:5:
crash and burn
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

* a panic will also occur from various bugs, such as attempting to access an array out of bounds

## 09.02 Reocverable `Error`s with `Result`
```rust
enum Result<T, E> {
	Ok(T),
	Err(E),
}

//example
use std::fs::File;
use std::io::Error;

let file_result: Result<File,Error> = File::Open("text.txt");
```

* `Ok` and `Err` are in scope from the standard prelude and don't require the `Result::` prefix

### Matching errors
```rust
use std::fs::File;
fn main() {
	let file_result = File::open("hello.txt");
	let file = match file_result {
		Ok(file) => file,
		Err(error) => panic!("Problem opening file: {error:?}"),
	};
}

// or to match the kind of error
use std::io::ErrorKind;
fn main() {
	let file_result = match File::open("hello.txt");
	let file = match file_result{
		Ok(file) => file,
		Err(error) => match error.kind() {
			ErrorKind::NotFound => match File::create("hello.txt") {
				Ok(fc) => fc,
				Err(e) => panic!("Can't create file: {e:?}"),
			},
			_ => panic!("Can't open file: {error:?}"),
		}
	};
}
```
### Alternatives to match**

`result.unwrap_or_else(..)`
: a method that takes a closure and returns the result or sends the error to the closure
```rust
usie std::{fs::File, io::ErrorKind};
fn main(){
	let file = File::open("hello.txt").unwrap_or_else(|error| {
		if error.kind() == ErrorKind::NotFound {
			File::create("hello.txt").unwrap_or_else(|er| {
				panic!(Couldn't create the file ({er:?})");
			})
		} else {
			pannic!("Other error {error:?}");
		}
	});
}
```

** When the operation is expected to never fail**


`result.unwrap()`
: returns the `Ok`, or panics on `Err`

`result.expect(msg: &str)`
: returns the `Ok`, or panics with the specified error message on `Err`

### Propagating Errors

`?` Operator
: Evaluates to the `Ok` result or causes the function to return the `Err` result
```rust
use std::fs::File;
use std::io::{self, Read};

//The long version:
fn read_from_file2() -> Result<String, io::Error> {
	let file_result = File::open("hello.txt");
	let mut file = match file_result {
		Ok(file) => file,
		Err(e) => return Err(e),
	};
	let mut st = String::new();
	match file.read_to_string(&mut st) {
		Ok(_) => Ok(st),
		Err(e) => Err(e),
	}
}

//A shorter version
fn read_from_file1() -> Result<String, io::Error> 
	let mut st = String::new();
	File::open("hello.txt")?.read_to_string(&mut st)?;
	Ok(st)
}
```

**Can only be used when the return types are compatible.**
: It must implement `FromResidual` such as `Result` and `Option`
: `Result<T,Box<dyn Error>>` works with any `Result`
: `Result` and `Option` types cannot be mixed


**Main can return any of a number of different types**
: They must implement `std::process::Termination`
: it has a `report` function that returns `ExitCode`
: the `ExitCode` should be 0 on success, or something else if the program terminates with an error

### 09.03 When to use

Examples
: Use `.unwrap()` for readability

Prototype Code
: use `.unwrap` and `.expect` while deciding how to handle errors

Tests
: `.unwrap()` and `.expect()` 
: because `panic!` is how tests are supposed to fail

Cases in which you have more info than the compiler
: for things that will never fail, use `.expect` to document why the code is expected to never fail

#### Guidelines for error handling

bad state
: when some assumption, guarantee, contract, or invariant has been borken
: i.e. invalid, contradictory, or missing values are passed to your code plus at least on of
: * the state is unexpected, rather than occasionally expected.
: * subsequent code needs to rely on not being in a bad state (rather than checking for problems at every step
: * there is no good way to encode this information in types see [“Encoding States and Behavior as Types”](https://rust-book.cs.brown.edu/ch18-03-oo-design-patterns.html#encoding-states-and-behavior-as-types) 

**Some reasons to panic**
* continuing could be insecure or harmful
* when calling external code and there is no way to fix the problem
* Invalid inputs from code. The user code needs to fix the problem, not the called code. i.e. the solution is to stop sending bad arguments


**Some reasons not to panic**
* when failure is expected sometimes

#### Creating custom types for validation
* the type's `new` function or other constructor functions can panic when given invalid inputs
* this means that the caller has to validate the input first to avoid the panic

# 10 Generic Types, Traits, and Lifetimes
## 10.01 Generic Data Types
### In Function Definitions
* Generic params go inside `<..>` between the function name and the param list
```rust
fn largest<T: std::cmp::PartialOrd>(list: &[T]) -> &T {
	let mut largest = &list[0];
	for item in list{ if item > largest {
		lagest = item;
	}}
	largest
}
```
* The trait bounds are required to use methods on the generic types
### In `struct` definitions
```rust
struct Point<T> {
	x: T,
	y: T,
}
fn main() {
	let integer = Point{x: 5, y: 10};
	let float = Point{ x: 1.0, y: 5.0};
}
```
* generic params go between the struct name and its members

### In `enum` Definitions
* Same as in `struct`s

### In method definitions
```rust
impl<T> Point<T> {
	fn x(&self) -> &T {
		&self.x
	}
}
```

* Generic params are placed immediately after `impl`

```rust
//Only defines the function for a particular Point type
// Doesn't take any generic params
impl Point<f32> {
	fn distance_from_origin(&self) -> f32 {
		(self.x.powi(2) + self.y.powi(2)).sqrt()
	}
}
```

>You cannot simultaneously implement specific _and_ generic methods of the same name this way. For example, if you implemented a general `distance_from_origin` for all types `T` and a specific `distance_from_origin` for `f32`, then the compiler will reject your program: Rust does not know which implementation to use when you call `Point<f32>::distance_from_origin`. More generally, Rust does not have inheritance-like mechanisms for specializing methods as you might find in an object-oriented language, with one exception (default trait methods)

### Using different generics in an `impl` and a `fn`
```rust
struct Point<X1, Y1> {
    x: X1,
    y: Y1,
}

impl<X1, Y1> Point<X1, Y1> {
    fn mixup<X2, Y2>(self, other: Point<X2, Y2>) -> Point<X1, Y2> {
        Point {
            x: self.x,
            y: other.y,
        }
    }
}

fn main() {
    let p1 = Point { x: 5, y: 10.4 };
    let p2 = Point { x: "Hello", y: 'c' };

    let p3 = p1.mixup(p2);

    println!("p3.x = {}, p3.y = {}", p3.x, p3.y);
}
```

## 10.02 Traits

Traits
: Basically interfaces

### Defining and implementing traits
```rust
pub trait Summary {
	fn sumarize(&self) -> String;
	fn no_default(&self);
	fn has_default_implementation(){/*implementation */ }
}

pub struct NewsArticle {
	pub headline: String,
	pub author: String, //...
}
impl Summary for NewsArticle {
	fn summarize(&self) -> String {
		format!("{} by {}", self.headline, self.author)
	}
}
pub struct SocialPost{
	pub username: String,
	pub content: String, //...
}

impl Summmary for SocialPost{
	fn summarize(&self) -> String {
		format!("{}: {}",self.username, self.content)
	}
}
```

Similar to implementing methods for structs, but `impl` *trait* `for` *struct* `{...}`

Orphan Rule
: You can not implement an external trait for an external type


### Default implementations
* including a function definition in the trait definition
* Then to use the default implementation have an empty `impl` block
```rust
pub trait Summary {
	fn summarize(&self) -> String {
		String::from("(Read more...)")
	}
}
impl Summary for NewsArticle {}
```

* The syntax for overriding a default implementation is the same as the syntax for providing an implementation for a function that does not have a default implementation

### Traits as Parameters

`impl trait` syntax
: `pub fn notify(item: &impl Summary) {...}`
: `impl` keyword is a prefix to the type

Trait Bound syntax
: used with generics
: `pub fn notify<T: Summary>(item: &T){...}`

`+` Syntax for specifying multiple bounds
: `pub fn notify(item: &(impl Summary + Display)) {`
: `pub fn notify<T: Summary + Display>(item: &T) {`

`where` clauses to declutter
: specifies the bounds immediately before the body, in a `,` separated list
```rust
fn func_1<T: Display + Clone, U: Clone+Debug> (t: &T, u: &U) ->i32 {
	//...
}
//or for readability
fn func_2<T,U>(t: &T, u: &U) -> i32
where
	T: Display + Clone,
	U: Clone + Debug,
{ 
	//...
}
```

### `impl` *trait* as a return type
* can **only** return **one** concrete type
* often used when the return type is anonymous or very long
	* such as closures and iterators
* [“Using Trait Objects That Allow for Values of Different Types”](https://rust-book.cs.brown.edu/ch18-02-trait-objects.html#using-trait-objects-that-allow-for-values-of-different-types) will be covered in ch. 18
* Also the compiler will be limited in what it can do with the returned value
	* only methods defined in the trait can be invoked
	* i.e. the calling code only knows the return type as specified in the called function
```rust
use std::fmt::Display;
fn displayable<T: Display>(t:T) -> impl Display { t }
fn disp2<T: Display>(t:T) -> T {t}

fn main() {
	let s = String::from("hello");
	let mut s2 = disp2(s);
	let s = String::from("hello");
	let mut s3 = display(s);
	s2.push_str(" world"); // s2="hello world"
	//The next line will not compile
	s3.push_str(" world"); // s3 doesn't have 'push_str'
}

```
### Trait bounds for conditional implementations

blanket implementation
: generic implementations apply to any type that implements the trait bounds
: there are several in the standard library
: such as `impl<T: Display> ToString for T { ...`

```rust
use std::fmt::Display

struct Pair<T> {
	x: T,
	y: T,
}

//No bounds, will implement for any type
impl<T> Pair<T> {
	fn new (x: T, y: T) -> Self {
		Self{ x,y }
	}
}

//This blanket implementation has 2 bounds
impl<T: Display + PartialOrd> Pair<T> {
	fn cmp_display(&self) {
		if self.x >= self.y {
			println!("x is larger");
		}else{
			println!("y is larger");
		}
	}
}
```

## 10.03 Validating References with Lifetimes
Lifetime Annotations
: A type of generic that ensures that a reference is as valid for as long as needed
: they are often inferred

Lifetime
: the region of code where a value is available

Dangling Ref
: when a ref outlives the data it points to aka **use after free**

### Lifetime Annotation Syntax

* specified with a single quote: `'`
```rust
&i32 // a reference
&'a i32 // a refreence with an explicit lifetime
&'a mut i32 // a mut ref with explicit lifetime

fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
	if x.len() > y.len() { x } else { y }
}
```
* the specifier means "at least as long as"
* so in the above example, the return value expires with the shortest lived parameter
	* i.e. the *concrete* lifetime that the compiler uses will be the shortest
* the specifiers do not change the lifetime of the values
* rather they specify a contract that the compiler can enforce
* Annotations go in the signature, not the body, making them part of the function's contract
* Explicit lifetimes allow the compiler to more precisely point out and describe the error
	* even where implicit lifetimes are possible, the eventual error may be elsewhere


```rust
fn longest <'a>(x: &'a str, y: &str) -> &'a str {x}
```
* only lifetimes that can apply to the return value need to be specified
* the return lifetime cannot be local

** Lifetime annotations in struct definitions **
```rust
struct Excerpt<'a> {
	part: &'a str,
}
```
* references held by structs require a lifetime annotation
* the struct can't outlive the reference it holds

### Lifetime Elision
: like type inferences, but for lifetimes
: typically, when a func takes one ref and returns another, the lifetime is assumed the same for both

lifetime elision rules
: the rules the compiler follows to determine lifetimes without explicit specifiers

input lifetimes
: the lifetimes of parameters

output lifetimes
: the lifetimes of return values

The lifetime elision rules
1. the compiler assigns a different lifetime parameters to each input
2. If there is exactly one input lifetime, then it is assigned to all output lifetime parameters
3. if there are multiple input lifetime parameters, but one is `&self` or `&mut self`, the lifetime of `self` is assigned to all output lifetime parameters
4. Otherwise explicit lifetime parameters are required

### Method definitions and 'static

Lifetime names for struct fields need to be declared after the `impl` keyword and used after the struct's name because they are a part of the struct's type
```rust
impl<'a> Excerpt<'a> {...}

```


'static lifetime
: special lifetime that denotes that the reference *can* live for the entire duration of the programs
: like all string literals

### Combined example Type params, trait bounds, and lifetimes
```rust
use std::fmt::Display;
fn longest_with_ann<'a, T>(
	x: &'a str,
	y: &'a str,
	ann: T,
) -> &'a str
where
	T: Display,
{
	println!("Announcement! {ann}");
	if x.len() > y.len() {x} else {y}
}

```

**Quiz**
```rust
//the following will not compile
struc Foo<'a> {
	bar: &'a i32,
}
fn baz(f: &Foo) -> &i32 {/* ...*/}
//because the lifetime of the return could be tied to Foo or to Foo.bar
```

# 11 Automated Tests
## 11.01 How to Write Tests
Tests typically
1. Set up any needed data or state
2. Run the code to be tested
3. Assert that the results are as expected

### Anatomy of a Test Function
`#[test]`
: attribute that annotates test functions
: will be run by `cargo test`

`#[cfg(test)]`
: marks a module to be compiled for tests only
* Creating a new library project in Cargo creates a test module aotumatically
```bash
$ cargo new adder --lib
	Created library 'adder' project
$ cd adder
$ code src/lib.rs
```
```rust
pub fn add(left: u64, right: u64) -> u64 { left + right }
#[cfg(test)]
mod tests {
	use super::*;
	#[test]
	fn it_works() {
		let result = add(2,2);
		assert_eq!(result, 4);
	}
}
```

### `assert!` macros
* takes a boolean argument
* calls `panic!` if the bool is `false`
* `assert_eq!` and `assert_ne!`
	* takes two arguments, compares with `==` and `!=`
* Extra arguments can be provided to generate a custom error message
	* the formatting is the same as for `println!`
```rust
#[test]
fn greeting_contains_name() {
``` let result = greeting("Carol");
assert!(
	result.contains("Carol"),
	"Greeting did not contain name, value was '{result}'"
}
```

### Checking for panics with `#[should_panic]`
```rust
#[test]
#[should_panic]
fn greater_than_100(){
	panic!("Oops");
}
```
* Can test for specific types  of panic by matching the error message with the `expected=` parameter
```rust
#[test]
#[should_panic(expected="asdf")]
fn will_panic(){
	panic!("foo asdf bar");
}
```
### Using `Result<T,E>` instead of panicing
```rust
#[test]
fn it_workd() -> Result<(), String> {
	let result = add(2,2);
	if result == 4 {
		Ok(())
	} else {
		Err(String::from("2 + 2 != 4")
	}
}
```
* Cannot be used with `#[should_panic]`
* allows the use of `!`
* Note: to 

## 11.02 Controlling How Tests are Run
`cargo test`
: runs all tests, in parallel by default

`cargo test -- --help`
: displays the options you can use after the `--` separator

### Running Tests in Parallel or Consecutively
`cargo test -- --test-threads=1`
* tests are run in parallel by default, with multiple threads

### Showing function output
* Output is hidden for tests that pass by default
* shown for failures only, by default
* `cargo test -- --show-output`
	* will display output for test s that pass
### Running a subset of tests by name
* Single tests , pass the name of that test without a separator
	* `cargo test this_one_test`
* The last line of cargo's output will show how many tests were filtered out
* Filtering to run multiple tests
	* `cargo test part_of_the_function_name`
	* will run all test with the specified text as **part** of the name

### Ignoring tests unless specifically requested
```rust
#[test]
#[ignore]
fn expensive_test() { /* ... */ }
```
* ignored tests are listed after failed, before ignored, in the last line of output
* use `cargo test -- --ignored` to run **only** the ignored tests
* use `cargo test -- --include--ignored` to run them in addition to the normal tests

## 11.03 Test Organization
Unit tests
: small, focused tests that evaluate one module in isolation
: can test private interfaces

Integration tests
: external to the library
: use the code in the same way as a n application
: can only use the public interfaces

### Unit Tests
* created in a module in the same file as the module they test, usually named test
* should have the `#[cfg(test)]` attribute
* The annotation tells rust to compile and run the code only when invoked with `cargo test`
* The annotation is not needed for integration tests
* can use `import super::*` to get access to the entire parent module
* Apparently testing private functions is controversial

### Integration tests
* located in the `tests` directory, which should be a sibling of `src`
* must `import` the stuff they need as though it were in a separate crate
* each file is compiled as a separate crate
* `cargo` test will evaluate in the order:
	* unit tests first
	* integration tests seconds
	* doc tests last
* The process terminates when one test fails
	* so if a unit tests fails then no integration or doc tests will run
* `cargo test --test integration_test` to only run tests in
	* `proj/tests/integration_test.rs`

### Submodules in integration tests
* special care must be taken when using a module to share code between tests
* because each `.rs` file is compiled as its own crate
* so to make a shared module, give it a folder in `tests`
* and create a `mod.rs` in that folder
* i.e. `proj/tests/common/mod.rs`
* then in the tests `use common::*` or whatever

# 12 An I/O Project: Building a Command Line Program
* implementing grep (**G**lobally search and **R**egular **E**xpression **P**rint)
* command line tool
* reads an environment variable
* print to `stderr` and `stdout`
* Note that `ripgrep` by Andrew Gallant is a fast and fully featured grep written in rust

The project will combine:
* Organizing code (Chapter 07)
* Using vectors and strings (Ch. 08)
* Handling errors (Ch. 09)
* Using traits and lifetimes (Ch.10)
* Writing test
Will briefly introduce
* closures, iterators, and trait objects (Ch. 13 and Ch.18)


## 12.01 Accepting Command line arguments (and project setup)
Creating the project
```bash
$ cargo new minigrep
	Created binary (application) 'minigrep' project
$ cd minigrep
``` 
It will be invoked like:
`$ cargo run -- searchstring example-filename.txt`

the `--` tells cargo to pass the remaining arguments to the binary

### Reading the argument values
the `std::env::args` function returns an iterator of `String`s
```rust
use std::env;
fn main() {
	let args: Vec<String> = env::args().collect();
	dbg!(args);
}
```
Note: `std::env::args` will panic if the arguments contain invalid unicode. If you need to accept non-unicode characters, use `std::env::args_os` instead. That iterator produces `OsString`s instead of regular `String`s

* the first item returned is the path of the executable. 
	* `target\\debug\\minigrep.exe` for me
	* the example has forward slashes
so
```rust
let query = &args[1];
let file_path = &args[2];
```
* to extract the search text and file path
## 12.02 Reading a File
poem.txt, in the project root

```
I'm nobody! Who are you?
Are you nobody, too?
Then there's a pair of us -- don't tell!

How dreary to be somebody!
How public, like a frog
To tell you name the livelong drearyTo an admiring bog!
```
To reaad the file:
```rust
use std::{env, fs};
// -- snip -- after setting file_path
let contents = fs::read_to_string(file_path);
println!("Contents:\n{contents}");
```
## 12.03 Refactoring to Improve Modularity and Error Handling
Fixing 4 problems:
* `main` has two tasks (parsing arguments and reading files)
* configuration variables are mixed in with other locals
* use of `expect` with a vague error message. Should say why we couldn't read the file
* If the user runs the program without enough arguments, it crashes with `index  out of bounds`. We should provide a better message

### Separating concerns  in binary projects
* Split the program into `main.rs` and `lib.rs`, with most of the logic going in `lib.rs`
* The command line parsing logic can stay in `main` as long as it is small
* If it gets complicated, move it to `lib.rs`

So  `main(){}` should now have the following responsibilities
* call command line parsing logic with the argument values
* set up any other configuration
* calling a `run` function in `lib.rs`
* handling the error if `run` returns an error

### Extracting the argument parser
```rust
use std::{env, fs};
fn main() {
    let args: Vec<_> = env::args().collect();
    let (query, file_path) = parse_config(&args);
   
    println!("Query: {query}\t File: {file_path}");
    let contents = fs::read_to_string(file_path)
        .expect("Should have been able to read the file");
    println!("Contents:\n{contents}");
}
fn parse_config(args: &[String]) -> (&str, &str) {
    let query = &args[1];
    let file_path = &args[2];
    (query, file_path)
}
```
* note the signature of `parse_config`
### Grouping configuration values
```rust
use std::{env, fs};
fn main() {
	let args: Vec<_> = env::args().collect();
	let config = parse_config(&args);
	println!("Searching for: {}", config.query);
  println!("In file: {}", config.file_path);
  let contents = fs::read_to_string(config.file_path)
      .expect("Should have been able to read the file");
  println!("Contents\n{contents}");
}
struct Config {
    query: String,
    file_path: String,
}
fn parse_config(args: &[String]) -> Config {
    let query = args[1].clone();
    let file_path = args[2].clone();
    Config {query, file_path}
}
```
* now we clone out the args, and the `Config` struct owns them.

* Basically renamed `parse_config` to `Config::new`
### Fixing the error handling
* Start by renaming `Config::new` to `Config::build` and the return type to `Result<Config, &'static str>`
* The call the book has to the new constructor is nicer than the one I came up with
```rust
// in the book
let config = Config::build(&args).unwrap_or_else(
	|err| { 
		println!("Problem parsing arguments: {err}");
		process::exit(1);
	}
);

// what I came up with
let result = Config::build(&args);
if let Err(err) = result {
	println!("Problem with arguments: {err}");
	return;
}
```
*`std::process` has `process::exit` 
* `std::process::exit(1)` seems to indicate an error.
### Extracting logic from `main()` to `run(config: Config)`
* The first version just coppies a few lines into a new function
* Then the signature is changed to `fn run(config: Config) -> Result<(), Box<dyn Error>>`
```rust
use std::error::Error;
// -- snip --
fn run(config: Config) -> Result<(), Box<dyn Error>> {
	let contents = fs::read_to_string(config.file_path)?;
	println!("with text:\n{contents}");
	Ok(())
}
```
* uses `?` to unwrap, so a box'ed dyn of whatever error is thrown from `fs::read_to_string(...)`
* I used a similar technique to handling the `Config::build` result
* the book used an `if let Err(...` this time, prolly because we don't need the `Ok(())` for anything

### Splitting code into a library crate
* the section ends by adding
	* `lib.rs` with a `search` function:
		* `pub fn<'a> search(query: &str, contents: &'a str) -> Vec<&'a str>...`
		* in `main.rs` `use minigrep::search;`

## 12.04 Adding Functionality with Test-Driven Development
Steps in TDD
1. Write a test that fails, and run it to make sure it fails for the right reason
1. Write or modify just enough code to pass the test
1. Refactor the code and make sure the test continues to pass
1. Repeat
* Writing tests frequently helps maintain high test coverage

### Writing a Failing test
* add a test module to `src/lib.rs`
```rust
// -- snip --
mod test{
    use super::*;
    #[test]
    fn one_result() {
        let query = "duct";
        let contents = "Rust:\nsafe, fast, productive.\nPick three.";
        assert_eq!(vec!["safe, fast, productive."], search(query, contents));
    }
}
```
### Writing code to pass the test
1. Iterate through each line of contents
1. Check whether the line contains the query string
1. if it does, add it to the list of return values
1. if not, do nothing
1. return the list of results that match

* Iterate through lines with the `.lines()` method of `&str`
* check for matches with `.cointains(query)` method of `&str`
* store matches with `push(line)` method of `Vec`

The finished code in `lib.rs`
```rust
pub fn search<'a>(query: &str, contents: &'a str) -> Vec<&'a str> {
    let mut results = Vec::new();
    for line in contents.lines(){
        if line.contains(query){
            results.push(line);
        }
    }
    results
}
//-- Snip --
```

## 12.05 Working with Environment Variables
* adding a case-insensitive search option controlled via an environment variable
### Writing a failing test
I moved the example string to a `const SEARCH_TEXT`
```rust
#[cfg(test)]
mod test{
    use super::*;
    const SEARCH_TEXT: &str =
        "Rust:\nsafe, fast, productive.\nPick three.";
    const SEARCH_TEXT2: &str =
        "Rust:\nsafe, fast, productive.\nPick three.\nDuct tape.";
    const SEARCH_TEXT3: &str =
        "Rust:\nsafe, fast, productive.\nPick three.\nTrust Me.";
    #[test]
    fn one_result() {
        let query = "duct";
        assert_eq!(vec!["safe, fast, productive."], search(query, SEARCH_TEXT));
    }
    #[test]
    fn case_sensitive() {
        let query="duct";
        assert_eq!(vec!["safe, fast, productive."],
            search(query, SEARCH_TEXT2)
        );
    }
    #[test]
    fn case_insensitive() {
        let query ="rUsT";
        assert_eq!(vec!["Rust:", "Trust Me."],
            search_case_insensitive(query, SEARCH_TEXT3)
        );
    }
}
```

### Writing `pub fn search_case_insensitive...`
```rust
pub fn search_case_insensitive<'a>(query: &str, contents: &'a str) -> Vec<&'a str> {
    let mut result = Vec::new();
    let query2=query.to_lowercase();
        for line in contents.lines(){
            if line.to_lowercase().contains(&query2){
                result.push(line);
            }
        }
    result
}
```
### Updating `main.rs`
```rust
struct Config {
    query: String,
    file_path: String,
    ignore_case: bool,
}
// -- snip --
// in Config::build(args)
	let ignore_case = env::var("IGNORE_CASE").is_ok();
// -- snip -- the return vlue is
	Ok(Config{query, file_path, ignore_case})
//`and run gets updated
let results = if config.ignore_case {
	search_case_insensitive(&config.query, &contents)
} else {
	search(&config.query, &contents)
};
// then print from results
// the previous version printed from contents
```

### setting the environment variable
In linux: `IGNORE_CASE=1 cargo run -- to poem.txt`
* sets the variable temporarily, or just that invocation
In windows: `$Env:IGNORE_CASE=1; cargo run -- to poem.txt`
* setting the var is a separate command
* to clear it : `Remove-Item Env:IGNORE_CASE` 



## 12.06 Redirecting Errors to `stderr`









> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIxMDEzOTczNTcsLTE3MTYyODI5MzIsOD
A5NDIxNjY2LDE4NDU3NDc0NzAsLTM0NTc5NDI5OSw4Mjk5MDk1
ODEsOTc4NjgwMzcsLTE0MDM2Njc0MTcsMTA4OTQ0NTExNSw3MD
I4ODg4NjQsLTE5NTE3ODY4NjEsLTk3MTM0NDI5NSwtMTAyMDg4
MDg1LC0xOTU5MzUzNzg1LDIwODA4NzM3ODYsNjgxMTIwMzg1LC
0xODc4NzQ2MDA5LC0xODE1ODY0MjUsMTA4NTQ2NjU3NSwtMTc5
MDA0MTM1N119
-->