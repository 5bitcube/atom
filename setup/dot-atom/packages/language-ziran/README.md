# language-ziran

Ziran (`.zi`) syntax highlighting for Atom, ported from the
[ziran-vscode](https://github.com/ziranlang/ziran-vscode) extension
(version 0.6.3).

- `grammars/ziran.json` is the extension's `syntaxes/ziran.tmLanguage.json`
  with Atom's `fileTypes` added. The scope name is the same, `source.ziran`,
  so any syntax theme colours it the same way it colours VS Code's tokens:
  directives (`#import`, `#foreign`, ...), line and block comments, strings,
  declarations (`Name :: (...)` as functions, `Name :: value` as constants),
  scalar types, `struct`/`enum`, capitalised type names, numbers, keywords
  and operators.
- `settings/language-ziran.cson` carries the editor rules from
  `language-configuration.json`: `//` line comments (Ctrl+/) and brace-based
  auto-indent. Bracket and quote auto-closing is Atom's default `bracket-matcher`
  behaviour.

The extension's other features (`ziran check` diagnostics, the Run pane,
the `.zir` viewer, Kryon preview) are VS Code extension code and are not
part of this grammar package.

Installed as a local package in `~/.atom/packages/language-ziran`; reload
Atom (Ctrl+Shift+F5) or restart it after changes.
