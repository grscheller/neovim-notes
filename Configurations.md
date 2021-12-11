# Configuring Neovim

## Past Configurations

* A very early Vim [~/.vimrc](init_examples/first_vimrc)
* A more fleshed out Vim [~/.vim/vimrc](init_examples/later_vimrc)
* My last Neovim [~/.config/nvim/init.vim](init_examples/last_init.vim)
* My first Neovim [~/.config/nvim/init.lua](init_examples/first_init.lua)
* My last stable Neovim [~/.config/nvim/init.lua](init_examples/later_init.lua)

## Bootstraping Paq 

Paq is a minimal Neovim package manager written in Lua.  Paq manages
itself, but initially needs to be bootstrapped into the standard
Neovim package directory.

```
   $ git clone https://github.com/savq/paq-nvim.git \
       ~/.local/share/nvim/site/pack/paqs/start/paq-nvim
```

By minimal, I mean simple, no auto-loading.

## Periodic Mantenance
Typically I do this about twice a week.  If Neovim is working
for me, and I am in the middle of something important, I might
let it go a while longer before doing maintenance.

```
    $ nvim -c ':PaqSync'
    :q
    $ nvim -c ':checkhealth'
    :q
```

I would advice not to try to reload your init.lua configuration
from within a running nvim session.  Yes, it is possible, but
you are asking for complications.  Hence, that is why I exit
nvim between the `:PaqSync` and the `:checkhealth`.

---

| prev: [Regular Expressions][1] | [Home][2] |

[1]: RegularExpressions.md
[2]: README.md
