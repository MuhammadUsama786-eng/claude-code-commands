# Production Claude Code Commands (free)

Two slash commands I use every day on a large production codebase. They make Claude Code behave like a careful senior engineer instead of an eager intern.

- **`/babysit <PR>`** — watches a PR's CI, and when a check fails it reads the job log and tells you the *exact* cause (`file:line`, the missing coverage line, the failing assertion) plus the fix. No "probably flaky, re-run".
- **`/review-pr <PR> [--post]`** — a Principal-Engineer-level review where **every finding must be proven** against the code (a concrete failing input → wrong output) before it's reported. Severity-tiered: 🔴 Blocking · 🟠 Should-fix · 🟡 Nit.

## Install (30 seconds)
```bash
mkdir -p ~/.claude/commands
cp commands/*.md ~/.claude/commands/
```
Then in Claude Code: `/babysit 123` or `/review-pr 123`. Needs the GitHub CLI (`gh auth login`).

Tip: edit the `Customize` comment at the bottom of each file and name your repo's real CI gates. Specific beats generic.

## Want the full setup?
The **Production AI Engineering Kit** adds:
- `/ship` — isolated worktree → change → your full CI locally → PR only after you say yes
- `/triage` — error tracker + logs + traces + code → verified root cause
- **Autopilot** — agents keep working with the laptop lid closed, auto-continue when the network comes back (loop-safe), phone control and notifications — one-command installer
- **PR review-comment bot** — an hourly cloud agent that fixes, explains or flags review comments on your PRs (with a token diet that cut empty runs from ~30 agent turns to 4)
- **The Playbook** — real case studies: what the agents got wrong, and the exact fixes

👉 <GUMROAD_PRO_LINK>

MIT licensed. Built by Muhammad Usama.
