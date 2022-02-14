# Advanced Traditional Vi Commands

The problem a lot of people have with Vim is
[that they don't grok vi][4].

Much of the information here I obtained from
[Lagmonster][5].
This site no longer seems to be active.  These commands
existed in the original ex version of vi.  When the behavior
differs from the original vi, I will indicate the nvim behavior.

The name vi comes from the "visual interface" for the ex
line editor.

Vi was often called "bimodal" where *normal mode* and
*command mode* were conflated together and called
"command mode" and "insert mode" was the second mode.

## Normal Mode Commands

### Misc commands

| Command | Description                                |
|:-------:|:------------------------------------------ |
| `<C-G>` | show filename and other useful status info |
| `<C-L>` | redraw view                                |
| `ZZ`    | save changes and exit vim                  |
| `<C-Z>` | suspend vim to shell background            |

For `<C-Z>`, the shell command `fg %1` will usually work to
un-suspend vim.  If you have other things suspended, hunt for it
via the `jobs` shell command.

### Commands to move cursor in Normal Mode

| Command  | Description                                        |
|:--------:|:-------------------------------------------------- |
| `+`      | move to first non-space character next line        |
| `-`      | move to first non-space character prev line        |
| `nG`     | move to nth line in file                           |
| `G`      | move to last line in file                          |
| `ngg`    | move to nth line in file                           |
| `gg`     | move to first line in file                         |
| `0`      | move to beginning of line                          |
| `H`      | move to top of screen                              |
| `M`      | move to middle of screen                           |
| `L`      | move to bottom of screen                           |
| `nH`     | move to nth line from top of screen                |
| `nL`     | move to nth line from bottom of screen             |
| `<C-U>`  | move cursor/view up half a screen                  |
| `<C-D>`  | move cursor/view down half a screen                |
| `<C-B>`  | move cursor/view up a full screen                  |
| `<C-F>`  | move cursor/view down a full screen                |
| `%`      | move between matching `( )`, `[ ]`, `{ }` or `< >` |
| `n<Bar>` | move to nth column in line                         |
| `<Bar>`  | move to beginning of line                          |

Where `<Bar>` = `|`

With `%`, if you are not currently on a grouping symbol, move
to the first one on the current line and jump to its matching
partner.  Plugs-ins like Syntastic can change the meaning of
what is a matching symbol for different file types.

When scrolloff is set, some of these commands get modified.
In my init.vim file, I use

```
    set scrolloff=3
```

to keep the cursor 3 lines from the edge of the screen.

### Commands to move screen view

| Command | Description                            |
|:-------:|:-------------------------------------- |
| `<C-E>` | move view down one line                |
| `<C-Y>` | move view up one line                  |
| `zt`    | make current line top line of view     |
| `zz`    | make current line middle line of view  |
| `zb`    | make current line bottom line of view  |
| `[n]zt` | make line `n` top line of view      |
| `[n]zz` | make line `n` middle line of view   |
| `[n]zb` | make line `n` bottom line of view   |

Where applicable, you can type a number before these commands
to repeat them that many times.

### Cursor commands useful for written text

| Command | Description                                 |
|:-------:|:------------------------------------------- |
| `(`     | move cursor to beginning of sentence        |
| `)`     | move cursor to beginning of next sentence   |
| `{`     | move cursor up a paragraph                  |
| `}`     | move cursor down paragraph                  |
| `[[`    | move cursor to beginning previous section   |
| `]]`    | move cursor to beginning next section       |

What "section" means is most easily understood in the
context of file types.  For text files, different sections
can be separated by a formfeed (U+000c). `[[` and `]]`
jump you to the previous and next one respectively.

(TL;DR) For pre-ANSI K&R C files, The last two will jump
between `{` which are in the first column.  Programmers
used these to jump between C functions in source code.
For troff files various constructs were understood as
defining "sections."

### Commands to change text

| Command    | Description                                               |
|:----------:|:--------------------------------------------------------- |
| `C`        | change from cursor to end of line (enter *insert mode*)   |
| `R`        | from cursor, overwriting text (enter *replace mode*)      |
| `S`        | change entire line (enter *insert mode*)                  |
| `D`        | delete from cursor to end of line                         |
| `I`        | insert text at beginning of line after initial whitespace |
| `i`        | enter *insert mode*                                       |
| `a`        | advance cursor one char and enter *insert mode*           |
| `A`        | advance cursor to end of line and enter *insert mode*     |
| `x`        | delete char at cursor, stay in *normal mode*              |
| `X`        | delete char before cursor, stay in *normal mode*          |
| `>>`       | move entire line 1 tab stop right, stay in *normal mode*  |
| `<<`       | move entire line 1 tab stop left, stay in *normal mode*   |

## Insert Mode Commands

| Command       | Description                                         |
|:-------------:|:--------------------------------------------------- |
| `<C-H>`       | delete previous character                           |
| `<BS>`        | delete previous character                           |
| `<C-V>{char}` | insert character `{char}` literally                 |
| `<C-V><Tab>`  | insert literal `<Tab>` (handy for makefiles)        |
| `<C-W>`       | delete previous word                                |
| `<C-O>`       | go to normal mode for just one command              |
| `<C-O>n`      | go to next search item, remain in insert mode       |
| `<C-O>D`      | delete everything to right of cursor                |
| `<C-U>`       | delete everything to left of cursor                 |
| `<C-C>`       | break out of *insert mode*, punt on any auto cmds   |
| `<C-X>`       | enter *insert mode* completion submode (vim not vi) |

For more information on `<C-X>` see
[ins-completion section](BasicTextEditing.md#ins-completion-sub-mode-commands)
in BasicTextEditing, or

```
    :help ins-completion
```

If you accidentally typed `<C-X>` while in insert mode, typing any
non-control character, except `s` will get you back.  If you have
terminal flow control turned on, and you hit the unfortunate key
combination `<C-X><C-S>`, something EMACS users are likely to do, you
will find your vim editing session frozen.  Type `<C-Q>` to unlock.

### Insert Mode vs Replace Mode

* *replace mode* is similar to *insert mode* but
  characters are overwritten instead of inserted.
* You can toggle between them via the terminal
  `<Insert>` key.
* You can enter *replace mode* directly from *normal mode*
  via the `R` command.
* Like in *insert mode* you can navigate around the text
  via the arrow keys creating multiple undo events.
* In *replace mode*, the `<BS>` and `<C-H>` keys undo
  only current set of replacements, otherwise they
  act like the `<Left>` arrow key.

## Command Mode Commands

| Command        | Description                                          |
|:-------------- |:---------------------------------------------------- |
| `:r file`      | read file and insert it after current line           |
| `:nr file`     | read file and insert it after line `n`               |
| `:w!`          | write file overriding normal checks                  |
| `:n,mw file`   | save lines `n` thru `m` to file                      |
| `:n,mw >>file` | append lines `n` thru `m` to existing file           |
| `:'a,'bw file` | save lines from line with mark a to line with mark b |
| `:e!`          | reedit file discarding any unsaved changes           |
| `:#`           | show current line number and print line              |
| `:.=`          | show current line number                             |
| `:=`           | show number of lines in buffer                       |
| `:n,md`        | delete lines `n` thru `m`                            |

## Marks

Marks allow you to set locations to either be able to jump to
or use with *normal mode* editing commands.

Marks within the file being edited are denoted via letters `a-z`.
A mark is a "zero-width" entity between the cursor and the preceding character.

| Command   | Description                                                  |
|:---------:|:------------------------------------------------------------ |
| `ma`      | set mark `a` for the current editing buffer                  |
| `` `a ``  | jump to mark `a` current buffer                              |
| `'a`      | jump to first non-space char in line with mark `a`           |
| `` d`a `` | delete from cursor to mark `a`                               |
| `` y`a `` | yank from cursor to mark `a`                                 |
| `d'w`     | deletes current line thru line with mark `w`                 |

Like a mark, the cursor is also a "zero-width" entity between the
highlighted character and the preceding character.  If the mark is
before the cursor in the file, the selection does not contain the
highlighted character.  Just like the behavior of the `yb` *normal mode*
command.

---

| prev: [Vim Factoids][1] | [Home][2] | next: [Vim Specific Features][3] |

[1]: VimFactoids03.md
[2]: README.md
[3]: VimSpecificFeatures05.md
[4]: https://stackoverflow.com/questions/1218390/what-is-your-most-productive-shortcut-with-vim/1220118#1220118
[5]: http://www.lagmonster.org/docs/vi2.html
