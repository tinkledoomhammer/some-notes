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
`return_type func_name (param1, param2, ...) { statements }`
* `param` 1, 2, etc, are `param_type param_name` 
	* adding `=val` to the end of the param declaration allows it to be omitted when calling the function
* `return val` is required if `return_type` is not `void`
* `void` can also be supplied as the parameter list for functions that take no parameters
* `main()` returns an `int`
		* `return 0` is automatically added
		* `<cstdlib>` defines some constants:
			* `EXIT_SUCCESS`
			* `EXIT_FAILURE`
*`inline` before the return type is a hint to the compiler that it should inline the function
* functions can be declared before they are defined
	* replace the `{ statements }` with `;`
	* parameter names are optional in this case, but should be included for clarity

#### Arguments passed by value vs by reference
* by default all arguments are passed by value (ie. copied)
* arguments passed by reference are an alias to the variable that was passed
* reference arguments are identified with `&` after the type:
	* `int& a`
* `const` before the type indicates that reference params will not be altered by the function
  

### Overloads and templates
* Multiple functions with the same name can exist
	* They must have different parameter types
	* They can also have different return types
	* Functions that differ only by return type are not valid overloads
* Function Templates:
	* `template <class ClassName>` before the normal function body
	* then `ClassName` can be used as a type in the function declaration and definition
	* `typename` can be used instead of class
	* invoke the function : `funcName<type>(args);`
	* `<type>` is optional when the type can be  inferred from the args
* Non-type arguments:
```c++
template <class T, int n> 
T fixed_multiply(T val){
	return val*n
}
int main(){
	std::cout << fixed_multiply<int,2>(10) << endl;
	...
```

### Name Visibility

#### Scope

* Global - declared outside of functions, visible anywhere
* Block scope - local variables, only visible to the end of that block (including children blocks)
	* A block's scope includes variables from declarations that introduce a block
	* a local variable can shadow variables from parent scopes

#### Namespaces

* Used to break down the global scope into groups

```c++
// create a namespace
namespace ns_id {
	named_entities
}
//refer to an entity in the namespace
ns_id::named_entity
//`using` for a single entity
using ns_id::named_entity
named_entity

//`using` for the entire namespace
using ns_id;
named_entity

//namespace aliasing
namespace ns2 = ns_id;
```
* `using` is scoped like a declaration
* the standard library puts its symbols in `namespace std`

#### Storage classes
* Static storage:
	* Used for variables with global or namespace scope
	* Will always be initialized. zeros will be used if no initializer is provided
* Automatic storage:
	* used for local variables
	* will not be zero'd automatically. use initializers or the values may be random


## Compound data types
### Arrays
* A series of elements of the same type placed in contiguous memory locations,
* individually referenced by adding an index
* Declared: `type array_identifier [no_of_elements];`
* Initialization `int array[5] = {1,2,3,4};`
	* the initializer can have no more elements than the array
	* If it has fewer, then new elements will be set to default values
	* the `=` is optional
	* if `no_of_elements` is omitted then the number of elements in the initializer will be used
* The array cannot be assigned after initialization. Instead its contents can be assigned one member at a time
* Accessing element `n` : `array_identifier[n] = x;`
	* It is syntactically valid to access elements beyond the end of the array
* Multidimensional arrays
	* `int two_by_10 [2][10]`	
		* Creates a 20 element array and remembers that the array is n * 10 elements
* Arrays as parameters: `void procedure(int arg[]);`
	* They are internally passed as pointers
	* for multidimensional arrays, all but the first dimension must be specified
	* `void procedure(int arg[][5][6]);`
* Library arrays `#include <array>`
	* `std::array<T,size> = {vals};`
	* will be coppied when passed as an argument
	* provides a `.data` member to get a pointer
	* has a `.size()` method which returns the allocated size
	
### Character sequences
* character arrays can be used to store strings
* typically they are terminated with a null character (`\n`)
* They can be initialized like other arrays or with a string literal
```c++
char myword[] = {'h','e','l','l','o','\0'};
char myword[] = "hello";
```
* after initialization, they cannot be assigned but their contents can be
* These are called 'C-strings'
* Most library functions are overloaded to support both c-strings and 'library strings' (`std::string` from `<string>`)
* Library strings are dynamically sized
```c++
char myntcs[] = "some text"
string mystring = myntcs; // convert a c-string to string
cout << mystring.c_str(); // printed as a c-string
// both c_str and data memberrs of string are equivalent
```

### Pointers
#### `*`
* `*` is the dereference operator in most expressions. meaning 'value pointed to by'
	* `int foo = *bar;` 
		* requires `int * bar;` 
		* returns the `int` pointed to by `bar`
* `*` in type declarations it means that the variable is a pointer :
	* `int * bar;` means that 'bar' is a pointer to an int
* `&` in most expressions means 'address of '
	* in declarations it means that the variable is a reference
	* `int * bar = &foo` points `bar` to `foo`, so `foo == *bar`
#### Pointers and arrays
```c++
int myArray[20];
int * myPointer;

myPointer=myArray; // points to the beginning of the array
//pointers support all array operations and additionally
myPointer[5] = *(myPointer+5) // + is the same as the offset operator ([])
```
#### pointer arithmetic
* only addition and subtraction are supported
* The results depend on the `sizeof(T)` the type pointed to
* Precedence:
	*  `*p++` is the same as `*(p++)`
	* `++*p` increments the value pointed to `++(*p)`
	* Postfix unary operators have a higher precedence than prefix operators

```c++
	*p++   // same as *(p++): increment pointer, and dereference unincremented address
	*++p   // same as *(++p): increment pointer, and dereference incremented address
	++*p   // same as ++(*p): dereference pointer, and increment the value it points to
	(*p)++ // dereference pointer, and post-increment the value it points to 
```
#### Pointers and `const`
```c++
int x;
      int *       p1 = &x;  // non-const pointer to non-const int
const int *       p2 = &x;  // non-const pointer to const int
      int * const p3 = &x;  // const pointer to non-const int
const int * const p4 = &x;  // const pointer to const int 
```
* the first  `const` can come before or after the pointer type
	* `const int *` and `int const *` mean the same thing
* `const char *` variables can be initialized with string literals
* 
### Dynamic Memory
### Data structures
### Other data types

## Classes
## Other Language Features
## C++ Standard Library
### Input/Output with files

	
 
> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTg2OTUxOTc1MCw3NzIzODEwOTMsLTM2Mj
E3MzYwMywtMzkzODA4NTE2LDk5MDE2ODM4MSwxODg5ODc1MzE5
LC02MTk0NzgwNTgsLTIwNjU4NTM1OTMsMzkzNTI5MjY4LC0xND
I2MDYyOTE1LDUyNzIwNzc0MSwtNzUwODU2NjkzLC0yNDk2MzQw
OTcsLTUwNDg4MDgwLC0yNzgyMzQ4NTRdfQ==
-->