# Absolute Minimum One Needs to Know

Let's say you've logged into some server which only has vi or vim
installed. No emacs, no nano. You only have one quick edit to do, so
it is not worth installing your favorite editor. Here is some critical
info you need to know in order to sub-marginally accomplish your task.

## How to exit vi/vim/nvim

You invoked vi via

```fish
    $ vi /etc/hosts
    $
```

You can move around with the arrow keys but when you try typing stuff
weird things happen. You try to exit, but nothing works. Damn it,
CTRL-C does not even work!

To get out,

```vim
    <Esc>:q!<CR>
```

The ESC key brought you back (or left you in) "command mode". In
vim/nvim this mode is known as *normal mode*. The `:` key put you into
"command line mode", known as *command mode* in vim/nvim. You are now
entering text on the last line of the terminal which begins with a `:`
prompt. The `q!` "command line mode" vi command quits the current
editing session without saving any changes. The `<CR>` key, also known
as the `<Return>`, `<Enter>` or `<EOL>` key, submits the command ending
your session.

From now on, we'll refer to these modes by their vim/nvim names. Also,
we will make a distinction between the file stored on disk and the "text
buffer" image of the file read into memory. Text buffers are usually,
but not always, associated with a file.

---

## The 3 main modes

* *normal mode*: Navigate text buffer & issue "visual interface" (vi) commands
* *insert mode*: Insert text into the text buffer
* *command mode*: Execute "line editor" (ex) commands on the text buffer

If in either *insert mode* or *command mode*, `<Esc>` will take you to
*normal mode*.

---

## Minimal command set common to vi, vim, and nvim

Here are a minimal common subset of commands for vi, vim, and nvim.

### Cursor movement in *Normal Mode*

*Normal mode* is the default mode you are put in when the editor is
started. Hitting `<Esc>` while in *insert mode* or *command mode* will
put you back into *normal mode*.

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

In vi/vim/nvim, depending on terminal type, arrow keys will sometimes
work to navigate the text buffer in *normal mode*.

### Commands to insert or manipulate text

These *normal mode* commands take vim to *insert mode*. To return to
*normal mode*, type `<Esc>`.

| Command | Description                                      |
|:-------:|:------------------------------------------------ |
| `i`     | insert text before character cursor is on        |
| `a`     | insert text after character cursor is on         |
| `A`     | insert text at end of line                       |
| `o`     | open new line after current line in insert text  |
| `O`     | open new line before current line in insert text |
| `3cw`   | delete next 3 words and insert text              |
| `c2w`   | delete next 2 words and insert text              |

### Changing text and/or interacting with the "buffer" in *Normal Mode*

The "buffer" is older vi jargon for what is now called the default
register in vim/nvim.

| Command   | Description                                            |
|:---------:|:------------------------------------------------------ |
| `dd`      | delete line and put in default register (cut)          |
| `5dd`     | delete 5 lines and put in default register             |
| `d$`      | delete to end of line, put in default register         |
| `d0`      | delete to beginning of line, put in default register   |
| `yy`      | yank line to default register (copy)                   |
| `x`       | delete character under cursor, put in default register |
| `r<char>` | change current character cursor highlights to `<char>` |
| `p`       | paste default register contents "after"                |
| `P`       | paste default register contents "before"               |
| `J`       | join current & next line, insert spaces as needed      |

What "before" or "after" mean depends on what is in the default register.

### *Insert Mode*

The original vi paradigm was that you do all navigation in *normal mode*
and type text into the text buffer in *insert mode*. From *insert mode*
you return to *normal mode* by pressing the `<Esc>` key.

For vim and nvim you can navigate through the text buffer while in
*insert mode* with the arrow keys. You cannot navigate this way in vi.

### *Command Mode*

From *normal mode* use  `:`, `/`, or `?` to enter *command mode*. The
cursor jumps down to the bottom of the terminal window and to prompts
you with either `:`, `\`, or `?` depending on which one you typed. The
later two are used to search forward and backward respectfully in the
text buffer.

| Command             | Description                                             |
|:------------------- |:------------------------------------------------------- |
| `:w`                | write to disk file being edited                         |
| `:w file`           | write to file, buffer still associated with orig file   |
| `:q`                | quit editing, will warn about unsaved changes           |
| `:wq`               | write to disk, then quit                                |
| `:q!`               | quit without saving unsaved changes                     |
| `:n`                | edit next buffer typically next file on command line    |
| `:prev`             | edit previous buffer                                    |
| `:42`               | move cursor to beginning of line 42                     |
| `:#`                | give line number of current line cursor is on           |
| `:s/foo/bar/`       | substitute 1st instance of foo with bar on current line |
| `:s/foo/bar/g`      | substitute all `foo` with `bar` on current line         |
| `:17,42s/foo/bar/g` | substitute all `foo` with `bar`, lines 17 to 42         |
| `:/dog/`            | jump to next line with `dog` in it                      |
| `:/dog/s/7/42/`     | jump to line with `dog` and replace first 7 with 42     |
| `/dog`              | jump forward to next instance of `dog` in text buffer   |
| `?cat`              | jump back to previous instance of `cat` in text buffer  |
| `/`                 | jump forward using last search pattern                  |
| `?`                 | jump back using last search pattern                     |

Entering `<CR>` will cause the above commands to be executed and return
you to *normal mode*. Hitting `<Esc>` instead will punt on running the
command and return you to *normal mode*.

### Repeating commands in *Normal Mode*

| Command | Description                                              |
|:-------:|:-------------------------------------------------------- |
| `.`     | repeat the last *normal mode* command which changed text |

This repeats the last *normal mode* command used which changed text. It
does not repeat *command mode* commands.

---

| [Home][0] | next: [Basic Text Editing][2] |

[0]: ../README.md
[2]: 02-BasicTextEditing.me
