
Resizing: 
```javascript
function resize(canvas)  {
  var realToCSSPixels = window.devicePixelRatio; 
  // Lookup the size the browser is displaying the canvas in CSS pixels
  // and compute a size needed to make our drawingbuffer match it in
  // device pixels.
  var displayWidth =  Math.floor(canvas.clientWidth * realToCSSPixels);
  var displayHeight =  Math.floor(canvas.clientHeight * realToCSSPixels);
  //or, to just use CSS pixels (i.e. for performance)
  // var displayWidth = canvas.clientWidth; 
  // Check if the canvas is not the same size.
  if  (canvas.width !== displayWidth || canvas.height !== displayHeight)  {
    // Make the canvas the same size
    canvas.width = displayWidth; canvas.height = displayHeight;
  }
}
```
* The above can be called on every draw loop
* The `canvas.width` and `.height` properties are the pixel size of the memory buffer. the `canvas.clientWidth` and `.clientHeight` are the size of the HTML element in CSS Pixels.  
* The above is the highest internal resolution that makes any sense.

An engine might also want to limit the buffer pixel size based on the resolution of images being drawn and the 'zoom level' selected. I.e. The app decides how much it wants on the canvas and the engine should limit the buffer size rather than scaling up images when they are drawn into the buffer.

Pixel sizes:
1. Buffer pixels
	* `canvas.width, canvas.height`
2. CSS pixels
	* `canvas.clientWidth, canvas.clientHeight`
3. Logical (source image) pixels
	* used with `ctx.drawImage(img, sX, sY, sWid, sHeight, x, y, Wid,Height)`
	* might be different for each source image/resource
4. Screen pixels
	* `window.devicePixelRatio` * css pixels

Other coordinates:
* cells/ tiles
* 0..1 bounding ("viewport coords")
How does an app engine handle changing viewport size and orientation? What about covering part of it?
	* The app should internally use cell coordinates everywhere
	* The viewport should be set in cells
	* `sizeRule={landscape: {width: [min,max]}, height: [min,max]}, `
	* ` portrait: {width: [min,max], height: [min,max]}, `
	* ` default: 'landscape'};`
	* coordinate conversions could be swap x and y for one mode.
		* i.e. if `sizeRule.defualt` is 'landscape' but the screen is in portrait, 
		* experiment with this idea
	* coordinate conversions should be object-type specific to get the correct offsets.

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMzgxNTAxODU3XX0=
-->