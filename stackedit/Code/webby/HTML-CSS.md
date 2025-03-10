# Intro to HTML and CSS
[Udacity ud304](https://classroom.udacity.com/courses/ud304)

## HTML, CSS, and Boxes
* DOM elements include:
	* the opening and closing tag
	* the contents
### CSS
```
selector {
	attribute:value;
}
```

<https://developers.google.com/web/updates/2016/02/css-variables-why-should-you-care>

### Doc Type and such
```
<!DOCTYPE html>
<html>
	<head>
		Meta information goes here!
	</head>
	<body>
		Content goes here!
	</body>
</html>
```
### Important `<head>` stuff
1. `<title>A title that shows up in the browser tab</title>`
2. `<link rel="stylesheet" type="text/css" href="style.css">`
3. `<script src="animations.js">` (?)
4. `<meta charset="utf-8">`
5. seo `<meta name='description' content='This is a description for search engines>`




* [How CSS Selectors work](http://css-tricks.com/how-css-selectors-work/)
* [MDN CSS Reference](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference)


### Box Model

* content
* padding
	* around the content
	* affected by the background of the box
* border
	* inherited from color property of box
* margin
	* the space between boxes
	* doesn't have its own color
* border+padding+content=size
	* i.e. width=content.width+2*border+t*padding

Css sizing rules - allows you to specify the outer box size
```
*{
	-webkit-box-sizing: border-box;
	-moz-box-sizing: border-box;
	-ms-box-sizing: border-box;
	box-sizing: border-box;
}
```
## HTML Tags Quickref
`<html>`
: The top-level element

### Document Metadata

`<base>`
: Specifies the url to use as the base for relative urls.

`<head>`
: container for general metadata

`<link>`
: Specifies relationships between the current document and external resources. Used to link in stylesheets.

`<meta>`
: Represents other metadata that cannot be specified with other metadata elements.

`<style>`
: Contains style information for the document or part of the document.

`<title>`
: Defines the title of the document. Contained tags are ignored.

### Content sectioning

`<address>`
: suplies content information for it's nearest ```<article> or <body>``` ancestor. 

`<article>`
: Represents a self-contained composition that can be independently distributed (i.e. syndicated). Eg: a forum post, a magazine or newspaper article, or a blog entry.

`<asside>`
: Represents a section of a document with content connected tangentially to the main document (usually presented as a sidebar).

`<footer>`
: represents a footer for the nearest sectioning content or sectioning root element.  Typically contains information about the author of the section, copyright data, or links to related documents.

`<h1>-<h6>`
: Heading levels

`<header>`
: represents a group of navigational aids. May also contain things like a logo, search form, etc.

`<nav>`
: represents a section of a page whose purpose is to provide navigation links, either within the current document or to other documents.  Commonly used for menus, tables of contents, and indexes.

`<section>`
: represents a standalone section of functionality contained within a document.  Typically with a heading. Used when there is not an appropriate element that is more specific.

### Text Content

`<blockquote>`
: Indicates that the enclosed text is an extended quotation. The **cite** attribute can give a URL for the source, while a `<cite>` element can give a text representation of the source.

`<dd>`
: A description of a term in a description list ```<dl>```.

`<div>`
: a generic container for flow content. It is used to group elements for styling (**class** or **id** attributes) or marking a section in a different language (**lang** attribute), etc.

`<dl>`
: A description list. Contains key-value pairs

`<dt>`
: The terms in a ```<dl>```. Can only occur as a child element of that tag. It is usually but not necessarily followed by a ```<dd>```.

`<figure>`
: represents self-contained content, usually with a ```<figcaption>```, and is usually referenced as a single unit.

`<figcaption>`
: used once inside a ```<figure>...</figure>```.

`<hr>`
: A thematic break between paragraph level elements.

`<li>`
: used to denote individual items inside an ```<ol>, <ul>, or <menu>```.

`<main>`
: Used to denote the main content of a the body of the document.  The w3c doesn't allow it to descend from ```<article>, <aside>, <footer>, <header>, or <nav>```.

`<ol>`
: An ordered list, each of which gets an ```<li>...</li>```

`<p>`
: A paragraph of text.

`<pre>`
: preformatted text. Usually used with a fixed-width ("monospace") font. Whitespace inside is displayed as typed.

`<ul>`
: An un-ordered list.  Like an ordered list but without numbering.

### Inline text semantics

`<a>`
: an anchor. Used to create hyperlinks.

`<abbr>`
: abbreviation. i.e. ```<abbr title="Internationalization">I18N</abbr>```

`<b>`
: represents a span of text stylistically different from normal text without conveying any special relevance.  Usually rendered in bold.

`<bdi>`
: bidirectional isolation - isolates a span of text that might be formatted in a different direction from other text outside it.

`<bdo>`
: bidirectional override - used to override the current directionality of text.

`<br>`
: A blank carriage-return. Useful for poems and addresses.

`<cite>` represents a reference to a creative work.  Must include the title of a work or a URL reference, which may be abbreviated according to the conventions used for the addition of citation metadata.

`<code>`
: represents a fragment of computer code.  By default it renders in monospace.

`<data>`
: links a given content with a machine-readable translation. if the content is time- or date- related, the ```<time>``` element must be used.

`<dfn>`
: represents the defining instance of a term.

`<em>`
: marks text that has stress emphasis. It can be nested.

`<i>` 
: represents a range of text that is set off from the normal text for some reason. i.e. technical terms, foreign language phrases, or the thoughts of fictional characters.  Usually displayed in italics.

`<kbd>`
: represents user input and produces an inline element displayed in the browser's default monospace font.

`<mark>`
: represents highlighted text. i.e. a run of text marked for reference purpose.

`<q>`
: indicates that the enclosed is a short inline quotation. Used for short quotes that don't require paragraph breaks.

`<ruby>`
: represents a ruby annotation, for showing the pronunciation of east asian characters.

`<s>`
: renders text with a strikethrough. Not intended for showing document edits, which should use ```<ins> and <del>```

`<samp>`
: is intended to identify sample output from a computer program. Usually displayed in the default monospace.

`<small>`
: the enclosed text is one font size smaller. In HTML5, it is repurposed ot represent side-comments and small print, like copyright and legal text, independent of styled presentation.

`<span>`
: a generic inline container for phrasing content, which does not inherently represent anything. Used to group elements for styling (**class** or **id** attributes) or in a different language (**lang** attrubute).

`<strong>`
: used to give text strong importance, usually displayed in bold.

`<sub>`
: subscript - used for text that should be displayed lower and smaller than normal.

`<sup>`
: superscript - used for text that should be displayed higher and smaller than normal.

`<time>`
: represents a time on a 24-hour clock or a precise date on the Gregorian calendar, optionally with time-zone information.

`<u>`
: renders the enclosed with an underline. "In HTML5, this element represents a span of text with an unarticulated, though explicitely rendered, non-textual annotation, such as labeling the text as being a proper name in Chinese text (a Chinese proper name mark), or lebeling the text as being misspelled.

`<var>`
: represents a variable in a mathematical expression or a programming context.

`<wbr>`
: a word break opportunity--a position where the browser may break a line, even if its own line-breaking rules say otherwise.

### Embedding and media

`<area>`
: defines a hot-spot region on an image, and optionally associates it with a link. Only used in a ```<map>``` element.

`<audio>`
: used to embed sound content in documents. 

`<img>`
: represents an image

`<map>`
: used to define an image map

`<track>` 
: subtitles in .vtt format, must be a child element of ```<audio> or <video>``` 

`<video>`
: used to embed video content.


`<embed>`
: an integration point for an external application (or interactive content). For plugins.

`<object>`
: an external resources that can be treated as an image, a nested browsing context, or a resource to be handled by a plugin.

`<param>`
: defines parameters for an ```<object>``` element.

`<source>`
: specifies multiple media resources for ```<picture>, <audio>, or <video>``` elements. 

### Scripting and edits

`<canvas>`
: used with the canvas scripting api for graphics and animation.

`<noscript>`
: used as a fallback. Its's contents are only inserted when the script is not supported or turned off.

`<script>`
: Used to embed or reference an executable script.

`<del>`
: marks a range of text that has been deleted from the document. Usually rendered with strike-through text.

`<ins>`
: marks a range of text that has been added to the document.

### Tables

`<caption>`
: The title of a table. Must be the first descendant of the table, but may be moved with CSS.

`<col>`
: Defines a column within a table, used to define common semantics on all common cells. Usually found within a ```<colgroup>``` element.

`<colgroup>`
: defines a group of columns

`<table>`
: the parent element used to represent 2d data.

`<tbody>`
: groups one or more ```<tr>``` elements

`<td>`
: defines a cell that contains data.

`<tfoot>`
: defines a set of rows summarizing the columns of the table.

`<th>`
: defines a cell as header of a group of cells. The nature of the group is defined by the **scope** and **headers** attributes.

`<thead>`
: defines a set of rows defining the head of the columns of the table.

`<tr>`
: defines a row of cells in a table. Contains a mix of ```<th> and <td>``` elements.

### Forms

`<button>`
: A clickable button

`<datalist>`
: contains a set of ```<option>``` elements that represent values available for other controls.

`<fieldset>`
: used to group several contrals and labels

`<form>`
: a document section that contains interactive controls to submit information to a server

`<input>`
: used to create interactive controls

`<label>`
: a caption for an item in a user interface

`<legend>`
: represents a caption for the contents of its ```<fieldset>```

`<meter>`
: represents either a fraction or a scalar value within a known range

`<optgroup>`
: creates a grouping of options within a ```<select>``` element

`<option>`
: used to define an item within one of ```<selece>, <optgroup>, or <datalist>``` elements.

`<output>`
: represents the result of a calculation or user interaction.

`<progress>`
: represents the completion progress of a task.

`<select>`
: represents a control that provides a menu of options.

`<textarea>`
: represents multi-line plain-text editing control.

### Interractive elements

`<details>`
: used as a disclosure widget from which the user can retrieve additional information.

`<dialog>`
: represents a dialog box or other interactive component, such as an inspector or window. 

`<menu>`
: represents a group of commands that a user can activate.  Includes list menus and context menus.

`<menuitem>`
: represents a command that the user can invoke through a popup menu, including context menus and menus attached to a menu button.

`<summary>`
: used to caption, summarize, or legend the contents of a ```<details>``` element.


## Validation

* [W3 HTML Validator](http://validator.w3.org/#validate_by_input)
* [W3 CSS Validator](http://jigsaw.w3.org/css-validator/#validate_by_input)

## Layout
### Stickies
<https://gedd.ski/post/position-sticky/>

### Grid
### Flex

### media queries
```
@media only screen and (max-width: 400px){
	/*This rule is invoked for small screens*/
	p{	background-color: blue;}
}
@media only screen and (min-width: 401px){
	/*This rule is invoked for large screens*/
	p{	background-color: blue;}
}
@media only screen and (max-width: 800px) and (layout: landscape){
	/*for small screens in landscape mode*/
	p{	background-color: blue;}
}

@media only print{
	/*This rule is for printers*/
	p{  background-color: white;}
}
```










.

.


.

.

.

.



> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTI2NzEyMzczMl19
-->