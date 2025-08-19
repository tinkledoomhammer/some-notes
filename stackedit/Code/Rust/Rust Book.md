# The Rust Book
"The Rust Programming Language"
https://rust-book.cs.brown.edu/

## 01 Getting Started
Rust is installed and it's versions are managed with `rustup` which is a small download. https://www.rust-lang.org/tools/install Running it in windows uses a cli installer (press 1 for default setup). It will update the command path so that `rust c` `cargo` and `rustup` are all available

Also see
https://users.rust-lang.org/t/setting-up-rust-with-vs-code/76907
for vs code setup
* using the 'Code Runner' extension, the executor settings need to be updated: `"rust" : "cargo run # $fileName",`

### Hello World
```bash
$ mkdir ~/projects
$ cd ~/projects
$ mkdir hello_world
$ cd hello_world
```
then in `main.rs`
```rust
fn main() {
	println!("Hello, World~");
}
```

Then to compile without cargo:
```bash
$ rustc main.rs
$ ./main
```

### Hello Cargo
`cargo --version`
`cargo new hello_cargo`
`cd hello_cargo`

* `cargo new` creates a folder with
	* `src` folder
	* `.gitignore`
	* `Cargo.toml`

`Cargo.toml`
```toml
[package]
name = "hello_cargo"
version = "0.1.0"
edition = "2021"

# See more keys and their definitions at 
# https://doc.rust-lang.org/cargo/reference/manifest.html

[dependencies]

```

`cargo build`
	* creates a `targets/debug` folder with the executable and debug info and whatnot
`cargo run` will build and run the project
`cargo check` will check for errors without building the project
`cargo build --release` will optimize the build

