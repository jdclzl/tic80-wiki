# Command Line Arguments

Native versions of TIC support some arguments when called from the command line or other programs (such as an external editor). Absolute and relative file paths are supported.

Which version:

- [Stable Release](#stable-release) (0.90)
- [Old Build](#old-build) (0.80)

## Stable Release

```
$ ./tic80 -h
Usage: tic80 [cart] [options]

TIC-80 startup options:

    -h, --help        show this help message and exit
    --skip            skip startup animation
    --nosound         disable sound output
    --fullscreen      enable fullscreen mode
    --fs=<str>        path to the file system folder
    --scale=<int>     main window scale
    --cmd=<str>       run commands in the console
    --version         print program version
```

#### Examples

    tic80 --skip --fs .                                           skip startup animation and use current directory as storage
    tic80 --skip --cmd "load cart.tic"                            skip startup animation and load `cart.tic` cart
    tic80 --cmd surf                                              start TIC-80 in SURF mode
    tic80 --cmd "load cart.tic & export html game.zip & exit"     load `cart.tic` cart, export html `game.zip` and exit
    tic80 --cmd "load cart.tic & save cart.lua & exit"            load `cart.tic`, save as a text cart and exit
    tic80 --cmd "load cart.tic & export sprites spr.gif & exit"   load `cart.tic`, export sprites to `spr.gif` and exit
    tic80 --cmd "load cart.tic & export track5 music.wav & exit"  load `cart.tic`, export 5th track to `music.wav` and exit
    tic80 ../path/to/cart.tic --fs ../path/to                     use `../path/to/` as storage and load `cart.tic`
    tic80 > log.txt                                               all trace output is redirected to the named file
    tic80 | Out-File log.txt                                      same as above for PowerShell users

## Old Build

0.80 series:

    tic80 .                                  use current directory as storage
    tic80 -code game.lua                     load and run the code without startup animation
    tic80 -code-watch game.lua               same as -code but also reload it when TIC is focused
    tic80 game.tic -code game.lua            load cartridge, inject the code and run without startup animation
    tic80 cart.tic -sprites blahblah.gif     inject sprites from the given gif format file to cart.tic
    tic80 -surf                              start TIC-80 in SURF mode
    tic80 -nosound                           start in silent mode
    tic80 -fullscreen                        start in fullscreen mode
    tic80 -skip                              skip startup animation (0.60.0)

You can also run [console commands](Console) by passing them in from the command line. Commands consisting of more than one word should be enclosed in quotes, for example:

    tic80 path/to/cart.tic "export html"
