The Music Editor has two separate views that can be toggled using the top buttons in the top right: Piano Mode and Tracker Mode. Either view can be used to product the same result as there is no functional difference between the two, but one style may preferable over the other for different users.

TIC-80 can store up to 8 **tracks** (0 to 7) that act like separate pieces of music. The **tempo**, measured in beats per minute (BPM), is how fast the music will play back. The number of **rows** determines how many beats are in a given track, to a maximum of 64.

Tracks created in the Music Editor can be played using the [music](music) command.

## Piano Mode
![Music Editor Screen Capture - Piano Mode](https://i.imgur.com/LvHS62i.png)

In piano mode, the left column represents frames (known as "patterns" in most other trackers). Each frame will contain a series of notes (shown in the middle column) specified in the Rows count. Like tracker mode, up to four frames can be simultaneously playing in a given track. The frame currently being edited is highlighted in white.

At the bottom of the frame columns, the individual columns can have their mute status toggled.

The note entry columns become active once the frame number is created/specified by typing its number into the appropriate column. Note that frame index count begins at `01`.

A note break (or "stop") can be inserted by right-clicking on a row in the current frame. Stops are indicated by a horizontal line of red dots in the piano column.

### Commands
The commands in the command column are available when an active pattern is selected in the frame column. The commands are: **M**aster volume, **C**hord play, **J**ump to frame/beat, **S**lide, **Pitch** shift, **V**ibrato, and **D**elay trigger.

Commands are parameterized by using the X and Y values in the column to the right. Only a single command can be applied in each row.

## Tracker Mode
![Music Editor Screen Capture - Piano Mode](https://i.imgur.com/EARkaE1.png)

This mode may be familiar to users of previous TIC-80 versions, as well as those used to creating music on limited-resource systems.

To enter a note, click on a cell (or move to it using the arrow keys) and press a key on the bottom row of the keyboard similar to the [SFX Editor](SFX-Editor). You can also enter notes from the next octave apart. Once a note is entered, it will play until the next note or note stop.

A note break (or "stop") can be entered by pressing `A` or `SPACE`. Stops are indicated by blank values in the note region instead of dashes.

### Notes
A note on the track has three components: **note**, **SFX**, and **command**.

![Note on a track](https://i.imgur.com/FR6uJnh.png)

The note component is a letter from A to G indicating the actual note value. Sharp values are indicated by `#` after the letter. The number next to the note value is the octave that note belongs to.

The SFX component is an orange number in the range of `00` to `63`. This number is the index of the voice (or instrument) created in the [SFX Editor](SFX-Editor) that will be played.

The command component consists of two parts: the command itself, and the X/Y parameters for the command. For more information, see the [Commands](#Commands) section of Piano Mode.

## Keys
Most keyboard shortcuts are usable in both modes in the Music Editor, as well as the [SFX Editor](SFX-Editor).

![Music Editor Hotkeys](https://i.imgur.com/nG6D9vK.png)

```
Del | Delete selection / selected row
Backspace | Delete the row above
Insert | Insert rows below
Ctrl+F1 | Decrease notes by Semitone
Ctrl+F2 | Increase notes by Semitone
Ctrl+F3 | Decrease octaves
Ctrl+F4 | Increase octaves
Space | Play current track
Ctrl+Right | Jump forward one frame
Ctrl+Left | Jump backward one frame
Tab | Go to next channel
Shift+Tab | Go to previous channel
+ | Next pattern
- | Previous pattern
Ctrl+Up | Next instrument
Ctrl+Down | Previous instrument
```

## Crash course tracking music in TIC-80

[https://www.youtube.com/watch?v=V9oAccwjeDA](https://www.youtube.com/watch?v=V9oAccwjeDA)

[![Video](https://img.youtube.com/vi/V9oAccwjeDA/0.jpg)](https://www.youtube.com/watch?v=V9oAccwjeDA)
