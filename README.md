# .github

Org-wide defaults for UMB Advisors repositories.

**This repository must be public for GitHub to apply the defaults below.** Default
community health files are used by every repository in the org, public or private,
but only when they live in a *public* repository named `.github`. While this repo is
private, nothing here is applied anywhere.

Everything in here is deliberately generic. No client names, no internal hosts, no
credentials: it is world-readable once made public.

| Path | What it does |
|---|---|
| `SECURITY.md` | Default security policy for repos without one |
| `.github/PULL_REQUEST_TEMPLATE.md` | Default PR body for repos without one |
| `.github/workflows/node-ci.yml` | Reusable pnpm/Node CI, called via `workflow_call` |
| `.github/workflows/python-ci.yml` | Reusable uv/Python CI, called via `workflow_call` |
| `.github/workflows/linear-issue-link.yml` | Reusable check that a PR names a Linear issue key |

A repository that has its own file of the same type keeps its own; these are fallbacks.

## Using the reusable workflows

Both are **reference implementations that no repository calls yet.** The first caller
is what proves them; expect to adjust inputs on that first run.

```yaml
# .github/workflows/ci.yml in a consuming repository
name: CI
on:
  push: { branches: [main] }
  pull_request:
    paths-ignore: ['docs/**', '**/*.md']
concurrency:
  group: ci-${{ github.ref }}
  cancel-in-progress: true
jobs:
  ci:
    uses: UMB-Advisors/.github/.github/workflows/node-ci.yml@main
    with:
      node-version: '22'
```

`paths-ignore` and `cancel-in-progress` belong to the caller, not the reusable
workflow, and are where most of the Actions bill is saved.

## Linear issue link

Fails a pull request whose branch name, title and body all lack a Linear issue key
(`UMB-123`, any case, so Linear's own `user/umb-123-slug` branch names count). A PR
labelled `no-linear` is skipped: use it for dependency bumps and typo fixes. Create
that label in the calling repository.

```yaml
# .github/workflows/linear-issue-link.yml in a consuming repository
name: linear-issue-link
on:
  pull_request:
    types: [opened, edited, reopened, synchronize, labeled, unlabeled]
permissions: {}
jobs:
  linear:
    uses: UMB-Advisors/.github/.github/workflows/linear-issue-link.yml@main
```

`edited` re-runs it when the title or body is fixed; `labeled` and `unlabeled` re-run
it when the opt-out label changes. Inputs `prefix` and `opt-out-label` override the
defaults.
