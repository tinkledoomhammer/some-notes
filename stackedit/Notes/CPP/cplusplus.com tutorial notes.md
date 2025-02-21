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
Character Types | char | exactly one byte, at least 8 bits
  || `char16_t` | not smaller than char, at least 16 bits
  || `char32_t` | not smaller than `char16_t`. At least 32 bits
  | | `wchar_t` | can represent the largest supported char set 
Integer types (signed) | `signed char`| same size as char
 | | `signed short int`
> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTk5MDExODQ0OV19
-->