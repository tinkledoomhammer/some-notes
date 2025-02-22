https://cplusplus.com/doc/tutorial/
## Basics of C++

### Structure of a program
```c++
// My first program in C++
#include <iostream>
using namespace std;

int main(){
	std::cout << "Hello World";
}

```
### Variables and types
```c++
alignas, alignof, and, and_eq, asm, auto, bitand, bitor, bool, break, case, catch, char, char16_t, char32_t, class, compl, const, constexpr, const_cast, continue, decltype, default, delete, do, double, dynamic_cast, else, enum, explicit, export, extern, false, float, for, friend, goto, if, inline, int, long, mutable, namespace, new, noexcept, not, not_eq, nullptr, operator, or, or_eq, private, protected, public, register, reinterpret_cast, return, short, signed, sizeof, static, static_assert, static_cast, struct, switch, template, this, thread_local, throw, true, try, typedef, typeid, typename, union, unsigned, using, virtual, void, volatile, wchar_t, while, xor, xor_eq
```

Group | type names* | Notes on size/precision
---|---|---
**Character Types** | char | exactly one byte, at least 8 bits
  || `char16_t` | not smaller than char, at least 16 bits
  || `char32_t` | not smaller than `char16_t`. At least 32 bits
  | | `wchar_t` | can represent the largest supported char set 
**Integer types (signed)** | `signed char`| same size as char
 `signed` is optional| | `signed short int` | Not smaller than `char`, at least 16 bits
 `int` is optional except in `signed int`| | `signed int` | Not smaller than `short`
  | | `signed long int` | Not smaller than `int`, at least 32 bits
  | | `signed long long int` | Not smaller than long. At least 64 bits
  **Integer Types** | `unsigned char` | same as their signed counterpart
  **(unsigned)** | `unsigned short int` |..
  `int` is optional except for `unsigned int` | `unsigned long int`|
  | | `unsigned long int` |
  | | `unsigned long long int` | ..
  **Floating-point types** | `float` |
  | | `double` | precision not less than float
  | | `long double` | precision not less than double
** Others ** | `bool` |
| | `void` | no storage
Null Pointer | `decltype(nullptr)` | or assign `0` to a pointer0

* The actual size of these types depends on the data model.  
	* The `numeric_limits`[https://cplusplus.com/numeric_limits] classes in the standard header `<limits>` provides info.
	* The `<cstdint>` header has fixed size type aliases

* There are 3 types of initialization:
	* "c-like" : ` int x = 0;`
	* "constructor initialization" ` int x (0);`
	* "uniform initialization" `int x {0};`
* Types can be inferred at compile time :
	* `auto foo = 0;` Will use the type of the initializer value as the type of the variable
	* `decltype(foo) bar` Uses the type of foo as the type of bar

* String: A compound type
```c++
#include <string>

int main(){
	std::string myString = "This is a string"
}
```
### Constants
#### Literals
* Integers
	* Default to `int`
	* can be changed with suffexes:

Suffix | Type modifier
--|--|
`u` or `U`| `unsigned`
`l` or `L` | `long`
`ll` or `LL` | `long long`

	*	They can be combined for unsigned longs, etc
*	Floating point
	*	default `double`
	*	suffix with `f` or `F` for `single`
	*	suffix with `l` or `L` for `long double`
*	character and string literals
*	single quotes for char : `'a', 'b'`, etc
*	double quotes for strings `"a", "ab"`
*	Escape Codes
	
Escape Code | Description
--|--|
`\n` | newline
`\r` | Carriage return
`\t` | Tab
`\v` | Vertical tab
`\b` | Backspace
`\f` | form/page feed
`\a` | alarm (beep)
`\'` | single quote
`\"` | double quote
`\?` | ?
`\\` | \

* Strings are null terminated
* They are automatically concatenated, ignoring white space outside quotes:
	* `"a str"  "ing"` is the same as `"a string"`
* lines can be concatenated with `\`:
```c++
x = "string expressed in\
two lines"
```


* Does not include the newline in the string
* Prefixes can specify character types
	* `u` `char16_t`
	* `U` `char32_t`
	* `L` `wchar_t`
	* `u8` stored in the executable using UTF-8
	* `R` the string literal is raw, ignoring escape sequences
		* raw strings are prefixed snd suffixed with a custom sequence
		* `R"(string with \backslash)"` 
		* `R"sdg2&&^%dw(string with \backslash)sdg2&&^%"`
		* `"string with \\backslash"`
* Other LIterals
	* `true` `false` `nullptr`

#### Typed Constant expressions

* the `const` keyword in the variable decoration
* local constants can be initialized using variables

#### Preprocessor definitions
`#define identifier replacement`
* Is not a statement and does not need a semicolon
* `replacement` ends at the end of the line

### Operators
* Assignment `=` 
	* `x = y = z = 5;` is allowed (i.e. it is an expression)
* Arithmetic Operators
	* `+` , `-` , `*` . `/` , `%`(modulo)
* Compound assignment
	* `+=` , `-=` , `*=` , `/=` , `%=` , `>>=` , `&=` , `^=` , `|=` 
* Increment and decrement `++` and `--`
	* `++x` (pre-increment) increases x then returns the value
	* `x++` (post-increment) evaluates to the value of x before it was increased
* Relational and comparison operators
	* `==` , `!=` , `>` , `<` , `<=` , `>=`
* Logical Operators
	* `!` , `||` , `&&`
	* short circuit evaluation
* Conditional ternary operator `? :`
	* `condition ? result_true : result_false`
* Comma operator `,`
	* Is used to separate two expressions that are included where only one is expected
	* returns the value of the right-most (last) expression
* Bitwise operators
	* `&` , `|` , `^`(xor) , `~`(not) , `<<` , `>>` 
* Explicit type casting operator
	* c style: `i = (int) f`
	* new style : ` i = int(f)`
* sizeof
	* `x = sizeof (type)`
	* can be used on a variable or a type

Precidence
Level | Group | Operator | Description | Grouping|
--| --| --| --| --|
1 | Scope | `::`| scope qualifier | left-to-right
2 | Postifx(unary)|`++` `--` | postfix inc/decrement | Left-to-right|
| | | `()` | functional forms | "
|"|"|`[]`| subscript | "
|"|"|`.` `->` | member access | "
3|Prefix (unary)|`++` `--` | prefix inc/decrement | Right-to-left
|"|"|`~` `!` | Bitwise and logical not | "
|"|"|`+` `-`| unary prefix|"
|"|"|`&` `*` | reference / dereference
|"|"|`new` `delete` | allocation / deallocation|"
|"|"|`sizeof` | "parameter pack" | "
|"|"|`(type`) | C-style type-casting|"
4|Pointer-to-member|`.*` `->*` | access pointer | left-to-right
5| Arithmetic : scaling | `*` `/` `%` |multiply, divide, modulo | left-to-right
6| Arithmetic: addition | `+` `-` | addition, subtraction | left-to-right
7| Bitwise shift|||left-to=right
8|relational||comparison operators | left-to-right
9|Equality| `==` `!=` | equality/inequality | left
10|And|`&`| bitwise AND |left
11|Exclusive or|`^`|bitwise|left
12|Inclusive OR|`|`|bitwise|left
13|conjunction|`&&`|logical and | left-to-right
14|Disjunction|`||`|logical or | left
15|Assignment-level-expressions||assignment/ compound assignment| Right-to-left
|"|"|`?:` | conditional operator | "
16|Sequencing|`?`|coma separator | left-to-right





