# Fieldmouse issues

The GitHub issue tracker is currently disabled for this repository, so this file is the working backlog for the Edriç rewrite.

## 1. Support JavaScript arrays and objects

The current interpreter handles primitive values, bindings, expressions, blocks, conditionals, loops, and `console.log`, but not arrays or ordinary objects.

Done when:
- array literals and indexed reads/writes work;
- object literals and named property reads/writes work;
- missing properties have one consistent JavaScript-like result;
- nested arrays/objects and mutation are tested;
- the implementation stays in Edriç rather than restoring the old MuJS C core.

## 2. Add functions, calls, and property access

Build scripts need user-defined functions and ordinary call/property syntax before Fieldmouse is useful beyond tiny expressions.

Done when:
- function declarations or expressions can be defined and called;
- parameters and return values work;
- lexical/local scope behavior is explicit and tested;
- `object.name`, `object[name]`, and method-call syntax work;
- recursion is not required merely to claim this slice complete.

## 3. Provide native text file I/O

The first useful file surface should not wait for objects, property calls, or a Node compatibility layer.

Surface:
- `readText(path)` returns the complete text;
- `writeText(path, text)` creates or truncates a file;
- `appendText(path, text)` creates or appends to a file;
- `fileExists(path)` returns a Boolean;
- I/O errors stop execution with an operation, path, and underlying error;
- tests exercise create, truncate, append, read, existence, invalid arguments, and missing-file failure.

This slice does not include directories, binary buffers, `fs`, `path`, `process`, or general function compatibility.

## 4. Implement the small Node build-script surface

Fieldmouse is not trying to become all of Node. Implement only the filesystem/process/path behavior that real project build scripts require.

Initial surface:
- `process.argv`, environment access, and exit status;
- basic `path` joining/normalization;
- basic `fs` reads, writes, existence checks, and directory operations;
- useful errors rather than silent fallbacks for unsupported operations.

Add features from real scripts, not from a Node compatibility checklist.

The native text calls above are useful independently, but do not satisfy this Node-facing backlog item.

## 5. Keep Fieldmouse building against current Edriç

Fieldmouse CI is pinned to an older Edriç commit while Catfood follows current Idriç/Edriç development. That creates a compiler-drift trap.

Done when:
- Fieldmouse is tested against the compiler Catfood actually installs;
- either the old pin is removed or the reason for retaining it is explicit;
- compiler incompatibilities fail in CI with a focused diagnostic;
- a compiler update cannot silently leave Fieldmouse unbuildable.

## 6. Make the Catfood installation contract green

Catfood should be able to feed Fieldmouse into a fresh environment and leave a runnable stable command behind.

Done when:
- Catfood clones the `edric-rewrite` line;
- the required Edriç compiler is available or bootstrapped;
- `fieldmouse.ipkg` builds;
- a small interpreter smoke test passes;
- `fieldmouse` is available from Catfood's stable `bin` directory.

Catfood PR #11 is the current integration attempt.

## 7. Build a real-world JavaScript fixture corpus

Tiny parser tests are not enough to tell whether Fieldmouse can replace Node for the build-script jobs we actually care about.

Add a fixture directory containing small, legal-to-copy scripts representative of real project work:
- argument parsing;
- filesystem traversal;
- JSON-ish data manipulation once objects/arrays exist;
- path construction;
- simple code generation;
- shell/build orchestration where appropriate.

Each unsupported construct should become a specific compatibility issue rather than a vague "support JavaScript" task.

## 8. Preserve the default-branch transition

The Edriç rewrite is now on `master`, while the old C implementation remains recoverable from Git history. Keep the active implementation visible at the repository root and do not restore the old MuJS directory layout as compatibility work grows.
