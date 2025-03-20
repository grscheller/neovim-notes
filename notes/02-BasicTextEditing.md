# Basic Text Editing

This should be enough to enable you to be productive with nvim/vim as
a text editor. I think with a few months of practice, the material
covered here can be internalized and eventually become part of your
"muscle memory."

## Overview

### Nvim has 4 main modes

- [Normal Mode](#normal-mode)
- [Insert Mode](#insert-mode)
- [Command Mode](#command-mode)
- [Visual Mode](#visual-mode)

______________________________________________________________________

### Other useful Vim Information

- [Undo/Redo normal mode commands](#undo-and-redo-normal-mode-commands)
- [Spell checking](#spell-checking)
- [Replace tabs with spaces as you type](#replace-tabs-with-spaces-as-you-type)
- [Getting help](#getting-help)

______________________________________________________________________

## Normal Mode

I use *normal mode* as the default mode to enter when I pause to think.
I known people to use *insert mode* for this, but I first learned on the
original vi where *insert mode* was not as rich.

### Close Editor Window from Normal Mode

| Command | Description |
|:----:|:------------------------- |
| `ZZ` | exit window, save changes |
| `ZQ` | exit window, don't save changes, vim/nvim |

If you have unsaved changes, or have files you have not edited yet, you
will have to hit `ZZ` or `ZQ` again. In nvim, `ZZ` and `ZQ` will only
close the current window if multiple windows or tabs are open.

### Cursor movement in Normal Mode

| Command | Description |
|:---------:|:------------------------------------------- |
| `h,j,k,l` | move cursor one character (also arrow keys) |
| `w, W` | move forward to beginning next word |
| `b, B` | move backward to beginning word or WORD |
| `e, E` | move forward to end of word or WORD |
| `$` | move to end of line |
| `^` | move to first non-whitespace character on line |
| `0` | move to beginning of line |
| `G` | move to last line in file |
| `gg` | move to first line in file |
| `3w` | move forward 3 words on current line |
| `5l` | move forward 5 characters on current line |
| `/RegExp<CR>` | forward search for regular expression pattern |
| `?RegExp<CR>` | backward search for regular expression pattern |
| `/<CR>` | search forward for last pattern |
| `?<CR>` | search backward for last pattern |
| `n` | search forward or backward for last pattern |
| `N` | search for last pattern in reverse sense of above |

### Changing text and/or interacting with the default register

| Command | Description |
|:----:|:--------------------------------------------- |
| `dd` | delete line and put in default register (cut) |
| `3dd` | delete 3 lines and put in default register |
| `d$` | delete to end of line, put in default register |
| `D` | delete to end of line and put in default register |
| `Y` | yank to end of line and put in default register |
| `yy` | yank line to default register (copy) |
| `2y3w` | ends up yanking 6 words |
| `x` | delete character under cursor, put in default register |
| `~` | change case of current char and advance one char |
| `r<char>` | change current char to `<char>` |
| `J` | join current & next line, insert spaces as needed |
| `p` | paste default register contents "after" |
| `P` | paste default register contents "before" |

Where what "before" and "after" mean above depends on what the default
register contains.

### You can use named registers to store text

In vi these were referred to as named buffers.

| Command | Description |
|:------:|:------------------------------------ |
| `"adw` | delete word and put in register `"a` |
| `"B2yy` | yank 2 lines and append to register `"b` |
| `"sd$` | delete to end of line and put in register `"s` |
| `"sp` | paste contents of register `"s` after char cursor is on |
| `"aP` | paste contents of register `"a` before cursor |

One use case for named registers is copying multiple items
from multiple files and pasting them into other files.

### Commands to insert or manipulate text

These *normal mode* commands take vim to *insert mode*. To return to
*normal mode*, type either `<Esc>` or `<C-[>`.

| Command | Description |
|:---:|:----------------------------------------- |
| `i` | insert text before character cursor is on |
| `I` | insert text at beginning of line after initial white space |
| `a` | insert text after character cursor is on |
| `A` | insert text at end of line |
| `o` | open new line after current line in insert text |
| `O` | open new line before current line in insert text |
| `C` | change to end of line |
| `3cw` | change next three words starting at cursor |
| `c3w` | change next three words starting at cursor |
| `c3l` | change next three characters starting at cursor |
| `5cc` | change next 5 lines |
| `3cb` | change previous 3 words |
| `c$` | change to end of line |
| `c^` | change text before cursor, excluding initial white space |
| `c0` | change text before cursor to beginning of line |
| `"a3w` | delete 3 words into `"a` |
| `"b3C` | delete rest of line & next 2 two into `"b` |

### Repeating commands in Normal Mode

| Command | Description |
|:---:|:------------------------------------------ |
| `.` | repeat the last command which changed text |

This repeats the last *normal mode* command used which changed text. It
does not repeat *command mode* commands.

This is frequently used with the `n` or `;` *normal mode* commands. For
example, `n.n.nn.n` keeps moving to the beginning of the next match for
the last search pattern where you can either decide to repeat, or not,
the change at each location.

______________________________________________________________________

## Insert Mode

The whole vi paradigm is that you do all navigation in *normal mode* and
type text in *insert mode*. You return to *normal mode* by pressing the
`<Esc>` key.

### Useful insert mode commands

| Command | Description |
|:------:|:---------------------------------- |
| `<BS>` | delete character to left of cursor |
| `<C-h>`| delete character to left of cursor |
| `<C-w>` | delete word to left of cursor |
| `<C-u>` | delete all characters entered so far |
| `<C-t>` | increase line indentation one tab width |
| `<C-d>` | decrease line indentation one tab width |
| `<C-a>` | insert text from last insert mode |
| `<C-v><char>` | insert literal character |

The first four commands come from the original vi.

______________________________________________________________________

## Command Mode

Vim is an open source version of the Unix editor vi, which is the visual
interface of the Berkeley Unix line editor ex, which itself is
a re-implementation of the AT&T Unix line editor ed. On really old
terminals, essentially line printers with keyboards, the descendants of
teletypes, you edited files one line at a time.

*command mode* commands developed from the original ex line editing
commands.

Use the `:` in *normal mode* to enter *command mode*. The cursor jumps
down to the bottom of the terminal window and prompts you with `:`.

| Command | Description |
|:---- |:--------------------------------------- |
| `:w` | write buffer to associated file on disk |
| `:w file` | write to file, still editing original file |
| `:q` | quit editing, will warn if unsaved changes |
| `:wq` | write current buffer to disk, quit current window |
| `:wa` | write all buffers to disk |
| `:q!` | quit current window without saving changes |
| `:qa` | quit all windows |
| `:qa!` | quit all windows, even if there are unsaved changes |
| `:n` | edit next buffer typically next file on command line |
| `:next` | edit next buffer typically next file on command line |
| `:prev` | edit previous buffer |
| `:wn` | write buffer to disk and move on to next buffer |
| `:42` | move cursor to line 42 |
| `:+5` | move cursor 5 lines down |
| `:-3` | move cursor 3 lines up |
| `:#` | give line number of current line cursor is on |
| `:s/foo/bar/` | substitute first instance of foo with bar |
| `:s/foo/bar/i` | case insensitive version of above |
| `:1,$s/foo/bar/g` | substitute all instances of foo with bar in file |
| `:%s/foo/bar/gc` | same as above but ask for confirmation each time |
| `:17,42s/foo/bar/g` | substitute all foo with bar, lines 17 to 42 |
| `:g/baz/s/foo/bar/` | substitute first foo with bar on all lines with baz |
| `:p` | print current line cursor is on |

Execute these *command mode* command via `<CR>`, which returns you to
*normal mode*. To punt *command mode* and return to normal mode use
`<Esc>`.

### Navigating the command mode line

While in *command mode*, up & down arrow keys cycle through previous
*command mode* commands. The left & right arrow keys help you re-edit
the line. Press `<Esc>` to return to *normal mode* without
issuing a command.

Using the up & down arrow keys with something typed will cycle through
only those commands which begin with the typed text.

______________________________________________________________________

## Visual Mode

This mode allows you to select region of text by visually highlighting,
and then modify as a unit.

To enter *visual mode* from *normal mode*

| Command | Description |
|:---:|:------------------- |
| `v` | for character based |
| `V` | for line based |
| `<C-v>` | for block visual mode |
| `gv` | to re-select last visual mode selection |

Highlight text with *normal mode* cursor navigation commands like `h`,
`j`, `k`, `l`, `w`, `e`, `W`, `B`, `f` or the arrow keys. Once selected,
you can issue either *normal mode* or *command mode* commands.

*Normal mode* commands such as `d`, `y`, `c`, `I`, `A`, `>>`, `<<`, `/`
act on the highlighted region. The behavior of some commands, like I or
A, vary depending on which *visual mode* (character, line or block) you
are in. Others, like indenting commands `>>` or `<<`, just act on the
entire line.

*Command mode* commands act on lines in their entirety that contain the
selected region.

In *block visual mode* the `I` and `A` commands append text to the
beginning and end respectfully of each line in the selected region.
Short lines, lines that end before the selected region, are ignored.

A *block visual mode* the selected region can be extended to the end of
all lines, even if uneven, via the `$` command. In this case, the `I`
command works as before but the `A` command will append to short lines
too.

To punt out of *visual mode* without doing anything, press the `<Esc>`
key.

If you have enabled mouse support, mouse actions can cause you to enter
*visual mode*. When I first the transition from vi to vim, I found it
less confusing to enable mouse support for *normal mode* only. After
becoming more comfortable with *visual mode*, I found it completely
natural enabling mouse support for all modes.

______________________________________________________________________

## Other Useful Vim Information

### Undo and redo normal mode commands

| Command | Description |
|:---:|:------------------ |
| `u` | undo previous edit |
| `<C-r>` | redo edit undone |

These can be used to linearly undo and redo edits, like the arrow
buttons in a web browser. Navigating with the arrow keys while in
*insert mode* will result in multiple undo/redo events.

### Spell checking

| Command | Description |
|:------------ |:---------------------- |
| `:set spell` | Turn spell checking on |
| `:set nospell` | Turn spell checking off |
| `:set invspell` | Toggle spell setting |

### Replace tabs with spaces as you type

Put the following options in your `~/.config/nvim/init.vim` or
`~/.vim/vimrc` file

```vim
   set tabstop=4
   set shiftwidth=4
   set softtabstop=4
   set expandtab
```

To insert an actual tab, enter *insert mode* and type `<C-v><Tab>`.

### Getting help

To get started, from within vim, type

- `:help`
- `:help help`

Neovim built in help is very powerful, but not beginner friendly. To
get the most out of it,

- Use `<C-]>` or `double-click` mouse to follow vim "hyperlinks"
- Use `<C-o>` to jump back to previous location
- Use `<C-i>` or `<Tab>` to jump forward again
- familiarize yourself with how to use [multiple vim windows][7]
- configure the [mouse](03-VimFactoids.md#using-the-mouse)
- setting up the [wildmenu](03-VimFactoids.md#configuring-wildmenu)

______________________________________________________________________

| [Absolute Minimal Text Editing][1] | [Home][0] | next: [Vim Factoids][3] |

[0]: ../README.md
[1]: 01-AbsoluteMinimalTextEditing.md
[3]: 03-VimFactoids.md
[7]: 07-MultipleWindows.md
