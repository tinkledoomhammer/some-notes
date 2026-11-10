

# 16 Fearless Concurrency
**The book often uses *concurrent* to mean *concurrent and/or parallel***


## 16.01 Using threads to run code simultaneously

Process
: An OS object that has at least one thread and is managed by the os

Thread
: the feature that runs an independent part of a process simultaneously

Race Conditions
: when threads access data in an inconsistent order

Deadlocks
: when two threads wait for eachother, preventing either from continuing

Rust standard threads
: use the OS api in a *1:1* model.
: each rust thread is one OS thread

Rust supports other threading models, such as the async system in the next chapter

### Creating a new thread with `std::thread::spawn`

```rust
use std::thread;
use std::time::Duration;

fn main() {
    thread::spawn(|| {
        for i in 1..10 {
            println!("hi number {i} from the spawned thread!");
            thread::sleep(Duration::from_millis(1));
        }
    });

    for i in 1..5 {
        println!("hi number {i} from the main thread!");
        thread::sleep(Duration::from_millis(1));
    }
}
```
> hi number 1 from the main thread!
> hi number 1 from the spawned thread!
> hi number 2 from the main thread!
> hi number 2 from the spawned thread!
> hi number 3 from the main thread!
> hi number 3 from the spawned thread!
> hi number 4 from the main thread!
> hi number 4 from the spawned thread!
> hi number 5 from the spawned thread!

`std::thread::spawn(FnOnce) -> JoinHandle`
: `.join().unwrap()` to wait until the thread ends

### Using `move` closures with threads
* Closures default to refs where possible
* This means that when a closure that captures by ref is passed to `thead::spawn()`, the compiler does not know how long the ref will live
* Closure captures are the only way to pass data to `thread::spawn`


## 16.02 Transfer data between threads with message passing
> Don't communicate by sharing memory; instead, share memory by communicating
```rust
use std::sync::mpsc;
use std::thread;

fn main() {
    let (tx, rx) = mpsc::channel();

    thread::spawn(move || {
        let val = String::from("hi");
        tx.send(val).unwrap();
    });

    let received = rx.recv().unwrap();
    println!("Got: {received}");
}
```

`try_recv()` method returns immediately 

`send()` transfers ownership of its arguments

**Channels can only transmit one type**

**Transmitters can be cloned**

## 16.03 Shared-State Concurrency

`std::sync::Mutex`
: mutual exclusion
: allows one thread to access some data at a given time
: 1. Must attempt to acquire the lock before opening
: 2. must unlock the data after use so that other threads can acquire the lock
: `.lock()` returns `LockResult<MutexGuard, E>`

`MutexGuard<T>`
: imlements `Deref target = T` 
: implements `Drop` to release the lock automatically at end of scope

```rust
use std::sync::{Arc, Mutex};
use std::thread;

fn main() {
    let counter = Arc::new(Mutex::new(0));
    let mut handles = vec![];

    for _ in 0..10 {
        let counter = Arc::clone(&counter);
        let handle = thread::spawn(move || {
            let mut num = counter.lock().unwrap();

            *num += 1;
        });
        handles.push(handle);
    }

    for handle in handles {
        handle.join().unwrap();
    }

    println!("Result: {}", *counter.lock().unwrap());
}
```

