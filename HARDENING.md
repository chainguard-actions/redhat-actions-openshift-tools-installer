<!-- markdownlint-disable -->

# Hardening Report: redhat-actions--openshift-tools-installer/v3.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **redhat-actions--openshift-tools-installer/v3.1** was hardened automatically. 8 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All uses: references in ci.yml use mutable tag refs instead of pinned 40-character SHA commits: `actions/checkout@v7`, `actions/setup-node@v7`, `redhat-actions/common/bundle-verifier@v2`, `redhat-actions/common/action-io-generator@v2`. These can be silently updated to point to malicious code.

Locations:

- `.github/workflows/ci.yml:21`
- `.github/workflows/ci.yml:22`
- `.github/workflows/ci.yml:33`
- `.github/workflows/ci.yml:40`
- `.github/workflows/ci.yml:52`
- `.github/workflows/ci.yml:58`

### unpinned-uses (severity: high)

All uses: references in dependabot_rebuild.yml use mutable tag refs instead of pinned 40-character SHA commits: `actions/checkout@v7`, `actions/setup-node@v7`.

Locations:

- `.github/workflows/dependabot_rebuild.yml:18`
- `.github/workflows/dependabot_rebuild.yml:23`

### unpinned-uses (severity: high)

All uses: references in example_github.yml use mutable tag refs instead of pinned 40-character SHA commits: `actions/checkout@v7` (multiple occurrences).

Locations:

- `.github/workflows/example_github.yml:30`
- `.github/workflows/example_github.yml:65`
- `.github/workflows/example_github.yml:97`
- `.github/workflows/example_github.yml:117`
- `.github/workflows/example_github.yml:137`
- `.github/workflows/example_github.yml:156`

### unpinned-uses (severity: high)

All uses: references in example_mirror.yml use mutable tag refs instead of pinned 40-character SHA commits: `actions/checkout@v7` (multiple occurrences).

Locations:

- `.github/workflows/example_mirror.yml:30`
- `.github/workflows/example_mirror.yml:62`
- `.github/workflows/example_mirror.yml:91`
- `.github/workflows/example_mirror.yml:110`
- `.github/workflows/example_mirror.yml:128`
- `.github/workflows/example_mirror.yml:163`
- `.github/workflows/example_mirror.yml:198`
- `.github/workflows/example_mirror.yml:218`

### unpinned-uses (severity: high)

uses: references in link_checker.yml use mutable tag refs instead of pinned 40-character SHA commits: `actions/checkout@v7`, `tcort/github-action-markdown-link-check@v1.1.3`.

Locations:

- `.github/workflows/link_checker.yml:22`
- `.github/workflows/link_checker.yml:23`

### unpinned-uses (severity: high)

uses: references in security_scan.yml use mutable tag refs instead of pinned 40-character SHA commits: `actions/checkout@v7`, `actions/setup-node@v7`.

Locations:

- `.github/workflows/security_scan.yml:20`
- `.github/workflows/security_scan.yml:23`

### script-injection (severity: high)

Sub-rule (a): Multiple run: steps in example_github.yml directly interpolate a ${{ steps.* }} expression inside a shell command: `run: echo "${{ steps.install_clients.outputs.installed }}"`. The steps.*.outputs.* context is workflow-controllable and flows through YAML template substitution before the shell sees it, enabling script injection if the output contains shell metacharacters.

Locations:

- `.github/workflows/example_github.yml:44`
- `.github/workflows/example_github.yml:82`
- `.github/workflows/example_github.yml:108`
- `.github/workflows/example_github.yml:127`
- `.github/workflows/example_github.yml:148`
- `.github/workflows/example_github.yml:167`

### script-injection (severity: high)

Sub-rule (a): Multiple run: steps in example_mirror.yml directly interpolate a ${{ steps.* }} expression inside a shell command: `run: echo "${{ steps.install_clients.outputs.installed }}"`. The steps.*.outputs.* context is workflow-controllable and flows through YAML template substitution before the shell sees it, enabling script injection if the output contains shell metacharacters.

Locations:

- `.github/workflows/example_mirror.yml:44`
- `.github/workflows/example_mirror.yml:74`
- `.github/workflows/example_mirror.yml:100`
- `.github/workflows/example_mirror.yml:119`
- `.github/workflows/example_mirror.yml:138`
- `.github/workflows/example_mirror.yml:208`
- `.github/workflows/example_mirror.yml:228`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection

**Notes:**

Fixed all unpinned action references across 6 workflow files by resolving mutable tags to full 40-character SHA commits: actions/checkout@v7 → 3d3c42e5aac5ba805825da76410c181273ba90b1, actions/setup-node@v7 → 820762786026740c76f36085b0efc47a31fe5020, redhat-actions/common@v2 → 19c680ff95a52ee905481b54fc08d5c47788600c, tcort/github-action-markdown-link-check@v1.1.3 → e047c5b37f24ab722bbef1a27b6fab7f96bc4068. Fixed script injection in example_github.yml (6 occurrences) and example_mirror.yml (7 occurrences) by moving ${{ steps.install_clients.outputs.installed }} expressions into step-level env: blocks and referencing them as $INSTALLED in the shell commands.

