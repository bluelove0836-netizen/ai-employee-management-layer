# Autopilot Charter — [LINE NAME]

> The delegation contract between you (the owner) and your AI staff for one automation line.
> Copy this file per line you delegate (blog-autopilot, shorts-autopilot, launch-autopilot…).
> An agent operating under a charter acts freely INSIDE the fence and stops AT the fence.

---

## 0. Mandate

- **Owner instruction (date):** "[e.g., 2026-08-01: Run the blog for revenue. Don't wait for me.]"
- **Period:** [START] → [END] (then owner review: extend / pivot / stop)
- **Operator:** [agent name, e.g., blog-lead] — triggered by [schedule, e.g., daily 09:30 heartbeat]
- **Single goal:** [ONE measurable outcome, e.g., "first verified sale by END date" / "≥25 published posts with rising organic traffic"]

A charter with two goals is two charters. Split it.

## 1. Revenue/priority ladder

Ranked levers, so the operator never wonders what matters most today:
1. [Primary lever — e.g., "product X live and linked from every post"]
2. [Secondary — e.g., "ad approval: accumulate publish volume + traffic"]
3. [Tertiary — e.g., "consulting inquiries via About page CTA"]

## 2. Daily routine (what the operator does every run)

1. **Status check first** — live-not-logs (see `automation/STATUS_CHECKS.md`): did yesterday's outputs actually ship? Are sessions/tokens alive *now*? Record, don't assume.
2. **Guarantee today's output** — if the scheduled pipeline already ran, verify it; if it missed (weekend/holiday/failure), run it once manually.
3. **Queue management** — if the content/task queue is below [N] items, refill it using [method]. Never silently rewrite unused queue items.
4. **Performance-based adjustment** — only reorder priorities on sufficient sample size (e.g., ≥20 visitors over 7 days). Small samples stay observational.
5. **Revenue device check** — CTAs render, links resolve, checkout opens.
6. **Report** — append one line to the ops log (section 7) and notify the owner only on: new blocker, revenue event, or broken pipeline.

## 3. Autonomous powers (no approval needed)

- [e.g., Write/publish/edit posts within brand rules; edit the queue; improve copy/CTAs/internal links; build & deploy the site; collect stats]
- Daily output cap: [e.g., max 2 new posts/day — quality over volume]

## 4. Guardrails (absolute rules)

- **Money/number content: verified facts only.** No invented earnings, no estimated metrics presented as real. If there's no live data, the number does not appear.
- **Identity: faceless.** Never expose the owner's personal info, account credentials, or API keys. The public persona is [brand persona], not a person.
- **No account creation, no payments, no accepting paid terms** → record as a blocker (section 5) and continue other work.
- **Never delete or reschedule existing pipelines** — add alongside; removal requires owner sign-off.
- **Respect other workers.** On start, check for uncommitted changes by the owner or other automations; never overwrite, revert, or bulk-format work you didn't author today.
- **Published content is stable.** No retroactive rewrites/moves/deletions of public URLs except verified error fixes.
- **Rate limits:** max [3] deploys/day; failed publish retries [once]; then stop and report.
- **Success definition is external:** a publish counts only when the public URL returns 200 with the expected content. Local files and logs are not success.
- **Quality gate is not bypassable:** if generated content mismatches its queue brief (topic/title/angle), don't ship it.

## 5. Owner blockers (things only the human can do)

Standing list. Operator adds items with date + what exactly is needed; owner clears with one action.

- [ ] [e.g., Payment platform account creation — needed before lever #1]
- [ ] [e.g., Ad network review request — needed for lever #2]
- [x] [cleared example — keep history, it's institutional memory]

## 6. KPI line (recorded every run)

`published_total / yesterday_visitors / queue_remaining / subscribers / revenue_verified`

## 7. Report format (one JSON line per run, append-only)

```json
{"date":"YYYY-MM-DD","actions":["what was done"],"published":0,"queue_left":0,"yesterday_uv":0,"blockers":["waiting on X"],"note":"one-line assessment + next plan"}
```

## 8. Revenue records (separate file, strict)

Only on a **verified** payment/sale notification, append to `revenue.json`: `{date, source, item, amount, evidence}` and update the total. Estimates are never written. An empty file that's true beats a full file that isn't.

---

### Why each section exists (delete this block after customizing)

- **0 Mandate** kills scope drift: one goal, one period, one operator.
- **2 Routine** makes runs idempotent — safe to fire twice, safe to miss once.
- **3 vs 4** is the fence: generous inside, absolute outside. Agents behave measurably better when both are explicit.
- **5 Blockers** converts "the AI got stuck" into "the human has a 2-minute todo list."
- **7/8 split** keeps operational noise away from money truth. Auditable revenue is the point.
