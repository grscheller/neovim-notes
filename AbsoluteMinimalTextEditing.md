# Absolute Minimum One Needs to Know

Let's say you have ssh to some server that only has vi or vim
installed.  No emacs, no nano.  You only have one quick
edit to do, so it is not worth installing your favorite
editor.  Here is some critical info you need to know in
order to sub-marginally accomplish your task.

## How to exit vi/vim/nvim

You invoked vi via

```
   $ vi /etc/hosts
```

You can move around with the arrow keys but when you try typing
stuff weird things happen.  You try to exit, but nothing works.
Damn it, CTRL-C does not even work!

To get out,

```
   <Esc>:q!<CR>
```

The ESC key brought you back (or left you in) "command mode".
In vim/nvim this mode is known as *Normal Mode*.  The `:` key
put you into "command-line mode", known as *Command Mode*
in vim/nvim.  You are now entering text on the last line of
the terminal which begins with a `:` prompt.  The `q!`
"command-line mode" vi command quits the editor without
saving any changes.  The `<CR>` key, also known as `<Return>`,
`<Enter>` or `<EOL>` key, submits the command ending your session.

From now on, we'll refer to these modes by their vim/nvim names.

## The 3 main modes

* *Normal Mode*: Used to navigate file and issue "visual interface" (vi) commands
* *Insert Mode*: Used to type text into the file
* *Command Mode*: Used to issue "line editor" (ex) commands

## Minimal command set common to vi, vim, and nvim

Here are a minimal common subset of commands for vi, vim, and nvim.

### Cursor movement in *Normal Mode*

| Command  | Description                                 |
|:--------:|:------------------------------------------- |
| `h`      | move cursor one character to left           |
| `l`      | move cursor one character to right          |
| `j`      | move cursor one line directly down          |
| `k`      | move cursor one line directly up            |
| `w`      | move forward to beginning next word         |
| `b`      | move back to beginning of word cursor is on |
| `e`      | move forward to end of of word cursor is on |
| `$`      | move cursor to end of line                  |
| `0`      | move cursor to start of line                |

In vi, depending on terminal type, arrow keys will also work to
navigate the file in the editing buffer.

### Changing text and/or interacting with "the buffer" in Normal Mode

Buffer is older vi jargon for what is now called the
default register in vim/nvim.

| Command   | Description                                            |
|:---------:|:------------------------------------------------------ |
| `dd`      | delete line and put in default register (cut)          |
| `5dd`     | delete 5 lines and put in default register             |
| `D`       | delete to end of line and put in default register      |
| `yy`      | yank line to default register (copy)                   |
| `x`       | delete character under cursor, put in default register |
| `r<char>` | change current char cursor is on to `<char>`           |
| `p`       | paste default register contents "after"                |
| `P`       | paste default register contents "before"               |
| `J`       | join curent & next line, insert spaces as needed       |

What "before" or "after" mean depends on what is in the default register.

### Commands to insert or manipulate text

These *Normal Mode* commands take vim to *Insert Mode*.
To return to *Normal Mode*, type either `<Esc>` or `<C-[>`.

| Command | Description                                                |
|:-------:|:---------------------------------------------------------- |
| `i`     | insert text before character cursor is on                  |
| `a`     | insert text after character cursor is on                   |
| `A`     | insert text at end of line                                 |
| `o`     | open new line after current line in insert text            |
| `O`     | open new line before current line in insert text           |
| `C`     | change to end of line                                      |
| `3cw`   | change next three words                                    |
| `2c3w`  | change next six words                                      |

### Insert Mode

The whole vi paradigm is that you do all navigation in *normal mode*
and type text into the file buffer in *insert mode*.  You return
to *normal mode* by pressing the `<Esc>` key.

### Command Mode

Use the `:` command to enter *Command Mode*.  The
cursor jumps down to the bottom of the terminal window
and prompts you with `:`.

| Command             | Description                                            |
|:------------------- |:------------------------------------------------------ |
| `:w`                | write to disk file being edited                        |
| `:w file`           | write to file, still editing original file             |
| `:q`                | quit editing, will warn about unsaved changes          |
| `:wq`               | write to disk, then quit                               |
| `:q!`               | quit without saving unsaved changes                    |
| `:n`                | edit next buffer typically next file on command line   |
| `:prev`             | edit previous buffer                                   |
| `:42`               | move cursor to beginning of line 42                    |
| `:#`                | give line number of current line cursor is on          |
| `:s/foo/bar/`       | substitute first instance of foo with bar              |
| `:s/foo/bar/g`      | substitute all instances of foo with bar               |
| `:17,42s/foo/bar/g` | substitute all foo with bar, lines 17 to 42            |
| `:/dog/`            | jump next line with `dog` in it (first non-whitespace) |
| `/dog`              | jump to next instance of `dog` in file                 |

### Repeating commands in Normal Mode

| Command | Description                                |
|:-------:|:------------------------------------------ |
| `.`     | repeat the last command which changed text |

This repeats the last *Normal Mode* command used which changed text.
It does not repeat *Command Mode* commands.

## POSIX command  line editing mode

The non-multiline *Normal Mode* commands above
also apply to POSIX shell when in vi cmdline
editing mode.  By default, Bash uses emacs editing
mode.  Put `set -o vi` in your `~/.bashrc` file to
enable vi editing mode.

* When the prompt is printed you are in *Insert Mode*
* Pressing `<Esc>` puts you in *Normal Mode* with a one line view
* Pressing `k` takes you back through your command line history
* Pressing `j` takes you forward through your command line history
* Pressing `h` and `l` moves you forward and back on the command line
* The `f`, `t`, `F`, `T`, `;`, `,` commands, covered later, are useful

---

| [Home][1] | next: [Basic Text Editing][2] |

[1]: README.md
[2]: BasicTextEditing.md
