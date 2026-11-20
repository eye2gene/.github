# .github

Org-wide conventions and templates for Eye2Gene repositories. The binding rules
are in **SOP-25 Software Development Process** (QMS); this repo holds what people
and agents use day to day to meet them.

| File | Applies to other repos | Purpose |
| --- | --- | --- |
| [`CONTRIBUTING.md`](CONTRIBUTING.md) | Automatically | One-page quick reference: flow, commits, review comments |
| [`PULL_REQUEST_TEMPLATE.md`](PULL_REQUEST_TEMPLATE.md) | Automatically | PR body with the SOP-2 impact assessment |
| [`.github/ISSUE_TEMPLATE/`](.github/ISSUE_TEMPLATE/) | Automatically | Bug, feature and task forms; routes quality events to the QMS repo |
| [`SECURITY.md`](SECURITY.md) | Automatically | Vulnerability reporting |
| [`.gitmessage`](.gitmessage), [`git-hooks/commit-msg`](git-hooks/commit-msg), [`.gitattributes`](.gitattributes) | Copy in | Commit template and format check; the attributes keep both on LF line endings |
| [`pr-lint.yml`](.github/workflows/pr-lint.yml) | Call from the repo | CI check: PR title format and issue link |
| [`CODEOWNERS.template`](CODEOWNERS.template) | Copy in | Required per repo; does not inherit |
| [`AGENTS.md`](AGENTS.md) | Copy in | Minimal always-on rules for AI agents. Extend below it without contradicting it; recheck your copy when this one changes |
| [`plugins/eye2gene-qms`](plugins/eye2gene-qms) | Install | Optional agent skills: commit, PR, issue, review |
| [`EXAMPLES.md`](EXAMPLES.md) | No | How this repo reaches other repos, with worked examples |

GitHub inherits only the community health files marked "Automatically" (repos
can override them). Everything else must be copied or called per repo, and
branch protection must be set in each repo's settings (SOP-25 §5.1.1.2), unless an org
admin covers all repos with one org ruleset.

Language-specific CI (Python today) lives in
[`eye2gene/e2g-workflows`](https://github.com/eye2gene/e2g-workflows); this repo
holds only checks that apply to every language, such as `pr-lint`.

## Set up a repo

The issue forms, PR template, CONTRIBUTING and SECURITY reach every repo by
themselves. The rest needs one PR per repo.

**1. Required: two files.**

- `.github/CODEOWNERS`: copy [`CODEOWNERS.template`](CODEOWNERS.template) and set the owners.
- `.github/workflows/pr-lint.yml`:

  ```yaml
  name: PR Lint
  on:
    pull_request:
      types: [opened, edited, reopened, synchronize]
  jobs:
    lint:
      uses: eye2gene/.github/.github/workflows/pr-lint.yml@main
  ```

**2. Admin, once.** Branch protection: the org ruleset, or the repo's own settings
until that exists. Merge settings: see
[EXAMPLES.md](EXAMPLES.md#example-1-adopting-the-conventions-in-an-existing-repo).

**3. Optional.**

- *AI agents:* copy `AGENTS.md`. If the repo already has a `CLAUDE.md`, add
  `@AGENTS.md` as its first line.
- *Commit check before you push* (recommended where the repo uses merge commits):
  copy `.gitmessage`, `.gitattributes` and `git-hooks/`. Each person then runs
  `git config core.hooksPath git-hooks && git config commit.template .gitmessage`
  once in their clone.

Repos generated from `e2g-pypkg` already have step 3: add only step 1, and change
anything else in the template so `cruft update` carries it.

## Agent skills (optional)

Claude Code: `/plugin marketplace add eye2gene/.github`, then
`/plugin install eye2gene-qms@eye2gene`. The skills run only when invoked
(`/eye2gene-qms:commit`, `:pr`, `:issue`, `:review`). Other agents can read the
`SKILL.md` files directly; they're plain Markdown.
