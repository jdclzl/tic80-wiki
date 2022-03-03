# font

`font text, x, y, [transcolor], [char width], [char height], [fixed=false], [scale=1] -> text width`

## Parameters

* **text** : the string to be printed
* **x, y** : [coordinates](coordinate) of print position
* **transcolor** : the [palette](palette) index to use for transparency
* **char width** : distance between start of each character, in pixels
* **char height** : distance vertically between start of each character, in pixels, when printing multi-line text.
* **fixed** : indicates whether the font is fixed width (defaults to false ie variable width)
* **scale** : font scaling (defaults to 1)

## Returns

* **text width** : returns the width of the rendered text in pixels

## Description

This function will draw text to the screen using the foreground spritesheet as the font. Sprite #256 is used for ASCII code 0, #257 for code 1 and so on. The character 'A' has the ASCII code 65 so will be drawn using the sprite with sprite #321 (256+65). See the example below or check out the [In-Browser Demo](https://tic80.com/play?cart=20)

* To simply print text to the screen using the [system font](ram#font), please see [print](print)
* To print to the console, please see [trace](trace)


## Example

![Example](https://i.imgur.com/2DVaG8J.png)

``` lua
-- title:  Font Demo
-- author: paul59
-- desc:   Shows the working of font()
-- script: lua


function TIC()
	cls()
	-- The # character is ascii code 35
	-- so the sprite with ID 256+35 (#291)
	-- will be used to draw that character

	-- A and B have ascii codes 65 and 66.
	-- Sprites 256+65 (#321) and 256+66
	-- (#322) will be used to draw those
	-- letters

	font('#AB',20,20,2,9,9,true,3)

	-- The above prints '#AB' at position
	-- 20,20. The sprites use color 2 as
	-- transparency colorkey, are drawn 9
	-- pixels apart with fixed width and
	-- scaled up by a factor of 3
end
```
For list of ASCII codes, please refer to [this link](https://simple.m.wikipedia.org/wiki/ASCII#Extended_ASCII)