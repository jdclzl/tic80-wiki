# Code Snippets



## Utilities

- [Copy Sprite Data to a Table](#copy-sprite-data-to-a-table)
- [Load string palette](#load-string-palette)
- [Inc and Dec](#inc-and-dec)
- [Trace all global variables](#trace-all-global-variables-by-al-rado)
- [Write and read function from memory](#write-and-read-function-from-memory)
- [Interpolation functions](#interpolation-functions)
- [Normally distributed random number generator](#normal-random-number)
- [Perlin Noise](https://gist.github.com/AnastasiaDunbar/d1ec3f0f678a00ddc5947b1a3fdd10ea)
- [Rescale RangeA to RangeB](#rescale-rangea-to-rangeb)
- [Lapse of time spent at function](#lapse-of-time)

## Drawing

- [Draw ellipse function on Moonscript](#draw-ellipse-function-on-moonscript)
- [Draw Bézier curves on Moonscript](#draw-bézier-curves-on-moonscript)
- [Set/Get spritesheet pixel](#setget-spritesheet-pixel)
- [Sky gradient](Sky-gradient)
- [Clear map](#clear-map)
- [Palette swapping](#palette-swapping)
- [pal function](#pal-function)
- [palette functions](#javascript-palette-functions) (JavaScript)
- [Example Edit the palette using poke](Sample-RGB-Color-RAM-Address)
- [Rainbow color](#rainbow-color-function)

## Animation / Effects

- [Glitch](#glitch)
- [Shake screen](#shake-screen)
- [Screen wave and sky gradient](#screen-wave-and-sky-gradient)

## Sound / Music

- [Mute one channel of a music track](#mute-one-channel-of-a-music-track)

## Input

- [Text Only Game](example-text-only-game)


## Other

- [BrainF**k on Moonscript](#brainfk-on-moonscript)


## Uncategorized

- [Some code snippets made by Raidez (contains useful class for: collision (rectangle and circle), sprite, menu)](https://itch.io/t/63610/some-code-snippets)

### For older versions of TIC-80

The latest version of TIC-80 supports all of these things with built-in API functions.

1. [Mouse Scroll](#mouse-scroll)
1. [Peek and Poke Nibbles](#peek4-and-poke4) `peek4` and `poke4`
1. [Sprite Flags](#simple-sprite-flags) `fset` and `fget`
1. [Draw circles](#draw-circle-function) `circ` and `circb`

---


## Copy Sprite Data to a Table
Sometimes you might want to copy sprite data to a table for manipulation, rather than display it on the screen directly. This example shows how to get pixel data for a given sprite by supplying its ID, width and height:

```lua
-- Parameters sprite number, width and height of the sprite(s).
-- returns a table containing pixel data
function gspr(num, w, h)
  local addr=0x4000
  local offset = w * num % 128
  local hoffset= h * math.floor((w*num)/128)
  local g = {}

  for x=0,w-1 do
    g[x] = {}
    for y=0, h-1 do
     g[x][y] = peek4((addr + (x//8+y//8*16)*32) *2+x%8+y%8*8)
    end
  end
  return g
end
```


## Load String Palette

Useable with palette string defined in [palette page](https://github.com/nesbox/tic.computer/wiki/palette) or copied from the Sprite Editor.

```lua
-- script: lua
--Load palette string
function loadPalette(palette)
  for i=0,15 do
    r=tonumber(string.sub(palette,i*6+1,i*6+2),16)
    g=tonumber(string.sub(palette,i*6+3,i*6+4),16)
    b=tonumber(string.sub(palette,i*6+5,i*6+6),16)
    poke(0x3FC0+(i*3)+0,r)
    poke(0x3FC0+(i*3)+1,g)
    poke(0x3FC0+(i*3)+2,b)
  end
end
```

```js
// script: js
function loadPalette(string){
	for(var i=0;i<16;i++){
		poke(0x03FC0+(i*3),parseInt(string.substr(i*6,2),16));
		poke(0x03FC0+(i*3)+1,parseInt(string.substr((i*6)+2,2),16));
		poke(0x03FC0+(i*3)+2,parseInt(string.substr((i*6)+4,2),16));
	}
}
```

## Clear map
Use `sync(4,0,true)` to save the editing of the map to cart.
```js
function clearEntireMap(){
	for(var y=0;y<136;y++){
		for(var x=0;x<240;x++){
			mset(x,y,0);
		}
	}
}
function clearMap(mx,my){ //Since there are 8x8 maps.
	mx=Math.floor(mx)*30;my=Math.floor(my)*17;
	for(var y=0;y<17;y++){
		for(var x=0;x<30;x++){
			mset(mx+x,my+y,0);
		}
	}
}
```
```lua
function clearEntireMap()
	for y=0,135 do
		for x=0,239 do
			mset(x,y,0)
		end
	end
end
```

## Inc and Dec
These functions increment and decrement variables by 1. Works only for globals.
The use is for minifying the code when you have many somevar = somevar+1 calls
which can waste many characters when names are very descriptive such as enemy_health,
while still keeping the code readable.

``` lua
function inc(v)
 _G[v] = _G[v] + 1
end
function dec(v)
 _G[v] = _G[v] - 1
end
```
With MoonScript you can use the `+=` and `-=` operators.

## Glitch
``` lua
-- 'Change screen offset in every scanline' demo
-- author: Vadim
shake=0
d=4
function TIC()
	if btnp()~=0 then shake=30 end
	if shake>0 then
		poke(0x3FF9+1,math.random(-d,d))
		shake=shake-1
		if shake==0 then memset(0x3FF9,0,2) end
	end
	cls(12)
	print("PRESS ANY KEY TO GLITCH!",54,64)
end

function scanline(row)
	if shake>0 then
		poke(0x3FF9,math.random(-d,d))
	end
end
```

## Palette swapping
``` lua
-- swap c0 and c1 colors, call pal() to reset
function pal(c0,c1)
	if(c0==nil and c1==nil)then for i=0,15 do poke4(0x3FF0*2+i,i)end
	else poke4(0x3FF0*2+c0,c1)end
end
```
```js
// script: js
function pal(c0,c1) {
    if (arguments.length==2)
        poke4(0x3ff0*2+c0,c1)
    else
        for (var i=0;i<16;i++)
            poke4(0x3ff0*2+i,i)
}
```

## Screen wave and sky gradient
``` lua
-- title: screen wave and sky gradient snippet
-- author: Darkhog

-- TIC function cut because it doesn't matter here.
-- This code can be used for e.g. water or some hazy/hot environments.
wavelimit = 136/2
function scanline(row)
	-- skygradient
	poke(0x3fc0,190-row)
	poke(0x3fc1,140-row)
	poke(0x3fc2,0)
	-- screen wave
	if row>wavelimit then
		poke(0x3ff9,math.sin((time()/200+row/5))*10)
	else
		poke(0x3ff9,0)
	end
end
```

## pal function
```lua
--sets the palette indice i to specified rgb
--or return the colors if no rgb values are declared.
function pal(i,r,g,b)
	--sanity checks
	if i<0 then i=0 end
	if i>15 then i=15 end
	--returning color r,g,b of the color
	if r==nil and g==nil and b==nil then
		return peek(0x3fc0+(i*3)),peek(0x3fc0+(i*3)+1),peek(0x3fc0+(i*3)+2)
	else
		if r==nil or r<0 then r=0 end
		if g==nil or g<0 then g=0 end
		if b==nil or b<0 then b=0 end
		if r>255 then r=255 end
		if g>255 then g=255 end
		if b>255 then b=255 end
		poke(0x3fc0+(i*3)+2,b)
		poke(0x3fc0+(i*3)+1,g)
		poke(0x3fc0+(i*3),r)
	end
end
```
```moonscript
pal = (i=0, r, g, b) ->
	mx = math.max
	mn = math.min
	fl = math.floor

	i = fl(mx(0, mn(15, i)))
	
	if r==nil and g==nil and b==nil
		return peek(0x3fc0+(i*3)), peek(0x3fc0+(i*3)+1), peek(0x3fc0+(i*3)+2)
	
	r = fl(mx(0, mn(255, r or 0)))
	g = fl(mx(0, mn(255, g or 0)))
	b = fl(mx(0, mn(255, b or 0)))
	
	poke(0x3fc0+(i*3),   r)
	poke(0x3fc0+(i*3)+1, g)
	poke(0x3fc0+(i*3)+2, b)
```

```js
function pal(i,r,g,b){
	i=(i|0)*3;
	if(r===undefined&&g===undefined&&b===undefined){
		return[peek(16320+i),peek(16321+i),peek(16322+i)];
	}else{
		poke(16320+i,r);poke(16321+i,g);poke(16322+i,b);
	}
}
```

## Shake screen
``` lua
-- 'shake screen demo' demo
-- author: Vadim

shake=0
d=4
function TIC()
	if btnp()~=0 then shake=30 end
	if shake>0 then
		poke(0x3FF9,math.random(-d,d))
		poke(0x3FF9+1,math.random(-d,d))
		shake=shake-1
		if shake==0 then memset(0x3FF9,0,2) end
	end
	cls(12)
	print("PRESS ANY KEY TO SHAKE THE SCREEN!",24,64)
end
```

## Trace all global variables by Al Rado

``` lua
-- title:  Trace all global variables
-- author: Al Rado 28.02.2017
-- desc:   Standard Lua demo
-- script: lua
-- input:  gamepad
-- pal:    DB16

local seen={}

function dump(t,i)
    seen[t]=true
    local s={}
    local n=0
    for k in pairs(t) do
        n=n+1 s[n]=k
    end
    table.sort(s)
    for k,v in ipairs(s) do
        trace(i..v)
        v=t[v]
        if type(v)=="table" and not seen[v] then
            dump(v,i.."\t")
        end
    end
end

cls()
trace("---------------------")
trace("All global variables:")
dump(_G,"")
trace("---------------------")

print("See all global variables in console!")

function TIC()
  exit()
end
```

## Simple Sprite Flags

**Update:** [fset](fset) and [fget](fget) are now built-in API functions and the Sprite Editor also supports setting flags during editing.

``` lua
FLAGS={}
function fset(i,f,v)
  local val=FLAGS[i] or 0
  local mask=1<<f
  FLAGS[i]=v
    and val|mask
    or val&~mask
end

function fget(i,f)
  local val=FLAGS[i] or 0
  return val&(1<<f)~=0
end

fset(5,5,true)
trace(fget(5,5))
fset(5,5,false)
trace(fget(5,5))

cls()

function TIC()
end
```

## Set/Get spritesheet pixel

The spritesheet isn't a 128x256 image in the memory, it's 512 consecutive 8x8 4 bit sprites.
These functions map it to a 128x256 image.

``` lua
-- set spritesheet pixel
function sset(x,y,c)
    local addr=0x4000+(x//8+y//8*16)*32 -- get sprite address
    poke4(addr*2+x%8+y%8*8,c) -- set sprite pixel
end
```
``` lua
-- get spritesheet pixel
function sget(x,y)
    local addr=0x4000+(x//8+y//8*16)*32 -- get sprite address
    return peek4(addr*2+x%8+y%8*8) -- get sprite pixel
end
```
```js
function sget(x,y){
	return peek4(32768+56*(x/8|0)+960*(y/8|0)+x+8*(y|0));
}
function sset(x,y,c){
	poke4(32768+56*(x/8|0)+960*(y/8|0)+x+8*(y|0),c);
}
```

```fennel
;parlortricks
;script:fennel
(fn sget [x y]
 "get sprite sheet pixel"
 (local addr (+ 0x4000 (* (+ (// x 8) (* (// y 8) 16)) 32)))
 (peek4 (+ (* addr 2) (% x 8) (* (% y 8) 8))))
```
```fennel
;parlortricks
;script:fennel
(fn sset [x y c]
 "set sprite sheet pixel"
 (local addr (+ 0x4000 (* (+ (// x 8) (* (// y 8) 16)) 32)))
 (poke4 (+ (* addr 2) (% x 8) (* (% y 8) 8)) c))
```

## Draw circle function
made by [Raidez](https://itch.io/profile/raidez)
``` lua
function _360(x,y,r,color,fill)
 if not fill then
  for a=1,360,1 do --draw outline only
   dx=math.cos(a)*r+x
   dy=math.sin(a)*r+y
   pix(dx,dy,color)
  end
 else
  for r=r,1,-1 do --draw circle for each radius (for fill circle)
   for a=1,360,1 do
    dx=math.cos(a)*r+x
    dy=math.sin(a)*r+y
    pix(dx,dy,color)
   end
  end
 end
end


function _bresenham(x,y,r,color)
 px=0
 py=r
 m=5-4*r

 while(px<=py) do
  pix((px+x),(py+y),color)
  pix((py+x),(px+y),color)
  pix((-px+x),(py+y),color)
  pix((-py+x),(px+y),color)
  pix((px+x),(-py+y),color)
  pix((py+x),(-px+y),color)
  pix((-px+x),(-py+y),color)
  pix((-py+x),(-px+y),color)

  if m>0 then
   py=py-1
   m=m-8*py
  end

  px=px+1
  m=m+8*px+4
 end
end


function _andres(x,y,r,color,fill)
 px=x
 py=y

 d=r-1
 a=r
 --a=r-1
 b=0

 while(a>=b) do
  if not fill then
   pix((px+b),(py+a),color)
   pix((px+a),(py+b),color)
   pix((px-b),(py+a),color)
   pix((px-a),(py+b),color)
   pix((px+b),(py-a),color)
   pix((px+a),(py-b),color)
   pix((px-b),(py-a),color)
   pix((px-a),(py-b),color)
  else
   line((px+b),(py-a),(px+b),(py+a),color)
   line((px+a),(py-b),(px+a),(py+b),color)
   line((px-b),(py-a),(px-b),(py+a),color)
   line((px-a),(py-b),(px-a),(py+b),color)
  end

  if d>=2*b then
   d=d-2*b-1
   b=b+1
  elseif d<2*(r-a) then
   d=d+2*a-1
   a=a-1
  else
   d=d+2*(a-b-1)
   a=a-1
   b=b+1
  end
 end
end


function _axis(cx,cy,r,color,fill)
 if not fill then
  for y=-r,r,1 do --outline
   for x=-r,r,1 do
    if (x*x+y*y)<=(r*r) then
     pix(cx+x,cy+y,color)
    end
   end
  end

  r=r-1
  for y=-r,r,1 do --unfill
   for x=-r,r,1 do
    if (x*x+y*y)<=(r*r) then
     pix(cx+x,cy+y,-1)
    end
   end
  end
 else
  for y=-r,r,1 do
   for x=-r,r,1 do
    if (x*x+y*y)<=(r*r) then
     pix(cx+x,cy+y,color)
    end
   end
  end
 end
end


function circ(x,y,r,color,algo)
 if algo=="360" then
  --don't use it, it's false/incomplete
  --_360(x,y,r,color,true)
 elseif algo=="bresenham" then
  _bresenham(x,y,r,color)
  _andres(x,y,(r),color,true)
 elseif algo=="andres" then
  _andres(x,y,r,color,true)
 elseif algo=="axis" then
  _axis(x,y,r,color,true)
 end
end


function circb(x,y,r,color,algo)
 if algo=="360" then
  --don't use it, it's false/incomplete
  --_360(x,y,r,color,false)
 elseif algo=="bresenham" then
  _bresenham(x,y,r,color)
 elseif algo=="andres" then
  _andres(x,y,r,color,false)
 elseif algo=="axis" then
  _axis(x,y,r,color,false)
 end
end

function TIC()
	cls()

	circb(100,60,40,14,"bresenham")
end
```

## Draw ellipse function on Moonscript
made by [harraps](https://itch.io/profile/harraps)
``` lua
-- script: moon
--function to draw an ellipse
--x,y : position of the ellipse
--A,B : major and minor radius
--ang : angle of the ellipse in radian
--col : color of the filled ellipse
ell=(x,y,A,B,ang,col)->
	--angles used
	cos=math.cos ang
	sin=math.sin ang
	cos2=math.cos ang+math.pi*0.5
	sin2=math.sin ang+math.pi*0.5
	--vectors in euclidian space
	ux=A*cos
	uy=A*sin
	vx=B*cos2
	vy=B*sin2
	--bounds of ellipse
	w=math.sqrt ux*ux + vx*vx
	h=math.sqrt uy*uy + vy*vy
	--rect x-w+1,y-h+1,w*2,h*2,8
	--for each pixel within the bounds
	for  x0=-w,w
		for y0=-h,h
			x1=( cos*x0 + sin*y0)/A
			y1=(-sin*x0 + cos*y0)/B
			if x1*x1 + y1*y1 < 1
				pix x+x0,y+y0,col
```

## Draw Bézier curves on Moonscript
made by [harraps](https://itch.io/profile/harraps)
```
-- title:  bezier
-- desc:   function to draw Bezier curves
-- script: moon
-- input:  gamepad

class Point
	new:(x,y)=>
		@x=x
		@y=y

Bezier =
	-- simple line
	linear:(t,p0,p1)->
		return{
			x:p0.x+t*(p1.x-p0.x)
			y:p0.y+t*(p1.y-p0.y)
			--z:p0.z+t*(p1.z-p0.z)
		}
	-- simple curve
	quadratic:(t,p0,p1,p2)->
		n =1-t
		t0=n*n
		t1=n*t*2
		t2=t*t
		return{
			x:t0*p0.x+t1*p1.x+t2*p2.x
			y:t0*p0.y+t1*p1.y+t2*p2.y
			--z:t0*p0.z+t1*p1.z+t2*p2.z
		}
	--better curve
	cubic:(t,p0,p1,p2,p3)->
		n =1-t
		t0=n*n*n
		t1=3*n*n*t
		t2=3*n*t*t
		t3=t*t*t
		return{
			x:t0*p0.x+t1*p1.x+t2*p2.x+t3*p3.x
			y:t0*p0.y+t1*p1.y+t2*p2.y+t3*p3.y
			--z:t0*p0.z+t1*p1.z+t2*p2.z+t3*p3.z
		}


class Linear

	new:(col,seg,x0,y0,x1,y1)=>
		@col=col
		@seg=seg
		@p0 =Point x0,y0
		@p1 =Point x1,y1

	draw:=>
		t=0
		pad=1/@seg
		pp=@p0
		for i=1,@seg
			t+=pad
			np=Bezier.linear t,@p0,@p1
			line pp.x,pp.y,np.x,np.y,@col
			pp=np
		line pp.x,pp.y,@p1.x,@p1.y,@col


class Quadratic

	new:(col,seg,x0,y0,x1,y1,x2,y2)=>
		@col=col
		@seg=seg
		@p0 =Point x0,y0
		@p1 =Point x1,y1
		@p2 =Point x2,y2

	draw:=>
		t=0
		pad=1/@seg
		pp=@p0
		for i=1,@seg
			t+=pad
			np=Bezier.quadratic t,@p0,@p1,@p2
			line pp.x,pp.y,np.x,np.y,@col
			pp=np
		line pp.x,pp.y,@p2.x,@p2.y,@col


class Cubic

	new:(col,seg,x0,y0,x1,y1,x2,y2,x3,y3)=>
		@col=col
		@seg=seg
		@p0 =Point x0,y0
		@p1 =Point x1,y1
		@p2 =Point x2,y2
		@p3 =Point x3,y3

	draw:=>
		t=0
		pad=1/@seg
		pp=@p0
		for i=1,@seg
			t+=pad
			np=Bezier.cubic t,@p0,@p1,@p2,@p3
			line pp.x,pp.y,np.x,np.y,@col
			pp=np
		line pp.x,pp.y,@p3.x,@p3.y,@col

c=Cubic 6,60, 0,0, 246,0, 0,136, 246,136

cls 1
c\draw!

export TIC=->
```

## <a name="peek4-and-poke4"></a>`peek4` and `poke4`

**Update:** [peek4](peek) and [poke4](poke) are now built-in API functions.

```js
// script: js
function peek4(address){
	var value=peek(address>>1);
	return address&1?value>>4:value&0x0f;
}
function poke4(address,value){
	var tmp=peek(address>>1);
	value&=0x0f;
	poke(address>>1,address&1?(tmp&0x0f)|(value<<4):(tmp&0xf0)|value);
}
```
```lua
-- script: lua
function peek4(address)
	local value=peek(address//2)
	if(address&1)then
		return value>>4
	else
		return value&0x0f
	end
end
function poke4(address,value)
	local tmp=peek(address//2)
	value=value&0x0f
	if(address&1)then
		tmp=(tmp&0x0f)|(value<<4)
	else
		tmp=(tmp&0xf0)|value
	end
	poke(address//2,tmp)
end
```

## BrainF**k on Moonscript
made by [harraps](https://itch.io/profile/harraps)
``` lua
-- script: moon
-- title:  brainfuck interpreter
-- author: by oSchyns
-- version 0.3.0


BF="++++ ++++"..
"[>+++++ +++++<-]"..
">++++.----- ----- -.--- ---."--..
--uncomment to test error output
--"<< force a tape outbound"


IN=""


--tape to perform computation
class Tape

    new:(l,m)=>
        @l=l  --cell limit
        @m=m  --loop the finite tape
        @t={} --tape
        @h=0  --read head
        --correct values
        @l-=1 if @l~=nil
        @m-=1 if @m~=nil

    --rhead out of the bounds of the tape
    unbound:=>
        if @m~=nil then return @h>@m
        @h<0

    --init nil cell to zero
    init:=> @t[@h]=0 if @t[@h]==nil

    --move read head to the left
    mvL:=>
        @h-=1
        if @m~=nil then if @h<0
            @h=@m

    --move read head to the right
    mvR:=>
        @h+=1
        if @m~=nil then if @h>@m
            @h=0

    --decreament cell
    decr:=>
        @init!
        @t[@h]-=1
        if @l~=nil then if @t[@h]<0
            @t[@h]=@l

    --increament cell
    incr:=>
        @init!
        @t[@h]+=1
        if @l~=nil then if @t[@h]>@l
            @t[@h]=0

    --ins: insert value in cell
    --out: get value from cell

    ins:(i)=> @t[@h]=i if i~=nil
    out:   => @t[@h]   or 0


--stack to hold indexes of brackets
class Stack
    new:    => @s={}
    top:    => @s[#@s]
    pop:    => @s[#@s]=nil
    push:(i)=> @s[#@s+1]=i




--automaton to execute the program
class Automaton

    new:(p,i,l,m)=>
        @t=Tape l,m --tape
        @s=Stack!   --stack
        @r=1        --read head
        @p=""       --program
        @i=i        --input
        @o=""       --output
        @e=nil      --error
        --strip useless chars of program
        for c in p\gmatch "[<>%+%-%[%]%.%,]+"
            @p..=c
        b=@check!
        if b~=nil
            @e="brackets mismatch at "..b

    --check for brackets mismatch
    check:=>
        s=Stack!
        for i=1,#@p
            if     @program(i)=='[' then s\push i
            elseif @program(i)==']'
                return i    if #s.s<=0
                s\pop!
        return s\top! if #s.s> 0
        return nil

    --get char from input
    input:=>
        if @i==nil or @i=="" then return nil
        c1=@i\byte! --1st char of i
        @i=@i\sub 2 --remove 1st char
        return c1

    --output:   add char to output
    --program:  get instruction at 'n'
    --continue: continue execution

    output: (n)=> @o..=string.char n
    program:(n)=> @p\sub n,n
    continue:  => @r<=#@p and @e==nil

    --find matching bracket
    match:(b)=>
        m=1
        while m>0 and b<=#@p
            b+=1
            if     @program(b)=='[' then m+=1
            elseif @program(b)==']' then m-=1
        return b

    --opening bracket
    open:=>
     --jump to matching bracket
        if  (@t\out!)==0 then @r=@match @r
        else @s\push @r

    --closing bracket
    clos:=>
        if    (@t\out!)==0   then    @s\pop!
        elseif @s\top! ~=nil then @r=@s\top!

    --automaton's' execution step
    step:=>
        switch @program @r
            when '<' then @t\mvL!
            when '>' then @t\mvR!
            when '-' then @t\decr!
            when '+' then @t\incr!
            when ',' then @t\ins  @input!
            when '.' then @output @t\out!
            when '[' then @open!
            when ']' then @clos!
        @r+=1
        if @t\unbound!
            @e="tape outbound !"




--display to see the execution
class Display

    new:(a,s=1,f=nil)=>
        @a=a    --automaton
        @s=s/60 --number of update per second
        @c=0    --counter
        @f=3    --display format of cells
        if     f=="hexadecimal" then @f=2
        elseif f=="character"   then @f=1

    --execute automaton all at once
    execute:=>
        while @a\continue!
            @a\step!
        @output!

    --update automaton at given frame rate
    update:=>
        if @a\continue!
            if @c<1 then @c+=@s
            else
                @c=0
                @a\step!
                @draw!

    --draw automaton state and output
    draw:=>
        cls!
        @tape    8, 2
        @program 8,12
        @output 4


    --draw the tape
    tape:(w=6,h=0,c=1,a=3)=>
        rect 0,h,240,w,c
        l=math.ceil (40/@f)*0.5
        p=(w-6)*0.5
        q=6*@f
        b=l-@a.t.h
        d=15
        if     @f==2 then d=6
        elseif @f==1 then d=3
        rect l*q-d,h,6*@f,w,a
        for i=@a.t.h-l,@a.t.h+l
            unless i<0
                c=15
                c=14 if i==@a.t.h
                v=@a.t.t[i] or 0
                if     @f==3 then v="%3d"\format v
                elseif @f==2 then v="%2x"\format v
                elseif @f==1 then v=string.char  v
                print v,(i+b)*q-d,h+p,c

    --draw the program
    program:(w=6,h=6,c=1,a=3)=>
        rect   0,h,240,w,c
        rect 117,h,  6,w,a
        p=(w-6)*0.5
        b=20-@a.r
        for i=@a.r-20,@a.r+20
            unless i<1
                c=15
                c=14 if i==@a.r
                v=@a\program i
                print v,(i+b)*6-2,h+p,c

    --print result of execution
    output:(s=0)=>
        i=s
        o=@a.o.."\n"
        --print each line
        for l in o\gmatch "[%S \t]*\n"
            print l,0,i*6
            i+=1
            break if i==21 --no more room
        --print error
        unless @a.e==nil
            print @a.e,0,130,6


--create automaton 'TM' using
--program : BF
--input   : IN
TM=Automaton BF,IN,128,nil


--create display 'DP' using
--automaton    : TM
--frame rate   : 30
--cell display : decimal
DP=Display TM,30,"decimal"


--execute the automaton and display it
export TIC=->
    DP\update!


--http://moonscript.org/reference/
--http://moonscript.org/compiler/
--https://esolangs.org/wiki/Brainfuck
--http://zacstewart.com/2013/09/15/learning-cpp-a-brainfuck-interpreter.html
```

## Write and read function from memory

```lua
-- title:  Write function to memory
-- author: Al Rado
-- desc:   Shows how write code to memory
-- script: lua

-- addres to write code block / sprites addres
ADDR = 0x4000

-- target function, for example
function prints(msg,x,y,c)
	for i=-1,1 do
		for j=-1,1 do
			print(msg,x+i,y+j,0);
		end
	end
	print(msg,x,y,c);
end

-- write dump of target function to memory
local funcBin = string.dump(prints, true)
for i=1,#funcBin do
  local code = funcBin:byte(i)
  poke(ADDR+i-1,code)
end
sync(0,0,true)

-- trace lenght of memory block
trace("len: "..#funcBin)
exit()

function TIC() end
```

After the function is written to the cartridge memory, it can be read and run.
It is important to specify the length of the code block (LEN) shown in the previous example

```lua
-- title:  Read function from memory
-- author: Al Rado
-- desc:   Shows how read code from memory
-- script: lua

-- addres of code block / sprites addres
ADDR = 0x4000

-- lenght of memory block in bytes
LEN = 211

-- get dump of function from memory
local funcStr=""
for i=1,LEN do
  local code = peek(ADDR+i-1)
  funcStr=funcStr..string.char(code)
end

-- load function from string
local prints = assert(load(funcStr));

-- call target function
cls(3)
prints("HELLO WORLD!",84,84,15)

function TIC() end
```

## Mute one channel of a music track

The 6-bit structure of the music tracks makes muting a single channel tricky. A channel consists of 16 patterns, and one 6-bit pattern is always span across two nibbles (a half-byte) that are from separate bytes. The other nibble also contains data from some other pattern, which should be left intact. To mute all patterns of the channel in a song, some bitmasking is needed.

```lua
-- title:  Mute one channel of a music track
-- author: @oggborbis
-- script: lua

function mutechannel(track, channel) --tracks 0-8, channels 1-4
    local offset=3*(channel+channel%2)/2-2-channel%2
    local mask
    if channel%2==0 then
        mask=0x3 --the high nibble is bitmasked empty for channels 2&4
    else
        mask=0xc --the low nibble is bitmasked empty for channels 1&3
    end
    for i=0,15 do --there are 16 sections in a track, each one taking 3 bytes for four 6-bit patterns
        local adr=(0x13e64+51*track+3*i)*2 --a track is 51 bytes long
		local q=adr+offset+channel%2
		poke4(q,peek4(q)&mask) --one half of this nibble (2bit) is bitmasked empty
		poke4(adr+offset+1-channel%2,0) --the other nibble (4bit) can be emptied
	end
end
```

## Sample RGB Color RAM Address
Go to page here [RGB Address Page](https://github.com/nesbox/TIC-80/wiki/Sample-RGB-Color-RAM-Address)

This RGB color **(Red Green Blue)** is mixed colors. Can make fade in with color.
Be simple code example. Easy to use animate colors.

```lua
-- title:  RGB color animation test
-- author: masternama
-- script: lua

sw=0 -- screen wait

function TIC()
  if sw>0 then
  cls() -- black color
  poke(0x3FC0,255) -- Red
end

if sw>10 then
  cls() -- black color
  poke(0x3FC1,255) -- Green
end

if sw>20 then
  cls() -- black color
  poke(0x3FC2,255) -- Blue
end

if sw>30 then
  reset() -- Loop reset
end

print("HELLO WORLD!",88,70)
--sync(0,0,true)

sw=sw+1
end
```


## Javascript palette functions


Some useful JavaScript 16-color palette functions for the console in your browser:
```js
// author: Anastasia Dunbar
function reverse(x){return typeof x==="string"?x.split("").reverse().join(""):x.reverse();}
function chunkString(string,n){
	var chunks=[];
	for(var i=0;i<string.length;i+=n){
		chunks.push(string.substring(i,i+n));
	}
	return chunks;
}
function hexChars(string){return string.replace(/[^0-9A-F]/gi,"");}
function hexTo24RGB(string){
	return chunkString(hexChars(string).substring(0,6),2).map(x=>parseInt(x,16));
}
function perceivedBrightness(R,G,B){ //Doesn't matter what range R, G and B is within.
	return Math.sqrt((.299*(R**2))+(.587*(G**2))+(.114*(B**2)));
}
function sortPaletteByBrightness(array){ //From dark to bright.
	array=array.map(x=>{
		if(typeof x==="string"){return hexTo24RGB(x);}
		return x;
	});
	return array.sort((x,y)=>perceivedBrightness(...x)-perceivedBrightness(...y));
}
function rgbArrayToHex(array){
	return array.flat().map(x=>x.toString(16).padStart(2,"0")).join("");
}
function hexToRgbArray(string){
	return chunkString(hexChars(string),6).map(x=>chunkString(x,2).map(y=>parseInt(y,16)));
}
function makeSortedPalette(string){
	return rgbArrayToHex(sortPaletteByBrightness(string.split(/\s+/).filter(x=>x)));
}
function visualizePalette(string){ //Normal TIC-80 palette string as a parameter.
	chunkString(string,6).forEach((x,i)=>{
		console.log(`${String(i).padStart(2)}: %c${x}`,`background-color:#${x};color:#${perceivedBrightness(...hexTo24RGB(x))<100?"fff":"000"}`);
	});
}
```

## Mouse scroll

**Update:** Mouse scroll can now be retrieved easily via the [mouse](mouse) API function.

Returns mouse Y-scroll as an integer from -32 to +31
``` lua
function mscroll()
	local scroll=(peek(0x0FF87)&0x7E)>>1
	return -(scroll&32)+(scroll&31)
end
```
## Interpolation functions
Linear interpolation
``` lua
function lerp(a,b,mu)
	return a*(1-mu)+b*mu
end
```
Cosine interpolation
``` lua
function coserp(a,b,mu)
	local T=(1-math.cos(mu*math.pi))/2
	return a*(1-T)+b*T
end
```
Cubic interpolation
``` lua
function cuberp(a,b,c,d,t)
	local A=d-c-a+b
	local B=a-b-A
	local C=c-a
	local D=b
	local T=t*t
	return A*t*T+B*T+C*t+D
end
```
## Normal random number
Return a random number in a pseudo-normal distribution with a mean of m and d as standard deviation (with a "quality" of q)
```lua
function randomnorm(m,d,q)
	if m==nil then m=0 end
	if d==nil then d=1 end
	if q==nil then q=10 end
	local sum=0
	for i=1,q*2 do
		sum=sum+(math.random())
	end
	val=(sum-q)*d+m
	if val%1>0.5 then val=math.ceil(val) end
	if val%1<0.5 then val=math.floor(val) end
	return val
end
```

## Sky gradient

Sky gradient it works for using `SCN()` or `scanline()` Page: [Sky gradient Page](https://github.com/nesbox/TIC-80/wiki/Sky-gradient)

```lua
-- title:  Sky Gradient
-- author: masternama
-- script: lua

--[[

 TODO: I used SCN() or scanline(),
	Color Gradient looks like e.g
	(sky, lava, water & shadow)

	Result will be smooth gradient...

]]

function SCN(scn)
 --poke(0x3fc0,scn) -- Unused Sky Color
	poke(0x3fc1,scn)
	poke(0x3fc2,scn+120)
end

function TIC()
 cls()
end
```
## Rainbow color function

```js

red=255
grn=0
blu=0
cyRG=true
cyGB=false
cyBR=false

function rainbowColor(slot) {

	if (cyRG == true) {
		grn++
	if (grn > 255) {
			grn = 255
			red--
		}
			if (red == 0) {
				cyRG=false
				cyGB=true
			}
		}

		if (cyGB == true) {
		blu++
		if (blu > 255) {
			blu = 255
			grn--
			if (grn == 0) {
				cyGB=false
				cyBR=true
			}
		}
	}

		if (cyBR == true) {
		red++
		if (red > 255) {
			red = 255
			blu--
			if (blu == 0) {
				cyBR=false
				cyRG=true
			}
		}
	}
	if (red==255) {
	}
		poke(0x3fc0+(slot*3)+2,blu)
		poke(0x3fc0+(slot*3)+1,grn)
		poke(0x3fc0+(slot*3),red)
}
```

## Rescale RangeA to RangeB
Sometimes you come across wanting to map/scale a number from one range to another. For example you have a numer from the range [0-10] and wants to scale it to [100-1000]. Using the rescale-linear function you could do the following:

```fennel
(fn _G.TIC []
 (cls)
 (print (rescale-linear 9 0 10 100 1000)))
```
The value 9 would end up being 910.0

### Rescales number linearly
```fennel
;parlortricks
;script:fennel
(fn rescale-linear [value start1 stop1 start2 stop2]
 "remap rangeA to rangeB for a given value"
 (+ start2 (* (- stop2 start2) (/ (- value start1) (- stop1 start1)))))
```

### Lapse of time 
```moon
-- script: moon
-- author: darltrash

lapse = (fn, ...) ->
	t = time()
	fn(...)
	time() - t

export TIC=->
	cls 6

	spent = lapse ->
		for x=0, 240/8
			for y=0, 136/8
				cx = math.sin(x*4+time()/300)*5
				cy = math.cos(y*4+time()/300)*5
				
				circ cx+x*8, cy+y*8, 3, 7
				
	print "Took: #{spent}ms", 10, 10, 12
```
```lua
-- script: lua
-- author: darltrash

local function lapse(fn, ...)
	local t = time()
	fn(...)
	return time() - t
end

function TIC()
	cls(6)

	local spent = lapse(function()
		for x=0, 240/8 do
			for y=0, 136/8 do
				local cx, cy
				
				cx = math.sin(x*4+time()/300)*5
				cy = math.cos(y*4+time()/300)*5
				
				circ(cx+x*8, cy+y*8, 3, 7)
			end
		end
	end)
				
	print("Took: " ..spent.. "ms", 10, 10, 12)
end
```