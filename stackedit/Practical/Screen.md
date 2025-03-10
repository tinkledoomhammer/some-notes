# Screen sizes
1.78:1
1 * 1.78*2.04
1 \* 0.49\*0.873
Nom|small|long
--|--|--
5|2.45|4.36
5.5|2.7|4.8
6|2.94|5.23
7|3.43|6.1
8|3.92|6.98
10|4.9|8.72

I like the idea of being able to work on my phone.  I think that I could go as big as 7 in in my pocket. Perhaps a 6 in phone should be next. 4.5-5 in seems best fore something that i might use one handed. Switching keyboards for one handed operation seems reasonable on a 5 in screen but I'm not sure about bigger. 

The most attractive option right now is probably the Lenovo tab 4 7in, about $100. There is also a $10 cheaper "essential" version for $10 less. They both have 1gb ram and 16gb storage. Perhaps the next gen will have more ram. It seems likely that the future is lower ram than I expected. 

IPads are also looking good these days. The mini is about 8in, super high res for $300. I like the more square-like shape for tabs that aren't going in my pocket.

Perhaps a bigger phone will be the way to go. If ram requirements don't increase, this one might last for years to come. I could then add a normal-size tablet as well. Not sure which I would use to read most of the time.  Being able to leave my phone safely on a charger all night while I read or watch stuff would be handy. It just seems silly to go bigger on the phone if a tablet is coming intovthe mix.

Phone+tablet+laptop seems like enough screens. I like the idea of being able to scroll and read without moving the mouse. Not so eager to try to figure out how to copy and past, etc. Perhapsbthe "wifi screen" feature would work for some devices. Does Lenovo make phones?

Drag to phone/tablet would be a very handy app. Could, for example copy or link files.  It would be nice if I could do everything in a browser.

## Two page view

480px is not quite enough for the esp32 datasheets. To have two pages side by side would take at least 1000x800px. I think. The Lenovo 7 is 1280x720. That seems like it might be too short. Most of it doesn't look too bad on my laptop which is only slightly larger. It is limited vertically. Even in fullscreen it is a bit small. I could crop the headers and footers in fbreader to make it work at least in terms of clarity. 
For fullscreen landscape, my 5in almost works.

1:1.78
Split in 2 becomes 0.89:1 or 1:1.236
The halves have .657 the linear size and .631 of the aspect ratio. 1.236x9=11.1. 

Ratio|9|.5|9
--|--|--|--
1.33|12|1.5|13.5
1.78|16|1.12|10.1
1.29|11/8.5|
1.23|8/6.5
The last two are paper and paper less margins. 
So menu placement is crucial especially when splitting 1.78. Any menus that run along the entire long edge of the screen will destroy the split screen. 
1.55 looks about perfect. That would be like 1024x660. 1200x780.

Of course none of that matters except for things in stupid formats.  Too bad that so much stuff is in such shitty formats.

Or better (?) yet $\sqrt2$. 1.41 = 12.7:9. That's only slightly wider than 4:3. It would suck for movies. 

A4:  
* 8.27x11.69 in or 
* 21.01x29.62 cm
* 1.41:1
* 1:(1.41/2) = 1.41
* 14.32 in diagonal
* 


3-page split: 1.78/3 = 0.593.  1/0.593 = 1.685.
$$x/1.78=1.29\to x=1.29*1.78=2.3 $$

## Retinality
$60px/1^\circ\to\frac{360\times60}{2\pi r}=3440$
Thus the size of a retinal pixel at distance d is $d/3440$.

The nominal size in inches of a 16:9 screen is equivalent to 1.148 pixels times its resolution.
$3440/1.148 \approx 3000$
And so a display of given resolution is retinal when the view distance $\frac{d}{nom size}>\frac{3000}{res}$

Res | dist/size | x5|x8
--|--|--|--
1920|1.56|7.8|12.5
1366|2.2|11|17.6
1280|2.34|11.7|17.7
854|3.5|17.5|28

~~These look like they are off by a factor of two or so. These seem pretty reasonable for an optimum distance.~~
18in for my phone, 34in for my laptop.


So it occurs to me that the short dimension of a 10 in tablet is about the same as the long dimension of my phone. Thus typing on the tablet in portrait mode should be similar to thping on my phone in landscape. Using a keyboard of 30% was a bit tight. Im using 33 now. That's about 160 pixels. The stretch doesn't seem that bad at all. The space bar is a bit tricky. 160 px of 1280 would be tiny, like 12% tiny. One-handed typing would be out of the question. The viewable area is 4.36 vs 4.9. My phone has a lit of dead space in that dimension, but a tablet might also. 

Is viewing two pages at once really that good? It would be suck without cropping even at 10in. $8.5/11*800\approx620$ which leaves $1280-1240=40$ unused pixels. 


## Source code
```
123456789112345678921234567893 234567894 234567895 234567896 234567897 234567898 23456789
```
39 chars are visible in the app. About 430~460 of 480 pixels.
69 are visible in landscape. 
$39a+b=480$
$69a+b=850$
$(69-39)a=850-480$
$30a=370$
$a\approx12.3, b\approx50.5$
$(800-50)/12.3\approx 61$

So scaling down is definitely desirable but that might not always be an option and will likely often be an inconvenience.  
On the other hand it might happen automatically when fonts are scaled to inches. 70 or more characters might be normal on a 10in screen. It is 4.9 which is .5 wider then the screen that gave me 69. I think 76 is likely. That would be fine for lots of things. 


## Some sizes
Model|L|W|T
--|--|--|--
Yoga 3 8"|8.26|5.74|0.13
Rebel 3 5"|5.7|2.9|0.31
Lenovo Tab 5 10.1"|9.7|6.7|0.3
Lenovo Tab 10|9.7|6.7|0.38
Asus Zenpad S8 8"|8|5.3|0.3
Yoga 10|10|7.3|0.3

So it looks like 10in tablets are more than an inch wider than I expected. 


> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbNzYwOTMwMzMxXX0=
-->