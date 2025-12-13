# Commit Check (Reusable Workflow)

A reusable GitHub Actions workflow that enforces **commit and branch hygiene** across repositories by validating:

- Commit message format
- Branch naming convention
- Optional author name and author email checks
- Optional publishing to Job Summary
- Optional PR comments with results

This workflow is designed to be called via `workflow_call` and plugged into multiple repositories as a standardized compliance gate.

---

## Features

- ✅ Commit message validation (`message`)
- ✅ Branch name validation (`branch`)
- ✅ Optional author validation (`author-name`, `author-email`)
- ✅ Job Summary output (`job-summary`)
- ✅ Pull Request comments (`pr-comments`)
- ✅ Full history checkout (`fetch-depth: 0`) for reliable validation

---

## Usage

### Basic example

Create a workflow in the consumer repository, e.g. `.github/workflows/commit-check.yml`:

```yaml
name: Commit Check

on:
  pull_request:
  workflow_dispatch:

jobs:
  commit-check:
    uses: Diaszano/actions/.github/workflows/commit-check.yml@main
````

### Customizing inputs

```yaml
name: Commit Check

on:
  pull_request:
  workflow_dispatch:

jobs:
  commit-check:
    uses: Diaszano/actions/.github/workflows/commit-check.yml@main
    with:
      message: true
      branch: true
      author-name: false
      author-email: false
      job-summary: true
      pr-comments: false
```

> Replace `Diaszano/actions` and `main` with your repository coordinates (e.g., a tag like `v1`, `v1.2.0`, or a commit SHA).

---

## Inputs

| Name           | Type    | Default | Description                                        |
| -------------- | ------- | ------- | -------------------------------------------------- |
| `message`      | boolean | `true`  | Enable commit message validation.                  |
| `branch`       | boolean | `true`  | Enable branch naming validation.                   |
| `author-name`  | boolean | `false` | Enable author/committer name validation.           |
| `author-email` | boolean | `false` | Enable author/committer email validation.          |
| `job-summary`  | boolean | `true`  | Publish results to the GitHub Actions Job Summary. |
| `pr-comments`  | boolean | `false` | Post results as comments on Pull Requests.         |

---

## Secrets

| Name    | Required | Description                                                                                                |
| ------- | -------- | ---------------------------------------------------------------------------------------------------------- |
| `token` | no       | Optional token used for PR comments and API access. If omitted, the workflow falls back to `github.token`. |

### Recommended secret strategy

* **Preferred:** `secrets: inherit` (simplest for org/internal usage)
* **Alternative:** explicitly pass a token secret (useful for cross-org calls)

Example passing a custom token:

```yaml
jobs:
  commit-check:
    uses: Diaszano/actions/.github/workflows/commit-check.yml@main
    secrets:
      token: ${{ secrets.GITHUB_TOKEN }}
```

---

## Permissions

This workflow sets:

* `contents: read`
* `pull-requests: write`

> `pull-requests: write` is required only if `pr-comments: true`. Keeping it enabled makes the workflow plug-and-play; if you want a stricter posture, create a variant workflow with conditional permissions strategy.

---

## How it works

1. **Checkout with full history** (`fetch-depth: 0`) to ensure the action can inspect commits reliably.
2. Runs `commit-check/commit-check-action@v2` using inputs to enable/disable checks.
3. Optionally publishes output to:

   * Job Summary (`job-summary: true`)
   * PR comment (`pr-comments: true`)

---

## Operational Notes

* The workflow is best used on `pull_request` to gate changes before merge.
* Use stable references for `@<ref>` (tags/releases) to keep supply-chain risk under control.
* If PR comments are enabled, ensure the token has permission to write PR comments (the default `github.token` is usually sufficient within the same org/repo context).

---

## Troubleshooting

### No PR comment posted

* Confirm `pr-comments: true`.
* Confirm `pull-requests: write` permissions are in effect.
* If calling cross-repo/cross-org, pass an appropriate token via `secrets.token`.

### Missing commits / incomplete validation

* Ensure checkout is using `fetch-depth: 0` (already set by this workflow).
* Ensure the workflow runs in contexts where git history is available (PRs and normal pushes are OK).

---

## License

This project is licensed under the **GNU General Public License v3.0 (GPL-3.0)**.

You may copy, distribute, and modify the software under the terms of the GPL-3.0.
See the [`LICENSE`](../../../LICENSE) file for the full text of the license.
