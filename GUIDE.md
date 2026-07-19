# Running Claude Code agents past one-off tasks: a governance guide

Most "how to use Claude Code subagents" guides stop at *how to create an agent*. This one starts where the pain actually begins: what breaks when you run agents **unattended, in production, for months** — and the small set of rules that fix it.

It's written from a real solo operation that runs several content + commerce pipelines on Claude Code every day. Everything here is plain markdown and framework-free. No SDK, no orchestration engine, no lock-in.

## The core reframe: agents are commodities, management isn't

You can download packs of 100+ agents for free. If your automation is failing, it is almost never because you're missing a `code-reviewer.md`. It's failing for boring operational reasons:

- an agent reported success it didn't earn
- two agents fought over the same file
- one invented a number and put it in a report you almost sent
- a job died silently and nothing noticed for days
- you re-pasted the same context into every session because the agent had no memory

None of that is an *agent* problem. It's a **governance** problem. The rules below are the governance layer.

---

## Rule 1 — Delegation is a written contract, not a prompt

A prompt says *what to do this once*. A charter says *what this role is allowed to be*. That's the difference between a task and an employee.

A charter is a short markdown file that travels with the agent:

```markdown
# Charter: publish-lead
Mandate:      Publish 1 post/day to the blog queue.
Authority:    May edit drafts in /drafts. May call the publish endpoint.
Boundaries:   Never touches billing. Never publishes without a draft on disk.
Definition of done:
              A live public URL is returned AND written to
              /published/YYYY-MM-DD.json
Escalate to human when:
              - auth/login is required
              - a payment or new account is involved
              - the queue is empty
```

Why it works: the agent runs **free inside the fence** (it doesn't stop to ask permission for things clearly inside its authority) and **stops at the fence** (because the boundary is written down, not inferred). Agents behave measurably better when both the freedom and the fence are explicit.

## Rule 2 — Trust artifacts, not exit codes

The single most expensive failure mode in unattended automation: **the log says success, reality says nothing happened.**

The incident that taught me this: a scheduled job logged `success` for six days straight while publishing nothing. The session had expired on day one. The error scrolled off the top of the log. The wrapper only ever checked *"did the process run"* — never *"did a post actually go live."* I found out from a reader.

The fix is to verify **live external state**, every run:

```bash
# not: "did the script exit 0?"
# but: "does today's public artifact actually exist?"
curl -s https://your-site.com/ | grep -q "$(date +%Y-%m-%d)" \
  && echo ALIVE || echo "NO ARTIFACT TODAY — pipeline is DOWN"
```

Three signals, checked live:
1. **Session/credential valid *now*** — one cheap authenticated call, not "it worked yesterday."
2. **The source was actually called** — the platform's own record, not your client-side log.
3. **Today's artifact exists publicly** — fetch the URL and find it.

If the public artifact for today doesn't exist, the pipeline is DOWN — whatever the logs say. Exit codes lie; artifacts don't.

## Rule 3 — Facts only

Give every agent one non-negotiable rule: **no number without a live source; otherwise say `unverified`.**

Agents don't fabricate to be malicious — they fabricate to be *helpful*, filling a gap with something plausible. That's worse, because it reads as confident. The facts-only rule turns "the agent quietly made something up" into "the agent said `unverified`," which is a complete and honest answer.

This one rule is the difference between a dashboard you trust and a dashboard that flatters you.

## Rule 4 — One lead per lane

Give each pipeline exactly one owning agent. A blog lead, a shorts lead, a social lead.

- A **do-everything** agent re-derives context every session and drifts within a week (blog voice on social, social voice on the blog).
- A **narrow** agent builds deep, reusable context and stays coherent.
- **Failure stays contained** — a broken shorts pipeline can't take down your newsletter.

Twelve narrow agents beat one wide one, for the same reason job descriptions beat "just handle stuff."

## Rule 5 — Memory is infrastructure

Sessions are amnesiac. Decisions, preferences, and post-mortems must not be. Keep durable memory as **one fact per file** plus an index the agent reads at the start of every session:

- **one fact per file** — atomic memories can be corrected/deleted precisely; journals rot
- **write the *why*** — "we post at 19:00 *because* engagement peaked there in tests" survives re-evaluation; "post at 19:00" becomes cargo cult
- **delete wrong memories aggressively** — a stale memory is worse than none

Month two should feel like working with someone who was there in month one.

---

## Blockers are a first-class object

Some things an autonomous agent must never do: create accounts, make payments, accept legal terms, publish under a real name. Instead of failing silently — or worse, improvising — the agent records a **blocker** in a standing "human action needed" list and continues with everything inside the fence. You clear blockers in minutes; the machine keeps running around them.

## The human's job shrinks to four verbs

With this in place, your day compresses to: **set goals, clear blockers, review reports, decide pivots.** Everything else is staffed.

---

## What's in this repo (free)

- [`charters/AUTOPILOT_CHARTER.md`](charters/AUTOPILOT_CHARTER.md) — the full delegation-contract template
- [`agents/ops-monitor.md`](agents/ops-monitor.md) — the live-not-logs watchdog (Rule 2)
- [`agents/qa-reviewer.md`](agents/qa-reviewer.md) — the ship-gate reviewer (facts + leak-grep + link-resolves)

## The full system

The complete **AI Employee Company — Starter Kit** has all 12 agents (chief-of-staff, CMO, blog/shorts/social/newsletter/commerce leads, analytics lead, research scout, product lead, plus these two watchdogs), the brand-base file they all inherit, the persistent-memory system with copy-paste templates, launchd/cron scheduling recipes, and the 12 expensive lessons that shaped every rule above.

**→ [Get the kit](https://bluelove3.gumroad.com/l/ai-employee-company?utm_source=github&utm_medium=guide&utm_campaign=organic)** — launch price is a public ladder: starts at $19, rises $10 with every sale (the count is shown on the page, so it's verifiable). Earlier is genuinely cheaper — that's the whole mechanic, no fake countdown.

*If this guide was useful, a ⭐ helps other people running Claude Code find it.*
