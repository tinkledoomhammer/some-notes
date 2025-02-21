
# ES6 (ES2015)

## Syntax
### `let` `const` and hoisting

`var` 
* Scoped at the function level
* Declarations shadow any variables with the same name in the containing scopes.
* If used in the global scope in a browser, it creates a property of `window`
* Declarations are hoisted to the beginning of the scope, but assignments aren't
```javascipt
  	var a=0;//global
	var b=1;//global
	function asdf(){
		console.log(b);//1
		console.log(a);//undefined
		var a=2;
		console.log(a);//2
	}
```
`let`
* Scoped at the block level
* Most `{..}` create a block.
* Also, files are a block scope, so using `let` and `const` outside of any function will not create a global variable(?)
* Caution: `if(){}` and `else{}` are separate blocks, so declarations inside the `if` block are not accessible in the `else` block.




`const`
* like `let` but can only be assigned once

### Template literals
* Previously called "template strings"
* Enclosed in backticks (``` ` ```) instead of single or double quotes.
* Can contain placeholders using `${expression}`
* The expression will be evaluated and stringified and the result will be inserted into the string.
* They can also contain un-escaped newline characters, allowing for multi-line strings.

### Destructuring
* Destructuring allows assignment from parts of an `array` or `Object` into variables.
* `let [a,b,c] = ["Some value", "another", "third", 4,5,6];`
	* Elements of the array can be ignored by omitting variables:
		* `let [a,,c]=[1,2,3,4]; // a==1 and c==3`

	```
		let {type:type, color, carat} = 
			{type: "asdf", color: "blue", carat: 2.4,
				otherProp: "other val"};
	```
* When the variable is not the same as the property, the format is `property: varaible`(?)


### Object Literal shorthand
* When an object literal contains a property that is assigned from a variable of the same name, the `: varName` can be omitted from `propName: varName`
	```
		let type = 'quartz';
		let color = 'rose';
		let gemstone = {
			type	:	type,
			color	:	color,
		};
		//or equivalently:
		gemstone = {
			type,
			color
		}; 
	```
* This also works with destructuring.
* There is a new shorthand for adding methods to object literals
	```
	let gemstone = {
		type,
		color,
		calculateWorth() {...}
	};
	```

### Iterations
`for` ... `of` loop
* Iterates using the new protocol
* Does not work on objects by default

`for ... in` loops
* Will also include things (like methods) added to `Array.prototype` when used on an array.

`for of` loops provide a better alternative: 
* `for (let index in digits) console.log(digits[index]);`
* `for (let i=0; i<digits.length; i++) console.log(digits[i]);`
* `for (let d of digits) console.log(d);`

###  `...` Spread Operator and Rest parameter
Assigning from `...` unpacks arrays (Spread operator)
```
		const nums = [1, 2, 3];
		console.log(nums); // [1, 2, 3]
		console.log(...nums); // 1 2 3
```
* Can be used in array literals to replace `array.concat`
	* Considering : `const evens=[2,4,6]; const odds = [1,2,3]`
	* Then these two do the same thing:
		* `const nums = odds.concat(evens)`
		* `nums = [...odds, ...evens];`

Assigning to `...` packs into an array (rest parameter)
* When destructuring:
	```
	const vals=[1,2,3,4,5];
	const [first,second, ...rest]=vals;
	console.log(rest);// [3,4,5]
	```
* In varidaic functions (which take a variable number of arguments)
	```
		function sum(...nums) {
		  let total = 0;  
		  for(const num of nums) {
		    total += num;
		  }
		  return total;
		}
	```
* Is equivalent to the old way: 
	```
	function sum() {
	  let total = 0;  
	  for(const argument of arguments) {
	    total += argument;
	  }
	  return total;
	}
	```
## Functions
### Arrow Functions
```
const names = ['bob', 'betty', 'butch'].map(function(name){
	return name.toUpperCase();
});
const names2 = names.map(name=>name.toUpperCase());
```
* This format removes the `{}` braces, `function` keyword, and `()` its parens and replaces the return statement (and `;`) with a single expression.
* The parens around the parameter list are required when there is **not exactly one** parameter.

Arrow functions are always expressions and never declarations.
When there are 0 arguments, some developers use an `_` as the single parameter
* It will be undefined in the function body, though, unless an argument is actually passed.
Concise body format 
* When the body is a single return statement
* Then the normal body (including `{}` can be replaced with the expression that is returned.
Block Body syntax
* When you need more than one statement, then `{}`, `;`s, and `return` are required.

`this` is always bound to the  lexical `this` regardless of how the arrow function is invoked.
* This is very handy for callbacks that would normally require adding `this` to a closure:
	```
	function IceCream(){
		this.scoops=0;
	}
	IceCream.prototype.addScoop = function(){
		const cone = this;
		setTimeout(function(){
			//setTimeout will invoke this function 
			//without binding 'this'
			cone.scoops++;
			console.log('scoop added');
		}, 0.5);
	};
	```
* Using arrow functions
	```
	IceCream.prototype.addScoop = function(){
		//this will be set when the method is invoked
		setTimeout(()=>{ // Keep our 'this' 
			this.scoops++;
			console.log('scoop added');
		},0.5);
	```
On the other hand, some libraries (notably JQuery) will bind a usefull `this` when invoking callbacks, and that value is lost if the callback is an arrow function.

### Default function parameters
```
function greet(name = 'Student', greeting = 'Welcome') {
  return `${greeting} ${name}!`;
}
greet(); // Welcome Student!
greet('James'); // Welcome James!
greet('Richard', 'Howdy'); // Howdy Richard!
```
As opposed to
```
function greet(name, greeting) {
  name = (typeof name !== 'undefined') ?  name : 'Student';
  greeting = (typeof greeting !== 'undefined') ?  greeting : 'Welcome';

  return `${greeting} ${name}!`;
}

greet(); // Welcome Student!
greet('James'); // Welcome James!
greet('Richard', 'Howdy'); // Howdy Richard!
```
Defaults and Destructuring
Basic Arrays
```
function createGrid([width = 5, height = 5]) {
  return `Generates a ${width} x ${height} grid`;
}

createGrid([]); // Generates a 5 x 5 grid
createGrid([2]); // Generates a 2 x 5 grid
createGrid([2, 3]); // Generates a 2 x 3 grid
createGrid([undefined, 3]); // Generates a 5 x 3 grid
createGrid(); // Error
```
**Uncaught TypeError:** Cannot read property `Symbol(Symbol.iterator)` of undefined
A default can also be provided for the entire array
```
function createGrid([width=5, height=5]=[]){
		return `${width} x ${height} grid`
}
createGrid(); // 5x5 grid
```
To skip an argument, pass `undefined`. (Can `,` be used like in assignment destructuring(?))
Objects can be used to bind by name rather than position
```
function houseDescriptor({
	houseColor = 'green',
	shutterColors = ['red']
} = {}) {
  return `I have a ${houseColor} house with `+
	  ${shutterColors.join(' and ')} shutters`;
}
```
NOTE that an `=` is used rather than a `:` to separate variable names from values.

### Classes
Classes are still just functions, but there are some new keywords:
`class` 
* `cunstructor()` method
`static`
`super` - Calls the parent instructor
`extends`- Much cleaner way to specify deeper prototype chains

Old way:
```
function MyClass(p1,p2){
	SuperClass.call(this);
	this.p1=p1;this.p2=p2;
}
MyClass.prototype=Object.create(SuperClass.prototype);
MyClass.prototype.constructor = MyClass;
MyClass.prototype.method1 = function(pn){..};
MyClass.staticMethod = function(params){..};

let myObject = new MyClass(a,b);
```

New way:
```
class MyClass extends SuperClass{
	constructor(p1,p2){
		super(this); // MUST come before 'this'
		this.p1=p1;this.p2=p2;
	}
	static staticMethod(params){..}// no ;
	method1(pn){..}//still no ; or ,
}
let myObject = new MyClass(a,b);	
```

`super()` must be called before `this` is used.

## Built-ins
### Symbols
New primitive data type that is unique and immutable.
They are created by calling the `Symbol()` constructor with an optional description parameter. The fact that they are unique makes them good for keys to an object.

### Iteration protocol

The Iterable interface defines one method and is used by `for of`
* It is keyed with the constant `Symbol.iterator`
* It takes no arguments and returns an object that implements the Iterator interface

The Iterator interface defines one method: `.next()`
* It returns two values in an `object`
* the `value:` for that iteration
* `done:` `bool` `true` on the last iteration
* (?) It looks like SetIterators return `true` with the last value, then undefined and false.

### Sets
Sets are collections that contain unique elements.
They are created with the `Set()` constructor:
```
var set1 = new Set();
var set2 = new Set([1,1,2,3]);
console.log(set1,set2);//Set {} Set {1,2,3}
```
`set.add(element)` Adds a new element to the set if it does not exist. Returns the (modified) set.

`set.delete(element)` Removes an element from the set. Returns a `true` if the item was in the set.
`set.clear()` Removes all elements from the set.
`set.size` Property returns the number of items in the set.
`set.has(element)` returns `true` if the item is in the set
`set.keys()` and `set.values()`  return a `SetIterator` object.

### Weak sets
Like sets, but they
* don't prevent garbage collection
* can only contain objects
* Are not iterable.
* Don't have a `.clear()` method.
* Elements are removed when they are garbage collected.
```
let student1 = { name: 'James', age: 26, gender: 'male' };
let student2 = { name: 'Julia', age: 27, gender: 'female' };
let student3 = { name: 'Richard', age: 31, gender: 'male' };

const roster = new WeakSet([student1, student2, student3]);
console.log(roster);
```
> `WeakSet {Object {name: 'Julia', age: 27, gender: 'female'}, Object {name: 'Richard', age: 31, gender: 'male'}, Object {name: 'James', age: 26, gender: 'male'}}`

…but if you try to add something other than an object, you’ll get an error!
`roster.add('Amanda');`

> `Uncaught TypeError: Invalid value used in weak set(…)`

### Maps
Maps store key-value pairs.
`map = new Map()`
`map.set(key,val)` adds or updates items, returns the map.
`map.delete(key)` moves an item, returns `true` if it was there.
`map.get(key)` returns the value (if any) associated with the key.
`map.keys()` and `map.values()` return iterators
* `.next()` returns `{value,done}`

`for (member of maps) ` gives `[key,value]` for each iteration.
`map.forEach((value,key)=>...);` 

### Weak maps
Only objects can be used as keys.
Are not iterable.
Don't have a `.clear()` method
Members are removed when they are garbage collected (or perhaps when they are available for gc?



### Promises
TODO: A good, concise summary of the promises course

### Proxies
Proxies are objects that act on behalf of another object, with the option to pragmatically intercept lookup requests and such.  
`const proxy = new Proxy(obj,handler);`
`obj` is just any object.
Handler is an object that contains trap functions:
1.  [the get trap](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy/handler/get)  - lets the proxy handle calls to property access
2.  [the set trap](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy/handler/set)  - lets the proxy handle setting the property to a new value
3.  [the apply trap](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy/handler/apply)  - lets the proxy handle being invoked (the object being proxied is a function)
4.  [the has trap](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy/handler/has)  - lets the proxy handle the  `in`  operator
5.  [the deleteProperty trap](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy/handler/deleteProperty)  - lets the proxy handle if a property is deleted
6.  [the ownKeys trap](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy/handler/ownKeys)  - lets the proxy handle when all keys are requested
7.  [the construct trap](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy/handler/construct)  - lets the proxy handle when the proxy is used with the  `new`  keyword as a constructor
8.  [the defineProperty trap](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy/handler/defineProperty)  - lets the proxy handle when defineProperty is used to create a new property on the object
9.  [the getOwnPropertyDescriptor trap](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy/handler/getOwnPropertyDescriptor)  - lets the proxy handle getting the property's descriptors
10.  [the preventExtenions trap](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy/handler/preventExtensions)  - lets the proxy handle calls to  `Object.preventExtensions()`  on the proxy object
11.  [the isExtensible trap](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy/handler/isExtensible)  - lets the proxy handle calls to  `Object.isExtensible`  on the proxy object
12.  [the getPrototypeOf trap](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy/handler/getPrototypeOf)  - lets the proxy handle calls to  `Object.getPrototypeOf`  on the proxy object
13.  [the setPrototypeOf trap](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy/handler/setPrototypeOf)  - lets the proxy handle calls to  `Object.setPrototypeOf`  on the proxy object

`handler.get(target, propName)` is used to intercept property access.
`handler.set(target, propName, value)` is used to handle property assignment.

```
var handler = {
	get(target,propName){return target['propName'];},
	set(target,prapName,value){target['propName']=value;}
}
var proxy = new Proxy({...},handler);
```
#### ES5 getters and setters
```
var obj = {
    _age: 5,
    _height: 4,
    get age() {return this._age;},
    get height() { return this.height; }
};
```
### Generators and Iterators 
Generators are functions that when called return an iterator.  
Internally, they use the `yield` keyword to return values.
The value of the `yield`, inside the generator, will be the value passed to `.next(val)`
`.next()` will return `{value: val, done: false}` when `val` is yielded. 
If the end of the generator has been reached, `.next()` will return `{value: undefined, done: true}`

It looks like a `return val` inside the generator will cause return `{value: val, done: true}` to that invocation of `.next()`
```
function* getEmployee() {
    console.log('the function has started');
    const names = ['Amanda', 'Diego', 'Farrin',
	    'James', 'Kagure', 'Kavita', 'Orit', 'Richard'];
    for (const name of names) {yield name;}
    console.log('the function has ended');
}
const generator = getEmployee();
console.log(generator);
// 	{[[GeneratorStatus]]: "suspended",
//	 [[GeneratorReceiver]]: Window}
console.log(generator.next().value);//'Amanda' the first time
...
console.log(generator.next().value);//'Richard' is last
```

TODO: Figure more out about starting and stopping generators. Can the first `.next()` pass data? What about ending? 
What about iterating over an array that has `undefined` as its last element.



## Compatibility



.
.
.
.

.
.


.


> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTExMDIxNzUwMV19
-->