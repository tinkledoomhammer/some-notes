# The Rust Book
"The Rust Programming Language"
https://rust-book.cs.brown.edu/

## Installation and such
Rust is installed and it's versions are managed with `rustup` which is a small download. https://www.rust-lang.org/tools/install Running it in windows uses a cli installer (press 1 for default setup). It will update the command path so that `rust c` `cargo` and `rustup` are all available

Also see
https://users.rust-lang.org/t/setting-up-rust-with-vs-code/76907
for vs code setup

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
	* 
> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTczNjkyNjI2NSwtMjEyMzkyNjEwMSwtMT
ExNDAwOTgxNSw5ODk3NDI2NTldfQ==
-->