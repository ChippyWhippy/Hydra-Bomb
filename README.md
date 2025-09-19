# CI PipeBomb

Self-modifying GitLab CI pipelines for chaos testing and resource consumption analysis.

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