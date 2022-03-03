# clip

`clip` Unsets the clipping region (draws to the full screen)

`clip x y width height` Sets the clipping region

**Parameters**

* **x**, **y** : [coordinates](coordinate) of the top left of the clipping region
* **width** : width of the clipping region in pixels
* **height** : height of the clipping region in pixels

## Description

This function limits drawing to a clipping region or 'viewport' defined by `x`,`y`, `width`, and `height`.
Any pixels falling outside of this area will not be drawn.

Calling `clip()` with no parameters will reset the drawing area to the entire screen.

## Example

``` lua
-- clip example
-- author: paul59
-- script: lua

local BLACK=0
local BLUE=8
local c=false

function TIC()
	cls(BLACK)
	if c then
		-- limit drawing to a 100 pixel wide/high
		-- region, with top left at 60,20
		clip(60,20,100,100)
		-- cls() is also affected by
		-- the clipping region
		cls(BLUE)
	else
		-- reset to entire screen
		clip()
	end
	print("Press 'Up' To Toggle",60,0)
	print("Clipping",90,20)
	if btnp(0) then c=not c end

end

```