# Final Notes

## My personal nvim configuration

My current nvim configuration can be found in my dotfiles repo here
[grscheller/dotfiles][21] on GitHub. I directly use [folke/lazy.nvim][22]
as my package manager. My `init.lua` bootstraps this plugin which then
manages my other plugins. It contains lots of good examples that can be
followed, and it is all in the public domain!

It is also highly personalized and opinionated.

## Neovim distributions

There are so called "neovim distributions" which will give you
a complete "out-of-the-box" LSP/DAP based IDE configuration.

See these well known examples.

* [LazyVim/LazyVim][41]
* [LunarVim/LunarVim][42]
* [NvChad/NvChad][43]

A problem arises when reverse engineering these Neovim distribution to
figure out how something is done. These configurations need to allow
user changes to override the distribution's default configuration. This
additional complexity can be confusing to a beginner. Many times there
are much simpler ways to accomplish what the distribution is doing since
there is no need to "override" anything.

## Final advise

I find looking at plugin documentation, other users' dotfiles, and,
alas, using AI to be the best ways to learn how to configure nvim.

Neovim distributions can be useful if you don't have the time to develop
your own configuration, but to truly master Neovim, and make it your
own, you need to spend the time to "grok" it. Creating your own
configuration helps a lot with this. Practicing editing text efficiently
helps too.

In answer to a Stack Overflow question titled
[What is your most productive shortcut with Vim?][41]
the top-rated answer (1124 up votes) was "Your problem with Vim is that
you don't grok vi." Likewise, you will be limited if all you do is
install one of these Neovim distributions and use it like you would
IntelliJ or vscode.

---

| prev: [PluginManagers][12] | [Home][0] |
[10]: 10-RegularExpressions.md
[0]: ../README.md
[12]: 12-PluginManagers.md
[21]: https://github.com/grscheller/dotfiles
[22]: https://github.com/folke/lazy.nvim
[31]: https://github.com/LazyVim/LazyVim
[32]: https://github.com/LunarVim/LunarVim
[33]: https://github.com/NvChad/NvChad
[41]: https://stackoverflow.com/questions/1218390/what-is-your-most-productive-shortcut-with-vim/1220118#1220118
