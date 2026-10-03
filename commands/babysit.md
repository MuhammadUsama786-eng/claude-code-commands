---
description: Watch a PR's CI to green — on a coverage/lint/test failure, pull the job log, diagnose the exact cause, and propose the fix.
argument-hint: <PR number>
---

You are running **/babysit** on PR `$ARGUMENTS`. Get it to green with minimum
babysitting from the user.

## Steps
1. `gh pr checks <n>` — summarize pass/fail/pending. Identify the repo.
2. **Watch to completion** without polling in a tight loop: arm a background monitor (or a single `until`-style wait) that reports each check's terminal state, and re-arm on expiry. Don't send "are we done yet" spam.
3. When a check **fails**, fetch that job's log (`gh run view --log-failed` / `gh api .../jobs/<id>/logs`) and diagnose the *specific* cause:
   - **diff-cover / coverage**: find the exact `Missing lines N` and which changed file. Work out why (e.g. a branch no test reaches) and identify the smallest test that would cover it.
   - **lint / typecheck**: extract the exact rule + `file:line`.
   - **unit/integration test**: find the failing assertion and the real reason (don't just re-run hoping it flakes).
   - **Aggregated gates**: if CI has a "report"/"summary" job that enforces coverage across shards, green shards ≠ green CI — check that job explicitly.
4. Present the root cause plainly with `file:line`, and a concrete proposed fix.
5. **Do not push a fix without a yes.** On confirm, verify locally first (for coverage, prove the line is now covered with a local coverage/diff-cover run), then commit + push.
6. Keep watching until the whole PR is green (or a failure needs a human decision), then report.

Rules: verify every diagnosis against the log/code (don't guess), and never treat a red check as "probably flaky" without evidence.

<!-- Customize: add your repo's own gates under step 3 (e.g. "the X job is a 95% diff-cover gate"). The more specific, the better the diagnosis. -->
