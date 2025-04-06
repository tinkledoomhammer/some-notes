# The Little Book of Rust Macros
https://veykril.github.io/tlborm/

## 1 Syntax Extensions
### 1.1 Source Analysis

Tokenization
: The first stage of rust compilation
: source text is transformed into a sequence of tokens

Tokens
: Indivisible lexical units i.e. words

Some types of tokens
1. Identifiers
2. Literals
3. Keywords `_` `fn` `self` `match`...
4. Symbols `[` `:` `::` `?` ...

**self** is both an *identifier* and a *keyword*
* In most cases it is a keyword but it is possible for it ot be treated as an identifier

Keywords and Symbols include tokens that ren't actually used in the language (at least not yet or not anymore)

`::` is a different than two `:` tokens

Parsing
: The stage after tokenization
: generates an AST

Token Trees
: somewhere between tokens and the AST

* Most tokens are also trees
	* Specifically they are leaves
* Most leaves are tokens
* The only basic tokens that are not tokens are **grrouping** tokens `(...)` `[...]` and `{...}`
* The AST is not actually composed of token trees. Each token gets its own tree (ish)

### 1.2 Macros in the AST
* Macros are processed **after** construction of the AST
* There are several "syntax extension" forms that are part of rust's syntax:
1. `# [ $arg ]` i.e. `#[derive(Clone)]`
2. `# ! [ $arg ]` i.e. `#![allow(dead_code)]`
3. `$name ! $arg` i.e. `println!("Hi!")`
4. `$name ! $arg0 $arg1` i.e. `macro_rules! dummy { () => {};}`

The 4th form is not available to macros, and is only used in the `macro_rules!` construct

`$arg`, the argument of a syntax extension invocation is a single token tree
* i.e. a single non-leaf token tree



> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTA0Mjk2MjU4OV19
-->