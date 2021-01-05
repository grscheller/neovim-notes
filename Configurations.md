# Configuring Vim and Neovim

## Vim startup files

Without this information, it can be very frustrating
reverse engineer Vim behavior.  Vim sources a number
of files before your editing session starts.  These files
are examples of what is known as vimscript.  Vimscript
is a scripting language based *command mode*.

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

### Very simplistic ~/.vimrc

```
   " ~/.vimrc
   "
   " First .vimrc file I used after being life long vi user.

   " Turn off ugly colors
   syntax off

   " use utf-8
   set encoding=utf-8
   set fileencoding=utf-8

   " Don't use TABS!!!, replace with 4 spaces when <Tab> key is pressed.
   " Use <C-v><Tab> to actually add a real tab.
   set tabstop=4
   set shiftwidth=4
   set expandtab
```

### Last vimrc file I used

Currently used for Syntastic plugin.

```
   " Vim configuration file
   "
   " ~/.vim/vimrc
   "

   "" Enter the 21st Century
   if &compatible
     set nocompatible
   endif
   filetype plugin indent on
   syntax enable

   " Force all files ending in .md, besides just README.md,
   " to be intepreted as MarkDown and not Modula-2
   autocmd BufNewFile,BufReadPost *.md set filetype=markdown

   " Set default encoding and localizations
   set encoding=utf-8
   set fileencoding=utf-8
   set spelllang=en_us

   "" Personnal preferences

   " Setup color scheme
   colorscheme ron

   " Allow :find and gf to use recursive sub-folders
   set path+=**
   set hidden

   " More powerful backspacing
   set backspace=indent,eol,start

   " Make tab completion in command mode more useful
   set wildmenu
   set wildmode=longest:full,full

   " Set default tabstops and replace tabs with spaces
   set tabstop=4
   set shiftwidth=4
   set softtabstop=4
   set expandtab

   " Misc. configurations
   set history=10000   " Number lines of command history to keep
   set mouse=a         " Enable mouse for all modes
   set scrolloff=3     " Keep cursor away from top/bottom of window
   set nowrap          " Don't wrap lines
   set sidescroll=1    " Horizontally scroll nicely
   set sidescrolloff=5 " Keep cursor away from side of window
   set splitbelow      " Horizontally split below
   set splitright      " Vertically split to right
   set ruler           " Show line/column info
   set laststatus=2    " Allows show the status line
   set hlsearch        " Highlight / search results after <CR>
   set incsearch       " Highlight / search matches as you type
   set ignorecase      " Case insensitive search,
   set smartcase       " ... unless query has caps
   set nrformats=bin,hex,octal " bases used for <C-a> & <C-x>,
   set nrformats+=alpha        " ... also single letters too
   set showcmd  " Show partial normal mode commands in lower right corner

   " Duplicate neovim cursor behavior on xterm/urxvt family of terminals
   if &term =~ "xterm\\|rxvt"
     let &t_SI = "\<Esc>[6 q"
     let &t_SR = "\<Esc>[4 q"
     let &t_EI = "\<Esc>[2 q"
   endif

   "" Add optional plugins bundled with nvim
   packadd! matchit    " Add additional matching functionality to %

   "" Setup plugins

   " Setup the Plug plugin manager
   "
   " Bootstrap manually by installing plug.vim into the right place:
   "
   "   $ curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
   "     https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
   "
   " and then from command mode run
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

   " Extend <C-a> and <C-x> to work
   " with dates and not just numbers.
   Plug 'tpope/vim-speeddating'

   " Enable repeating supported plugin maps with "."
   Plug 'tpope/vim-repeat'

   " Indent text objects; defines 2 new text objects
   " based on indentation levels, i and I
   Plug 'michaeljsmith/vim-indent-object'

   " Shows what is in registers
   " extends " and @ in normal mode and <C-r> in insert mode
   Plug 'junegunn/vim-peekaboo'

   call plug#end()

   " Configure user settings for Syntastic
   let g:syntastic_always_populate_loc_list = 1
   let g:syntastic_auto_loc_list = 1
   let g:syntastic_check_on_open = 1
   let g:syntastic_check_on_wq = 0
   let g:syntastic_cursor_column = 0
   let g:syntastic_enable_balloons = 0

   "" Setup key mappings

   " Define <Leader> explicitly as a space
   nnoremap <Space> <Nop>
   let mapleader = "\<Space>"

   " Clear search highlighting
   nnoremap <Leader><Space> :nohlsearch<CR>

   " Toggle Synastic into and out of passive mode
   nnoremap <Leader>st :SyntasticToggleMode<CR>

   " Get rid of all trailing spaces for entire buffer
   nnoremap <Leader>w :%s/ \+$//<CR>

   " Navigating in insert mode without using arrow keys
   inoremap <C-h> <Left>
   inoremap <C-j> <Down>
   inoremap <C-k> <Up>
   inoremap <C-l> <Right>

   " Toggle between 3 line numbering states via <Leader>n
   set nonumber
   set norelativenumber

   function! MyLineNumberToggle()
     if(&relativenumber == 1)
       set nonumber
       set norelativenumber
     elseif(&number == 1)
       set nonumber
       set relativenumber
     else
       set number
       set norelativenumber
     endif
   endfunction

   nnoremap <Leader>n :call MyLineNumberToggle()<CR>
```

