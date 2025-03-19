# The Rust Book
"The Rust Programming Language"
https://rust-book.cs.brown.edu/

## Getting Started
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

# See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html

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
use rand:Rng;
use std::cmp::Ordering;
use std::io;

fn main(){
	println!("Guess the
```
> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE4MDY4MTY0NTYsNDE2NTc1OTAyLC0xOT
g1MTY5MDIzLDQwNjkzMjU5MSwtMzcxMzI5Mzg1LDY2MTE4NzQ2
NCwtNjAzNjkyMTg4LDE1MzQ1ODIzNCwtMjEyMzkyNjEwMSwtMT
ExNDAwOTgxNSw5ODk3NDI2NTldfQ==
-->