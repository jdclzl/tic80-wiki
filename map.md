# map

`map [x=0 y=0] [w=30 h=17] [sx=0 sy=0] [colorkey=-1] [scale=1] [remap=nil]`

## Parameters
* **x, y** : The coordinates of the top left map cell to be drawn.
* **w, h** : The number of cells to draw horizontally and vertically.
* **sx, sy** : The screen coordinates where drawing of the map section will start.
* **colorkey** : index (or array of indexes 0.80.0) of the color that will be used as transparent color. Not setting this parameter will make the map opaque.
* **scale** : Map scaling.
* **remap**: An optional function called before every tile is drawn. Using this callback function you can show or hide tiles, create tile animations or flip/rotate tiles during the map rendering stage: `callback [tile [x y] ] -> [tile [flip [rotate] ] ]`

## Description

The map consists of cells of 8x8 pixels, each of which can be filled with a tile using the map editor.

The map can be up to 240 cells wide by 136 deep. This function will draw the desired area of the map to a specified screen position. For example, `map(5,5,12,10,0,0)` will draw a 12x10 section of the map, starting from map co-ordinates (5,5) to screen position (0,0). `map()` without any parameters will draw a 30x17 map section (a full screen) to screen position (0,0).

The map function’s last parameter is a powerful callback function​ for changing how each cells is drawn. It can be used to rotate, flip or even replace tiles entirely. Unlike `mset`, which saves changes to the map, this special function can be used to create animated tiles or replace them completely. Some examples include changing tiles to open doorways, hiding tiles used only to spawn objects in your game and even to emit the objects themselves.

The tilemap is laid out sequentially in RAM - writing 1 to 0x08000 will cause tile #1 to appear at top left when `map` is called. To set the tile immediately below this we need to write to 0x08000 + 240, ie 0x080F0

## Examples

``` lua
-- title:  map() example
-- author: Paul59

-- start a new cart before running this example
-- (so we have Ticsy's sprite available)

-- fill some of the map with the top left quarter of Ticsy's sprite
-- (sprite id#1)
for x=0,30 do
	for y=0,30 do
		mset(x,y,1)
	end
end

function TIC()
	cls(13)
	-- draw a 12x10 section of the map to screen pos (0,0)
	map(5,5,12,10,0,0)
end
```

```lua
-- title:  Remap demo
-- author: AnastasiaDunbar, Lua translation by StinkerB06

--Note that `yourWaterfallTile`, `speed` and `frames` aren't defined here.
--This is up to you. The value of `speed` should be greater than 0 but less than or equal to 1.
gameTicks=0
function remap(tile,x,y)
	local outTile,flip,rotate=tile,0,0
	if tile==yourWaterfallTile then
		outTile=outTile+math.floor(gameTicks*speed)%frames
	end
	return outTile,flip,rotate --or simply `return outTile`.
end
function TIC()
	cls()
	map(0,0,30,17,0,0,-1,1,remap) --The `remap()` function is used here.
	gameTicks=gameTicks+1
end
```