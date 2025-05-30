
# *Rust and WebAssembly*
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
: a wasm module has access to a single "linear memory" that can be




> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEyNjM4NjM5MjhdfQ==
-->