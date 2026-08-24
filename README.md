# Fieldmouse

Fieldmouse is a small JavaScript interpreter being rewritten in **Edriç**.

The repository started from MuJS. The C implementation remains in Git history and on `master`; the `edric-rewrite` line deliberately does not preserve MuJS's `src` / `include` / `docs` / `tests` directory layout. The active implementation is meant to be visible when the repository opens.

## Current slice

The Edriç interpreter currently has:

- JavaScript values represented by Edriç `choice` declarations;
- a lexer for identifiers, decimal numbers, strings, comments, and common operators;
- precedence parsing for assignment, Boolean, comparison, and arithmetic expressions;
- `var`, `let`, and `const` bindings;
- blocks, `if` / `else`, and `while`;
- `console.log(...)`;
- execution from `-e` or a `.js` file.

This is intentionally smaller than ECMAScript and much smaller than Node. The next useful compatibility work is arrays and objects, functions, property access and calls, then the small `fs` / `path` / `process` surface that build scripts actually use. Exact standards compliance is not the design constraint.

## Build

Fieldmouse is pinned in CI to Edriç commit:

`61970be77769f607cca8650bf424c0f0b22ddee7`

With that compiler available:

```text
idris2 --build fieldmouse.ipkg
```

Then:

```text
build/exec/fieldmouse -e 'var x = 6 * 7; console.log(x);'
build/exec/fieldmouse script.js
```

## Layout

The active code is intentionally flat:

- `Fieldmouse.idric` — language model, lexer, parser, and interpreter;
- `Main.idric` — command-line entry point;
- `fieldmouse.ipkg` — build description.

The only directory retained is `.github`, for automated builds.

## License

The original MuJS-derived code and notices are preserved in repository history and `COPYING`.