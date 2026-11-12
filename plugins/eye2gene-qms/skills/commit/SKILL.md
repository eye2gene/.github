---
name: commit
description: Draft an Eye2Gene commit message (Conventional Commits, optional Refs trailer) for the staged changes. Use when the user asks to commit or write a commit message in an Eye2Gene repository.
disable-model-invocation: true
---

# Commit message

If the repo has its own commit rules (`AGENTS.md`, `CLAUDE.md`,
`.kiro/steering/commits.md`), follow those; this skill only fills the gaps.

1. Read `git diff --staged`. If nothing is staged, ask; don't stage files yourself.
2. Draft the subject only:

   ```
   <type>[(<scope>)][!]: <imperative subject, max 72 chars, no full stop>
   ```

   Add a body (what changed and why, wrapped at 72) only when the reason
   isn't obvious from the subject. Add `Refs: #N` only if the issue number
   is known (branch `<type>/<N>-<slug>`); never invent one and don't ask.
   The PR body's `Closes: #N` links the issue.

   Types: `feat` (requirement), `fix` (defect), `chore(deps)` (dependency change),
   `test`, `refactor`, `perf`, `style`, `docs`, `build`, `ci`, `chore`, `revert`,
   `chore(release)`. Scope is optional; use one only if the repo already does.
   For a breaking change add `!` before the colon (`feat!:`, `fix(api)!:`)
   and a `BREAKING CHANGE: <what breaks>` trailer.
   Add `Requirement:` or `Risk-Control:` trailers only with IDs the user or
   the issue gives you.
3. If the diff changes a dependency manifest, say the SOUP list may need updating.
4. Show the message. Commit only if the user asked you to.

No patient data, customer file paths or secrets in the message.
