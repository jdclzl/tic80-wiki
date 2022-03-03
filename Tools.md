The following is a list of tools that can be used outside of TIC-80 to assist with the creation of carts.

- [Bundling](#bundling)
- [Maps](#maps)

## Bundling

TIC-80 does not support loading external code files, so having multiple files and bundling them together into the cart can ease code writing.

* [ScriptPacker](https://github.com/RobLoach/scriptpacker): Package multiple Lua, Wren, Squirrel, or Javascript scripts together, keeping the dependency chain intact.
* [tic-bundle](https://github.com/chronoDave/tic-bundle#readme): Simple CLI tool for bundling TIC-80 cartridge code. Supports any language.
* [TQ-Bundler](https://github.com/scambier/TQ-Bundler): A zero-config single-file executable bundler/watcher/launcher. Supports any language.
* [Amalg](https://github.com/siffiejoe/lua-amalg): A tool that can package a Lua script and its modules into a single file.
* [tic80-stitcher](https://github.com/jahodfra/tic80-stitcher): A tool for building the cartridge from multiple files. Useful when you generate part of the cartridge during the build.
* [tic80-typescript](https://github.com/scambier/tic80-typescript): write your games in TypeScript. Handles transpilation, bundling, and minification.

## Maps

While the TIC-80 map editor is very functional, it can be nice to use external tools to enhance the map creation experience.

* [TiledMapEditor-TIC-80](https://github.com/AlRado/TiledMapEditor-TIC-80): Converts between `.tmx` files to TIC-80 `.map` files
* [Fantasy Console Map Tool](https://monstersgoboom.itch.io/fcmt): Fills a gap between desktop 2D tilemap editing programs and fantasy consoles.