# Scheduled Tasks with Continuity — Pattern

Scheduled tasks that carry context across runs, dedupe against previous output, and deliver only what changed.

---

## The pattern

```yaml
schedule: "0 12 * * *"
prompt: "Query analytics, compare to last report, highlight changes"
continuity: true
```

When `continuity: true`:
- The agent wakes up with its previous output injected as context
- It deduplicates — only reports what's new or changed
- It doesn't repeat last week's data unless it's relevant to a trend

---

## Deduplication rule

The agent should compare current output against previous output and report:

- **New items** that didn't appear before
- **Changed items** where a metric moved significantly
- **Stable items** only if they're at risk or notable
- **Omit** items that are unchanged and unremarkable

---

## Example: Daily analytics report

**Without continuity:**
Every day, the agent reports all numbers. The user sees the same 170 impressions, 24 clicks, 14.1% CTR every morning. Noise.

**With continuity:**
The agent sees yesterday's numbers, compares, and reports:
> "Impressions up 12% (170 → 191). CTR stable at 14%. New keyword entered top 3: 'project alpha tech reviews'."

---

## Example: Weekly research digest

**Without continuity:**
Every Monday, the agent reports on the same repositories. The user sees the same stars, same descriptions. Useless.

**With continuity:**
The agent compares against last week's report:
> "repo-A gained 847 stars this week (was 12.4k, now 13.2k). repo-B has new release v2.1."

---

## Implementation note

Continuity works by storing the agent's previous output in a file (e.g., `~/.hermes/cron/output/<job-id>.json`) and injecting it as context on the next run. The agent is told to compare and dedupe.

This is what makes scheduled agents useful rather than annoying.
