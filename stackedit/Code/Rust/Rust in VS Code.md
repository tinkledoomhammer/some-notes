# M$ guide to using rust in vs code

https://code.visualstudio.com/docs/languages/rust

## Installation
1. Install rustup via its installer
	1. make sure that the path variable is set in the installer
2. Install rust analyzer  extension https://rust-analyzer.github.io
	1. Crl-shift-x and type in rust
3. Check rust installation
	1. `rust --version`
	2. `rustup update`
4. `rustup doc` to view the documentation

## Hello world 
`cargo new hello_world`
`cargo build`
`cargo run`
## InetlliSense
* provided by rust-analyzer
### Inlay hints
* show inferred types, returned types, and named parameters
* can be configured in `Editor > Inlay Hints:Enabled` setting

### Hover information
### Auto completions
* use `Ctrl+Space` to manually trigger
### Semantic syntax highlighting
* i.e. underlining mutable variables
* style can be changed with `editor.esmanticTokenColorCustomizations` in settings
```json
{
  "editor.semanticTokenColorCustomizations": {
    "rules": {
      "*.mutable": {
        "fontStyle": "", // set to empty string to disable underline, which is the default
      },
    }
  },
}

```
## Code Navigation
* `F12`  Go to definition (of a type)
* `Alt+F12` Peek definition 
* `Shift+F12` - go to references - show all references for a type
* `Shift+Alt+h` - show call hierarchy - shows all calls from or to a function

In the command Palette (`Ctrl+Shift+P`) `Go to Symbol` commands
* Go to symbol in file `ctrl+shift+O`
* go to symbol in workspace `Ctrl+T`

## Linting
>The rustc linter, enabled by default, detects basic Rust errors, but you can use [clippy](https://github.com/rust-lang/rust-clippy) to get more lints. To enable clippy integration in rust-analyzer, change the **Rust-analyzer > Check: Command** (`rust-analyzer.check.command`) setting to `clippy` instead of the default `check`. The rust-analyzer extension will now run `cargo clippy` when you save a file and display clippy warnings and errors directly in the editor and Problems view.

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEzNjMxNTU0NSwtMTA5NzY5NzAyMl19
-->