# Advanced Traditional Vi Commands

The problem a lot of people have with Vim is
[that they don't grok vi][4].

Much of the information here I obtained from
[Lagmonster](http://www.lagmonster.org/docs/vi2.html).
This site no longer seems to be active.  These commands
existed in the original ex version of vi.  Where the behavior
differs from the original vi, I will indicate the nvim behavior.

The name vi comes from the "visual interface" for the ex
line editor.

Vi was often called "bimodal" where *normal mode* and
*command mode* were conflated together and called
"command mode" and "insert mode" was the second mode.

## *Normal Mode* Commands

### Misc commands

| Command | Description                                |
|:-------:|:------------------------------------------ |
| `<C-g>` | show filename and other useful status info |
| `<C-l>` | redraw view                                |
| `ZZ`    | save changes and exit vim                  |
| `<C-z>` | suspend vim to shell background            |

For `<C-z>`, the shell command `fg %1` will usually work to
un-suspend vim.  If you have other things suspended, hunt for it
via the `jobs` shell command.

### Commands to move cursor in *Normal Mode*

| Command   | Description                                        |
|:---------:|:-------------------------------------------------- |
| `+`       | move to first non-space character next line        |
| `-`       | move to first non-space character prev line        |
| `nG`      | move to nth line in file                           |
| `G`       | move to last line in file                          |
| `ngg`     | move to nth line in file                           |
| `gg`      | move to first line in file                         |
| `n\<Bar>` | move to nth column in line                         |
| `<Bar>`   | move to beginning of line                          |
| `0`       | move to beginning of line                          |
| `H`       | move to top of screen                              |
| `M`       | move to middle of screen                           |
| `L`       | move to bottom of screen                           |
| `nH`      | move to nth line from top of screen                |
| `nL`      | move to nth line from bottom of screen             |
| `<C-u>`   | move cursor/view up half a screen                  |
| `<C-d>`   | move cursor/view down half a screen                |
| `<C-b>`   | move cursor/view up a full screen                  |
| `<C-f>`   | move cursor/view down a full screen                |
| `%`       | move between matching `( )`, `[ ]`, `{ }` or `< >` |

Where `<Bar>` = `|`

With `%`, if you are not currently on a grouping symbol, move
to the first one on the current line and jump to its matching
partner.  Plugs-ins like Syntastic can change the meaning of
what is a matching symbol for different file types.

When scrolloff is set, some of these commands get modified.
In my .vim/vimrc file, I use

```
   set scrolloff=3
```

to keep the cursor 3 lines from the edge of the screen.

### Commands to move screen view

| Command | Description                            |
|:-------:|:-------------------------------------- |
| `<C-e>` | move view down one line                |
| `<C-y>` | move view up one line                  |
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
| `I`        | insert text at beginning of line after initial whitespace |
| `i`        | enter *insert mode*                                       |
| `a`        | advance cursor one char and enter *insert mode*           |
| `A`        | advance cursor to end of line and enter *insert mode*     |
| `x`        | delete char at cursor, stay in *normal mode*              |
| `X`        | delete char before cursor, stay in *normal mode*          |
| `>>`       | move entire line 1 tab stop right, stay in *normal mode*  |
| `<<`       | move entire line 1 tab stop left, stay in *normal mode*   |

## *Insert Mode* Commands

| Command       | Description                                         |
|:-------------:|:--------------------------------------------------- |
| `<C-h>`       | delete previous character                           |
| `<BS>`        | delete previous character                           |
| `<C-v>{char}` | insert character `{char}` literally                 |
| `<C-v><Tab>`  | insert literal `<Tab>` (handy for makefiles)        |
| `<C-w>`       | delete previous word                                |
| `<C-o>`       | go to normal mode for just one command              |
| `<C-o>n`      | go to next search item, remain in insert mode       |
| `<C-o>D`      | delete everything to right of cursor                |
| `<C-u>`       | delete everything to left of cursor                 |
| `<C-t>`       | indent current line one tab stop                    |
| `<C-d>`       | un-indent current line one tab stop                 |
| `<C-c>`       | break out of *insert mode*, punt on any auto cmds   |
| `<C-x>`       | enter *insert mode* completion submode (vim not vi) |

For more information on `<C-x>` see,

```
   :help ins-completion
```

If you accidentally typed `<C-x>` while in insert mode, typing any
non-control character will get you back.  If you have terminal flow
control turned on, and you hit the unfortunate key combination
`<C-x><C-s>`, something EMACS users are likely to do, you will
find your vim editing session frozen.  Type `<C-q>` to unlock.

### *Insert Mode* vs *Replace Mode*

* *replace mode* is similar to *insert mode* but
  characters are overwritten instead of inserted.
* You can toggle between them via the terminal
  `<Insert>` key.
* You can enter *replace mode* directly from *normal mode*
  via the `R` command.
* Like in *insert mode* you can navigate around the text
  via the arrow keys creating multiple undo events.
* In *replace mode*, the `<BS>` and `<C-h>` keys undo
  only current set of replacements, otherwise they
  act like the `<Left>` arrow key.

## *Command Mode* Commands

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

Marks within a given buffer are denoted via letters `a-z`.  For marks between
different buffers, use letters `A-Z`.  The mark is a "zero-width" entity
between the cursor and the preceding character.

| Command   | Description                                                  |
|:---------:|:------------------------------------------------------------ |
| `ma`      | set mark `a` for the current editing buffer                  |
| `mB`      | set mark `B` for all buffers                                 |
| `` `a ``  | jump to mark `a` current buffer                              |
| `` `B ``  | jump to mark `B` current or another editing buffer           |
| `'a`      | jump to first non-space char in line with mark `a`           |
| `` d`a `` | delete from cursor to mark `a`                               |
| `` y`a `` | yank from cursor to mark `a`                                 |
| `` y`B `` | yank from cursor to mark `B`, fails if not in current buffer |
| `d'w`     | deletes current line thru line with mark `w`                 |

Like a mark, the cursor is also a "zero-width" entity between the
highlighted character and the preceding character.  If the mark is
before the cursor in the file, the selection does not contain the
highlighted character.  Just like the behavior of the `yb` *normal mode*
command.

---

| prev: [Vim Factoids][1] | [Home][2] | next: [Vim SpecificFeatures][3] |

[1]: VimFactoids.md
[2]: README.md
[3]: NeovimSpecificFeatures.md
[4]: https://stackoverflow.com/questions/1218390/what-is-your-most-productive-shortcut-with-vim/1220118#1220118
