<!-- markdownlint-disable -->

# Hardening Report: Justintime50--homebrew-releaser/v3.0.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Justintime50--homebrew-releaser/v3.0.1** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple `uses:` references and the Docker image reference use mutable tags/branches instead of pinned full SHA commits or SHA digests, making the action vulnerable to supply-chain attacks.

In action.yml:
- `image: docker://justintime50/homebrew-releaser:3.0.1` uses a mutable tag instead of a SHA digest (e.g. `@sha256:<64-hex-char-digest>`).

In .github/workflows/build.yml:
- `actions/checkout@v5` (line 13)
- `extractions/setup-just@v3` (line 14)
- `actions/setup-python@v6` (line 15)
- `actions/checkout@v5` (line 22)
- `extractions/setup-just@v3` (line 23)
- `actions/setup-python@v6` (line 24)
- `actions/checkout@v5` (line 31)
- `extractions/setup-just@v3` (line 32)
- `Homebrew/actions/setup-homebrew@master` (line 33)
- `actions/checkout@v5` (line 47)
- `extractions/setup-just@v3` (line 48)
- `actions/setup-python@v6` (line 49)
- `codecov/codecov-action@v5` (line 51)

In .github/workflows/update-stable.yml:
- `actions/checkout@v5` (line 10)

Locations:

- `action.yml:63`
- `.github/workflows/build.yml:13`
- `.github/workflows/build.yml:14`
- `.github/workflows/build.yml:15`
- `.github/workflows/build.yml:22`
- `.github/workflows/build.yml:23`
- `.github/workflows/build.yml:24`
- `.github/workflows/build.yml:31`
- `.github/workflows/build.yml:32`
- `.github/workflows/build.yml:33`
- `.github/workflows/build.yml:47`
- `.github/workflows/build.yml:48`
- `.github/workflows/build.yml:49`
- `.github/workflows/build.yml:51`
- `.github/workflows/update-stable.yml:10`

### script-injection (severity: high)

Sub-rule (a): A `${{ }}` expression is interpolated directly inside a `run:` shell command string. In the `docker` job of build.yml, the `docker run` command includes `-e INPUT_GITHUB_TOKEN=${{ secrets.GITHUB_TOKEN }}` directly in the shell script. Any `${{ ... }}` expression inside a `run:` block flows through YAML template substitution before the shell processes it, bypassing shell quoting protections. The value should be passed via an `env:` block and referenced as `$INPUT_GITHUB_TOKEN` instead.

Locations:

- `.github/workflows/build.yml:43`

### missing-permissions (severity: medium)

Neither workflow file has a top-level `permissions:` key, and no individual jobs define their own `permissions:` block. Without explicit permissions, workflows run with the default (potentially broad) token permissions. Both files should declare minimal required permissions (e.g. `permissions: read-all` at the top level, or specific scopes per job).

Locations:

- `.github/workflows/build.yml:1`
- `.github/workflows/update-stable.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection, missing-permissions

**Notes:**

1. action.yml: Pinned Docker image `justintime50/homebrew-releaser:3.0.1` with SHA digest `sha256:dd98e724640e3fc765fa28b491422465c3ad962f7e41e9f1e3c61621e8fe462b`, preserving the `docker://` scheme and tag inline.
2. build.yml: Pinned all 6 unique action references to full commit SHAs (actions/checkout@v5, extractions/setup-just@v3, actions/setup-python@v6, Homebrew/actions/setup-homebrew@master, codecov/codecov-action@v5). Fixed script injection in the docker job by moving `${{ secrets.GITHUB_TOKEN }}` into an `env:` block and referencing it as `$INPUT_GITHUB_TOKEN` in the shell. Added `permissions: contents: read` at the top level.
3. update-stable.yml: Pinned `actions/checkout@v5` to its full SHA. Added `permissions: contents: write` (required to push git tags).

