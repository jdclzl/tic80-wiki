# trace

`trace message [color=15]`

## Parameters

* **message** : the message to print in the [console](console).
* **color** : the index of a color in the current [palette](palette) (0..15)

## Description

This is a service function, useful for debugging your code. It prints the supplied string or variable to the console in the (optional) color specified.

_Tips:_

1. The [Lua](lua) concatenation operator is `..` (two periods)
2. Use the console [cls](cls) command to clear the output from trace

## Example

![Example](https://i.imgur.com/Pk31XXa.png)

``` lua
-- trace demo
cls()
function TIC()
	trace('Hello console:'..time())
	trace('This text is blue',8)
end
```