---
name: pr
description: Draft an Eye2Gene pull request title and body from the branch's changes, using the org PR template with its SOP-2 impact assessment. Use when the user asks to open, create or describe a pull request in an Eye2Gene repository.
disable-model-invocation: true
---

# Pull request

1. Gather `git log main..HEAD`, `git diff main...HEAD --stat` and the linked
   issue (from the branch name, or ask).
2. Use the repo's own PR template if it has one, otherwise the org default:
   What changed, Why, Impact assessment, Verification, Checklist, and a final
   `Closes: #N` line. Keep every section; write "None: <reason>" instead of deleting one.
3. Title: a Conventional Commits subject, max 72 characters. It becomes the
   squash commit on `main`.
4. Impact assessment (SOP-2 §5.1.2.3): tick only what the diff shows. Flag
   these to the human rather than deciding them:
   - dependency manifests changed → SOUP;
   - data handling, de-identification, auth or logging changed → data protection / risk;
   - anything ticked other than "None" → possible product change control.
5. Verification: list only commands you actually ran, with their results.
6. Show the draft. Run `gh pr create` only when asked; open as a draft unless told otherwise.

No patient data, identifying filenames or secrets in the PR.
