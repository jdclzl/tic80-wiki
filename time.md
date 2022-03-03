# time

`time -> ticks` elapsed since game start

## Returns

- **ticks** : the number of _milliseconds_ elapsed since the game was started 

## Description
This function returns the number of _milliseconds_ elapsed since the cartridge began execution. Useful for keeping track of time, animating items and triggering events.

## Example

![Example](https://imgur.com/A6apDDY.gif)

``` lua
-- time demo

function TIC()
 cls()
 --Show rising time
 print('Seconds elapsed: '..time()/1000)  
 
 --Blink warning
 if(time()%500>250) then
  print('Warning!',0,30)
 end
 
 --After 2 seconds show this message
 if(time()>2000)then
  print('Fugit inreparabile tempus',0,60)
 end 
end
```