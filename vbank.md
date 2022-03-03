# vbank

*This API has not yet been released yet, but is available on GitHub.*

`vbank 0` Switch to bank 0 of VRAM

`vbank 1` Switch to bank 1 of VRAM ("overlay" VRAM)

## Parameters

* **id** : the VRAM bank ID to switch to (0 or 1)

## Description

VRAM is double-banked, such that the entire 16kb VRAM address space can be "swapped" at any time between banks 0 and 1.  This is most commonly used for layering effects (background vs foreground layers, or a HUD that sits overtop of your main gameplay area, etc).

Note: `vbank` should not be confused with `sync`.  VRAM banks can be switched
many times during a single `TIC` (though this isn't common) - this is not true for the other banked RAM.

### Default gfx pipline rendering behavior

The banks are rendered to the screen simultaneously with any opaque portions of bank 1 appearing on "top" of bank 0.  Bank 0 data is rendered using the bank 0 palette and bank 1 is rendered using the bank 1 palette.  `OVR TRANSPARENCY` (0x03FF8) controls which palette index of bank 1 is considered transparent.

Currently this behavior cannot be modified.
### OVR vs VRAM bank 1

Previously there was no way to access bank 1 VRAM, one had to rely on the magic of the [OVR](ovr) callback - whose graphics calls magically wrote only to bank 1.  Now anyone can implement this functionality using `vbank`.

```lua
function draw_overlay()
    -- switch to vram bank 1
    vbank(1)
    -- set palette entry 15 to transparent
    poke(0x03FF8, 15)
    -- clear the overlay layer (fully transparent, all index 2)
    cls(15)
    -- draw your custom overlay graphics here
end

function TIC()
    -- switch back to vram bank 0
    vbank(0)
    -- draw normal stuff here

    draw_overlay()
end
```

## Examples

None yet.