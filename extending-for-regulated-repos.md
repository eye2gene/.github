# Extending These Defaults for Regulated Repositories

The files in this repository are org-wide **defaults**. GitHub automatically
prefers a repository's own community-health file over the org default of the
same kind — so a repo never has to ask permission to override these; it just
adds its own `CONTRIBUTING.md`, `PULL_REQUEST_TEMPLATE.md`,
`ISSUE_TEMPLATE/`, `CODEOWNERS`, `SECURITY.md`, or `branch-protection.md` at
its own root/`.github/`, and that wins.

## When a repo should override

Repositories that are, or feed into, a regulated medical device (SaMD)
generally need more than the lightweight org defaults — specifically:

- **Risk Impact tracking on PRs** — whether a change affects an existing risk
  control or introduces a new hazard (ISO 14971 / IEC 62304 §7.4.2).
- **SOUP change tracking** — third-party/dependency changes need to be
  identifiable and risk-assessed, not just labelled "dependency update."
- **Traceability to requirements/risk items in issue forms** — e.g. dedicated
  issue types for requirements, test scenarios, software problem reports
  (per SOP-36), and release checklists, each carrying explicit trace fields.
- **A CODEOWNERS split between engineering and QA** — e.g. QA/regulatory
  affairs owning `.github/`, `docs/`, `CHANGELOG.md`, `CONTRIBUTING.md`
  while engineering owns source code.

## Reference implementation

`trends_dashboard` is the current reference implementation of a fully
regulated repo's `.github` setup:

- `PULL_REQUEST_TEMPLATE.md` — adds **Change Type**, **Risk Impact**,
  **SOUP Changes**, and an expanded **Pre-Merge Checklist** on top of the
  org-default sections.
- `ISSUE_TEMPLATE/` — `requirement-issue.yml`, `test-scenario-issue.yml`,
  `software-problem-report.yml`, and `release-checklist.yml`, each with
  explicit trace fields (requirement ID, risk control ID, hazard ID).
- `CODEOWNERS` — splits source/test/config ownership from
  docs/QMS/template ownership.
- `.gitmessage` / `git-hooks/commit-msg` — same convention as this repo's
  defaults; repos don't need to change this part.

When building out a similar setup for another regulated repo, copy that
pattern rather than inventing a new one — the goal is that any two regulated
repos in the org look structurally the same to an auditor, even though their
requirement/risk IDs differ.

## What should stay on the org default

Keep using the org-wide `CONTRIBUTING.md` commit convention, `.gitmessage`,
and `git-hooks/commit-msg` as-is even in a regulated repo — there's no need
to fork those, only the PR/issue templates and CODEOWNERS typically need
repo-specific extensions.
