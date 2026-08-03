# Neovim Configuration

The only two plugin managers I wish to touch on are:

- Lazy Nvim package manager: `folke/lazy.nvim`
- Neovim's native built-in plugin manager: `vim.pack`

With lazy.nvim, load order is something the plugin manager computes
for you from declarative spec fields.

With vim.pack, load order is just Lua execution order plus whatever
native autocmds you wire up by hand. The plugin manager itself has
no opinion on timing beyond "now" vs. some autocmd you write wrapping
the `:packadd` command.

## Lazy Nvim Plugin manager (folke/lazy.nvim)

The most popular, and currently (as of Aug 2026) the default standard.

The [lazy nvim][20] package manager is the most popular
plugin manager as of August 2026. It is almost a default
standard. This is not to be confused with the [LazyVim][21]
Neovim distribution.

### Startup event order

Let's assume require("lazy").setup(...) is called early in init.lua,
the standard setup, and we set `Lazy = False,`, the recommended default,
in lazy.nvim's opts table.

1. init.lua begins executing — up through the require("lazy").setup(...) call
2. lazy = false plugins load synchronously, during the setup() call itself.
   Load order among them follows priority (higher first) and dependency
   resolution.
3. Rest of init.lua executes, anything after lazy.nvim's setup() call.
4. BufNewFile/BufReadPre/BufReadPost fire for any file arguments passed
   on the command line. Any plugin with these events configured in
   lazy-specs is loaded here.
5. BufEnter fires for the initial buffer.
6. VimEnter fires — all startup stuff (vimrc/init.lua sourced, -c cmd args
   run, windows created, buffers loaded) is done. All event = "VimEnter"
   autocmds/lazy-specs trigger and run to completion.
7. UIEnter fires, once the UI (TUI/GUI) has attached — after VimEnter.
8. vim.schedule() (queued by lazy.nvim inside its VimEnter handler) runs on
   the next event-loop tick, firing the VeryLazy User autocmd.

Anything gated behind event = "VeryLazy" is guaranteed to load after all
lazy = false plugins. VeryLazy is lazy.nvim's recommended catch-all for
"load this shortly after startup, but don't block VimEnter."

## Native Neovim plugin manager: vim.pack

Install, update, and delete external plugins.

Note: vim.pack is still considered experimental, yet should
be stable enough for daily use.

### Startup event order

1. init.lua begins executing.
2. Any vim.pack.add() calls that live directly in init.lua's own body run
   synchronously and immediately, in the exact order written — install
   (if missing) then :packadd (or a custom `load` function) happens right
   there, blocking. There is no priority system and no dependency
   resolution: if plugin B needs plugin A, you must vim.pack.add(A) before
   vim.pack.add(B) yourself.
3. The rest of init.lua executes — any config/setup() calls after those
   vim.pack.add() calls.
4. The plugin/ directory runtime files are sourced automatically, in
   alphabetical order, across 'runtimepath'. This is native Neovim
   startup behavior, not something vim.pack provides — but it's the
   idiomatic way to split a vim.pack config across files (the analogue
   of lazy.nvim's auto-imported lua/plugins/ directory). Any
   vim.pack.add() calls inside these files execute here, in filename
   order — hence patterns like 00-mini-hues.lua to force a plugin
   (e.g. a colorscheme) to load first.
5. BufNewFile/BufReadPre/BufReadPost fire for any file arguments passed
   on the command line. (Unaffected by plugin-manager choice — same as
   with lazy.nvim.)
6. BufEnter fires for the initial buffer.
7. VimEnter fires — all startup stuff done.
8. UIEnter fires, after VimEnter.
9. No built-in deferred-load event exists. To replicate lazy.nvim's
   VeryLazy behavior, you write your own:
     vim.schedule(function() vim.pack.add({...}) end)
   placed in init.lua. This fires on the next event-loop tick after the
   calling context finishes — if scheduled during init.lua's initial
   run, that lands shortly after VimEnter/UIEnter, similarly to
   VeryLazy, but it's config you authored, not something the plugin
   manager triggers for you.
10. Any further "lazy loading" beyond that is just ordinary user-defined
    autocmds — InsertEnter, CmdlineEnter, FileType, etc. — each calling
    vim.pack.add() inside its callback, firing whenever that native
    event next occurs. The only vim.pack-specific events at all are
    PackChangedPre/PackChanged, and those relate to install/update/
    delete state changes, not load timing.

---

| prev: [Configuration][11] | [Home][0] | [FinalNotes][13] |

[0]: ../README.md
[11]: 11-Configuration.md
[13]: 13-FinalNotes.md
[20]: https://github.com/folke/lazy.nvim
[21]: https://github.com/LazyVim/LazyVim
