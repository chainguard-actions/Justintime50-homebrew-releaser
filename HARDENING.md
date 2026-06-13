<!-- markdownlint-disable -->

# Hardening Report: Justintime50--homebrew-releaser/v3.0.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **Justintime50--homebrew-releaser/v3.0.1** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The action.yml uses a Docker image reference with a mutable version tag instead of an immutable SHA digest. `image: docker://justintime50/homebrew-releaser:3.0.1` uses the tag `3.0.1`, which can be silently moved to point to a different (potentially malicious) image. It should be pinned to a SHA256 digest, e.g. `image: docker://justintime50/homebrew-releaser@sha256:<64-hex-char-digest> # 3.0.1`.

Locations:

- `action.yml:63`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Replaced the mutable Docker image tag `docker://justintime50/homebrew-releaser:3.0.1` with the immutable SHA256 digest `docker://justintime50/homebrew-releaser@sha256:dd98e724640e3fc765fa28b491422465c3ad962f7e41e9f1e3c61621e8fe462b # 3.0.1` in action.yml line 63.

