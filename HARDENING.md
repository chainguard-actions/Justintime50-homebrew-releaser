<!-- markdownlint-disable -->

# Hardening Report: Justintime50--homebrew-releaser/v3.3.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Justintime50--homebrew-releaser/v3.3.0** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple `uses:` references in workflow files use mutable tags or branch names instead of full 40-character SHA commit hashes, making the workflows vulnerable to supply-chain attacks. Failing references:
- `justintime50/composite-actions/bootstrap-python@v1` (4 occurrences in build.yml)
- `Homebrew/actions/setup-homebrew@master` (build.yml)
- `actions/checkout@v6` (build.yml and update-stable.yml)
- `codecov/codecov-action@v5` (build.yml)

Additionally, `action.yml` references a Docker image with a mutable tag instead of a SHA digest: `docker://justintime50/homebrew-releaser:3.2.1`

Locations:

- `.github/workflows/build.yml:13`
- `.github/workflows/build.yml:20`
- `.github/workflows/build.yml:27`
- `.github/workflows/build.yml:33`
- `.github/workflows/build.yml:36`
- `.github/workflows/build.yml:43`
- `.github/workflows/build.yml:52`
- `.github/workflows/update-stable.yml:11`
- `action.yml:72`

### script-injection (severity: high)

Sub-rule (a): A `${{ ... }}` expression is interpolated directly inside a `run:` shell command string. In the `docker` job of build.yml, `${{ secrets.HOMEBREW_GITHUB_TOKEN }}` is embedded directly in the `docker run` shell command. Any `${{ ... }}` expression inside a `run:` block flows through YAML template substitution before the shell processes it, making it a script-injection risk. Offending line: `-e INPUT_GITHUB_TOKEN=${{ secrets.HOMEBREW_GITHUB_TOKEN }} \`

Locations:

- `.github/workflows/build.yml:47`

### missing-permissions (severity: medium)

Neither `build.yml` nor `update-stable.yml` defines a top-level `permissions:` key, and no individual job within either file defines a `permissions:` key. Without explicit permissions, workflows run with the default (potentially broad) token permissions, which may grant unintended write access to repository contents, packages, or other resources.

Locations:

- `.github/workflows/build.yml:1`
- `.github/workflows/update-stable.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection, missing-permissions

**Notes:**

Fixed all three findings:

1. **unpinned-uses**: Pinned all action references to full SHA hashes in build.yml and update-stable.yml (justintime50/composite-actions/bootstrap-python@v1 → SHA, Homebrew/actions/setup-homebrew@master → SHA, actions/checkout@v6 → SHA, codecov/codecov-action@v5 → SHA). Also pinned the Docker image in action.yml with its sha256 digest while preserving the docker:// scheme and :3.2.1 tag.

2. **script-injection**: In build.yml's docker job, moved `${{ secrets.HOMEBREW_GITHUB_TOKEN }}` from the inline `run:` shell command into the step's `env:` block as `INPUT_GITHUB_TOKEN`, then referenced it as `"$INPUT_GITHUB_TOKEN"` in the shell script.

3. **missing-permissions**: Added `permissions: {}` at the top level of both build.yml and update-stable.yml. For update-stable.yml, added `permissions: contents: write` at the job level since that job needs to push git tags.

