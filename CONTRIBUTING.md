# Contributing to Eye2Gene

Quick reference. The binding rules are in **SOP-25 Software Development Process**
(QMS); this page is how we meet them. Where a repo has its own conventions
(its own `CONTRIBUTING.md`, agent rules, or a template such as `e2g-pypkg`), those win.

## Flow (SOP-25 §5.1.3, SDP §10.3)

1. **Issue first.** Every change traces to a GitHub issue: bug, requirement or change request.
2. **Branch** from `main`: `<type>/<short-name>`, e.g. `fix/pagination-offset`.
3. **Commit** freely on the branch. Rebase and fixups are fine; never rewrite `main`.
4. **Open a PR** with the template.
5. **Merge** once CI is green and a code owner approves (SOP-25 §5.1.3.5).
   The branch is deleted and the issue closes (bugs in released regulated products
   stay open for SQA: see [Issues](#issues)).

Squash or merge commit:

- **Squash** (default) for short, focused branches such as fixes and small changes.
  The PR title becomes the one commit on `main`.
- **Merge commit** for longer features whose history matters, or when each commit
  should get its own changelog line (semantic-release reads every commit). Every branch
  commit lands on `main`; one without a type is left out of the version and changelog.
  Fold fixups in first (`git rebase -i --autosquash main`). The merge commit takes the
  PR title and description, so it carries `Closes: #N`.

## Commits: [Conventional Commits](https://www.conventionalcommits.org) (SDP-QTF-1 §4.2)

```
fix(scan-list): correct pagination offset
```

| Type                                                                  | Use for                                                                          |
| --------------------------------------------------------------------- | -------------------------------------------------------------------------------- |
| `feat`                                                                | New or changed requirement                                                       |
| `fix`                                                                 | Defect; the PR links the bug issue (SOP-25 §5.6.2.10)                            |
| `chore(deps)`                                                         | Dependency added, updated or removed; tick SOUP (third-party software) in the PR |
| `test` `refactor` `perf` `style` `docs` `build` `ci` `chore` `revert` | As named                                                                         |
| `chore(release)`                                                      | Version bump, release prep                                                       |

- Format: `<type>[(<scope>)][!]: <subject>`. Scope is optional: `fix: …` and `fix(scan-list): …` are both fine.
- Subject: imperative, max 72 characters, no full stop.
- `!` marks a breaking change (`feat!: …`, `fix(api)!: …`); release tooling uses it to bump the version.
  Explain what breaks in the body or a `BREAKING CHANGE:` trailer, and assess impact in the PR.
- Default: the subject line only. Avoid a body and `Refs:`; the why goes in the PR
  description. Add a trailer (last paragraph)
  only when needed: `BREAKING CHANGE:`, or in regulated repos `Requirement:` and
  `Risk-Control:` with IDs exactly as in Formwork, qualified by document when the ID is
  document-scoped (e.g. `PRS-DP-1 SEC-001`).
- Every commit on `main` traces to its issue (SOP-25 §5.6.2.10) through the PR body's
  `Closes: #N`, which `pr-lint` requires. A squash commit carries it; with a merge commit,
  the merge commit carries it and contains the branch commits.
  Bot PRs (Dependabot, release automation such as `chore(release): 1.2.0`) are exempt, and
  a PR from GitHub's Revert button links the PR it reverts.

Local setup (copy `.gitmessage` and `git-hooks/` into the repo first):

```bash
git config core.hooksPath git-hooks && git config commit.template .gitmessage
```

CI backstop: call [`pr-lint.yml`](.github/workflows/pr-lint.yml), which checks the PR title and issue link.

## Review comments

Start each comment with a label so intent is unambiguous:

| Label         | Meaning                                   |
| ------------- | ----------------------------------------- |
| `blocking:`   | Must be fixed before merge. Say why.      |
| `suggestion:` | Optional improvement; author decides      |
| `question:`   | Needs an answer, not necessarily a change |
| `nit:`        | Trivial or style; never blocks            |
| `praise:`     | Worth keeping                             |

The author replies with the fixing commit or a reason. The reviewer resolves their own `blocking:` threads.

## Issues

- Use the forms: Bug Report, Feature Request, or Task for docs, chores and other work.
- Before a bug is assigned, the triager sets its scope, type and criticality (Safety bugs
  are always High) and adds the component label (SOP-25 §5.1.3.4, §5.6.2.2).
- A fix for a bug in a released regulated product uses `Refs: #N`, not `Closes:`, so the issue
  stays open until SQA (software quality assurance) has verified the fix and closes it
  (SOP-25 §5.6.2.11).
- Bug statuses (SOP-25 §5.6.2) in GitHub:

  | Status      | In GitHub                                                                                              |
  | ----------- | ------------------------------------------------------------------------------------------------------ |
  | New         | Open, no assignee                                                                                      |
  | Assigned    | Assigned to a developer                                                                                |
  | In Progress | A PR is open                                                                                           |
  | Feedback    | PR merged; issue open while SQA tests                                                                  |
  | Closed      | Closed by SQA after verification, or by triage (duplicate, rejected or criticality None) with a reason |

- Quality events (NCR, CAPA, complaint, security incident, data breach) go in the
  QMS repository, not here (SOP-3, SOP-33). The issue chooser links to them.

## Data

Never put patient data, identifying filenames, customer files or secrets in an
issue, PR, commit, comment, log or screenshot.

## AI agents

Agents follow [`AGENTS.md`](AGENTS.md). The human who opens or approves the
PR is accountable for every line.
