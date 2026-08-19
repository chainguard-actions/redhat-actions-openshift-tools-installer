<!-- markdownlint-disable -->

# Hardening Report: redhat-actions--openshift-tools-installer/v1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **redhat-actions--openshift-tools-installer/v1** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All `uses:` references across every workflow file are pinned to mutable tags or version strings (e.g. @v1, @v2) rather than immutable 40-character commit SHAs. This exposes the workflows to supply-chain attacks if any referenced action is compromised or its tag is moved. Affected references include: actions/checkout@v2, actions/setup-node@v2, redhat-actions/common/bundle-verifier@v1, redhat-actions/common/action-io-generator@v1, gaurav-nelson/github-action-markdown-link-check@v1, redhat-actions/openshift-tools-installer@v1, redhat-actions/crda@v1.

Locations:

- `.github/workflows/ci.yml:15`
- `.github/workflows/ci.yml:24`
- `.github/workflows/ci.yml:30`
- `.github/workflows/ci.yml:38`
- `.github/workflows/ci.yml:43`
- `.github/workflows/example_github.yml:23`
- `.github/workflows/example_mirror.yml:23`
- `.github/workflows/link_checker.yml:18`
- `.github/workflows/link_checker.yml:19`
- `.github/workflows/security_scan.yml:15`
- `.github/workflows/security_scan.yml:18`
- `.github/workflows/security_scan.yml:22`
- `.github/workflows/security_scan.yml:30`

### missing-permissions (severity: medium)

None of the workflow files define a top-level `permissions:` key, and no individual job within any workflow defines a `permissions:` block. Without explicit permissions, workflows run with the default (potentially broad) GITHUB_TOKEN permissions, violating the principle of least privilege.

Locations:

- `.github/workflows/ci.yml:1`
- `.github/workflows/example_github.yml:1`
- `.github/workflows/example_mirror.yml:1`
- `.github/workflows/link_checker.yml:1`
- `.github/workflows/security_scan.yml:1`

### script-injection (severity: high)

Sub-rule (a): Multiple `run:` steps directly interpolate `${{ steps.install_clients.outputs.installed }}` into a shell command string: `run: echo "${{ steps.install_clients.outputs.installed }}"`.

The `steps.*.outputs.*` context is a workflow-controllable value that flows through YAML template substitution before the shell ever sees it. If the action's output contains shell metacharacters or newlines, this can lead to command injection. The value should be passed via an `env:` variable and the shell expansion double-quoted instead.

Locations:

- `.github/workflows/example_github.yml:41`
- `.github/workflows/example_github.yml:79`
- `.github/workflows/example_github.yml:110`
- `.github/workflows/example_github.yml:135`
- `.github/workflows/example_github.yml:158`
- `.github/workflows/example_github.yml:181`
- `.github/workflows/example_mirror.yml:42`
- `.github/workflows/example_mirror.yml:79`
- `.github/workflows/example_mirror.yml:110`
- `.github/workflows/example_mirror.yml:131`
- `.github/workflows/example_mirror.yml:152`
- `.github/workflows/example_mirror.yml:173`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions, script-injection

**Notes:**

Fixed all 5 workflow files: (1) Pinned all uses: references to full 40-char commit SHAs with tag comments preserved — actions/checkout@v2, actions/setup-node@v2, redhat-actions/common/bundle-verifier@v1, redhat-actions/common/action-io-generator@v1, gaurav-nelson/github-action-markdown-link-check@v1, redhat-actions/openshift-tools-installer@v1, redhat-actions/crda@v1. (2) Added top-level `permissions: {}` to all 5 workflow files and job-level `permissions: contents: read` to each job. (3) Fixed all 12 script injection occurrences in example_github.yml and example_mirror.yml by moving `${{ steps.install_clients.outputs.installed }}` into an `env:` block as INSTALLED and referencing it as `$INSTALLED` in the shell.

