<div align="center">

# 🤖 agent-os

**The operating system for coordinating specialized AI agents.**

*Not generic workers. Not chatbots. A team of specialists with structured briefs, verification gates, and memory that survives sessions.*

> **2026 is the Year of Multi-Agent Systems** — Deloitte's tech predictions explicitly frame agent orchestration as a key enterprise unlock. We built agent-os to be the operating system for this shift.

## 📸 Demo

![Agent OS Agent Diagram](https://github.com/projectalphatech/agent-os/blob/main/assets/agent-diagram.png)

<!-- Placeholder image — replace with a real diagram of your agent topology -->

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?logo=typescript&logoColor=white)](https://www.typescriptlang.org)
[![Cloudflare](https://img.shields.io/badge/Cloudflare-F38020?logo=cloudflare&logoColor=white)](https://workers.cloudflare.com)
[![Stars](https://img.shields.io/github/stars/projectalphatech/agent-os?style=social)](https://github.com/projectalphatech/agent-os)

[Quick Start](#-quick-start) •
[The Problem](#-the-problem) •
[The Solution](#-the-solution) •
[Core Patterns](#-core-patterns) •
[Real Results](#-real-results) •
[Docs](docs/)

</div>

---

## 🤔 The problem

Most multi-agent frameworks treat agents as **generic workers**.

You say: *"Build me a travel booking system."*
The agent says: *"Done!"*
You check: It's a TODO app with a different label.

**The issue:** No structure. No verification. No memory. No specialization.

---

## ✅ The solution

**agent-os** gives you:

| Feature | What it does |
|---|---|
| **Specialized roles** | Researcher, Builder, Commercial Analyst, Orchestrator — each with distinct tools and boundaries |
| **Structured briefs** | Not "do this" — but "do this, with these constraints, these acceptance criteria, and stop if X" |
| **Verification gates** | Agents must prove completion with evidence. No claims without proof. |
| **Persistent memory** | Cross-session continuity. Confidence levels. Source-of-truth hierarchy. |
| **Delegation contracts** | Objective, scope, constraints, validation, stop conditions — all in one brief |

---

## 🏗️ Architecture

```
                    ┌─────────────────────────────────────┐
                    │         ORCHESTRATOR                │
                    │   (understands request,              │
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

### 2. Write a delegation brief

```markdown
PROJECT: Travel Booking System
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

STOP AND RETURN IF:
- Database migration conflicts with schema
- Cloudflare Browser Rendering format issue
```

### 3. Set up memory

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

## 🌍 Real results

This system has shipped:

| System | What it does | Stack |
|---|---|---|
| **Travel booking + dispatch** | GPS clustering, Arabic PDF dispatch, WhatsApp driver coordination | Next.js · Cloudflare · D1 |
| **Industrial B2B platform** | Multi-lingual, multi-currency, edge deployment | Next.js · Cloudflare · D1 |
| **Custom CRM** | Real-time, edge-deployed, Arabic/English | Next.js · Cloudflare · D1 |
| **Research pipelines** | Automated repository analysis, commercial intelligence | Python · APIs |
| **Daily analytics** | GSC monitoring, ad performance, automated reports | APIs · Cron |

> Enterprise multi-agent adoption surged from 18% to 61% in one year. agent-os is built for the 61%.

> By 2027, 70% of multi-agent systems will use narrow, focused roles — DruidAI. Our registry (Researcher, Builder, Commercial, Architect) is built exactly for this.

---

## 🔍 Why verification gates?

Agent accuracy on structured benchmarks rose from 12% to 66.3% in one year — but agents still fail ~1 in 3 tasks. The gap drives everything we build.

**Verification-gated delegation (SEMAP) reduces coordination failures by 69.6%** — explicit contracts between agents are the single most effective intervention for reliable multi-agent systems.

Every agent in agent-os operates within verification gates:
- **Brief**: Objective, constraints, acceptance criteria, stop conditions
- **Execution**: Agent works independently
- **Verification**: Agent proves completion with evidence (build output, curl responses, screenshots)
- **Review**: Orchestrator validates independently

No claims without proof.

---

## 🔗 Ecosystem and related work

agent-os is part of a growing multi-agent ecosystem:

- [LangGraph](https://github.com/langchain-ai/langgraph) — stateful graph-based workflows
- [AutoGen](https://github.com/microsoft/autogen) — enterprise multi-agent framework
- [CrewAI](https://github.com/joaomdmoura/crewai) — role-based multi-agent orchestration
- [Agno](https://github.com/agent-os/agno) — FastAPI for AI Agents (AgentOS runtime)

agent-os differs by focusing on **verification-gated delegation** as the core abstraction, not just orchestration.

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

## 🔗 Part of the Project Alpha ecosystem

- [agent-os](https://github.com/projectalphatech/agent-os) — the operating system for coordinating specialized AI agents
- [structured-delegation](https://github.com/projectalphatech/structured-delegation) — delegation briefs + anti-patterns that prevent build failures
- [arabic-edge-pdf](https://github.com/projectalphatech/arabic-edge-pdf) — Arabic PDF generation at the edge, zero tofu, zero libraries
- [gps-cluster-engine](https://github.com/projectalphatech/gps-cluster-engine) — group GPS points by proximity with capacity constraints
- [nextjs-cloudflare-deploy](https://github.com/projectalphatech/nextjs-cloudflare-deploy) — the definitive Next.js + Cloudflare Workers deployment guide

---

## 📄 License

MIT © [Project Alpha Tech](https://projectalpha.tech)

---

<div align="center">

**⭐ Star this repo if you're tired of agents that claim success without proof!**

</div>
