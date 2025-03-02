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
#### Reserved Words
```c++
alignas, alignof, and, and_eq, asm, auto, bitand, bitor, bool, break, case, 
catch, char, char16_t,char32_t, class, compl, const, constexpr, const_cast,
continue, decltype, default, delete, do, double, dynamic_cast, else, enum,
explicit, export, extern, false, float, for, friend, goto, if, inline, int,
long, mutable, namespace, new, noexcept, not, not_eq, nullptr, operator, or,
or_eq, private, protected, public, register, reinterpret_cast, return, short,
signed, sizeof, static, static_assert, static_cast, struct, switch, template,
this, thread_local, throw, true, try, typedef, typeid, typename, union,
unsigned, using, virtual, void, volatile, wchar_t, while, xor, xor_eq
```

#### Simple types
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
#### `*`, `&`, and `->`
* `*` is the dereference operator in most expressions. meaning 'value pointed to by'
	* `int foo = *bar;` 
		* requires `int * bar;` 
		* returns the `int` pointed to by `bar`
* `*` in type declarations it means that the variable is a pointer :
	* `int * bar;` means that 'bar' is a pointer to an int
* `&` in most expressions means 'address of '
	* in declarations it means that the variable is a reference
	* `int * bar = &foo` points `bar` to `foo`, so `foo == *bar`
* `->` is a dereference and member access operator
	* `a -> b` is the same as `(*a).b`
	* as opposed to `*a.b` which is `*(a.b)`
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
#### Void, null, invalid, and function pointers
* `void * ptr;` must be cast before they can be dereferenced
* null pointers can be `0` , `nullptr` , and in older code `NULL`
* Accessing uninitialized pointers our out of bounds pointers (like with arrays) causes undefined behavior
* function pointers enclose the name (with a preceeding *) in parens, and include a parameter list:
	* `int (*func_ptr)(int,int));`
```c++
int funcName (int,int); // the function declaration
int (*funcPtr)(int,int) = &funcName; // the pointer declared/initialized

(*funcPtr)(a,b);// call with pointer
funcName(a,b);//equivalent call
```
### Dynamic Memory

```c++
//Can be allocated as an array
int * foo;
foo = new int [5]
//use delete[]
delete[] foo;

//Or a single item can be allocated
int *bar = new int;
delete bar;
```
* C-style dynamic memory `<cstdlib>` (in c it is called `<stdlib.h>`)
	* functions include `malloc` `calloc` `realloc` and `free`
	* These should not be mixed... either use `new` and `delete` or the c versions


### Data structures
```c++
//declare the struct type with a type name and/or varaible names
struct structname_t {
	int member1;
	char * member2;
	...
} instance_1, instance_2;
//the type-name can be used to create more
structname_t *instance 3;
//instance members can be accessed like other types
instance_1.member1; 
instance_3->member1;
```
* structs can be nested

### Other data types
#### Type aliases
* `typedef existing_type new_type;` i.e. `typedef char C`
	* older c-style
	* has some limitations when used with templates
* `using new_type = existing_type;` i.e. `using C = char;`
* i.e. `using field = char[50]` is the same as `typedef char field[50]`
	* not sure how to parse the the second one
#### Unions
* Creates a type with multiple members that all occupy the same memory
* The size of the union is the size of the largest member
* The format is the same as for structs, but with the `union` keyword
* can be nested with structs
```c++
// a 32-bit value that is also 2 16-bit values and 4 chars
union mix_t {
	int ax;
	struct {
		short low;
		short high;
	} a
	char c[4]
} mix;
sizeof(mix_t) // == 4
```
* "The exact alignment and order of the members of a union in memory depends on the system, with the possibility of creating portability issues."
* Anonymous unions
	* A union declared within a struct does not need to have a variable name
	* Its members are accessed as though they were members of the struct directly
#### Enums
`enum type_name { val1, val2, ...} var1, var2...;`
` var1 = val1;`

* values of enum types can be implicitly converted to integer types
* the default value is 1 + the previous element, 0 for the first
* values can be assigned manually : `enum color_t {black = 0, white = 10, red =1, blue, green};`
	* `green == 3`
* values can be scoped : `color_t::white`
* enum classes ` enum class typename {val1, val2...};`
	* do not convert to ints
	* must be scoped in : `typename::val1`
* they can alternatively specify an integer type : 
	* `enum class typename : char {val1, ...};`
* 
## Classes
### Basics
#### Declaring classes

