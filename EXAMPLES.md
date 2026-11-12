# Examples

How this repo reaches other repos, and what working under it looks like. The rules
are in [CONTRIBUTING.md](CONTRIBUTING.md) and WI-19; this page only shows them in use.

## How this repo applies to other repos

Nothing is copied automatically. There are two mechanisms:

| Files | How they reach a repo | Change here applies to |
|---|---|---|
| `ISSUE_TEMPLATE/`, `PULL_REQUEST_TEMPLATE.md`, `CONTRIBUTING.md`, `SECURITY.md` | **Inherited.** GitHub shows these in any org repo that has no file of its own. The repo never holds a copy. | Every repo, immediately, including new and private ones |
| `pr-lint.yml` | **Called.** A six-line caller in the repo points at `@main` here. | Every repo that has the caller |
| `CODEOWNERS.template`, `AGENTS.md`, `.gitmessage`, `git-hooks/` | **Copied** once per repo. | No repo: each keeps its copy |
| Branch protection, merge settings | **Set** by an admin in each repo's settings. Not a file. | No repo |

So a new repo gets the issue forms and PR template from minute one, but nothing is
checked or enforced until someone does the one-time setup below.

## Example 1: adopting the conventions in an existing repo

Repo: `vscode-oct-viewer`, a small VS Code extension. It has CI (`build.yml`) but no
`CODEOWNERS`, PR template or agent rules, and is not generated from `e2g-pypkg`.

**When this repo's changes reach `main`**, without anyone touching `vscode-oct-viewer`:

- *New issue* shows the Bug Report and Feature Request forms, plus links to the QMS
  for quality events. Blank issues are no longer offered (`blank_issues_enabled: false`).
- Every new PR opens with the What / Why / Impact / Verification / Checklist template.
- *Contributing* and *Security policy* links appear on the repo.

**One-time setup** by the maintainer, following the same flow it introduces:

1. Issue from the Feature Request form, e.g. *Adopt org conventions*: **#7**.
2. Branch and files:

   ```bash
   git switch main && git pull
   git switch -c chore/7-adopt-org-conventions
   ORG=../.github                                   # local clone of eye2gene/.github

   mkdir -p .github/workflows
   cp $ORG/CODEOWNERS.template .github/CODEOWNERS   # then edit the owners
   cp $ORG/AGENTS.md . && printf '@AGENTS.md\n' > CLAUDE.md
   cp $ORG/.gitmessage . && cp -r $ORG/git-hooks .
   git config core.hooksPath git-hooks && git config commit.template .gitmessage
   ```

   `.github/workflows/pr-lint.yml`:

   ```yaml
   name: PR Lint
   on:
     pull_request:
       types: [opened, edited, reopened, synchronize]
   jobs:
     lint:
       uses: eye2gene/.github/.github/workflows/pr-lint.yml@main
   ```

3. Commit, push, open the PR titled `chore: adopt org commit and PR conventions`,
   body ending `Closes: #7`. `pr-lint` already runs on this PR.
4. A repo admin sets merge settings (squash or merge commit, no rebase; both take
   the PR title and description, so `Closes: #N` lands on `main`; delete merged branches):

   ```bash
   gh api -X PATCH repos/eye2gene/vscode-oct-viewer \
     -F allow_squash_merge=true -F allow_merge_commit=true -F allow_rebase_merge=false \
     -f squash_merge_commit_title=PR_TITLE -f squash_merge_commit_message=PR_BODY \
     -f merge_commit_title=PR_TITLE -f merge_commit_message=PR_BODY \
     -F delete_branch_on_merge=true
   ```

   and adds branch protection on `main` (WI-19 §5.7): PR with one code-owner approval,
   required checks `pr-lint` and the repo's CI, no force pushes. The owners in
   `CODEOWNERS` must have write access, or code-owner review enforces nothing.

## Example 2: a feature under the conventions

Same repo, existing issue **#3** *Add button to export scan to folder*.

**Branch and commits.** The hook rejects a free-text subject:

```text
$ git commit -m "export button"

ERROR: Subject must follow Conventional Commits.

  export button

  Format: <type>[(<scope>)][!]: <subject>   e.g. fix(api): handle empty scan list
```

```bash
git switch -c feat/3-export-scan-folder
git commit -m "feat(webview): add export-to-folder button"
git commit -m "feat(extension): write selected scan files to chosen folder"
git commit -m "fixup! feat(webview): add export-to-folder button"   # allowed; folded in on merge
```

The commits don't need `Refs:`. The PR body's `Closes: #3` carries the issue link onto
`main`, whether the PR is squashed or merged with a merge commit.

**Pull request** `feat(viewer): export scan to folder`, from the template:

```markdown
## What changed
Export button in the viewer toolbar; the extension copies the open scan's files
to a folder the user picks.

## Why
Users want to hand a scan to another tool without finding it on disk (#3).

## Impact assessment
- [x] None outside this repository
- [ ] Data protection / patient data   <!-- files stay on the user's machine -->

## Verification
CI green; exported a synthetic test scan, file count matches.

## Checklist
- [x] CI green, no new lint warnings
- [x] Impact assessment completed
- [x] Code-owner review requested

Closes: #3
```

**CI.** `pr-lint` fails a title such as `Export button`, or a body without `Closes: #N`:

```text
PR title: Use '<type>[(<scope>)][!]: <subject>', e.g. 'fix(api): handle empty scan list'. ... Got: Export button
Issue link: Add 'Closes: #N' or 'Refs: #N' to the PR body. Every change on main traces to an issue (SOP-25 §5.6.2.10).
```

**Review.** Every comment starts with a label:

> **`blocking:`** An existing file with the same name is overwritten silently. Prompt, or add a suffix.
>
> **`question:`** Does this export the stabilised frames or the raw ones?
>
> **`suggestion:`** Disable the button while the scan is loading.
>
> **`praise:`** Reusing the cache keeps the export instant.

The author fixes the blocker (`fix(extension): do not overwrite existing files on export`),
replies with the commit, and answers the question. The reviewer resolves their
`blocking:` thread and approves.

**Squash merge** (a short branch). `main` gets one commit; issue #3 closes and the branch is deleted:

```text
feat(viewer): export scan to folder (#8)

## What changed
Export button in the viewer toolbar; ...

Closes: #3
```

## Variants

| Case | PR title | Notes |
|---|---|---|
| Bug | `fix(scan-list): correct pagination offset` | Bug Report form records the version first observed |
| Breaking change | `feat(api)!: drop v1 scan routes` | Say what breaks; impact is never "None" |
| Dependency | `chore(deps): bump fastapi to 0.115` | Tick *SOUP / supplier* |
| Long feature, history matters | `feat(viewer): add measurement tools` | Merge commit: every commit lands on `main`, so run `git rebase -i --autosquash main` first; the merge commit carries `Closes: #N` |
| Repo from `e2g-pypkg` | as above | Its own agent rules apply; add only `CODEOWNERS` and the `pr-lint` caller |

## With the optional agent skills

After `/plugin install eye2gene-qms@eye2gene` in Claude Code:

| Command | Does |
|---|---|
| `/eye2gene-qms:issue <description>` | Drafts the issue in the form's fields; asks before creating it |
| `/eye2gene-qms:commit` | Proposes a commit message from the staged diff |
| `/eye2gene-qms:pr` | Fills the PR template from the diff and issue; shows it before creating |
| `/eye2gene-qms:review <PR>` | Drafts labelled review comments; never approves |

The person who opens or approves the PR is accountable for what these produce.
