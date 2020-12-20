# Vim Specific Features

Vim commands and features not in your grandfather's vi.

## Jump Lists

Associated with each vim window (not buffer!) is a list of
past locations "jumped" to.  Jumps are remembered in a jump
list.  Just navigating via the `hjkl` keys will not create
jump points.  Nor will editing text.

| Command          | Description                          |
|:---------------- |:------------------------------------ |
| `:jumps`         | List jump points for active window   |
| `:help jumplist` | For more detailed info on jump lists |

The `:jumps` command will list a table consisting like this:

| jump | line | col | file/text                 |
|:----:| ----:| ---:|:------------------------- |
|  3   | 23   | 5   | previousFile.txt          |
|  2   | 2    | 11  | previousFile.txt          |
|  1   | 10   | 5   | some text in current file |
|  0   | 12   | 2   | other text current file   |
|  1   | 5    | 10  | text on line 5            |
|  2   | 3    | 5   | moreRecentFile.txt        |

Your current location in the jump list is always 0.

| Command   | Description                                 |
|:---------:|:------------------------------------------- |
| `<C-o>`   | go back to previous location in jump list   |
| `<C-i>`   | go forward to next location in jump list    |
| `3 <C-o>` | go back 3 jumps in jump list                |
| `2 <C-i>` | go forward 2 jumps in jump list             |

## Types of registers

### Default register

The default register has a name `""`, that is double-quote double-quote.

### Numbered registers

These contain only multiline (one or more whole lines) data.

| Register       | Purpose                                        |
|:--------------:|:---------------------------------------------- |
| `"0`           | contains contents of most recent yank command  |
| `"1`           | contains most recent delete/substitution       |
| `"2` thru `"9` | contents shift downward when `"1` is updated   |

These can be written to in Command Mode via `:let @5 = "foobar"`

### Small delete register

| Register       | Purpose                                      |
|:--------------:|:-------------------------------------------- |
| `"-`           | contains deletes/changes less than one line  |

### Named registers

| Register       | Purpose                                               |
|:--------------:|:----------------------------------------------------- |
| `"a` thru `"z` | storage registers across all file buffers             |
| `"A` thru `"Z` | same registers, used to appending instead of replace  |

### Read only registers

| Register  | Purpose                                      |
|:---------:|:-------------------------------------------- |
| `".`      | contains last inserted text                  |
| `"%`      | contains the name of the current file        |
| `":`      | contains most recent *Command Mode* command  |

Use `:@:` to repeat last *Command Mode* command.

### Alternate file register

The alternate file register `"#` is an assignable name, useful when jumping
between 2 buffers via <C-^>.

### Expression register

The expression register `"=` is used for expressions in commands
which use registers.

### Selection and drop registers (Interacts with Desktop GUI)

| Register  | Purpose                                  |
|:---------:|:---------------------------------------- |
| `"*`      | copy/paste from/to the X11 clipboard     |
| `"+`      | copy/paste from/to desktop clipboard     |
| `"~`      | paste from last drag-and-drop operation  |

On Arch, the first two only seem to work in vim when the gvim
package is installed.  I've never gotten the last one to work
at all, but this may have to do with how tightly I have the
mouse locked down.

### Black hole register

The black hole register `"_` allows deleting text without affecting
other registers.  Reading from it returns nothing.

### Last search pattern register

The last search pattern register `"/` is readable from *Normal Mode*.
You can assign values to it in *Command Mode* via

```
   :let @/ = "Some String"
```

## Vim Macros

A useful *Normal Mode* feature of vim is the `.` command which
repeats the last *Normal Mode* command which changed text.  Combining
with the `n` command is an extremely useful and powerful paradigm.

But, what if you want to do a series of commands between searches?  The
vim macro feature comes to the rescue.

This feature allows you to repeat a sequence of *Normal Mode* and
*Command Mode* commands.  Macros are stored in vim registers.

To create a macro, issue the *Normal Mode* `q` command followed by a
vim register name, say `a`.  At the bottom of the screen you see the
text `recording @a`.  Issue both *Normal Mode* and *Command Mode*
commands and edit as usual.  To finish, issue another
*Normal Mode* `q` command.  At a later time, to execute this macro
and repeat the sequence of commands, type `@a`.

As an example, say you want to change instances of "Unix programming"
or "Unix System programming" in similar ways.  You want "Unix" replaced
by "UNIX" and the "p" capitalized:

```
    qa/Unix<CR>l~~~fp~q
    @a@a@a@a
```

You may want to be able to choose whether to perform the macro or not.
For instance, you don't want to change "Unix is perfect".

```
    /Unix<ret>
    qbl~~~fp~q
    nn@bn@bnnn2@b
```

---

| prev: [Adv Trad Vi Commands][1] | [Home][2] | next: [Multiple Vim Windows][3] |

[1]: <advTradViCommands.md>
[2]: <README.md>
[3]: <multipleVimWindows.md>
