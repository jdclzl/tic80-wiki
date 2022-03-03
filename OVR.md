# OVR - Overlay

_This callback function was added to the API in 0.60.0._

The **OVR** callback is the final step in rendering a frame. It draws on a separate layer and can be used together with [SCN](https://github.com/nesbox/TIC-80/wiki/SCN) to create separate background or foreground layers and other visual effects.

## Usage

``` lua
function OVR()
  --Draw foreground
end
```

_Note:_ You cannot currently `poke` to VRAM during `OVR`. Ref: https://github.com/nesbox/TIC-80/issues/1305

Since `OVR()` happens after all the scanline callbacks it can be used to draw sprites with a static palette (even if `SCN()` is otherwise changing the palette on each line). The example below shows this in action.

## Example

``` lua
-- title:  Overlap demo
-- author: librorumque
-- desc:   OVR() example
-- script: lua

PALETTE_ADDR=0x03FC0
CHANGE_COL=8
SCR_HEI=136

x=96
y=24

function SCN(line)
  --[[
    Gradient background
  ]]
  local color=(0xff*line/SCR_HEI)//32*32
  poke(PALETTE_ADDR+3*CHANGE_COL, color)
end

function OVR()
  --[[
    This sprite uses the same blue as the
    background, but it's not affected by
    scanline() palette swap
  ]]
  spr(1,x,y,14,3,0,0,2,2)
end

function TIC()
  cls(8)
  print("OVR() example", 85, 3)
  print("Press up or down to move the sprite", 22, 10)
  if btn(0) then y=y-1 end
  if btn(1) then y=y+1 end
end
```