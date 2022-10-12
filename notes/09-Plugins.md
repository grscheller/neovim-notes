# Plugins

## Overview

### Traditionally

### Packages

## Popular Plugins

### Vim Commentary (tpope/vim-commentary)

Comment out sections of code quickly.

| mode   | Command      | Description      |
|:------ |:------------:|:---------------- |
| normal | `gc<motion>` | Comment out code |
| visual | `gc`         | Comment out code |

### Vim Sneak (justinmk/vim-sneak)

Two character, multiline version of `f` & `t` allowing
you to navigate quickly through a Neovim window.  Highlights
other matches in window.  Navigate to next or previous
instances with `;` or `,`.  Positions moved through by
`;` and `,` are not recorded on the vim jumplist.

| mode    | Command           | Description                                   |
|:------  |:-----------------:|:--------------------------------------------- |
| normal  | `s<char1><char2>` | Jump forward to `<char1><char2>`              |
| normal  | `S<char1><char2>` | Jump backward to `<char1><char2>`             |
| normal  | `;`               | Jump to next instance of `<char1><char2>`     |
| normal  | `,`               | Jump to prev instance of `<char1><char2>`     |
| normal  | `f<char>`         | One char version of `s` (`f` replacement)     |
| normal  | `t<char>`         | One char version of `s` (`t` replacement)     |
| normal  | `F<char>`         | One char version of `S` (`F` replacement)     |
| normal  | `T<char>`         | One char version of `S` (`T` replacement)     |
| visual  | `z<char1><char2>` | Extend selection forward to `<char1><char2> ` |
| visual  | `Z<char1><char2>` | Extend selection back to `<char1><char2>`     |
| visual  | `f<char>`         | `f` replacement                               |
| visual  | `t<char>`         | `t` replacement                               |
| visual  | `F<char>`         | `F` replacement                               |
| visual  | `T<char>`         | `T` replacement                               |
| op pend | `z<char1><char2>` | Perform on motion forward to `<char1><char2>` |
| op pend | `Z<char1><char2>` | Perform on motion back to `<char1><char2>`    |
| op pend | `f<char>`         | `f` replacement                               |
| op pend | `t<char>`         | `t` replacement                               |
| op pend | `F<char>`         | `F` replacement                               |
| op pend | `T<char>`         | `T` replacement                               |

`z` and `Z` are used above because Vim Surround had taken `s` and `S`.

To use the `f`,`t`,`F`,`T` key mappings, you will need to manually
configure them

```viml
    map f <Plug>Sneak_f
    map F <Plug>Sneak_F
    map t <Plug>Sneak_t
    map T <Plug>Sneak_T
```

There is alsoa "label mode" which makes Vim Sneak more
like easymotion/vim-easymotion.  Not availble for `f`,`t`,`F`,`T`.

### Vim Surround (tpope/vim-surround)

Manipulate matching surrounding symbols.  Due to the limitations of
Markdown, I will sometimes need to use a period `.` to indicate a space.

| mode   | Command  | Description/Example                                         |
|:------ |:--------:|:----------------------------------------------------------- |
| normal | `ds"`    | `"foo bar"` -> `foo bar`                                    |
| normal | `dst`    | delete surrounding HTML tags (has to be an html file)       |
| normal | `ds(`    | `foo({ foo = bar, n = 42 })` -> `foo{ foo = bar, n = 42 }`  |
| normal | `cs(.`   | `foo({ foo = bar, n = 42 })` -> `foo { foo = bar, n = 42 }` |
| normal | `cs'[`   | `'hello 42'` -> `[ hello 42 ]`                              |
| normal | `cs']`   | `'hello 42'` -> `[hello 42]`                                |
| normal | `cst<p>` | `<title>My homepage</title>` -> `<p>My homepage</p>`        |
| normal | `ysiw(`  | `hello world` -> `hello ( world )` <= cursor was on the 'r' |
| normal | `yss{`   | `..printf("Hi there\n")` -> `..{ printf("Hi there\n") }`    |
| normal | `ySS{`   | `..printf("Hi there\n")` -> `..{`                           |
|        |          | `......................` -> `......printf("Hi there\n")`    |
|        |          | `......................` -> `..}`                           |
| visual | `S"`     | wrap visual selection in `"`,  result varies by submode     |
| visual | `S}`     | wrap visual selection in `{}`, result varies by submode     |

Due to the limitations of Markdown, I will need to represent the
location of the cursor with `$` instead of the more logical choice `|`.

| mode    | Typed             | Result           |
|:------  |:------------------|----------------- |
| insert  | `foo = bar<C-s>(` | `foo = bar( $ )` |
| insert  | `foo<C-g>s(`      | `foo( $ )`       |
| insert  | `do <C-g>S(`      | `do { `          |
|         |                   | `....$`          |
|         |                   | `}    `          |

### Vim Repeat (tpope/vim-repeat)

* Allows opted in plugins to repeat actions via `.`
* Allows `u` and `<C-r>` to behave as expected for opted in plugins

---

| prev: [Regular Expressions][1] | [Home][2] | next: [Vimscript][3] |

[1]: 08-RegularExpressions.md
[2]: ../README.md
[3]: 10-Lua.md