### My neovimrc ~/.config/nvim/init.vim file

Currently this is just a translation of my vim version.
I intend to remove configurations which are the defaults
for neovim.  After that, I will start diverging it from
the vim one.

```
   " Neovim configuration file
   "
   " ~/.config/nvim/init.vim
   "

   "" Enter the 21st Century
   if &compatible
     set nocompatible
   endif
   filetype plugin indent on
   syntax enable

   " Force all files ending in .md, besides just README.md,
   " to be intepreted as MarkDown and not Modula-2
   autocmd BufNewFile,BufReadPost *.md set filetype=markdown

   " Set default encoding and localizations
   set encoding=utf-8
   set fileencoding=utf-8
   set spelllang=en_us

   "" Personnal preferences

   " Setup color scheme
   colorscheme ron

   " Allow :find and gf to use recursive sub-folders
   set path+=**
   set hidden

   " More powerful backspacing
   set backspace=indent,eol,start

   " Make tab completion in command mode more useful
   set wildmenu
   set wildmode=longest:full,full

   " Set default tabstops and replace tabs with spaces
   set tabstop=4
   set shiftwidth=4
   set softtabstop=4
   set expandtab

   " Misc. configurations
   set history=10000   " Number lines of command history to keep
   set mouse=a         " Enable mouse for all modes
   set scrolloff=3     " Keep cursor away from top/bottom of window
   set nowrap          " Don't wrap lines
   set sidescroll=1    " Horizontally scroll nicely
   set sidescrolloff=5 " Keep cursor away from side of window
   set splitbelow      " Horizontally split below
   set splitright      " Vertically split to right
   set ruler           " Show line/column info
   set laststatus=2    " Allows show the status line
   set hlsearch        " Highlight / search results after <CR>
   set incsearch       " Highlight / search matches as you type
   set ignorecase      " Case insensitive search,
   set smartcase       " ... unless query has caps
   set nrformats=bin,hex,octal " bases used for <C-a> & <C-x>,
   set nrformats+=alpha        " ... also single letters too
   set showcmd  " Show partial normal mode commands in lower right corner

   "" Add optional plugins bundled with nvim
   packadd! matchit    " Add additional matching functionality to %

   "" Setup plugins

   " Setup the Plug plugin manager
   "
   " Bootstrap manually by installing plug.vim into the right place:
   "
   "   $ curl -fLo ~/.local/share/nvim/site/autoload/plug.vim --create-dirs \
   "     https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
   "
   " and then from command mode run
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
   call plug#begin('~/.local/share/nvim/plugged')

   " Provide syntax checking for a variety of languages
   Plug 'vim-syntastic/syntastic'

   " Provide Rust file detection, syntax highlighting,
   " formatting, syntastic integration, and more
   Plug 'rust-lang/rust.vim'

   " Extend */# functionality while in visual mode
   Plug 'nelstrom/vim-visual-star-search'

   " Surrond text objects with matching (). {}. '', etc
   Plug 'tpope/vim-surround'

   " Extend <C-a> and <C-x> to work
   " with dates and not just numbers.
   Plug 'tpope/vim-speeddating'

   " Enable repeating supported plugin maps with "."
   Plug 'tpope/vim-repeat'

   " Indent text objects; defines 2 new text objects
   " based on indentation levels, i and I
   Plug 'michaeljsmith/vim-indent-object'

   " Shows what is in registers
   " extends " and @ in normal mode and <C-r> in insert mode
   Plug 'junegunn/vim-peekaboo'

   call plug#end()

   " Configure user settings for Syntastic
   let g:syntastic_always_populate_loc_list = 1
   let g:syntastic_auto_loc_list = 1
   let g:syntastic_check_on_open = 1
   let g:syntastic_check_on_wq = 0
   let g:syntastic_cursor_column = 0
   let g:syntastic_enable_balloons = 0

   "" Setup key mappings

   " Define <Leader> explicitly as a space
   nnoremap <Space> <Nop>
   let mapleader = "\<Space>"

   " Clear search highlighting
   nnoremap <Leader><Space> :nohlsearch<CR>

   " Toggle Synastic into and out of passive mode
   nnoremap <Leader>st :SyntasticToggleMode<CR>

   " Get rid of all trailing spaces for entire buffer
   nnoremap <Leader>w :%s/ \+$//<CR>

   " Navigating in insert mode without using arrow keys
   inoremap <C-h> <Left>
   inoremap <C-j> <Down>
   inoremap <C-k> <Up>
   inoremap <C-l> <Right>

   " Toggle between 3 line numbering states via <Leader>n
   set nonumber
   set norelativenumber

   function! MyLineNumberToggle()
     if(&relativenumber == 1)
       set nonumber
       set norelativenumber
     elseif(&number == 1)
       set nonumber
       set relativenumber
     else
       set number
       set norelativenumber
     endif
   endfunction

   nnoremap <Leader>n :call MyLineNumberToggle()<CR>
```

---

| prev: [Regular Expressions][1] | [Home][2] |

[1]: RegularExpressions.md
[2]: README.md
