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

## VimL Scripting

### Calling Lua from Vimscript

Use a VimL here document to run Lua ccode from Vimscript.

```
   Lua << EOF
   --[[ Rust configuration, rust-tools.nvim will call lspconfig itself ]]
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

---

| prev: [Plugins][1] | [Home][2] | next: [Lua][3] |

[1]: 09-Plugins.md
[2]: ../README.md
[3]: 11-Lua.md
