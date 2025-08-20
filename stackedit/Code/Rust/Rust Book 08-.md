# The Rust Book
"The Rust Programming Language"
https://rust-book.cs.brown.edu/

## 08 Common Collections

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


### 08.02 UTF-8  encoded `String`s

### 08.03 `HashMap`s



> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTExOTg2NTU4NzAsMTI0MzY4MzcxMV19
-->