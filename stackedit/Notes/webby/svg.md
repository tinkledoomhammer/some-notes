## Intro

[Tutorial from MDN](https://developer.mozilla.org/en-US/docs/Web/SVG/Tutorial)
 * [Element Reference](https://developer.mozilla.org/en-US/docs/Web/SVG/Element)
 * [DOM interface reference](https://developer.mozilla.org/en-US/docs/Web/SVG/Element)

Versions of SVG
* 1.1 is the most recent "full" version. [Spec](https://www.w3.org/TR/SVG/)
* The second edition of SVG 1.1 became a recommendation from W3C in 2011
* 1.2 was discontinued in favor of the upcoming SVG 2.0 [Spec](https://www.w3.org/TR/SVG/)
* Since 2003, Tiny and basic versions have been devveloped, for mobile devices
* In 2008, SVG Tiny 1.2 became a recommendation
	* It does not include animations or other features that are intensive to implement.
* Different viewers have different degrees of support for CSS, animations, and other features.
### Other MDN references
[Elements](https://developer.mozilla.org/en-US/docs/Web/SVG/Element#svg_elements_by_category)
[Attributes](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute#svg_attributes_by_category)

## Getting Started

https://developer.mozilla.org/en-US/docs/Web/SVG/Tutorial/Getting_Started

### First example

```svg

<svg version="1.1"
     width="300" height="200"
     xmlns="http://www.w3.org/2000/svg">
  <rect width="100%" height="100%" fill="red" />
  <circle cx="150" cy="100" r="80" fill="green" />
  <text x="150" y="125" font-size="60" text-anchor="middle" fill="white">SVG</text>
</svg>
```
![Rendered](https://developer.mozilla.org/en-US/docs/Web/SVG/Tutorial/Getting_Started/svgdemo1.png)

### Basic Properties

1. Order of rendering: Later elements are drawn on top of earlier elements.
2. They can be embedded in several ways: 
	* XHTML and HTML both support the `<svg> `element being included directly.
	* An `<img>` element can be used
	* An object element: `<object data="img.svg' type='img/svg+xml"></object>`
	* An iframe: `<iframe src="img.svg"></iframe>`
	* It can also be created dynamically with javascript
3. Sizes and units are covered in [This tutorial](https://developer.mozilla.org/en-US/docs/Web/SVG/Tutorial/Positions)

### SVG file types

* `.svg` all lower case for standard files
	* HTTP headers should be:
```
Content-Type: image/svg+xml
Vary: Accept-Encoding
```

* `.svgz` for gzipped svg files.
	* Firefox cannot load them correctly from local files
	* Many user agents have trouble loading them from IIS.
	* The correct HTTP headers: 
```
Content-Type: image/svg+xml
Content-Encoding: gzip
Vary: Accept-Encoding
```
MDN has a [server configuration page](https://www.w3.org/services/svg-server/) for help.

## Positions
https://developer.mozilla.org/en-US/docs/Web/SVG/Tutorial/Positions

### The Grid
* `(0,0)` is the top left.
* Positive x is right
* Positive y is down

### Pixels
* By default, units are the same as the surrounding context. i.e. virtual pixels
* `<svg width="200" height="200" viewBox="0 0 100 100"> ...</svg>`
	* The `width` and `height` attributes are in the parent context's coordinates.
	* The `viewBox` attribute specifies the internal units that will be mapped into the parent coordinate system.
	* This example will "zoom in" or double the size of coordinates when converting from the internal svg to external page or whatever.
* The current mapping from 'user units' to 'screen units' is called the user coordinate system.
	* It can also be skewed, flipped, rotated, etc.
	* By default it maps one user pixel to one device pixel.
* If units like 'cm' are used in the user coordinate system, then the conversion factors appropriate to the device pixels will be used. 
	* In the above example, `1cm` in the user coordinate system will be converted to `2cm` in the device coordinate system.

### Angles
* angles seem to be in clockwise degrees.


## Basic Shapes
https://developer.mozilla.org/en-US/docs/Web/SVG/Tutorial/Basic_Shapes

### Combined example
```svg
<?xml version="1.0" standalone="no"?>
<svg width="200" height="250" version="1.1" xmlns="http://www.w3.org/2000/svg">

  <rect x="10" y="10" width="30" height="30" stroke="black" 
	  fill="transparent" stroke-width="5"/>
  <rect x="60" y="10" rx="10" ry="10" width="30" height="30"
	  stroke="black" fill="transparent" stroke-width="5"/>

  <circle cx="25" cy="75" r="20" stroke="red" fill="transparent"
	  stroke-width="5"/>
  <ellipse cx="75" cy="75" rx="20" ry="5" stroke="red" 
	  fill="transparent" stroke-width="5"/>

  <line x1="10" x2="50" y1="110" y2="150" stroke="orange" stroke-width="5"/>
  <polyline points="60 110 65 120 70 115 75 130 80 125 85 140 90 135 95 150 100 145"
	  stroke="orange" fill="transparent" stroke-width="5"/>

  <polygon points="50 160 55 180 70 180 60 190 65 205 50 195 35
	  205 40 190 30 180 45 180"
      stroke="green" fill="transparent" stroke-width="5"/>

  <path d="M20,230 Q40,205 50,230 T90,230" fill="none" stroke="blue"
	  stroke-width="5"/>
</svg>
```
![rendered](https://developer.mozilla.org/en-US/docs/Web/SVG/Tutorial/Basic_Shapes/shapes.png)

### `<rect>`
* Has 6 basic attributes that all default to `0`
* `x` and `y` the position of the top left corner of the rectangle
* `width` and `height` the size of the rectangle
* `rx` and `ry` the corner rounding radius.

### `<circle>`
* `cx` and `cy` the center of the circle
* `r` the radius of the circle

### `<elipse>`
* like `<circle>` but instead of `<r>` uses `<rx>` and `<ry>`

### `<line>`
```svg
<line x1="10" x2="50" y1="110" y2="150" stroke="black" stroke-width="5"/>
```
### `<polyline>`
* uses a single basic attribute, `points` to specify all the lines
* lines are connected one to another
* A polyline going from `(0,0)` to `(1,0)` to `(1,1)` to `(0,1)` would be:
	* `<polyline points='0,0 1,0 1,1 0,1' />`
	* or simply `<polyline points="0 0 1 0 1 1 0 1">`
* Valid separators are space, coma, EOL, or linefeed

### `<polygon>`
same as polyline except it adds a line back to the beginning, forming a closed shape.


## Paths
https://developer.mozilla.org/en-US/docs/Web/SVG/Tutorial/Paths

```svg
	<path d="M 0 0 h10 v10 h-10 v-10">
```
The `path` element uses a `d` attribute to encode a complex shape.
The `d` attribute is a string that is formatted as a sequence of {command param} substrings.
Upper case commands use absolute coordinates while lower-case commands use relative coordinates.
### Move command.

 `M x y` or `m dx dy` moves the cursor to the specified position without drawing anything.

### Line drawing commands.
`L x y` and `l dx dy` "Line to" commands draw a straight line to the specified point.
`H x` and `h dx` draw a horizontal line to the specified column.
`V y` and `v dy` draw a vertical line to the specified row.
`Z` or `z` draw a straight line back to the starting position. Usually used to end a path that describes a closed shape. The starting position seems to be where drawing starts.

### Curve Commands
* `C x1 y1, x2 y2, x y` and `c dx1 dy1, dx2 dy2, dx dy` create a cubic bezier curve from the current position to `x y` or `dx dy`. The other two points are control points.
* `S x1 y1, x y` and `s dx1 dy1, dx dy` infer a control point to simplify/shorten series of bezier curves. The assumed control point is a reflection of the control point used in the previous cubic bezier curve.  If the previous command is not `c` `s` `C` or `S` then the cursor position is used instead.
* `Q x1 y1, x, y` and `q dx1 dy1, dx dy` specify quadratic bezier curves (which use a single control point). `x y` or `dx dy` is the end point while `x1 y1` specify the control point.
* `T x y` and `t dx dy` are a shorthand for continuing quadratic bezier curves.  These commands infer a control point that is a reflection of the previous control point.

* `A rx ry x-axis-rotation large-arc-flag sweep-flag x y` and `a rx ry x-axis-rotation large-arc-flag sweep-flag dx dy` draw arcs (which can be circular or elliptical).
	* `rx` and `ry` are the radii of the unrotated ellipse. they should be equal for circles
	* `x-axis-rotation` is in degrees clockwise.
	* `large-arc-flag` should be `0` if the arc is less than $180\degree$
		* or `1` if the angle of the arc is $>10-\degree$
	* `sweep-flag` determines which circle/ellipse the arc should be taken from.
		* should be `0` if the center of thee arc is to the left of the arc (i.e. counter-clockwise)
		* or `1` if the center of the arc is to the right (i.e. clockwise)
			* I'm not sure about this flag because the examples are all from left to right.
	* ![Arc explanation diagram](https://developer.mozilla.org/en-US/docs/Web/SVG/Tutorial/Paths/svgarcs_flags.png)	   			

```svg
	 <svg width="325" height="325" xmlns="http://www.w3.org/2000/svg">
	  <path d="M 80 80
           A 45 45, 0, 0, 0, 125 125
           L 125 80 Z" fill="green"/>
	  <path d="M 230 80
           A 45 45, 0, 1, 0, 275 125
           L 275 80 Z" fill="red"/>
	  <path d="M 80 230
           A 45 45, 0, 0, 1, 125 275
           L 125 230 Z" fill="purple"/>
	  <path d="M 230 230
           A 45 45, 0, 1, 1, 275 275
           L 275 230 Z" fill="blue"/>
	</svg>
```

## Fill and Stroke Attributes
https://developer.mozilla.org/en-US/docs/Web/SVG/Tutorial/Fills_and_Strokes


### Painting
* both `fill` and `stroke` attributes take color values.
	* They can use the same options as html, css, etc
	* Some viewers support rgba, but there is a separate `fill-opacity` and `stroke-opacity` attributes for that.
* `stroke` refers to lines (or outlines)
* `fill` refers to the color filled inside the lines

### Strokes
* `stroke-width` specifies the width of the lines. They are centered around the path.
* `stroke-linecap` specifies how the stroke ends. the options are 
	* `stroke-linecap = "butt"` - no linecap, the stroke ends at the end of the path
	* `stroke-linecap = "square"` - like `="butt"` except that it extends the stroke beyond the path by a distance `stroke-width / 2`
	* `= "round"` - adds rounded ends with a diameter = `stroke-width`
	* ![illustration](https://developer.mozilla.org/en-US/docs/Web/SVG/Tutorial/Fills_and_Strokes/svg_stroke_linecap_example.png)
* `stroke-linejoin` controls how the joint between lines are drawn
	* `stroke-linejoin = "miter"` extends the line to create a single point
	* `= "round"` creates a rounded cap
	* `= "bevel"` creates a flat edged extension to ease the transition (two angles)
* `stroke-dasharray = "n1,n2,n3"` creates dashed lines
	* The comas are necessary, white space is ignored
	* the first number (and every odd^th^ number ) is the length of a dash.
	* The second (and subsequent even^th^ ) number is the length of a gap
	* The pattern loops as many times as needed to complete the line.

### Using CSS
* attributes that are properties can be modified with CSS. Others cannot.
* CSS can be added to an individual element with the `style=` attribute. 
	* `<rect x="10" height="180" y="10" width="180" style="stroke: black; fill: red;"/>`
* CSS files can be included like a normal stylesheet for html:
	* `<?xml-stylesheet type="text/css" href="style.css"?>` after the `<?xml version...>` line.
* Css can also be included within a `<style>` element within a `<defs>` element.
	* The `<defs> ... </defs>` element creates a block of elements that are not displayed immediately but instead saved for use later.
```svg
<?xml version="1.0" standalone="no"?>
<svg width="200" height="200" xmlns="http://www.w3.org/2000/svg" version="1.1">
  <defs>
    <style><![CDATA[
       #MyRect {
         stroke: black;
         fill: red;
       }
    ]]></style>
  </defs>
  <rect x="10" height="180" y="10" width="180" id="MyRect"/>
</svg>
```

## Gradients
https://developer.mozilla.org/en-US/docs/Web/SVG/Tutorial/Gradients

### `linearGradient`
* Deffined within a `<defs>`
* Must have an `id=`
* 
```svg
	<linearGradient id="Gradient2" x1="0" x2="0" y1="0" y2="1">
      <stop offset="0%" stop-color="red" />
      <stop offset="50%" stop-color="black" stop-opacity="0" />
      <stop offset="100%" stop-color="blue" />
    </linearGradient>
```
* the `stop-color` attribute is a property and can be specified in CSS by assigning a `class=` to each stop.
* They are used by assigning  `fill="url(#Gradient2)"` to a render-able object.
	* `stroke=` also works
* the `stop-opacity` attribute is also available.
* The other attributes of `linearGradient` specify the orientation of the gradient
	* By default it goes from left to right.
	* Otherwise it goes from `x1 y1` to `x2 y2`
* `xlink:href` attribute can be used to copy attributes and stops from one gradient to another.
```svg
xmlns:xlink="http://www.w3.org/1999/x.ink"
xlink:href="#Gradient1"
```

### Radial Gradient
* similar to radial gradients except that it uses a center, focus, and radius.
* `<radialGradient id="Gradient" cx="0.5" cy="0.5" r="0.5" fx="0.25" fy="0.25">`
	* defines the center at 0.5,0.5 (the center of the fill) and the focus at 0.25,0.25. 
	* The focus is the 0% point of the gradient. (i.e. chromatic center)
	* The center is the geometric center of the gradient.
	* The focal point must be inside the circle.
### Other gradient attributes
* `gradientUnits=` specifies which coordinate system to use.
	* `gradientUnits = 'objectBoundingBox'` is the default 0-1 in both dimensions
	* `= 'userSpaceOnUse'` uses absolute coordinates (relative to the whole svg), so offsetting may be required
* `gradientTransform=` will be covered in the section on transforms
* `spreadMethod=` attribute specifies what happens after the end  of the gradient coordinates (i.e. beyond the radius of the circle of a radial gradient)
	* `='pad'` is the default. It uses the value for 100% for the remainder of the fill
	* `='reflect'` goes back down from 1 to 0, then back up again, down again, etc as needed
	* `='repeat'` goes from 1 immediately back to 0 and then slowly up again.

### Patterns
https://developer.mozilla.org/en-US/docs/Web/SVG/Tutorial/Patterns

* like gradients, patterns go in the `<defs>` section
* they contain shapes to be drawn
* they use two coordinate systems
	* `patternUnits=` works like `gradientUnits`, by default `patternUnits = 'objectBoundingBox'`
		* `= userSpaceOnUse` also works
	* `patternContentUnits=` has the same options, but defaults to `='userSpaceOnUse'`
* `width = ` and `height = ` attributes control how often the pattern is repeated. They are both in `patternUnits=`
	* When `patternContentUnits = 'userSpaceOnUse'` and `patternUnits = 'objectBouningBox'`(the default)
		* The size of the pattern that gets repeated is the object's `width=` * the pattern's `width=`.
		* anything drawn outside that area within the pattern's definition will be clipped
		* so the number of horizontal repetitions will be $1/pattern.width$
* `x = ` and `y = ` are used to offset the beginning of the pattern. It is scaled in `patternUnits =` so by default, 0=left, top


## Text
### Elements
* `<text> text to display</text`
	* `x=`,`y=`, set the location of the viewport for the text
	* `text-anchor = ` determines the direction text will flow
		* can be `="start"`, `="middle"`, `="end"` or `="inherit"`
	* `dominant-baseline =` decides vertical alignment
		*  `="auto"` , `="middle"` or `="hanging"`, and a few more
		* See this [mdn article](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/dominant-baseline) for more info
	* `fill=` and `stroke=` work like other shapes.
* `<tspan>text</tspan>`
	* Used inside of `<text>` or `<tspan>` to have different formatting, etc
	* `x=` and `y=` attributes override the placement of the span
		* They can take a list of values that will be applied to each character
	* `dx=` and `dy=` work the same as above, but use coordinates relative to where the span would have started if these attributes weren't provided.
	* `rotate=` rotates characters (in degrees, clockwise)
		* can also take a list of values.  If there are more characters in the string than values in the list, then the last value will be applied to the remaining characters
	* `textLength=` is meant to allow the rendering engine to fine-tune the positions of glyphs when it's measurements don't match the length provided by this attribute.
* `<textPath xmlns:xlink="http://w3.org/1999/xlink" xlink:href="#my_path"> sometext </textPath>`
	* This text element has a baseline that conforms to an arbitrary path, linked by id
### Font properties
*  [font-family](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/font-family)
 * [font-style](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/font-style)
 *  [font-weight](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/font-weight)
 *  [font-variant](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/font-variant)
 *  [font-stretch](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/font-stretch)
 *  [font-size](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/font-size)
 *  [font-size-adjust](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/font-size-adjust)
 *  [kerning](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/kerning)
 *  [letter-spacing](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/letter-spacing)
 *  [word-spacing](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/word-spacing)
 *  [text-decoration](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/text-decoration)

## Basic Transformations
https://developer.mozilla.org/en-US/docs/Web/SVG/Tutorial/Basic_Transformations

### the `<g> </g>` element
* used to group elements

### the `transform="..."` Attribute
* `transform="translate(x,y)"` translates the element
* `="ratate(theta)"` rotates the element (degrees clockwise)
* `="skewX"` and `="skewY"` both take angular arguments
* `="scale(x,y)"` stretches the element
	* if `y` is not provided then it is assumed to be `=x`
* `="matrix(a,b,c,d,e,f)"` the arguments define a 2x3 transformation matrix
	* $x_n = a x_o +c y_o+e$
	* $y_n=b x_o + d y_o +f$
* Transformations can be concatenated by combining the strings:
	* `transform="translate(10,20) rotate(45)"`
		* rotates $45\degree$ clockwise
		* then translates $[10,20]$

### How do transformations work with `userSpaceOnUse`?
### Embedding `<svg>` within `<svg>...</svg>`
* Specifying `width=`, `height=` and `viewBox=`, a new coordinate system can be created

## Clipping and Masking
https://developer.mozilla.org/en-US/docs/Web/SVG/Tutorial/Clipping_and_masking

* Masking uses alpha blending while clipping is all-or-none.
* both should be defined in a `<defs>..</defs>` section
### `<clipPath id=...>...</clipPath>`
* should be assigned an `id=` so that it can be used 
* used by assigning a `clip-path="url($clip-path-id)"` attribute on the object to be clipped
* Only the parts of the object being drawn that are overlapped by drawn pixels of the clipPath will be drawn
###  `<mask id=...> ... </mask>`
* apply it to an element (mask the element) by assigning `mask="url(#mask-id)"`
* the luminance value of each pixel in the mask will be used as the alpha (transparency) of the object being drawn
* Transparency for an entire element can be achieved using the `opacity="` attribute
### Well-known CSS techniques
* the css property `display=`
	* use `display: none` to hide an element entirely
	* the default value is `inline`
* CSS 2 also defines `visibility` and `clip`

## Other content in SVG
https://developer.mozilla.org/en-US/docs/Web/SVG/Tutorial/Other_content_in_SVG

```svg
<svg
  version="1.1"
  xmlns="http://www.w3.org/2000/svg"
  xmlns:xlink="http://www.w3.org/1999/xlink"
  width="200"
  height="200">
  <image
    x="90"
    y="-65"
    width="128"
    height="146"
    transform="rotate(45)"
    xlink:href="https://developer.mozilla.org/en-US/docs/Web/SVG/Element/image/mdn_logo_only_color.png" />
</svg>
```

### `<image />`
* Can use `transform=` and whatnot
* Requires `xmlns:xlink="http://www.w3.org/1999/xlink"`

### `<foreignObject>`
* support for the content varies between implementations
* could be HTML
* "Its sole purpose is to be a container for other markup and a carrier for SVG styling attributes (most prominently `width` and `height` to define the space the object will take)".

## Filter effects
### Example:
```svg
<svg
  width="250"
  viewBox="0 0 200 85"
  xmlns="http://www.w3.org/2000/svg"
  version="1.1">
  <defs>
    <!-- Filter declaration -->
    <filter
      id="MyFilter"
      filterUnits="userSpaceOnUse"
      x="0" y="0" width="200" height="120">
      <!-- offsetBlur -->
      <feGaussianBlur in="SourceAlpha" stdDeviation="4" result="blur" />
      <feOffset in="blur" dx="4" dy="4" result="offsetBlur" />

      <!-- litPaint -->
      <feSpecularLighting
        in="blur"
        surfaceScale="5"
        specularConstant=".75" specularExponent="20"
        lighting-color="#bbbbbb"
        result="specOut">
        <fePointLight x="-5000" y="-10000" z="20000" />
      </feSpecularLighting>
      <feComposite
        in="specOut"
        in2="SourceAlpha"
        operator="in"
        result="specOut" />
      <feComposite
        in="SourceGraphic"
        in2="specOut"
        operator="arithmetic"
        k1="0" k2="1" k3="1" k4="0"
        result="litPaint" />
      <!-- merge offsetBlur + litPaint -->
      <feMerge>
        <feMergeNode in="offsetBlur" />
        <feMergeNode in="litPaint" />
      </feMerge>
    </filter>
  </defs>

  <!-- Graphic elements -->
  <g filter="url(#MyFilter)">
    <path
      fill="none"
      stroke="#D90000"
      stroke-width="10"
      d="M50,66 c-50,0 -50,-60 0,-60 h100 c50,0 50,60 0,60z" />
    <path
      fill="#D90000"
      d="M60,56 c-30,0 -30,-40 0,-40 h80 c30,0 30,40 0,40z" />
    <g fill="#FFFFFF" stroke="black" font-size="45" font-family="Verdana">
      <text x="52" y="52">SVG</text>
    </g>
  </g>
</svg>
```
### `<filter>...</filter>`
* Contains [filter primitive elements](https://developer.mozilla.org/en-US/docs/Web/SVG/Element#filter_primitive_elements)
* attributes include `x=`, `y=`, `width=`, `height=`,  and `xlink:href=`
* `id=` must be specified to use the filter
* `filterUnits=`, `primitiveUnits=` use `=objectBoundingBox` and `userSpaceOnUse`
* Filters are connected to the object being rendered through the filter by assigning the `filter="url($myfilter)"` 

### Buffers
* Many filter primitive attributes use buffers as values
* There is a list of pre-defined buffers
	* `SourceGraphic` the graphics elements that are assigned to the filter with the `filter=` attribute
		* This is the default `in=` for the first primitive within a filter
	* `SourceAlpha` the alpha channel of the source (the element with the `filter=` attribute
	* `BackgroundImage` an image snapshot of the SVG document under the filter region
		* This is not supported in modern browsers
	* `BackgroundAlpha` the alpha channel of the SVG document beneath the filter region
	* `FillPaint` the value of the `fill=` property of the target element. Usually opaque
	* `StrokePaint` The value of the `stroke=` property of the target element.
* Custom names can also be used.

### Filter Primitives
* most have one or more `in=` attributes that refer to a source buffer
	* the first one defaults to `SourceGraphic`
	* subsequent primitives `in=` defaults to the `result=` of the previous primitive
* `result=` also refers to a buffer that stores the result of the primitive
	* if no value is specified then the result of the primitive is only available as the implicit input of the next filter primitive
	* their names start with `fe`
	












> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE4ODg1ODUzNzldfQ==
-->