[Cargo Docs](https://doc.rust-lang.org/cargo/)


## 02 - Programming a Guessing Game
https://rust-book.cs.brown.edu/ch02-00-guessing-game-tutorial.html

* `cargo new guessing_game`
* It also created `src/main.rs`

### Processing a guess
* edit `src/main.rs`
```rust
use std::io;

fn main(){
	println!("Guess the number!");
	println!("Please input your ugess.");
	let mut guess = String::new();
	io::stdin()
		.read_line(&mut guess)
		.expect("Failed to read line");
	println!("You guessed: {}", guess);
}
```
* `use std::io;`
	* see the [standard library documentation](https://doc.rust-lang.org/std/prelude/index.html)
	* https://doc.rust-lang.org/std/prelude/index.html
* `let mut guess = String::new()`
	* `let` declares variables. Variables are immutable by default. the `mut` keyword allows them to be modified
	* `String::new()` just what it looks like
* 

#### Receiving User Input
```rust
io::stdin()
	.read_line(&mut guess)
```
* If we had not `use std::io` then the first line could be `std::io::stdin()`
* the `read_line` method appends user input ot the end of the argument, so it has t obe mutable
* `&` means to pass by reference
* references are also immutable by default, hince `&mut guess`
#### Handling Potential failure with Result
* `.expect("Failed to read line");`
* The `read_line` method puts the data into the argument and returns a `Result`
* `Result`s are enumerations that can be `Ok` or `Err`
	* If `Ok` was returned, then it will contain the generated value
	* `Err` idicates an error and it contains information about how/why the failiure occured
	* `.expect` is a method on the `Result` type
		* It causes the program to crash and print the argument passed to `.expect` when an  `Err` is returned
		* if the call is omitted then the compiler will generate warning
* Error handling is covered in chapter 9  https://rust-book.cs.brown.edu/ch09-02-recoverable-errors-with-result.html

#### `println!` placeholders
`println!("You guessed: {}", guess);`
* `{}` is the insertion point
* when printing the value of a variable, then the name of the var can go inside the brackets
* the result of an expression has to be in a separate argument:
	* `println!("x={x} and y+2 = {}",y+2);`

#### Dependencies
* Uses a crate named `rand`
	* in `Cargo.toml`:
```toml
[dependencies]
rand = "0.8.5"
```
* 
	* cargo understands SemVer (http://semver.org/) and "0.8.5" is shorthand for "^0.8.5" which means any version >=0.8.5 and <0.9.0 $[0.8.5, 0.9.0)$
	* `cargo build` will now download and build a bunch of dependencies
		* It uses the registry at `Crates.io`
* Subsequent builds will only recompile changes

`Cargo.lock`
* It tracks the exact versions of all dependencies to make sure that builds are reproducable
* It is usually tracked in version control
* running `cargo update` will download newer versions as long as they satisfy the requirements in `Cargo.toml`
* If the version specification in `Cargo.toml` change, then the next `cargo build` will update the registry of crates and re-evaluate which versions to use
* The cargo ecosystem is covered more in chapter 14 and also https://doc.rust-lang.org/cargo/

#### Generating a random number
```rust
use std::io;
use rand::Rng;

fn main() {
    println!("Guess the number!");
    let secret_number = rand::thread_rng().gen_range(1..=100);
    println!("The secret number is: {secret_number}");
    println!("Please input your guess.");
    let mut guess = String::new();
    io::stdin()
        .read_line(&mut guess)
        .expect("Failed to read line");
    println!("You guessed: {guess}");
}
```
* `cargo doc --open` will autogenerate documentation, including all the dependencies, and open it in a browser

* `use rand::Rng;` The `Rng` trait defines methods that number random generators implement. Traits are covered in chapter 10

*`rand::thread_rng()` function gives a random number generator that is local to the current thread and seeded by the os
* the `.gen_range()` method is defined by the `Rng` trait
	* takes an expression as an argument and returns a random number in the range
	* the expression `1..=100` expresses a range that is inclusive at both bounds

#### Comparing the Guess to the Secret Number
```rust
use std::cmp::Ordering;
//snip
fn main(){
	// --snip--
	let guess: u32 = guess.trim().parse().expect("Please Type a number");
	println!("You guessed: {guess}");
	match guess.cmp(&secret_number) { //Type mismatch
		Ordering::Less => println!("Too small!"),
		Ordering::Greater => println!("Too big!"),
		Ordering::Equal => println!("You win1"),
	}
}
```
`std::cmp::Ordering` gives us the `Ordering` type whish is an enum of `Less` `Greater` and `Equal`
* the `cmp` method (in this case of a `u32` returns an ordering
* `match <expr> { <case> => <statement>, <case> => <statement> }`
	* like switch-case
	* multiline statements can be enclosed in `{}`
* `let guess :u32 =...` re-declares guess, as an integer 
	* `:<type` is how types are statically declared
	* assignment without `let` would fail here because `guess` was previously a string
* string's `.parse()` converts a string to some other type. In this case it is the type specified in the `let` statement

#### Looping, handling invalid input, and quitting the loop
```rust
loop {
	//-- the guess and check code goes here
	//...
		Orderring::Equal => {
			println!("You win!");
			break; // exit the loop
		}
	}//ends the match block
} // ends the loop
```
Handle bad input by restarting the loop
```rust
let guess:u32 = match guess.trim().parse(){
	Ok(num) => num,
	Err(_) => continue,
};
```
* `(_)` matches any pattern
 
The final version of the code:
```rust
use rand::Rng;
use std::cmp::Ordering;
use std::io;

fn main(){
	println!("Guess the number!");
	let secret_number = rand::thread_rng().gen_range(1..=100);
	loop {
		println!("Please input your guess.");
		let mut guess = String::new();
		io::stdin()
			.read_line(&mut guess)
			.expect("Failed to read line");
		let guess: u32 = match guess.trim().parse(){
			Ok(num) => num,
			Err(_) => continue,
		};
		println!("You guessed: {guess}");
		match guess.cmp(&secret_number){
			Ordering::Less => println!("Too small!"),
			Ordering::Greater => println!("Too big!"),
			Ordering::Equal =>{
				println!("You win!");
				break;
			}
		}
	}
}

```

## 03 Common Programming Concepts
https://rust-book.cs.brown.edu/ch03-00-common-programming-concepts.html
### 03.01 Variables and Mutabbility
* Variables are immutable by default
* They can be redefined with another `let` but they cannot be simply assigned
	* assigning to an immutable variable generates a compiler error
* `let` must be used in a local scope(??)
* `let` can also change the type of the variable
#### Constant
* Like immutable variables, but they must:
	* be known at compile time
	* explicitly specify a type
	* `const` can be used in the global scope
* They are usually named in all caps `const THREE_HOURS_IN_SECONDS: u32 = 60*60*3`
* [Rust Reference’s section on constant evaluation] (https://doc.rust-lang.org/reference/const_eval.html)
#### Shadowing
* a variable can shadow a variable of the same name in a containing scope
* It can be initialized from the parent scope
* The variable in the child scope will only be accessible in that scope
* The variable in the containing scope will not be modified by changes in the inner scope
```rust
fn main(){
	let x = 5;
	println!("x = {x}"); // 5
	let x = x+1;
	{
		let x = x*2; // different x
		println!("x = {x}"); // x = 12
	}
	println!("x = {x}"); // x = 6
}
```
### 03.02 Data Types
Type annotation
`let guess: u32 = ...`
#### Scalar Types
#### Integers
* `i8`, `u8`, powers of 2 up to `i128` and `u128`
	* `i32` is the default
* `isize` and `usize` are architecture dependent 
* Signed ints use **two's complement** representation
* The range for signed ints of size n is $[-(2^{n-1}) , 2^{n-1}]$
* The range for unsigned ints is $[0 , 2^n -1]$

Integer literal formats:

Base | Example
--|--
Decimal | 12_345
Hex | 0xff
Octal | 0o77
Binary | 0b1111_1010
Byte `u8` only | b'A'

#### Integer Overflow
* integer overflow will cause a panic for code built in debug mode
* In `--release` builds, the checking code is removed and rust will perform *two's complement wrapping*
* Rust has a family of methods to customize overflow handling
	* `wrapping_*` such as `wrapping_add` will cause wrapping
	* `checked_*` will return none in the event of overflow
	* `overflowing_*` returns a bool indicating whether an overflow occurred 
	* `saturating_*` will clamp the return value

#### Floating point types
`f32` and `f64`
* `f64` is the default
* IEEEE-754

#### Numerical operations
* `+` `-` `*` `/` and `%` work as expected
* For a full list of operators, see https://rust-book.cs.brown.edu/appendix-02-operators.html

#### `bool`s
* one byte in size
* allowed values are `true` and `false`

#### `char` Type
* 4-byte character codes
* literals use single quotes `let c: char = 'z';`
* They are *Unicode Scalar Value*s
* Valid values are in [U+0000 , U+D7FF]$ and U+E0000 , U+10FFFF]
* See also [“Storing UTF-8 Encoded Text with Strings”](https://rust-book.cs.brown.edu/ch08-02-strings.html#storing-utf-8-encoded-text-with-strings)


#### Tuples (A Compound Type)
* a coma separated list of values inside parens
* `let tup: (i32, f64, u8) = (500, 6.4, 1);`
* Members are accessed with dot notation and a numerical index
	* `tup.0 //500`
* A tuple with no values is called a **unit** 
	* its value and type are both written `()`
	* Expressions implicitly return `()` when they don't explicitly return a value
* Tuples can be used for multiple assignment:
```rust
let tup = (50,6.4);
let (x,y) = tup;
// x == 50, y == 6.4
```
#### Array type (compound)
* every element must have the same type
* The size is fixed
* Values are specified in a coma separated list inside square brackets
	* `let a = [1,2,3];`
* The type can be specified to include the size
	* `let a: [i32;3] = [1,2,3];`
* They can be initialized by repeating a value:
	* `let a = [3;5]` will produce `[3,3,3,3,3]`
* To specify the type and initialize with repetition:
	* `let a: [u32;5] = [3;5]`
	* the size must be repeated
* Elements are accessed with square brackets
	* `a[0]` 
* Bounds are checked automatically 
	* Out-of-bounds access will cause a panic

### 03.03 Functions
* declared with `fn`
* parameter types **must** be declared in the signature

### Statements and expressions
Statements 
: instructions that perform some action and do not return a value

Expressions
: evaluate to a resultant value

Function definitions are statements
Function invocations are part of an expression

This means that *assignment can**not** be chained* because assignments are statements and therefore have no value

**Scope blocks created with curly brackets are expressions**:
```rust
let y = {
	let x = 3;
	x+1
};
// y is 4
```
#### Functions with return values
`fn five() -> i32 {5}`

caution: `fn five() ->i32 {5;}` will cause a mismatched type error
* because `5;` is a statement and not an expression
* therefore the returned value is actually `()`
* return can be used as an expression
	* the `;` is optional

Also note that it is `->` and not `=>`

### 03.04 Comments
```rust
// comment to the end of the line
/* this
	is a multiline
	comment
*/
```
### 03.05 Control flow
#### `if` (an expression!)
```rust
fn main() {
    let number = 6;

    if number % 4 == 0 {
        println!("number is divisible by 4");
    } else if number % 3 == 0 {
        println!("number is divisible by 3");
    } else if number % 2 == 0 {
        println!("number is divisible by 2");
    } else {
        println!("number is not divisible by 4, 3, or 2");
    }
}
```
* the `else` and `else if` clauses are optional
* `if` is an expression so it can be used in a `let` statement
* `let number = if condition {5} else {6}`
	* The if and else sections have to have the same type

* The condition must explicitly be a `bool` there are no `truthy` of `falsy` values

#### Loops
```rust
let mut counter =0;
let result = loop{
	counter +=1;
	if counter ==10{
		break counter*2;
	}
}
```
* `loop`s are expressions
* they are exited with break
* `break` can be used to exit the loop and optionally return a value
* the `;` after the `break` is optional
* `break` will exit the current loop only
* **Loop Lables** can be used to disambiguate loops
	* Loop labels begin with a single quote, and are ended with a colon 
	```rust
	let mut count = 0;
	`counting_up: loop{
		println!("count = {count}")
		let mut remaining = 10;
		loop{ 
			print("some stuff";
			remaining -=1;
			if remaining <=0 {
				break `counting_up;
			}
		count +=1;
	}
	```
	* To exit to a label with a  value , `break 'lable retv;`

#### `while` and `for`loops
`while <condition> {stufff}`
	* avoids having to use if..break 
`for element in collection {stuff}`
`for num in (start..end)` // [start , end)
`for num in (start..=end)` [start , end]

## 04 Ownership
### 04.01 Definition of ownership
Safety
: The absence of undefined behavior

Ownership
: A discipline for memory safety

 The Rust Reference maintains a large list of [“Behavior considered undefined”](https://doc.rust-lang.org/reference/behavior-considered-undefined.html).

#### Variables live in the Stack
* Variables exist in stack frames
* This doc describes changes as new frames. I.e. Main before a function call and main after a function call which is used to define a value are different frames
* Variables are copied into new frames, so changes will not affect the parent frame

#### Boxes live in the heap
Pointer
: a value that describes a memory location (internal in rust?)

Pointee
: The value that a pointer points to

Heap
: A separate region of memory where data can live indefinitely, not tied to a specific stack frame

Box
: A rust mechanism for allocating data on the heap
i.e.
	```rust
	let a= Box::new([0;1_000_000]);
	//A now points to the array. The pointer is 
		//all that lives on the stack
	let b = a;
	//Copying a gives coppies the pointer
		//so we avoid the 4MB copy
	```
#### Rust does not permit manual memory management
Memory Management
: The process of allocating and deallocating memory

The nearly correct deallocation principle
* If a variable is bound to a box then when rust deallocates the variable's frame, it also deallocates the box's heap memory
* the correct principle
	* If a variable owns a box, then rust deallocates the heap memory with the frame
* Ownership is transferred on assignment to avoid double deallocation 
* Assignment seems to include passing it as a argument

#### Variables cannot be used after being moved
Moved heap data principle
: If a variable `x` moves ownership of heap data to another variable `y`, then `x` cannot be used after the move

### 04.02 References and Borrowing
Having to pass/return all objects that are used more than once would suck...
```rust
fn greet(g1: String, g2: String) -> (String,String){
	println!("{} {}!",g1 ,g2);
	(g1,g2)
}
fn main(){
	let (m1,m2) = (String::from("Hello"), String::from("Hello"));
	let (m1,m2) = greet(m1,m2);
	println!("{m1} {m2}!");
}
```
References are
: Non-owning pointers
```rust
fn greet(g1: &String, g2: &String){
	println!("{} {}!", g1, g2);
}
// ...
greet(&m1, &m2);
```

#### Dereferencing a pointer Accesses its data
```rust
let mut x: Box<i32> = Box::new(1);
let a: i32 = *x;         // *x reads the heap value, so a = 1
*x += 1;                 // *x on the left-side modifies the heap value,
                         //     so x points to the value 2

let r1: &Box<i32> = &x;  // r1 points to x on the stack
let b: i32 = **r1;       // two dereferences get us to the heap value

let r2: &i32 = &*x;      // r2 points to the heap value directly
let c: i32 = *r2;    // so only one dereference is needed to read it
```
Dereferences are often implicit:
```rust
let x: Box<i32> = Box::new(-1);
let x_abs1 = i32::abs(*x); // explicit dereference  
let x_abs2 = x.abs(); // implicit dereference  
assert_eq!(x_abs1, x_abs2); 
let r: &Box<i32> = &x; 
let r_abs1 = i32::abs(**r); // explicit dereference (twice)  
let r_abs2 = r.abs(); // implicit dereference (twice)  
assert_eq!(r_abs1, r_abs2); 
let s = String::from("Hello"); 
let s_len1 = str::len(&s); // explicit reference  
let s_len2 = s.len(); // implicit reference  
assert_eq!(s_len1, s_len2);
```
#### Rust avoids Simultaneous Aliasing and mutation
Three things that can go wrong 
1. Deallocating aliased data, leaving the original variable pointing to deallocated memory
2. Mutating aliased data, invalidating expected runtime properties of that data
3. Concurrently mutating aliased data, resulting in a race



Pointer Safety Principle
: Data should never be aliased and mutated at the same time
* Boxes cannot be aliased -- assignment moves ownership
* References, which are mostly temporary references, have their safety ensured through the **borrow checker**

i.e. If you have a ref to a member of a vector, then resizing the vector will invalidate the ref

#### References change the permissions on places
Permissions
1. Read (R) : Data can be copied to another location
2. Write (W) : Data can be mutated
3. Own (O): Data can be moved or dropped
* The permissions only exist at compile time
* By default, a variale has R and O on its data.
	* If it was crated with `let mut` then it also has W
* References can temporarily remove these permissions
```rust
let mut v: Vec<i32> = vec![1,2,3];
	// v is initialized with +R+W+O
let num: &i32 = &v[2];
	// v is now +R only
	// num is initialized +R +O
	// *num is initialized +R
println!("asdf{},*num); // This is the last time that num is used
	// after this line, num no longer borrows from v
	// v is restored to +R+W+O
	// num is discarded (loses R and O)
	// *num is discarded (loses +R)
v.push(4); // valid because we are no longer using num
// Since this is the last time that v is used
	// All perms are dropped
	//The vec is deallocated
```

Places
: anything you can put on the left side of an assignment
* Varaibles
* Dereferences of a place
* Array accesses of places
* fields of places
* combinations of the above
```rust
let x = 0; // x: +R +O
let mut x_ref = &x; // mutable borrow
	// x: +R (lost O)
	// x_ref: +R+W+O
	// *x_ref +R only
// so x_ref can be re-assignedd but not mutated
```





#### The borrow checker finds permission violations
* creating references requires permissions
* i.e. the `Vec<i32>::push()`  requires read and write permissions
Once a reference is no longer needed, the borrow ends and the permissions are restored

#### Mutable references provide unique and non-owning access to data
Mutable reference
: aka Unique reference
: allows mutation but not aliasing

Immutable references
: aka shared references
: Allow aliasing but not mutation

While a mutable reference exists, it has exclusive read access, and cannot exist concurrently with other references

Unique references can be temporarily downgraded by borrowing from them with `&*`
```rust
let mut v: Vec<i32> = vec![1,2,3];
let num: &mut i32 = &mut v[2];
let num2: &i32 = &*num; // this borrow removes the W perm from num
// the perm will be returned at the end of num2's lifetime
```
#### Data must outlive all of its references
* within a single lexical scope, the permission system above is adequate
	*	because the lifetime is known to the compiler from that function's code alone
*	when references are input to a function or output from a function

Flow Permission (F)
: Indicates that a reference is allowed to flow (be used) within a particular expression
: Does not change through out a function body
: Only applies to function parameters and return values
: Can be set with *lifetime specifier*s
```rust
fn return_a_string() -> &String{
	let s = String::from("Hello world");
		//lifetime of s??
	let s_ref = &s
		// lifetime of s_ref??
	s_ref
		// returning a reference to a locally owned value is bad
}
```


#### Summary
* All variables can read, own, and optionally write their data
* creating a reference will transfer permissions from the borrowed place to the ref
	* write and ownership are always lost from the source
	* read is also removed when the ref is `mut`
* perms are returned at the end of the ref's lifetime
* data must outlive all references that point to it


### 04.03 Fixing Ownership Errors
**Sometimes rust will reject safe code**
* but it will always reject unsafe code unless it's marked `unsafe`

#### Unsafe program: returning a reference to the stack
```rust
//bad
fn return_a_string() -> &String{
	let s = String::from("Hello world");
	&s
}

//better
fn return_a_string() -> String{
	let s = String::from("hi");
	s
	//just return the string, not the ref
}

//or return a string literal
fn return_a_string() -> 'static str {
	"Hello world"
}

//Or do borrow checking at runtime with reference counting
use std::rc::Rc;
fn return_a_string() -> Rc<String> {
	let s = Rc::new(String::from("Hello world"));
	Rc::clone(&s)
	// not sure why a clone is needed... 
	//	Perhaps s gets collected even if it is returned?
}

//finally, send a mutable string
fn return_a_string(output: &mut String) {
	output.replace_range(.., "Hello world");
}
// more verbbose but allows the caller to control allocations
```

#### Fixing : Not enough permissions
i.e. trying to mutate read-only data, drop data behind a ref, etc

```rust
//Bad
fn stringify_name_with_title(name: &Vec<String>) -> String {
	name.push(String::from("Esq.")); // fails
		// String::push is a mutation but the
		//	argumment `name` is immutable
	let full = name.join(" ");
	full
}
	// Ideally ["Ferris", "Jr."] => "Ferris, Jr. Esq."

// a bad solution
fn stringify_name_with_title(&mut name: Vec...) // the rest is the same as abovve
	// This will satisfy the borrow checker but it mutates the vector


// a better solution: clone the argument
fn stringify_name_with_title(name: & Vec<String>) -> String {
	let mut full = name.join(" ");
		// .join() coppeis the data in `name` into the string `full`
	full.push_str(" Esq.");
	full
}
```
**It is very rare for rust functions to take ownership of heap-owning data structures like `Vec` and `String`**

#### Fixing unsafe: Aliasing and Mutating a data structure
```rust
//bad
fn add_big_strings(dst: &mut Vec<String>, src: &[String]) {
	let largest: &String = 
		dst.iter().max_by_key(|s| s.len()).unwrap();
		// ^^ Removes W perm from `dst` ^^
	for s in src {
		if s.len() > largest.len() {
			// ^^ uses largest, keeping W perms from dst ^^
			dst.push(s.clone());
			// ^^ requires W perm on `dst` ^^
		}
	}
}

// works, but slowly
fn add_big_strings(dst: &mut Vec<String>, src: &[String]) {
    let largest: String = dst.iter().max_by_key(|s| s.len()).unwrap().clone();
    // clone the largest string so that `largest` doesn't ref to `dst`
    for s in src {
        if s.len() > largest.len() {
            dst.push(s.clone());
        }
    }
}

// also works, also slow
fn add_big_strings(dst: &mut Vec<String>, src: &[String]) {
    let largest: &String = dst.iter().max_by_key(|s| s.len()).unwrap();
    let to_add: Vec<String> = 
        src.iter().filter(|s| s.len() > largest.len()).cloned().collect();
        // Use another Vec to store strings to add
    dst.extend(to_add);
}

//The best way 
fn add_big_strings(dst: &mut Vec<String>, src: &[String]) {
	let largest_len: usize = dst.iter().max_by_key(
		|s| s.len()).unwrap().len();
	// just store the length
	for s in src{
		if s.len() > largest_len {
			dst.push(s.clone());
		}
	}
}
```

#### Fixing unsafe copying vs moving out of a collection
```rust
// Below works fine
let v: Vec<i32> = vec![0,1,2]
let n_ref: &i32 = &v[0];
let n: i32 = *n_ref;
// works because i32 does not own heap data

// This \/ does not
let v: Vec<String> = vec![String::from("hi")];
let s_ref: &String = &v[0];
let s: String = *s_ref;// Error : 
// cannot move out of *s_ref which is behind a shared ref
// this would result in a double free
// because s is dropped and v is dropped
```
Some examples
* `i32` **does not** own heap data so it **can** be copied without a move
* a `String` **does** own heap data so it **can not** be copied without a move
* an `&String` **does not** own heap data so it **can** be copied without a move
* `&mut i32` **cannot** be copied without being moved
```rust
//Avoid taking ownership
let v: Vec<String> = vec![String::from("Hi")];
let s_ref: String = &v[0];
println!("{s_ref}!");

//Or clone the data
let v: Vec<String> = vec![String::from("Hi")];
let mut s_ref: String = &v[0];
s.push('!');

//Or remove the data
let mut v: Vec<String> = vec![String::from("Hi")];
let mut s: String = v.remove();
s.push('!');
```

#### Fixing a safe program: Mutating different tuple fields
* The borrow checker can track fine-grained permissions
* But it doesn't analyze called functions beyond looking at ther signatures
```rust
// works fine
fn main() {
	let mut name = (
	    String::from("Ferris"), 
	    String::from("Rustacean")
	);
	let first = &name.0;
	name.1.push_str(", Esq.");
	println!("{first} {}", name.1);
}

//Does not work
fn get_first(name: &(String, String)) -> &String {
    &name.0
}
fn main() {
    let mut name = (
        String::from("Ferris"), 
        String::from("Rustacean")
    );
    let first = get_first(&name);
    // first blocks W perms for `name` because
    //	 the borrow checker doesn't know which
    //	members of 'name' it refers to
    name.1.push_str(", Esq.");//Error
    // because 'name.1' might be referred to by 'first'
    println!("{first} {}", name.1);
}
```

#### Fixing a safe program: Mutating different array elements
Rust does **not** track elements of an array as different places.

```rust
//works
let mut a = [0,1,2,3]; // a: +RWO
let x = &mut a[1];//a[_]: -R-W
*x +=1; // (after) a[_]: +R+W
println("{a:?}");

//fails
let mut a = [0,1,2,3];
let x = &mut a[1];// a[_]: -R-W
let y = &a[2];//Error because a[_] must be +R
*x += *y;
// The above is actually safe but the compiler doesn't determine that

//Solution:
let (a_l, a_r) = a.split_at_mut(2);
// ^^ Uses an `unsafe` block to split a into two locations ^^
let x = &mut a_l[1];
let y = &a_r[0];
*x += *y;
```

From the exercise:
**Dereferencing an owned type takes ownership**

### 04.04 The Slice Type
Example: returning a part of a string
* Returning an index is bad because it is a number and is not tied to the lifetime of the string
* instead returning a slice will tie the lifetimes

** Memory**
: Allocates two sizes on the stack
* a pointer (like a ref)
* a size

String slice
: A reference to a part of a string 
: `&str`
```rust
let s = String::from("hello");
let len =s.len();
let slice1 = &s[..1];
let full = &s[..];
```

Range syntax
: `first..last+1`
: i.e. `[0..2]` means the first two elements
* the first number is optional, `0` is the default
* the second number is optional with the length-1 being default
	* i.e. the maximum allowed
* `[..]` refers to the entire allowed range 

```rust
fn first_word(s: &String) -> &str {
	let bytes = s.as_bytes();
	for (i, &item) in bytes.iter().enumerate() {
		if item ==b' ' {
			return &s[0..i]
		}
	}
	&s[..]
}
```
**String literals are slices**

Other types:
```rust
leat a = [1,2,3,5,5];
let slice = &a[1..3]; // slice: &[i32]
assert_eq!(slice, &[2,3]);
```


### 04.05 Ownership recap
In chapter 4
: ownership, borrowing, slices
: memory allocation, stack vs heap, pointers, and undefined behavior

#### Ownership vs Garbage collection
Garbage collection
: scans memory at runtime to deallocate unreachable variables.
: * Pros: Avoids use-after-free and double-free; avoids a complicated type system; avoids undefined behavior
: * Con:  runs at runtime, with either large but infrequent or frequent but short cleanup operations
: * Con: can be difficult to tell when variables will be garbage-collected from reading the source code
: * Cons: aliasing problems. obfuscating and restricting pointers
: Example 
	```python
		class Document:  
			def __init__(self, words: List[str]):
				"""Create a new document"""
				self.words = words 
			def add_word(self, word: str):
				"""Add a word to the document""" 
				self.words.append(word) 
			def get_words(self) -> List[str]:
				"""Get a list of all the words in the document"""  
				return self.words
		words = ["Hello"]
		d = Document(d.get_words())
		d2.add_word("world")
		
	```

Ownership
: uses a more complicated type system to deallocate as soon as memory goes out of scope, without reference counting

	```rust
		type Document = Vec<String>;

		fn new_document(words: Vec<String>) -> Document {
		    words
		}

		fn add_word(this: &mut Document, word: String) {
		    this.push(word);
		}

		fn get_words(this: &Document) -> &[String] {
		    this.as_slice()
		}

		fn main() {
		    let words = vec!["hello".to_string()];
		    let d = new_document(words);

		    // .to_vec() converts &[String] to Vec<String> by cloning each string
		    let words_copy = get_words(&d).to_vec();
		    let mut d2 = new_document(words_copy);
		    add_word(&mut d2, "world".to_string());

		    // The modification to `d2` does not affect `d`
		    assert!(!get_words(&d).contains(&"world".into()));
		}
	```
	* `new_document` consumes ownership of the vector, so the document owns the parameter it is passed
	* `add_word` requires a `&mut Document`, and consumes the input 	`word`
	* `get_words` returns an immutable reference to strings
 
 #### Concepts of ownership
 Ownership at Runtime
 : * Rust allocates local variables in stack frames
 : local variables can hold either data or pointers
 : Pointers can be either boxes (which own heap data) or references (non-owning pointers).
```rust
fn main() {
  let mut a_num = 0;
  inner(&mut a_num);
  // a_num == 5 
}

fn inner(x: &mut i32) {
  let another_num = 1;
  let a_stack_ref = &another_num;

  let a_box = Box::new(2);  
  let a_box_stack_ref = &a_box; //points to the stack
  let a_box_heap_ref = &*a_box; // points to the heap
  *x += 5;
}
```
Ownership at Compile-time
: Read, Write, and Own perms are tracked on each variable
: Rust requires that a variable have the appropriate perms for the operation
: Perms are changed if the variable is **moved** or **borrowed**
: Moving a non-copyable type requires RO perms, and eliminates all perms on the original var
: Borrowing a variable removes some perms

Connecting Ownership between run and compile time
: The system is designed to prevent undefined behavior, such as 
: **Use-after-free** : immut borrows remove W perms
: **double-free** : Dereferences of refs to non-copyable data do not have O perm

```rust
//Double free example
let v = vec![1,2,3];
let v_ref: &Vec<i32) = &v;
let v2 = *v_ref; // [L1]
// v and v2 point to the same heap address
// vref points to v in the stack
drop(v2); 
// now freeing v2 also frees v
drop(v); //This would double-free v and v2

//Use after free example
let mut v = vec![1,2,3];
let n = &v[0]; // [L1]
// v and n point ot the same heap address
v.push(4); [L2]
// v is the same but n points to nothing
// because mutating v is not allowed with a living ref
println!("{n}"); //[L3]
// this would be undefined because the address of n is unknown
```


#### From the quiz
```rust
//rejected by the borrow cheker
fn extract(b: &Box<i32>) -> i32 {
	let b2: Box(i32) = *b; // creates an owned copy!!
	*b2
	//b2 gets freed here
}
//casues a double free
```
## 05 Structures
`struct`
: A custom data type that packages together and name related values

### 05.01 Defining and Instancing Structs
Defined
: with the `struct <name>{...}` construct which is
: filled with `key: type,` pairs for each **field**

```rust
struct User {
	active: bool,
	username: String,
	email: String,
	sign_in_count: u64,
}

```
Creation
: Create an **instance** of the struct by 
: 	specifying concrete values in `key: value,` pairs
:	enclosed in a `<name>{...}` block

```rust
fn main() {
	let user1 = User {
		email: String::from("asdf@gasg"),
		username: String::from("asdf"),
		active: true,
		sign_in_count: 1,
	};
	println!("{}",user1.email);
}
```
Accessing values
: `structVar.field` i.e. `user1.email`


#### Field Init shorthand
```rust
fn build_user(email: String, username: String) -> User {
	user {
		active: true,
		username,
		email,
		sign_in_count: 1,
	}
}
```

#### Struct Update syntax

```rust
fn main() {
	// --snip--
	let user2 = User {
		email: String::from("blargh"),
		..user1
		// coppies copyable un-specified values from `user1`
		// moves the remaining values from user1
		
	};
}
```
* moves data


#### Tuple Structs, Unit-Like Structs

`struct Color(i32, i32, i32);`
:  creates a new tuple type
: it has a name and 
: is a different type than other tuples that contain the same types

`struct StructName;`
: an empty data type
: as with tuple-like structs, each one is a different type

#### Ownership of struct data
* Struct members that are references require lifetime parameters
* This will be discussed in ch. 10

##### Borrowing fields of a struct
* Rust will track ownership and permissions at both the struct- and field-level
```rust
struct Point {x: i32, y: i32};
let mut p = Point { x: 0, y: 0 };
//p, p.x, and p.y are all now +RWO
let x = &mut p.x;
//now p and p.x have lost all perms
// p.y is unaffected
*x+=1; //this is the LAST use of x
//perms are returned
```
#### Quiz example(s)
```rust
struct Point { x: i32, y: i32 };
fn test1(){
	let mut a = Point {x: 1, y: 2};
	a.x+=1;
	let b = Point{y: 1, ..a};
	a.x+=1;
	println!("{}", b.x);
}	// Prints "2"

fn test2(){
	let mut p = Point{x: 1, y: 2};
	let x = &mut p.x;
	let y = &mut p.y;
	*x+=1;
	*y+=1;
	println!("{} {}", p.x, p.y);
} // prints "2 3"
```

### 05.02 Example program using structs

#### Derive traits
```rust
#[derive(Debug)]
struct Rectangle { ... }
```
The `Debug` trait
: allows printing variables
: 1. with `println!("rect1 is {rect1:?}");`
: 2. or with `dbg!(&rect1);`

The `dbg!` macro
: prints an expression, the value of the expression, and the line and file where the macro is run
: takes ownership of its arguments but can be passed refs to retain ownership
: returns the value of the expression
: `dbg!(30*scale)` -> "[src/main.rs:10:16] 30*scale = 60"

### 05.03 Method Syntax
#### Defining methods
```rust
#[derive(Debug)]
struct Rectangle { width: u32, Height: u32,}

impl Rectangle{
	fn area(&self) -> u32 {
		self.width*self.height
	}
}
```
* called like `object.method()`
	* the `.` operator will reference and dereference as needed
* The first parameter must be `self` and must be the same type as the struct
	* `&self` is short for `self: &Self`
	* `Self` is an alias for the type implementing the method
	* Other options are
		* `self` to take ownership
		* `&mut self` to mutate the self variable
* Can be named the same as a field

Associated Functions
: functions defined in an `impl` block 
: they do not necessarily take a `self` parameter
* methods are a category of associated functions
* They can use the `Self` type alias in their definitions
* Can be called with `::` syntax: `Rectangle::square(3)`

#### Caveats

* Multiple `impl` blocks **are allowed**
* Methods are sugar for function calls
```rust
r.area()
// equivalent to
Rectangle::area(&r)
```

#### Methods and ownership
Accessing a method on  a struct requires perms on the struct depending on the `self` parameter.
* `&mut self` requires `mut struct` or `&mut struct` (+RW perms)
* `self` requires an owned struct (+O perms)
* `&self` requires only read perms (+R)

`#[derive(Copy, CLone)]`
: allows silent copying of data types as long as they don't own heap data
: This means that you can "take ownership" without taking ownership


## 06 Enums and Pattern Matching

### 06.01 Defining an Enum

```rust
struct StructVariant01{
	// -- snip --
}

enum Message {
	Var1, // Unit-like variant
	Var2(i32,i32), // Tuple-like variant
	Var3{x: i32, y: i32}, // Struct-like variant
	Var4(String), // Containing a numerical data type
}

impl Message {
	fn call(&self){
		// -- snip --
	}
}
```

* An `enum` *instance* represents exactly one of the defined variants.
#### `Option<T>`
```rust
enum Option<T> {
	None,
	Some(T),
}
```
`Option<T>` is defined in the standard prelude
* Its variants `Some` and `None` can be accessed directly 
	* `let some_number = Some(5);`  is equivalent to 
	* `let some_number = Option::<i32>::Some(5);`
	* or `let some_number: Option<i32> = Some(5);`
* also `let no_number : Option<i32> = None;`
* Using the enum means that you have explicitly opted in to allowing null values
	* and so must explicitly account for the possibility of null values

### 06.02 `match` control flow construct
```rust
enum Coin {
	Penny,
	Nickle,
	Dime,
	Quarter,
}
fn value_in_cents(coin: Coin) -> u8 {
	match coin{
		Coin::Penny => {
			println!("Lucky Penny!");
			1
		},
		Coin::Nickle => 5,
		Coin::Dime => 10,
		Coin::Quarter => 25,
	}
}
```
#### Structure of match
* `match` keyword and an expression
	*	The expression can be any type
*	One or more match arms
	*	each has is *pattern* `=>` *expression to run*
*	Only the first match will be executed
*	At least one arm must match
*	The entire match expression returns the value from the arm that matches
*	Longer arms can enclose the *code to run* in curley braces
	*	then the `,` coma is optional

#### Patterns that bind to values
```rust
#[derive(Debug)]
enum UsState {
	Alabama,
	Alaska,
	// -- ... 
}

enum Coin {
	Penny,
	Nickel,
	Dime,
	Quarter(UsState),
}
fn value_in_cents(coin: Coin) -> u8 {
	match coin {
		Coin::Penny=> // --snip--
		Coin::Quarter(state) => {
			println("State quarter from {state:?}!");
			25
		}
	}
}
```
```rust
fn decr_twice_v1(n: u32) -> Option<u32> {
	match n {
		0 => None,
		1 => None,
		n2 => Some(n2-2),
	}
}
```

#### Matching `Option<T>`
```rust
fn plus_one(x: Option<i32>) -> Option<i32> {
	match x {
		None => None,
		Some(i) => Some(i+1),
	}
}
```

#### Exhaustive matching and the `_` Placeholder
```rust
let dice_roll = 9;
match dice_roll {
	3 => add_fancy_hat(),
	7 => remove_fancy_hat(),
	_ => (), // will execute whenever no other patterns match
}
```

`_` will match any value or variant
```rust
// with coin: Coin
match coin{
	Penny => 1,
	Quarter(_) => 25, // Discardes the state
	_ => 10, // will match any other variant
}
```

### 06.03 `if let` and `let else`
`if let`
: for matching one pattern and ignoring the rest
```rust
let config_max = some(3u8);
if let Some(max) = config_max {
	println!("The max is {max}");
}
//equivalent match statement
match config_max {
	some(max) => println!("The max is {max}");
	_ => (),
}
```
* `if let` can have an `else` block
```rust
let mut count = 0;
if let Coin::Quarter(state) = coin {
	println!("State quarter from {{state:?}!");
} else {
	count +=1
}
```

`let else`
: Facilitates staying on the "happy path" 
: useful when there is an "exit early" case
```rust
fn describe_state_quarter(coin: Coin) -> Option<String> {
	let Coin::Quarter(state) = coin else{
		return None;
	};
	if state.existed_in(1900) {
		Some(format!("{state:?} is pretty old."))
	} else {
		Some(format!("{state:?} is fairly new."))
	}
}

//Equivalent with `if let`
let state = if let Coin::Quarter(state) = coin {
	state
} else {
	return None;
};
// ... --snip--
```

both contain a *pattern* = *source* construct










#### Matches and ownership
**when values are matched, they are assigned**
**assigning to** `_` **will not assign a value**
* Matching on a ref will **"Push Down"** the ref from the outer enum
```rust
let opt: Option<String> = 
	Some(String::from("Hello world"));

//This will take ownerthip of the string
match opt{
	Some(s) => Println!("Some: {}",s),
	None=> println!("None!"),
};
//Error \/ below
println("{:?}",opt);

//To avoid taking ownership

match &opt{
	Some(s) => println!("Some: {}",s), // now s is &String
	None => ...
}
//now we can still use opt
println!("{:?}", opt);
```



## 07 Packages, Crates, and Modules
* a package can contain one library crate and one or more binary crates
* As a package grows it can be split into multiple crates

Packages
: A cargo feature for building, testing, and sharing crates

Crates
: A tree of modules that produces a library or executable

Modules and use
: let you control the organization, scope, and privacy of paths
: manage namespaces

Paths
: A way of naming an item, such as a struct, function, or module

Cargo workspaces
: a cargo feature for managing multiple interrelated packages that evolve together

### 07.01 Packages and Crates
crate
: the smallest amount of code that the compiler considers at a time
: can contain modules
: may have multiple source files and modules
: is either binary or library. binary crates must have a `main` function

crate root
: a source file that the compiler starts from and makes the root module of the root crate

package
: a bundle of one or more crates that provides









> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE2ODU4Mjk1NjksMTM1MjY3Nzc0MywtMT
A2NzAwNjk3NSwxODM1MDEyMDA4LDE5OTU2Mjk1MDIsLTEyNjUx
OTkwNTUsOTcwMTgxMTcxLC01MzgyOTk0ODEsNTIyOTM0OTE1LD
E2MjE2MTQ5MTgsNDE4MzU5MDczLDE4NDQ1MTIyNTUsLTIwOTgx
NDE5MTIsLTE5NDY3NzYxMzAsLTMzODExMDk1Niw2OTQwMTYwNj
ksNjM0MTE2MzYwLDExMTQ2NDYyMTMsMzQxOTAwOTYzLC0xODAx
NjUwMF19
-->