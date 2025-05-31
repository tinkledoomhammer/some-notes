
# *Rust and WebAssembly*
## Conway's Game of Life tutorial
https://rustwasm.github.io/docs/book/

## 1 Frontmatter
> This book is for anyone interested in compiling Rust to WWebAsembly ... You should know some Rust, and be familiar with JavaSript, HTML, and CSS. You don't need to be an expert in any of them.
[Learn about them on MDN](https://developer.mozilla.org/en-US/docs/Learn)

## 2 Why rust and WebAssembly

* Low level ontrol with High-level Ergonomics
* Small `.wasm` files
	* Unused functions are not included
	* no runtime, garbage collector, etc
* Existing code can be replaced modularly
* Plays well with others
	* Integrates with existing JavaScript tooling. Support s ECMAScript modules
	* can continue using npm, WebPak, etc
* Amenities
	* `cargo` package management
	* expressive zero-cost abstractions
	* community

## 3 Background and concepts
### 3.01 What is webasembly
[extensive specification](https://webassembly.github.io/spec/)

`.wat` text format
: "**W**eb**A**ssembly **T**ext"
: Uses [S-expressions](https://en.wikipedia.org/wiki/S-expression) like Scheme and Clojure

`.wasm` binary format
; lower level, conceptually simiar to ELF and Mach-O

```wat
(module
	(func 4fac (param f64) (result f64)
		local.get 0
		f64.const 1
		f64.lt
		if (result f64)
			f64.const 1
		else
			local.get 0
			local.get 0
			f64.const 1
			f64.sub
			call $fac
			f64.mul
		end)
	(export "fac" (func $fac))
)
```

* [wat2wasm demo](https://webassembly.github.io/wabt/demo/wat2wasm/) to see how it compiles

Linear Memroy
: wasm has a very simple [memory model](https://webassembly.github.io/spec/core/syntax/modules.html#syntax-mem)
: a wasm module has access to a single "linear memory" that can be grown by multiples of 64K, but cannot shrink

Is webAssembly just for the web?
: for now, yes but it could be a general portable application format

## 4 Tutorial
* Intended audience
	* basic knowledge of Rust, JS, and HTML
* Learning goals
	* How to setup a Rust toolchain for compiling WebAssembly
	* A workflow for developing polyglot programs
	* How to design appropriate APIs for use with the various languages
	* How to debug wasm modules compiled from rust
	* how to (time and size) (profile and optimize) (Rust and WebAssembly) programs 

### 4.01 Setup
* Rust toolchain:
	* `rustup` `rustc` and `cargo`
* `wasm-pack`
	* [Get  `wasm-pack`  here!](https://rustwasm.github.io/wasm-pack/installer/)
	* or just `cargo install wasm-pack` to build from source
	* or `npm install wasm-pack` to install the binary
* `cargo-generate`
	* https://github.com/ashleygwilliams/cargo-generate
	* `cargo install cargo-generate`
* `npm`
	* JS package manager that this tutorial uses to
		* install and run a JS bundler and dev server
		* publish the compiled `.wasm` to `npm` at the end of the tutorual
	* [Follow these instructions to install  `npm`](https://www.npmjs.com/get-npm)
		* It doesn't actually have installation instructions
		* so I installed node
	* to update npm `npm install npm@latest -g`
*

### 4.02 "Hello, World"




> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbNjAxMjAzNTg5LDEyMjQxNzg3MjQsLTE3Mj
IyNjQyMjRdfQ==
-->