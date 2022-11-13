# Neovim Configuring and Mantenance

## My Current Neovim Configuration

Your Neovim configuration is stored here,
`${XDG_DATA_HOME}/nvim/` which defaults to `~/.config/nvim/`.

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

```
   $ git clone https://github.com/savq/paq-nvim.git \
         ~/.local/share/nvim/site/pack/paqs/start/paq-nvim
```

By minimalistic, I mean simple, no auto-loading.

Later on I switched to Packer as my package manager.  Packer also
has to be bootstrapped.

```
   $ git clone --depth 1 https://github.com/wbthomason/packer.nvim \
       ~/.local/share/nvim/site/pack/packer/start/packer.nvim  
```

Both Paq and Packer need to be configured in your Neovim configuration
files.  I found Packer to be substantially faster than Paq, even through
it does much more.

## Periodic Mantenance
Typically I do this about twice a week.  If Neovim is working
for me, and I am in the middle of something important, I might
let it go a while longer before doing maintenance.  This also
needs to be done after the above bootstrapping.

```
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

I would advice not to try to reload your init.lua configuration
after a `:PackerSync` from within a running nvim session.  Yes,
it is possible, but I have run into multiple complications.
Just exit nvim between each of these commands.

---

| prev: [Vimscript][11] | [Home][0] |

[11]: 11-Vimscript.md
[0]: ../README.md
