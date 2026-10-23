# Eye2Gene agent rules

For any AI coding agent working in an Eye2Gene repository. These are the only
always-on rules; everything else is the human's own workflow. A repo's own
`AGENTS.md` or `CLAUDE.md` adds to this, and the stricter rule wins.

You draft; the human decides, approves and is accountable.

## Never

- Send patient data, customer files or images, identifying filenames or secrets
  to any external service or model. Use synthetic data unless the human says otherwise.
- Claim something is compliant, validated or approved. Report evidence (the
  command and its output) and let humans decide under the QMS.
- Edit QMS records (Formwork, the QMS repo), requirement IDs or risk controls
  without explicit approval.
- Commit to `main`, force-push a shared branch or rewrite `main` history.

## Always

- Commits: Conventional Commits plus a `Refs: #N` trailer; see `CONTRIBUTING.md`.
- PRs: fill every section of `PULL_REQUEST_TEMPLATE.md`, including the impact
  assessment. Write "None" with a reason rather than deleting a section.
- Review comments: start with `blocking:`, `suggestion:`, `question:` or `nit:`.
- If a change might touch a requirement, risk control, SOUP or patient data,
  say so and ask; don't decide it.

Templates and optional skills: https://github.com/eye2gene/.github
