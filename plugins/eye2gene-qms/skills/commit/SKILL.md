---
name: commit
description: Draft an Eye2Gene commit message (Conventional Commits subject line) for the staged changes. Use when the user asks to commit or write a commit message in an Eye2Gene repository.
disable-model-invocation: true
---

# Commit message

If the repo has its own commit rules (`AGENTS.md`, `CLAUDE.md`,
`.kiro/steering/commits.md`), follow those; this skill only fills the gaps.

1. Read `git diff --staged`. If nothing is staged, ask; don't stage files yourself.
2. One logical change per commit. If the staged diff holds unrelated changes (the subject
   would need "and", or one part could be reverted without the other), say so and propose
   the split, one subject per commit. Split only if the user agrees.
3. Write the subject line only:

   ```text
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
4. If the diff changes a dependency manifest, say the SOUP list may need updating.
5. Show the message. Commit only if the user asked you to, and never on the default
   branch: check `git branch --show-current` first.

No patient data, customer file paths or secrets in the message.
The Never list in the org [`AGENTS.md`](https://github.com/eye2gene/.github/blob/main/AGENTS.md) applies.
