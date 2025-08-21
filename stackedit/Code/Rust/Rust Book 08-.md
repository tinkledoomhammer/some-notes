# The Rust Book part 2
"The Rust Programming Language"
https://rust-book.cs.brown.edu/

## 08 Common Collections

**Dropping a vector drops its elements**

### 08.01 `Vec`tors
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

#### Iterating over values in a `vec`
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

#### Safely using `iter`ators
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
#### Vecs of Enums
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



### 08.02 UTF-8  encoded `String`s

string slices
: `str` type, usually used as `&str`
: The native type for string literals
: the only string type in the core language

#### Creating a new String
```rust
let mut s = String::new();
let data: str = "initial contents";
let s2 = data.to_string();
let s3 = "initial contents".to_string();
```
**UTF-8 Encoding allows strings in a variety of languages**

#### Updating a String
* can grow in size
* the `+` operator or the `format!` macro can concatenate strings

**push_str**
* `s.push_str("bar");` => `"foobar"`
* takes a `&str` argument and appends it to the end of the string
**push**`
* `s.push('a')` -- adds a single char to the end of the string

`+` operator
: `fn add(self, s: &str) -> String {...`
: Modifies/consumes the self, and returns the resultant combination
: `s1 + &s2` where both are `String` coerces `&s2` to `&str`



### 08.03 `HashMap`s



> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTM5Mzg5OTI0NiwtOTkwNDg5OTk0LDMyNT
cyOTY3NywxMzMzMTYwNTUxLC0yNTkwNzMwMjksMTI0MzY4Mzcx
MV19
-->