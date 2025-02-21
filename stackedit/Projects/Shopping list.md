# Shopping list

## UI

### Initial assumptions

To start off, the only use case of concern is a phone in portrait mode.  

### The entire app is a single list with 3 sections

 1. To buy
 2. In cart
 3. Saved items

Each section heading has a button to add new items to that section. And a clear/"add all" button. Perhaps it could be called "check out" for the "In cart" section.
Each section is divided into departments / locations.

Only the "Saved items" section will list departments for which there are no items in the current section.
The departments are collapsible.


### Items (in the list)

Each item will have a control to move the item to a different section. Probably a check box in the left.

Each item will have an edit button, probably on the right.

### Departments

Each department is collapsible.
In their collapsed state they show how many items are in that department in the current section.

There will also be an add button to create new items that default to that department.
Creating new departments will be done on the edit/new item dialog.

### Stickies

The section labels will always be on screen. The current / open section will be on top. The others will be at the bottom.
The "current" department will also sticky at the top.  Current simply means the top most department that has items on the screen.

There should also be a menu of some sort at the top.

Undo and redo buttons. Eventually synch, open, etc.

It looks like the top/bottom buttons will probably be at fixed positions but the section headings will get `position: sticky;` <https://gedd.ski/post/position-sticky/> 

That technique could probably be used for both, but then it might be hard to get it the bottom buttons to consistently go to the bottom. 

### Layout
Current looks something like (after cleaning up some class names)
1. div.page-outer
	2. div.lists-outer
	3. div.list-outer x3
		4. div.list x1
			5. div.list-head x1
			6. div x1
				7. department xA
					8. department-head x1
					9. items x1
						10. item xB 

I think that this might work better:

2. div.page-outer
	3. div.list-active [absolute]
		4. div
			5. div.list-head
			6. table xA
				7. thead x1 [sticky]
				8. tbody x1
					9. trow xB
	4. div.list-inactive [fixed]
	5. div.list-inactive [fixed]

The advantage of tables is that they have the head/body structure that I want.  



### Sizing
|Location| Height| font size|
|--|--|--|
`list-header` | 6+20 | 3+10 |
`category-header` | 3+20 | 2.2+10|
`list-item` | auto | 2+10

Perhaps [CSS variables](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_variables) would be useful for specifying heights and font sizes. 

The final sizes in revision b44f4d5 seem to work pretty well, but the following vertical layout would probably work better.  
var|val
--|--
`--var-height`|`2vh + 7px`
`--lhh`|`3 * --var-height`
`--chh`|`2 * --var-height`
`--lh-top-1`|`--lhh`
`--lh-top-2`|`2 * --lhh`
`--lh-top-#`|`# * --lhh`
`--lh-bottom-1`|`100vh - 2 * --lhh`
`--lh-bottom-2`|`100vh - --lhh`
`--ch-top-0`|`--lhh`
`--ch-top-1`|`2 * --lhh`
`--ch-top-2`|`3 * --lhh`
`--ch-top-#`|`(# + 1) * --lhh`
`--cb-top-#`|`--ch-top-# + --chh`


## Switching from flexbox to grid
### Changing list heading positioning
I would prefer if the lists always stayed in the same order.  It would be more easy to tell which list is active by the number of list headers at the top vs bottom.

Each list can be in several states.
1. Active vs inactive
2. Top vs bottom

The padding-top that is needed on category-body will have one value per list.
The same for the margins.

### Horizontal alignment

The column positions and width should adapt to screen size, zoom, and the width of items in the list (?)
Would fixed columns be better? Perhaps a simple 12-column layout would be easiest

| Column | Width | Start | End |
|---|---|---|---|
|Category Header|11|2|12|
|Category Expand|1|2|2|
|Category Count|1|4|4|
|Category Label|5|6|10|
|Category Add|1|12|12|
The above doesn't leave any padding after the add button. I didn't have enough columns for both.
* Idea: All of the long/stretchy elements will include middle columns.  The left most and right-most columns could be tiny.
* So basically I just need to pick starting points and order them

|Column|Position|Approx Scale|
|--|--|--|
List | |
Padding|1|1|
Title|2|10|
Padding|3|1|
Add|4|2|
Padding|5|1
Category Header | |
Margin  |1|1|
Padding |2|1|
Expand  |3|2|
Count   |4|2|
Padding |5|1|
Title   |6|10|
Padding |7|1|
Add     |8|2|
Padding |9|1|
Category Item | |
Margin  |1|2|
Padding |2|1|
Title   |3|10|
Padding |4|1|
Edit    |5|2|
Padding |6|2|

### Switch item rows from flexbox to static and use floats for positioning.

## Redesign 1
Instead of three lists, use tabs, labeled with icons.  "List", "Cart", and "Saved". The last one might be "recent," "history," or "Add to list"

