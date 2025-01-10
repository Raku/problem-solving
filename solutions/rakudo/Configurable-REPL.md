# Configurable REPL Prompt

The default REPL for Rakudo is not configurable.

# Proposed Solution

Implement support for two environment variables:

| Variable | Description | Default |
|--|--|--|
| `RAKUDO_REPL_PROMPT` | the main prompt | `[\i] > ` |
| `RAKUDO_REPL_PROMPT2` | the continuation prompt | `* ` |

Each which supports the following interpolations:

| Sequence | Prompt Expansion |
|--|--|
| `\a` | alert / bell character |
| `\c` | color - see below for options |
| `\e` | escape character |
| `\i` | index - the current iteration of the REPL, which can be referred to later as, e.g. `$*1` |
| `\l` | `$*RAKU.version` |
| `\L` | `$*RAKU.gist` |
| `\n` | newline |
| `\t` | time - see below for options |
| `\v` | `$*RAKU.compiler.version` |
| `\V` | `$*RAKU.gist` |

## \c formatting

Provide some common ANSI codes with short names. Defaults to `reset` if a bare `\c` is used,
and multiple arguments can be separated with a `;`, e.g.

```
\c{bold;red}
```

### formatting

> reset, normal, bold, dim, italic, underline, blink, inverse, hiddne, strikethrough

### foreground colors

> black, red, green, yellow, blue, magenta, cyan, white, default

### background colors

> bg:black, bg:red, bg:green, bg:yellow, bg:blue, bg:magenta, bg:cyan, bg:white, bg:default

Not all escape sequences have aliases - you can always use 
`\e`  to manually generate codes.

## \t formatting

The `\t` construct takes an optional `{}` containing a subset of `strftime` codes,
defaulting to `%T` if a bare `\t` is used.

| Code | Value |
|--|--|
| `%T` | `%H:%M:%S` |
| `%R` | `%H:%M` |
| `%H` | 24-hour hours, 2 digits |
| `%I` | 12-hour hours, 2 digits |
| `%M` | minutes, 2 digits |
| `%S` | seconds, 2 digits |

```
RAKUDO_REPL_PROMPT='\t{%R} > '
```

Would generate a prompt containing, e.g. `06:18 > `

# Future Development

* More strftime support - perhaps even invoking it directly instead of manually mapping.
* Support `RAKUDO_REPL_COMMAND` to support arbitrary code invocation



