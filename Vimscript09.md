# Vimscript

Vimscript, also sometimes referred to in the Neovim documentation
as VimL, is Vim's built in scripting language.  Basically a super
set of Ex commands with some scripting language constructs thrown
in.

I have no desire to become an expert in Vimscript, but a certain
amount of knowledge is necessary when mastering Neovim.  Neovim
developers have stated they have no intention in abandoning
Vimscript support.

## Ex commands

Command mode should not be used as a VimL REPL.  As far as
I can tell, its best just to execute one line ex commands.
Command mode becomes very quirky when given Vimscript.  For example,
for loops begin executing before you finish typing them.

### set command

## Scripting constructs

## Calling Lua from Vimscript

---

| prev: [Regular Expressions][1] | [Home][2] | next: [Lua][3] |

[1]: RegularExpressions08.md
[2]: README.md
[3]: Lua10.md
