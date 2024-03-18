# Vim Factoids

These "factoids" are generally true of both Vim and Neovim.

## Buffers and registers in Vim

These are areas that can store text. The three most important ones,
for now, are:

* default register
* named registers
* file buffers

For more in depth information see the section on all the different
types of registers in
[Vim Specific Features](06-VimSpecificFeatures.md#types-of-registers).

Note: In older vi documentation and jargon, registers are referred to as
buffers.

### The default register

The default register is just the area where commands like `y` and `c`
write to and `p` and `P` read from by default.

### Named registers

Illustrated in
[Basic text editing](02-BasicTextEditing.md#you-can-use-named-registers-to-store-text),
Named registers are areas where you can store snippets of text. They
are named `"a` thru `"z` and are essentially 26 independent "clip
boards" that are shared between all the file buffers.

### File buffers

These are the in memory text associated with a file. To list them, use
the `:buffers` command. Among other things, vim gives a unique buffer
number and associates a filename (if any) with that buffer.

| Command       | Description                                              |
|:------------- |:-------------------------------------------------------- |
| `:buffers`    | list buffers in the bufferlist                           |
| `:ls`         | same as above, not the same as `!ls`                     |
| `:n`          | edit next buffer in bufferlist in current window         |
| `:next`       | same as above                                            |
| `:prev`       | edit previous buffer in bufferlist in current window     |
| `:b 2`        | edit buffer 2 in current window                          |
| `:sb 3`       | split window, use buffer 3 for new window                |
| `:sball`      | split all buffers into separate windows                  |
| `:sb`         | open new window using current buffer                     |
| `:sbuffer`    | same as above, basically 2 views of same buffer          |
| `:sp`         | same as above                                            |
| `:split`      | same as above                                            |
| `:vsp`        | same as above, but split window vertically               |
| `:vspilt`     | same as above                                            |
| `:new`        | open new window with a new empty buffer                  |
| `:e file`     | edit buffer associated with file in current window       |
| `:edit file`  | same as above, for both create new buffer if necessary   |
| `:w`          | write buffer to file associated with buffer              |
| `:w file`     | write buffer to file, buffer file association unchanged  |
| `:r file`     | read file into buffer, buffer file association unchanged |
| `:q`          | quit window, fails if last view & changes not saved      |
| `:q!`         | quit window, abandon any changes if last view            |

## Using the mouse

When configured to use the mouse, vim will steal the mouse events from
the terminal emulator. To enable vim mouse support in all modes,
`:set mouse=a` and to disable the mouse and let the terminal emulator
handle all mouse events, `:set mouse=`

Available mouse options are:

| Option | Mode                                 |
|:------:|:------------------------------------ |
| `n`    | for *normal mode*                    |
| `v`    | for *visual mode*                    |
| `i`    | for *insert mode*                    |
| `c`    | for *command mode*                   |
| `a`    | all previous modes                   |
| `h`    | all previous modes only when in help |
| `r`    | for *hit-enter* and *more* prompts   |

You can send mouse events directly to some terminal emulator programs
instead of the editor by holding down the SHIFT key.

When I first started using the mouse in Vim, I found it helpful to just
set `mouse=n`. As I got more comfortable with *visual mode*, setting
`mouse=a` worked well for me, especially when dealing with
[terminal windows](08-MultipleWindows.md#terminal-windows)
in Neovim.

Neovim requires an external program, such as xsel for Unix, so that the
`"*` and `"+` registers interact with the primary and secondary
clipboard buffers. Depending on how it was compiled, Vim can natively
do this.

## Configuring wildmenu

To make tab completion in command mode more efficient, put the following
lines in your ~/.config/nvim/init.vim file.

```vim
    set wildmenu
    set wildmode=longest:full,full
```

## Vi and Vim differences

On modern Unix systems, the vi "executable" is either a symbolic link
to the traditional BSD based ex, or a symbolic (sometimes hard) link
to vim. If the vim executable starts with the name vi, it launches in
so called vi compatibility mode, which is neither POSIX compliant nor
an ex clone.

* Vi only has one level of undo/redo, `u` undoes the
  last change and, if hit again, will redo the change.
  `<C-r>`has no effect, but `R` does puts vi into *replace mode*.
* In vi, hitting `<Insert>` while in *insert mode* or *replace mode*
  does not swap you between them.
* In vi, you cannot navigate around file in *insert mode* or
  *replace mode* with the arrow keys.
* In vi, `<C-o>` does not let you execute a *normal mode*
  command while in *insert mode*.
* There are fewer vi *insert mode* commands.
* In vi *normal mode* the `g`, `K`, `q`, `v`, and `V` keys are
  all unbound.

In *insert mode* vi commands interact with the text just typed, not with
the text in the file buffer. If the `backspace` option is not set, Vim
duplicates the old vi behavior. Setting `backspace` to
`"indent,eol,start"` is the Neovim default setting.

---

| prev: [Basic Text Editing][2] | [Home][0] | next: [Adv Trad Vi Commands][4] |

[2]: 02-BasicTextEditing.md
[0]: ../README.md
[4]: 04-AdvTradViCommands.md
