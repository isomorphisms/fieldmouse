# Fieldmouse

Fieldmouse is a small JavaScript interpreter being rewritten in **Edriç**.

The repository started from MuJS. The old C implementation remains recoverable from Git history; the active implementation is the small Edriç core visible in the repository now.

## Current slice

The Edriç interpreter currently has:

- JavaScript values represented by Edriç `choice` declarations;
- a lexer for identifiers, decimal numbers, strings, comments, and common operators;
- precedence parsing for assignment, Boolean, comparison, and arithmetic expressions;
- `var`, `let`, and `const` bindings;
- blocks, `if` / `else`, and `while`;
- `console.log(...)`;
- native text-file calls: `readText(path)`, `writeText(path, text)`,
  `appendText(path, text)`, and `fileExists(path)`;
- execution from `-e` or a `.js` file.

The file calls are deliberately a Fieldmouse surface, not a claim of Node compatibility. They operate on paths relative to the current working directory, do not create parent directories, and report read/write failures as interpreter errors with a nonzero exit status. `writeText` truncates or creates a file; `appendText` preserves existing contents or creates a file; both return `undefined`.

Fieldmouse remains intentionally smaller than ECMAScript and much smaller than Node. Arrays and objects, user-defined functions, property access, `fs`, `path`, and `process` remain separate work. Exact standards compliance is not the design constraint.

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
