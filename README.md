# CI Hydra

Self-modifying GitLab CI pipelines for chaos testing.

## What it does
- **NUKE (no knobs):** Appends a new job to `.gitlab-ci.yml`, commits, and pushes on every run — retriggers forever.
- **Controlled (with knobs):** Writes heads to `ci/generated.yml` with on/off and caps; cleaner and reversible.

## What it tests
- **Branch protections:** Can CI jobs push to this branch or are pushes blocked?
- **Repo write policy:** Are `CI_JOB_TOKEN`/deploy tokens permitted to modify the repo?
- **Push rules & governance:** Required reviews, signed commits, blocked paths (e.g., edits to `.gitlab-ci.yml`), commit message rules.
- **Runner/Platform limits:** Pipeline/queue growth, autoscaling, rate limits, retention/cleanup.

## Quick start
1. Use a **throwaway branch/project**.
2. Paste the **NUKE** or **Controlled** YAML (from this repo) as `.gitlab-ci.yml` and commit.
3. Ensure CI can **push back** to the same branch (or the loop will halt at `git push`).

## Stop / Cleanup
- Protect the branch or revoke CI push permission (prevents the next cycle).
- Cancel pipelines (UI/API) and pause runners if needed.
- Push a cleanup commit with **`[ci skip]`** that removes the hydra job (or `ci/generated.yml` for Controlled).

> If NUKE keeps looping, your protections allow CI to push.  
> If it stops at the first push, your controls are working.
