# Contributing to Eye2Gene

Quick reference. The controlled rules and their rationale are in
**WI-19 Git & GitHub Conventions** (QMS). Where a repo has its own conventions
(its own `CONTRIBUTING.md`, agent rules, or a template such as `e2g-pypkg`), those win.

## Flow (SOP-25 §5.1.3, SDP §10.3)

1. **Issue first.** Every change traces to a GitHub issue: bug, requirement or change request.
2. **Branch** from `main`: `<type>/<short-name>`, e.g. `fix/pagination-offset`.
3. **Commit** freely on the branch. Rebase and fixups are fine; never rewrite `main`.
4. **Open a PR** with the template.
5. **Merge** once CI is green and a code owner approves (SOP-25 §5.1.3.5).
   The branch is deleted and the issue closed.

Squash or merge commit:

- **Squash** (default) for short, focused branches such as fixes and small changes.
  The PR title becomes the one commit on `main`.
- **Merge commit** for longer features whose history matters, and in repos whose
  release tooling (e.g. semantic-release) reads every commit. Every branch commit
  lands on `main`, so each must follow the format below; fold fixups in first
  (`git rebase -i --autosquash main`). The merge commit takes the PR title and
  description, so it carries `Closes: #N`.

## Commits: [Conventional Commits](https://www.conventionalcommits.org) (SDP-QTF-1 §4.2)

```text
fix(scan-list): correct pagination offset
```

| Type                                                                  | Use for                                                   |
| --------------------------------------------------------------------- | --------------------------------------------------------- |
| `feat`                                                                | New or changed requirement                                |
| `fix`                                                                 | Defect; the PR closes the bug issue (SOP-25 §5.6.2.10)    |
| `chore(deps)`                                                         | Dependency added, updated or removed; tick SOUP in the PR |
| `test` `refactor` `perf` `style` `docs` `build` `ci` `chore` `revert` | As named                                                  |
| `chore(release)`                                                      | Version bump, release prep                                |

- Format: `<type>[(<scope>)][!]: <subject>`. Scope is optional: `fix: …` and `fix(scan-list): …` are both fine.
- Subject: imperative, max 72 characters, no full stop.
- `!` marks a breaking change (`feat!: …`, `fix(api)!: …`); release tooling uses it to bump the version.
  Explain what breaks in the body or a `BREAKING CHANGE:` trailer, and assess impact in the PR.
- Default: the subject line only. Avoid a body and `Refs:`; the why goes in the PR
  description and the PR's `Closes: #N` links the issue. Add a trailer (last paragraph)
  only when needed: `BREAKING CHANGE:`, or in regulated repos `Requirement:` and
  `Risk-Control:` with IDs exactly as in Formwork, qualified by document when the ID is
  document-scoped (e.g. `PRS-DP-1 SEC-001`).
- Every commit on `main` traces to its issue (SOP-25 §5.6.2.10) through the PR body's
  `Closes: #N`, which `pr-lint` requires. A squash commit carries it; with a merge commit,
  the merge commit carries it and contains the branch commits.
  Release automation commits (e.g. `chore(release): 1.2.0`) are exempt.

Local setup (copy `.gitmessage` and `git-hooks/` into the repo first):

```bash
git config core.hooksPath git-hooks && git config commit.template .gitmessage
```

CI backstop: call [`pr-lint.yml`](.github/workflows/pr-lint.yml), which checks the PR title and issue link.

## Python repositories

Python repos are generated from the [`e2g-pypkg`](https://github.com/eye2gene/e2g-pypkg)
template (private) and keep in step with it through `cruft update`: change the template,
not the copies. They carry their own commit and agent rules, so from this repo they need
only `.github/CODEOWNERS` and the `pr-lint` caller.

- `just qa` formats, lints, type-checks and audits; `just test` runs the tests with coverage.
  CI runs the same recipes through `python-ci.yml` of
  [`e2g-workflows`](https://github.com/eye2gene/e2g-workflows).
- A merge to `main` releases: python-semantic-release reads every commit since the last tag
  (`fix` bumps the patch version, `feat` the minor, a breaking change `!` the major), writes
  the changelog, tags and publishes. These are the repos whose release tooling reads every
  commit (see *Squash or merge commit* above).
- Dependencies come from PyPI through `uv` and the lock file, 7 days after their release;
  `just allow <package>` lets a security fix in early for every project at once.

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

- Use the forms. The triager adds the component label (SOP-25 §5.1.3.4).
- Quality events (NCR, CAPA, complaint, security incident, data breach) go in the
  QMS repository, not here (SOP-3, SOP-33). The issue chooser links to them.

## Data

Never put patient data, identifying filenames, customer files or secrets in an
issue, PR, commit, comment, log or screenshot.

## AI agents

Agents follow [`AGENTS.md`](AGENTS.md). The human who opens or approves the
PR is accountable for every line.
