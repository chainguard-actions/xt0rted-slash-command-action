<!-- markdownlint-disable -->

# Hardening Report: xt0rted--slash-command-action/v2.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **xt0rted--slash-command-action/v2.0.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference actions using mutable tags or branch names instead of immutable 40-character commit SHAs. This exposes the workflow to supply-chain attacks if the referenced tag or branch is moved to a malicious commit.

.github/workflows/ci.yml:
  - uses: actions/checkout@v3.1.0  (tag)
  - uses: actions/setup-node@v3.5.1  (tag)
  - uses: codecov/codecov-action@v3.1.1  (tag)

.github/workflows/codeql-analysis.yml:
  - uses: actions/checkout@v3.1.0  (tag)
  - uses: github/codeql-action/init@v2  (tag)
  - uses: github/codeql-action/autobuild@v2  (tag)
  - uses: github/codeql-action/analyze@v2  (tag)

.github/workflows/dependabot-auto-merge.yml:
  - uses: xt0rted/.github/.github/workflows/dependabot-auto-merge.yml@main  (branch)

.github/workflows/fixup-commits.yml:
  - uses: xt0rted/.github/.github/workflows/fixup-commits.yml@main  (branch)

Locations:

- `.github/workflows/ci.yml:27`
- `.github/workflows/ci.yml:31`
- `.github/workflows/ci.yml:40`
- `.github/workflows/codeql-analysis.yml:30`
- `.github/workflows/codeql-analysis.yml:33`
- `.github/workflows/codeql-analysis.yml:37`
- `.github/workflows/codeql-analysis.yml:40`
- `.github/workflows/dependabot-auto-merge.yml:9`
- `.github/workflows/fixup-commits.yml:8`

### missing-permissions (severity: medium)

The workflow file ci.yml has no top-level `permissions:` key and its only job (`build`) also has no job-level `permissions:` key. Without explicit permissions, the GITHUB_TOKEN is granted its default (potentially broad) permissions, which may include write access to repository contents. All permissions should be explicitly scoped to the minimum required.

Locations:

- `.github/workflows/ci.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all unpinned action references across 4 workflow files by replacing mutable tags/branches with full 40-character commit SHAs (preserving original refs as comments). Specifically: actions/checkout@v3.1.0 → SHA 93ea575..., actions/setup-node@v3.5.1 → SHA 8c91899..., codecov/codecov-action@v3.1.1 → SHA d9f34f8..., github/codeql-action/{init,autobuild,analyze}@v2 → SHA b8d3b6e..., xt0rted/.github@main → SHA cb2d8c6... (used for both dependabot-auto-merge.yml and fixup-commits.yml). Also added a top-level `permissions: contents: read` block to ci.yml to satisfy the missing-permissions finding with the minimum required access.

