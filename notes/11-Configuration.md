# Neovim Configuration

Your next step is to configure Neovim and make it your own.

## Configuration files

Neovim configuration files are stored here, `${XDG_DATA_HOME}/nvim/` which
defaults to `~/.config/nvim/`.

The "entry point" can be as either `init.lua` or `init.vim`. It is
located at the root of this configuration directory. The actually nvim
initialization process is quite complicated, see

```vim
    :h initialization
```

### Other configuration files (local "plugins")

* filetype plugins deal with filetype-specific configuration
  * located: `${XDG_DATA_HOME}/nvim/ftplugin`
* indent scripts deal with filetype-specific indenting matters
  * located: `${XDG_DATA_HOME}/nvim/indent`
* syntax scripts deal with filetype-specific syntax highlighting matters
  * located: `${XDG_DATA_HOME}/nvim/syntax`

A filetype plugin scripts is like a global plugin, but only runs when
entering a buffer nvim has identified by filetype. Filetype plugins can
set options, define keymappings, define abbreviations, define functions
and manipulate the buffer.

The second two types of scripts are pretty much obsoleted by LSP. The
only use case I see for them is getting around a broken nvim system
configurations.

The sourcing order is not "per-location", it is "per-function":

#### first wave: filetype plugins

* ~/.config/nvim/ftplugin/foobar.lua
* $VIMRUNTIME/ftplugin/foobar.lua
* ~/.config/nvim/after/ftplugin/foobar.lua

#### second wave: indent scripts

* ~/.config/nvim/indent/foobar.lua
* $VIMRUNTIME/indent/foobar.lua
* ~/.config/nvim/after/indent/foobar.lua

#### third wave: syntax scripts

* ~/.config/nvim/syntax/foobar.lua
* $VIMRUNTIME/syntax/foobar.lua
* ~/.config/nvim/after/syntax/foobar.lua

Where "foobar" is a filetype known to nvim. New filetypes can be added
via the Lua function vim.filetype.add() from the nvim filetype module.
For an example see:

```vim
    :help lua-filetype
```

The /after directory is useful when you want to override or add
to the distributed defaults or system-wide settings.

To see the default search locations and search order,

```vim
    :help after/directory
```

and scroll upwards.

---
:
| prev: [Regular Expressions][10] | [Home][0] | next: [PluginManagers][12] |

[0]: ../README.md
[10]: 10-RegularExpressions.md
[12]: 12-PluginManagers.md
