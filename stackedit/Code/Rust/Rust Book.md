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
* Uses a crate name d `rand`
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

## Common Programming Concepts
https://rust-book.cs.brown.edu/ch03-00-common-programming-concepts.html
### Variables and Mutabbility
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
### Data Types
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
	* `checked_*` will return none in the event of verflow
	* `overflowing_*` returns a bool indicating whether an overflow occurred 
	* `saturating_*` will clamp the return value

#### Floating point types
`f32` and `f64`
* `f64` is the default
* IEEEE-754

#### Numerical operations
* `+` `-` `*` `/` and `%` work as expected
* For a full list of operators, see https://rust-book.cs.brown.edu/appendix-02-operators.html

#### Bools
* one byte in size
* allowed values are `true` and `false`

#### Char Type
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
* Elements are accessed with square brackets
	* `a[0]` 
* Bounds are checked automatically 
	* Out-of bounds access will cause a panic

### Functions
* declared with `fn`
* parameter types **must** be declared in the signature

### Statements and expressions
Statements : instructions that perform some action and do not return a value
Expressions: evaluate to a resultant value

Function definitions are statements
Calling functions are part of an expression

This means that assignment cannot be chained because assignments are statements and therefore have no value

Scope blocks created with curly brackets are expressions:
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

### Comments
```rust
// comment to the end of the line
/* this
	is a multiline
	comment
*/
```
### Control flow
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
	let mut count =0;
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
#### `while` and `for`loops
`while <condition> {stufff}`
	* avoids having to use if..break 
`for element in collection {stuff}`
`for num in (start..end)` // [start , end)
`for num in (start..=end)` [start , end]

## Ownership
### Definition of ownership
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

### References and Borrowing
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
1. Deallocating aliased data, leavinng the original variable pointing to deallocated memory
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
* perms are returned at the end of the ref's lifetime
* data must outlie all references that point to it


### Fixing Ownership Errors


> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE1MjIwMjE2NjIsLTExNTU5OTgyNTEsMT
E3MzI2MzE0MCwtNTM4MDE5NzAwLDgzNjU1NTQ5NywtMzk0MTcz
ODkzLC0xNDQzNzg5NzA4LDYzNTQ1MzI1OSwxNTMzNTMyMTYxLC
0yNTAyMzAxODQsMjI0Nzk2NDAxLDM4Mjk1OTcxMiwxOTA3NDcx
MDM1LDY3MTg4NDIzMCwtMTkxODMyOTI2NiwxNTc5ODAyMTE1LD
I0NDY2NjAzMywtMTM1ODYzODQ2NiwxMDQ4MTI1MjM0LC0xNDEw
ODQzMjE4XX0=
-->