# print

`print text [x=0 y=0] [color=15] [fixed=false] [scale=1] [smallfont=false] -> text width`

## Parameters
* **text** : any string to be printed to the screen
* **x, y** : [coordinates](coordinate) for printing the text
* **color** : the [color](Palette) to use to draw the text to the screen
* **fixed** : a flag indicating whether fixed width printing is required
* **scale** : font scaling
* **smallfont** : use small font if true

## Returns
* **text width** : returns the width of the text in pixels.

## Description
This will simply print text to the screen using the font defined in config. When set to true, the fixed width option ensures that each character will be printed in a 'box' of the same size, so the character 'i' will occupy the same width as the character 'w' for example. When fixed width is false, there will be a single space between each character. Refer to the example for an illustration.

* To use a custom rastered font, check out [font](font).
* To print to the console, check out [trace](trace).


## Example 1

![Example 1](https://i.imgur.com/FBEiXYY.png)

``` lua
-- title:  print centered
-- author: Vadim
-- desc:   print text perfectly centered
-- script: lua
-- input:  gamepad

cls()
local string="my perfectly centered text"
local width=print(string,0,-6)
print(string,(240-width)//2,(136-6)//2)

function TIC()end

```
```lua
--Prints text where x is the center of text.
function printc(s,x,y,c)
    local w=print(s,0,-8)
    print(s,x-(w/2),y,c or 15)
end
```
## Example 2

![Example 2](https://i.imgur.com/lR279OS.png?2)

```lua
-- title:  Fixed width demo
-- author: paul59
-- desc:   Show effect of fixed width flag
-- script: Lua

function TIC()
  cls(0)
  print('FIXED',0,0,15,true)
  print('FIXED',0,8,15,false)
  print('width',0,32,15,true)
  print('width',0,40,15,false)
  for i=0,30,6 do
    line(i,0,i,6,8)
    line(i,8,i,16,9)
    line(i,32,i,40,8)
    line(i,40,i,48,9)
  end
end
```

## Example 3

![Example 3](https://imgur.com/StAx2jX.gif)

``` lua
-- title:  print demo
-- author: Filippo
-- desc:   print matrix
-- script: lua
-- input:  gamepad
-- pal:00000000ff0000e50000cc0000b200009900007f00006600004c00003300001900000000b2ffb2ccffcce5ffe5ffffff

msg="FNORD                      "
t=0
function TIC()
 cls()
 c=1
 for x=0,29 do
  for y=0,16 do
   c=(c+1)%#msg
   l=(c-math.floor(t))%#msg
   print(msg:sub(l,l),x*8,y*8,y%12)
  end
 end
 t=t+0.15
end
```

## Example 4

![Example 4](https://imgur.com/tu6CEbp.gif)

``` lua
-- title:  print demo scale
-- author: Filippo
-- desc:   scale print
-- script: lua
-- input:  gamepad

t=0
txt="[TIC]"

function TIC()
 for i=1,15 do
 local of=3*i
  poke(0x3FC0+of,100+i*10)
  poke(0x3FC0+of+1,i*5)
  poke(0x3FC0+of+2,32+32*math.sin(t/100))
 end

 cls()
 for z=15,0,-1 do
  y=12*z*math.sin(z/5+t/50)
  w=print(txt,0,-100,z,false,z)
  print(txt,(240-w)/2,
       68+y/1.5,
       15-z,false,z)
 end
 t=t+1
end
```

