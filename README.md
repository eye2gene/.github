# .github

Default health files (issue/PR templates, contributing guidelines) for all Eye2Gene repositories. Any file
here applies automatically to a repo that doesn't define its own copy.

## What's here

| File | Purpose |
|------|---------|
| [`CONTRIBUTING.md`](CONTRIBUTING.md) | Commit message convention (`[TYPE] Subject`), branch naming, PR/merge rules |
| [`branch-protection.md`](branch-protection.md) | Baseline `main` branch protection settings |
| [`PULL_REQUEST_TEMPLATE.md`](PULL_REQUEST_TEMPLATE.md) | Default PR template |
| [`ISSUE_TEMPLATE/`](ISSUE_TEMPLATE/) | Default bug report / feature request forms |
| [`CODEOWNERS`](CODEOWNERS) | Default review-request fallback |
| [`SECURITY.md`](SECURITY.md) | Vulnerability reporting policy |
| [`.gitmessage`](.gitmessage) + [`git-hooks/commit-msg`](git-hooks/commit-msg) | Copyable commit-message template and enforcement hook |
| [`.github/workflows/pr-title-lint.yml`](.github/workflows/pr-title-lint.yml) | Reusable CI workflow other repos can call to enforce PR title format |
| [`extending-for-regulated-repos.md`](extending-for-regulated-repos.md) | How regulated/SaMD repos layer stricter local overrides |

Start with [`CONTRIBUTING.md`](CONTRIBUTING.md) if you're new to the org's conventions.
