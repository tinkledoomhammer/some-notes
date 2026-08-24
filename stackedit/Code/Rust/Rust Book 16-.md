

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










> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE5Mjg0OTAzODYsLTIwNzkwMzE5OTIsLT
IwNzkwMzE5OTIsLTEzMDc2MTE3NTIsLTEwMTg3NTE3NjRdfQ==

-->