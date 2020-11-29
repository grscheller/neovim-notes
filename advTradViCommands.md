# Advanced Traditional Vi Commands

Many of the following commands existed in the original vi.
A good vi cheatsheet for traditional vi can be found here:
[Lagmonster](http://www.lagmonster.org/docs/vi2.html).

(TL;DR) The original vi was often called "bimodal" where
*Normal Mode* and *Command Mode* where conflated together
and called "*Command Mode*."  *Insert Mode* was the second
mode.  It was the Berkley Unix nvi (for new vi) which first
introduced multiple windows.

Where the behavior differs from the original Vi, I will indicate
the Vim behavior.

## *Normal Mode* Commands

### Misc commands

| Command    | Description                                |
|:----------:|:------------------------------------------ |
| `<ctrl-g>` | show filename and other useful status info |
| `<ctrl-l>` | redraw view                                |
| `ZZ`       | save changes and exit vim                  |
| `<ctrl-z>` | suspend vim to shell background            |

For `<ctrl-z>`, in bash usage `fg %1` will usually work to
unsuspend vim.  If you have other things suspended, hunt for it
using `$ jobs`.

### Commands to move cursor in *normal mode*

| Command    | Description                                        |
|:----------:|:-------------------------------------------------- |
| `+`        | move to first nonspace character next line         |
| `-`        | move to first nonspace character prev line         |
| `nG`       | move to nth line in file                           |
| `G`        | move to last line in file                          |
| `ngg`      | move to nth line in file                           |
| `gg`       | move to first line in file                         |
| `n\|`      | move to nth column in line                         |
| `\|`       | move to beginning of line                          |
| `0`        | move to beginning of line                          |
| `H`        | move to top of screen                              |
| `M`        | move to middle of screen                           |
| `L`        | move to bottom of screen                           |
| `nH`       | move to nth line from top of screen                |
| `nL`       | move to nth line from bottom of screen             |
| `<ctrl-u>` | move cursor/view up half a screen                  |
| `<ctrl-d>` | move cursor/view down half a screen                |
| `<ctrl-b>` | move cursor/view up a full screen                  |
| `<ctrl-f>` | move cursor/view down a full screen                |
| `%`        | move between matching `( )`, `[ ]`, `{ }` or `< >` |

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

| Command    | Description                            |
|:----------:|:-------------------------------------- |
| `<ctrl-e>` | move view down one line                |
| `<ctrl-y>` | move view up one line                  |
| `zt`       | make current line top line of view     |
| `zz`       | make current line middle line of view  |
| `zb`       | make current line bottom line of view  |
| `<nn>zt`   | make line `<nn>` top line of view      |
| `<nn>zz`   | make line `<nn>` middle line of view   |
| `<nn>zb`   | make line `<nn>` bottom line of view   |

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
can be separated by formfeeds (U+000c). `[[` and `]]`
jump you to the previous and next one respectively.

(TL;DR) For pre-ANSI K&R C files, The last two will jump
between `{` which are in the first column.  Programmers
used these to jump between C functions in source code.
For troff files various constructs were understood as
defining "sections."

### Commands to change text

| Command    | Description                                               |
|:----------:|:--------------------------------------------------------- |
| `C`        | change from cursor to end of line (enter *Insert Mode*)   |
| `R`        | from cursor, overwriting text (enter *Replace Mode*)      |
| `S`        | change entire line (enter *Insert Mode*)                  |
| `I`        | insert text at beginning of line after initial whitespace |
| `i`        | enter *Insert Mode*                                       |
| `a`        | advance cursor one char and enter *Insert Mode*           |
| `A`        | advance cursor to end of line and enter *Insert Mode*     |
| `x`        | delete char at cursor, stay in *Normal Mode*              |
| `X`        | delete char before cursor, stay in *Normal Mode*          |
| `>>`       | move entire line 1 tabstop right, stay in *Normal Mode*   |
| `<<`       | move entire line 1 tabstop left, stay in *Normal Mode*    |

## *Insert Mode* Commands

| Command          | Description                                       |
|:----------------:|:------------------------------------------------- |
| `<ctrl-h>`       | delete previous character                         |
| `<backspace>`    | delete previous character                         |
| `<ctrl-v><chr>`  | take `<chr>` literally                            |
| `<ctrl-w>`       | delete previous word                              |
| `<ctrl-o>`       | go to normal mode for just one command            |
| `<ctrl-o> n`     | go to next search item, remain in insert mode     |
| `<ctrl-o> D`     | delete everything to right of cursor              |
| `<ctrl-u>`       | delete everything to left of cursor               |
| `<ctrl-t>`       | indent current line one tab stop                  |
| `<ctrl-d>`       | un-indent current line one tab stop               |
| `<ctrl-c>`       | break out of *Insert Mode*, punt on any auto cmds |
| `<ctrl-x>`       | enter *Insert Mode* completion submode            |

For more information on `<ctrl-x>` see,

```
   :help ins-completion
```

If you accidentally typed `<ctrl-x>` while in insert mode, typing any
non-control character will get you back.  If you have terminal flow
control turned on, and you hit the unfortunate key combination
`<ctrl-x><ctrl-s>`, something EMACS users are likely to do, you will
find your vim editting session frozen.  Type `<ctrl-q>` to unlock.

### *Insert Mode* vs *Replace Mode*

* *Replace Mode* is similar to *Insert Mode* but
  characters are overwritten instead of inserted.
* You can toggle between them via the terminal
  `<insert>`key.
* You can enter *Replace Mode* directly from *Normal Mode*
  via the `R` command.
* Like in *Insert Mode* you can naviagte around the text
  via the arrow keys creating multiple undo events.
* In *Replace Mode*, the `<backspace>` and `<ctrl-h>` act
  like a back arrow key but undoes (only) last set of replacements.

## *Command Mode* Commands

| Command        | Description                                          |
|:-------------- |:---------------------------------------------------- |
| `:r file`      | read file and insert it after current line           |
| `:nr file`     | read file and insert it after line n                 |
| `:w!`          | write file overriding normal checks                  |
| `:n,mw file`   | save lines n thru m to file                          |
| `:n,mw >>file` | append lines n thru m to existing file               |
| `:'a,'bw file` | save lines from line with mark a to line with mark b |
| `:e!`          | reedit file discarding any unsaved changes           |
| `:#`           | show current line number and print line              |
| `:.=`          | show current line number                             |
| `:=`           | show number of lines in buffer                       |
| `:n,md`        | delete lines n thru m

## Marks

Marks allow you to set locations to either be able to jump to
or use with *Normal Mode* editing commands.

Marks within a given buffer are denoted via leters `a-z`.  For marks between
different buffers, use letters `A-Z`.  The mark is a "zero-width" entity
between the cursor and the preceding character.

| Command   | Description                                                  |
|:---------:|:------------------------------------------------------------ |
| `ma`      | set mark `a` for the current editing buffer                  |
| `mB`      | set mark `B` for all buffers                                 |
| `` `a ``  | jump to mark `a` current buffer                              |
| `` `B ``  | jump to mark `B` current or another editing buffer           |
| `'a`      | jump to first nonspace char in line with mark `a`            |
| `` d`a `` | delete from cursor to mark `a`                               |
| `` y`a `` | yank from cursor to mark `a`                                 |
| `` y`B `` | yank from cursor to mark `B`, fails if not in current buffer |
| `d'w`     | deletes current line thru line with mark `w`                 |

Like a mark, the cursor is also a "zero-width" entity between the
highlighted character and the preceeding character.  If the mark is
before the cursor in the file, the selection does not contain the
highlighted character.  Just like the behavior of the `yb` *Normal Mode*
command.

---

| prev: [Vim Factoids][1] | [Home][2] | next: [Vim SpecificFeatures][3] |

[1]: vimFactoids.md
[2]: README.md
[3]: vimSpecificFeatures.md
