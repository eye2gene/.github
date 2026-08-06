# Contributing to Eye2Gene Projects

This is the org-wide default. It applies to any Eye2Gene repository that doesn't
define its own `CONTRIBUTING.md`. If a repo has stricter local rules (e.g. regulated
SaMD products), those take precedence — see
[extending-for-regulated-repos.md](extending-for-regulated-repos.md).

## Merge Strategy

Repositories use **squash merge** as the only merge method into `main`.

| Rule | Reason |
|------|--------|
| Squash merge only | Each change on `main` is one atomic, traceable commit (IEC 62304 §8.2.4) |
| PR body becomes commit message | Traceability metadata from the PR template is preserved in git history |
| Feature branch commits are free-form | No enforcement on individual commits during development |
| Direct push to `main` prohibited | Branch protection ensures all changes go through PR review |

See [branch-protection.md](branch-protection.md) for the full repository settings baseline.

## Setup

To adopt the commit-message convention below in a repository, copy in
[`.gitmessage`](.gitmessage) and [`git-hooks/commit-msg`](git-hooks/commit-msg), then run:

```bash
git config core.hooksPath git-hooks
git config commit.template .gitmessage
```

This enables:
- **commit-msg hook** — rejects commits that don't match the `[TYPE] Subject` format
- **commit template** — pre-fills the message structure when using `git commit` without `-m`

Repositories can also opt into the reusable
[`pr-title-lint.yml`](.github/workflows/pr-title-lint.yml) workflow as a CI backstop for
contributors who haven't run the local setup.

## Commit Message Format

### Format

```
[TYPE] Short imperative description

Body explaining what and why.

Refs: #123
```

### Types

| Type | Use when | Should link to |
|------|----------|-----------------|
| REQ | Implementing a requirement | Requirement/feature issue |
| FIX | Fixing a defect | Bug issue |
| TEST | Adding/updating tests | Related issue |
| REFACTOR | Internal change, no functional impact | Parent issue (if applicable) |
| SOUP | Dependency added/updated/removed | SOUP list update, if the repo maintains one |
| DOCS | Documentation only | N/A |
| BUILD | CI/build system | N/A |
| RELEASE | Version bump, release prep | Release issue/checklist |

### Examples

```
[FIX] Correct pagination offset in scan list endpoint

Off-by-one error caused the last result on each page to be
dropped. Affected the Grading Queue view.

Refs: #78
Closes: #78
```

```
[SOUP] Update Plotly.js from 2.27.0 to 2.30.0

Security patch for a reported XSS in hover labels. No
functional changes to our usage.

Refs: #92
```

## Branch Naming

```
<type>/<issue-number>-<short-description>
```

Examples:
- `req/45-add-export-endpoint`
- `fix/78-pagination-offset`
- `docs/92-update-readme`

## Pull Request Requirements

Every PR to `main` must:

1. Use the PR template (auto-loaded from [`PULL_REQUEST_TEMPLATE.md`](PULL_REQUEST_TEMPLATE.md))
2. Link to at least one issue — create one first if none exists
3. Pass all CI checks
4. Be approved by a code owner (per [`CODEOWNERS`](CODEOWNERS) or the repo's own)
5. Be merged with squash merge (enforced by repository settings)

## Issues

Use the templates under [`ISSUE_TEMPLATE/`](ISSUE_TEMPLATE/). Quality events (CAPA,
NCR, complaints, RMF/hazard updates, QMS document changes) are filed through the QMS
system, not as GitHub issues — see [`ISSUE_TEMPLATE/config.yml`](ISSUE_TEMPLATE/config.yml)
for the routing links.

## For Auditors

To verify traceability for any change on a repo following this convention:

1. `git log --oneline main` — see all changes with type prefix
2. Pick any commit — read the body for `Refs:` lines
3. Follow the issue link — see the requirement/defect/test scenario with full metadata

This satisfies IEC 62304 §8.2.4 (traceability of change) and §8.3 (configuration status
accounting).
