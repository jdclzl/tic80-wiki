# sfx

`sfx id [note] [duration=-1] [channel=0] [volume=15] [speed=0]`

...or to stop playing:

`sfx -1`<br>
`sfx -1 nil nil channel`

## Parameters

* **id** : the SFX id (0..63), or -1 to stop playing
* **note** : the note number (0..95) or name (ex: `C#3`)
* **duration** : the duration (number of frames) (-1 by default, which plays continuously)
* **channel** : the audio channel to use (0..3)
* **volume** : the volume (0..15) (defaults to 15)
* **speed** : the speed (-4..3) (defaults to 0) 

## Description

This function will play the sound with **id** created in the sfx editor.  Calling the function with an **id** of `-1` will stop playing the channel.

The **note** can be supplied as an integer between 0 and 95 (representing 8 octaves of 12 notes each) or as a string giving the note name and octave. For example, a note value of '14' will play the note 'D' in the second octave. The same note could be specified by the string 'D-2'. Note names consist of two characters, the note itself (**in upper case**) followed by '-' to represent the natural note or '#' to represent a sharp. There is no option to indicate flat values. The available note names are therefore: C-, C#, D-, D#, E-, F-, F#, G-, G#, A-, A#, B-. The octave is specified using a single digit in the range 0 to 8.

The **duration** specifies how many ticks to play the sound for; since TIC-80 runs at 60 frames per second, a value of 30 represents half a second. A value of -1 will play the sound continuously.

The **channel** parameter indicates which of the four channels to use. Allowed values are 0 to 3. 

**Volume** can be between 0 and 15.

**Speed** in the range -4 to 3 can be specified and means how many 'ticks+1' to play each step, so speed==0 means 1 tick per step.

## Example

``` lua
-- title:  sfx example
-- author: paul59
-- script: lua

local t=0
function TIC()
	t=t+1
	-- play note 14 (D in octave 2)
	-- every half second with a duration of 
	-- 20 ticks
	if t%30==0 then sfx(0,14,20) end
	-- play D in octave 4 every second
	-- with a duration of 20 ticks
	-- use channel 1 and volume 10
	if t%60==0 then sfx(0,'D-4',20,1,10) end
	-- every 2 seconds
	-- play D quietly in octave 5
	if t%120==0 then sfx(0,'D-5',20,1,4) end
end
```