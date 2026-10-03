---
description: Principal-Engineer PR review — verify each claim against the code, severity-tiered, then (optionally) post inline via gh.
argument-hint: <PR number> [--post]
---

You are running **/review-pr** on `$ARGUMENTS` (a PR number; `--post` means also
publish the review as PR comments — otherwise just report it to the user).

Review at **Principal-Engineer depth** and — most importantly — **verify, don't
trust**: every claim (yours or a review bot's) is checked against the actual code
before you state it. Never write a number without re-deriving it.

## Steps
1. Fetch the PR: `gh pr view <n> --json title,body,headRefName,baseRefName,files` and `gh pr diff <n>`. Note the repo and target branch.
2. Read the **full diff** and open the changed files in context (not just the hunks) — a bug is often in what the diff *didn't* change around it.
3. Check against the repo's own rules (CLAUDE.md, contributing/style guides) and the failure modes this kind of codebase really has (data-fetching contracts, SSR/hydration, cache keys, auth boundaries, migrations, browser support, etc.).
4. For anything you'd flag, **prove it**: trace the exact code path, construct a concrete failing input → wrong output, and confirm it's reachable. Drop findings you can't substantiate.
5. Where relevant, sanity-check CI: is it green? does coverage pass? are failures real (not flaky)?

## Output — severity-tiered
Group findings as **🔴 Blocking · 🟠 Should-fix · 🟡 Nit**, each with:
- a one-line claim, the concrete failure scenario, and `file:line`;
- a suggested fix.
List genuine strengths briefly too. If nothing survives verification in a tier, say so.

## Posting (only with `--post`)
- Post inline review comments on the exact lines via `gh` (review threads, or `gh pr comment` for PR-level notes).
- Keep each comment self-contained (first line = the claim).
- Do **not** approve/merge; do not resolve others' threads. Report what you posted.

If not `--post`: present the review and offer to post it.
