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

`$arg` in form 3, the argument of a syntax extension invocation is a single token tree
* i.e. a single non-leaf token tree
```rust
bitflags! {
    struct Color: u8 {
        const RED    = 0b0001,
        const GREEN  = 0b0010,
        const BLUE   = 0b0100,
        const BRIGHT = 0b1000,
    }
}

lazy_static! {
    static ref FIB_100: u32 = {
        fn fib(a: u32) -> u32 {
            match a {
                0 => 0,
                1 => 1,
                a => fib(a-1) + fib(a-2)
            }
        }

        fib(100)
    };
}

fn main() {
    use Color::*;
    let colors = vec![RED, GREEN, BLUE];
    println!("Hello, World!");
}
```

`$arg` is different in form 1 and 2
* They are a 'simple path' that is either followed by 
	* an `=` token and a literal expression or
	* a token tree
Important Takeaways
1. There are multiple kinds of syntax extensions in Rust
2. Just seeing something inthe form `$name! $arg` doesn't tell what kind of syntax extension it is
3. THe input ot every `!` macro invocation (form 3) is a single non-leaf token tree
4. Syntax extensions are parsed as part of the AST

4 above means that macro invocations can only appear in places where they are specifcally supported such as:
1. Patterns
2. Statements
3. Expressions
4. Items (this includes `impl` items)
5. Types
They cannot be used in:
1. Identifiers
2. Match arms
3. Struct fields
There is no way to use syntax extensions in any position not on the first list

### 1.3 Expansion
* After Parsing
* Traverses the AST, locating syntax extension invocations and replacing them with their expansion
* A syntax expansion can result in any of the following depending on where it is expanded 
1. an expression
2. a pattern
3. a type
4. zero or more items (at the module scope)
5. zero or more statements

Expansion is an operation on the AST, not a textual operation

* The results of an expansion are AST nodes i.e. adding `(..)` around it doesn't do anything
* Syntax extensions cannot expand into incomplete of syntactically invalid constructs
* The expansion happens in passes
* The passes are limited to 128 recursions.
	* `#![recursion_limit="..."]` attribute applied to a crate can change the limit

### 1.4 Hygiene
Hygiene
: The ability for a macro to work in its own syntax context, not afecting nor being affected by its surroundings
: The macro can be invoked anywhere without interfering with its surrounding context

* Not all syntax extensions are fully hugenic
* Hygiene mainly affects identifiers and paths emitted by syntax extensions
* For identifiers created by syntax extensions hygiene means that the symbol cannot be accessed by the environment
* For identifiers used in the extension it means they canno reference something defined outside of the extension

create
: when an identifier introduces something new under a name

use
: when an identifier refers to something existing

### 1.5 Debugging
`rustc -Zunpertty=expanded` expands macros
the `cargo-expand` plugin is a wrapper around that cli option

## 2 Declarative macros
### 2.2 A practical introduction





> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE4NzU0Mjg0MDcsMTIyMTIwNjI3Nl19
-->