### Controls
So the top bar has
1. Menu button
2. Label, changes with the current list
3. Icon tabs
4. Zoom/scale control

Should probably start tracking scrolling.  The scroll state should be saved and restored when switching tabs and prossably also on department expand/contract.

New naming scheme
Categories are now departments
Saved should get a new name but I don't know what

Usage Modes | Notes
--|--
View List| [Move to cart / Remove]
Build List| New Item / [Move to list / Move to cart]
 | | It might be economical to fit the New Item dialog box above the first category
Edit Cart| All and Each: [Move to List / Remove from cart]
Modal Dialogs|
New Item| Select Department, (+New Department) Item Name, Cart/List/Save for later, Save/Cancel
New Category| One text box and save/cancel
Scale/zoom selector| A slider control and +/- buttons
Main menu| Save/Load? The button needs to be there
Additional Dialog Boxes|

### Scaling
The complexity of the variable header is greatly reduced.  There can be as few as one fixed header (not counting stickies). Other than the top bar, A fixed submenu (that changes contents but not thickness) could be added. Not sure what would go in it in each mode, but It could just be sparse in some modes. The New Item form could also be fixed, or alternatively that mode has a button in its sub-menu to scroll up.

### Layers
1. list items on the bottom
2. category headers above that
3. the top header above that
4. then a masking layer for modal dialog boxes
5. Then modal dialog boxes.

## Last major tasks
### Editing
One form will be created at the beginning of the 'new item' page.
It can be moved to the appropriate location in the DOM as needed and populated at that time.

Should updating an item stack a history state? Using the back button for undo would be handy. A redo/undo menu perhaps? Those changes could be stored elsewhere. The form should blend with second menu and dept headers.

Should names be unique? That would make syncing easier.

### Scroll management
When switching pages, the scroll state should be saved / restored.

Anytime content is added or removed, scroll values may need to be updated. Items moved from one page to another will be complicated. 

Perhaps `history.push` needs a wrapper that adds the scroll state to the previous state or some global. 

### Item grouping
Allow items to be grouped in subgroups of departments. This should be optional, perhaps implicite. Alphabetizing items would work ish.

So perhaps ordering departments and items is the logical first step. That might require a little refactoring.




### Second menu bar
For list-wide actions. Clear cart and table of contents  / jump to department come to mind.  And create new item.

1. Clear cart/cart all.
2. Expand/shrink all
3. Sync
4. New item

The colir should match the department headers.

### Buttons
The last page has no buttons at all and some buttons need confirmation. Perhaps there is a cleaner way to make buttons require confirmation.

Highlighting would also be nice. At least for the menu button n and probably sync as well. Perhaps a nifty css or svg animation.

That reminds me that I haven't thought about the sync button yet.

The svg should be refactored and perhaps the `<img>` tags replaced with `<svg>`

## Redesign #2
### What's left to figure out?
These things could be worked out on the current version (v2.js, et al):
1. Scrolling and measuring screen positions when expanding/contracting.
2. Perhaps the modal overhaul.
3. Saving and loading data.

### Use list tags
```html
<ol.list>
	<lh>List Header</lh>
	<li.department><ol.department>
		<lh>Department Header</lh>
		<li.item> </li>
	</ol></li>
</ol>
```
### Fewer buttons
Items:
* check box
* edit button

Departments:
* Move buttons
* Expand/collapse

Secondary Menu:
* Move buttons
* expand/collapse all

Top Menu
* Tabs : 
	* View List
	* View Saved Items
	* Should there even be a cart?

### Modal Menus 
* The modal menus should probably use the top menu bar.
* The menu button should become a back button.
* The list title could be replaced by a menu title
* The tabs could stay the same.

### History
* History entries should have two variants
	* modal
	* tabs
* The scroll should be saved when entering a modal state or switching between tabs
* The scroll should be restored when switching to a tab
* Does there need to be more than one of each? 
	* Probably one for each layer of modal.
* The modals should have a back button that does the same thing as the browser back button.
* Perhaps the main modal menu should be where 'back' goes when viewing tabs?

### SVG Improvements

Perhaps the `<svg>` element should be used instead of `<img>`. 
Consolidation of symbols into a single file would be handy. That would allow the creation of composite symbols in a single element. 

## Storage

1. Initially all data will be saved only in the browser. 
	* Just variables at first, resetting on each reload
	* Then local storage 
2. Then defaults will be loaded when the page is refreshed.
3. The next stage will be to synchronize data stored locally by the browser with the retrieved defaults.
4. Then more elaborate synchronization.
5. Then synchronizing with cloud storage.
	* first just a single gdrive file.
6. Then support for multiple lists and synch locations.

## Redesign #3
1. More intuitive buttons
2. Combine all SVG into a single file
	3. Perhaps using SVG.js.
3. Better data management. Something like vue.js? Should a persistent model be maintained (in addition to the DOM)?
4. 

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTE4Mjk4MTU5N119
-->