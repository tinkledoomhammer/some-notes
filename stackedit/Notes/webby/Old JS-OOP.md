# Object-Oriented JavaScript
ES<6 , no `class` keyword
[ud015](https://classroom.udacity.com/courses/ud015)

## Scope
### Lexical scope 
is the region of source code where a variable name can be accessed without error. 
1. Global scope is shared between files
2. A new lexical scope is created for each function definition.
3. Assignment to undeclared variables are added to the global scope.
	1. Trap for young players.
4. Blocks do not create new scopes, only functions.

### Execution contexts
are in memory storage of lexical scopes.
* There may be several execution contexts for a given lexical scope
* The global execution context is created at the beginning
* New execution contexts are created whenever a function is EXECUTED


## Closures
Closures are the execution context associated with a function object.
They are created whenever a reference to a local function is passed or returned to a function in another context. i.e. storing in global variable, passing to setTimeout(), or returning it to the caller.




## The `this` keyword
1. Mostly works like a regular parameter but
2. It is bound differently (there are about 5 different ways?)


### Definition
#### What it isn't
*  `var fn=function(){log(this);};`
	* `this` is NOT `fn`
	* nor is it a new instance (usually, but sometimes it is)
* `var ob= {method: fn}`
	* `this` is NOT `ob` or any other object that the function is attached to.
	* also applies to object literals
* `var obj.fn(3,4)`
	* `this` is NOT an arguments binding or execution context
	* `this` IS obj because the `.fn()` binds the method and `this`  asdf

#### how `this` is defined at call time 

* it mostly behaves like a function parameter
* it is defined by the dot in `obj.meth()` 
* or by the indexing in `obj['meth']()`
* it defaults to the global object `<default>` 
* it can also be bound using the `function` object's `.call(params)` which takes `this` as the first parameter
* 
#### To figure out what `this` points to
* work backwards through the stack
* until a method is invoked
* if it is not bound (with `obj.func()` or `func.call(myThis)`, etc then continue

#### The `new` keyword
Binds `this` to a new object regardless of how the constructor is evoked. 


## Prototype chains

Delegate failed lookups from an object to its prototype.

### Property lookup

`Object.create(proto)` returns a new object that delegates undefined elements to the specified prototype.

If an object has doesn't have a property, then it's prototype is checked.

#### The `object` prototype
Defines all the methods that are shared between all `object`s. 
`.constructor` property usually refers to `Object` (?)

#### The `array` prototype
* Delegates from object
* Defines a `.constructor`

## Object Decorator Pattern
 Constructor-like functions usually named as an adjective that add properties to an object i.e. 
 ```
 var carlike=function(obj,loc){
	obj.loc=loc;
	obj.move=function(){this.loc++;};
	return obj; 
}

var amy=carlike({},1);
var ben=carlike({},9);
amy.move();
ben.move();
```
The above is nice in that it puts all the code in the right visual place, but creates a new `.move()` function for each `carlike` object. Binding to a function that is defined outside of `carlike` will only create a reference to the function for each object. 

Changing `this` to `obj` in `obj.move=function(){this.loc++;};` will work only when the function is defined within the decorator. 

## Functional Classes
Constructor functions are like decorator functions.
* They produce a new object.
* They are usually named as a capitalized noun i.e. `Car` vs `carlike` 

To reduce in-memory duplicity, (at the cost of closure-access to the constructor's parameters), move methods to the containing scope so that only one function object exists, not one per instance.  (Functional-shared)
* This means that there are two lists of functions to be bound (where they are bound to new objects and where they are defined)
* to clean it up

 ```
var Car = function(loc){
	var obj={loc:loc};
	extend(obj, Car.methods);
	return obj;
};
Car.methods = { move: function(){this.loc++}};
 ```


## Prototpyical Classes
Like above, but with delegation instead of assignment for the methods.
`object.create(proto)` allows you to bind a prototype, so that failed lookups will refer to the methods collection
 ```
var Car = function(loc){
	var obj=Object.create(Car.methods);
	obj.loc=loc;
	return obj;
};
Car.methods = { move: function(){this.loc++;}};
 ```

### The `.prototype` property
*  is provided to be used like `Car.methods` in the example. 
* but it doesn't have any special behavior (other than that it exists already)
* so `Car.prototype.move=function(){this.loc++;}};` works.
* So the convention is to use `.prototype` instead of `.methods`. 
* It still has to be bound with `Object.create(proto)` or the `new` keyword.
* The `.prototype` has a `.constructor` property that points back to the constructor function. (`Car` in these examples.)

DON'T GET CONFUSED BY THE NAME `.prototype` it doesn't have much special behavior. `Car.prototype` is not `Car`'s prototype.

### `instanceof` operator
`obj instanceof Constr` is true when `Constr` can be found anywhere in `obj`'s prototype chain.
So classes have to use `Object.create(prot)` to bind it.
* (?) Is it looking for `Constr.prototype` or any `prototype.constructor === Constr`? Is there a difference (?)


## Pseudoclassical Patterns
A thin layer of syntactic convenience that makes the prototypical pattern look more like the classical patterns.
The keyword `new` is provided to remove the need for a few lines.
* this is called Constructor mode.
* It basically maps a `this` parameter that is created and 
* `this=Object.create(Car.prototype);` at the beginning
* `return this` at the end

So the `Car` example becomes:
```
var Car = function(loc){this.loc=loc;}
Car.prototype.move=function(){this.loc++};
//to use it
Amy = new Car(1);
```

There are performance optimizations that only work when using the pseudoclassical pattern. 

## Superclass and Subclass
### Functional subclass
```
var Car = function(loc){
	var obj={loc:loc};
	obj.move = function(){obj.loc++;};
	return obje;
}
var Van = function(loc){
	var obj = Car(loc);
	obj.grap = function{...};
	return obj;
}

var Cop = ...;
```

### Pseudoclassical subclass
```
var Car = function(loc){this.loc = loc;};
Car.prototype.move=function(){this.loc++;};

var Van = function(loc){
	Car.call(this,loc);
};
Van.prototype = Object.create(Car.prototype);
Van.prototype.constructor = =Van;
Van.prototype.grab = function(){/* garb */};
```

Before `Object.create(proto)` it was common to use a dummy instance the super class such as `new Car(dummyLoc)`.
`Van.prototype = new Car(someLoc);` may still be used in some outdated material.




## Final Project
[https://github.com/udacity/frontend-nanodegree-arcade-game]
* artwork
* Links to
	* [Rubric](https://review.udacity.com/#!/projects/2696458597/rubric)
	* [Guide]( https://docs.google.com/document/d/1v01aScPjSWCCWQLIpFqvg3-vXLH2e8_SZQKC8jNO0Dc/pub?embedded=true)
	* Also has scaffolding
	* [Video](https://www.google.com/url?q=https://www.youtube.com/watch?v%3DSxeHV1kt7iU%26feature%3Dyoutu.be&sa=D&ust=1531744601048000) showing what the completed project should look like. 

### Contents of the repo
* `css` folder - don't need to edit anything here
* `images` folder - contains the image assets. don't need to edit anything here
* `js` folder - contains the app engine, only need to edit `js/app.js` (see below)
* `index.html` used to load the game
* `README.md` the readme, which should be updated to include instructions
### `app.js` a partially complete app
The following modifications need to be made
####  `Enemy` class
* constructor
	* load the image by setting `this.sprite` to an image from the provided folder
	* setting the enemy initial location and speed
* `update` method
	* updates the enemy location
	* handles collision with the player
* add any additional functions needed
#### `Player` class
* constructor
	* set up the location and and this.sprite
* Add an update method (can be based on `Enemy.prototype.update`)
* Add a `render` method, can be based on the method of the `Enemy` class
* Add a `handleInput` method which should `allowedKeys(`the particular key that was pressed`)` and move the player accordingly
	* Left , right, up, and down should move the player in that direction
	* The player needs to stay on the screen
	* The player should reset upon reaching the water at the end (top)
* Add any other methods as needed
#### Glue it all together
* create a new `Player` object
* Create new `Enemy` objects and place them in an array called `allEnemies`

#### Notes 
So all the work for the project goes in `app.js`
Coordinates are in the `canvas` 's coordinate system
* pixels, [0,0] is the top left (?) and [width,0] is top right
Time is in seconds (the dt parameter passed to `.update(dt)` methods.

Keystrokes are reported on keyup commands, which means that the player sprite needs to move a fixed distance per keypress. 
* ~~could use time to or distance to determine when to stop.~~
* ~~could save ending coordinates or a velocity and time stamp~~
* `player.update()` doesn't get a `dt` argument, so just move it.
* The row and column widths are in `engine.js` on line 137 in the `render()` function. 
	* `ctx.drawImage(Resources.get(rowImages[row]), col  *  101, row  *  83);`

### Learnings from the gitHub
#### resources.js
* The whole thing s wrapped inside `(function(){...})()`
* It assigns `window.Resources  = {load:  load, get:  get, onReady:  onReady, isReady:  isReady};`
	* this is all of the externally available/published stuff
* The closure uses `var resourceCache={},loading=[], readyCallbacks=[];`
* private functions begin with `_` i.e. `_load(url)`
* `function load(urlOrArr)` takes an array or single url and simply calls unpacks and calls `_load(url)` 
* `function get(url)` returns a cached image. Like `load(url)` but assumes that the it has already been loaded.
* `function isReady()` returns `true` when all of the requested images are fully loaded.
* `function onReady(fun)` adds a callback to the chain that will be called after all images have been loaded.
* `function _load(url)` Internal function called by `function load(urlOrArr)`
	* The same as `function get(url)` if the url has already been loaded.
	* Otherwise, it creates a `new Image()`
		* Sets its `onLoad` callback to 
			* Store the  `Image` object in `resourceCache[url]`
			* Call the `readyCallbacks` if everything is done.
		* Caches the value `false` in  `resourceCache[url]` 
		* Sets the `image.src=url`, to initiate image loading
* To use
	* Register any callbacks with `Resources.onReady(fun);`
	* Call `Resources.load([url1,url2]);` to pre-load images
	* Access images with `Resources.load(url)` which will return an `Image` object.
	* Poll `Resources.isReady()` to determine whether there are images currently being loaded.
	* Optionally, use `Resources.get(url)` to access an image object that has already been completely loaded.
		* Will return `undefined` if the image load was never started
		* Will return `false` if the image is being downloaded now
#### engine.js
* It is wrapped in `var Engine=(function(global){...})(this);`
```
var  doc  =  global.document,		//convenience
	win  =  global.window,			//convenience
	canvas  =  doc.createElement('canvas'),//
	ctx  =  canvas.getContext('2d'),//
	lastTime;						//used to determine time intervals

canvas.width  =  505;
canvas.height  =  606;
doc.body.appendChild(canvas);
```
It defines several functions:
* `function main()` - the main game loop
	* `dt = (DateTime.now()-lastTime)/1000;`
	* `update(dt);	// Used to move things around.`
	* `render();`
	* `lastTime+=1000*dt;`
	* `win.requestAnimationFrame(main);`
		* That last line causes recursion. It is the polite way to make the main loop.
* `function init()` - for one-time setup
	* Calls `reset()` - which doesn't do anything now
	* Sets `lastTime=DateTime.now();`
	* Calls `main()` to start the render/update loop.
* `function update(dt)` - called in the main game loop each iteration. Calls `updateEntities(dt)` once per.
	* there is also a commented out line: `//checkCollisions()`
	* seems like a good hint for where to handle engine-wide collision detection.
* `function updateEntities(dt)` - called by `function update(dt)`
	* calls the `update()` methods on the objects stored in global variables.
	* `allEnemies.forEach(function(enemy){enemy.update(dt);});`
	* `player.update()`
	* This isn't particularly flexible, but you could put everything with an `.update(dt)` into `allEnemies`
* `function render()` - does all the drawing
	* Stores the contents of the background in an array
		* each element is the tile that is repeated across an entire row
	* Clears the canvas with `ctx.clearRect(0,0,canvas.width,canvas.height)`
	* Loops over rows and cols and draws the background image.
	* Delegates the moving stuff to `function renderEntities()`
* `function renderEntities()` called by render once per.
	* like update, relies on `allEnemies` and `player` global variables
	* but it calls their `.render()` instead of `.update(..)`
	* an example `.render()` : `{ctx..drawImage(Resources.get(this.sprite), this.x, this.y);}`
* `function reset(){//noop};`

Other initialization:
```
Resources.load(['images/stone-block.png', 'images/water-block.png',...]);
Resources.onReady(init);
global.ctx  =  ctx;	//convenience (for app.js)
``` 

Usage
* Create a global `player` object with a `.render()` and `.update()` method
* Create a global `allEnemies[]` which contains objects that
	* have a `.render()` method and a `.update(dt)` method
* Listen to keyboard events
```
document.addEventListener('keyup', function(e) {
	var  allowedKeys  = {
		37:  'left', 38:  'up',	39:  'right', 40:  'down'}
	player.handleInput(allowedKeys[e.keyCode]);
});
```
* the `player` variable also has to have a `.'handleInput(allowedKey)` method to work with the above event listener.
* `ctx.drawImage(img, sX, sY, sWid, sHeight, x, y, Wid,Height)`
* 
.
.
.
.

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMzU1NDgzMDMxXX0=
-->