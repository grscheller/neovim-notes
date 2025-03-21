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

## Neovim distributions

There are so called "neovim distributions" which will give you
a complete "out-of-the-box" debugging LSP based IDE configuration which
can be used as a starting point for your own configuration.

See these well known examples.

* [LazyVim/LazyVim][41]
* [LunarVim/LunarVim][42]
* [NvChad/NvChad][43]

### My personal nvim configuration

My current nvim configuration can be found in my dotfiles repo here
[grscheller/dotfiles][51] on GitHub. I directly use
[folke/lazy.nvim][52] as my package manager. My `init.lua` bootstraps
this plugin which then manages my other plugins. It contains lots of
good examples that can be followed, and it is all in the public domain!
It is also highly personalized and opinionated.

### Final advise

Neovim distributions can be useful if you don't have the time to develop
your own configurations, but to truly master Neovim, and make it your
own, you need to spend the time to "grok" it. Creating your own
configuration helps a lot with this. Practicing editing text efficiently
helps too.

In answer to a Stack Overflow question titled
[What is your most productive shortcut with Vim?][61]
the top-rated answer (1124 up votes) was "Your problem with Vim is that
you don't grok vi." Likewise, you will be limited if all you do is
install one of these Neovim distributions and use it like you would
IntelliJ or vscode.

Another problem arises when reverse engineering a Neovim distribution to
figure out how something is done. These configurations need to allow
user changes to override the distribution's default configuration. This
additional infrastructure can be confusing to a beginner. Many times
there are much simpler ways to accomplish what the distribution is
doing since there is no need to "override" anything. I find looking at
plugin documentation, GitHub README's & Wikis, as well as other users'
dotfiles to be the best way to learn how to configure nvim.

Finally, for a simple, nontrivial example of a single file Neovim
configuration that can be used as a starting point, I highly recommend
[nvim-lua/kickstart.nvim][62]

---

| prev: [Regular Expressions][10] | [Home][0] |

[10]: 10-RegularExpressions.md
[0]: ../README.md
[41]: https://github.com/LazyVim/LazyVim
[42]: https://github.com/LunarVim/LunarVim
[43]: https://github.com/NvChad/NvChad
[51]: https://github.com/grscheller/dotfiles
[52]: https://github.com/folke/lazy.nvim
[61]: https://stackoverflow.com/questions/1218390/what-is-your-most-productive-shortcut-with-vim/1220118#1220118
[62]: https://github.com/nvim-lua/kickstart.nvim
