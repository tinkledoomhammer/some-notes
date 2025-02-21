# Lua Part 1 - The Language
[Programming in Lua, 1^st^ edition](https://www.lua.org/pil/contents.html)
## Overview
### Chunks
A **chunk** is a sequence of Lua statements.  For example, a file or a single line in interactive mode.  
The `lua` command can be run with multiple chunks using the `-l` option:
`lua -la -lb` will execute files 'a' and 'b' if they are fund in the path.
Internally, this uses `require` which will be covered later.
This also works with interactive mode: `lua -i -la -lb`
`;` s are optional, and by convention are used to separate multiple statements on the same line.
Newlines are not significant.

### Global variables
* Do NOT require declaration.
* Will have a value of `nil` if unassigned.
* Can be deleted by assigning a value of `nil`
### Some Lexical conventions
Identifiers:
* Must begin with a letter or underscore
* Can contain letters, numbers, and underscores
* Avoid names that start with an underscore followed by one or more capital letters
	* They are reserved for special uses (i.e. `_VERSION`)
* Lua is case sensitive

Reserved words: 
```
    and       break     do        else      elseif
    end       false     for       function  if
    in        local     nil       not       or
    repeat    return    then      true      until
    while
```
Comments
* Start with `--` 
* Single-line comments run to the end of the line.
* Multiline comments: `--[[` until `--]]`

### The Stand-Alone Interpreter
Called `lua.c` or `lua` for the source and executable files, respectively.
` lua [options] [script [args]]`
* `-e`  option allows us to enter code directly into the command line. For instance,
	*	`prompt> lua -e "print(math.sin(12))"   --> -0.53657291800043`
*	`-i` can be used with `-e`
	*	  `  prompt> lua -i -l a.lua -e "x = 10"`
	*	will load the file `a.lua`, then execute the assignment `x = 10`, and finally present a prompt for interaction.

Interactive mode uses the global variable `_PROMPT` for the prompt.

`LUA_INIT` environment variable
* Either lua code or `@filename` where `filename` is a file to execute before the arguments specified on the command line.

`args` global variable
* Can be used by the main script to retrieve command line arguments
* It is a table, numerically indexed. 
* Index 0 contains the name of the script
* Higher indexes contain the arguments to the script
* Lower indexes contain the arguments passed to lua, in the order they were passed. i.e.
* `prompt> lua -e "sin=math.sin" script a b`
* `lua` collects the arguments as follows:
```
	    arg[-3] = "lua"
	    arg[-2] = "-e"
	    arg[-1] = "sin=math.sin"
	    arg[0] = "script"
	    arg[1] = "a"
	    arg[2] = "b"
```

## Types and Values
There are 8 types in lua: _nil_, _boolean_, _number_, _string_, _userdata_, _function_, _thread_, and _table_
The `type(v)` function returns the name of the type of `v` as a _string_.

`nil` type has a single value, called `nil`
* Used for non-values
* Like unassigned variables.
* Can be assigned to delete variables.

`boolean` has two values: `true` and `false`
* only 	`false` and `nil` are falsey.  All other values are true-ish.
* `0` and `''` both evaluate to `true`

`numbers`
* Lua by default uses double-precision floating point numbers only. It can be compiled to use other numerical types.
* Examples:   `4     0.4     4.57e-3     0.3e12     5e+20`

`functions` 
Lua has first class functions.  It can also call c functions easily. The entire standard library is in c.

`userdata` type is opaque and only supports assignment and equality testing.
Presumably it is for use by c functions.

`threads` will be described in [Chapter 9](https://www.lua.org/pil/9.html#CoroutineSec) which covers corutines. 

### Strings
* Immutable
* 8-bit clean
	* can store arbitrary binary data
* Delineated by single or double quotes.
* c-like escape sequences are supported: 
*   
|  Esc | Meaning  |  Esc | Meaning
|  --  | -- | -- | -- |
| `\a` | bell				| `\b` | back space|
| `\f` | form feed			| `\n` | newline |
| `\r` | carriage return	| `\t` | horizontal tab |
| `\v` | vertical tab		| `\\` | backslash | 
| `\"` | double quote 		| `\'` | single quote |
| `\[` | left square bracket| `\]` | right square bracket |
* Unicode characters can be escaped with `\ddd` where `ddd` is up to three decimal digits.

Multiline string literals
* begin with `[[` and end with `]]`
* can be nested
* don't interpret escape sequences
* ignores the first character if it is a newline.

Strings can be converted automatically at run time by applying an arithmetic operation to them.
Or explicitly with the global `tonumber(str)` function, which will return `nil` if it couldn't convert a number. 

`..` is the concatenation operator.  `+` is for numbers.

To convert from a number to a string
* call the global `tonumber(str)` or `..` concatenate it with an empty string.


### Tables
Tables are associative arrays.
The keys can be any value or object except `nil`
`obj.ind` is the same as `obj['ind']`

They can be iterated with `for k,v in pairs(obj) do ... end --for`
Numerically indexed arrays have the `ipairs(table)` iterator
* It stops on the first `nil` value
* Indexing starts with `1` by convention.

Be careful that strings and numbers will be different indexes. Be explicit and convert to numbers where appropriate. 
i.e. don't confuse `1`, `"1"`, `"01"` and `"+1"` 

## Expressions
Arithmetic operators:
* `+`, `-`, `/`, and `*` are part of the lua core and work as expected on numbers.
* `^` for exponentiation is part of the math library

Relational  Operators:
* `>` , `<`, `==`, `<=`, `>=`, and `~=` (i.e. "not equal")
* Values are only equal if they have the same type.
	* so no string will ever `==` a number
	* `nil` only `==` `nill`
* tables, userdata, and functions are compared **by reference**.
* Strings are compared by alphabetical order according to locale settings.
* Ordering operators only work with two numbers or two strings.

`..` is the string concatenation operator.

Operator precedence:
```
          ^ (right-associative)
           not  - (unary)
           *   /
           +   -
           ..		(right-associative)
           <   >   <=  >=  ~=  ==
           and
           or		(short-circuits)
```
All operators are left-associative except for `..` and `^`

### Table Constructors
* Similar to JavaScript
* but with `=` instead of `:`
* and keys can be omitted. 
	* They will be replaced with 1-based numbers.
* Semicolons `;` can be used as separators in addition to `,`
	* It is good style to use them to separate sections of the table.
* Trailing `,` is allowed.
* Keys can be expressions, and should be enclosed in square brackets:
	* `["keyName"] = val,`
* 
```
    polyline = {color="blue", ["thickness"]=2, npoints=4,
                 {x=0,   y=0},
                 {x=-10, y=0},
               }
    print(polyline[2].x)    --> -10
    print(polyline["color"]) --> "blue"
```



## Statements
Assignment
* multiple assignment is supported
	* `obj[x], obj[y] = obj[y], obj[x]`
	* Extra values on the right are ignored
	* extra values on the left get `nil`

Local variables
* Declared with the `local ` keyword: `local x = 1`
* Scoped to the block in which they are declared 
	* and any enclosed blocks
* Declarations are statements
* There is no hoisting. Local scope runs from the declaration to the end of the block.
	* This is idiomatically used to limit scope and prevent forgotten initialization
* If a `local` is declared but not initialized, its value will be `nil`
* It is a common idiom to shadow global variables with local variables
	* `local foo = foo`
	* This speeds up access and preserves the original value of `foo` even if the global `foo` is changed.

Blocks can be 
* the body of a control structure
* the body of a function
* or a chunk
* can be explicitly created with a `do`...`end` structure

### Control Structures
If then elseif end
```
    if op == "+" then
      r = a + b
    elseif op == "-" then
      r = a - b
    elseif op == "*" then
      r = a*b
    elseif op == "/" then
      r = a/b
    else
      error("invalid operation")
    end
```

While Loops
```
	local i = 1
    while a[i] do
      print(a[i])
      i = i + 1
    end
```

Repeat Loops
```
    -- print the first non-empty line
    repeat
      line = os.read()
    until line ~= ""
    print(line)
```

Numerical `for` loops
* ` for var = init, stop, step do ... end`
* `init`, `stop`, and `step` are only evaluated once
* `step` is optional and defaults to 1
* the `var` is always local to the for-loop block
* The last iteration may be when `var == stop` 

Generic `for` loops
* `for k,v in pairs(obj) do ... end`
* Other iterators include 
	* `ipairs()` which only iterates over numerical indecies starting from `1` and ending before the first `nil` value.
	* `io.lines` - 
	* `string.gfind` - [Chapter 20](https://www.lua.org/pil/20.html#StringLib)
	* custom iterators are covered in [Chapter 7](https://www.lua.org/pil/7.html#GenericFor).

`break` and `return` 
* must be the last statement in their block
* `break` must be used inside a loop (`for` , `repeat`, or `while`)
* Enclosing them in a `do ... end` block will prevent a syntax error 
	* but the code after will still be unreachable.
* `return` can return one or more results
	* separated by a coma: `return 1, "abc", expr`

## Functions
Declaration
`function funcName(param1, param2...) 
	...
end
`

Evocation
`funcName (arg1, arg2...)`

If there is 1 argument and it is a string literal or object literal, the parens around the argument are optional:
`funcName "An argument"`  is the same as `funcName("An argument")`

If there are fewer arguments than parameters, the unused parameters get `nil`
If there are more arguments than parameters, the excess gets discarded.

`or` can be used to assign default values: 
```
function f(a,b)
	a = a or 1
	b = b or 2
	-- do stuff
end
```
* Remember that only `nil` and `false` are falsey.

### Functions can return multiple results:
```
function f(a,b)
	return a, a+b, a-b
end
```

Using those results can get a little weird. These 3 functions will be used to explain:
```
    function foo0 () end                  -- returns no results
    function foo1 () return 'a' end       -- returns 1 result
    function foo2 () return 'a','b' end   -- returns 2 results
```
In a multiple assignment, a function call as the **last** (or only) expression produces as many results as needed to match the variables:
```
    x,y = foo2()        -- x='a', y='b'
    x = foo2()          -- x='a', 'b' is discarded
    x,y,z = 10,foo2()   -- x=10, y='a', z='b'
```
If a function has no results, or not as many results as we need, Lua produces **nil**s:
```
    x,y = foo0()      -- x=nil, y=nil
    x,y = foo1()      -- x='a', y=nil
    x,y,z = foo2()    -- x='a', y='b', z=nil
```
A function call that is **not the last** element in the list always produces **only one** result:
```
    x,y = foo2(), 20      -- x='a', y=20
    x,y = foo0(), 20, 30  -- x=nil, y=20, 30 is discarded
```
When a function call is the **last (or the only) argument** to another call, all results from the first call go as arguments. We have seen examples of this construction already, with  `print`:
```
    print(foo2())          -->  a   b
    print(foo2(), 1)       -->  a   1
    print(foo2() .. "x")   -->  ax         (see below)
```
When used **in an expression** the number of results is reduced to 1.

To convert the results to an expression, use parens.
`print ((foo2()) -->a`

Caution: `return (result1, result2)` will drop `result2` because of the parentheses. 
The global function `unpack(a)` takes an array and returns it's values (numerically indexed from 1)

### Variable number of arguments
`...` can be used in an argument list
The extra arguments will be packed into a table named `arg`
`arg` works like an array, with an extra parameter `n=` the number of arguments passed.

With `function g (a, b, ...) end`, we have:
|Call|Parameters  |
|--|--|
| g(3) | a=3, b=nil, arg={n=0} |
| g(3,4) | a=3, b=4, arg={n=0} |
| g(3,4,5,8) | a=3, b=4, arg={5,8;n=2}|

Variable arguments can be used with `unpack` to pass them to another function that also takes a variable number of arguments.
```
function fwrite(fmt, ...)
	return io.write(string.format(fmt,unpack(arg)))
end
```
There is no support for named arguments, but the object literal syntax is comparable.

### Closures
Function assignments
	`function funcName(params) ... end` 
		is the same as 
	`funcName = function(params) ... end`
The sugar works for any `funcName` even something like `myLib.myFunc` 
`function myLib.myFunc(params)... end`

The down side is that functions may require forward declarations:
```
	local f,g
	function g() ... return f() end
	function f() return g() end
```
The functions won't compile correctly unless the declaration precedes the invocation.

Closures are fully supported. Upvalues are retained by function references
```
	function newCounter ()
	  local i = 0
	  return function ()   -- anonymous function
	           i = i + 1
	           return i
	         end
	end

	c1 = newCounter()
	print(c1())  --> 1
	print(c1())  --> 2
	c2 = newCounter()
	print(c2())  --> 1
	print(c1())  --> 3
	print(c2())  --> 2
```

Every block is a scope, so creating a closure to store upvals is most easily done with a `do...end` block:
```
	do
	  local oldSin = math.sin
	  local k = math.pi/180
	  math.sin = function (x)
	    return oldSin(x*k)
	  end
	end
```
This block redefines the global `math.sin()` function to work in radians instead of degrees.
The pattern is useful for creating sandboxes.

Lua also has full support for proper tail calls.

## Iterators and the Generic For
Iterators are functions that maintain internal state, and each call returns the next item. `nil` is returned to signal that there are no more.
An example that iterates over the words in a file:
```
	function allwords ()
		local line = io.read()  -- current line
		local pos = 1           -- current position in the line
		return function ()      -- iterator function
			while line do         -- repeat while there are lines
			local s, e = string.find(line, "%w+", pos)
			if s then           -- found a word?
			  pos = e + 1       -- next position is after this word
			  return string.sub(line, s, e)     -- return the word
			else
			  line = io.read()  -- word not found; try next line
			  pos = 1           -- restart from first position
			end
		  end
		  return nil            -- no more lines: end of traversal
		end
	end
	for word in allwords() do
		print(word)
	end
```
https://www.lua.org/pil/7.5.html

The generic `for` loop can maintain state in cases where the expense of a closure is undesirable.  The iterator returns up to three values
1. The function to return the next item is the only required one.
2. An invariant state that will be passed to the function on each iteration
3. A variant state (control variable) that will also be passed, and will be assigned with the first return of the next function.

`for var_1, ..., var_n in explist do block end` is equivalent to:
```
	do
		local _f, _s, _var = explist
		while true do
			local var_1, ... , var_n = _f(_s, _var)
			_var = var_1
			if _var == nil then break end
			block
		end
	end
```

There are several stateless iterators in Lua. `ipairs()` can be implemented like this
```
	function iter (a, i)
		i = i + 1
		local v = a[i]
		if v then
			return i, v
		end
	end
	function ipairs (a)
		return iter, a, 0
	end
```

Tables can be used to track more complicated state in an iterator, but
1. Closures are cheaper to create
2. Accessing upvalues is faster than a table lookup.

A common pattern for stateless "true iterators" in previous versions of lua was to pass a function to the iterator i.e.
```
	function allwords(f)
		for l in io.lines do 
			for w in string.gfind(l,"%w") do f(w) end
		end
	end
	allwords(print)
```

## Compilation, Execution, and Errors

### Compiling external code
#### `loadfile(fname)` and `loadstring(str)`

The Lua compiler is available at run time.
`dofile(filename)` - Is the simplest way to use it. The function is a wrapper around `loadfile()` which looks something like:
```
function dofile(filename)
	local f = assert(loadfile(filename))
	return f()
end
```
`loadstring(str)` is similar to `loadfile(filename)` but it compiles the string rather than a file.

Both `loadstring(str)` and `loadfile(filename)` will handle errors by `return`ing both `nil` and an error value, rather than throwing an error.

The return of both files is the compiled chunk, wrapped in an anonymous function so
`f = loadstring('local a = 10; return a+20") `
is the same as :
`function f() local a = 10; return a+20; end`

`assert(expr)` is useful because `loadstring(s)()` will behave oddly if there is an error.
* Since `loadstring` returns `nil`, the error that gets thrown will be `"attempt to call a nil value"` rather than the actual error. Instead use: `assert(loadstring(s))()`. If `s` has a syntax error, then it becomes `assert(nil, error)`. 

The downsides to `loadstring` and `loadfile`: 
* They are expensive/slow. If they are used, the results should be saved for re-use where possible.
* They get a fresh lexical scope. Variables that are local to the scope where `loadstring` etc are invoked will not be accessible inside the chunks they create.
* They can make code much harder to read.

If the compiled chunks end with a `return` statement, then invoking the function returned will return the result. 

(?)Is there a way to pass arguments to the chunks?(?)

#### `require(filename)`
**NOTE: This may have changed.**  The following notes may not apply after Lua 5.0. Mushclient uses 5.1

The `require` function keeps a file of files it has loaded in `_LOADED`

The argument passed to `require` is used to key `_LOADED`. This means that `require"a.lua"` and `require.a` may have separate entries, even if they load the same file.  The module may thus be executed more than once. 

The files are looked up using a path variable.  To determine this path, first the global variable `LUA_PATH` is checked. If it is a string then that is the path. Otherwise, the **envrionment variable** `LUA_PATH` is used. If both fail, then a fixed default path is used.  This is usually `"?;?.lua"`.

The path may contain a fixed file that will be loaded if no matches were found. It should be listed at the end of the path: `"?;?.lua;/usr/local/default.lua"` 

Before `require` runs a chunk, it sets a **global variable** _REQUIREDNAME	which contains the argument passed to require.  One could theoretically set the path to a single file which uses that variable to load the desired file. 

#### C Packages: `loadlib(path,init)`
The `loadlib(..)` function is (perhaps) the only part of lua that is not strictly ANSI C. It is implemented differently on different platforms because dynamic linking is nowhere in the standard, and handled differently depending on the OS.
* It links the library to lua and returns the initialization function.
* It is not supported in mushclient.
* The first argument is the complete path to the library including the file itself. 
* The second argument is the name of the initialization function.
* Most C libraries are packaged with a LUA stub that calls `loadlib` and runs the init function when the stub is `require`d

### Errors
`error(msg)` is the "throw" of lua.  
There are two conventional ways to handle errors
1. For errors that are **not** easy to prevent, functions `return` `nil` or `false` and the error message.  
2. For errors that are easy to prevent (at compile time, for example) it is more common to use `error` to throw an error.

`assert(expr,msg)` converts the first case to the second.
* it uses the handiness of multiple return values
* If the first part is truthy, it is returned.  If it is falsey, an `error` is produced.

`pcall(func)` is the try-catch of lua.
* It calls its first argument in "protected mode" -- errors are caught.
* It returns `true` plus any values returned by the function if there were no errors.
* If there were errors, it returns `false` plus the error message.
* It unwinds the stack, so the error's origin will be shown as the `pcall`. 
* Use `xpcall(msg,handler)` to get the stack trace within the pcall.

`error(msg, level)` - `error` takes a second argument which changes the stack level at which the error is reported.
* level 1 is the current function. Higher numbers move up the call stack

`xpcall(msg,handler)` - Like `pcall` but the optional error handler will be called before the stack is unwound if there is an error
* `debug.debug` an error handler that opens a debugger
* `debug.traceback` an error handler that builds and extended error message with a traceback.
	* This one can be called at any time to get the current call stack
	* i.e. `print(debug.traceback())`

## Coroutines
[Chapter 9](https://www.lua.org/pil/9.html)

Lua has asymmetrical coroutines. They are created with `coroutine.create(func)`, which returns the thread object which will be in the `suspended` state. They are reactivated with `coroutine.continue(co)` and can be suspended from within with `coroutine.yield()`. Their status is obtained from `coroutine.status(co)`  which will return `'suspended'` while the coroutine can be resumed.

`coroutine.continue(co)` will return `false` and an error message when the coroutine has exited.  It will return the value `yield`ed by the coroutine, and will pass in any additional arguments which will be returned by the `yield` inside the coroutine.

Filters / Pipes
* coroutines that `resume` a producer, modify the result, and `yield` it to a consumer.
* They are usually very light weight because switching threads with coroutines is much faster than switching processes.

Iterator example
Here is a function that produces all the permutations of an array:
```
function permgen(a,n)
	if n==0 then printResult(a)
	else
		for i = 1,n do
			a[n],a[i] = a[i],a[n]
			permgen(a,n-1)
			a[n],a[i] = a[i],a[n]
		end
	end
end	
```
And its invocation and supporting functions
```
function printResults(a)
	for i,v in ipairs(a) do
		io.write(v," ")
	end
	io.write("\n")
end
permgen({1,2,3,4},4)
```
Using coroutines, that becomes
```
function permgen(a,n)
	if n==0 then coroutine.yield(a) --the only change here
	else
		for i = 1,n do
			a[n],a[i] = a[i],a[n]
			permgen(a,n-1)
			a[n],a[i] = a[i],a[n]
		end
	end
end

function perm(a)
	local n = table.getn(a)
	local co = coroutine.create(function() permgen(a,n) end)
	return function() -- the iterator function
		local code,res = coroutine.resume(co)
		return res
	end
end

for p in perm{1,2,3,4} do
	printResult(p)
end
```

`coroutine.wrap(func)` is similar to `coroutine.create(func)`
* returns a function that resumes the coroutine
* the returned function throws an error(when one occurs) rather than returning a success/fail code.
* simpler to use than `create` but less flexible
	* Cannot check the status of the coroutine
	* cannot check for errors


## Complete Examples



# Part 2 Tables and Objects


# Part 3 The Standard Libraries


## String library

# Part 4 The c API

.



.
.

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTc3NjkyMTYwNV19
-->