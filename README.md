# Actions

A curated set of **reusable GitHub Actions workflows** designed to provide **CI/CD guardrails** (commit/branch governance, pre-commit enforcement, secret scanning, and automated releases) with opinionated, production-ready defaults.

> Primary goal: standardize quality gates across repositories with minimal duplication and consistent compliance.

---

## Contents

- [Actions](#actions)
  - [Contents](#contents)
  - [What’s included](#whats-included)
  - [Quickstart](#quickstart)
  - [Reusable workflows](#reusable-workflows)
    - [Commit Check](#commit-check)
    - [Pre-commit](#pre-commit)
    - [Gitleaks](#gitleaks)
    - [Release (semantic-release)](#release-semantic-release)
  - [Versioning strategy](#versioning-strategy)
  - [Security and permissions](#security-and-permissions)
  - [Contributing](#contributing)
  - [License](#license)

---

## What’s included

- **Commit & branch governance**: enforce consistent conventions during PR lifecycle.
- **Pre-commit enforcement**: run your repository hooks as a CI gate.
- **Shift-left security**: secret scanning via Gitleaks.
- **Release automation**: semantic-release pipeline to standardize changelogs, tags, and releases.

---

## Quickstart

In your consumer repository, create a workflow file (e.g. `.github/workflows/quality-gates.yml`) and call the reusable workflows.

> Recommended: **pin to a tag** (e.g. `@v1`) for stability.

```yaml
name: Quality Gates

on:
  pull_request:
  workflow_dispatch:

jobs:
  commit-check:
    uses: Diaszano/actions/.github/workflows/commit-check.yaml@v1
    secrets: inherit

  pre-commit:
    uses: Diaszano/actions/.github/workflows/pre-commit.yaml@v1
    needs: commit-check

  gitleaks:
    uses: Diaszano/actions/.github/workflows/gitleaks.yml@v1
    secrets: inherit
````

---

## Reusable workflows

### Commit Check

**File:** `.github/workflows/commit-check.yaml`
Validates commit messages, branch names, and (optionally) author metadata. Can publish results to the job summary and optionally comment on PRs.

**Usage**

```yaml
jobs:
  commit-check:
    uses: Diaszano/actions/.github/workflows/commit-check.yaml@v1
```

**Inputs**

* `message` *(boolean, default: true)* — enable commit message validation
* `branch` *(boolean, default: true)* — enable branch name validation
* `author-name` *(boolean, default: false)* — enable author name validation
* `author-email` *(boolean, default: false)* — enable author email validation
* `job-summary` *(boolean, default: true)* — publish results to job summary
* `pr-comments` *(boolean, default: false)* — post results as PR comments *(requires `pull-requests: write`)*

**Secrets**

* `token` *(optional)* — token used for PR comments; if omitted, uses the workflow token

**Example (custom inputs)**

```yaml
jobs:
  commit-check:
    uses: Diaszano/actions/.github/workflows/commit-check.yaml@v1
    with:
      message: true
      branch: true
      author-name: false
      author-email: false
      job-summary: true
      pr-comments: false
```

---

### Pre-commit

**File:** `.github/workflows/pre-commit.yaml`
Runs `pre-commit` hooks as a CI gate (all files), enabling consistent linting/formatting/security checks across repos.

**Usage**

```yaml
jobs:
  pre-commit:
    uses: Diaszano/actions/.github/workflows/pre-commit.yaml@v1
```

**Inputs**

* `python-version` *(string, default: "3.13")* — Python version used to run pre-commit

**Example**

```yaml
jobs:
  pre-commit:
    uses: Diaszano/actions/.github/workflows/pre-commit.yaml@v1
    with:
      python-version: "3.13"
```

---

### Gitleaks

**File:** `.github/workflows/gitleaks.yml`
Runs a Gitleaks scan to detect leaked secrets and provides PR feedback (when permissions allow).

**Usage**

```yaml
jobs:
  gitleaks:
    uses: Diaszano/actions/.github/workflows/gitleaks.yml@v1
```

**Secrets**

* `token` *(optional)* — token used for PR interactions; if omitted, uses the workflow token

---

### Release (semantic-release)

**File:** `.github/workflows/release.yaml`
Standardizes releases using `semantic-release`, including changelog generation and Git tags.

**Usage**

```yaml
name: Release

on:
  push:
    branches: [main]

jobs:
  release:
    uses: Diaszano/actions/.github/workflows/release.yaml@v1
```

**Inputs**

* `node-version` *(string, default: "24")* — Node.js version for semantic-release

**Secrets**

* `token` *(optional)* — token used for release operations; if omitted, uses the workflow token

---

## Versioning strategy

This repository is designed to be consumed via **Git refs**:

* Prefer **major tags** like `@v1` to minimize breaking changes impact.
* Tags follow the format `vX.Y.Z`, and releases are driven via **semantic-release**.

---

## Security and permissions

Some workflows require additional permissions depending on enabled features:

* **Commit Check**: `pull-requests: write` only if `pr-comments: true`
* **Gitleaks**: `pull-requests: write` for PR feedback
* **Release**: needs write permissions for repository contents/releases

For internal/org usage, `secrets: inherit` is typically the simplest operational model.

---

## Contributing

PRs are welcome. Please:

* keep workflows reusable (`workflow_call`)
* avoid breaking changes without a clear versioning plan
* document new workflows with a usage snippet and inputs/secrets table

---

## License

Licensed under the **GNU General Public License v3.0**. See [`LICENSE`](./LICENSE).
