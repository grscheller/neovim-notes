# Multiple Neovim Windows

It was the Berkley Unix nvi (for new vi) which first
introduced multiple windows.  Within a single vim editing
session, using multiple CLI windows can be very useful.

## Basic concepts

* A *file buffer* is the in memory text associated with a file
* A *window* is a viewport on a buffer
* A *tab page* is a collection of windows
* A *terminal window* displays a terminal based program like bash

## Creating and closing windows

| *Norm Mode* Command  | Description                                 |
|:--------------------:|:------------------------------------------- |
| `<C-w>n`             | new window with empty buffer above          |
| `<C-w>s`             | new window same buffer above                |
| `<C-w>v`             | new window same buffer to left              |
| `<C-w>q`             | close current window, same as `:quit`       |
| `<C-w>c`             | close current window, same as `:close`      |
| `<C-w>o`             | close all windows in tab except current one |

| *Cmd Mode* Command   | Description                                  |
|:-------------------- |:-------------------------------------------- |
| `:new`               | new window with empty buffer above           |
| `:vnew`              | new window with empty buffer to left         |
| `:sp`                | new window same buffer above                 |
| `:vsp`               | new window same buffer to left               |
| `:sp <file>`         | edit `<file>` in a horizontal split          |
| `:vsp <file>`        | edit `<file>` in a vertical split            |
| `:vert help <topic>` | view help on a `<topic>` in a vertical split |

The directional sense of all these commands can be adjusted via

| Command           | Description                            |
|:----------------- |:-------------------------------------- |
| `:set splitbelow` | open new windows below, not above      |
| `:set splitright` | open new windows to right, not to left |

Since these behaviors seem more natural to me, I configure them
in my `~/.config/nvim/init.vim` file.

```
    set splitbelow
    set splitright
```

## Navigating between and arranging Windows *Normal Mode*

| Command  | Description                                                  |
|:--------:|:------------------------------------------------------------ |
| `<C-w>h` | move one window left                                         |
| `<C-w>l` | move one window right                                        |
| `<C-w>k` | move one window up                                           |
| `<C-w>j` | move one window down                                         |
| `<C-w>t` | move to top-left window                                      |
| `<C-w>b` | move to bottom-right window                                  |
| `<C-w>p` | move to previous (last accessed) window                      |
| `<C-w>w` | cycle through all windows                                    |
| `<C-w>=` | equalize heights/widths of all windows                       |
| `<C-w>H` | move window far left, full screen height                     |
| `<C-w>J` | move window to bottom, full screen width                     |
| `<C-w>K` | move window to top, full screen width                        |
| `<C-w>L` | move window far right, full screen height                    |
| `<C-w>x` | exchange adjacent windows (vertically or horizontally)       |
| `<C-w>r` | rotate adjacent windows (vertically or horizontally)         |

I find the last 2 work best when window layout is kept simple.
They won't work if one of the windows involved is further split,
thus only work with an innermost split.

## Manually setting/adjusting window sizes

| Command        | Description                            |
|:--------------:|:-------------------------------------- |
| `20<C-w>_`     | set active window height 20 lines      |
| `72<C-w><Bar>` | set active window width 72 chars       |
| `10<C-w>+`     | increase active window height 10 lines |
| `15<C-w>-`     | decrease active window height 15 lines |
| `10<C-w>>`     | increase active window width 10 char   |
| `15<C-w><`     | decrease active window width 15 char   |

Also, note that

| Command      | Description                   |
|:------------:|:----------------------------- |
| `<C-w>_`     | maximize active window height |
| `<C-w><Bar>` | maximize active window width  |

but

| Command  | Description                            |
|:--------:|:-------------------------------------- |
| `<C-w>+` | increase active window height 1 lines  |
| `<C-w>-` | decrease active window height 1 lines  |
| `<C-w>>` | increase active window width 1 char    |
| `<C-w><` | decrease active window width 1 char    |

I tend not to use these directly.  Instead I use them
in other key bindings, or just use the mouse.

## Tab pages

Allows one to group related windows together.  A Tab in vim
is a collection of one or more windows.  With mouse support,
can switch between windows via clicking the "tab".

| Command             | Description                           |
|:------------------- |:------------------------------------- |
| `gt`                | move to next tab                      |
| `gT`                | move to previous tab                  |
| `<C-w>T`            | break current window out into new tab |
| `:tabn`             | move to next tab                      |
| `:tabp`             | move to previous tab                  |
| `:tabnew`           | open new tab with empty buffer        |
| `:tabedit filename` | open new tab to edit file "filename"  |
| `:tabclose`         | close current tab                     |
| `:tabonly`          | close all tabs except current tab     |

## Starting Vim with multiple windows/tabs

```
    $ vim -p[n]    # Open n tab pages (default: one for each file)
    $ vim -o[n]    # Open n windows (default: one for each file)
    $ vim -O[n]    # Like -o but split vertically
```

## Terminal Windows

Traditionally in vi one could interact with the Unix shell via

| Command      | Description                                              |
|:------------ |:-------------------------------------------------------- |
| `:!<cmd>`    | display output of shell command `<cmd>`                  |
| `:2,7!<cmd>` | replace lines 2 thru 7 with shell command output         |
| `:.!<cmd>`   | replace current line with shell command output           |
| `!!<cmd>`    | same as above but from *normal mode*                     |
| `:sh`        | replace editing session with a new shell (not in Neovim) |

Both Neovim & Vim allow you to open a shell in a separate editing Window.
The behaviors between Neovim and Vim are different, so I'll document
the Neovim behavior.  Interacting with the terminal window as a
terminal emulator is called *terminal mode*.  Neovim is emmulating
a VT220/xterm terminal.

| Command               | Description                                    |
|:--------------------- |:---------------------------------------------- |
| `:term`               | opens your default shell in the current window |
| `:vsp term://ksh`     | opens ksh in a new vertical split window       |
| `:sp term://htop`     | opens htop program in a new split window       |
| `:tabnew term://bash` | open bash session in new tab                   |
| `<C-\><C-n>`          | return to *normal mode* from *terminal mode*   |

This terminal window is essentially a read only buffer that
displays the user's interactions with the terminal program running
in the terminal window.  You are put into *normal mode*.  Any
*normal mode* command to enter *insert mode* actually puts you
into *terminal mode*.  In *terminal mode* all key strokes except
`<C-\><C-n>` get passed to the underlying process running
in the terminal window.  If the mouse is enabled, mouse events
get past down too.

When the cursor is on the last line, regardless of mode,
output from the terminal process is scrolled.  Enabling the
mouse makes for a smoother workflow by allowing you to
change windows and tabs more easily.  You can switch to another
vim window/tab with the mouse.  Returning to a terminal
window via the mouse puts you into *normal mode*.

Cutting and pasting via the underlying terminal emulator is
sometimes possible.  For gnome-terminal, hold down the shift key
to allow select/paste the primary buffer via middle mouse button and
shift-rt-click to interact with the secondary buffer.  For alacritty,
the primary buffer is similar, but to copy text to the secondary
buffer, you need to put alacritty into vi-mode via CTRL+SHIFT+SPACE.

---

| prev: [Vim Specific Features][1] | [Home][2] | next: [Encodings & Unicode][3] |

[1]: 05-VimSpecificFeatures.md
[2]: ../README.md
[3]: 07-EncodingsUnicode.md
