---
name: commit
description: Draft an Eye2Gene commit message (Conventional Commits plus a Refs trailer) for the staged changes. Use when the user asks to commit or write a commit message in an Eye2Gene repository.
disable-model-invocation: true
---

# Commit message

1. Read `git diff --staged`. If nothing is staged, ask; don't stage files yourself.
2. Find the issue number from the branch name (`<type>/<N>-<slug>`) or ask.
   Never invent one.
3. Draft:

   ```
   <type>[(<scope>)][!]: <imperative subject, max 72 chars, no full stop>

   <what changed and why, wrapped at 72; omit for trivial changes>

   Refs: #N
   ```

   Types: `feat` (requirement), `fix` (defect), `build(deps)` (SOUP change),
   `test`, `refactor`, `perf`, `style`, `docs`, `ci`, `chore`, `revert`,
   `chore(release)`. Scope is optional; use one only if the repo already does.
   For a breaking change add `!` before the colon (`feat!:`, `fix(api)!:`)
   and a `BREAKING CHANGE: <what breaks>` trailer.
   Add `Requirement:` or `Risk-Control:` trailers only with IDs the user or
   the issue gives you.
4. If the diff changes a dependency manifest, say the SOUP list may need updating.
5. Show the message. Commit only if the user asked you to.

No patient data, customer file paths or secrets in the message.
