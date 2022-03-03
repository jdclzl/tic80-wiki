# elli

_This was added to the API in version 0.90._

`elli x y a b color`

## Parameters

* **x, y** : the [coordinates](coordinate) of the ellipse's center
* **a** : the horizontal radius of the ellipse in pixels
* **b** : the vertical radius of the ellipse in pixels
* **color**: the index of the desired color in the current [palette](palette)

## Description

This function draws a filled ellipse of the radiuses **a** **b** and **color** with its center at **x**, **y**. It uses the _Bresenham_ algorithm.

See also:

- [ellib](ellib) - draw only the boundary of the ellipse

## Example

```
-- title:  ellipse example
-- author: paul59
-- script: lua

local CX,CY=120,68
local RED,GREEN=2,6
local MIN,MAX=8,48

local hrad,vrad=16,16

function TIC()

	if btn(0) and vrad<MAX then vrad=vrad+1 end
	if btn(1) and vrad>MIN then vrad=vrad-1 end
	if btn(2) and hrad<MAX then hrad=hrad+1 end
	if btn(3) and hrad>MIN then hrad=hrad-1 end

	cls(0)
	print('Use the cursor keys to adjust size',24,2)
	--draw filled ellipse
	elli(CX,CY,hrad,vrad,RED)
	--draw outline ellipse
	--swap the horizontal and vertical radii
	ellib(CX,CY,vrad,hrad,GREEN)

end
```
