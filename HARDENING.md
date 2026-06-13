<!-- markdownlint-disable -->

# Hardening Report: Justintime50--homebrew-releaser/v3.3.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **Justintime50--homebrew-releaser/v3.3.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The action.yml uses a Docker image reference with a mutable version tag instead of an immutable SHA digest. `image: docker://justintime50/homebrew-releaser:3.2.1` uses the tag `3.2.1`, which can be overwritten on Docker Hub at any time, enabling a supply-chain attack. It should be pinned to a specific SHA digest, e.g. `image: docker://justintime50/homebrew-releaser@sha256:<64-hex-char-digest> # 3.2.1`.

Locations:

- `action.yml:76`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Replaced the mutable Docker image tag `docker://justintime50/homebrew-releaser:3.2.1` with the immutable digest `docker://justintime50/homebrew-releaser@sha256:54161ddccdcb64d16209a6a6b0c9512f71ff106542b45e6f5ac03108d11723b7 # 3.2.1` in action.yml at line 76.