```c++
class type_name {
	access_specifier1:
		type member_1;
} var_names;	
type_name var_name;
```

* class can be a struct or union
	* the default access specifier is `public:` for `struct`s and `union`s
	* the default access specifier is `private:` for `class`es
* Allowed access specifiers:
	* `private:`  only accessible from the same class or "friends"
	* `protected:` also allows access from derived classes
	* `public:` available everywhere
* Classes work like regular structures, but can define methods
* Defining methods:
```c++
class ClassName{
	//private by default
	int m_x
public:
	int methodName(int);
};
int ClassName::methodName(int par1...){
	//this is a ClassName* that points to this object
	return this.x+par1;
}
```
#### Constructors
* methods with the return type of the class, and no name
* never return values, not even void
* cannot be called explicitly i.e. `classInstance.ClassName(args);//Not valid`
* They can be overloaded with a different argument list
* The default constructor takes no arguments, and cannot be called
	* it is only used for uninitialized instances i.e. `ClassName instance;` not `ClassName instance();`
	* the second case looks like a function declaration
* 
```c++
class ClassName {
	int member;
public:
	ClassName(int);
}

ClassName::ClassName(int val){
	member = val;
	//same as this.member = val
}
```
* there are lots of ways to call constructors
```c++
// classes and uniform initialization
#include <iostream>
using namespace std;

class Circle {
    double radius;
  public:
    Circle(double r) { radius = r; }
    double circum() {return 2*radius*3.14159265;}
};

int main () {
  Circle foo (10.0);   // functional form
  Circle bar = 20.0;   // assignment init.
  //the following can be used with default constructors: Cricle xyz {}
  Circle baz {30.0};   // uniform init.
  Circle qux = {40.0}; // POD-like

  cout << "foo's circumference: " << foo.circum() << '\n';
  return 0;
}
```
* Member initialization in constructors
```c++
Rectangle::Rectangle(int x, int y) {width = x; height = y) //normal
Rectangle::rectangle(int x, int y) : width(x), height(y) {//empty block}
Rectangle::rectangle(int x, int y) : width{x}, height{y} {} //curley braces
```
* Initializer example with no default constructor
```c++
Class Circle{
	double radius
public:
	Circle(double r) : radius{r} {}
};
class Cylinder{
	Circle base;
	double height;
public:
	//This initializes the circle with the radius
	Cylinder(double r, double h) : base(r), height(h){}
};
```

* member access operator for use with pointers
* `x->y` is the same as `(*x).y`
### Classes II
#### Operator overloading
* Overloadable operators:
```c++
+    -    *    /    =    
    >    +=   -=   *=   /=   

   >>  

=  >>=  ==   !=   
=   >=   ++   --   %    &    ^    !    |  
~    &=   ^=   |=   &&   ||   %=   []   ()   ,    ->*  ->   new   
delete    new[]     delete[]
```
* operators are regular functions with special names
	* `type operator sign (params) {/*body*?}`
	* i.e. `CVector Cvector::operator+ (const CVector& param){...`
* They can be called implicitly `a+b` using the operator
* they can be called explicitly `a.operator+(b)`
* For member functions, `this` will be the left operand
* Overload parameters (the `@`in the table should be replaced with the operator)

Generic expression | Operators | Member function | non-member function
-|-|-|-
`@a` | `+ - * & ! ~ ++ --` | `A::operator@()` | operator@(A)
`a@` | `++ --` | `A::operator@(int)` | `operator@(A,int)`
`a@b` | <code>+ - * / % ^ & < > == != <= >= << >> && , \| \|\| </code> | `A::operator@(B)`| `operator@(A,B)`
`a@b` | <code>= += -= *= /= %= ^= &= \|= <<= >>= []</code> | `A::operator@(B)` | n/a
`a(b,c...)` | `()` | `A::operator()(B,C...)` | n/a
`a->b` | `->` | A::operattor->() | n/a
`(TYPE) a` | `TYPE` | `A::operator TYPE()` n/a
#### `this` is a pointer available in method bodies that points to the instance onwhich the method is invoked
#### `static` members
* class variables
* can be accessed as members of an instance or as members of a class
```c++
class A{
	public:
		static int x;
		A(){x++;};
		static void func(){
			this->x;//Error. 'this' is undefined
		}
}
int main(){
	A::x ;// uninitialized
	A::x = 0; // must be initialized outside of the class 
			//to avoid calling the initializer repeatedly
	A a;
	a.x ; // = 1 because the the constructor increased it
	A::x == a.x;	
}
```
* static members should be initialized outside of the class
* static methods do not have a `this`

