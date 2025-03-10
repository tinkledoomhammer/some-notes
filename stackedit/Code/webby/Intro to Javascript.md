# Intro to JavaScript
## What is JavaScript
### History
JS was invented by Brendan Ike in 10 days in 1995 while he was working on Netscape Navigator.
1. Originally called livescrpt
2. renamed to javascript to capitalize on the name
3. Development was taken over by ECMA due to diverging standards
4. Today javascript is formally referred to by its ECMAScript version number. I.e. ES5 or ES6
5. The most recent versions are years i.e. ES2016, ES2017, ...
6. The old versions numbering is still used: ES6 = ES2015
7. The most recent version is ES8=ES2017
. 

### The JavaScript console
[https://developers.google.com/web/tools/chrome-devtools/shortcuts]
* `Ctrl-Shft-J` opens the console
 
#### 	`console.log("message");`
#### [thecatapi.com]

## Data types and variables

### comments
* //comment to eol
* /* ... */ multiline comment

### strings
* enclosed in single or double quotes
* concatenated with the `+` operator

Escaping strings

* prefix the character to escape with `\`

Code	|	Character
---|---
`\\`	|	`\` (backslash)
`\"` | `"`  (double quote)
`\'`|	`'` (single quote)
`\n`|	(newline)
`\t`| (tab)

Comparing strings
* case sensitive: `"Yes"=="yes"` << `false`
* lower case are higher than uppercase

### variables
`var varname = varvalue;`
named using camelCase .
[https://google.github.io/styleguide/jsguide.html]

### Indexing
Indexing for string objects takes a numerical 0-based index.
`'12345'[0]` is `"1"`

### Booleans

### Null, Undefined, and Nan
* `null` is a datatype that means 'nothing' or totally empty
	* will be returned as the value of a variable if it has been purposefully set to `null`
* `undefined` is a datatype that means it has never been assigned. 
* `NaN` is returned by numerical calculations when there is an error examples:
	* `Math.sqrt(-10)`
	* `"hello"/5`

### Equality and type coercion
* strange behavior examples (`==true`)
	* `"1"==1` << `true`
	* `0==false` << `true`
	* `"julia"+1 == "julia1"`
	* `"hello" %10 == Nan`
	* `"1" == true`
	* `true>=0` << `true`
* when comparing values of different types with `==` or `!=` both values are converted to the same type
* `===` or "strict equality" is true when they are the same type and their value is the same
* `!==` is its compliment
	
## Conditionals
[https://classroom.udacity.com/courses/ud803/lessons/3ace947b-b5f6-40c1-bc11-3ec98fd1d936/concepts/b13df814-a7c7-4b65-ac3f-112c56ee7726]

### If .. else
```
if (/* this expression is true */) {
  // run this code
} else {
  // run this code
}
```
* the conditional expression is always converted to `true` or `false`
* the `else` clause can be preceeded by one or more `else if` clauses.
```
var weather = "sunny";

if (weather === "snow") {
  console.log("Bring a coat.");
} else if (weather === "rain") {
  console.log("Bring a rain jacket.");
} else {
  console.log("Wear what you have on.");
}
```
* only one of the clauses will be executed.

### Logical Operators
Operator | Meaning | result true when
-|-|-
`&&` | Logical AND | both arguments are true
`||` | Logical OR | either or both arguments are true
`!` | Logical NOT | returns true if the argument (unary) is false
* they are evaluated from left to right, and can use parenthesis
* 
#### Truth tables and short-circuiting
When a logical expression can be evaluated based on the first argument without even knowing the value of the second, then the second argument will not be evaluated. So when the first argument of `||` is true, it is returned immediately. Similarly, when the first argument of `&&` is false, false is returned without ever evaluating the second argument.
#### Truthy and Falsy values
Falsey values convert to `false` when evaluated as bools. There are 6 such values:
1. `false`
2. the `null` type
3. the `undefined` type
4. the number `0`
5. the empty string `""`
6. `NaN`
Everything else is truthy.

#### Ternary operator
`cond ? ifTrue : ifFalse`
i.e. `var color = isGoing ? "green" : "red";`

#### `switch` , `case`, and `default`

```
switch (option) {
  case 1:
    console.log("You selected option 1.");
    break;
  case 2:
    console.log("You selected option 2.");
    //fallthrough
  case 3:
    console.log("You selected option 3.");
  default:
	//more stuff
}
```

## Loops

### `while` loops
Every loop construction needs to include 
1. When to start
2. When to stop
3. How to get to the next step.
While loops make it easy to forget one

### `for` loops
`for (init;cond;incr){body;}`
* Init is run before the body
* The iteration order is init > cond > body > incr > cond > body > inc ... > incr > cond > (done)

### Assignment operators
`x++` , `++x`,  `x--`, `--x`, `x+=y`, `x-=y`, `x*=y`, `x/=y`

 
## Functions
### basic syntax
declaration: `function funcName(parameters){code;}`
evocation: `funcname(arguments);`
return: `return value` (optional) doesn't need parens 
* unreachable returns do not raise an error


### scope
The code from which a variable is visible.
There are two kinds:
1. global scope is where variables that are declared outside of a function
	2. can be accessed from anywhere
2. function/local scope
	3. accessible from the function and also functions defined within that function

#### shadowing
Function scope variables shadow global variables and variables with the same name declared in enclosing functions.
```
var val=0;
function outer(){
	val=val+1; // outer scope val gets increased
	console.log(val); // 2
	function inner(){
		var val=2;	// shadows the global value
		val=val+1;
		console.log(val); // 3
	}
	console.log(val);	// 2, still using the global val
}
console.log(val);// 0
outer();
console.log(val);// 2
```

#### hoisting
All variables and functions declared in a function are 'hoisted' to the top of the function.
* includes inside blocks
* only applies to the current scope, doesn't change the scope to a different function
* all declarations in the function, excluding declarations within child functions, are accessible from anywhere in the function, even before the declaration
* assignments are not hoisted
```
console.log(say);	// say() is accessible because it's declaration gets hoisted.
function say(){
	return(greeting);	// returns `undefined` because the assignment hasn't happened yet
	var greeting="hi";
} 
```

### function expressions
Anonymous functions stored in variables. 

`var catSays = function(stuff) {statements;};` is equivalent to `function catSays(stuff) {statements;};`

* Because assignments are not hoisted, function expressions aren't either, and so cannot be used before their assignment.
* 
#### Named function expressions
` var pubName = function otherName(params){body;};`
otherName is usable inside the function body, pubName is usable in the containing scope.

### functions as parameters, callbacks



## Arrays
### declaration
`var donuts = ["glazed", "powdered", "jelly"];`
`var mixed = ["abc", 1, true, "def"];`
`var nested = [[1, 2, 3], ["Julia", "James"], [1,true, false]`
	`nested [0][1]==2	//true`

### indexing
element
: signifies individual pieces of data in an array

index
: refers to the data needed to access the element.
Arrays are always indexed numerically, starting with `[0]` (?)

If the index is too high, `undefined` is returned.
	i.e. `["1", 1, "one"][3]	//	undefined` because `[2]` is the max index defined.

Elements can be assigned/re-assigned using their index:
	``` 
	var a=[1,2,3];
	a[0]=0;
	a		//	[0,2,3]
	```

### Array Properties and methods

#### `array.length` 
* property containing the number of elements in the array

#### `array.push(val)` 
* method that adds an element to the end of the array
* returns the length of the array

#### `array.pop()` 
* compliment to push, returns the element at the end of the array, and shortens the array

#### `array.splice()`
* can add or remove elements from anywhere in the array
* `[1, 2, 3].splice(start,remove, new)`
	* removes `remove` elements starting at the index `start` and then adds the new elements at the same location.
	* `[1, 2, 3].splice(1,1,4,5); // [1, 4, 5, 3]`
	* `start` can have a negative index, meaning 'back from the end'
		* `-x` is equivalent to `array.length-x`, but apparently 0 is treated as the beginning.
		* Is there a `-0` equivalent other than passing in `a.length` ?
	* returns an array of the elements that were removed

#### other methods
1. `array.reverse()`
2. `array.sort()`
3. `array.shift()`
	4. `.unshift()`
4. `array.join()` returns a concatenation of the elements of an array

#### `foreach`
`array.forEach(myFunction(element, index, array));`
* the parameters are optional
* returns `undefined`
#### `map`
* similar to `foreach`
* returns a new array instead of making modifications in place
`array.map(function(element){dostuff;});`
* the callback only takes one argument

## Objects

### `typeof`
primitive types: `string`, `number`, `undefined`, `null`, `boolean`
returns a string containing the data type

### object literals
```
var sister = {
  name: "Sarah",
  age: 23,
  parents: [ "alice", "andy" ],
  siblings: ["julia"],
  favoriteColor: "purple",
  pets: true,
  paintPicture: function() { return "Sarah paints!"; }
};
```
### Mambers
Members can be accessed in two ways
1. `obj.key==value`
2. `obj["key"]==value`

Valid (non-quoted) member names

1. must start with a letter
2. cannot contain hyphens












.




.
.
.
.
.



> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE4NjI1NjQ2MF19
-->