[`std::sync::atomic`  module of the standard library](https://doc.rust-lang.org/std/sync/atomic/index.html)
: provides thread-safe access to several primitive types

**`Arc<&T>`** is not allowed. `T` must be owned


## 16.04 Extensible concurrency with `Send` and `Sync`

`Send` and `Sync` traits in `std::marker`

`std::marker::Send`
: A marker trait that indicates the type can be transferred between threads
: Automatically applied to most types
: * A notable exception is `Rc<T>`
: Any type composed entirely of `Send` types is also `Send`

`std::marker::Sync`
: an auto marker trait that indicates that a type is safe to be referenced from multiple threads.
: `T` is `Sync` if `&T` is `Send`
: all primitive types are `Sync` 
: any type composed entirely of `Sync` items is also `Sync`

Some examples
* `Rc<T>` neither `Send` nor `Sync`
* `RefCell<T>` is never `Sync` but is `Send` if `T` is `Send`
* `Mutex<T>` is `Send` and `Sync`
* `MutexGuard<'a,T>` (returned by `Mutex::lock`) 
	* Is never `Send`
	* Is `Sync` if `T: Sync`

### Implementing `Send` and `Sync` manually is unsafe
Manually implementing these Traits is possible but requires `unsafe` code.
> the important information is that building new concurrent types not made up of `Send` and `Sync` parts requires careful thought to uphold the safety guarantees. [“The Rustonomicon”](https://doc.rust-lang.org/nomicon/index.html) has more information about these guarantees and how to uphold them.


# 17 Fundamentals of Async: `Async`, `Await`, `Future`s and `Stream`s

## 17.1 Futures and Async Syntax

future
: a value that may not be ready now but will become ready at some point in the future
: similar to a *task* or *promise*
: in rust, they are  types that implement `Future`

`async` keyword
: applied to blocks and functions that specify that they can be interrupted and resumed

`await` keyword
: used within an `async` block to wait for a future to become ready

polling
: the process of checking to see if a future to see if its value is available

**Async/await code in rust** is compiled into equivalent code that uses the `Future` trait

###  Our first Async program
```bash
$ cargo new hello-async
$ cd hello-async
$ cargo add trpl
```

`trpl` crate
: (short for "the rust programming language")
: re-exports all the types, traits, and functions that will be needed
: particularly from `futures` and `tokio` crates
: renames some items to better suit this chapter of this book

`futures` crate
: the official home for experimental async code
: where the `Future` trait was originally designed 

### Defining the `page_title` function

```rust
use trpl::Html;
async fn page_title(url: &str) -> Option<String> {
	let response = trpl::get(url).await;
	let response_text = response.text().await;
	Html::parse(&response_text)
		.select_first("title")
		.map(|title| title.inner_html())
}
```
**Rust futures are *lazy* **, so we must `.await` for them to do anything.
Rust will give a compiler warning if a future is not used
* this is different than `thread::spawn` which begins execution immediately

`.await`
: is a *postfix* keyword to make it easier to chain
: i.e. `let response_text = trpl::get(url).await.text().await;`

> When rust sees a **block** marked with the `async` keyword, it compiles it into a unique, anonymous data type that implements the `Future` trait. When rust sees a **function** marked with `async`, it compiles it into a non-async function whose body is an async block. An async function's return type is the type of the anonymous data type the compiler creates for that async block.

```rust
//Roughly equivalent to the above code
use std::future::Future;
use trpl::Html;
fn page_title(url: &str) -> impl Future<Output = Option<String>> {
	async move {
		let text = trpl:get
		//... same as above
	}
}
```
### Executing an Async function with a runtime

** Main cannot be async **

runtime
: a rust crate that manages the details of executing asynchronous code
: can be *initialized* by `main`

In this example, `trpl::block_on` is used
```rust
fn main() {
	let args: Vec<String> = std::env::args().collect();
	trpl::block_on(async {
		let url = &args[1];
		match page_title(url).await {
			Some(title) => println!("The title ffor {url} was {title}"),
			None => println!("{url} had no title"),
		}
	})
}
```

await point
: every place where code uses the `await` keyword
: represents a place where control is handed back to the runtime
: rust keeps track of the state for each async block

If the state machine was visible it would look like
```rust
enum PageTitleFuture<'a> {
	Initial {url: &'a str },
	GetAwaitPoint {url: &'a str},
	TextAwaitPoint {response: trpl::Response },
}
```
* some runtimes provide macros to allow `async fn main() {...}` by rewriting it to a normal main that launches the runtime

### Racing two URLs concurrently
```rust
use trpl::{Either, Html};
fn main() {
	let args: Vec<String> = std::env::args().collect();
	trpl::block_on(async {
		let title_fut_1 = page_title(&args[1]); //lazy
		let title_fut_2 = page_title(&args[2]);
		let (url, maybe_title) = 
			match trpl::select(title_fut_1, title_fut_2).await {
				Either::Left(left) => left,
				Either::Right(right) => right,
			};
		println!("{url} returned first");
		match maybe_title {
			Some(title) => println!("Its page title was: '{title}'"),
			None => println!("It had no title."),
		}
	})
}

async fn page_title(url: &str) -> (&str, Option<String>) {
	let response_text = trpl::get(url).await.text().await;
	let title = Html::parse(&response_text)
		.select_first("title")
		.map(|title| title.inner_html());
	(url, title)
}
```

`trpl::select()`
: built on a more general `futures::select()`
: returns a value to indicate which future finishes first
: its return type is an `enum`
### exercises
* to wait on a future in non-async code, pass it to a runtime

```rust
//async version
async fn calculate(nums: &[i32]) -> i32;
//desugars to
fn calculate<'a>(nums: &'a [i32]) -> impl Future <Output = i32> + 'a;
```
* the future captures any lifetimes in the function's arguments
* The above specifies that the slice must live as long as the future that captures it.

## 17.2 Applying Concurrency with Async
### Creating a new task
```rust
use std::time::Duration;
fn main() {
	trpl::block_on(async {
		trpl::spawn_task(async {
			for i in 1..10 {
				println!("hi number {i} from the first task!");
				trpl::sleep(Duration::from_millis(500)).await;
			}
		}); //spawn_task returns a join handle
		for i in 1..5 {
			println!("hi number {i} from the second task!");
			trpl::sleep(Duration::from_millis(500)).await;
		}		
	});//exits when main ends, regardless of spawned tasks
}
```
The wrapper will be used (but omitted) throughout the rest of the chapter.
```rust
fn main() {
	trpl::block_on(async {
		// ...
	}
}
```

The above example terminates early. To keep it alive 
```rust
let handle = trpl::spawn_task(async {
	for i in 1 .. 10 //...
});
for i in 1..5 //...
handle.await.unwrap(); // wait until the spawned task ends
```

`trpl::join()`
: takes futures, returns a future

`trpl::join!()`
: joins an arbitrary number of futures
```rust
let fut1 = async {
	for i in 1..10 {
		println!("hi number {i} from the first task");
		trpl::sleep(Duration::from_millis(500)).await;
	}
};
let fut2 = async {
	for i in 1..5 {
		println!("hi number {i} from the second task!");
		trpl::sleep(Duration::from_millis(500)).await;
	}
};
trpl::join(fut1, fut2).await;
```
: the result alternates between the first and second task, printing everything in order
* `trpl::join` is **fair** meaning that checks each future equally often, alternating between them

### Sending data between tasks using message passing
```rust
let (tx, mut rx) = trpl::channel();
let val = String::from("hi!");
tx.send(val).unwrap();
let recieved = rx.recv().await.unwrap();
println!("received: '{received}'");
```

tx
: non-blocking, returns immediately
: clonable. the channel is mpsc

rx
: `.recv()` returns a `Future<Option>`
: will resolve when a message is received or the tx closes the channel


### Code within one Async bloc executes linearly
```rust
let (tx, mut rx) = trpl::channel();
let tx_fut = async move{
	//without the `move` keyword, the program will never exit
	//with the move keyword, `tx` gets dropped at the end of the
	//  async block
	let vals = vec![
		//...
	];
	for val in vals {
		tx.send(val).unwrap();
		trpl::sleep(Duration::from_milis(100).await;
	}
};
let rx_fut = async {
	while let Some(value) = rx.recv().await {
		println!("recieved '{value}'");
	}
};
trpl::join(tx_fut, rx_fut).await;
```



## 17.3 Working with any number of futures

### Yielding Control to the Runtime

Rust **only** pauses async blocks at `await` points
When one task prevents others from running for a long time it is called **starving** other futures.

To simulate a long-running task:
```rust
fn slow(name: &str, ms: u64){
	thread::sleep(Duration::from_millis(ms));
	//thread::sleep instead of trpl::sleep to prevent awaiting
	println!("'{name}' ran for {ms} ms");
}
```

To cause a thread to yield:
`trpl::sleep(Duration::from_millis(1)).await;`
or
`trpl::yield_now().await;`

### Building your own Async abstractions
Creating a `timeout` function
```rust
//usage
let slow = async {
	trpl::sleep(Duration::from_secs(5)).await;
	"Finally finished"
};
match timeout(slow, Duration::from_secs(2)).await {
	Ok(message) => println!("Succeeded with '{message}'")
	Err(duration) => {
		println!("Failed after {} seconds", duration.as_secs())
	}
}
```
* needs to be an async function (so that it can be `await`ed
* First param is a future to run, will be generic
* Param 2 is the duration to wait
* Should return a `Result`
	* `Ok` with the value produced by the first param if it finishes first
	* `Err` with the duration of the timeout if the task does not finish first

The signature
```rust
async fn timeout<F: Future>(f_to_tr: F, time: Duration)-> Result<F::Output, Duration>
```
* use `trpl::sleep` to create the timer
* use `trpl::select` to set up the race

```rust
use trpl::Either
// --snip--
async fn timeout<F: Future>(fut: F, time: Duration)->Result<F::Output, Duration>{
	match trpl::select(fut, trpl::sleep(time)).await {
		Either::Left(output) => Ok(output),
		EIther::Right(_) => Err(max_time),
	}
}
```

## 17.04 Streams: Futures in Sequence

`trpl::stream`
: an async iterator
: `let mut stream = trpl::stream_from_iter(an_iterator);`

`trpl::StreamExt`
: an extension trait for stream objects
: must `use trpl::StreamExt` to iterate over streams
: defines the `next` method among others

## 17.05 A closer look at the traits of async

### Future
```rust
use std::pin::Pin;
use std::task::{Context, Poll};
pub trait future {
	type Output;
	fn poll(self: Pin<&mut Self>, cx: &mut Context<'_>) -> Poll<Self::Output>;
}
pub enum Poll<T> {
	Ready(T),
	Pending,
}
```
* Internally, futures are enums with a variant for each (await delimited) block0
* note the type specifier for `self` in `poll`



### The `Pin` type and the `Unpin` trait

Futures are often self-referential
The different blocks (separated by `.await`) take ownership of the block's local variables
So, for example, a ref created before an `.await` that is dereferenced after that `.await` must point to the same address, even if the runtime moves the future to a different memory address.

`Pin` type
: guarantees that values won't be moved in memory

`Box<T>` implements `Future` only if the `T` it wraps is a future that implements `Unpin` 

Type annotations for `self` must be
: the type on which the method is implemented
: a reference or smart pointer to that type
: or a `Pin` wrapping a ref to that type

`Pin` can wrap types that implement `Deref` or `DerefMut`
: It doesn't have its own behavior (like `Rc` and `Arc`)
: only tells the compiler to enforce additional constraints on pointer usage

`Unpin`
: auto marker trait applied to most types
: will be implemented automatically for types that can be moved safely while pinned
: basically means that they provably cannot reference themselves

`!Unpin`
: marker trait applied to types that can reference themselves

`pin!({async block})`
: macro that pins a future
: required to, for example, put futures into a Vec or whatever
```rust
let tx1_fut = pin!(async move { 
	//... -- snip00
});
let futures: Vec<Pin<&mut dyn Future<Output = () >>> = vec![tx1_fut, ]
```
> Directly awaiting a future with `await` pins the future implicitly. That's why we don't need to use `pin!` everywhere we want to await futures.**

### The `Stream` trait (crate trpl)
recall `Iterator` has `next() -> Option<Self::Item>`
recall `Future` has `poll() -> Poll<Self::Output>`
```rust
use std::pin::Pin;
use std::task::{Context, Poll};
trait Stream {
	type Item;
	fn poll_next(slef: Pin<&mut Self>, cx: &mut Context<'_>)
		-> Poll<Option<Self::Item>>;
}
```
* Note that it's `Poll<Option>`
```rust
trait StreamExt: Stream {
	async fn next(&mut self) -> Option<Self::Item>
	where Self: Unpin;
	//other methods...
}
```

## 17.06 Putting it all together: Futures, Tasks, and Threads
```rust
use std::{thread, time::Duration};
fn main() {
	let (tx, mut rx) = trpl::channel();
	thread::spawn(move || {
		for i in 1..11 {
			tx.send(i).unwrap();
			thread::sleep(Duration::from_secs(1));
		}
	});
	trpl::block_on(async {
		while let Some(Message) = rx.recv().await {
			println!("{message}");
		}
	});
}
```
The apis for tasks and threads are similar.
Threads are best for
* Cpu-bound workloads (parallelizable)
* i.e. processing a lot of data
Async is best for 
* I/O-bound workloads (concurrent)
* i.e. data that is sporadic, from different sources, etc

# 18 Object-Oriented Programming features
## 18.01 Characteristics of OO Languages

### Objects contain data and behavior

### Encapsulation hides implementation Details

* keep struct members private if they need to be maintained
* use `pub` methods to selectively expose the desired behavior

### Inheritance for Polymorphism and Code Reuse
Rust does not support inheritance
* for polymorphism, generics and trait bounds
* for code reuse, default implementations on traits
 
## 18.02 Using Trait Objects to Abstract over Shared Behavior

Unlike enums, new types can be added to a trait.
i.e. a `Vec<SomeEnum>` must have all of its variants in the same crate, while `Vec<Box<dyn SomeTrait>>` can include types defined by the user

### Defining a Trait for Common Behavior
* trait objects i.e. `&dyn SomeType`, `Box<dyn SomeType>`, `Vec<dyn SomeType>` have 
	* can be stored in a heterogenous collection
	* can only be used through the specified trait(s?)
	* can **not** be cast to the original type
	* Except for the `Any` trait

### Implementing the Trait

### Using the Trait

### Trait Objects and Type Inference

The compiler will need additional info to establish types
```rust
//falis
let components = vec![Box::new(SelectBox{/*..*/}),Box::new(Button{/*..*/}),];
//works using as
let components = vec![Box::new(SeslectBox{}) as Box<dyn Draw>, /* ... */];
//works using type annotation in the let-binding
let components: Vec<Box<dyn Draw>> = vec![ /* ... */
```

### Performing Dynamic Dispatch
* Trait object methods cannot be statically dispatched
* this will prevent some compiler optimizations
* there are limits on which methods can be called dynamically that are described [in the reference](https://doc.rust-lang.org/reference/items/traits.html#dyn-compatibility)

## 18.03 **Incomplete**
TODO: Review this and the next section

# 19 Patterns and Matching
 A pattern consists of some combination of:
 * Literals
 * Destructured arrays, enums, structs, or tuples
 * Variables
 * Wildcards
 * Placeholders

## All the Places Patterns can be used
### `match` arms
```rust
match VALUE	 {
	PATTERN => EXPRESSION,
	PATTERN => EXPRESSION,
}
```
* They need to be exhaustive (all possible values in the `match` expression must be accounted for
* the `_` pattern will match anything, but never bind a variable. Using it as  the last pattern will function as a  catchall

### `let` statements
```rust
let PATTERN = EXPRESSION
```

* While the pattern is usually just a variable name, it could also be a destructured tuple
* ` let (x,y,z) = (1,2,3);`
* if the entire expression does not match the pattern, a compiler error will occur
* creates a new scope

### `if let` Expressions
Also `else if` and `else if let`
```rust
let fav_color: Option<&str> =None;
let is_tuesday = false;
let age: Result<u8, _> = "34".parse();
if let Some(color)=fav_color  {
	println!("Using {color}");
} else if is_tuesday {
	println!("Tuesday is green day");
} else if let Ok(age) = age {
	if age > 30 { // <- can't be part of the above conditional
			// because the scope of the inner age starts here
		println!("Using purple");
	} else {
		println!("Using orange");
	}
} else {
	println("blue");
}
```
* The compiler does **not** check for exhaustiveness 
### `while let` conditional loops
* runs for as long as the pattern continues to match
```rust
while let Ok(value) = rx.recv() {
		println!("{value}");
}
// ^^-will loop until an Err is recv'ed-^^
```
* Creates a new scope

### `for` loops
```rust
let v = vec!['a','b','c'];
for (index,value) in v.iter().enumerate() {
	println!("{value} at index {index}");
}
```
### Function Parameters
```rust
fn print_coordinates(&(x,y): &(i32,i32)) {
	println!("Location: ({x}, {y})");
}
fn main(){
	let point = (3,5);
	print_coordinates(&point);
}
```
## 19.02 Refutability: Whether a Pattern Might Fail to Match

A pattern if **refutable** if
: it can fail to match
: is prohibited for function parameters, `let` statements, and `for` loops

`if let` and `let while` can accept refutable patterns

What the compiler says when a refutable pattern is used where not allowed
```
$ cargo run
   Compiling patterns v0.1.0 (file:///projects/patterns)
error[E0005]: refutable pattern in local binding
 --> src/main.rs:3:9
  |
3 |     let Some(x) = some_option_value;
  |         ^^^^^^^ pattern `None` not covered
  |
  = note: `let` bindings require an "irrefutable pattern", like a `struct` or an `enum` with only one variant
  = note: for more information, visit https://doc.rust-lang.org/book/ch19-02-refutability.html
  = note: the matched value is of type `Option<i32>`
help: you might want to use `let else` to handle the variant that isn't matched
  |
3 |     let Some(x) = some_option_value else { todo!() };
  |                                     ++++++++++++++++

For more information about this error, try `rustc --explain E0005`.
error: could not compile `patterns` (bin "patterns") due to 1 previous error
```

What the compiler says when an irrefutable pattern is used inappropriately

```
$ cargo run
   Compiling patterns v0.1.0 (file:///projects/patterns)
warning: irrefutable `let...else` pattern
 --> src/main.rs:2:5
  |
2 |     let x = 5 else {
  |     ^^^^^^^^^
  |
  = note: this pattern will always match, so the `else` clause is useless
  = help: consider removing the `else` clause
  = note: `#[warn(irrefutable_let_patterns)]` on by default

warning: `patterns` (bin "patterns") generated 1 warning
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.39s
     Running `target/debug/patterns`
```

* Match arms must use refutable patterns except for the last arm

## 19.03 Pattern Synntax
### Matching Literals
```rust
match x {
	1 => println!("one"),
	2 => //.. snip
	_ => println!("anything else"),
}
```
### Matching named variables
**Named variables are irrefutable patterns that match any value**
* When the pattern structure creates a new scope, then variable names will be a named-variable pattern rather which will shadow any variables with the same name in the outer scope
* The section "Adding Conditionals with Match Guards" allows the use of variables in match arms

### Matching multiple patterns
the `|` pipe char is used to separate patterns to match disjunctively
```rust
match x {
	1 | 2 => println!("one or two"),
	3 => println!("three"),
}
```
### Matching Ranges of values with `..=`
```rust
match x {
	1..=5 => println!("one through 5"),
	_ => println!("something else"),
}
let x = 'c';
match x {
    'a'..='j' => println!("early ASCII letter"),
    'k'..='z' => println!("late ASCII letter"),
    _ => println!("something else"),
}
```
### Destructuring to Break Apart Values

**Structs**
```rust
struct Point { x: i32, y: i32 }
fn main(){
	let p=Point{x:0, y:7};
	// verbose destructuring with common names
	let Point{x:a, y:b} = p;
	assert_eq!(0,a);
	assert_eq!(7,b);
	//more concise, uses the field nams as var names
	let Point{x,y} = p;
	assert_eq!(0,x);
	assert_eq!(7,y);
	//can mix varname with literal values
	match p {
			Point {x, y:0} => println!("On y axis at x={x}"),
			Point {x:0, y} => println!("On x axis at {y}"),
			Point {x,y} => println!("On neither axis {x},{y}"),
	}
}
```
** Enums**
```rust
enum Message { 
	Quit,
	Move{x:i32,y:i32},
	Write(String),
	Color(i32,i32,i32),
}
let msg = Message::ChangeColor(0,155,255);
match msg {
	Messagge::Quit => /*...*/,
	Message::Move {x,y} => /* binds x and y ... */,
	Message::Write(text) => /* binds text */,
	Message::ChangeColor(r,g,b) => /* binds r,g, and b */,
}
// also structs and enums can be nested
enum Inner{
	Ver1(i32,i32),
	Ver2{a: i32, b: ie2),
}
enum Outer{
	Var1(Inner),
	Ver2,
}
let m = Outer::Var1(Ver1(1,2));
match m {
	Outer::Ver2 => /* no vars bound*/ ,
	Outer::Ver1(a,b) => /* a and b bound */,
	Outer::Ver1{a,b} => /* also binds a and b*/,
}
```
** Structs and tuples can be mixed and matched **
```rust
let ((feet, inches), Point {x,y}) = ((3,10), Point{x:3, y:10});
```
### Ignoring Values in a pattern

`_` matches and discards a value
: can be used in any pattern
: can be nested
: prefix var names to suppress unused variable warnings
** can be used in nested patterns to ignore part of a value **
```rust
let mut val1 = Some(5);
let val2 = Some(10);
match (val1,val2){
	(Some(_), Some(_)) =>/* both vals are 'Some' */,
	(Some(_),None) => val1=None,
	(None,Some(v)) => val1=Some(v),
}
let vals = (1,2,3,4);
match vals {
	(first, _, 3, _, fifth) => (first,fifth),
	_ => (1,2),/* didn't have 3 in position 2 */
}
```
`..` matches and discards remaining parts
: binds nothing
: ignores as many as needed to make a match
: i.e. `(first, ..  ,last)=>` or `(first, ... )`
: must be used unambiguously `(.. , second, ..)` will generate an error
: can only be used once per tuple

### Adding conditionals with Match Guards
Match Guard
: an additional `if` condition after a pattern in a `match` arm
: can only be used with `match` not with `if let` or `while let` expressions
: used to express more complex ideas than patterns alone
: is lower precidence than the pattern itself
```rust
match x {
	4 | 5 | 6 if y => "y is true and it was 4,5,or 6",
	_ => "y was false or it was some other number",
}
```
### Using `@` Bindings

`@`
: tests and binds in the same pattern
```rust
match m{
	M:Hello{id: id@3..=7} => prinln!("{id} in range"),
	M:Hello{id} => println("{id} not in range"),
}
```

# 20 Advanced Features
## 20.01 Unsafe Rust
* Exists because static analysis is conservative
* And because underlying hardware is inherently unsafe
`unsafe` keyword starts an unnsafe block
* unsafe blocks can
1. Dereference a raw pointer
1. Call an unsafe function or method
1. Access or modify a mutable static variable
1. Implement an unsafe trait
1. Access fields of `union`s

Tips
: keep `unsafe` blocks small
: enclose it within a safe abstraction and a safe API

### Dereferencing a Raw Pointer
`* const T`
: an immutable raw pointer to `T`

`* mut T`
: a mutable raw pointer to `T`

Raw pointers are different from smart pointers
* are allowed to ignore borrowing rules by
	* having both mut and immut pointers 
	* or multiple mut pointers
	* to the same location
* aren't guaranteed to point to valid memory
* are allowed to be null
* don't implement automatic cleanup
* BUT they can be created in safe code (just not dereferenced)
```rust
let mut num = 5;
let r1 = &raw const num; // borrow operators
let r2 = &raw mut num;// ref's can't do this
let address = 0x012345usize;
let r = address as *const i32; //cast with `as`
unsafe { // required to deref the pointers
	println!("r1 is : {}", *r1);
	println!("r2 is : {}", *r1);
}
```
### Calling an Unsafe Function or Method
* A function can be safe and contain an *unsafe* block
* an unsafe function indicates that it has requirements
	* that the compiler can't guarantee
	* so it is on the caller to check the documentation
* can only be called from an unsafe block
* the body of an `unsafe` function is not assumed to be an `unsafe` block
	* because the entire body might not need to be unsafe

#### Example: `split_at_mut` function
* takes a mut slice and returns it as two mut slices
* The borrow checker doesn't check different parts of a slice independently
* So to generate two `&mut`s from the same source, unsafe is required

```rust
use std::slice;
//usage 
let mut v = vec![1,2,3,4,5,6];
let r = &mut v[..];
let (a,b) = r.split_at_mut(3);
assert_eq!(a, &mut[1,2,3]);
assert_eq!(b, &mut[4,5,6]);
//implementation
fn split_at_mut(values: &mut[i32], mid: usize) 
	-> (&mut[i32],&mut[i32]) {
	let len = values.len();
	let ptr = values.as_mut_ptr();
	assert!(mid<=len);
	unsafe{ (
		slice::from_raw_parts_mut(ptr, mid),
		slice::from_raw_parts_mut(ptr.add(mid), len - mid),
	) }
}
```

### Using `extern` functions 

Foreign Function Interface (FFI)
: a way for programming languages to define functions and enable a different programming language to call those functions

**Calling extern functions**
```rust
//wraps the `abs` function from the c standard library
unsafe extern "C" {
	fn abs(input: i32) -> i32;
}
//can only be called in an unsafe block
let n= unsafe{abs(-3)};
//n==3
```


#### Calling Rust functions from other languages
```rust
#[unsafe(no_mangle)]
pub extern "C" fn call_from_c() {
		println!("Just called a rust function from C!");
}
```
The `unsafe` is used only in the attribute, not the `extern`



### Acessing or modifying a static mut var
Static variables 
: have a fixed memory address
: they can only store `'static` references
: can be accessed saffely if they are immutable
: `static mut` can only be safely accessed from one thread
: mut access statics requires `unsafe`

```rust
static mut COUNTER: u32 = 0;
/// SAFETY: Calling this from more than a single thread at a time is undefined
/// behavior, so you *must* guarantee you only call it from a single thread at
/// a time.
unsafe fn add_to_count(inc: u32) {
    unsafe {
        COUNTER += inc;
    }
}
fn main() {
    unsafe {
        // SAFETY: This is only called from a single thread in `main`.
        add_to_count(3);
        println!("COUNTER: {}", *(&raw const COUNTER));
    }
}
```


### Implementing an unsafe trait
`unsafe` trait
: has at least one method that has some  invariant that the compiler can't verify
: declared by placing the `unsafe` keyword before the `trait` keyword

```rust
unsafe trait Foo { /* methods */ }
unsafe impl Foo { /* method implementations */ }
```
* Manually implementing `Send` and `Sync` requres `unsafe`
* For example a struct that contains a raw pointer

### Accessing fields of a union
union
: similar to a struct
: primarily used to interface with C unions

Described in [the Rust Reference](https://doc.rust-lang.org/reference/items/unions.html).


### Using Miri to check unsafe code

>Miri [is] an official Rust tool for detecting undefined behavior. Whereas the borrow checker is a _static_ tool that works at compile time, Miri is a _dynamic_ tool that works at runtime. It checks your code by running your program, or its test suite, and detecting when you violate the rules it understands about how Rust should work.

Miri requires a nightly build of rust
Installed with `rustup +nnightly component add miri`
run with `cargo +nightly miri run` or `cargo +nightly miri test`

An example of its output:
`error: Undefined Behavior: pointer not dereferenceable: pointer must be dereferenceable for 40000 bytes, but got 0x1234[noalloc] which is a dangling pointer (it has no provenance)`

* Miri can only catch problems with code that actually gets run
* It does not cover every way a program can be unsound


## 20.02 Advanced Traits

### Defining Traits with Associated Types
```rust
pub trait Iterator {
	type Item;
	fn next(&mut self) -> Option<Self::Item>;
}
impl Iterator for Counter {
	type Item = u32;
	fn next(&mut self) -> Option<Self::Item> {/*...*/}
}
```
Differs from generics in that
* a generic trait can be implemented for the same type multiple times
* with differen generic params for each impl
With Associated types, type annotations are not needed
Implementations **must** specify the associated type

### Using Default Generic Parameters and Operator Overloading

`<PlaceholderType=ConcreteType>`
: syntax allows omitting type specifiers in the implementation when the default is desired
Uses:
1. To extend a type without breaking existing code
1. To allow customization in specific cases most users won't need
	* Operator overloading does the second 
**Operator Overloading**
Operators with corresponding traits in `std::ops` can be overloaded by implementing the trait
```rust
use std::ops::Add;
#[derive(Debug, Copy, Clone, PartialEq)]
struct Point{
	x: i32,
	y: i32,
}
impl Add for Point {
	type Output = Point;
	fn add(self, other: Point) -> Point {
		Point{x: self.x+ other.x, y: self.y + other.y}
	}
}
//The std::ops::Add trait:
trait Add<Rhs=Self>{
	// Rhs = "right hand side"
	type Output;
	fn add(self, rhs: Rhs) -> Self::Output;
}
```

### Disambiguating Between Identically Named Methods

**When a type impl's more than one trait with the same method**
use `Trait::method(receiver)` or `Triat::associated_function()`
```rust
trait Pilot{ fn fly(&self); }
trait Wizard{ fn fly(&self);}
struct Human;
impl Pilot for Human{
	fn fly(&self) { println!("This is your captain.");}
}
impl Wizard for Human{
	fn fly(&self) { println!("Up!");}
}
impl Human{
	fn fly(&self) { println!("*waving arms furiously*");}
}
fn main(){
	let person = Human;
	Pilot::fly(&person);//captain
	Wizard::fly(&person);//up
	person.fly(); // waving arms
}
```
** The fn on the concrete type impl takes priority **

when `trait::method` is ambiguous
: because more than one type implements the trait, and it doesn't take a `self`
: `<Type as Trait>::fn_name()`



### Using Supertraits
Supertrait
: A trait upponwhich another trait depends
: declared `trait SubbTrait: SuperTrait { /* ... */ }`
: Types implementing the supertrait must also implement the subtrait


### Implementing External Traits with NewTypes Pattern

Newtype pattern
: Thin wrappers that allow bypassing the orphan rule
: i.e. `struct Wrapper(WrappedType);`
: allows implementin

## 20.03 Advanced Types

## 20.04 Advanced Functions and Closures

## 20.05 Macros






.
> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE0MzU1OTIyMDksLTgyOTQ5NDg4MiwxMz
Q4OTc0OTI4LC0xNTUyNTM1NzY3LDIxMjM4NzIyOTIsLTU2NTQy
ODQ0Myw5MzIxMjk1MjQsMTUzMTEyMDM4OCwyNTc5NTk2NzEsMT
MxMjEyODU1NSw1MzEyNjgwNTMsNTMxMjY4MDUzLDE5MDg4Mzgy
ODEsMTk4MTcwNjQ2OSw4ODc5MzM1NDAsLTYwMDAyOTY0NSwxNz
cwMjYzNTMwLC0xMzE4ODgwMzkyLDIwMTk5MDI3OSwtMTA0Nzkw
MzI1MF19
-->