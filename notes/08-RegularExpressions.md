# Regular Expressions

Regular expressions, also called RegExps, are used in
Unix/Posix like environments to match textual patterns.
They are not to be confused with shell globbing pattern matching.

## History

The concept of regular expressions arose in the 1950s from
the work of mathematitian/logician Stephen Kleen in his study of
[regular languages](https://en.wikipedia.org/wiki/Regular_language).
A regular language, also called a Type-3 grammar, is at the
bottom of what is called the
[Chomsky Hierarchy](https://en.wikipedia.org/wiki/Chomsky_hierarchy).
Regular languages have the property that they can be recognized
by a
[Finite State Machine](https://en.wikipedia.org/wiki/Finite-state_machine).
This property has nice computational space and time implications when using
RegExps to search for text patterns.

Before he invented Unix, computer engineer Ken Thompson built Kleen's
notation for regular expressions into the editor QED.  The editor used a RegExp
to construct a finite state machine, in machine code, to search for the
text patterns represented by the RegExp.  This was an early example
of just-in-time complication!  He later added this capability to the
Unix text editor ed.

From ed, vi inherited them.  They are used in many UNIX based
utilities; lex, sed, AWK, Emacs, Perl, Vim, and Ruby have built in
regular expression support.  Many computer languages, like Python and
C/C++, have standard library support for them.  Note that many of these
"regular expression" implementations contain features that cannot be
described in the sense of the formal language theory concept of a regular
grammar.

### Regular Languages (TL;DR)

A **formal language** `L` over an alphabet `Σ` is a subset of `Σ*`,
where `Σ*` is the set of *words* over `Σ`.

By **word** I mean a juxtaposition of symbols from an alphabet.

Let `A` and `B` be collections of words,

```math
    A ∪ B = { w ∥ w ∈ A or w ∈ b }
    A⋅B = { ab ∥ a ∈ A and b ∈ B }
```

Given a set `V` of words, let `ε` be the **empty word** , then

```math
    V₀   = {ε}
    V₁   =  V

    Vᵢ₊₁ = { wv ∥ w ∈ Vᵢ and v ∈ V }

    V* = V₀ ∪ V₁ ∪ V₂ ∪ V₃ ∪ …
    V+ = V₁ ∪ V₂ ∪ V₃ ∪ V₄ ∪ …
```

The collection of **regular languages** over an alphabet `Σ` is recursively
defined by

* languages `{ }` and `{ε}` are regular languages
* `∀a ∈ Σ`, `{a}` is a regular language
* if `A` and `B` are regular languages, then
  * `A∪B`is a regular language
  * `A⋅B`is a regular language
  * `A*` is a regular language
* no other languages over `Σ` are regular

From *formal language theory* it can be shown that a *regular language* is
a formal language which can be expressed using a *regular expression*.  Vim uses
a regular expression pattern to search the text document for strings that match
the pattern (are contained in the formal language defined by the RE).  It does
this via "compiling" the RE down to a finite state machine which scans the
documents for strings contained in the RE's formal language.

## Extended Regular Expressions(ERE)

Regular expressions (REs) are patterns used to match strings.  These
days, "strings" means a data structure representing an ordered sequence
of Unicode code points.  We'll assume we are using a "string-based" regular
expression engine.

* metacharacters:`{}[]()^$.|*+?\`
* any other char (unless modified by a meta-char) matches itself
* a meta-char's meaning can change in the context of another meta-char
* `^` matches from beginning of string, if at start of RE, else match itself
* `$` matches from end of string, if at end of RE, else match itself
* `.` matches any single character one time
* `\` escapes next character
  * turns off meta-meaning if next char is a meta-char
  * can give a meta-meaning to next char
  * in either case `\` itself is ignored (not part of the match)
* `[ ]` matches any char enclosed between brackets
  * use `^` as first char to match any char not included in rest
  * match `^` as itself in any position but first
  * use `-` to indicate a range of characters to match
  * match `-` as itself if first or last enclosed character
* `( )` define subexpression
  * groups concatenated RegExps together as single unit
  * reference subexpression later in RE via `\n` where `n` in range `1`-`9`
  * multiple groupings hierarchical, ordered by left paren

Let `S` and `T` represent regular expressions

* `ST` concats `S` and `T`
* `(ST)` concat `S`and `T` but treat as single subexpression
* `S|T` matches either `S` or `T`
* `S?` matches `0` or `1` of `S`
* `S*` matches `0` or more instances of `S`
* `S+` matches `1` or more instances of `S`
* `S{n,m}` matches at least `n`but not more than `m` of `S`
* `S{n,}` matches at least `n` of `S`
* `S{,m}` matches not more than `m` of `S`
* `S{m}` matches exactly `m` of `S`

Note, `*`, `+`, `?`, and `{m,n}` all bind more closely than concatenation.

## Basic Regular Expressions(BRE)

These are what Vim uses.  The big difference is that the meta
characters `(){}|+?` are treated litterally and you must
escape them with `\` for them to take on their meta-meaning.

* BRE backward compatible to Simple Regular Expressions(SRE)
* SRE are deprecated
* ERE extend SRE
* grep uses BRE; egrep uses ERE

Due to the common use of `(){}|+` in programming languages, makes
sense that vim uses BREs.  Probably more likely done for backward
compatibility with vi.

## Extended Regexp Examples

It is usually easiest to learn regular expressions using simple examples.

| RegExp            | Description                                            |
|:----------------- |:------------------------------------------------------ |
| `foo.*`           | match `foo` followed by zero or more characters        |
| `fooba+r`         | match `foobar`, `foobaar`, `foobaaar`, ...             |
| `foo(bar\|baz)`   | match either `foobar` or `foobaz`                      |
| `fo{2,4}bar`      | match `foobar`, `fooobar`, or `foooobar`               |
| `(fo){2,}bar`     | match `fofobar`, `fofofobar`, `fofofobar`, ...         |
| `(fo){2}bar`      | match `fofobar`                                        |
| `(fo){,3}bar`     | match `bar`, `fobar`, `fofobar`, or `fofofobar`        |
| `^foobar`         | match `foobar` at beginning of a line                  |
| `foobar$`         | match `foobar` at end of a line                        |
| `^foo(bar\|baz)$` | match line containing only`foobar` or `foobaz`         |
| `fooba[rz]`       | match `foobar` or `foobaz`                             |
| `foob[^ui]r`      | matches `fobar` or `fobqz` but not `fobur` nor `fobir` |

## Using Regular Expressions in Vim

I like to think of all my regular expressions as extended regular
expressions.  When working with basic regular expressions in Vim, I
still think in terms of extended regular expressions but with the
need to escape the `(){|+?` characters with a backslash to turn on
their meta-meaning.  The characters `[].` are meta without escaping.
The character sequences `}` or `\}` will match a matching meta `\{`,
otherwise they are taken as a literal `}`.

Neovim recommends keeping the default `magic` setting.
See `:help magic`

### Examples of Vim Regular Expressions

Find next `buf` or `bug`

```vim
    /bu[fg]
```

Find next `buf` or `bug`

```vim
    /bu\(f\|g\)
```

Find next `buf`, `bug`, `bufg`, `bugf`, `buff`, and `bugg`

```vim
    /bu[fg]\{2,1}
```

Replace "Unix" & "UNIX" with "Linux" on lines 5 thru end of file

```vim
    :5,$s/U\(nix\|NIX\)/Linux/g
```

For more examples, see `:help usr_27.txt`

### Searching via patterns

These all start or stay in *normal mode*.  They end in *normal mode*.
In what follows, a regular expression pattern is denoted `{regex}`.

See `:help pattern-searches` for more details.

| Searches           | Description                                            |
|:------------------ |:------------------------------------------------------ |
| `/{regex}<CR>`     | Search forwards for `{regex}`                          |
| `/{regex}/3<CR>`   | Search forwards 3 lines past `{regex}`                 |
| `/{regex}/-5<CR>`  | Search forwards stop 5 lines before `{regex}`          |
| `?{regex}<CR>`     | Search backwards for `{regex}`                         |
| `/<CR>`            | Repeat last search forwards                            |
| `/10<CR>`          | Search forwards 10 lines after next match              |
| `?<CR>`            | Repeat last search backwards                           |
| `n`                | Repeat last search in same direction as last search    |
| `N`                | Repeat last search in oposite direction as last search |
| `*`                | Search forward for keyword/word under/near cursor      |
| `#`                | Search backwards for keyword/word under/near cursor    |
| `g*`               | Same as `*` but not restricted to whole word matches   |
| `g#`               | Same as `#` but not restricted to whole word matches   |
| `gd`               | Go to (best guess) of local declaration                |
| `gD`               | Same as `gd` except always start on search at line 1   |
| `/dogbert/e`       | Search for dogbert, leave cursor at end                |
| `/dogbert/e5`      | Search for dogbert, leave cursor 5 chars after end     |
| `/dogbert/e-2`     | Search for dogbert, leave cursor 2 chars before end    |
| `/dogbert/b2`      | Search for dogbert, leave cursor on the `g`            |
| `/dogbert/b-3`     | Search for dogbert, leave cursor 3 chars before `d`    |

## POSIX.2 Regular Expressions

For a description of POSIX.2 regular expressions see

```fish
    $ man -s7 regex
    $
```

According to this Linux man page,
POSIX.2 refers to "*extended regular expresions*" as both
"*modern regular expresions*" and "*egrep regular expressions*."
It also refers to "*simple regular expressions*" as
"*obsolete regular expressions*" and "*basic regular expressions*"
as "*ed regular expressions*."

For "*simple regular expressions*" the characters `|+?` have no
special meta-meaning.  This type of regular expressions are in the
POSIX.2 standard for backward compatibility, but are considered a wart.

---

| prev: [Encodings and Unicode][7] | [Home][0] | next: [Plugins][9] |

[7]: 07-EncodingsUnicode.md
[0]: ../README.md
[9]: 09-Plugins.md
