# syntax

Syntax highlighting for [Quirk](https://github.com/AlexVachon/quirk) programs —
editors, viewers, anything that colours code. Written in Quirk.

```quirk
use syntax

lang := syntax.for_file("main.qk")            // Quirk; anything else: plain text
hl := syntax.Highlighter(lang)
theme := syntax.dark()

for t in hl.tokens(lines, 12) {               // line 12's tokens
    tok: syntax.Token = t
    color := theme.color(tok.kind)            // "#c586c0"
    // draw lines[12].substring(tok.start, tok.end) in that colour
}

// After an edit on line 12 (typing, a line inserted or removed):
hl.changed(12)
```

- **A line at a time.** `lang.tokenize(line, state)` gives the line's `Token`s
  (`start`, `end`, `kind`) and the state the next line starts in — inside a
  `--- docstring ---`, a `"""triple-quoted string"""`, … — so a file is coloured
  line by line.
- **`Highlighter`** keeps each line's tokens; `changed(first_line)` forgets that
  line and the ones below, which are worked out again when asked for. About
  10 µs a line.
- **Kinds:** `keyword`, `constant`, `type`, `function`, `string`, `escape`,
  `interpolation`, `number`, `comment`, `doc`, `operator`, `punctuation`,
  `decorator`, `plain`.
- **Quirk:** keywords, `true` / `false` / `null`, types (capitalised names),
  functions (a name before `(`, or after `define` / `fn`), strings — `"…"`,
  `'…'`, `"""…"""`, raw `r"…"` (no escapes or `$`) — with `\n` escapes and
  `${…}` / `$name` interpolation, `// comments`, `--- docstrings ---`.
- **Themes:** `syntax.dark()`, `syntax.light()` — a `"#rrggbb"` for each kind,
  plus the editor's own (`background`, `gutter`, `line`, `selection`, `cursor`).
- **Another language:** a struct extending `syntax.Language`, with its own
  `tokenize(line, state)`.

## License

MIT
