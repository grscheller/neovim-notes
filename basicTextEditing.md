# Basic Text Editing

This should be enough to enable you to be productive with nvim/vim
as a text editor.

I think with a few months of practice, the material covered here
can be internalized and eventually become part of your "muscle memory."

## Vim has 4 main modes

* [Normal Mode](#normal-mode)
* [Insert Mode](#insert-mode)
* [Command Mode](#command-mode)
* [Visual Mode](#visual-mode)

---

* [Other Useful Vim Information](#useful-vim-information)

---

## Normal Mode

### Cursor movement in Normal Mode

| Command       | Description                                       |
|:-------------:|:------------------------------------------------- |
| `h,j,k,l`     | move cursor one character (also arrow keys)       |
| `w, W`        | move forward to beginning next word               |
| `b, B`        | move back to beginning word                       |
| `e, E`        | move forward to end of word                       |
| `$`           | move to end of line                               |
| `^`           | move to first non-whitespace character on line    |
| `0`           | move to beginning of line                         |
| `G`           | move to last line in file                         |
| `gg`          | move to first line in file                        |
| `f<char>`     | move forward to next `<char>` on current line     |
| `F<char>`     | move backward to next `<char>` on current line    |
| `t<char>`     | move forward before next `<char>` on current line |
| `T<char>`     | move backward after next `<char>` on current line |
| `;`           | next target for last `f`, `F` ,`t` ,`T` command   |
| `,`           | prev target for last `f`, `F`, `t`, `T` command   |
| `3w`          | move forward 3 words on current line              |
| `5l`          | move forward 5 characters on current line         |
| `(`           | move to start of "sentence" (file type dependant) |
| `)`           | move to start of next "sentence"                  |
| `{`           | move to start of "section" (file type dependant)  |
| `}`           | move to start of next "section"                   |
| `/RegExp<CR>` | forward search for regular expression pattern     |
| `?RegExp<CR>` | backward search for regular expression pattern    |
| `/<CR>`       | search forward for last pattern                   |
| `?<CR>`       | search backward for last pattern                  |
| `n`           | search forward or backward for last pattern       |
| `N`           | search for last pattern in reverse sense of above |

### Changing text and/or interacting with the default register

| Command   | Description                                             |
|:---------:|:------------------------------------------------------- |
| `dd`      | delete line and put in default register (cut)           |
| `3dd`     | delete 3 lines and put in default register              |
| `D`       | delete to end of line and put in default register       |
| `yy`      | yank line to default register (copy)                    |
| `Y`       | yank line to default register (copy)                    |
| `x`       | delete character under cursor, put in default register  |
| `X`       | delete character before cursor, put in default register |
| `~`       | change case of current char and advance one char        |
| `r<char>` | change current char to `<char>`                         |
| `p`       | paste default register contents "after"                 |
| `P`       | paste default register contents "before"                |
| `J`       | join curent & next line, insert spaces as needed        |

What "before" or "after" mean depends on what is in the
default register.  Both `y` and `d` can be used with all
the *normal mode* cursor positioning commands.

| Command | Description                                                 |
|:-------:|:----------------------------------------------------------  |
| `d$`    | delete to end of line and put in default register           |
| `d0`    | delete everything before cursor and put in default register |
| `3yw`   | yank three words to default register, starting at cursor    |
| `y^`    | yank everything before cursor to first non-whitespace char  |
| `d2fz`  | delete from cursor to 2nd z on current line                 |
| `2db`   | delete 2 previous words starting from cursor                |
| `2y3w`  | ends up yanking 6 words                                     |
| `5x`    | delete next 5 characters on current line                    |
| `5X`    | delete previous 5 characters on current line                |

### You can use named registers to store text

In vi these were referred to as named buffers.

| Command | Description                                    |
|:-------:|:---------------------------------------------- |
| `"adw`  | delete word and put in register `"a`           |
| `"B2yy` | yank 2 lines and append to register `"b`       |
| `"sd$`  | delete to end of line and put in register `"s` |
| `"sp`   | paste contents of register `"s` after cursor   |
| `"aP`   | paste contents of register `"a` before cursor  |

One use case for named registers is copying multiple items
from multiple files and pasting them into other files.

### Commands to insert or manipulate text

These *Normal Mode* commands take vim to *Insert Mode*.
To return to *Normal Mode*, type either `<Esc>` or `<C-[>`.

| Command | Description                                                     |
|:-------:|:--------------------------------------------------------------- |
| `i`     | insert text before character cursor is on                       |
| `I`     | insert text at beginning of line after initial white space      |
| `0i`    | insert text beginning of line                                   |
| `a`     | insert text after character cursor is on                        |
| `A`     | insert text at end of line                                      |
| `o`     | open new line after current line in insert text                 |
| `O`     | open new line before current line in insert text                |
| `s`     | delete current character and enter *Insert Mode*                |
| `S`     | delete line contents and enter *Insert Mode*                    |
| `C`     | change to end of line                                           |
| `3cw`   | change next three words starting at cursor                      |
| `c3w`   | change next three words starting at cursor                      |
| `5cc`   | change next 5 lines                                             |
| `3cb`   | change previous 3 words                                         |
| `c$`    | change to end of line                                           |
| `c^`    | change text before cursor, excluding initial white space        |
| `c0`    | change text before cursor to beginning of line                  |
| `ciw`   | change inner word (change word cursor is on)                    |
| `cis`   | change inner sentence (works best for prose)                    |
| `"a3S`  | delete 3 lines into `"a`, enter *Normal Mode* on new line       |
| `"b3C`  | delete rest of line & next 2 two into `"b`, enter *Normal Mode* |

### Repeating commands in Normal Mode

| Command | Description                                |
|:-------:|:------------------------------------------ |
| `.`     | repeat the last command which changed text |

This repeats the last *Normal Mode* command used which changed text.
It does not repeat *Command Mode* commands.

This is frequently used with the `n` or `;` *Normal Mode* commands.
For example, `n.n.nn.n` keeps moving to the beginning of the next match
for the last search pattern where you can either decide to repeat, or
not, the change at each location.

---

## Insert Mode

The whole vi paradigm is that you do all navigation in *normal mode*
and type text in *insert mode*.  You return to *normal mode*
by pressing the `<Esc>` key.

### Navigating in Insert Mode

Sometimes it is convenient to be able to navigate while
in *insert mode*.  I tend to do this only to navigate near
where the cursor is.

Most "out of the box" vim configurations allow you to navigate
with the arrow keys while in *Insert Mode*.  Usually text can also
be deleted with the backspace key.  In *Normal Mode*, the backspace
and space keys are just extra navigation keys.

It is also possible to perform a single *normal mode* action within
*insert mode* by using`<C-o>` key sequences.

| Command   | Description                              |
|:---------:|:---------------------------------------- |
| `<C-o>h`  | move cursor left one character           |
| `<C-o>l`  | move cursor right one character          |
| `<C-o>k`  | move cursor up one line                  |
| `<C-o>j`  | move cursor down one line                |
| `<C-o>3w` | move cursor three words left             |
| `<C-o>2j` | move down two lines                      |
| `<C-o>J`  | join current line with the next line     |
| `<C-o>D`  | delete everything to the right of cursor |

### Other Insert Mode commands

| Command       | Description                                     |
|:-------------:|:----------------------------------------------- |
| `<C-w>`       | delete word to left of cursor                   |
| `<C-u>`       | delete everything to left of cursor             |
| `<C-h>`       | delete character to left of cursor              |
| `<C-j>`       | insert newline - why not just press `<return>`? |
| `<C-t>`       | indent current line one tab stop                |
| `<C-d>`       | un-indent current line one tab stop             |
| `<C-v><char>` | insert literal character                        |

---

## Command Mode

Vim is an open source version of the Unix editor vi,
which is the visual interface of the Berkeley Unix
line editor ex, which itself is a re-implementation of
the AT&T Unix line editor ed.  On really old terminals,
essentially line printers with keyboards, the descendants
of teletypes, you edited files one line at a time.

*Command Mode* commands developed from the original ex
line editing commands.

Use the `:` command to enter *Command Mode*.  The
cursor jumps down to the bottom of the terminal window
and prompts you with `:`.

| Command             | Description                                          |
|:------------------- |:---------------------------------------------------- |
| `:w`                | write to disk file being edited                      |
| `:w file`           | write to file, still editing original file           |
| `:q`                | quit editing, will warn if unsaved changes           |
| `:wq`               | write current buffer to disk, then quit current view |
| `:wa`               | write all buffers to disk                            |
| `:q!`               | quit current view without saving unsaved changes     |
| `:qa`               | quit program if there are no unsaved changes         |
| `:qa!`              | quit program without saving unsaved changes          |
| `:n`                | edit next buffer typically next file on command line |
| `:next`             | edit next buffer typically next file on command line |
| `:prev`             | edit previous buffer                                 |
| `:wn`               | write to disk and move on to next file to edit       |
| `:42`               | move cursor to beginning of line 42                  |
| `:#`                | give line number of current line cursor is on        |
| `:s/foo/bar/`       | substitute first instance of foo with bar            |
| `:s/foo/bar/g`      | substitute all instances of foo with bar             |
| `:17,42s/foo/bar/g` | substitute all foo with bar, lines 17 to 42          |

### Navigating the Command Mode line

While in *Command Mode*, up & down arrow keys cycle through previous
*Command Mode* commands.  The left & right arrow keys help you
re-edit the line.  Press `<Esc>`or`<C-[>` to return to *Normal Mode* without
issuing a command.

---

## Visual Mode

This mode allows you to select region of text by visually highlighting,
and then modify as a unit.

To enter *Visual Mode* from *Normal Mode*

| Command | Description                |
|:-------:|:-------------------------- |
| `v`     | for character based        |
| `V`     | for line based             |
| `<C-v>` | for block visual mode      |
| `gv`    | to reselect last selection |

Highlight text with *Normal Mode* cursor navigation commands
like `h`, `j`, `k`, `l`, `w`, `e`, `W`, `B`, `f` or the arrow keys.
Once selected, you can issue either *Normal Mode* or
*Command Mode* commands.

*Normal Mode* commands such as `d`, `y`, `c`, `I`, `A`, `>>`, `<<`, `/`
act on the highlighted region.  The behavior of some
commands, like indenting commands `>>` or `<<`, vary
depending on which *Visual Mode* (character, line or block)
you are in.

*Command Mode* commands act on lines in their entirety
that contain the selected region.

To punt out of *Visual Mode* without doing anything,
press the `<esc>` key.

If you have enabled mouse support, mouse actions can cause you
to enter *Visual Mode*.  That is one reason I enable mouse
support for *Normal Mode* only.

---

## Useful Vim Information

### Undo/redo commands

| Command | Description        |
|:-------:|:------------------ |
| `u`     | undo previous edit |
| `<C-r>` | redo edit undone   |

These can be used to linearly undo and redo edits,
like the arrow buttons in a web browser.
Navigating with the arrow keys while in *Insert Mode*
will result in multiple entries undo/redo levels.

### Some Vim/Neovim command line option examples

```
   $ nvim file1 file2 file3  # Open/create 3 files for editting
   $ nvim +[n] file     # Open file for editing on line n,
                        # defaults to last line of file
   $ nvim +/pattern file  # Open file for editing at first reg-exp pattern match
   $ nvim -R file         # Open file read only, can still write via :w!
   $ vim -g file  # Run as gvim GUI
   $ nvim -r       # List swap files, then exit
   $ nvim -r file  # Recover crashed vim session, uses swap file
   $ nvim -h       # List help message for command-line options and exit
```

### Dealing with whitespace characters

| Command       | Description                            |
|:------------- |:-------------------------------------- |
| `:set list`   | Indicate line endings & tabs           |
| `:set nolist` | Display line endings & tabs normally   |
| `:%s/ \+$//`  | Strip off trailing spaces on all lines |

Helps when getting rid of tabs and trailing whitespace.

### Spell checking

| Command        | Description             |
|:-------------- |:----------------------- |
| `:set spell`   | Turn spell checking on  |
| `:set nospell` | Turn spell checking off |

### Detailed help

To get started, from within vim, type

* `:help`
* `:help help`

Vim/Neovim built in help is very powerful, but not beginner friendly.
To get the most out of it,

* Use `<C-]>` or `double-click` mouse to follow vim "hyperlinks"
* Use `<C-o>` to jump back to previous location
* Use `<C-i>` or `<Tab>` to jump forward again
* familiarize yourself with how to use [multiple vim windows](multipleVimWindows.md)
* configure the [mouse](vimFactoids.md#using-the-mouse)
* setting up the [wildmenu](vimFactoids.md#configuring-wildmenu)

---

| [Absolute Minimal Text Editing][1] | next: [Vim Factoids][2] |

[1]: absoluteMinimalTextEditing.md
[2]: vimFactoids.md
