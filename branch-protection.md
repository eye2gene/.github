# Branch Protection — Org-Wide Baseline

## 1. Purpose

This document defines the baseline branch protection rules for the `main` branch
of Eye2Gene repositories. These settings enforce a controlled merge process
consistent with IEC 62304 §8.2.4 (traceability of change) and ISO 13485 §4.2.4
(control of documents). Individual repositories may add stricter rules but
should not weaken this baseline — see
[extending-for-regulated-repos.md](extending-for-regulated-repos.md).

## 2. Protected Branch: `main`

### 2.1 Pull Request Requirements

| Setting | Value | Rationale |
|---------|-------|-----------|
| Require a pull request before merging | Yes | All changes go through documented review |
| Required approvals | 1 (minimum) | Code owner review per CODEOWNERS |
| Dismiss stale PR approvals when new commits are pushed | Yes | Re-review required after changes |
| Require review from Code Owners | Yes | Designated reviewers per file area |

### 2.2 Status Checks

| Setting | Value | Rationale |
|---------|-------|-----------|
| Require status checks to pass before merging | Yes | CI must pass before merge |
| Require branches to be up to date before merging | Yes | Prevents untested merge conflicts |
| Required checks | Repo-specific — whatever the repo's CI job is named | Each repo configures this for its own workflow(s) |

### 2.3 Merge Strategy

| Setting | Value | Rationale |
|---------|-------|-----------|
| Allow squash merging | Yes (default) | PR body becomes the commit message with full traceability |
| Allow merge commits | No | Loses PR body traceability in commit history |
| Allow rebase merging | No | Individual commits lack traceability metadata |
| Default commit message for squash | PR body | Ensures IEC 62304 §8.2.4 traceability is preserved |

### 2.4 Additional Protections

| Setting | Value | Rationale |
|---------|-------|-----------|
| Restrict who can push to matching branches | Enabled | Only merge via PR |
| Allow force pushes | No | Prevents history rewriting |
| Allow deletions | No | Branch cannot be deleted |
| Require signed commits | Repo's discretion | Not mandated org-wide at this time |

## 3. Repository Settings (Pull Requests)

Configure these in each repo's Settings > General > Pull Requests:

- [x] Allow squash merging (set as default)
- [ ] Allow merge commits (disabled)
- [ ] Allow rebase merging (disabled)
- [x] Default commit message: PR body
- [x] Always suggest updating pull request branches
- [x] Automatically delete head branches

## 4. Configuration Notes

- These settings must be configured manually in each repository's GitHub UI —
  this file documents the intended configuration, it does not enforce it.
- Changes to branch protection rules require repository admin access.
- Any repo deviating from this baseline should document why in its own
  `branch-protection.md` (or equivalent), which takes precedence over this one.

## 5. References

- IEC 62304:2006+Amd1:2015 §8.2.4 — Provide means for traceability of change
- IEC 62304:2006+Amd1:2015 §8.3 — Configuration status accounting
- ISO 13485:2016 §4.2.4 — Control of documents
- ISO 13485:2016 §7.3.5 — Design and development review
- [CODEOWNERS](CODEOWNERS) — Designates required reviewers per file area
- [CONTRIBUTING.md](CONTRIBUTING.md) — Documents the merge and commit conventions
