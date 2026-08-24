# Fieldmouse

Fieldmouse is a small language for the Node-style build-script work that should not require all of Node. Its active implementation is written in **Edriç**.

The repository started from MuJS. That C implementation remains recoverable from Git history; the active Edriç implementation is now on `master`.

## Language contract

The first syntax contract is deliberately small:

- `name ← value` assigns leftward;
- `value → name` assigns rightward;
- `=` compares with primitive coercion and is never assignment;
- `≠` is inequality;
- `≟` is strict, non-coercive equality;
- `Ø` is false.

Declaration families, full `let` / `const` scope behavior, the source-file extension, and any ordinary-JavaScript compatibility mode remain provisional. Tests do not silently settle those questions.

## Current slice

The Edriç interpreter currently has:

- primitive dynamic values represented by Edriç `choice` declarations;
- a lexer for identifiers, decimal numbers, strings, comments, and operators;
- precedence parsing for assignment, Boolean, comparison, and arithmetic expressions;
- provisional `var`, `let`, and `const` bindings;
- blocks, `if` / `else`, and `while`;
- `console.log(...)`;
- execution from `-e` or a source file.

This is intentionally smaller than ECMAScript and much smaller than Node. Arrays and objects, functions, property access and calls, then the small `fs` / `path` / `process` surface required by real build scripts remain to be implemented.

## Build and test

CI builds with both the reproducible pinned Edriç revision and current Idriç `main`.

With either compiler available:

```text
idris2 --build fieldmouse.ipkg
idris2 --build tests.ipkg
build/exec/fieldmouse-tests
```

Then:

```text
build/exec/fieldmouse -e 'var answer ← 6 * 7; console.log(answer);'
build/exec/fieldmouse script
```

Parse errors, runtime errors, unreadable files, and invalid command lines return a failing process status.

## Layout

The active code is intentionally flat:

- `Fieldmouse.idric` — language model, lexer, parser, and interpreter;
- `Main.idric` — command-line entry point;
- `Tests.idric` — executable language-contract tests;
- `fieldmouse.ipkg` and `tests.ipkg` — application and test builds.

The only directory retained is `.github`, for automated builds.

## License

The original MuJS-derived code and notices are preserved in repository history and `COPYING`.
