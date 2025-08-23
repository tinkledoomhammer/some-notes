# The Rust Book part 2
"The Rust Programming Language"
https://rust-book.cs.brown.edu/

# 08 Common Collections

**Dropping a vector drops its elements**

## 08.01 `Vec`tors
* `let v: Vec<i32> = Vec::new();`
* `Vec`s are generics, with the type being stored as the generic parameter
* `let v = vec![1, 2, 3];`

`Vec<T>.push(val)`
: adds `val` to the end of the vector

#### Reading elements
```rust
let v = vec![1,2,3,4,5];
//using [] to index
let third: &i32 = &v[2];
// will panic if the index is out of bounds

// with Vec<T>.get(index) -> Option<&T>
let third: Option<&i32> = v.get(2);
// will not panic, ruturns None if out of bounds
```
**Note:** The borrow checker considers the entire vec to be one item

### Iterating over values in a `vec`
```rust
let v = vec![100,32,57];
for i in &v {
	println!("{i}");
}
let mut v = vec![100,32,57];
for i in &mut v {
	*i +=50;
}
```

### Safely using `iter`ators
More details in [13.2 Processing a series...](https://rust-book.cs.brown.edu/ch13-02-iterators.html)

 `Vec::iter` and `Iterator::next`

```rust
fn main() {
use std::slice::Iter;  
let mut v: Vec<i32>         = vec![1, 2];
let mut iter: Iter<'_, i32> = v.iter();
let n1: &i32                = iter.next().unwrap();
let n2: &i32                = iter.next().unwrap();
let end: Option<&i32>       = iter.next();
}
```
Values can**not** be added or removed while the iterator is alive.
Instead, iterate over a range:
```rust
let mut v: Vec<i32>			= vec![1,2];
let mut iter: Range<usize>	= 0..v.len();
let i1: usize				= iter.next().unwrap();
let n1: &i32				= &v[i1];
```
### Vecs of Enums
```rust
enum SpreadsheetCell {
    Int(i32),
    Float(f64),
    Text(String),
}

let row = vec![
    SpreadsheetCell::Int(3),
    SpreadsheetCell::Text(String::from("blue")),
    SpreadsheetCell::Float(10.12),
];
```


**Dropping a vector drops its elements**



## 08.02 UTF-8  encoded `String`s

string slices
: `str` type, usually used as `&str`
: The native type for string literals
: the only string type in the core language

### Creating a new String
```rust
let mut s = String::new();
let data: str = "initial contents";
let s2 = data.to_string();
let s3 = "initial contents".to_string();
```
**UTF-8 Encoding allows strings in a variety of languages**

### Updating a String
* can grow in size
* the `+` operator or the `format!` macro can concatenate strings

**push_str**
* `s.push_str("bar");` => `"foobar"`
* `fn push_str(&mut self, s: &str) ;`
* takes a `&str` argument and appends it to the end of the string

**push**
* `s.push('a')` -- adds a single char to the end of the string

`+` operator
: `fn add(self, s: &str) -> String {...`
: Modifies/consumes the self, and returns the resultant combination
: `s1 + &s2` where both are `String` coerces `&s2` to `&str`

`format!` macro
: uses `println!` like syntax to make combining strings less cumbersome 
: can avoid many re-allocations
```rust
let s1 = String::from("tic");
let s2 = String::from("tac");
let s3 = String::from("toe");
let s = format!("{s1}-{s2}-{s3}");
//"tic-tac-toe", one allocation
//equivalent to
let s = s1 + "-" + &s2 + "-" + &s3;
//same result but up to 7 reallocations
```

### Indexing into `String`s
* regular indexing is invalid in rust due to the variable-length of UTF-8 chars
* And also indexing cannot be guaranteed to be O(1) (constant time) 

**Internal Representation**
* `String` is a wrapper over a `Vec<u8>`
* The elements of that `Vec` are u8, not chars
* Rust's chars are Unicode scalars

**Bytes vs Scalars vs Grapheme Clusters**
* bytes are bytes
* scalars are Unicode scalar values, which can be characters or diacritics
* Grapheme clusters are one or more scalar values

### Slicing and Iterating over Strings

Slicing
: Technically allowed with ranges
: Indexes are in bytes
: Will cause a panic if it includes part of a char
: `let s =&hello[0..4]`

Iterating
: `.chars()` or `.bytes()`
: The standard library does not provide a way to iterate over grapheme clusters
```rust
for c in "Зд".chars() {
    println!("{c}");
}
for b in  "Зд".bytes() {
 qprintln!("{b}");
}
```
>З
>д
>208
>151
>208
>180


## 08.03 `HashMap`s

### Creating and using `HashMap`s

`HashMap<K, V>`
: stores mapping of `K` type keys to `V` type values
: uses a *hashing function* to determine how keys and values are stored in memory

```rust
use std::collection::HashMap;
let mut scores = HashMap::new();
scores.insert(String::from("Blue"), 10);
scores.insert(String::fom("Yellow"),50);

let team_name = String::from("Blue");
let score = scores.get(&team_name).copied().unwrap_or(0);

for (key, value) in &scores {
	println("{key}: {value}");
}

scores[somekey] // will panic if `somekey` is not present
```

### Ownership and Updating
* Values with the `Copy` trait are copied into the hm
* other values are moved
* References can be added, and will not move the target of the ref
	* but the ref must be valid for at least as long as the hm is valid
	
**Each key can only have one associated value**

```rust
map.insert(key1, val1);
map.insert(key1, val2);
// key1 => val2
// insert will replace a value if the key exists

map.entry(key2).or_insert(val2);
//the entry() method returns an Entry enum
//the Entry.or_isert(val) method
//	will return a mutable referrence to the value, assigning one if none is present
```

```rust
use std::collections::HashMap;
let text = "hello world wonderful world";
let mut map = HashMap::new();
for word in text.split_whitespace(){
	let count = map.entry(word).or_insert(0);
	*count +=1;
}
println!("{map:?}");
//{"world": 2, "hello: 1, "wonderful:: 1}
```

### See Also [Chapter 10](https://rust-book.cs.brown.edu/ch10-02-traits.html) for changing the hash function
* The built in one is fairly slow for security reasons

## 08.04 Ownership quiz 2

# 09 Error Handling
`panic!`
: macro causes the program to exit immediately 

`Result<T,E>`
: type used to represent possible errors

## 09.01 Unrecoverable errors with `panic!`

### Unwinding vs Aborting
* By default panics cause the stack to unwind, deallocating memory as it goes
* alternatively the binary can be compiled to abort
	* `panic = abort` in the appropriate `[profile]` section i.e. `[profile.release]`
	* this will force the os to clean up memory, but is a faster and results in a smaller binary

```rust
fn main() {
	panic!("crash and burn");
}
```

```bash
$ cargo run
   Compiling panic v0.1.0 (file:///projects/panic)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.25s
     Running `target/debug/panic`

thread 'main' panicked at src/main.rs:2:5:
crash and burn
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

* a panic will also occur from various bugs, such as attempting to access an array out of bounds

## 09.02 Reocverable `Error`s with `Result`
```rust
enum Result<T, E> {
	Ok(T),
	Err(E),
}

//example
use std::fs::File;
use std::io::Error;

let file_result: Result<File,Error> = File::Open("text.txt");
```

* `Ok` and `Err` are in scope from the standard prelude and don't require the `Result::` prefix

### Matching errors
```rust
use std::fs::File;
fn main() {
	let file_result = File::open("hello.txt");
	let file = match file_result {
		
	}
}
```

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE5MjQ5MzgzMTcsMTc1NTI0MzU2MCwtMT
UwNjczMjIyNywtMTk3NzM4MDY2NSwtMjAwNTMzNDI2NiwxNTky
NDA4OTY3LC0xMzE0ODgzODAzLC0xMjg0MDUwNDM5LC05OTA0OD
k5OTQsMzI1NzI5Njc3LDEzMzMxNjA1NTEsLTI1OTA3MzAyOSwx
MjQzNjgzNzExXX0=
-->