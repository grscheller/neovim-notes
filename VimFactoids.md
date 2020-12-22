# Vim Factoids

## Buffers and registers in Vim

These are areas that can store text.  The three most important ones,
for now, are:

* default register
* named registers
* file buffers

For more in depth information see the section on all the different
types of registers in
[Vim Specific Features](VimSpecificFeatures.md#types-of-registers).

Note: In older vi documentation and jargon, registers are referred
to as buffers.

### The default register

The default register is just the area which `y` and `c` commands
write to and `p` and `c` commands read from by "default."

### Named registers

Illustrated in [Basic text editing](BasicTextEditing.md#you-can-use-named-registers-to-store-text),
Named registers are areas where you can store snippets of text.
They are named `"a` thru `"z` and are essentially 26
independent "clip boards" that are shared between all the
file buffers.

### File buffers

These are the in memory text associated with a file.  To list
them, use the `:buffers` command.  Among other things, vim
gives a unique buffer number and associates a filename (if any)
with that buffer.

| Command       | Description                                 |
|:------------- |:-------------------------------------------------------- |
| `:n`          | edit next buffer                                         |
| `:next`       | same as above                                            |
| `:prev`       | edit previous buffer                                     |
| `:edit file`  | edit buffer associated with file in current window       |
| `:e file`     | same as above, for both, creates new buffer if necessary |
| `:buffers`    | list buffers                                             |
| `:ls`         | same as above, not the same as `!ls`                     |
| `:b2`         | edit buffer 2 in current window (not commonly used)      |
| `:new`        | open new window with a new empty buffer                  |
| `:split`      | open new window but use the same buffer                  |
| `:spl`        | same as above, basically 2 views of same buffer          |
| `:w`          | write buffer to file associated with buffer              |
| `:w file`     | write buffer to file, buffer file association unchanged  |
| `:q`          | quit window, fails if last view & changes not saved      |
| `:q!`         | quit window, abandon any changes if last view            |

## Using the mouse

When configured to use the mouse, vim will steal the mouse
events from the terminal emulator.  To enable full vim mouse
support, `:set mouse=a` and to disable the mouse and let the
terminal emulator handle all mouse events, `:set mouse=`

Available mouse options are:

| Option | Mode                                 |
|:------:|:------------------------------------:|
| `n`    | *Normal Mode*                        |
| `v`    | *Visual Mode*                        |
| `i`    | *Insert Mode*                        |
| `c`    | *Command Mode*                       |
| `a`    | All previous modes                   |
| `h`    | All previous modes only when in help |

As a workaround, you can send mouse events directly to the
terminal emulator instead of Vim by holding down the SHIFT
key.

## Configuring wildmenu

To make tab completion in command mode more efficient, put the
following lines in your ~/.vim/vimrc or ~/.vimrc file.

```
   set wildmenu
   set wildmode=longest:full,full
```

## Vi and Vim differences

* Vi only has one level of undo/redo, `u` undo the
  last change and, if hit again, will redo the change.
  `<C-r>`has no effect.
* On modern Linux systems, the vi "executable" is either a
  symlink to ex, traditional BSD based vi, or a symlink
  to vim.  If the vim executable starts with the name vi,
  it launches in vi compatibility mode.  Vim in compatibility
  mode is neither POSIX compliant nor an ex clone.
* In vi, you cannot navigate around file in *Insert Mode* or
  *Replace Mode* with the arrow keys.
* Hitting `<Insert>` while in *Insert Mode* or *Replace Mode* does
  not swap you between them.
* In vi, `<C-o>` does not let you execute a *Normal Mode*
  command while in *Insert Mode*.

---

| prev: [Basic Text Editing][1] | [Home][2] | next: [Adv Trad Vi Commands][3] |

[1]: BasicTextEditing.md
[2]: README.md
[3]: AdvTradViCommands.md