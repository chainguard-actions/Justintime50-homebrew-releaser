<!-- markdownlint-disable -->

# Hardening Report: Justintime50--homebrew-releaser/v3.1.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Justintime50--homebrew-releaser/v3.1.0** was hardened automatically. 1 finding(s) were identified and resolved across 3 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The action uses a Docker image pinned to a mutable version tag (`docker://justintime50/homebrew-releaser:3.1.0`) instead of an immutable SHA digest. This means the image could be replaced with a different (potentially malicious) version without changing the action.yml, creating a supply-chain risk. The `image:` reference should use a SHA digest, e.g. `docker://justintime50/homebrew-releaser@sha256:<64-hex-char-digest>`.

Locations:

- `action.yml:72`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Pinned the Docker image reference in action.yml from `docker://justintime50/homebrew-releaser:3.1.0` to `docker://justintime50/homebrew-releaser:3.1.0@sha256:386d4f0ffd625be95306175caa6de531e75467073e491d11f9abcfa0f3214c4c`. The `docker://` scheme and `:3.1.0` tag are preserved inline as required.

### Iteration 2

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all unpinned action references in both workflow files by resolving them to full 40-character commit SHAs (preserving original tags as comments): actions/checkout@v6 → df4cb1c069e1874edd31b4311f1884172cec0e10, extractions/setup-just@v3 → f8a3cce218d9f83db3a2ecd90e41ac3de6cdfd9b, actions/setup-python@v6 → ece7cb06caefa5fff74198d8649806c4678c61a1, Homebrew/actions/setup-homebrew@master → df4b09108a1de9d6f995fe68f302b3f68bd6d2ef, codecov/codecov-action@v5 → 0fb7174895f61a3b6b78fc075e0cd60383518dac. Added top-level `permissions: {}` to both workflow files and job-level `permissions: contents: read` to all jobs in build.yml. For update-stable.yml, the update-stable job requires `contents: write` to push git tags.

### Iteration 3

**Fixes applied:** script-injection

**Notes:**

Fixed script injection in hardened/action/.github/workflows/build.yml (docker job, line 57). Moved `${{ secrets.HOMEBREW_GITHUB_TOKEN }}` from the `run:` shell command string into an `env:` block as `INPUT_GITHUB_TOKEN`, and updated the docker run command to reference it as `"$INPUT_GITHUB_TOKEN"` (double-quoted environment variable). This eliminates the risk of the secret value being interpreted as shell syntax before the shell parses the command.

