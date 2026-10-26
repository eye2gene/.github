# .github

Org-wide templates for Eye2Gene repositories. The rules and their rationale
live in the QMS, in **WI-19 Git & GitHub Conventions**; this repo holds only
what people and agents use day to day.

| File | Applies to other repos | Purpose |
|---|---|---|
| [`CONTRIBUTING.md`](CONTRIBUTING.md) | Automatically | One-page quick reference: flow, commits, review comments |
| [`PULL_REQUEST_TEMPLATE.md`](PULL_REQUEST_TEMPLATE.md) | Automatically | PR body with the SOP-2 impact assessment |
| [`ISSUE_TEMPLATE/`](ISSUE_TEMPLATE/) | Automatically | Bug and feature forms; routes quality events to the QMS repo |
| [`SECURITY.md`](SECURITY.md) | Automatically | Vulnerability reporting |
| [`.gitmessage`](.gitmessage), [`git-hooks/commit-msg`](git-hooks/commit-msg) | Copy in | Commit template and format check |
| [`pr-lint.yml`](.github/workflows/pr-lint.yml) | Call from the repo | CI check: PR title format and issue link |
| [`CODEOWNERS.template`](CODEOWNERS.template) | Copy in | Required per repo; does not inherit |
| [`AGENTS.md`](AGENTS.md) | Copy in | Minimal always-on rules for AI agents; skip if the repo has its own |
| [`plugins/eye2gene-qms`](plugins/eye2gene-qms) | Install | Optional agent skills: commit, PR, issue, review |

GitHub inherits only the community health files marked "Automatically" (repos
can override them). Everything else must be copied or called per repo, and
branch protection must be set in each repo's settings (WI-19).

## New repo setup

```bash
cp <this-repo>/{.gitmessage,AGENTS.md} . && cp -r <this-repo>/git-hooks .
cp <this-repo>/CODEOWNERS.template .github/CODEOWNERS   # then edit
printf '@AGENTS.md\n' > CLAUDE.md                        # Claude Code reads CLAUDE.md
git config core.hooksPath git-hooks && git config commit.template .gitmessage
```

Then add the `pr-lint.yml` caller (see the comment at the top of that file) and apply branch protection.

Repos generated from `e2g-pypkg` (they have a `.cruft.json`) get commit and agent rules from the
template: add only `.github/CODEOWNERS` and the `pr-lint` caller, and change anything else in the
template so `cruft update` carries it.

## Agent skills (optional)

Claude Code: `/plugin marketplace add eye2gene/.github`, then
`/plugin install eye2gene-qms@eye2gene`. The skills run only when invoked
(`/eye2gene-qms:commit`, `:pr`, `:issue`, `:review`). Other agents can read the
`SKILL.md` files directly; they're plain Markdown.
