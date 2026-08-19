<!-- markdownlint-disable -->

# Hardening Report: redhat-actions--openshift-tools-installer/v1.13.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **redhat-actions--openshift-tools-installer/v1.13.1** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All workflow files reference actions using mutable version tags (e.g. @v1, @v2) instead of pinned 40-character SHA commits. This exposes the workflow to supply-chain attacks if the referenced tag is moved or the upstream repository is compromised.

Affected references:
- ci.yml: actions/checkout@v2 (×3), redhat-actions/common/bundle-verifier@v1, redhat-actions/common/action-io-generator@v1
- example_github.yml: actions/checkout@v2 (×6)
- example_mirror.yml: actions/checkout@v2 (×6)
- link_checker.yml: actions/checkout@v2, gaurav-nelson/github-action-markdown-link-check@v1
- security_scan.yml: actions/checkout@v2, actions/setup-node@v2, redhat-actions/openshift-tools-installer@v1, redhat-actions/crda@v1

Locations:

- `.github/workflows/ci.yml:15`
- `.github/workflows/ci.yml:26`
- `.github/workflows/ci.yml:32`
- `.github/workflows/ci.yml:39`
- `.github/workflows/ci.yml:44`
- `.github/workflows/example_github.yml:23`
- `.github/workflows/example_mirror.yml:23`
- `.github/workflows/link_checker.yml:18`
- `.github/workflows/link_checker.yml:19`
- `.github/workflows/security_scan.yml:15`
- `.github/workflows/security_scan.yml:18`
- `.github/workflows/security_scan.yml:23`
- `.github/workflows/security_scan.yml:29`

### missing-permissions (severity: medium)

None of the workflow files define a top-level `permissions:` key, and no individual job defines its own `permissions:` block. Without explicit permissions, workflows run with the default (potentially write) token permissions, violating the principle of least privilege.

Locations:

- `.github/workflows/ci.yml:1`
- `.github/workflows/example_github.yml:1`
- `.github/workflows/example_mirror.yml:1`
- `.github/workflows/link_checker.yml:1`
- `.github/workflows/security_scan.yml:1`

### script-injection (severity: high)

Sub-rule (a): A `${{ steps.install_clients.outputs.installed }}` expression is directly interpolated inside a `run:` shell command string in multiple jobs. The expression `${{ steps.*.outputs.* }}` is substituted by the Actions template engine before the shell sees it, allowing a malicious value in the output to inject arbitrary shell commands.

Offending line in each job: `run: echo "${{ steps.install_clients.outputs.installed }}"`

This pattern appears 6 times in example_github.yml and 6 times in example_mirror.yml. Fix: move the value into an env var and double-quote the expansion, e.g.:
  env:
    INSTALLED: ${{ steps.install_clients.outputs.installed }}
  run: echo "$INSTALLED"

Locations:

- `.github/workflows/example_github.yml:45`
- `.github/workflows/example_github.yml:84`
- `.github/workflows/example_github.yml:113`
- `.github/workflows/example_github.yml:135`
- `.github/workflows/example_github.yml:157`
- `.github/workflows/example_github.yml:178`
- `.github/workflows/example_mirror.yml:45`
- `.github/workflows/example_mirror.yml:83`
- `.github/workflows/example_mirror.yml:118`
- `.github/workflows/example_mirror.yml:143`
- `.github/workflows/example_mirror.yml:166`
- `.github/workflows/example_mirror.yml:192`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions, script-injection

**Notes:**

Fixed all three findings across five workflow files:

1. unpinned-uses: Pinned all action references to full 40-char commit SHAs with original tags as comments. Actions pinned: actions/checkout@v2 (SHA: 0717577d), actions/setup-node@v2 (SHA: 7c12f801), redhat-actions/common@v1 (SHA: a7e9fd36), gaurav-nelson/github-action-markdown-link-check@v1 (SHA: 5c5dfc0a), redhat-actions/openshift-tools-installer@v1 (SHA: 144527c7), redhat-actions/crda@v1 (SHA: 6310ee94).

2. missing-permissions: Added `permissions: contents: read` top-level block to all five workflow files (ci.yml, example_github.yml, example_mirror.yml, link_checker.yml, security_scan.yml).

3. script-injection: Fixed all 12 occurrences of direct ${{ steps.install_clients.outputs.installed }} interpolation in run: commands across example_github.yml (6 occurrences) and example_mirror.yml (6 occurrences) by moving the expression into an env: block (INSTALLED: ${{ steps.install_clients.outputs.installed }}) and referencing it as $INSTALLED in the shell script.

