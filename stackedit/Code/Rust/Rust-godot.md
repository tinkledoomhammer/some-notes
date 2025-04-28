
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

* LLVM
	* Required for older versions
	* still required to use `api-custom` feature
	* fore more info see https://godot-rust.github.io/book/toolchain/godot-version.html

### Hello World
```
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
```

Create the `rust` folder and `Cargo.toml` file:
`cargo new "rust" --lib`

--------

`HelloWorld.gdextension` - in the godot project folder
```ini
[configuration]
entry_symbol = "gdext_rust_init"
compatibility_minimum = 4.1
reloadable = true

[libraries]
linux.debug.x86_64 =     "res://../rust/target/debug/lib/rust_project.so"
linux.release.x86_64 =   "res://../rust/target/release/lib/rust_project.so"
windows.debug.x86_64 =   "res://../rust/target/debug/rust_project.dll"
windows.release.x86_64 = "res://../rust/target/release/rust_project.dll"
macos.debug =            "res://../rust/target/debug/lib/rust_project.dylib"
macos.release =          "res://../rust/target/release/lib/rust_project.dylib"
macos.debug.arm64 =      "res://../rust/target/debug/lib/rust_project.dylib"
macos.release.arm64 =    "res://../rust/target/release/lib/rust_project.dylib"
```

--------

`cargo.toml`
```ini
[package]
name = "rust_project"
version = "0.1.0"
edition = "2024"
[lib]
crate-type = ["cdylib"]
[dependencies]
godot = "0.2.4"
```

--------------

`rust/src/lib.rs`
```rust
use godot::prelude::*;

use godot::classes::Sprite2D;
use godot::classes::ISprite2D;


struct MyExtension;

#[gdextension]
unsafe impl ExtensionLibrary for MyExtension {}

#[derive(GodotClass)]
#[class(base=Sprite2D)]
struct Player{
    speed: f64,
    angular_speed: f64,
    base: Base<Sprite2D>

}

#[godot_api]
impl ISprite2D for Player{
    fn init(base: Base<Sprite2D>) -> Self{
        godot_print!("Hello, world!");
        Self { speed: 400.0,
            angular_speed: std::f64::consts::PI,
            base
        }
    }

    fn physics_process(&mut self, delta: f64){
        let radians = (self.angular_speed * delta) as f32;
        self.base_mut().rotate(radians);

        let rotation = self.base().get_rotation();
        let velocity = Vector2::UP.rotated(rotation) * self.speed as f32;
        self.base_mut().translate(velocity * delta as f32);
    }
}

#[godot_api]
impl Player{
    #[func]
    fn increase_speed(&mut self, amount: f64){
        self.speed+=amount;
        self.base_mut().emit_signal("Speed_increased", &[]);
    }

    #[signal]
    fn speed_increased();
}
```
--------------

`cargo add godot`
: Adds the libraries for gdext

`cargo build`
: The first run took about 5 minutes, and was mostly building dependencies

#### `.gdextension` file
Tells godot how to load the compiled extension

##### The `[configuration]` section
`entry_symbol`
: the entry point
* must be `"gdext_rust_init"` -- this can be configured

`compatibility_minimum`
: minimum godot version required by teh extension

`reloadable`
: if `true` then the extension will reload when the editor window loses and regains focus
* see https://github.com/godotengine/godot/pull/80284

##### The `[libraries]` section
: contains the path to the binary files
* in `os.dist.arch = "res://..."` format
* see [GDExtension Docs](https://docs.godotengine.org/en/stable/tutorials/scripting/gdextension/gdextension_cpp_example.html#using-the-gdextension-module) for allowed keys
* At a minimum the current os in debug mode must be specified

#### Troubleshooting first-time setup
* run `cargo build`
* does `Cargo.toml` have `crate-type = ["cdylib"]`
* in `my-extension.gdextension` must have `entry_symbol= "gdext_rust_init"`
* check paths in `my-extension.gdextension`
	* paths must be relative to `project.godot` and use `res://...`
* is an entry point established in the rust project (see below)
* are the versions of gdext and godot compatible
* Try clearing the godot cache in the `.godot` folder. 
	* leave the `extension_list.cfg`
* if `api-custom` is used:
	* godot must be in `PATH` env var as `godot4` (not sure what that means)
	* or an env var anmed `GODOT4_BIN` that points to the godot executable
* verify the directory structure
	* there must be a usable library file i.e. `rust/target/debug/my_extension.dll`
* 

#### lib.rs details

##### Entry point
```rust
use godot::prelude::*;
struct MyExtension;

#[gdextension]
unsafe impl ExtensionLibrary for MyExtension {}
```
* `godot::prelude::*` has the most common symbols that will be used by gdextensions
* `struct MyExtension;` just a tag type, can be named whatever
* `impl ExtensionLibrary` - must be marked with the `#[gdextension]` attribute
	* will declare the entry point, proc-macros handle the details

##### Rust class
Declared like 
```rust
use godot::prelude::*;
use godot::classes::Sprite2D;

#[derve(GodotClass)]
#[class(base=Sprite2D)]
struct Player {
	...
	base : Base<Sprite2D>,
}
```
* the `godot::prelude` has the most used symbols
	* `godot::engine` has some less-frequently used classes
* `#[derive]` attribute registers the class with the game engine
	* https://godot-rust.github.io/docs/gdext/master/godot/register/derive.GodotClass.html for details
* the `[#class]` attribute is optional and configures how the class is registered. If no `base` is specified then `RefCounted` will be used by default
* the `Base<T>` type allows the base object to be accessed via `self.base()` and `self.base_mut()`
	* This is a composition relationship
	* `T` must 

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE0NjQ2NTM0MDIsLTEyOTM2OTMwMDksLT
E4NjAwOTMxMTIsLTczMzE4Mjc2NSwtMTM5MzE3Nzg4MCwtMTcx
MTIwMDUzM119
-->