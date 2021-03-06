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

* [Other Useful Vim Information](#other-useful-vim-information)

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

| Command | Description                                              |
|:-------:|:-------------------------------------------------------- |
| `"adw`  | delete word and put in register `"a`                     |
| `"B2yy` | yank 2 lines and append to register `"b`                 |
| `"sd$`  | delete to end of line and put in register `"s`           |
| `"sp`   | paste contents of register `"s` after char cursor is on  |
| `"aP`   | paste contents of register `"a` before cursor            |

One use case for named registers is copying multiple items
from multiple files and pasting them into other files.

### Commands to insert or manipulate text

These *normal mode* commands take vim to *insert mode*.
To return to *normal mode*, type either `<Esc>` or `<C-[>`.

| Command | Description                                                     |
|:-------:|:--------------------------------------------------------------- |
| `i`     | insert text before character cursor is on                       |
| `I`     | insert text at beginning of line after initial white space      |
| `0i`    | insert text beginning of line                                   |
| `a`     | insert text after character cursor is on                        |
| `A`     | insert text at end of line                                      |
| `o`     | open new line after current line in insert text                 |
| `O`     | open new line before current line in insert text                |
| `s`     | delete current character and enter *insert mode*                |
| `S`     | delete line contents and enter *insert mode*                    |
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
| `"a3S`  | delete 3 lines into `"a`, enter *normal mode* on new line       |
| `"b3C`  | delete rest of line & next 2 two into `"b`, enter *normal mode* |

### Repeating commands in Normal Mode

| Command | Description                                |
|:-------:|:------------------------------------------ |
| `.`     | repeat the last command which changed text |

This repeats the last *normal mode* command used which changed text.
It does not repeat *command mode* commands.

This is frequently used with the `n` or `;` *normal mode* commands.
For example, `n.n.nn.n` keeps moving to the beginning of the next match
for the last search pattern where you can either decide to repeat, or
not, the change at each location.

---

## Insert Mode

The whole vi paradigm is that you do all navigation in *normal mode*
and type text in *insert mode*.  You return to *normal mode*
by pressing the `<Esc>` key.

### Pasting from registers into insert mode

To paste text from a Vim register while in *insert mode*,
use `<C-R>`.

| Command       | Description                                      |
|:-------------:|:------------------------------------------------ |
| `<C-R>"`      | paste from default register into buffer          |
| `<C-R>a`      | paste from register "a into buffer (as if typed) |
| `<C-R><C-R>a` | paste from register "a into buffer (literally)   |
| `<C-R>*`      | paste from X11 clipboard                         |
| `<C-R>+`      | paste from desktop clipboard                     |
| `<C-R>%`      | paste current filename                           |
| `<C-R>#`      | paste alternate filename                         |
| `<C-R>=`      | prompted to enter expression and paste result    |

### Navigating in *insert mode*

Sometimes it is convenient to be able to navigate while
in *insert mode*.  I tend to do this only to navigate near
where the cursor is.

Most "out of the box" vim configurations allow you to navigate
with the arrow keys while in *insert mode*.  Usually text can
also be deleted with the backspace key.  In *normal mode*, the
backspace and space keys are just extra navigation keys.

It is also possible to perform a single *normal mode* action within
*insert mode* by using`<C-O>` key sequences.

| Command   | Description                              |
|:---------:|:---------------------------------------- |
| `<C-O>h`  | move cursor left one character           |
| `<C-O>l`  | move cursor right one character          |
| `<C-O>k`  | move cursor up one line                  |
| `<C-O>j`  | move cursor down one line                |
| `<C-O>3w` | move cursor three words left             |
| `<C-O>2j` | move down two lines                      |
| `<C-O>J`  | join current line with the next line     |
| `<C-O>D`  | delete everything to the right of cursor |

### Other insert mode commands

| Command       | Description                                                 |
|:-------------:|:----------------------------------------------------------- |
| `<C-W>`       | delete word to left of cursor                               |
| `<C-U>`       | delete everything to left of cursor                         |
| `<C-H>`       | delete character to left of cursor                          |
| `<BS>`        | delete character to left of cursor                          |
| `<C-V><char>` | insert literal character                                    |
| `<C-T>`       | indent current line one tab stop                            |
| `<C-D>`       | un-indent current line one tab stop                         |
| `<C-A>`       | repeat last text insertion                                  |
| `<C-E>`       | enter character below cursor (from line below current line) |
| `<C-Y>`       | enter character above cursor (from line above current line) |
| `<Esc>`       | Quit *insert mode* go back to *normal mode*                 |
| `<C-C>`       | Quit *insert mode*, InsertLeave autocmd event not triggered |

The first five commands come from the original vi.  A subtle difference is
that in vi these commands edited not the buffer, but the current edit of the
buffer.  This explains a `<C-U>` idiosyncratic bahavior.  Vim/Neovim will
first delete up to what was just typed, just like vi would have done, before
deleting to the beginning of the line.

### Ins-completion sub-mode commands

This "sub-mode" is used for text completions.  While in *ins-completion mode*,
`<C-Y>` will accept the completion and `<C-E>` will return what was originally
typed.  `<C-N>` will move to the next completion in the drop down, and `<C-P>`
will move to the previous one.

| Command      | Description                                               |
|:------------:|:--------------------------------------------------------- |
| `<C-P>`      | complete keyword backwards from various sources           |
| `<C-N>`      | complete keyword forward from various sources             |
| `<C-X><C-L>` | search for line forwards in buffer                        |
| `<C-X><C-I>` | search for keyword forwards in file and included files    |
| `<C-X><C-D>` | search for definition forwards in file and included files |
| `<C-X><C-]>` | search for tag and insert before cursor                   |
| `<C-X><C-K>` | search words in dictionary                                |
| `<C-X><C-T>` | search words in thesaurus                                 |
| `<C-X>s`     | search for spelling suggestions                           |

What "various sources" for the first two above is configured via
the complete flag:

```
    :set complete
    complete=.,w,b,u,t
```

---

## *Command Mode*

Vim is an open source version of the Unix editor vi,
which is the visual interface of the Berkeley Unix
line editor ex, which itself is a re-implementation of
the AT&T Unix line editor ed.  On really old terminals,
essentially line printers with keyboards, the descendants
of teletypes, you edited files one line at a time.

*command mode* commands developed from the original ex
line editing commands.

Use the `:` *normal_mode* command to enter *command mode*.  The
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
| `:s/foo/bar/i`      | case insensitive version of above                    |
| `:s/foo/bar/g`      | substitute all instances of foo with bar             |
| `:s/foo/bar/gc`     | same as above but ask for confirmation each time     |
| `:17,42s/foo/bar/g` | substitute all foo with bar, lines 17 to 42          |

### Navigating the command mode line

While in *command mode*, up & down arrow keys cycle through previous
*command mode* commands.  The left & right arrow keys help you
re-edit the line.  Press `<Esc>`or`<C-[>` to return to *normal mode* without
issuing a command.

Using the up & down arrow keys with something typed will
cycle through only those commands which begin with the
typed text.

### Pasting from registers into the command mode line

use `<C-R>` while in *command mode* to paste text
from a Vim register to the command line.

| Command   | Description                                 |
|:---------:|:------------------------------------------- |
| `<C-R>"`  | paste from default register to command line |
| `<C-R>a`  | paste from register "a to command line      |
| `<C-R>*`  | paste from X11 clipboard                    |
| `<C-R>+`  | paste from desktop clipboard                |

---

## Visual Mode

This mode allows you to select region of text by visually highlighting,
and then modify as a unit.

To enter *visual mode* from *normal mode*

| Command | Description                |
|:-------:|:-------------------------- |
| `v`     | for character based        |
| `V`     | for line based             |
| `<C-V>` | for block visual mode      |
| `gv`    | to reselect last selection |

Highlight text with *normal mode* cursor navigation commands
like `h`, `j`, `k`, `l`, `w`, `e`, `W`, `B`, `f` or the arrow keys.
Once selected, you can issue either *normal mode* or
*command mode* commands.

*normal mode* commands such as `d`, `y`, `c`, `I`, `A`, `>>`, `<<`, `/`
act on the highlighted region.  The behavior of some
commands, like indenting commands `>>` or `<<`, vary
depending on which *visual mode* (character, line or block)
you are in.

*command mode* commands act on lines in their entirety
that contain the selected region.

To punt out of *visual mode* without doing anything,
press the `<Esc>` key.

If you have enabled mouse support, mouse actions can cause you
to enter *visual mode*.  When I first the transition from vi to vim,
I found it useful to enable mouse support for *normal mode* only.
After becoming more comfortble with *visual mode*, I found it
completely natural enabling mouse support for all modes.

---

## Other Useful Vim Information

### Undo/redo normal mode commands

| Command | Description        |
|:-------:|:------------------ |
| `u`     | undo previous edit |
| `<C-R>` | redo edit undone   |

These can be used to linearly undo and redo edits,
like the arrow buttons in a web browser.
Navigating with the arrow keys while in *insert mode*
will result in multiple undo/redo events.

### Some Vim/Neovim command line option examples

```
    $ nvim file1 file2 file3  # Open/create 3 files for editing
    $ nvim +<n> file     # Open file for editing on line n,
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
* Use `<C-O>` to jump back to previous location
* Use `<C-I>` or `<Tab>` to jump forward again
* familiarize yourself with how to use [multiple vim windows](MultipleWindows.md)
* configure the [mouse](VimFactoids.md#using-the-mouse)
* setting up the [wildmenu](VimFactoids.md#configuring-wildmenu)

---

| [Absolute Minimal Text Editing][1] | [Home][2] | next: [Vim Factoids][3] |

[1]: AbsoluteMinimalTextEditing.md
[2]: README.md
[3]: VimFactoids.md
