# fget

_This function was added to the API in version 0.80._

`fget sprite_id flag -> bool`


## Parameters

* **sprite_id** : sprite index (0..511)
* **flag** : flag index to check (0..7)


## Returns

* whether the flag was set (true/false)


## Description

Returns `true` if the specified flag of the sprite is set.

See also:

* [fset](fset) (0.80)