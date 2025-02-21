# JavaScript and the DOM
## The Document Object Model

characters -> tags ->tokens -> nodes -> DOM

### Selecting elements
`document.getElementById(id)` - takes the id without the `#`.
* Returns `null` if there is no such element

`document.getElementsByClassName(clsname)`
`document.getElementsByTagName(tagname)`
The plural functions return an array-like collection of type `HTMLCollection`

#### Query Selectors
* `Element.querySelector(query)` - uses CSS selectors (like jQuery)
	* Returns a `Node` object representing the first match
	*  [https://developer.mozilla.org/en-US/docs/Web/API/Element/querySelector]
* `element.querySelectorAll(query)` - returns a `NodeList`
	* [https://developer.mozilla.org/en-US/docs/Web/API/Element/querySelectorAll]

### DOM interfaces
A complete list: [https://developer.mozilla.org/en-US/docs/Web/API]
* EventTarget - super of Node
	* [https://developer.mozilla.org/en-US/docs/Web/API/EventTarget]
* Node
	*  [https://developer.mozilla.org/en-US/docs/Web/API/Node]
	* `node.textContent`

#### Element - inherits from Node
* [https://developer.mozilla.org/en-US/docs/Web/API/Element]
* `element.hasClass(classname)`
* `element.outerHTML`
* `element.getElementsByClassName()` - like the one on `document` but it only returns children
* `element.firstElementChild` property 
* `element.parentElement` property - needed to remove children



## Creating and Editing Content with JavaScript

#### `element.innerHTML` property
* contains ALL the text that is enclosed in the element's tag
* `element.outerHTML` is the same but also includes the tagset that created the element
* both are assignable strings
* Assignments that include HTML tags will create child elements.

#### `element.textContent`
* Like `.innerHTML` but strips all tags, attributes, etc
* It can be assigned, which will remove all child elements.
* Assigning a value that includes HTML tags will not create child elements. The tags themselves will be printed.
* `element.innerText` is the same, but applies CSS
	* so `display: none` text is missing
	* CSS capitalization rules are applied.
	* `.innerText` is new and may not be fully supported in all browsers
* There is a `document.createTextNode(txt)` method that can be used with `.appendChild(..)`
	* but it is largely obsolete because of `.textContent`

#### `document.createElement(tagname)`
* returns an `element` or `HTMLUnknownElement`  if the tag is unknown
* does NOT add the element to the page

#### `element.appendChild(element)`
* The method is NOT defined on `document`
*  If the element is already in the DOM, then it will be RELOCATED	to the new location
* An element can only have one location in the DOM tree.

#### `element.insertAdjacentHTML(where,htmlText)`
* Works with HTML in text format rather than DOM elements
* The first parameter determines where to put it:
	* `'beforebegin'` - before the opening tag
	* `'afterbegin'` - after the opening tag (i.e. first child)
	* `'beforeend'` - before the closing tag (i.e. last child)
	* `'afterend'` - after the closing tag (i.e. first sibling)
* The method parses the text into nodes and appends them.
* This prevents the corruption of the tags that are already there
* `'beforebegin'` and `'afterend'` only work if the element is in the DOM and has a parant.

#### `element.removeChild(child-element)`
[removeChild docs](https://developer.mozilla.org/en-US/docs/Web/API/Node/removeChild)
* May require `element.parentElement` 
#### `element.remove()`

### `Node.insertBefore(newNode, refNode)`
<https://developer.mozilla.org/en-US/docs/Web/API/Node/insertBefore>
If the `refNode` is `null` then it is inserted at the end.




### Style page content

#### `element.style` attribute
* Used by assigning or reading it's properties
* i.e. `element.style.<property>=<value`
* `document.getElementById('#mything').style.color='red';`
* It can't handle hyphens in property names, so omit them
	* `.fontsize` rather than `.font-size`

#### `element.style.cssText` property
* Used to assign multiple properties at once.
* The text is in a string, so normal CSS property names are used.

#### `element.setAttribute(attr, val)` method
* Can be used to set styles, id, etc.
#### `element.className` and `element.classList`properties
* `element.className`
	* A space-separated string of CSS classes that apply to the element.
	* Use `str.split(' ')` to break it into an array
* `element.classList` is an array-like `DOMTokenList`
	* defines methods for editing the element's classes:
	* `.add(cls)`, `.remove(cls)`, `.toggle()`, `.contains()`

### Further Research
-   [style on MDN](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/style)
-   [Article: Using dynamic styling information](https://developer.mozilla.org/en-US/docs/Web/API/CSS_Object_Model/Using_dynamic_styling_information)
-   [DOM methods to control styling](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference#DOM-CSS_CSSOM)
-   [nextElementSibling on MDN](https://developer.mozilla.org/en-US/docs/Web/API/NonDocumentTypeChildNode/nextElementSibling)
-   [className on MDN](https://developer.mozilla.org/en-US/docs/Web/API/Element/className)
-   [classList on MDN](https://developer.mozilla.org/en-US/docs/Web/API/Element/classList)
-   [Specificity on MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity)
-   [Article: CSS Specificity: Things You Should Know](https://www.smashingmagazine.com/2007/07/css-specificity-things-you-should-know/)


## Working with Browser Events
Check out the documentation on the Chrome DevTools site: [monitorEvents documentation](https://developers.google.com/web/tools/chrome-devtools/console/events#monitor_events)
* `monitorEvents()` will start logging every event in the console
* `unmonitorEvents()` will stop logging

MDN [list of events](https://developer.mozilla.org/en-US/docs/Web/Events)

### Event Listener methods
#### `target.addEventListener('eventType',callback(e))`
* Invoked on the target, i.e. the object being clicked or whatever.
	* Usually `document` or an `element`
	* There is an optional (and not yet widely supported) third
		* that can be used to make the event one-off
* The callback receives an event object when fired.
#### `target.removeEventListener('eventType',callback)`
* Requires maintaining a reference to the callback that was registered.

#### `.dispatchEvent()`

### Event Phases
1. The **capturing** phase
	* Lets a parent intercept an event before it reaches a child
	* Starts at `<html>` and works it's way down the DOM
	* Transitions to 'at target' phase when it reaches the child-est element 
		* i.e. the thing that was actually clicked.
	* Events can be registered to fire during this phase if their third argument is `true`
		* `target.addEventListener('eventType',callback(e),true)` 
2. The **at target** phase
	* Where most event handlers are fired
	* Only one element can fire event handlers in this phase: the child-most one.
3. the **bubbling** phase
	* When the bottom most child doesn't have a handler
	* It's parent is checked
	* Propagates upward until a handler is found or there is no higher level
	* This is the default phase for event handlers to fire.

### The Event Object
`event.preventDefault` property
`event.target` property
`event.target.nodeName` property - a capitalized string of the tag i.e. "P" or "SPAN"

### Knowing when the DOM is ready
* Scripts at the end of the `body` will run after the 	DOM is generated.
* Scripts in the `<head>` will run much sooner (ASAP?)
* `document` fires a `'load'` event when all resources are loaded.
* And also a `'DOMContentLoaded'` event once the DOM is ready. 
	* This will be before the `'load'` event. 
* In chrome dev tools, the network tab provides info on the timing of these events.
	* The status bar shows times in ms
	* In the graphs, there is a blue line for `'DOMContentLoaded'` 
	* `'load'` is basically the length of the graphs.


### Further Research

* [Event dispatch and DOM event flow](https://www.w3.org/TR/DOM-Level-3-Events/#event-flow)  on W3C
	*  [capture phase](https://www.w3.org/TR/DOM-Level-3-Events/#capture-phase)  on W3C
    *  [target phase](https://www.w3.org/TR/DOM-Level-3-Events/#target-phase)  on W3C
    *  [bubble phase](https://www.w3.org/TR/DOM-Level-3-Events/#bubble-phase)  on W3C
*   [Event](https://developer.mozilla.org/en-US/docs/Web/API/Event)  on MDN
*  [Event reference](https://developer.mozilla.org/en-US/docs/Web/Events)  on MDN
*   [addEventListener](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener)  on MDN

-   [Article: Event delegation](https://javascript.info/event-delegation)
-   [Article: How JavaScript Event Delegation Works](https://davidwalsh.name/event-delegate)


## Performance
### Testing code performance with `performance.now()`
* The method returns the time in ms, usually with around 5us precision.
* [performance.now() on MDN](https://developer.mozilla.org/en-US/docs/Web/API/Performance/now)
* The 0 time is when the page was loaded.

### Document fragments
* Adding elements to the DOM causes reflow and repaint.
* You can add children to a `<div>` that is not attached to the document
	* And then attach the `<div>` after all of its children have been added and created.
* Or to `document.createDocumentFragment()` 
	* This works about the same, but doesn't create an extra `<div>`
	* -   [DocumentFragment Interface on MDN](https://developer.mozilla.org/en-US/docs/Web/API/DocumentFragment)

### Managing Reflow/Repaint
* Hiding elements doesn't cause a reflow
* So hide the parent element, make all changes, then unhide the parent.

### Break up long-running processes
* The JS concurrency model is single threaded.
* Events cannot fire while JS code is running
* Use `setTimeout(callback,timeInMiliS)` to breake up long jobs.
* If the time argument is `0` then the call back will be placed in the event queue immediately
	* and run after the running code exits
	* and after any other pending events that are earlier in the queue.

### Further Research
-   [Website Performance Optimization](https://www.udacity.com/course/website-performance-optimization--ud884)  course by Udaicty
-   [Minimizing browser reflow](https://developers.google.com/speed/articles/reflow)  from PageSpeed Tools Guides
-   [Avoid Large, Complex Layouts and Layout Thrashing](https://developers.google.com/web/fundamentals/performance/rendering/avoid-large-complex-layouts-and-layout-thrashing)  from Google's Web Fundamentals Guides
-   [Performance Analysis Reference](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/reference#rendering)  from Google's Web Fundamentals Guides
-   Article  [Reflows & Repaints: CSS Performance Making Your JavaScript Slow?](http://www.stubbornella.org/content/2009/03/27/reflows-repaints-css-performance-making-your-javascript-slow/)


> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE3MDg3OTU4OTJdfQ==
-->