#### `const` instances and member functions
* when an instance of the class is declared const (`const A a;`)
	* all members of the class are considered `const`
	* only methods declared as `const` are permitted
	* constructors can initialize members as normal
* `const` methods
		* `public: void constFunc() const {...}`
		* such methods can change static member variables but not instance members
		* they can overload non-const methods. In that case the const version will only be called on `const` instances
* `mutable` 
```c++
int get() const {return x;}        // const member function
const int& get() {return x;}       // member function returning a const&
const int& get() const {return x;} // const member function returning a const& 
```
#### Class Templates
```c++
// template specialization
#include <iostream>
using namespace std;

// class template:
template <class T>
class mycontainer {
    T element;
  public:
    mycontainer (T arg) {element=arg;}
    T increase () {return ++element;}
};

// class template specialization:
template <>
class mycontainer <char> {
    char element;
  public:
    mycontainer (char arg) {element=arg;}
    char uppercase ()
    {
      if ((element>='a')&&(element<='z'))
      element+='A'-'a';
      return element;
    }
};

int main () {
  mycontainer<int> myint (7);
  mycontainer<char> mychar ('j');
  cout << myint.increase() << endl;
  cout << mychar.uppercase() << endl;
  return 0;
}
```
* mostly work like function templates
* Specialization allows for a custom template for specific types
	* specializations must define all members, even those that are identical to the generic template

### Special member functions
#### Default constructor `C::C();`
* called when the object is initialized without any arguments
* takes no arguments, has no return type (not even `void`)
* if the class defines no constructors, then an automatic default constructor will be created
	* It does nothing
* If a class defines one or more constructors, but not a default constructor, then it cannot be declared without initialization
* 
#### Destructor `C::~C();`
* called automatically at the end of the object's lifetime
* takes no arguments, has no return type (not even `void`)
* should de-allocate resources (i.e. `delete` if the class uses `new`)
* If no destructor is defined, a default will be created implicitly but it does nothing

#### Copy Constructor `C::C(const C&);`
* called when initializing new objects
* If no move or move assignment constructor is defined, a default will be usedd
	* it just shallow copies members


#### Copy Assignment `C & operator= (const C&);`
* like the copy constructor but it is used to assign new values in circumstances other than initialization
* has a default
	* when no move constructor or assignment is defined
	* performs a shallow copy

#### Move Constructor `C::C(C&&);` and move assignment `C& operator=(C&&);`
* `&&` is called an 'r-value reference'
	* it can be used to refer to temporary r-values, like the return from a method or cast operation
* Moves occur when the rvalue is discarded ( i.e. initializing or assigning from an unnamed value )
* Is used when the object has pointers and such

>>	Compilers already optimize many cases that formally require a move-construction call in what is known as _Return Value Optimization_. Most notably, when the value returned by a function is used to initialize an object. In these cases, the _move constructor_ may actually never get called.  
  
>>Note that even though _rvalue references_ can be used for the type of any function parameter, it is seldom useful for uses other than the _move constructor_. Rvalue references are tricky, and unnecessary uses may be the source of errors quite difficult to track.

*An implicit default  can be used
	*	when no destructor, nor copy or move constructor or assignment is defined
	*	it moves all members

#### Default/ implicitely defined
* only under certain circumstances
* Can be removed or made explicit with
	* `function_declaration = default;` 
	* or `function_declaration = delete;` to prevent the creation of the implicit default

>Note that, the keyword `default` does not define a member function equal to the _default constructor_ (i.e., where _default constructor_ means constructor with no parameters), but equal to the constructor that would be implicitly defined if not deleted.  
  
>In general, and for future compatibility, classes that explicitly define one copy/move constructor or one copy/move assignment but not both, are encouraged to specify either `delete` or `default` on the other special member functions they don't explicitly define.

### Friendship and inheritance
#### Friendship
* functions declared with the `friend` keyword have access to protected and private members
	* `friend returnType funcName(args);`
	* the function is not a member of the class
	* mostly used for operator overloads
*friend classes are listed in the class definition like:  `class A{friend class B};`
	* methods of class `B` will have access to private and protected members of `A`;
