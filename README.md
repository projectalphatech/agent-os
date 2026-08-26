# agent-os

> How to coordinate specialized AI agents with structured delegation, persistent memory, and verification gates.

## Why this exists

Most multi-agent frameworks treat agents as generic workers.

LangGraph, CrewAI, AutoGen — they give you *orchestration*. They don't give you:

- **Specialized roles** with distinct toolsets and reporting formats
- **Structured delegation briefs** — not just "do this", but "do this, verify with evidence, report in this format"
- **Persistent memory** — cross-session continuity that survives context compaction
- **Verification gates** — agents must prove completion, not claim it

This repo shows how we do it.

---

## The pattern

```
                    ┌─────────────────────────────────────┐
                    │         ORCHESTRATOR                │
                    │   (understands user request,         │
                    │    plans, delegates, validates)      │
                    └──────────┬──────────────────────────┘
                               │
              ┌────────────────┼────────────────────┐
              │                │                     │
              ▼                ▼                     ▼
     ┌────────────┐   ┌────────────┐        ┌────────────┐
     │ RESEARCHER │   │  BUILDER   │        │ COMMERCIAL │
     │  (Scout)   │   │  (Cursor)  │        │  (Meter)   │
     └────────────┘   └────────────┘        └────────────┘
              │                │                     │
              └────────────────┼────────────────────┘
                               │
                               ▼
                    ┌─────────────────────────────────────┐
                    │         ORCHESTRATOR                │
                    │   (reviews output, validates,       │
                    │    reports to user)                 │
                    └─────────────────────────────────────┘
```

---

## Core patterns

### 1. Agent Registry

Define WHO each agent is, their capabilities, boundaries, and escalation relationships.

```markdown
# Agent: Builder

## Identity
- Primary software builder
- CLI-based execution environment

## Responsibilities
- Websites, apps, dashboards, APIs, CRM systems

## Boundaries
- Must NOT choose an arbitrary project directory
- Must NOT deploy to production without authority
- Must NOT claim success without evidence

## Escalation
If difficult technical reasoning needed → escalate to Architect agent
```

### 2. Delegation Brief

Every substantial delegation uses a structured contract:

```markdown
PROJECT: <canonical project>
WORKSPACE: <verified absolute path>

OBJECTIVE: <precise desired outcome>

REQUIREMENTS:
- <concrete requirement>
- <concrete requirement>

CONSTRAINTS:
- <technical/business constraint>

ACCEPTANCE CRITERIA:
- <observable success condition>

VALIDATION:
- <build/test/check commands>

STOP AND RETURN IF:
- <condition requiring human decision>

RETURN:
- summary of changes
- files changed
- validation performed
- remaining risks
```

### 3. Memory system

Conversation is temporary. Memory is persistent.

- **Global memory** — user preferences, reusable patterns, confirmed facts
- **Project memory** — architecture decisions, project state, lessons
- **Admission test** — Is it durable? Useful? Verified? Correct place?
- **Confidence levels** — CONFIRMED / OBSERVED / TENTATIVE

### 4. Cron jobs with continuity

Scheduled tasks that dedupe against previous output:

```yaml
schedule: "0 12 * * *"
prompt: "Query analytics, compare to last report, highlight changes only"
continuity: true  # carry previous output into context
```

---

## Real-world use

This pattern has shipped:

- **Travel booking system** — GPS clustering, Arabic PDF dispatch, WhatsApp driver coordination
- **Industrial B2B platform** — multi-lingual, multi-currency, Cloudflare edge deployment
- **Research pipelines** — automated repository analysis, commercial intelligence, daily analytics reports
- **Agent ecosystem** — specialized roles (researcher, builder, commercial analyst, escalation architect)

---

## Adapt to your framework

These patterns are framework-agnostic. They've been implemented on Hermes Agent but work with any agent system that supports:

- Role definitions
- Persistent memory
- Tool use
- Subprocess execution

---

## License

MIT
