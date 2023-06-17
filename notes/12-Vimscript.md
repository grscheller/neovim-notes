# Vimscript

Vimscript, also sometimes referred to in the Neovim documentation as
VimL, is Vim's built in scripting language. Basically a super set of ex
commands with some scripting language constructs thrown in.

I have no desire to become an expert in Vimscript, but a certain amount
of knowledge is necessary when mastering Neovim. Neovim developers have
stated they have no intention in abandoning Vimscript support.

## Ex commands

Command mode is not a good VimL REPL. It is only really good for one
line ex commands. Command mode can be very quirky when given Vimscript.
For example, "for loops" begin executing before you are finished typing
them.

### set command

---

## VimL Scripting

### Calling Lua from Vimscript

Use a VimL here document to run Lua code from Vimscript.

```vim
   lua << EOF
   local rust_opts = {
       tools = {
           autoSetHints = true,
           hover_with_actions = true,
           inlay_hints = {
               show_parameter_hints = false,
               parameter_hints_prefix = "",
               other_hints_prefix = ""
           }
       }
   }
   require('rust-tools').setup(rust_opts)
   EOF
```

Note: Each "Lua chunk" defined this way is in its own Lua namespace.
Local lua variables are local to each chenk. Global Lua variables are
shared between different chunks.

---

| prev: [Lua][11] | [Home][0] | next: [Neovim Configuring & Maintenance][13] |

[11]: 11-Lua.md
[0]: ../README.md
[13]: 13-Configuration.md