* Friendships are not transitive or reciprocal. If that behavior is desired, then both classes will need a friendship
#### Inheritance
* Inheritance is declared like : `class derived : public base {...};`
	* If the access modifier applied to the base class is more restrictive than `public` then more accessible members of the base class will be made more restrictive in the derived class. i.e.  `public` can become `private` or `protected` but not the other way around
* `protected` access means that the class and derived class have access
* Some things are not inherited:
	* constructors and destructors
	* assignment operator members
	* friendships
	* private members
* Constructors and destructors of the base class are called automatically by the constructors and destructors of the derived class
	* The default constructor is used unless specified otherwise in the derived constructor's definition
		* `DerivedClass(arg1,arg2,...) : BaseClass(arg2...){...}`
#### Multiple Inheritance
* multiple base classes can be declared in a `,` separated list, each with its own access modifier
*  `class Rectangle: public Polygon, protected Output{...};`
* TODO: what if it inherits the same member from two classes
* Constructors: `Rectangle(int a, int b) : Polygon(a,b) {/*constructor for rectangle calls the polygon constructor*/}
* TODO: figure out the syntax for when class or method declarations are separate from their definitions

### Polymorphism
* pointers to a derived object are also valid pointers to the base class
* virtual members - functions with the `virtual` keyword can re-define functions of the parent class
```c++
class Base {
	public:
		void func1();
		virtual void func2();
};
class Derived : public Base{
	public:
		void func1();
		void func2():
};

int main(){
	Derived derived;
	Base *base = &derived;
	
	base->func1();//Base::func1
	base->func2();//Derived::func2
	derived.func1();//Derived::func1
}
```
* non - virtual members can be redefined but only the definition from the base class can be used with objects declared as the base type
* **Polymorphic classes** declare or inherit a virtual function
* **Abstract classes**
	* have **pure virtual** functions `virtual void func()=0`
	* cannot be used to instantiate objects instead use `Base * obj1 = new Derived()`
* 
	 
## Other Language Features
### Type Conversions
#### Basics
* **Standard conversions** can be made implicitly
	* These conversions affect fundamental data types
	* allow conversion between numerical types, to/from bool, and some pointer conversions
* **Promotion** is converting when the exact value can be guaranteed to be produced
	* i.e. small integers to bigger ints
	* Conversion from signed to unsigned values gives a 2's compliment
		* i.e. -1 => maximum allowed value, etc
	* Guaranteed to produce an exact result
* Conversion from /to bool considers `false` to be equivalent to `0` and `nullptr`
* Conversions from float truncate. If the value is outside of  the range for the int, then the result is undefined behavior
* Other numerical conversions are valid but the value is implementation-specific
* Lossy conversions may trigger a warning, but that can be avoided with an explicit cast
* For non-fundamental types:
	* null pointers can be converted to any other pointer type
	* pointers to any type can be converted to `void *`
	* **Pointer upcast** pointers to a derived class can be converted to an accessible and unambiguous base class
		* without modifying its `const` or `volatile` qualification
#### Conversion of classes
```c++
// implicit conversion of classes:
#include <iostream>
using namespace std;
class A {};

class B {
public:
  // conversion from A (constructor):
  B (const A& x) {}
  // conversion from A (assignment):
  B& operator= (const A& x) {return *this;}
  // conversion to A (type-cast operator)
  operator A() {return A();}
};

