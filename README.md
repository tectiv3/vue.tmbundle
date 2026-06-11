# Vue.tmbundle

TextMate 2 bundle for Vue single-file components (`*.vue`, scope `text.html.vue`).

The grammar derives from [vuejs/vue-syntax-highlight](https://github.com/vuejs/vue-syntax-highlight)
(Sublime Text). That project [declined to carry TextMate support](https://github.com/vuejs/vue-syntax-highlight/pull/197)
and recommended maintaining it in a separate repository — this is that repository.

## Fixes over the original grammar

- `<script lang="ts">` rule was dead (missing `begin`/`end` patterns); it now matches and
  falls back to `source.js` when no TypeScript grammar is installed.
- Slot shorthand (`#title`) and dynamic directive arguments (`:[prop]`, `@[event]`) are
  recognized as directives.
- Valueless directives (`v-else`, `#default`) are scoped as attribute names.
- Corrected backreferences in the `lang=` detection lookaheads (`\1` pointed at the tag
  bracket instead of the quote character).

## Install

```sh
mkdir -p ~/Library/Application\ Support/TextMate/Bundles
cd ~/Library/Application\ Support/TextMate/Bundles
git clone https://github.com/tectiv3/vue.tmbundle.git Vue.tmbundle
```

Then reload bundles in TextMate (or relaunch).

## Known issue: `??` breaks highlighting with the stock JavaScript bundle

This grammar embeds `source.js` for `<script>` sections and `{{ … }}` interpolations.
The stock [textmate/javascript.tmbundle](https://github.com/textmate/javascript.tmbundle)
grammar treats every `?` as the start of a ternary expression and waits for a `:` to close
it, so nullish coalescing (`??`) opens a scope that never closes and corrupts highlighting
for the rest of the file.

Until that is fixed upstream, edit the JavaScript grammar's ternary rule
(Bundles → Select Bundle Item… → JavaScript / Syntaxes):

- change `begin` from `\?` to `(?<!\?)\?(?![?.])`
- add operator rules ahead of it for `\?\?` (`keyword.operator.logical.js`) and
  `\?\.` (`keyword.operator.accessor.js`)

## License

MIT — see [LICENSE](LICENSE). Original grammar © Evan You (vue-syntax-highlight).
