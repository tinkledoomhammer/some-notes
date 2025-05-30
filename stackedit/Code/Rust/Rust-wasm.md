
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
	(func 4fac (p
```






> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE4OTIzMzA5NjJdfQ==
-->