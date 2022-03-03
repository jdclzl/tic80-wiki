In [Part 1](https://github.com/nesbox/TIC-80/wiki/A-step-by-step-introduction-to-TIC-80,-Part-1:-The-Default-Cart), we went through the default cart of TIC-80. Now it's time to go through the general workflow of making games with TIC-80.

## Saving and loading

In the previous part, we created a new default cart with the command `new`. It might be a good idea to now _save_ it! That's done with the command

    save cartname.tic

After the first save we can just save the cart with plain `save` or by pressing **CTRL+S**.
And similarly, a cart is loaded with command

    load cartname.tic

(Tip: You can _autocomplete_ commands: In the previous command, you could just write `load c` and press **TAB** and TIC will complete the command!)

## TIC's file system

But where _are_ the carts?

They are inside the TIC's _data folder_, the only folder visible to TIC-80. For easy access, the folder can be revealed in Explorer with command `folder`.

You can use file system commands akin to unix to navigate and manipulate the folders and files inside the data folder:

Commands `ls` or `dir` show the contents of your current _working directory_. Folders are shown in gray inside `[brackets]`.

Move to a folder with command `cd foldername`. To go back to the _parent directory_, use command `cd ..`.

Additionally, you can use `mkdir foldername` to create a new, empty folder. Similarly, `del filename` can be used to delete files or folders. Be careful here, though!

## Adding demo carts

If there isn't any carts to see, we see the prompt

    use ADD or DEMO command to add carts

`add` copies a cart from any folder to TIC's data folder.

`demo` is a more interesting command: it adds nine _demo carts_ to the data folder.



## Carts on TIC's website

A good way to see the capabilities of any tool is to see what people have made with it. So where to start?


Well, there are many carts hosted on the [TIC's website!](https://tic80.com/play)


## My example cart

[an example file ](url)`buut.tic` which contains some basic collision checking and some ideas for code structuring.

Download the file to your computer. *You can't open a*`.tic` *file by just clicking it* - you have to open it in TIC-80. And to get TIC-80 to see it, move the file to TIC-80's **data folder,** which is the *only folder TIC-80 can see in your computer*. It's easily accessed by executing the command

    folder

Which opens the folder in file explorer. By default, the folder only contains the folder `.local `and nothing else.

Move the file `buut.tic` to the data folder (not inside `.local`). You can see the contents of the data folder with commands `ls `or `dir` and open the file with command

    load buut

(wip)

Storing data in tables

Three different kinds of for loops

Storing functions in tables….?

Tables as entities

Tallenna giffejä F9

Muista tallentaa cart consolesta käsin savella ennen ku voit tallentaa (voi ajaa ennen ku tallentaa)

some commands you'll need

**help** gives a list of available commands.

**new** loads a new default cart. It contains a very barebones Hello World example.

**save cartname.tic** saves the current cart as cartname.tic.

**load cartname.tic** loads the cart named cartname.tic.

The cartridges are stored in TIC's own data folder, you can open it with **folder** command.

To see the folder's contents, use the **dir** or **ls** command.

The one you need the most:** **

To play your game, use the **run** command. You can exit the game back to command line by pressing **ESC**. To resume your game, use the **resume** command.

**The editors**

The command line isn't everything, though: you can open the five **editors** with buttons **F1-F5** (or command **edit)**: code editor, sprite editor, map editor, sound effect editor and music editor. We're only going to need the first three editors now. You can jump between the editors and the command line with **ESC** button.

**The code editor**

Write your game code here! You can change the editor font to a bit smaller one with the **F** button on the top right. The three bars button shows the **outline**, or the *list of functions* in use.

**The sprite editor**

Draw your characters, backgrounds and everything else here.

**The map editor**

To jump between close-up view and a general view of the whole map, use **Tab**.

To add stuff to the tile map, press and hold **Shift** and choose the tile you want to use.

**music editor**
press 1 to add "stop note" command
press delete to remove a note OR aforementioned stop note command.
