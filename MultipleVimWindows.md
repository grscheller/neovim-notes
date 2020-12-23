# Multiple Vim Windows

It was the Berkley Unix nvi (for new vi) which first
introduced multiple windows.

Within a single vim terminal editing session, using multiple
CLI ncurses windows can be very useful.  For example,
using the help facility `:help <command>` can be confusing
if you don't know how to navigate multiple vim windows.

## Basic concepts

* A *file buffer* is the in memory text associated with a file
* A *window* is a viewport on a buffer
* A *tab page* is a collection of windows
* A *terminal window* is a window that displays a Shell

## Creating new windows *Command Mode*

| Command | Description                             |
|:------- |:--------------------------------------- |
| `:new`  | new window with empty buffer above      |
| `:vnew` | new window with empty buffer to left    |
| `:spl`  | new window new view same buffer above   |
| `:vspl` | new window new view same buffer to left |

## Creating new windows *Normal Mode*

| Command  | Description                             |
|:--------:|:--------------------------------------- |
| `<C-w>n` | new window with empty buffer above      |
| `<C-w>s` | new window new view same buffer above   |
| `<C-w>v` | new window new view same buffer to left |
| `<C-w>T` | break current window out in new tab     |

The directional sense of these commands can be adjusted via

| Command           | Description                            |
|:----------------- |:-------------------------------------- |
| `:set splitbelow` | open new windows below, not above      |
| `:set splitright` | open new windows to right, not to left |

I configure these in my`~/.vim/vimrc`

```
   set splitbelow
   set splitright
```

## Navigating between Windows *Normal Mode*

| Command     | Description           |
|:-----------:|:--------------------- |
| `<C-w>h`    | move one window left  |
| `<C-w>l`    | move one window right |
| `<C-w>k`    | move one window up    |
| `<C-w>j`    | move one window down  |

## Adjusting windows size (without mouse support)

### Equalize window size

| Command     | Description                            |
|:-----------:|:-------------------------------------- |
| `<C-w>=`    | equalize heights/widths of all windows |

### Setting/adjusting window sizes

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

## Tab pages

Allows one to group related windows together.  A Tab in vim
is a collection of one or more windows.  With mouse support,
can switch between windows via clicking the "tab".

| Command           | Description                    |
|:----------------- |:------------------------------ |
| `:tabn`           | move to next tab               |
| `:tabp`           | move to previous tab           |
| `:tabnew`         | open new tab with empty buffer |
| `:tabedit <file>` | open new tab to edit `<file>`  |
| `gt`              | move to next tab               |
| `gT`              | move to previous tab           |

## Starting Vim with multiple windows/tabs

```
   $ vim -p[n]    # Open n tab pages (default: one for each file)
   $ vim -o[n]    # Open n windows (default: one for each file)
   $ vim -O[n]    # Like -o but split vertically
```

## Terminal Windows

Traditionally in vi one could interact with the Unix shell via

| Command   | Description                                              |
|:--------- |:----------------------------------------                 |
| `:!<cmd>` | display output of shell command `<cmd>`                  |
| `:sh`     | replace editing session with a new shell (not in Neovim) |

Vim allows you to open a shell in a separate Vim Window

| Command      | Description                                 |
|:------------ |:------------------------------------------- |
| `:term`      | open a shell in a new horizontal Vim window |
| `:vert term` | open a shell in a new vertical Vim window   |

This terminal window is essentially a read only vim session that
displays the shell session interactions.  You are put into
*insert mode* and input is passed to the shell.  You can copy
text by jumping to *normal mode* using the usual vim buffers.
Entering *insert mode* will return you to the terminal session.
You can only paste into the shell session itself while in *insert mode*.

| Command   | Description                                      |
|:---------:|:------------------------------------------------ |
| `<C-w>""` | Paste contents default buffer into shell session |
| `<C-w>"a` | Paste contents buffer `"a` into shell session    |
| `<C-w>N`  | Put vim terminal window into *normal mode*       |

---

| prev: [Vim Specific Features][1] | [Home][2] | next: [Encodings & Unicode][3] |

[1]: VimSpecificFeatures.md
[2]: README.md
[3]: EncodingsUnicode.md
