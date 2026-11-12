---
name: commit
description: Draft an Eye2Gene commit message (Conventional Commits subject line) for the staged changes. Use when the user asks to commit or write a commit message in an Eye2Gene repository.
disable-model-invocation: true
---

# Commit message

If the repo has its own commit rules (`AGENTS.md`, `CLAUDE.md`,
`.kiro/steering/commits.md`), follow those; this skill only fills the gaps.

1. Read `git diff --staged`. If nothing is staged, ask; don't stage files yourself.
2. Write the subject line only:

   ```
   <type>[(<scope>)][!]: <imperative subject, max 72 chars, no full stop>
   ```

   Don't add a body or `Refs:` unless the user asks: the why goes in the PR
   description, and the PR's `Closes: #N` links the issue.

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
