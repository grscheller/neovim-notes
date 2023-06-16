# Neovim Configuring and Mantenance

## My Current Neovim Configuration

Your Neovim configuration is stored here, `${XDG_DATA_HOME}/nvim/` which
defaults to `~/.config/nvim/`.

The entry point can be written in Lua `init.lua` or VimL `init.vim`.

See the
[nvim](https://github.com/grscheller/dotfiles/tree/main/home/config/nvim)
section of my
[dotfiles](https://github.com/grscheller/dotfiles)
repo for my current Neovim configuration.

## Bootstraping Paq and Packer

I initially used a minimalistic Neovim package manager called Paq.
Paq is written in Lua.  Paq manages itself, but initially needs to
be bootstrapped into the standard Neovim package directory.

```fish
   $ git clone https://github.com/savq/paq-nvim.git \
         ~/.local/share/nvim/site/pack/paqs/start/paq-nvim
```

By minimalistic, I mean simple, no auto-loading.

Later on I switched to Packer as my package manager.  Packer also has to
be bootstrapped.

```fish
   $ git clone --depth 1 https://github.com/wbthomason/packer.nvim \
       ~/.local/share/nvim/site/pack/packer/start/packer.nvim  
```

Both Paq and Packer need to be configured in your Neovim configuration
files.  I found Packer to be substantially faster than Paq, even through
it does much more.

Currently, I am using
[lazy.nvim](https://github.com/folke/lazy.nvim)
directly as my plugin manager and lazy loader.  See also
[LazyVim](https://github.com/lazyvim).

## Periodic Mantenance

Typically I do this about twice a week.  If Neovim is working for me,
and I am in the middle of something important, I might let it go a while
longer before doing maintenance.  This also needs to be done after the
above bootstrapping.

```fish
    $ nvim
    :PackerSync
    :q

    $ nvim
    :TSUpdateSync
    :q

    $ nvim
    :checkhealth
    :q
```

I would advice not to try to reload your init.lua configuration after
a `:PackerSync` from within a running nvim session.  Yes, it is
possible, but I have run into multiple complications.  Just exit nvim
between each of these commands.

## Other configuration files

* indent scripts deal with filetype-specific indenting matters
* syntax scripts deal with filetype-specific syntax highlighting matters
* filetype plugins deal with everything else filetype-specific

Indent scripts are sourced after all filetype plugins, including those
in after/, and syntax scripts are sourced after all indent scripts,
including those in after/.

In other words, the sourcing order is not "per-location", it is
"per-function":

### first wave: filetype plugins

* ~/.config/nvim/ftplugin/foo.lua
* $VIMRUNTIME/ftplugin/foo.lua
* ~/.config/nvim/after/ftplugin/foo.lua

### second wave: indent scripts

* ~/.config/nvim/indent/foo.lua
* $VIMRUNTIME/indent/foo.lua
* ~/.config/nvim/after/indent/foo.lua

### third wave: syntax scripts

* ~/.config/nvim/syntax/foo.lua
* $VIMRUNTIME/syntax/foo.lua
* ~/.config/nvim/after/syntax/foo.lua

A filetype plugin is like a global plugin, except that it sets options
and defines mappings for the current buffer only.  The /after directory
is useful when you want to override or add to the distributed defaults
or system-wide settings.

To see the default search locations and search order, do a

```vim
   :help after/directory 
```

and scroll upwards.

---

| prev: [Vimscript][12] | [Home][0] |

[12]: 12-Vimscript.md
[0]: ../README.md
