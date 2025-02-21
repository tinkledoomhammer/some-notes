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

	
 
> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbNTI3MjA3NzQxLC03NTA4NTY2OTMsLTI0OT
YzNDA5NywtNTA0ODgwODAsLTI3ODIzNDg1NF19
-->