### Basic Input/output
#### Streams
* A program can insert or extract characters sequentially
* standard streams : `cin` `cout` `clog` `cerr`
* `std::cout` (`<iostream>`)
	* i.e. `std::cout << "string"` 
	* `<<` is called the insertion operator
	* `<<`  can be chained
	* It accepts a variety of types
	* `std::endl` sends `\n` and flushes the output buffer
* `std::cin`
	* used with the extraction operator `>>`
	* interprets characters based on the type of the right argument
		* i.e. `cin >> x` will parse numbers if `x` is a float or int
	* separated by white space
	* will hang until enter is pressed
	* can be chained i.e. `cin >> x >> y`
* `cin` with strings
	* to obtain strings with whitespace, use `getline(cin, mystr)`
* `std::stringstream(str)` from `<sstream>`
	* returns a stream from a string
	* using this with `getline()` is a good way to accept console input
```c++
#import <sstream>
#import <iostream>

int main(){
	std::string myString;
	std::getline(cin , myString);
	std::stringstream myStream( myString);
	myStream >> someVar;
}
```


## Program Structure

### Control Structures
#### blocks `{}`
* when the syntax calls for **a** *statement*, multiple statements can be combined in curly braces to form a single statement
#### `if` `else` 
* `if (condition) statement`
* `if (condition) statement1 else statement2`
* `if else` statements are chainable (i.e. `statement2` can be an `if` statement
#### Loops
* `while (expression) statement`
* `do statement while (expression)`
* `for (initialization ; condition ; increase) statement`
	* init, increase, and condition are all **expressions**
		* any/all of them can be compound (`,` separated lists) expressions
	*  The order is Init -> condition -> statement -> increase  -> condition
* `for ( declaration : range ) statement;`
	* `range` can be any type supporting the functions `begin()` and `end()`
		* this includes arrays and strings
* Jump statements
	* `break` leaves a loop
	* `continue` moves to the start of the next iteration (the condition?)
	* `goto` jumps to a label, ignoring nesting levels
	* labels `myLabel:` on its own line
```c++
switch (expression ) {
	case const1:
		group-of-statements-1;
		break;
	case ...
	default:
		final group of statements;
}
```
* if `break` is omitted, then execution will fall through to the next case
*  each case must be defined by a *constant* expression

### Functions


### Overloads and templates
### Name Visibility

## Compound data types
### Arrays
### Character sequences
### Pointers
### Dynamic Memory
### Data structures
### Other data types

## Classes
## Other Language Features
## C++ Standard Library
### Input/Output with files

	
 
> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTYwMDQ4ODA3LC02MTk0NzgwNTgsLTIwNj
U4NTM1OTMsMzkzNTI5MjY4LC0xNDI2MDYyOTE1LDUyNzIwNzc0
MSwtNzUwODU2NjkzLC0yNDk2MzQwOTcsLTUwNDg4MDgwLC0yNz
gyMzQ4NTRdfQ==
-->