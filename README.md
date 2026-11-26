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
| [`pr-lint.yml`](.github/workflows/pr-lint.yml) | Called from each repo | CI check: PR title format and issue link |

GitHub applies the "Automatically" files to every repo in the org that has no
file of its own of that type. The issue forms come as a set: a repo with any file
of its own in `.github/ISSUE_TEMPLATE/` gets none of the org forms.

Language-specific CI (Python today) lives in
[`eye2gene/e2g-workflows`](https://github.com/eye2gene/e2g-workflows); this repo
holds only checks that apply to every language, such as `pr-lint`.

## Set up a repo

Each repo also needs `.github/CODEOWNERS` and a `pr-lint` caller, which don't
inherit. Org members set both up with one command from the members-only
[`eye2gene/.github-private`](https://github.com/eye2gene/.github-private), which
also holds the commit hook, the agent rules (`AGENTS.md`) and the agent skills:

```bash
gh api repos/eye2gene/.github-private/contents/setup-repo.sh \
  -H "Accept: application/vnd.github.raw" | bash -s -- --repo eye2gene/<repo>
```

It opens a setup issue and PR, and sets the merge settings if you're an admin on
the repo. Branch protection (SOP-25 §5.1.1.2) comes from the org ruleset once an
org owner creates it; until then an admin can add `--protect`.
