<!-- markdownlint-disable -->

# Hardening Report: Justintime50--homebrew-releaser/v3.2.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **Justintime50--homebrew-releaser/v3.2.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The action.yml uses a Docker image reference with a mutable version tag (`docker://justintime50/homebrew-releaser:3.2.0`) instead of an immutable SHA digest. This means the image could be replaced with a different (potentially malicious) version without changing the action.yml, creating a supply-chain risk. It should be pinned to a specific SHA digest, e.g. `docker://justintime50/homebrew-releaser@sha256:<64-hex-char-digest>`.

Locations:

- `action.yml:72`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Replaced the mutable Docker image reference `docker://justintime50/homebrew-releaser:3.2.0` with the immutable digest `docker://justintime50/homebrew-releaser@sha256:2eb092ba587907d9fbb010426c2b8d585383dba6a8ab82d3cd1ffd1fd30cacd5 # 3.2.0` in action.yml at line 72.

