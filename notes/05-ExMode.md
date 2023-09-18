# Ex mode commands

The name vi comes from the "visual interface" for the ex line editor.
That is the reason vi's configuration file is called `~/.exrc`. When vi
is launched via an ex sym-linked or hard linked name, it runs in Ex mode.

The ex editor is the Berkeley Unix replacement for the AT&T System V ed
editor. It is a line editor geared to teletype and DECwriter paper
terminals.

Vi/Vim *command mode* was based on *ex mode* commands.

## Why use ex mode?

* Your console, terminal, ssh, or TMUX session is all messed up,
  * and you are trying to fix this,
  * or you want to do a quick edit on some server & be done with it.
* You want to script multiple bulk edits on multiple files and either
  * your Sed and AWK skills are a bit rusty, or
  * you need something more "stateful" between edits and/or files.
* You accidentally enter *ex mode* and you want to get out of it.

## Entering ex mode

Just giving a useful, but incomplete, subset of the possibilities.

### Vi 

* From vi *normal mode* `Q`
  * switch back to visual mode via `vi` command
* From terminal `ex fileToEdit1 fileToEdit2 fileToEdit3`
  * switch to visual mode via `vi` command
* From terminal `vi -e fileToEdit1 fileToEdit2 fileToEdit3`
  * switch to visual mode via `vi` command
* From terminal enter `vi -es filename` or `ex -s filename`
  * command mode commands taken from stdin
    * interactively edit
    * pipe in *command mode* commands from stdin
    * heredocs
  * `$EXINIT` and `.exrc` files are not processed
  * does not start a UI
  * cannot switch to visual mode

### Neovim
* From nvim *normal mode* `gQ`
  * switch back to visual mode via `vi` command
* From terminal `nvim -e fileToEdit1 fileToEdit2 fileToEdit3`
  * command mode commands taken from stdin
    * interactively edit
    * pipe in *command mode* commands from stdin
    * heredocs
  * does not play nice with some plugins
    * Launch `nvim --clean -e fileToEdit1 fileToEdit2 fileToEdit3`
  * switch to visual mode via `vi` command
* From terminal non-interactively
  * from terminal `cat myScript | nvim -es fileToEdit`
  * does not start a UI
  * cannot switch to visual mode
  * session ends at end of script, `q` not necessary


## Neovim Examples

Piping commands in thru stdin

```bash
   $ echo '10,20p
   n
   10,15p
   q' | nvim -es .bashrc .bash_profile
```

Using a heredoc, note: real tabs

```bash
   $ nvim -es .bashrc <<-EOF
   >	10,20p
   >	1,5p
   >	q
   > EOF
```

Create a non-interactive script interactively,
then run the script non-interactively.

```bash
   $ nvim --clean -w myscript -e .bashrc
   $ cat myscript | nvim -es .bashrc
```

---

| prev: [Adv Trad Vi Commands][4] | [Home][0] | next: [Vim Specific Features][6] |

[4]: 04-AdvTradViCommands.md
[0]: ../README.md
[6]: 06-VimSpecificFeatures.md
[50]: http://www.lagmonster.org/docs/vi2.html
