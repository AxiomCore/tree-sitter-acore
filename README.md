# Tree-sitter Acore

Syntax grammar for Acore configuration, backend, frontend and database sources.
The parser supplies editor structure; the native compiler remains responsible
for types, diagnostics and semantic meaning. Only `.acore` is registered.

This grammar extends the vendored Apple Pkl grammar at
`c837eff683d62f3cb5e6309b44c257640f202d4b`. See [NOTICE](NOTICE),
[LICENSE](LICENSE) and [upstream provenance](vendor/pkl/UPSTREAM.json).

## Generate and verify

Use Tree-sitter CLI 0.25.4. Generated parser files are checked in.

```sh
tree-sitter generate
tree-sitter test
python3 ../acore-zed/scripts/check-grammar.py
python3 ../acore-zed/scripts/check-grammar.py --examples ../axiom-frontend/apps/playground/src/examples
```

The golden corpus checks all four starter profiles plus imports, composition,
inheritance, interpolation, lambdas, qualified overrides, named arguments,
lists, records, units and nested styles. Review expected trees when grammar
behavior changes; `tree-sitter test --update` alone is not verification.

The adapter's companion check validates six Zed queries, expected outline and
highlight captures, bracket/indent/text-object captures, CSS injection and
unfinished-string/body recovery. Incremental tests delete and repair closing
delimiters after multibyte Unicode with CRLF, then compare the result to a fresh
parse. The optional playground path is local validation input, not a packaged
dependency.

After verification, commit the grammar revision locally. The adapter's
`scripts/configure-dev.py` pins that exact clean revision into its ignored
development manifest. Zed compiles the grammar to WebAssembly during dev
installation or `zed: rebuild dev extension`.

Public source releases are published independently of the native compiler.
Pin the exact tested grammar commit in the adapter manifest; a grammar push alone
does not update an installed Zed extension. The adapter's maintenance guide
describes versioning and registry updates.
