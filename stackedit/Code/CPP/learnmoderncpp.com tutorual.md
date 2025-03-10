
#  https://learnmoderncpp.com/ tutorial
https://learnmoderncpp.com/

The C++20 version is at https://github.com/cpp-tutor/learnmoderncpp-tutorial/tree/v1.0-C%2B%2B20

There are also articles on the site such as 
[C++ Concurrency 101](https://learnmoderncpp.com/2024/11/09/c-concurrency-101/#more-2785)

## Code Assignments for each chapter
https://learnmoderncpp.com/coding-assignments/


## 00 - About the tutorial

## 01 - String and Character Literals

## 02 - Variables, Scopes, and Namespaces

## 03 - Conditions and Operators

## 04 - Functions

## 05 - Arrays, Pointers, and Loops

## 06 - Enums and Structs

## 07 - Strings, Containers, and Views

## 08 - Files and Formatting

## 09 - Classes, Friends, and Polymorphism

## 10 - Templates, Exceptions, Lambdas, Smart Pointers

# Other Articles on the site

## C++ Concurrency 101

https://learnmoderncpp.com/2024/11/09/c-concurrency-101/
See Also: https://learnmoderncpp.com/2022/06/30/c-coroutines-primer-1/

`std::thread` from `<thread>`

```c++
#include <thread>
void f(){ /* */}
int main() {
	std::thread t(f);
	//The program terminates immediately with an error message
		//The error is because there is still a running thread
		//Exiting the program with an active thread calls 
		//`std::terminate` which should usually be avoided
}
```

* instead calling `t.join();` will cause the main thread wait for `t` to terminate
* `t.detach();`

A Longer example
```c++
`#include <thread>`

`#include <mutex>`

`#include <chrono>`

`#include <vector>`

`#include <iostream>`

`using` `namespace` `std::chrono_literals;`

`class` `ThreadID {`

`std::``thread``::id id;`

`inline` `static` `std::mutex mut;`

`public``:`

`ThreadID(``const` `std::``thread``::id id) : id{ id } {`

`std::lock_guard<std::mutex> lock(mut);`

`std::cerr <<` `"Launched thread: "` `<< id <<` `'\n'``;`

`}`

`~ThreadID() {`

`std::lock_guard<std::mutex> lock(mut);`

`std::cerr <<` `"Finished thread: "` `<< id <<` `'\n'``;`

`}`

`};`

`void` `f() {`

`ThreadID` `log``(std::this_thread::get_id());`

`std::this_thread::sleep_for(3000ms);`

`}`

`int` `main() {`

`std::vector<std::``thread``> threads;`

`unsigned threadCount = (std::``thread``::hardware_concurrency() < 2) ? 1 : std::``thread``::hardware_concurrency();`

`for` `(``int` `t = 0; t != threadCount; ++t) {`

`threads.emplace_back(f);`

`}`

`for` `(``auto``& t : threads) {`

`t.join();`

`}`

`}`
```

## Move Semantics in Modern C++

### Part 1

https://learnmoderncpp.com/2025/01/07/move-semantics-in-modern-c-1/

### Part 2

https://learnmoderncpp.com/2025/02/01/move-semantics-in-modern-c-2/

### Part 3

https://learnmoderncpp.com/2025/03/01/move-semantics-in-modern-c-3/

## Operator Overloading

### Part 1

https://learnmoderncpp.com/2024/11/28/operator-overloading-in-modern-c-1/

## When to use `std::string_view`

https://learnmoderncpp.com/2024/10/29/when-to-use-stdstring_view/

## Making Algorithms faster

https://learnmoderncpp.com/2024/10/12/making-algorithms-faster/

## How could reflection be applied

https://learnmoderncpp.com/2024/09/30/how-could-reflection-be-applied-to-modern-c/

## Pattern Matching

https://learnmoderncpp.com/2024/09/19/pattern-matching-is-coming-to-modern-c/

## Rang-for enhancements in C++23

https://learnmoderncpp.com/2024/09/11/range-for-enhancements-in-c23/

## Understanding Iterator Invalidation

https://learnmoderncpp.com/2024/09/04/understanding-iterator-invalidation/

## Uses of Parser Generators

https://learnmoderncpp.com/2024/08/14/uses-of-parser-generators/

##  Memory Allocation Techniques

https://learnmoderncpp.com/2024/08/05/memory-allocation-techniques-in-modern-c/

## Using Lambda Functions for Delayed Evaluation

https://learnmoderncpp.com/2024/02/24/using-lambda-functions-for-delayed-evaluation/

## Exploring C++23’s flat_map

https://learnmoderncpp.com/2024/01/30/exploring-c23s-flat_map/

## Linear Algebra support in C++26

https://learnmoderncpp.com/2024/01/11/linear-algebra-support-in-c26/

## Replacing the Preprocessor in Modern C++

https://learnmoderncpp.com/2023/12/29/replacing-the-preprocessor-in-modern-c/

## Modern C++ 101: Refactoring Legacy Code

https://learnmoderncpp.com/2023/12/05/modern-c-101-refactoring-legacy-code/

## Selecting Functions at Runtime

https://learnmoderncpp.com/2023/11/22/selecting-functions-at-runtime/

## Signaling error conditions without exceptions

https://learnmoderncpp.com/2023/10/28/signaling-error-conditions-without-exceptions/

## Selecting Functions at Compile Time

https://learnmoderncpp.com/2023/09/26/selecting-functions-at-compile-time/

## Concepts 101 (C++20)

https://learnmoderncpp.com/2023/09/03/concepts-101/


## 






> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE3Mjk0MjMzODFdfQ==
-->