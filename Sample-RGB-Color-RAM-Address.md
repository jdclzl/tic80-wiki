# Edit the palette with poke

You can edit the [palette](palette) directly by poking into [RAM](ram).

```
-ADDR-
0x3FC0 - Index 0 - Red component
0x3FC1 - Index 0 - Green component
0x3FC2 - Index 0 - Blue component
```

To learn more please see [RAM: Palette](ram#palette).

## Examples

### Red

```lua
function TIC()
  cls()
  -- make the first palette entry RED
  poke(0x3FC0, 255)
end
```

![](https://i.ibb.co/wrxLK00/red.gif)

### Green

```lua
function TIC()
  cls()
  -- make the first palette entry GREEN
  poke(0x3FC1,255)
end
```


![](https://i.ibb.co/cty36Y6/green.gif)

### Blue

```lua
function TIC()
  cls()
  -- make the first palette entry BLUE
  poke(0x3FC2,255)
end
```

![](https://i.ibb.co/8PP1wPh/blue.gif)



