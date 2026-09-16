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
