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




> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTUyMjEzNzA0OV19
-->