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
	* 



> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTExMDM3MTU1NzEsLTM3MTMyOTM4NSw2Nj
ExODc0NjQsLTYwMzY5MjE4OCwxNTM0NTgyMzQsLTIxMjM5MjYx
MDEsLTExMTQwMDk4MTUsOTg5NzQyNjU5XX0=
-->