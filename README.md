# CI PipeBomb
[![Repo Stars](https://img.shields.io/github/stars/ChippyWhippy/ci-pipebomb?style=for-the-badge)](https://github.com/ChippyWhippy/ci-pipebomb/stargazers)
[![Forks](https://img.shields.io/github/forks/ChippyWhippy/ci-pipebomb?style=for-the-badge)](https://github.com/ChippyWhippy/ci-pipebomb/network/members)
[![Issues](https://img.shields.io/github/issues/ChippyWhippy/ci-pipebomb?style=for-the-badge)](https://github.com/ChippyWhippy/ci-pipebomb/issues)
[![Pull Requests](https://img.shields.io/github/issues-pr/ChippyWhippy/ci-pipebomb?style=for-the-badge)](https://github.com/ChippyWhippy/ci-pipebomb/pulls)
[![Closed PRs](https://img.shields.io/github/issues-pr-closed/ChippyWhippy/ci-pipebomb?style=for-the-badge)](https://github.com/ChippyWhippy/ci-pipebomb/pulls?q=is%3Apr+is%3Aclosed)
![Clones](https://img.shields.io/badge/dynamic/json?url=https://gist.githubusercontent.com/ChippyWhippy/ebaf7fa591cbbd446641f47406382aa0/raw/clone.json&label=Clones&query=$.count&logo=github&color=blue&cacheSeconds=1800&style=for-the-badge)
![Views](https://img.shields.io/badge/dynamic/json?url=https://gist.githubusercontent.com/ChippyWhippy/4d0e2963b0bce85abf8f9f3cdbe22863/raw/views.json&label=Views&query=$.count&logo=github&color=green&cacheSeconds=1800&style=for-the-badge)
![Last Commit](https://img.shields.io/github/last-commit/ChippyWhippy/ci-pipebomb?style=for-the-badge)
![Contributors](https://img.shields.io/github/contributors/ChippyWhippy/ci-pipebomb?style=for-the-badge)
![Commit Activity](https://img.shields.io/github/commit-activity/m/ChippyWhippy/ci-pipebomb?style=for-the-badge)


Self-modifying GitLab CI pipelines for chaos testing and resource consumption analysis.

---

> **DISCLAIMER: READ THIS BEFORE USING**
>
> CI PipeBomb is a tool for controlled chaos engineering and resource testing. It is designed for educational and research purposes only.

---

## Intended Use & Warning

This software is intended to be used by:

- Site Reliability Engineers (SREs) testing the scalability and limits of their own GitLab infrastructure.
- Platform teams validating branch protection rules, CI/CD job permissions, and runner autoscaling configurations.
- Security researchers understanding CI/CD security boundaries in a controlled, authorized environment.

---

## What This Is Not For

You **DO NOT** have permission to use this tool:

- On any GitLab instance you do not explicitly own or have written authorization to test.
- To disrupt services for others.
- For any malicious, disruptive, or unauthorized activity.

---

## For Research Only

This project is published for academic and instructional purposes. The author does **not** condone using this tool for:

- Denial-of-Service (DoS) attacks
- Vandalism
- Unauthorized access
- Any activity that violates GitLab's Terms of Service or acceptable use policy.

---

## You Assume All Risk

By using this software, you agree to the following:

- You are solely responsible for any and all outcomes resulting from its use.
- You understand this tool can rapidly consume compute resources, potentially incurring significant costs if used on a cloud platform.
- You understand this can lead to runner exhaustion, project suspension, or account termination by your GitLab provider.
- It is your responsibility to ensure you have permission to run this on the target infrastructure.

---

## Recommended Practices

- Use a throwaway project in a personal namespace, not a shared company group.
- Use a dedicated branch (e.g., `ci-chaos-testing`).
- Start with the CONTROLLED version and low limits before even considering the NUKE version.
- Monitor your pipeline quotas and runner metrics closely.
- Have a kill plan ready (branch protections, revoking permissions, pausing runners).

> This project is provided "AS IS", without warranty of any kind. The author is not liable for any damages or consequences resulting from the use of this software.

---

## What It Does

- **NUKE (no safety):**
  - Appends new explosive charges to `.gitlab-ci.yml`, commits, and pushes on every run — creating an exponential chain reaction.
- **Controlled (safety enabled):**
  - Writes detonations to `ci/detonation-sequence.yml` with configurable limits and throttling.

---

## What It Tests

- **Branch protections:** Can CI jobs push to protected branches?
- **Repo write policy:** Are `CI_JOB_TOKEN`/deploy tokens permitted to modify the repository?
- **Push rules & governance:** Required reviews, signed commits, blocked paths, commit message rules
- **Resource limits:** Pipeline/queue growth, autoscaling capabilities, rate limits, retention policies
- **Runner capacity:** How many concurrent jobs can your infrastructure handle?

---

## Quick Start

1. **Use a throwaway branch/project** — this will create exponential growth.
2. Paste either the NUKE or Controlled YAML as your `.gitlab-ci.yml`.
3. Ensure CI has permission to push to the same branch (or the chain reaction will stop).

---

## Safety Controls (Controlled Version)

- `PIPEBOMB_ENABLED`: Set to `"false"` to defuse the bomb
- `PIPEBOMB_MAX_EXPLOSIONS`: Cap the maximum number of detonations (`0 = unlimited`)
- `PIPEBOMB_FUSE_SECONDS`: Add delay between explosions to throttle resource consumption

---

## Emergency Stop Procedures

- **Immediate:** Protect the branch or revoke CI push permissions
- **Containment:** Cancel active pipelines via UI/API
- **Cleanup:** Push a commit with `[ci skip]` that removes the pipebomb job
- **Nuclear option:** Temporarily pause CI/CD runners

> If the NUKE version continues detonating, your CI permissions are too permissive.
> If it stops after the first explosion, your branch protections are working correctly.

---

## Warning

> This pipeline is designed to intentionally consume resources. Use with extreme caution in production environments. The NUKE version will quickly exhaust CI minutes and may trigger rate limiting or account suspension.
