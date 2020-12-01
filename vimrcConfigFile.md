# Configuring Vim

## Vim startup files

Without this information, it can be very frustrating
reverse engineer Vim behavior.  Vim sources a number
of files before your editing session starts.  These files
are examples of what is known as vimscript.  Vimscript
is a scripting language based *Command Mode*.

First sourced is `/etc/vimrc`.  Historically on UNIX, this
was the location for system-wide vim configuration changes.
Now-a-days, this file has a command that causes vim to source
your Linux distribution's vim-package related configuration
changes.  For Arch Linux this command is,

```
   runtime! archlinux.vim
```

Vim looks for this file at this location: `/usr/share/vim/vimfiles/`,
which is compiled into the vim executable.

Vim next looks for user configuration changes in `~/.vimrc`,
if it does not exist, it then looks in `~/.vim/vimrc`.

**Warning:** If neither `~/.vimrc` nor `~/.vim/vimrc` exist,
vim will source the `defaults.vim` file.  This has the
currently compiled in of location: `/usr/share/vim/vim82/`.
This can very well override behavior in
both `/etc/vimrc` and `/usr/share/vim/vimfiles/archlinux.vim`.
Not knowing about the existence of these mechanisms can be
very confusing to new and intermediate vim users.  Simply
creating an empty ~/.vimrc file can radically change
vim behavior, and the user has no clue how to recover
previous desirable features.  Putting the line

```
    let skip_defaults_vim=1
```

in `/etc/vimrc` will stop this "feature."

Your linux system admins may also have added other "helpful"
system-wide configuration changes into any of these files,
possibly even creating other files in the compiled in
locations.

## Sample vimrc files

The commands in these .vimrc example files run
as if in vim *command mode*.  Comments begin with `"`.

### Simplistic .vimrc

```
   " ~/.vimrc
   "
   " A very simplistic .vimrc file I first
   " used after being a life long vi user.

   " Turn off ugly colors
   syntax off

   " use utf-8
   set encoding=utf-8
   set fileencoding=utf-8

   " Don't use TABS!!!, replace with 4 spaces when <tab> key is pressed.
   " Use <ctrl-v> <tab> to actually add a real tab.
   set tabstop=4
   set shiftwidth=4
   set expandtab
```

### The vimrc file I currently use

```
   " Vim configuration file
   "
   " ~/.vim/vimrc
   "

   " Enter the 21st Century
   if &compatible
     set nocompatible
   endif
   syntax enable
   filetype plugin on

   " Set default encoding and localizations
   set encoding=utf-8
   set fileencoding=utf-8
   set spelllang=en_us

   " Allow :find and gf to use recursive sub-folders
   set path+=**
   set hidden

   " More powerful backspacing
   set backspace=indent,eol,start

   " Make tab completion in command mode more efficient
   set wildmenu
   set wildmode=longest:full,full

   " Set default tabstops and replace tabs with spaces
   set tabstop=4
   set shiftwidth=4
   set softtabstop=4
   set expandtab

   " Misc. configurations
   set history=5000  " Number lines of command history to keep
   set mouse=n       " Enable mouse for normal mode only
   set scrolloff=3   " Keep cursor away from edge of window
   set nowrap        " Don't wrap lines
   set sidescroll=1  " Horizontally scroll nicely
   set splitbelow    " Horizontally split below
   set splitright    " Vertically split to right
   set ruler         " Show line/column info

   " Setup the Plug plugin manager
   "
   " Bootstrap manually by installing it into the right place:
   "
   "   $ curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
   "     https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
   "
   " and then from within vim run
   "
   "   :PlugInstall
   "
   " Plug Commands:
   "   :PlugInstall [name ...] [#threads] Install plugins
   "   :PlugUpdate [name ...] [#threads]  Install or update plugins
   "   :PlugClean[!]           Remove unlisted plugins
   "   :PlugUpgrade            Upgrade Plug itself
   "   :PlugStatus             Check the status of plugins
   "   :PlugDiff               Diff previous update and pending changes
   "   :PlugSnapshot[!] [path] Generate script to restore current plugin state
   "
   call plug#begin('~/.vim/plugged')

   " Provide syntax checking for a variety of languages
   Plug 'vim-syntastic/syntastic'

   " Provide Rust file detection, syntax highlighting,
   " formatting, syntastic integration, and more
   Plug 'rust-lang/rust.vim'

   " Extend */# functionality while in visual mode
   Plug 'nelstrom/vim-visual-star-search'

   " Surrond text objects with matching (). {}. '', etc
   Plug 'tpope/vim-surround'

   " Enable repeating supported plugin maps with "."
   Plug 'tpope/vim-repeat'

   " Indent text objects; defines 2 new text objects
   " based on indentation levels, i and I
   Plug 'michaeljsmith/vim-indent-object'

   " Visualizes undo history; switch between undo branches
   Plug 'mbbill/undotree'

   " Shows what is in registers
   " extends " and @ in normal mode and <CTRL-R> in insert mode
   Plug 'junegunn/vim-peekaboo'

   " Extend <ctrl>-A <ctrl>-X to work with dates and not just numbers
   Plug 'tpope/vim-speeddating'

   call plug#end()

   " Configure user settings for Syntastic
   let g:syntastic_always_populate_loc_list = 1
   let g:syntastic_auto_loc_list = 1
   let g:syntastic_check_on_open = 1
   let g:syntastic_check_on_wq = 0
   let g:syntastic_cursor_column = 0
   let g:syntastic_enable_balloons = 0

   " Force all files ending in .md, besides just README.md,
   " to be intepreted as MarkDown and not Modula-2
   autocmd BufNewFile,BufReadPost *.md set filetype=markdown

   " Define <Leader> explicitly as \
   let mapleader = "\\"

   " Clear search highlighting with \\
   nnoremap <leader>\ :nohlsearch<return>

   " Toggle Synastic into and out of passive mode
   nnoremap <leader>st :SyntasticToggleMode<return>
```

---

| prev: [Regular Expressions][1] | [Home][2] |

[1]: regExp.md
[2]: README.md
