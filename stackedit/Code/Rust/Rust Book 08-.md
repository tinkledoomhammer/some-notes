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

Cases in which you have more info that the compiler
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
* Invalid inputs from code. The user code should coder needs to fix the problem, not the code. i.e. the solution is to stop sending bad arguments


**Some reasons not to panic**
* when failure is expected sometimes

#### Creating custom types for validation
* the type's `new` function or other constructor functions can panic when given invalid inputs
* this means that the caller has to validate the input first to avoid the panic

## 10 Generic Types, Traits, and Lifetimes
### 10.01 Generic Data Types
### 10.02 Traits

Traits
: Basically interfaces

#### Defining and implementing traits
```rust
pub trait Summary {
	fn sumarize(&self) -> String;
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


#### Default implementations
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

#### Traits as Parameters

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

#### `impl` *trait* as a return type
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
#### Trait bounds for conditional implementations

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

### 10.03 Validating References with Lifetimes
Lifetimes
: A type of generic that ensures that a reference is as valid for as long as needed
: they are often inferred

















> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTcwNTI2MDMyMSwtMTMxOTA0OTkyNiwxNT
gxMjQ4NTA5LDExMzgyNTIyNSwxNDMxNzQxMjEzLDE2OTc0OTM3
MDcsLTQ2OTY2NTM2LDE3NTA3MDkwMzEsLTY1NDE5MjA2NSwxND
I4OTcxNjAxLC0xNjI2MDgxOTYxLC0xMTg2ODQ4MTM3LDE3NTUy
NDM1NjAsLTE1MDY3MzIyMjcsLTE5NzczODA2NjUsLTIwMDUzMz
QyNjYsMTU5MjQwODk2NywtMTMxNDg4MzgwMywtMTI4NDA1MDQz
OSwtOTkwNDg5OTk0XX0=
-->