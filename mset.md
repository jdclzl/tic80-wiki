# mset

`mset x y tile_id`

## Parameters

* **x, y** : map coordinates
* **tile_id** : The background tile (0-255) to place in map at specified coordinates.

## Description

This function will change the tile at the specified [MAP](ram#map) coordinates. By default, changes made are only kept while the current game is running. To make permanent changes to the map (persisting them back to the cartridge), see [sync](sync).

### Related

* [map](map)
* [mget](mget)
* [sync](sync)