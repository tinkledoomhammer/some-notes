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

In the command Palette (`Ctrl+Shi

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEyMzEzMjA0NjUsLTEwOTc2OTcwMjJdfQ
==
-->