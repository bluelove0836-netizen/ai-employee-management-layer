# ai-employee-management-layer

**The management layer for AI "employees" — free sample.**

> Everyone hands you 100 free Claude Code subagents. Nobody tells you how to stop them from quietly wrecking your business. This repo is the *management* side.

The agents are the easy part now — there are free packs of 100+. What actually breaks a multi-agent setup is management: an agent reports success it didn't earn, two agents fight over the same file, one invents a number you almost ship, a job dies silently for a week. None of that is an agent problem. It's a **governance** problem.

This repo is the delegation contract and the two watchdog agents from a real solo operation that runs multiple content + commerce pipelines daily on Claude Code.

## The 4 rules this encodes

1. **Delegation is a written contract, not a prompt.** An agent runs free *inside* a fence — because the fence is written down (goal, authority, boundaries, definition-of-done, escalate-to-human).
2. **Facts only.** No agent may state a number without a live source; otherwise it says `unverified`. Your dashboard stops flattering you.
3. **Trust artifacts, not exit codes.** A pipeline is "alive" only if today's public output actually exists — not because a script exited `0`. (This rule was born from a scheduler that logged `success` for 6 days while publishing nothing.)
4. **Blockers are a checklist, not a chat message.** Things only a human may do get surfaced in every report until cleared.

```
owner ── chief-of-staff ── CMO ── blog / shorts / social / newsletter / commerce leads
                              ├── analytics-lead   (facts only)
                              ├── qa-reviewer      (last gate)  ← in this repo
                              └── ops-monitor      (live-not-logs) ← in this repo
```

## What's here (3 files, plain markdown, zero dependencies)

| File | What it is |
|---|---|
| `charters/AUTOPILOT_CHARTER.md` | The one-page **delegation contract** you sign with any agent that runs autonomously: goal, daily routine, autonomous powers, absolute guardrails, human-only blockers, report format. The single highest-leverage file in the whole system. |
| `agents/ops-monitor.md` | Verifies pipelines are **ALIVE** using live external state — public artifacts, working auth *right now*, loaded schedulers — never log files. Born from a scheduler that logged "success" for 6 days of publishing nothing. |
| `agents/qa-reviewer.md` | The last gate before anything ships: truth checks, personal-data leak grep, link resolution, brand and platform rules. BLOCK verdicts cannot be overridden by other agents. |

## Install (2 minutes)

```bash
mkdir -p .claude/agents charters
cp agents/*.md .claude/agents/
cp charters/AUTOPILOT_CHARTER.md charters/
# fill in the bracketed placeholders in the charter
```

Claude Code picks up `.claude/agents/*.md` automatically.

## The four rules these files encode

1. **Delegation is a written contract** — the agent acts freely inside the fence and stops at the fence, because the fence is written down.
2. **Facts only** — a number without a live source is reported as "unverified." No exceptions.
3. **Trust artifacts, not exit codes** — a pipeline is alive only if today's public output exists right now.
4. **Blockers are a checklist, not a chat message** — things only a human can do get surfaced in every report until cleared.

## Want the whole company?

The full **AI Employee Company — Starter Kit** has all 12 agents (chief-of-staff, CMO, blog/shorts/social/newsletter/commerce/analytics leads, research scout, product lead + these two), the brand base file they all inherit, the persistent memory system, launchd/cron scheduling recipes, and the 12 expensive lessons that shaped the rules.

**→ [Get the kit](https://bluelove3.gumroad.com/l/ai-employee-company?utm_source=github&utm_medium=teaser&utm_campaign=launch)** — launch price ladder: starts at $19 and rises $10 with every sale (sales count is public on the listing, so the ladder is verifiable).

---
License: MIT for the files in this repo. No personal data inside; everything generalized from a real operation.
