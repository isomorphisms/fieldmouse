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

## 3. Implement the small Node build-script surface

Fieldmouse is not trying to become all of Node. Implement only the filesystem/process/path behavior that real project build scripts require.

Initial surface:
- `process.argv`, environment access, and exit status;
- basic `path` joining/normalization;
- basic `fs` reads, writes, existence checks, and directory operations;
- useful errors rather than silent fallbacks for unsupported operations.

Add features from real scripts, not from a Node compatibility checklist.

## 4. Keep Fieldmouse building against current Edriç

Fieldmouse CI is pinned to an older Edriç commit while Catfood follows current Idriç/Edriç development. That creates a compiler-drift trap.

Done when:
- Fieldmouse is tested against the compiler Catfood actually installs;
- either the old pin is removed or the reason for retaining it is explicit;
- compiler incompatibilities fail in CI with a focused diagnostic;
- a compiler update cannot silently leave Fieldmouse unbuildable.

## 5. Make the Catfood installation contract green

Catfood should be able to feed Fieldmouse into a fresh environment and leave a runnable stable command behind.

Done when:
- Catfood clones the `edric-rewrite` line;
- the required Edriç compiler is available or bootstrapped;
- `fieldmouse.ipkg` builds;
- a small interpreter smoke test passes;
- `fieldmouse` is available from Catfood's stable `bin` directory.

Catfood PR #11 is the current integration attempt.

## 6. Build a real-world JavaScript fixture corpus

Tiny parser tests are not enough to tell whether Fieldmouse can replace Node for the build-script jobs we actually care about.

Add a fixture directory containing small, legal-to-copy scripts representative of real project work:
- argument parsing;
- filesystem traversal;
- JSON-ish data manipulation once objects/arrays exist;
- path construction;
- simple code generation;
- shell/build orchestration where appropriate.

Each unsupported construct should become a specific compatibility issue rather than a vague "support JavaScript" task.

## 7. Decide when the Edriç rewrite becomes the repository default

`master` still exposes the old MuJS-derived C implementation while the active rewrite lives on `edric-rewrite`.

Do not flip the default merely for cosmetic reasons. Make the transition when the rewrite has enough useful behavior that opening the repository should naturally land on it.

Before switching:
- the core interpreter smoke suite is green;
- arrays/objects and functions/calls are present;
- the intended Node build-script slice is documented;
- Catfood can build and expose Fieldmouse;
- the old C implementation remains recoverable from Git history without dominating the active tree.
