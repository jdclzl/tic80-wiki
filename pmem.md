# pmem

`pmem index -> val32` Retrieve value from persistent memory

`pmem index val32 -> prior_val32` Save new value to persistent memory, retrieve prior value.

**Parameters**

* `index` : an index (0..255) into the persistent memory of a cartridge.
* `val32` : the 32-bit integer value you want to store. Omit this parameter to read vs write.

**Returns**

* `val32` : the current/prior value saved to the specified memory slot.

This function allows you to save and retrieve data in one of the 256 individual 32-bit slots available in the cartridge's persistent memory. This is useful for saving high-scores, level advancement or achievements. Data is stored as unsigned 32-bit integer (from 0 to 4294967295).

When writing a new value the previous value is returned.

### Memory Persistence
* pmem depends on the cartridge hash (md5), so don't change your lua script if you want to keep the data.
* Use *saveid:* with a personalized string in the header [metadata](https://github.com/nesbox/TIC-80/wiki/the-code#cartridge-metadata) to override the default MD5 calculation. This allows the user to update a cart without losing their saved data.

## Example

![Example](https://imgur.com/UVV00WN.png)

``` lua
--pmem demo
--load saved value at slot zero and save it back
--incremented by 1
pmem(0,pmem(0)+1)

function TIC()
 cls()
 print("Started "..pmem(0).." times");
end

```