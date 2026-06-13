<!-- markdownlint-disable -->

# Hardening Report: Justintime50--homebrew-releaser/v3.1.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **Justintime50--homebrew-releaser/v3.1.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The action.yml uses a Docker image reference with a mutable version tag (`docker://justintime50/homebrew-releaser:3.1.0`) instead of an immutable SHA digest. This means the action could silently pull a different (potentially malicious) image if the tag is overwritten on Docker Hub. The `image:` field should reference a SHA digest, e.g. `docker://justintime50/homebrew-releaser@sha256:<64-hex-char-digest>`.

Locations:

- `action.yml:68`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Replaced the mutable Docker image tag `docker://justintime50/homebrew-releaser:3.1.0` with the immutable SHA256 digest `docker://justintime50/homebrew-releaser@sha256:386d4f0ffd625be95306175caa6de531e75467073e491d11f9abcfa0f3214c4c # 3.1.0` in action.yml at line 68.

