<!-- markdownlint-disable -->

# Hardening Report: Justintime50--homebrew-releaser/v3.2.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Justintime50--homebrew-releaser/v3.2.0** was hardened automatically. 6 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple `uses:` references in build.yml use mutable tag or branch refs instead of full 40-character SHA commit pins, making the workflow vulnerable to supply-chain attacks if those tags are moved. Failing references: `actions/checkout@v6`, `extractions/setup-just@v3`, `actions/setup-python@v6`, `Homebrew/actions/setup-homebrew@master`, `codecov/codecov-action@v5`.

Locations:

- `.github/workflows/build.yml:13`
- `.github/workflows/build.yml:14`
- `.github/workflows/build.yml:15`
- `.github/workflows/build.yml:21`
- `.github/workflows/build.yml:22`
- `.github/workflows/build.yml:23`
- `.github/workflows/build.yml:29`
- `.github/workflows/build.yml:30`
- `.github/workflows/build.yml:31`
- `.github/workflows/build.yml:38`
- `.github/workflows/build.yml:52`
- `.github/workflows/build.yml:53`
- `.github/workflows/build.yml:54`
- `.github/workflows/build.yml:58`

### unpinned-uses (severity: high)

`uses:` reference in update-stable.yml uses a mutable tag ref instead of a full 40-character SHA commit pin. Failing reference: `actions/checkout@v6`.

Locations:

- `.github/workflows/update-stable.yml:11`

### unpinned-uses (severity: high)

The `runs.image:` field in action.yml references a Docker image using a mutable version tag (`docker://justintime50/homebrew-releaser:3.2.0`) instead of an immutable SHA digest (e.g. `docker://justintime50/homebrew-releaser@sha256:<digest>`). This allows the image to be silently replaced with a malicious version.

Locations:

- `action.yml:72`

### script-injection (severity: high)

Sub-rule (a): A `${{ ... }}` expression is interpolated directly inside a `run:` shell command string. The expression `${{ secrets.HOMEBREW_GITHUB_TOKEN }}` is embedded inline in the `docker run` shell command (`-e INPUT_GITHUB_TOKEN=${{ secrets.HOMEBREW_GITHUB_TOKEN }}`). Any `${{ ... }}` expression inside a `run:` block is subject to YAML template substitution before the shell processes it, making it a script-injection risk. The value should be passed via an `env:` block and referenced as `$INPUT_GITHUB_TOKEN` (double-quoted) in the shell command.

Locations:

- `.github/workflows/build.yml:44`

### missing-permissions (severity: medium)

The workflow file has no top-level `permissions:` key and none of its jobs define a `permissions:` block. Without explicit permissions, the workflow inherits the default repository permissions (which may include broad write access), violating the principle of least privilege.

Locations:

- `.github/workflows/build.yml:1`

### missing-permissions (severity: medium)

The workflow file has no top-level `permissions:` key and none of its jobs define a `permissions:` block. The `update-stable` job pushes git tags and requires `contents: write`, but no permissions are declared, leaving the workflow with implicit defaults.

Locations:

- `.github/workflows/update-stable.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection, missing-permissions

**Notes:**

Fixed all 6 findings across 3 files:

1. build.yml - Pinned all action references to full SHA commits:
   - actions/checkout@v6 → @df4cb1c069e1874edd31b4311f1884172cec0e10
   - extractions/setup-just@v3 → @f8a3cce218d9f83db3a2ecd90e41ac3de6cdfd9b
   - actions/setup-python@v6 → @ece7cb06caefa5fff74198d8649806c4678c61a1
   - Homebrew/actions/setup-homebrew@master → @df4b09108a1de9d6f995fe68f302b3f68bd6d2ef
   - codecov/codecov-action@v5 → @0fb7174895f61a3b6b78fc075e0cd60383518dac
   - Fixed script injection: moved ${{ secrets.HOMEBREW_GITHUB_TOKEN }} to env block, referenced as "$INPUT_GITHUB_TOKEN" in shell
   - Added top-level permissions: contents: read

2. update-stable.yml - Pinned actions/checkout@v6 → @df4cb1c069e1874edd31b4311f1884172cec0e10; added permissions: contents: write (required for git push)

3. action.yml - Pinned Docker image to immutable digest: docker://justintime50/homebrew-releaser:3.2.0@sha256:2eb092ba587907d9fbb010426c2b8d585383dba6a8ab82d3cd1ffd1fd30cacd5 (preserving docker:// scheme and tag)

