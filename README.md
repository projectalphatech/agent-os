<div align="center">

# 🤖 agent-os

**How to coordinate specialized AI agents with structured delegation, persistent memory, and verification gates.**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](http://makeapullrequest.com)
[![Twitter](https://img.shields.io/twitter/follow/projectalpha.tech?style=social)](https://twitter.com/projectalpha.tech)

*Most multi-agent frameworks treat agents as generic workers. This is how you give them **specialized roles**, **structured briefs**, and **verification gates**.*

[Quick Start](#quick-start) •
[Patterns](#core-patterns) •
[Examples](examples/DELEGATION_BRIEFS.md) •
[Docs](docs/)

</div>

---

## 🤔 Why this exists

LangGraph, CrewAI, AutoGen — they give you *orchestration*. They don't give you:

| Feature | Other Frameworks | agent-os |
|---|---|---|
| **Specialized roles** | ❌ Generic workers | ✅ Distinct toolsets, boundaries, escalation paths |
| **Structured delegation** | ❌ Vague instructions | ✅ Full contract: objective, constraints, acceptance criteria, stop conditions |
| **Persistent memory** | ❌ Session-only | ✅ Cross-session continuity with confidence levels |
| **Verification gates** | ❌ Trust the agent | ✅ Agents must prove completion with evidence |
| **Scheduled tasks** | ❌ Repetitive reports | ✅ Continuity-aware deduplication |

---

## 🏗️ The pattern

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

## 🚀 Quick Start

### 1. Define your agents

Create a registry with specialized roles, boundaries, and escalation paths:

```markdown
# Builder Agent

## Responsibilities
- Websites, apps, dashboards, APIs

## Boundaries
- Must NOT deploy to production without authority
- Must NOT claim success without evidence

## Escalation
If difficult technical reasoning needed → escalate to Architect
```

### 2. Write delegation briefs

Every substantial task gets a structured contract:

```markdown
PROJECT: Sharm Trips
WORKSPACE: /home/adham/projects/sharm-trips/

OBJECTIVE: Add tokenized public PDF links for driver dispatch

REQUIREMENTS:
- Add pdf_token column (UUID, unique)
- Public route: /api/d/<token>/ returns PDF inline
- WhatsApp message includes public URL

CONSTRAINTS:
- No auth on public route (token is access control)
- Must not break existing admin PDF download

ACCEPTANCE CRITERIA:
- GET /api/d/<token> returns 200 with application/pdf
- GET /api/d/<invalid-token> returns 404

VALIDATION:
- npx next build passes
- curl -sI https://<worker>/api/d/<token> returns 200
```

### 3. Set up persistent memory

```markdown
## User Preferences
- Prefers concise, direct communication
- Expects evidence-based completion, not claims

## Project Standards
- Cloudflare Workers via @opennextjs/cloudflare
- D1 for database, never better-sqlite3
- Arabic UI must be RTL-first
```

### 4. Schedule with continuity

```yaml
schedule: "0 12 * * *"
prompt: "Query GSC analytics, compare to last report, highlight changes"
continuity: true  # deduplicates against previous output
```

---

## 📚 Core patterns

### 1. Agent Registry
Define WHO each agent is, their capabilities, boundaries, and escalation relationships.

→ [Full pattern](docs/AGENT_REGISTRY_PATTERN.md)

### 2. Delegation Brief
Every substantial delegation uses a structured contract with objective, constraints, acceptance criteria, and stop conditions.

→ [Full pattern](docs/DELEGATION_PATTERN.md)

### 3. Memory System
Conversation is temporary. Memory is persistent. Cross-session continuity with confidence levels.

→ [Full pattern](docs/MEMORY_PATTERN.md)

### 4. Scheduled Tasks with Continuity
Scheduled tasks that dedupe against previous output — report what changed, not what stayed the same.

→ [Full pattern](docs/CRON_PATTERN.md)

---

## 🌍 Real-world use

This pattern has shipped:

- **Travel booking system** — GPS clustering, Arabic PDF dispatch, WhatsApp driver coordination
- **Industrial B2B platform** — multi-lingual, multi-currency, Cloudflare edge deployment
- **Research pipelines** — automated repository analysis, commercial intelligence, daily analytics reports
- **Agent ecosystem** — specialized roles (researcher, builder, commercial analyst, escalation architect)

---

## 🔧 Adapt to your framework

These patterns are framework-agnostic. They've been implemented on Hermes Agent but work with any agent system that supports:

- Role definitions
- Persistent memory
- Tool use
- Subprocess execution

---

## 📖 Examples

- [Research brief](examples/DELEGATION_BRIEFS.md#example-1-research-brief)
- [Implementation brief](examples/DELEGATION_BRIEFS.md#example-2-implementation-brief)
- [Planning brief](examples/DELEGATION_BRIEFS.md#example-3-planning-brief)
- [Commercial brief](examples/DELEGATION_BRIEFS.md#example-4-commercial-brief)
- [Escalation brief](examples/DELEGATION_BRIEFS.md#example-5-escalation-brief)

---

## 🤝 Contributing

PRs welcome! Read the [patterns](docs/) first, then open an issue to discuss before submitting.

---

## 📄 License

MIT © [Project Alpha Tech](https://projectalpha.tech)

---

<div align="center">

**⭐ Star this repo if you find it useful!**

</div>
