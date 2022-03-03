# mouse

`mouse -> x y left middle right scrollx scrolly`

## Returns
* **x y** : [coordinates](coordinate) of the mouse pointer
* **left** : left button is down (true/false)
* **middle** : middle button is down (true/false)
* **right** : right button is down (true/false)
* **scrollx** : x scroll delta since last frame (-31..32)
* **scrolly** : y scroll delta since last frame (-31..32)

## Description
This function returns the mouse coordinates and a boolean value for the state of each mouse button, with true indicating that a button is pressed.

## Reference

- [Mouse data in RAM](RAM#mouse)

## Example

```lua
-- title:  mouse position and buttons
-- author: paul59
-- script: lua
-- input:  mouse

local GR,YE,WH=6,4,12
local barx,bary=10,10

function TIC()
	local x,y,left,middle,right,scrollx,scrolly=mouse()
	
	barx=barx+scrollx
	bary=bary+scrolly
	if barx<1 then barx=1 end
	if bary<1 then bary=1 end	

	cls(0)
	print('Move Mouse:',10,10,YE)
        print('Position '..string.format('x=%03i y=%03i',x,y),100,10,WH)

	print('Press Buttons:',10,20,YE)
	print('Left '..tostring(left),100,20,WH)
	print('Middle '..tostring(middle),100,40,WH)
	print('Right '..tostring(right),100,30,WH)

	print('Scroll Wheel:',10,80,YE)
	print('Scroll X',100,80,WH)
	print('Scroll Y',160,80,WH)
	rect(100,136/2-barx,8,barx,GR)
	rect(160,136/2-bary,8,bary,GR)

end
```

## Example

![Example 1](https://imgur.com/XIdyNN1.gif)

``` lua
-- title:  mouse demo
-- author: Raidez
-- script: lua
-- input:  mouse

t=0
x=104
y=24

function TIC()
	mx,my,md=mouse() --get x,y and pressed

	if md then
		x=mx
		y=my
	end

	cls(12)
	spr(1+(t%60)/30,x,y,-1,4)
	t=t+1
end
```

## Example

![Example 2](https://imgur.com/JTDD67u.gif)

``` lua
-- title:  demo mouse
-- author: Filippo
-- desc:   wiki demo mouse
-- script: lua
-- input:  mouse

r=0
function TIC()
 cls()

 --get mouse info
 x,y,p=mouse()

 --if pressed
 if p then r=r+2 end r=r-1
 r=math.max(0, math.min(32, r))

 --draw stuff
 line(x,0,x,136,11)
 line(0,y,240,y,11)
 circ(x,y,r,11)

 --show coordinates
 c=string.format('(%03i,%03i)',x,y)
 print(c,0,0,15,1)
end

```