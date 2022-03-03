# Console

- [Console](#console)
    - [Available Commands](#available-commands)
    - [Evaluating Code](#evaluating-code)

---

![Console Demo](https://raw.githubusercontent.com/wiki/nesbox/tic.computer/images/console.gif)

### Available Commands

* `help` - Show available commands
* `vram` - Show 16K [VRAM](RAM#vram) layout (0.80)
* `ram` - Show 80K [RAM](RAM) layout
* `exit` / `quit` - Exit the application
* `edit` - Switches to the code editor
* `new [lua | moon | js | wren | fennel | ruby]` - Creates a new [Hello World cartridge](hello-world)
* `load <cart> [sprites | map | cover | code | sfx | music | palette]` - Load cartridge <cart> from the local filesystem (there's no need to type the .tic extension). You can also load just the data (sprites, map etc) from another cart
* `save <cart>` - Save cartridge <cart> to the local filesystem, use `.lua / .js / .moon` cart extension to save it in text format **[PRO version]**
* `run` - Run current project
* `resume` - Resume last loaded project
* `eval` / `=` - run code
* `dir` / `ls` - Show list of local files
* `cd` - Change directory
* `mkdir` - Make directory
* `folder` - Open working directory in OS (Windows, Linux, Mac OS)
* `add` - Show file open dialog to add file to TIC
* `del <file>` - Delete <file> from the filesystem
* `get <file>` - Show file save dialog to download file
* `export [html | native | sprites | cover | map | sfx | music]` - Export cart to HTML or as a native build, export sprites or cover as image, or export sfx and music to wav files (0.80). Use `alone=1` to export the game without the editors **[PRO version]**
* `import [code | map | screen | sprites | tiles]` - Import code/images from an external file
* `cls` / `clear` - Clear screen
* `demo` - Install demo carts
* `version` - Show the current version (0.60.3)
* `config [reset | default]` - Show **config.tic** file editor, use **reset** param to reset current configuration, use **default** to edit default cart template
* `surf` - Open carts browser
* `menu` - Show game menu where you can setup keyboard/gamepad buttons mapping (0.80)

For those operating systems that support it, tab completion and command history is available in the console.

### Evaluating Code

You can also evaluate code from the console using [eval](eval). To log the results use [trace](trace).

```
> eval trace("Hello World")
Hello World
> eval trace(2+2)
4
```