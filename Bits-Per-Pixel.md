# BPP **_(Bits Per Pixel)_**


![bpp us](https://user-images.githubusercontent.com/71924055/95566559-562ded80-0a3f-11eb-9b48-90cdf260da95.gif)

**BPP** is a setting in the **TIC-80** Sprite Editor.

BPP Values:
* 1 Bit - **_(Palette of 2 colors, including transparency)_**
* 2 Bit - **_(Palette of 4 colors, including transparency)_**
* 4 Bit - **_(Palette of 16 colors, including transparency)_**


### NOTE
***
* Changing to lower BPP values will result in **stretched out sprites**.
* Changing to higher BPP values will result in **Corrupted sprites**.
* When pasting a sprite created with a different BPP value, the same corruptions will occur for higher and lower values respectively
### Address List
***

Use this address VRAM: `0x3FFC` - _Blit Segment_
* Required using `poke()` and `poke4()`

**List:**
```
0000 SYS GFX
0001 FONT

0010 4bpp BG Page 0
0011 4bpp FG Page 0

0100 2bpp BG Page 0
0101 2bpp BG Page 1
0110 2bpp FG Page 0
0111 2bpp FG Page 1

1000 1bpp BG Page 0
1001 1bpp BG Page 1
1010 1bpp BG Page 2
1011 1bpp BG Page 3
1100 1bpp FG Page 0
1101 1bpp FG Page 1
1110 1bpp FG Page 2
1111 1bpp FG Page 3
```
### BPP Examples
***
## 4 Bit:
![image](https://user-images.githubusercontent.com/71924055/95568916-76ab7700-0a42-11eb-8b66-f265d7e765f9.png)

![image](https://user-images.githubusercontent.com/71924055/95569236-ea4d8400-0a42-11eb-86db-e852b0cb7dab.png)

## 2 Bit:
![image](https://user-images.githubusercontent.com/71924055/95569027-9cd11700-0a42-11eb-9de6-d6467804d0de.png)

![image](https://user-images.githubusercontent.com/71924055/95569315-09e4ac80-0a43-11eb-8c82-3f8579939d26.png)

## 1 Bit:
![image](https://user-images.githubusercontent.com/71924055/95569119-c25e2080-0a42-11eb-8ee3-08fe3ae01e5a.png)

![image](https://user-images.githubusercontent.com/71924055/95569395-28e33e80-0a43-11eb-8284-210934cce7f7.png)


### Links
***
[More information on Bits Per Pixel](https://www.tutorialspoint.com/dip/concept_of_bits_per_pixel.htm)

