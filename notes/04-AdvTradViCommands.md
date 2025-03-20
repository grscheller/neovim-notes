# Advanced Traditional Vi Commands

Much of the information here I obtained from [Lagmonster][50]. This
site no longer exists. These commands existed in the original ex version
of vi. When the behavior differs from the original vi, I will indicate
the nvim behavior.

The name vi comes from the "visual interface" for the ex line
editor. That is the reason vi's configuration file is called `~/.exrc`.

Vi was often called "bi-modal" where *normal mode* and *command mode*
were conflated together and called "*command mode*" and *insert mode*
was the second mode.

## Normal Mode Commands

### Misc commands

| Command | Description |
|:-------:|:------------------------------------------ |
| `<C-g>` | show filename and other useful status info |
| `<C-l>` | redraw view |
| `ZZ` | save changes and exit vim |
| `<C-z>` | suspend vim to shell background |

For `<C-z>`, the shell command `fg %1` will usually work to unsuspend
vim. If you have other things suspended, hunt for it via the `jobs`
shell command.

### Commands to move cursor/view in Normal Mode

| Command | Description |
|:---:|:------------------------------------------- |
| `+` | move to first non-space character next line |
| `-` | move to first non-space character prev line |
| `nG` | move to nth line in file |
| `G` | move to last line in file |
| `0` | move to beginning of line |
| `H` | move to top of screen |
| `M` | move to middle of screen |
| `L` | move to bottom of screen |
| `nH` | move to nth line from top of screen |
| `nL` | move to nth line from bottom of screen |
| `<C-u>` | move cursor/view up half a screen |
| `<C-d>` | move cursor/view down half a screen |
| `<C-b>` | move cursor/view up a full screen |
| `<C-f>` | move cursor/view down a full screen |
| `<C-e>` | move view down a one line, don't move cursor |
| `<C-y>` | move view up a one line, don't move cursor |
| `%` | move between matching `( )`, `[ ]`, `{ }` or `< >` |
| `n<Bar>` | move to nth column in line |
| `<Bar>` | move to beginning of line |

Where `<Bar>` = `|`

Both `<C-e>` & `<C-y>` will move cursor to keep it in the view.

With `%`, if you are not currently on a grouping symbol, move to the
first one on the current line and jump to its matching partner.
Neovim's treesitter can change the meaning of what is a matching
symbol for different file types.

If scrolloff is set in init.vim, some of these commands get modified,

```vim
    set scrolloff=3
```

will keep the cursor 3 lines from the edge of the screen.

### Cursor commands useful for written text

| Command | Description |
|:---:|:------------------------------------ |
| `(` | move cursor to beginning of sentence |
| `)` | move cursor to beginning of next sentence |
| `{` | move cursor up a paragraph |
| `}` | move cursor down paragraph |
| `[[` | move cursor to beginning previous section |
| `]]` | move cursor to beginning next section |

What "section" means is most easily understood in the context of file
types. For example, in pre-ANSI K&R C files, `[[` and `]]` will jump
between `{` which are in the first column. Programmers used these to
jump between C functions in source code. For troff files various
constructs were understood as defining "sections." Now-a-days, the
Marksman Markdown LSP server will cause `[[` and `]]` to jump between
headings.

### Commands to change text

| Command | Description |
|:---:|:------------------------------------------------------- |
| `C` | change from cursor to end of line (enter *insert mode*) |
| `R` | from cursor, overwriting text (enter *replace mode*) |
| `S` | change entire line (enter *insert mode*) |
| `D` | delete from cursor to end of line |
| `I` | insert text at beginning of line after initial whitespace |
| `i` | enter *insert mode* |
| `a` | advance cursor one char and enter *insert mode* |
| `A` | advance cursor to end of line and enter *insert mode* |
| `x` | delete char at cursor, stay in *normal mode* |
| `X` | delete char before cursor, stay in *normal mode* |
| `>>` | move entire line 1 tab stop right, stay in *normal mode* |
| `<<` | move entire line 1 tab stop left, stay in *normal mode* |

## Insert Mode Commands

| Command | Description |
|:-------:|:------------------------- |
| `<C-h>` | delete previous character |
| `<BS>` | delete previous character |
| `<C-v>{char}` | insert character `{char}` literally |
| `<C-v><Tab>` | insert literal `<Tab>` (handy for makefiles) |
| `<C-w>` | delete previous word |
| `<C-u>` | delete everything to left of cursor |
| `<C-c>` | break out of *insert mode*, punt on any auto cmds |
| `<C-a>` | previous insert |
| `<C-@>` | repeat previous insert and return to *normal mode* |
| `<C-t>` | ident line in to next tab stop |
| `<C-d>` | ident line out to previous tab stop |
| `<C-e>` | copy character which is below cursor |
| `<C-y>` | copy character which is above cursor |
| `<C-\>` | potentially a good *insert mode* "leader key" |
| `<C-g>` | potentially a good *insert mode* "leader key" |
| `<C-g>j` | move down a line, Down-Arrow not in vi |
| `<C-g>k` | move up a line, Up-Arrow not in vi |
| `<C-n>` | word completion, next word |
| `<C-p>` | word completion, previous word |
| `<C-o>` | do one *normal mode* command & return to *insert mode* |
| `<C-r>a` | paste from register `"a` while in *insert mode* |

