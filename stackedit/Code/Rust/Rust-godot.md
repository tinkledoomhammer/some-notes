
https://godot-rust.github.io/book/

`godot-rust`
: The projet covered in these notes, including bindings for gd3 and 4

`GDExtension`
: C API provided by Godot4

`GDNative`
: C API provided by Godot 3

`gdext`
: The rust binding for GDExtension (Godot4) what this book focuses (and these notes) focuses on

`gdnative`
: the rust bindings for GDNative (godot3) 

extension
: a dynamic C library developed in any language. These use the GDExtension API and can be loaded by Godot 4


The API docs: https://godot-rust.github.io/docs/gdext/master/godot/

## Intro
### Setup
https://godot-rust.github.io/book/intro/setup.html

* environment variable 

`GODOT4_BIN`
: `C:\Godot\Godot_v4.4-stable_win64\Godot_v4.4-stable_win64.exe`
* or it could be added to `PATH`

*s LLVM
	* Required for older versions
	* still required to use `api-custom` feature
	* fore more info see https://godot-rust.github.io/book/toolchain/godot-version.html

### Hello World

📂 project_dir
│
├── 📂 .git
│
├── 📂 godot
│   ├── 📂 .godot
│   ├── 📄 HelloWorld.gdextension
│   └── 📄 project.godot
│
└── 📂 rust
    ├── 📄 Cargo.toml
    ├── 📂 src
    │   └── 📄 lib.rs
    └── 📂 target
        └── 📂 debug





> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIwNDQwOTQ5MjIsLTE3MTEyMDA1MzNdfQ
==
-->