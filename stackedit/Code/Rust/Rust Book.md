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

#### Random numbers
* Uses a crate name d `rand`
* in `Cargo.toml`:
```toml
[dependencies]
rand = "0.8.5"
```
* 

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTA0OTI3NzczNCwtMzcxMzI5Mzg1LDY2MT
E4NzQ2NCwtNjAzNjkyMTg4LDE1MzQ1ODIzNCwtMjEyMzkyNjEw
MSwtMTExNDAwOTgxNSw5ODk3NDI2NTldfQ==
-->