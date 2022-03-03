### Cartridge Metadata

Metadata is simply a small block of tagged comments that provides additional information to TIC-80.  It's good practice to declare metadata at the  very top of your source code.

Example

```lua
-- title:  Worm
-- author: Bob Brown
-- desc:   A Clone of the classic Snake
-- script: lua

-- rest of your program goes here
```

_Note:_ If your code is in an external file, **dofile** must be placed in the very first line, **before** the metadata. (`dofile` is deprecated as of 0.80)


The full list of metadata tag:

``` lua
-- title:  game title                     -- The name of the cart.
-- author: game developer                 -- The name of the developer.
-- desc:   short description              -- Optional description of the game.
-- script: lua (or moon/wren/js/fennel)   -- Identifies the scripting language used; Lua is the default and most commonly used for TC-80 development.
-- input:  gamepad (or mouse or keyboard) -- Selects gamepad, mouse or keyboard input source. All input types are enabled by default.
-- saveid: MyAwesomeGame                  -- Allows save data to be shared within multiple games on a copy of TIC.
```

#### `title` and `author` tags

The `title` and `author` tags (as well as a cover image) are **required** for uploading to the official website.

#### `desc` tags

Should include a short description of your game.

#### `script` tag

You can use any of the other scripting languages that TIC-80 supports by adding one the following to the metadata:

* For [Moonscript](https://github.com/leafo/moonscript) support `-- script: moon` tag
* For [Javascript (ES5)](javascript) support `// script: js` tag
* For [Fennel](https://fennel-lang.org) support `;; script: fennel` tag
* For [Wren](http://wren.io/) support `// script: wren` tag
* For [Squirrel](http://squirrel-lang.org/) support `// script: squirrel` tag

#### `input` tag

To enable [mouse input](mouse-input) only (and disable gamepad/keyboard input) add the `-- input: mouse` tag.

#### `saveid` tag

It's highly recommended if using [pmem](pmem) to use a unique `saveid`.  This will allow your cartridge's
persistent memory to persist even if you are still continuing to edit the code.  Otherwise a `saveid`
will be generated for you based on a MD5 hash of your code.


### The TIC-80 API

Please see [API Reference](API) for a full list of built-in functions or to download
a quick function reference cheat sheet.

### Using an External Editor

Remember, it's also easy to use an [external editor](external-editor) to write your code.