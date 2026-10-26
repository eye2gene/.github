# Contributing to Eye2Gene

Quick reference. The controlled rules and their rationale are in
**WI-19 Git & GitHub Conventions** (QMS). A repo's own `CONTRIBUTING.md` or its
Software Development Plan may add stricter rules; the stricter rule wins.

## Flow (SOP-25 §5.1.3, SDP §10.3)

1. **Issue first.** Every change traces to a GitHub issue: bug, requirement or change request.
2. **Branch** from `main`: `<type>/<issue>-<slug>`, e.g. `fix/78-pagination-offset`.
3. **Commit** freely on the branch. Rebase and fixups are fine; never rewrite `main`.
4. **Open a PR** with the template. The PR title becomes the commit on `main`.
5. **Merge** by squash once CI is green and a code owner approves (SOP-25 §5.1.3.5).
   The branch is deleted and the issue closed.

## Commits: [Conventional Commits](https://www.conventionalcommits.org) (SDP-QTF-1 §4.2)

```
fix(scan-list): correct pagination offset

Off-by-one dropped the last result on each page.

Refs: #78
```

| Type | Use for |
|---|---|
| `feat` | New or changed requirement |
| `fix` | Defect; `Refs:` the bug issue (SOP-25 §5.6.2.10) |
| `build(deps)` | SOUP added, updated or removed; update the SOUP list |
| `test` `refactor` `perf` `style` `docs` `ci` `chore` `revert` | As named |
| `chore(release)` | Version bump, release prep |

- Format: `<type>[(<scope>)][!]: <subject>`. Scope is optional: `fix: …` and `fix(scan-list): …` are both fine.
- Subject: imperative, max 72 characters, no full stop.
- `!` marks a breaking change (`feat!: …`, `fix(api)!: …`); under semantic-release it bumps the major version.
  Explain what breaks in the body or a `BREAKING CHANGE:` trailer, and assess impact in the PR.
- Trailers go in the last paragraph: `Refs: #N` (or `Closes: #N`, or `org/repo#N`).
  Regulated repos may add `Requirement:` and `Risk-Control:` with IDs exactly as in Formwork,
  qualified by document when the ID is document-scoped (e.g. `PRS-DP-1 SEC-001`).
- `Refs:` is required on `main` and optional on branch commits, because the squash carries the PR's.

Local setup (copy `.gitmessage` and `git-hooks/` into the repo first):

```bash
git config core.hooksPath git-hooks && git config commit.template .gitmessage
```

CI backstop: call [`pr-lint.yml`](.github/workflows/pr-lint.yml), which checks the PR title and issue link.

## Review comments

Start each comment with a label so intent is unambiguous:

| Label | Meaning |
|---|---|
| `blocking:` | Must be fixed before merge. Say why. |
| `suggestion:` | Optional improvement; author decides |
| `question:` | Needs an answer, not necessarily a change |
| `nit:` | Trivial or style; never blocks |
| `praise:` | Worth keeping |

The author replies with the fixing commit or a reason. The reviewer resolves their own `blocking:` threads.

## Issues

- Use the forms. The triager adds the component label (SOP-25 §5.1.3.4).
- Quality events (NCR, CAPA, complaint, security incident, data breach) go in the
  QMS repository, not here (SOP-3, SOP-33). The issue chooser links to them.

## Data

Never put patient data, identifying filenames, customer files or secrets in an
issue, PR, commit, comment, log or screenshot.

## AI agents

Agents follow [`AGENTS.md`](AGENTS.md). The human who opens or approves the
PR is accountable for every line.
