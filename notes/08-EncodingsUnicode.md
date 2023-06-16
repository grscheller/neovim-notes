# Encodings and Unicode

Vim is capable of using different character encodings when editing and
saving files.  I find UTF-8 the most useful encoding.

## Character Encodings

Vim is capable of working with different character encodings.

| Command                  | Description                                  |
|:------------------------ |:-------------------------------------------- |
| :set encoding            | list current character encoding              |
| :set encoding=utf-8      | set current character encoding vim displays  |
| :set fileencoding        | list current encoding in which to save files |
| :set encoding=latin1     | edit using LATIN-1 character encoding        |
| :set fileencoding=latin1 | save files with LATIN-1 encoding             |
| :set fileencodings       | list encodings to try when loading a file    |

The two main encodings are UTF-8 for UNIX, MAC-OS and HTML/Web, and
UTF-16 for Microsoft Windows.  Windows tends to use a mix of UTF-16,
UTF-8, and legacy encodings.

On POSIX systems I used to put

```vim
    set encoding=utf-8
    set fileencoding=utf-8
```

in my `~/.vim/vimrc`.  Now I do a similar configuration via `init.lua`.

When I go native on Windows, vim automatically figures out whether the
edited file is in UTF-8 or UTF-16LE with `\r\n` line endings.

(TL;DR): GIT, for text files, does the conversion to the correct format
depending on the OS.  In the olde days, we used to FTP files in text
mode to convert format between different OS's.

## Vim Digraphs

Following the recommendations of
[RFC-1345](https://tools.ietf.org/html/rfc1345),
vim allows users to enter characters within whatever encodings they are
using via 2 character "diagraph" sequences.  While in *insert mode*,
type `<C-k>` followed by a two character sequence.

|  Command   | Description                                   |
|:----------:|:--------------------------------------------- |
|  `:dig`    | list diagraphs available for current encoding |
|  `<C-k>a^` | enter the character â                         |
|  `<C-k>o:` | enter the character ö                         |
|  `<C-k>i'` | enter the character í                         |
|  `<C-k>l*` | enter the character λ                         |

The choice of characters you can enter this way depends on the encoding
you are using.

## Using Unicode Code Points

In modern Unix terminal emulators and Libre Office, input and display of
Unicode code points just works.  Terminals are fixed width font beasts,
but Libre Office handles the variable width code points just fine.

When using gvim, or vim/nvim with a unicode aware terminal emulator like
rxvt-unicode or gnome-terminal, code points can be entered while in
*insert mode* via

|  Command           | Description            |
|:------------------:|:---------------------- |
|  `<C-S-u>03b2<CR>` | enter the character β  |
|  `<C-S-u>3bb<CR>`  | enter the character λ  |

Where `<C-S-u>` means holding down CTRL+SHIFT+u.

Note: Using `<S-C-u>` will not work.

Note: `<C-S-u>` does not work while on the linux console.

Note: Defining vim key mapping with "key chords" like `<C-S-u>` will not
work, neither will making it the target of the mapping.

(TL;DR): Be aware that for both gvim and vim/nvim running in a terminal
emulator, you are not interacting with the "terminal" being emulated,
but the underlying GUI application.  As far as a real terminal is
concerned, `CTRL+d` and `CTRL+D` are the same control character, ASCII
4.  That is what is coming down the RS-232 cable.  The Graphical User
Interface (GUI) is seeing all your keystrokes and can distinguish
whether you pressed `<C-d>` or `<C-S-d>`.

(TL;DR): Terminals in the vt102/vt220 family are keyboard/monitor
interfaces to RS-232 cables.  Holding the CTRL key down electrically
zeroed out bits 6 and 7 of the 7-bit character key typed.  Likewise,
holding the down the SHIFT key zeroed out bit 6.  The ASCII values were
choosen to make this work.

| ASCII | Decimal    | Octal | Binary     | Symbol   | Description           |
| -----:|:---------- |:-----:|:----------:|:--------:|:--------------------- |
| `107` | `64+32+11` | `153` | `01101011` | `k`      | `Lowercase A`         |
|  `75` | `64+11`    | `113` | `01001011` | `K`      | `Uppercse A`          |
|  `11` | `11`       | `013` | `00001011` | `VT`     | `Vertical Tab`        |
| `100` | `64+32+4`  | `144` | `01100100` | `d`      | `Lowercase D`         |
|  `68` | `64+4`     | `104` | `01000100` | `D`      | `Uppercase D`         |
|   `4` | `4`        | `004` | `00000100` | `EOT`    | `End of Transmission` |
| `123` | `64+32+27` | `173` | `01111011` | `{`      | `Curly Left Bracket`  |
|  `91` | `64+27`    | `133` | `01011011` | `[`      | `Square Left Bracket` |
|  `27` | `27`       | `033` | `00011011` | `ESC`    | `Escape`              |

Pro-trick: `<C-[>` is the same as `<Esc>`

---

| prev: [Mult Neovim Windows][7] | [Home][0] | next: [Regular Expressions][9] |

[7]: 07-MultipleWindows.md
[0]: ../README.md
[9]: 09-RegularExpressions.md
