
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
* after installation, using `c:\proj\rust\wasm\wasm01\`
* `cargo-generate --git https://github.com/rustwasm/wasm-pack-template`
* when prompted for a project name, use `wasm-game-of-life`
*I used `w-gol` instead

`cd w-gol`  and then `ls`

wgol/
: ├── Cargo.toml
: ├── LICENSE_APACHE
: ├── LICENSE_MIT
: ├── README.md
: └── src
:    ├── lib.rs
:    └── utils.rs

`w-gol/Cargo.toml`
: preconfigured with a `wasm-bindgen` dependency and a few optional dependencies
: uses the correct crate-type: `crate-type = ["cdylib", "rlib"]`

`w-gol/src/lib.rs`
: exports a `greet` rust funnction, and imports the `window.alert` JS function
: uses `wasm-bindgen` to interface with JS
```rust
mod utils;

use wasm_bindgen::prelude::*;

//When the 'wee_alloc' feature is enabled,
// use 'wee_alloc' as the global allocator
#[cfg(feature = "wee_alloc")]
#[global_allocator]
static ALLOC: wee_alloc::WeeAllocwee_alloc::weeAlloc::INIT;

#[wasm_bindgen]
extern{
	fn alert(s: &str);
}

#[wasm_bindgen]
pub fn greet(){
	alert("Hello, wasm-game-of-life!");
)
```
`w-gol/src/utils.rs`
: provides common utilities. For now it just sets the error panic hook if that feature is enabled

#### Build the project

`wasm-pack build`
: Compiles the `.wasm` via `cargo build` and 
: uses `wasm-bindgen` to generate the JS api

* new folder `pkg/`

pkg/
: ├── package.json
: ├── README.md
: ├── w-gol_bg.wasm
: ├── w-gol.d.ts
: └── w-gol.js
* `README.md` is copied from the project root
* `pkg/wasm_game_of_life_bg.wasm`
	* the compiled rust code in `wasm`
* `pkg/wasm_game_of_life.js`
	* generated bindings that will import and export as needed
	* mostly imports from other files... My dir has several more files than the example
* `pkg/wasm_game_of_life.d.ts`
	* the typescript definitions required to use TS goodness 
* `pkg/package.json`
	* used by `npm` and JS bunlers to determine dependencies,
	* helps to integrate with toolchain integration

#### Putting it into a web page
in `was-game-of-life/`
`npm init wasm-app www`


```json
module.exports = {  
  //......  
  devServer: {  
    ....        
  },  
  watchOptions: {  
    poll: 1000,   
    //......  
  },  
  ....  
}
```
Using this setting your CTRL + C command will be lightning fast.  
here is the  [documentation](https://webpack.js.org/configuration/watch/#watchoptionspoll)  if you want to read more about the config

New `www` subdirectory
* contains the files that will be used by the frontend

wasm-game-of-life/www/
: ├── bootstrap.js
: ├── index.html
: ├── index.js
: ├── LICENSE-APACHE
: ├── LICENSE-MIT
: ├── package.json
: ├── README.md
: └── webpack.config.js

* this `package.json` is preconfigured by the `wasm-app` template
* `www/webpack.config.js`
	* configures webpack and its local dev server. 
	* preconfigured and shouldn't need to be tweaked
	* See the above snippet for graceful exiting
	* Also changes are required in newer versions of webpack to use wasm at all
* `www/index.html`
	* the root HTML file for the webpage
	* mostly just loads `bootstrap.js`
* `www/index.js`
	* the main entry point for the page's JS.
	* imports `hello-wasm-pack` npm package which contains
		* `wasm-pack-template`'s comipled webassembly and JS glue
		* then calls `hello-wasm-pack` 's greet function
		
`npm install` within `www/`
: to install all the dependencies 
: like the dev server and the `webpack` JS bundler
: alternatives include Parcel and Rollup
: the bundler isn't strictly required
* it seems that `wasm-app` is quite old and links to a bunch of deprecated packages
#### Using our local `wasm-game-of-life` package in `www`
* instead of the `hello-wasm-pack` from npm
* editing `www/package.json` in the `devDependencies` section
	* add the `"dependencies"` field and include `"wasm-game-of-lafe" : "file: ../pkg"
	```json
	{
		//...
		"dependencies": {
			"was-game-of-life": "file:../pkg"
		},
		"devDependencies": {
			//...
		}
	}
	```
* Edit `www/index.js` to `import * as wasm from "wasm-game-of-life";` 
	* instead of `...from "hello-wasm-package";`

* found a typo. apparently mmy `wasm_game...` are all `was_game...`

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTc3MDIyMDgsLTY3MzMwNzQxNSw5NjA5OD
I0NCwtNDA1MDk3ODcsMTE0ODc1ODEwMyw2ODA4NTIwODUsLTE1
MDEwNjk0MDMsNjAxMjAzNTg5LDEyMjQxNzg3MjQsLTE3MjIyNj
QyMjRdfQ==
-->