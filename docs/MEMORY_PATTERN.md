# Memory System — Pattern

Conversation is temporary. Memory is persistent.

Most agents start every session from scratch. This pattern gives them continuity.

---

## Core principle

**Conversation** = temporary context for the current session
**Memory** = durable facts that survive across sessions
**Project files** = implementation truth
**OS config** = operating truth

Memory should contain only what improves future decisions. Not everything that appeared in conversation belongs in memory.

---

## Memory locations

### Global memory
Useful across many projects and sessions:
- User preferences (communication style, approval habits)
- Recurring development patterns
- Stable company standards
- Confirmed tool capabilities

### Project memory
Belongs to one project:
- Architecture decisions
- Client-specific requirements
- Database schema
- Deployment configuration
- Unresolved blockers

---

## Admission test

Before persisting anything, ask:

1. **Is it durable?** Will this matter in future sessions?
2. **Is it useful?** Will remembering it reduce repeated work?
3. **Is it verified?** Is this known rather than guessed?
4. **Is this the correct place?** Should it be in project docs instead?
5. **Is it already stored?** Avoid duplication.

If the answer is weak, don't persist it.

---

## Confidence levels

| Level | Meaning | Action |
|---|---|---|
| **CONFIRMED** | Explicitly stated or directly verified | Store as fact |
| **OBSERVED** | Derived from repeated verified behavior | Store with caveat |
| **TENTATIVE** | Potentially useful but unconfirmed | Rarely store globally |

Never convert an inference into a confirmed memory.

---

## Source-of-truth hierarchy

When memory conflicts with reality:

1. Directly verified current state wins
2. Actual project files / runtime evidence
3. Current OS policy
4. Project documentation
5. Current persistent memory
6. Old conversation assumptions

Memory is never authoritative enough to override current evidence.

---

## What belongs in user profile

- Preferred communication style
- Preferred level of detail
- Preferred workflow and approval patterns
- Recurring technical preferences
- Recurring business preferences

## What does NOT belong

- Casual conversation or one-time requests
- Temporary emotions or debugging details
- Speculative assumptions
- Passwords, credentials, API keys
- Temporary project requirements

One-time instruction ≠ permanent preference.

---

## Memory contradictions

If two memory entries conflict:

1. Inspect current evidence
2. Identify the newest verified fact
3. Ask the user if necessary
4. Remove stale information
5. Preserve one canonical truth

Do not silently choose one. Do not preserve contradictory versions.

---

## Memory compression

Periodically:
- Merge duplicate facts
- Remove obsolete information
- Shorten verbose entries
- Move project-specific facts into projects
- Keep only durable conclusions

Memory should become cleaner over time, not more contradictory.

---

## Memory expiration

Some memories naturally become stale:
- Pricing, versions, subscriptions
- Project status, infrastructure
- Team structure, provider limits

For time-sensitive facts, include: `Last verified: YYYY-MM-DD`

Do not assume time-sensitive memories remain current forever.

---

## Secrets — hard rule

**NEVER persist:**
- Passwords, API keys, tokens
- OAuth secrets, SSH keys
- Session cookies, payment credentials

**Allowed:**
- "API_KEY is configured in the environment"

If a user accidentally exposes a credential:
- Do not copy it to memory
- Recommend rotation
- Reference only the credential name later