If you accidentally typed `<C-x>` while in insert mode, typing any
non-control character, except `s` will get you back. If you have
terminal flow control turned on, and you hit the unfortunate key
combination `<C-x><C-s>`, something EMACS users are likely to do, you
will find your vim editing session frozen. Type `<C-q>` to unlock.

### Insert Mode vs Replace Mode

- *replace mode* is similar to *insert mode* but
  characters are overwritten instead of inserted.
- You can toggle between them via the terminal
  `<Insert>` key.
- You can enter *replace mode* directly from *normal mode*
  via the `R` command.
- Like in *insert mode* you can navigate around the text
  via the arrow keys creating multiple undo events.
- In *replace mode*, `<BS>` and `<C-h>` undo
  only current set of replacements.
  - Otherwise they act like the `<LefT>` arrow key.

## Command Mode Commands

| Command | Description |
|:--------- |:------------------------------------------ |
| `:r file` | read file and insert it after current line |
| `:nr file` | read file and insert it after line `n` |
| `:w!` | write file overriding normal checks |
| `:n,mw file` | save lines `n` thru `m` to file |
| `:n,mw >>file` | append lines `n` thru `m` to existing file |
| `:'a,'bw file` | save lines from line with mark a to line with mark b |
| `:e!` | reedit file discarding any unsaved changes |
| `:#` | show current line number and print line |
| `:.=` | show current line number |
| `:=` | show number of lines in buffer |
| `:n,md` | delete lines `n` thru `m` |

The `:set`, `:map`, and `:ab` commands are about the only useful things you
can put into vi's configuration file, `~/.exrc`.

### The :set Command

The `:set` command changes vi's default options. Unlike the vim and nvim
commands, there is no '-o' option to set these from the vi command line.

| Command | Description |
|:----------- |:---------------------------------------- |
| `:set list` | display `<Tab>` as `^I` and `EOL` as `$` |
| `:set nolist` | display `<Tab>` & `EOL` normally (the default) |
| `:set tabstop=6` | set the tab stop to 6 characters |
| `:set ts=2` | set the tab stop to 2 characters |
| `:set shiftwidth=4` | set indentation to 4 characters (uses tabs & spaces) |
| `:set` | show all options differing from the defaults |
| `:set all` | show all options and their set values |
| `:set ts?` | query the value of the tabstop option |

### The map: Command

The `:map` command is the only member of the map family of commands
in the original vi. It seems to behavior differently for nvim than vi.

```vim
    :map a 2l`
    :map l 5k`
```

- In nvim: `a` in *normal mode* moves the cursor up 25 lines (not 10).
- In vi: `a` and `l` both move the cursor up 5 lines.

Take away: Neovim uses lexical substitution, not recursive application.
Vi is just buggy.

Almost always, `:noremap` is the better choice for vim and nvim.

### The :ab Command

The vi `:ab` command stands for `:abbreviate`. It works in both
*insert mode* and *command mode*.

```vim
   :ab fb foo bab
```

- When `fb ` is typed, vi replaces it with `foo bar `
- When `fb<CR>` is typed, vi replaces it with `foo bar<CR>`

As with `:set` and `:map`, the `:ab` command is part a of a much larger
family of commands in Vim and Neovim.

```vim
    :iab teh the
    :cab d2f .,$/^foo/d`
```

When `teh ` is typed in *insert mode*, it is replaced by `the `.

When `d2f ` is typed in *command mode*, all lines from current line thru
the next line starting with foo are deleted.

Note: If you use `<space>` as your leader key, the :abbreviate
command will only work when triggered with `<CR>`.

## Marks

Marks allow you to set locations to either be able to jump to or use
with *normal mode* editing commands.

Marks within the file being edited are denoted via letters `a-z`.
A mark is a "zero-width" entity between the cursor and the preceding
character.

| Command | Description |
|:-------:|:---------------------------------------- |
| `ma` | set mark `a` for the current editing buffer |
| `` `a `` | jump to mark `a` current buffer |
| `'a` | jump to first non-space char in line with mark `a` |
| `` d`a `` | delete from cursor to mark `a` |
| `` y`a `` | yank from cursor to mark `a` |
| `d'w` | deletes current line thru line with mark `w` |
| `delm a` | delete mark `a` (Vim & Neovim only) |

Like a mark, the cursor is also a "zero-width" entity between the
highlighted character and the preceding character. If the mark is
before the cursor in the file, the selection does not contain the
highlighted character. Just like the behavior of the `yb` *normal mode*
command.

______________________________________________________________________

| prev: [Vim Factoids][3] | [Home][0] | next: [Ex Mode][5] |

[0]: ../README.md
[3]: 03-VimFactoids.md
[5]: 05-ExMode.md
[50]: http://www.lagmonster.org/docs/vi2.html
