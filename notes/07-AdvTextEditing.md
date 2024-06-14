# Advanced Text Editing

After a few months of practice with the basic text editing section,
here are some more commands to wrap your head around.

## Overview

### Additional nvim commands

* [Normal Mode](#normal-mode)
* [Insert Mode](#insert-mode)
* [Command Mode](#command-mode)

### Auto & manually formatting text

* [Control formatting with `formatoptions`](#formatting-text)

### Other useful Vim Information

* [Command line option examples](#command-line-option-examples)
* [Dealing with whitespace characters](#dealing-with-whitespace-characters)
* [Replace tabs with spaces as you type](#replace-tabs-with-spaces-as-you-type)

---

## Normal Mode

### Cursor movement in Normal Mode

These additional commands will make horizontal motion faster.

| Command       | Description                                                                        |
|:-------------:|:---------------------------------------------------------------------------------- |
| `ge, gE`      | move backward to end of previous word or WORD                                      |
| `f<char>`     | move forward to next `<char>` on current line                                      |
| `F<char>`     | move backward to next `<char>` on current line                                     |
| `t<char>`     | move forward before next `<char>` on current line                                  |
| `T<char>`     | move backward after next `<char>` on current line                                  |
| `;`           | next target for last `f`, `F` ,`t` ,`T` command                                    |
| `,`           | prev target for last `f`, `F`, `t`, `T` command                                    |
| `*`           | jump to next instance of word cursor on or before, can take count                  |
| `#`           | jump to prev instance of word cursor on or before, can take count                  |
| `_`           | like `^` but can take count, jumps to first non-blank char [count - 1 ] lines down |

### Changing text and/or interacting with the default register

| Command      | Description                                             |
|:------------:|:------------------------------------------------------- |
| `x`          | delete character under cursor, put in default register  |
| `X`          | delete character before cursor, put in default register |
| `s`          | delete current character and enter *insert mode*        |
| `S`          | delete line contents and enter *insert mode*            |
| `~`          | change case of current char and advance one char        |
| `g~<motion>` | change case via "vim motion" or "vim text object"       |
| `J`          | join current & next line, insert spaces as needed       |
| `gJ`         | join current & next line without inserting spaces       |

### Normal mode commands using motions and text objects

`y`, `d`, `c` can be used with all the *normal mode* motions

| Command | Description                                                 |
|:-------:|:----------------------------------------------------------- |
| `d$`    | delete to end of line and put in default register           |
| `d0`    | delete everything before cursor and put in default register |
| `y^`    | yank everything before cursor to first non-whitespace char  |
| `d2fz`  | delete from cursor to 2nd z on current line                 |
| `2db`   | delete 2 previous words starting from cursor                |
| `5x`    | delete next 5 characters on current line                    |
| `5X`    | delete PREVIOUS 5 characters on current line                |
| `d{`    | delete to beginning of paragraph whitespace                 |
| `d}`    | delete to end of paragraph                                  |
| `c3h`   | delete previous 3 characters and enter *insert mode*        |
| `c2k`   | delete current & prev line and enter *insert mode*          |
| `S`     | delete line contents and enter *insert mode*                |
| `c$`    | change to end of line                                       |
| `c^`    | change text before cursor, excluding initial white space    |
| `c0`    | change text before cursor to beginning of line              |

They can also be used with *text objects*.

Text Objects (TO) are similar to motions and they too also take
a count. Think "inner" for `i` and "around" for `a`.

| TO   | Description                                                |
|:----:|:---------------------------------------------------------- |
| `iw` | inner word - select words and the spaces between them      |
| `aw` | around word - select words, spaces between & trailing ws   |
| `iW` | inner Word - select Words and the spaces between them      |
| `aW` | around word - select Words, spaces between & trailing ws   |
| `is` | inner sentence - select sentences                          |
| `as` | around sentence - select sentences                         |
| `i(` | select what is between parentheses                         |
| `a(` | select what is between parentheses and the parentheses too |
| `i{` | select what is between brackets                            |
| `a{` | select what is between brackets and the brackets too       |
| `i<` | select what is between <>                                  |
| `a<` | same as above and the `<` `>` too                          |
| `it` | select what is between tags `<aaa>` and `</aaa>`           |
| `at` | same as above but include the tags too                     |

| Command   | Description                                       |
|:---------:|:------------------------------------------------- |
| `` yi` `` | yank what is between the tick marks               |
| `` ci` `` | change what is between the tick marks             |
| `` ca` `` | same as above but include tick marks too          |
| `dip`     | delete current paragraph                          |
| `dap`     | delete rest of paragraph below current line       |
| `diw`     | delete word cursor is on, leave whitespace        |
| `daw`     | delete word cursor is on, eat trailing whitespace |
| `ciw`     | change word cursor is in                          |
| `caw`     | same as `ciw` but include trailing whitespace     |
| `cis`     | change inner sentence (works best for prose)      |
| `cip`     | change paragraph cursor is in                     |
| `cap`     | change paragraph including trailing whitespace    |

Useful *normal mode* commands to jump to various locations

| Command    | Description                                             |
|:----------:|:------------------------------------------------------- |
| `` `[ ``   | to first character of previously changed or yanked text |
| `` `] ``   | to last character of previously changed or yanked text  |
| `` `< ``   | to first character of last visually selected text       |
| `` `> ``   | to last character of last visually selected text        |
| `` `( ``   | jump to beginning of sentence                           |
| `` `) ``   | jump to end of sentence                                 |
| `` `{ ``   | jump to beginning of paragraph                          |
| `` `} ``   | jump to end of paragraph                                |

Convenient *normal mode* commands taking nvim into *insert mode*.

| Command | Description                                               |
|:-------:|:--------------------------------------------------------- |
| `i`     | enter insert mode at cursor position                      |
| `gi`    | jump to end of previous insertion and enter *insert mode* |
| `cl`    | delete character and enter *insert mode*                  |

To paste lines at the current line's indentation

| Command       | Description                         |
|:-------------:|:----------------------------------- |
| `<]-p>"`      | paste at current line's indentation |

---

## Insert Mode

Some more things that can be done in *insert mode*.

### Pasting from registers into insert mode

To paste text from a Vim register while in *insert mode*, use `<C-r>`.

| Command       | Description                                   |
|:-------------:|:--------------------------------------------- |
| `<C-r>"`      | paste last deleted, yanked, or pasted content |
| `<C-r>a`      | paste from register "a                        |
| `<C-r>*`      | paste from X11 clipboard                      |
| `<C-r>+`      | paste from desktop clipboard                  |
| `<C-r>%`      | paste current filename                        |
| `<C-r>#`      | paste alternate filename                      |
| `<C-r>/`      | paste last search pattern                     |
| `<C-r>:`      | paste last *command mode* command             |

### Navigating in *insert mode*

Sometimes it is convenient to navigate while in *insert mode*. I tend
to do this only to navigate near where the cursor is.

Most "out of the box" vim configurations allow you to navigate with the
arrow keys while in *insert mode*. Usually text can also be deleted
with the backspace key. In *normal mode*, the backspace and space keys
are just extra navigation keys. This behavior can be adjusted via
options.

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

These tend to be more useful when used in keybindings.

### Other insert mode commands

| Command           | Description                                            |
|:-----------------:|:------------------------------------------------------ |
| `<C-v><char>`     | insert literal character                               |
| `<C-h>` or `<BS>` | delete character to left of cursor                     |
| `<C-w>`           | delete word to left of cursor                          |
| `<C-u>`           | delete all entered characters on a line ...            |
| `<C-u><C-u>`      |   and all characters up to initial whitespace ...      |
| `<C-u><C-u><C-u>` |     and all characters up to beginning of line         |
| `<C-t>`           | increase line indentation one tab width                |
| `<C-d>`           | decrease line indentation one tab width                |
| `<C-a>`           | insert text from last insert mode                      |
| `<C-y>`           | insert the character on line above cursor              |
| `<C-e>`           | insert the character on line below cursor              |
| `<Esc>`           | Quit *insert mode* go back to *normal mode*            |
| `<C-c>`           | Quit *insert mode*, InsertLeave autocmds not triggered |

The first four commands come from the original vi. A subtle difference
is that in vi these commands edited not the buffer, but the current edit
of the buffer. This may explain the above idiosyncratic behavior of
multiple `<C-u>`.

---

## Command Mode

Some more advanced things that can be done while in *insert mode*. I
find using these in keymaps and plugins much more useful than using
them directly.


| Command             | Description                                          |
|:------------------- |:---------------------------------------------------- |
| `:5,+5d`            | delete from line 5 thru 5 lines beyond current line  |
| `:5;+5d`            | go to line 5, delete lines 5 thru 10                 |
| `:10;+3y`           | go to line 10, yank it and next 3 lines              |
| `:,/^typed/y`       | yank from current line to line starting with "typed" |
| `:m+3`              | move current line down 3 lines                       |
| `:m-4`              | move current line up 3 = 4 - 1 lines                 |
| `:5,42p`            | print lines 5 thru 42 at bottom in command mode area |
| `:42,$p`            | print lines 42 thru last line in buffer              |
| `:,100p`            | print current line thru line 100                     |
| `:50,p`             | print line 50 thru current line cursor is on         |

Execute these *command mode* command via `<CR>`, which returns you to
*normal mode*.

The above `:m-4` is another example of vim/nvim exactly duplicating a vi
idiosyncrasy. Avoid using *command mode* commands with negative numbers
in them.

Unlike Vim, Neovim does not have an *EX mode*.

### Editing the `command mode* command line

One cannot edit the command line using *normal mode* commands! Typing
`<Esc>` just sends you back to *normal mode* in the editing buffer.

| Command       | Description                                     |
|:-------------:|:----------------------------------------------- |
| `<C-w>`       | delete word to left of cursor                   |
| `<C-u>`       | delete everything to left of cursor             |
| `<C-h>`       | delete character to left of cursor              |
| `<BS>`        | delete character to left of cursor              |
| `<C-r>"`      | paste from default register to command line     |
| `<C-r>a`      | paste from register "a to command line          |
| `<C-r>*`      | paste from X11 clipboard                        |
| `<C-r>+`      | paste from desktop clipboard                    |
| `<Esc>`       | Quit *command mode*, go back to *normal mode*   |
| `<C-c>`       | Quit *command mode*, don't perform any autocmds |

---

## Formatting text

A fairly old Vim feature is to auto-format via the `gw` command. The
behavior of this auto formatting is controlled via the `formatoptions`
options. The following table shows the values I usually set for this.

| Format          | Cmd to set             | Use case                 |
|:--------------- |:---------------------- |:------------------------ |
| text & comments | `:set fo=tcqjo1`       | Programming/config files |
| Just text       | `:setlocal fo=tqjp1n`  | Writing                  |
| Just text       | `:setlocal fo=tqjp1nl` | Markdown                 |

See `:h fo-table` for what letters in `formatoptions` mean.

The formatting behavior depends heavily on whatever `textwidth` is
set. For markdown, the `l` options makes dealing with long lines, like
links, a bit easier. The `n` option indents when wrapping numbered
lines.

Formatting can be manually triggered via `gq` or `gw`:

* `gq{motion}` - format, move cursor to end of formatted text
* `gw{motion}` - format, keep cursor at current position
* `gw{text-object}` - inner paragraph (ip) text-object works well
* `gww` - format current line (does not take a count)
* `gq` & `gw` - both work from visual mode

One difference between `gq` & `gw` is that the latter does not use
either the `formatprg` or `formatexpr` options.

* `formatprg` names an external program to do the formatting
* `formatexpr` is an expression to evaluate over the formatting range

---

## Other Useful Vim Information

### Jumping back to previously edited buffer

Here is a nice way to jump between 2 buffers in *normal mode*.

| Command | Description                                    |
|:-------:|:---------------------------------------------- |
| `<C-6>` | jump back to the last previously edited buffer |

### Command line option examples

```fish
    $ nvim file1 file2 file3  # Open/create 3 files for editing
    $ nvim +<n> file     # Open file for editing on line n,
                         # defaults to last line of file
    $ nvim +/pattern file  # Open file for editing at first reg-exp pattern match
    $ nvim -R file         # Open file read only, can still write via :w!
    $ nvim -r       # List swap files, then exit
    $ nvim -r file  # Recover crashed vim session, uses swap file
    $ nvim -h       # List help message for command-line options and exit
    $ nvim -c :checkhealth   # Check health of Neovim installation
```

### Dealing with whitespace characters

It is helpful if you can "see" what the whitespace is.

| Command       | Description                            |
|:------------- |:-------------------------------------- |
| `:set list`   | Indicate line endings & tabs           |
| `:set nolist` | Display line endings & tabs normally   |
| `:%s/ \+$//`  | Strip off trailing spaces on all lines |

When list is set, by default tabs are shown as ">", trailing spaces
as "-", and non-breakable space characters as "+" by default.  Further
changed by the 'listchars' option.

### Replace tabs with spaces as you type

Set the following options

```vim
   set tabstop=4
   set shiftwidth=4
   set softtabstop=4
   set expandtab
```

Assuming the above 4 settings, to remove all existing tabs in the buffer
and replace with 4 spaces,

| Command  | Description                    |
|:-------- |:------------------------------ |
| `:retab` | Redo all the tabbing in buffer |

To insert an actual tab, enter *insert mode* and type `<C-v><Tab>`.

---

| prev: [Vim Specific Features][6] | [Home][0] | next: [Mult Neovim Windows][8] |

[6]: 06-VimSpecificFeatures.md
[0]: ../README.md
[8]: 08-MultipleWindows.md
