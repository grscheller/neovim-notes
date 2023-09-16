# Neovim Configuration

Your next step is to configure Neovim and make it your own.

## Configuration files

Neovim configuration files are stored here, `${XDG_DATA_HOME}/nvim/` which
defaults to `~/.config/nvim/`.

The "entry point" can be as either `init.lua` or `init.vim`. It is
located at the root of this configuration directory. The actually nvim
initialization process is quite complicatted, see

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

A filetype plugin is like a global plugin, but only runs when entering
a buffer nvim has identified by filetype. Filetype plugins can set
options, define keymappings, define abbreviations, define functions and
manipulate the buffer.

The second two are obsoleted by LSP. The only use case I see for them is
getting around a broken nvim system configuration. The sourcing order is
not "per-location", it is "per-function":

#### first wave: filetype plugins

* ~/.config/nvim/ftplugin/foo.lua
* $VIMRUNTIME/ftplugin/foo.lua
* ~/.config/nvim/after/ftplugin/foo.lua

#### second wave: indent scripts

* ~/.config/nvim/indent/foo.lua
* $VIMRUNTIME/indent/foo.lua
* ~/.config/nvim/after/indent/foo.lua

#### third wave: syntax scripts

* ~/.config/nvim/syntax/foo.lua
* $VIMRUNTIME/syntax/foo.lua
* ~/.config/nvim/after/syntax/foo.lua

Where "foo" is a filetype known to nvim. New filetypes can be added via
the function vim.filetype.add() from the Lua filetype module. For an
example, see:

```vim
   :help lua-filetype
```

The /after directory is useful when you want to override or add
to the distributed defaults or system-wide settings.

To see the default search locations and search order, do a

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

My current nvim configuration, [grscheller/nvim][51], is on
GitHub. I directly use [folke/lazy.nvim][52] as my package manager. My
`init.lua` bootstraps this plugin which then manages my other
plugins. It contains lots of good examples that can be followed, and it
is all in the public domain! It is also highly personalized and
opininated.

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
install one of the above Neovim distributions and use it like you would
Intellij or VSCode.

Another problem arises when reverse engineering a Neovim distribution to
figure out how something is done. These configurations need to allow
user changes to override the distribution's default configuration. This
additional infrastructure can be confusing to a beginner. Many times
there are much simpler ways to accomplish what the distribution is
doing. I find looking at plugin documentation, GitHub README's & Wikis,
as well as other users' dotfiles to be the best way to grok nvim
configuration.

Finally, for a simple, nontrival example of a single file Neovim
configuration that can be used as a starting point, I highly recommend
[nvim-lua/kickstart.nvim][62]

---

| prev: [Regular Expressions][9] | [Home][0] |

[9]: 09-RegularExpressions.md
[0]: ../README.md
[41]: https://github.com/LazyVim/LazyVim
[42]: https://github.com/LunarVim/LunarVim
[43]: https://github.com/NvChad/NvChad
[51]: https://github.com/grscheller/nvim
[52]: https://github.com/folke/lazy.nvim
[61]: https://stackoverflow.com/questions/1218390/what-is-your-most-productive-shortcut-with-vim/1220118#1220118
[62]: https://github.com/nvim-lua/kickstart.nvim
