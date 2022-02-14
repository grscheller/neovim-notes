# Configuring Neovim

## Past Configurations

Here are some of my past Vim/Neovim configurations.

* A very early Vim [~/.vimrc](init_examples/first_vimrc)
* A more fleshed out Vim [~/.vim/vimrc](init_examples/later_vimrc)
* My last Neovim [~/.config/nvim/init.vim](init_examples/last_init.vim)
* My first Neovim [~/.config/nvim/init.lua](init_examples/first_init.lua)

The advantages of having a configuration in one single
place eventually gets outweighed by its complexity.  See the
[nvim](https://github.com/grscheller/dotfiles/tree/main/config/nvim)
section of my
[dotfiles](https://github.com/grscheller/dotfiles)
repo for my current Neovim configuration.

## Bootstraping Paq and Packer

Paq is a minimal Neovim package manager written in Lua.  Paq manages
itself, but initially needs to be bootstrapped into the standard
Neovim package directory.

```
   $ git clone https://github.com/savq/paq-nvim.git \
         ~/.local/share/nvim/site/pack/paqs/start/paq-nvim
```

By minimal, I mean simple, no auto-loading.

Later on I switched to Packer as my package manager.  Packer also
has to be bootstrapped.

```
   $ git clone --depth 1 https://github.com/wbthomason/packer.nvim \
       ~/.local/share/nvim/site/pack/packer/start/packer.nvim  
```

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
from within a running nvim session.  Yes, it is possible, but I
have run into multiple complications.  That is why I exit nvim
between each of these commands.

---

| prev: [Lua][1] | [Home][2] |

[1]: Lua10.md
[2]: README.md