int main ()
{
  A foo;
  B bar = foo;    // calls constructor
  bar = foo;      // calls assignment
  foo = bar;      // calls type-cast operator
  return 0;
}
```
* C++ allows one implicit conversion per argument in function calls
* Constructors with the `explicit` keyword will not be used for implicit casts
	* They also cannot be used with assignment-like syntax.

#### Explicit casts
```c++
double x = 10.3;
int y = int (x); //functional notation
y = (int) x;//c-like cast notation
```

*`(new_type) expression` or  `new_type (expression)`
	* These casts are are unsafe, and can be applied indiscriminately

* `dynamic_cast <new_type> (expression)`
	* Only works with pointer types
	* requires an optional feature called **Run-Time Type Information (RTTI)**
		* It is disabled by default on many compilers
	* Allows **downcast** ing pointers (from base to derived class
	* If the cast cannot be completed (i.e. the actual object pointed to is not a complete member of the derived class)
		* the `nullptr` is returned
	* It can also perform allowed implicit casts on pointers
* `reinterpret_cast <new_type> (expression)`  
	* converts any pointer type to any other pointer type
	* very not safe
	* can cast pointers to int types large enough to contain it 
		* i.e. `<intptr_t>`
	* behavior is system specific and not portable
* `static_cast <new_type> (expression)`
	* Can perform pointer type casts without type safety
	* Can perform any allowed implicit conversions **and** their reverse
	* when used to convert from `void *` the resulting pointer will be the identical
		* (unsure, but ie) `static_cast<int *>(static_cast<void*>(&x)) == &x`
	* it can convert to **rvalue references**
	* convert `enum class` values to integers and floatis
	* can convert any type to `void` which discards the value
* `const_cast <new_type> (expression)`
	* Changes the const-ness of an object
	* if  the object is modified, the result is undefined behavior
#### `typeid(obj)` and `typeid(type)`
* uses **RTTI** when used on a polymorphic object
* the returntype is `type_info` in `<typeinfo>`
* can be compared to other typids using `==` and `!=`
* `type_info::name()` - returns a string naming the type
	* depends on the compiler and library
* For pointers it returns the type of the pointer
* For objects it returns the typeid of their dynamic type (their most derived complete object)
* for dereferenced null pointers it throws a `bad_typeid` error

### Exceptions
```c++
try {
  // code here
}
catch (int param) { cout << "int exception"; }
catch (char param) { cout << "char exception"; }
catch (...) { cout << "default exception"; }
```
* try-catch blocks can be nested
* `throw` with an argument will throw that variable
* `throw` with no arguments can be used inside of a `catch`-block to re-throw the same error

#### Dynamic and standard exceptions
* Dynamic exception specifications (aka exception specifications) are a deprecated but still supported feature
	* they are declared with `throw(type)` at the end
	* exceptions other than that type will call `std::unexpected` instead of throwing an exception
	* if no type is specified, all exceptions will call `std::unexpected`
* Standard exceptions use a class called `std::exception` found in `<exception> header
	* defines a property `std::exception::what()` that returns a null-terminated string with a short description
* Exceptions thrown by standard library classes (all inherit from `std::exception`
	* `bad_alloc` , `bad_cast` (thrown by `dynamic_cast`) `bad_typeid`, `bad_function_call` (when the function object is empty), `bad_weak_ptr` (thrown by `shared_ptr`)
* `<exception>` also defines two classes for generic custom exceptions
	* `logic_error` and `runtime_error`

### Preprocessor Directives
* they all begin with `#` and end at the eol
	* Ending a line with `\` will include the next line in the directive
* no `;`
#### maro definitions (`#define` , `#undef`)
`#define identifier replacement` will replace all instances of `identifier` with `replacement`
* Macros don't understand c++ ; it they perform simple text replacements
* It can take function parameters
	* i.e. `#define getmax(a,b) a>b?a:b`
	* note that `getmax(n++,k++)` will translate to `n++>k++?n++:k++` meaning that `n` or `k` will be incremented twice
	* and `getmax(somefunc(),otherfunc())` will call both functions, and return the value of the second invocation of the one of them.
	* `#` and `##` are special operators that change how replacement works with parameters
	* `#arg` will be replaced with a string literal containing the macro argument
	* `arg1 ## arg2` will concatenate the arguments, with no space in between

* Defined macros are not affected by block structure
* they remain defined until removed with `#undef`
#### Conditional inclusions `#ifdef` `#ifndef` `#if` `#endif` `#else` `#elif`
* the work as expected
* `#if` blocks all end with `#endif` regardless of `#elif` and `#else` 
* `defined` and `!defined` can be used in the condition of `#if` and `#elif`
#### Other directives
`#line nummber "filename"` - used by compilers to generate error messages
`#error an error message` - will abort compilation with the specified error message
`#include <header>` -- inclu 

## C++ Standard Library
### Input/Output with files

	
 
> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEzNTE1NjQ4MDYsLTg1NTMyNTczOSwtMT
c4NDA5OTg2NCwyMTMzMjE5MjY3LC0yMDY2OTc2MjkyLC0xMDI1
NTU0NDM4LC05NTEwNTM1OTksMTExMDI5MjkwNSwtMTY0ODUzMT
I4MSwtMTMxMTY4ODUxMSwtNzQ5MTMyNjM2LDIwOTY5ODgyMjQs
MTk5NjIyNTQ5NywtMTY0OTI4MTc5NiwtMTg4NDcxNDU1NSwtOT
kxNDA3MjQsMTE1NjUyNzczOSwxNTI5MjE1OTM4LC0xOTUzODQw
MjgwLC01MzQzMDU1MTldfQ==
-->