# JS Modules
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules

### Asside: `.mjs` vs `.cjs`
* `cjs` is an older node standard called **common js**
	* server-side only
	* uses `require` and `module.exports`
* `mjs` is the ECMA standard which is newer and used in most newer node projects
	* part of the language standard
	* uses `import` and `export` keywords
	* allows top-level `await`
	* built-in support for tree shaking
	* loads modules asynchronously
	* more flexibility in exporting values
	* more predictable handling of cyclic dependencies
* when to use `.js` vs `.mjs`
	* V8 recommends using `.js` for regular js and `.mjs` for modules
	* `.mjs` will be recognized as a module by node and such
		*  node can be configured to  use a `.js` module
	* some servers might not recognize the correct `Content-Type` for `.mjs` 
* html can use `<script type="module">` to mark that the script file is a module
## Example
### Basic structure
* `index.html`
* `main.js`
* `modules/`
	* `canvas.js`
	* `square.js`
### Contents
* `canvas.js`
	* `create()` creates a canvas with a specified `width` and `height` inside a `<div>` specified by ID which gets appended to the parent element
	* `createReportList()` creates an unordered list appended inside a specified wrapper element
	* returns the list's ID
* `square.js` 
	* `name` = "square"
	* `draw()`
## Exporting
* can export `var` `let` `const` `function` and classes
* all exports must be top level
* two syntaxes
	* prefix each export with the `export` keyword
		* `export const name = "square";`
	* one one line at the end of the file
		* `export {name, draw, reportArea };

## Importing
	* `import { name, draw, reportArea } from "./modules/square.js";`
		> **Note:** In some module systems, you can use a module specifier like `modules/square` that isn't a relative or absolute path, and that doesn't have a file extension. This kind of specifier can be used in a browser environment if you first define an [import map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules#importing_modules_using_import_maps).



> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTQxNjk4OTU5NywtMTg0NDI5MDg3NF19
-->