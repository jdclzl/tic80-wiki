# poke*

_This API was extended in version 0.9 and 1.0._

- `poke addr val`
- `poke addr val [bits=8]` *added in 0.9*
- `poke4 addr4 val4`
- `poke2 addr2 val2` *added in 1.0*
- `poke1 bitaddr bitval` *added in 1.0*


Or using `bits` argument:

- `poke addr4 val4 bits=4`
- `poke addr2 val2 bits=2`
- `poke bitaddr bitval bits=1`

## Parameters

* **addr** : the address of [RAM](RAM) you desire to write (segmented based on `bits`)
* **val** : the integer value write to RAM (range varies based on `bits`)
* **bits** : the number of bits to write (1, 2, 4, or 8; default: 8)


## Description
This function allows you to write directly to [RAM](RAM).  The requested number of bits is written at the address requested.  The address is typically specified in hexadecimal format.

For in-depth detail on how addressing works with various `bits` parameters, please see [peek](peek).

See also:

- [peek](peek) - Read from a memory address


## Example

![Example 1](https://imgur.com/AOUr7qX.gif)

``` lua
-- demo poke
function TIC()

--Make some video noise
for i=0,(240*136)/2-1 do
 poke(0x0000+i,(i*i*time())/3000000000%2+1)
end

--Sound it
m=0
for i=30,80 do
 m=m+peek(0x0000+6000+i)
end
f=math.floor((10+((1+m)/20))*400)
poke(0xFF80+0,f&0XFF)
poke(0xFF80+2,(f&0XFF00)>>8)
poke(0xFF80+8,f&0XFF)
poke(0xFF80+10,(f&0XFF00)>>8)

end
```

## Example poke (2 player mode)

![Example 2](https://imgur.com/gcrReEf.gif)

``` lua
-- demo poke
l1=49
w1=18
l2=21
w2=12

function TIC()

--Video noise
for i=0,(240*136)/2-1 do
 poke(0x0000+i,(i*i*time())/3000000000%2+1)
end

--Sounds
if btnp(0,3,3)then l1=l1-1 end
if btnp(1,3,3)then l1=l1+1 end
if btnp(2,3,3)then w1=w1-1 end
if btnp(3,3,3)then w1=w1+1 end

l1=l1<136 and l1 or 135
l1=l1>0 and l1 or 0
w1=w1<120 and w1 or 112
w1=w1>2 and w1 or 2

if btnp(8,3,3)then l2=l2-1 end
if btnp(9,3,3)then l2=l2+1 end
if btnp(10,3,3)then w2=w2-1 end
if btnp(11,3,3)then w2=w2+1 end

l2=l2<136 and l2 or 135
l2=l2>0 and l2 or 0
w2=w2<120 and w2 or 112
w2=w2>2 and w2 or 2

--S1
off1=l1*120
m=0
for i=60-w1/2,60+w1/2 do
	m=m+peek(0x0000+off1+i)
end

f=(10+((1+m)/w1))*800
f=math.floor(f)
print(l1..'-'..w1..'-'..f,0,0)
poke(0xFF80+0,f&0XFF)
poke(0xFF80+2,(f&0XFF00)>>8)

--Draw line
for i=60-w1/2,60+w1/2 do
	poke(0x0000+off1+i,136)
end

--S2
off2=l2*120
m=0
for i=60-w2/2,60+w2/2 do
	m=m+peek(0x0000+off2+i)
end

f=(10+((1+m)/w2))*800
f=math.floor(f)
print(l2..'-'..w2..'-'..f,0,10)
poke(0xFF80+8,f&0XFF)
poke(0xFF80+10,(f&0XFF00)>>8)

--Draw line
for i=60-w2/2,60+w2/2 do
	poke(0x0000+off2+i,102)
end

end
```