<!-- markdownlint-disable -->

# Hardening Report: Justintime50--homebrew-releaser/v4.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Justintime50--homebrew-releaser/v4.0.0** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): A ${{ }} expression is interpolated directly inside a `run:` shell command string. The line `-e INPUT_GITHUB_TOKEN=${{ secrets.HOMEBREW_GITHUB_TOKEN }} \` passes the secret value through YAML template substitution before the shell sees it, making it a script-injection risk regardless of the source context.

Locations:

- `.github/workflows/build.yml:35`

### unpinned-uses (severity: high)

Multiple `uses:` references in workflow files use mutable tags or branch names instead of full 40-character SHA commit hashes. Failing references in build.yml: `justintime50/composite-actions/bootstrap-python@v1` (lines 12, 18, 24, 52), `Homebrew/actions/setup-homebrew@master` (line 27), `actions/checkout@v6` (line 33), `codecov/codecov-action@v5` (line 55). Failing reference in update-stable.yml: `actions/checkout@v6` (line 12). Additionally, action.yml uses `runs.image: docker://justintime50/homebrew-releaser:4.0.0` — a mutable image tag rather than a SHA digest.

Locations:

- `.github/workflows/build.yml:12`
- `.github/workflows/build.yml:18`
- `.github/workflows/build.yml:24`
- `.github/workflows/build.yml:27`
- `.github/workflows/build.yml:33`
- `.github/workflows/build.yml:52`
- `.github/workflows/build.yml:55`
- `.github/workflows/update-stable.yml:12`
- `action.yml:76`

### missing-permissions (severity: medium)

Neither workflow file has a top-level `permissions:` key, and no individual job within either file defines a `permissions:` block. Without explicit permissions, workflows run with the default (potentially broad) token permissions.

Locations:

- `.github/workflows/build.yml:1`
- `.github/workflows/update-stable.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, unpinned-uses, missing-permissions

**Notes:**

Fixed all three findings:

1. **script-injection** (build.yml): Moved `${{ secrets.HOMEBREW_GITHUB_TOKEN }}` from the `run:` shell string into the step's `env:` block as `INPUT_GITHUB_TOKEN`, then referenced it as `"$INPUT_GITHUB_TOKEN"` in the docker run command.

2. **unpinned-uses**: Pinned all mutable action references to full 40-char SHAs:
   - `justintime50/composite-actions/bootstrap-python@v1` → `@87f43fea23a5883f3935d3f0e1163401329a59bb` (4 occurrences)
   - `Homebrew/actions/setup-homebrew@master` → `@8f3d1ec8a696b3b9d9a6c3696b6c73033cab69e4`
   - `actions/checkout@v6` → `@d23441a48e516b6c34aea4fa41551a30e30af803` (in both workflow files)
   - `codecov/codecov-action@v5` → `@0fb7174895f61a3b6b78fc075e0cd60383518dac`
   - `docker://justintime50/homebrew-releaser:4.0.0` in action.yml → pinned with `@sha256:000779a3ade1deca826eefc73fa3571b2fc5b7403d650b378de02505ccd4586f`, preserving the `docker://` scheme and `:4.0.0` tag inline.

3. **missing-permissions**: Added `permissions: {}` at the top level of both workflow files. For update-stable.yml, also added `permissions: contents: write` at the job level since it pushes git tags.

