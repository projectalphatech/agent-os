<div align="center">

# agent-os

**An operating system for coordinating specialized AI agents.**

Structured briefs. Verification gates. Memory that survives sessions.

[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](./LICENSE)
[![Docs](https://img.shields.io/badge/docs-patterns-black.svg)](./docs)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](#contributing)

</div>

---

Most multi-agent frameworks treat agents as interchangeable workers: you describe a task, an agent reports success, and you discover later that the work was never done. `agent-os` is a set of patterns for the opposite approach — narrow specialists, explicit contracts, and completion that has to be proven rather than claimed.

It is framework-agnostic. There is nothing to install; these are documented patterns you implement in whatever agent runtime you already use.

## Contents

- [Why this exists](#why-this-exists)
- [How it works](#how-it-works)
- [Core patterns](#core-patterns)
- [Usage](#usage)
- [Skills](#skills)
- [Integrations](#integrations)
- [Prior art](#prior-art)
- [Contributing](#contributing)
- [License](#license)

## Why this exists

Three failures show up repeatedly when delegating real work to agents:

| Failure | What it looks like |
| --- | --- |
| Unverified completion | The agent reports success. The build was never run. |
| Scope drift | A narrow task quietly becomes a refactor of unrelated code. |
| Lost context | Every session starts from zero; the same corrections get repeated. |

`agent-os` addresses each with a specific mechanism: verification gates, delegation contracts, and persistent memory.

## How it works

Work flows through an orchestrator that never executes tasks itself. It plans, delegates to a specialist, then independently validates the result before reporting back.

```
                    ┌──────────────────────┐
   request ────────▶│     ORCHESTRATOR     │
                    │  plan → delegate →   │
   report  ◀────────│  validate → report   │
                    └──────────┬───────────┘
                               │  brief + acceptance criteria
              ┌────────────────┼────────────────┐
              ▼                ▼                ▼
      ┌──────────────┐ ┌──────────────┐ ┌──────────────┐
      │  RESEARCHER  │ │   BUILDER    │ │  COMMERCIAL  │
      │ read-only    │ │ writes code  │ │ market data  │
      └──────────────┘ └──────────────┘ └──────────────┘
              │                │                │
              └────────────────┴────────────────┘
                     evidence of completion
```

The critical property is that the arrow back up carries **evidence** — build output, HTTP status codes, test results — not an assertion of success.

## Core patterns

| Pattern | Problem it solves | Reference |
| --- | --- | --- |
| Agent registry | Agents with unclear capabilities take on work they cannot do | [AGENT_REGISTRY_PATTERN.md](./docs/AGENT_REGISTRY_PATTERN.md) |
| Delegation brief | Vague instructions produce vague results | [DELEGATION_PATTERN.md](./docs/DELEGATION_PATTERN.md) |
| Memory system | Context is lost between sessions | [MEMORY_PATTERN.md](./docs/MEMORY_PATTERN.md) |
| Scheduled continuity | Recurring jobs re-report unchanged information | [CRON_PATTERN.md](./docs/CRON_PATTERN.md) |

### Verification gates

A gate is the contract that makes delegation safe. Every brief declares, before work begins, how completion will be proven:

```
ACCEPTANCE CRITERIA:
- GET /api/documents/<token> returns 200 with application/pdf
- GET /api/documents/<invalid-token> returns 404

VALIDATION:
- npm run build exits 0
- curl -sI $DEPLOY_URL/api/documents/$TOKEN | head -1
```

An agent that cannot produce this evidence has not finished, regardless of what it reports. Acceptance criteria written *after* the work are not gates — they are rationalizations.

## Usage

### 1. Define an agent

Each agent needs responsibilities, hard boundaries, and an escalation path.

```markdown
# Builder

## Responsibilities
- Application code, APIs, database schema, deployment

## Boundaries
- MUST NOT deploy to production without explicit authorization
- MUST NOT report completion without validation output
- MUST NOT modify files outside the declared scope

## Escalation
Architecture decisions beyond the brief's scope → return to orchestrator
```

### 2. Write a delegation brief

```markdown
OBJECTIVE
Add tokenized public document links so recipients can open a file without an account.

CONTEXT
Documents are currently reachable only behind an authenticated admin session.

REQUIREMENTS
- Add a `pdf_token` column (UUID, unique) to the documents table
- Serve GET /api/documents/<token> with Content-Disposition: inline
- Include the resulting URL in the outbound notification

CONSTRAINTS
- The token is the access control; the route stays unauthenticated
- Existing authenticated download must keep working

ALLOWED SCOPE
- src/api/documents/**, src/db/schema.*, migrations/

ACCEPTANCE CRITERIA
- Valid token returns 200 with application/pdf
- Invalid token returns 404
- Existing admin download still returns 200

VALIDATION
- npm run build exits 0
- curl -sI on both a valid and an invalid token

STOP AND RETURN IF
- The migration conflicts with the existing schema
- Deployment credentials are unavailable
```

### 3. Persist what should not be re-learned

```markdown
## Preferences
- Evidence-based completion reports, not status claims

## Standards
- Edge deployment via Workers; no Node-native database drivers
- RTL-first layout for Arabic interfaces
```

### 4. Give recurring jobs continuity

```yaml
schedule: "0 12 * * *"
prompt: "Pull search analytics, diff against the previous report, surface only changes."
continuity: true   # injects the prior run's output to prevent duplicate reporting
```

Further examples: [research](./examples/DELEGATION_BRIEFS.md#example-1-research-brief) · [implementation](./examples/DELEGATION_BRIEFS.md#example-2-implementation-brief) · [planning](./examples/DELEGATION_BRIEFS.md#example-3-planning-brief) · [commercial](./examples/DELEGATION_BRIEFS.md#example-4-commercial-brief) · [escalation](./examples/DELEGATION_BRIEFS.md#example-5-escalation-brief)

## Skills

Self-contained capabilities that compose with the patterns above.

| Skill | Purpose |
| --- | --- |
| [structured-delegation](https://github.com/projectalphatech/structured-delegation) | Delegation briefs and the anti-patterns that break them |
| [arabic-edge-pdf](https://github.com/projectalphatech/arabic-edge-pdf) | Arabic PDF generation at the edge, without font bundling |
| [gps-cluster-engine](https://github.com/projectalphatech/gps-cluster-engine) | Proximity clustering under capacity constraints |
| [nextjs-cloudflare-deploy](https://github.com/projectalphatech/nextjs-cloudflare-deploy) | Next.js on Cloudflare Workers, including the dead ends |

## Integrations

These patterns require no particular vendor. The runtimes below are the ones we have exercised in production.

| Integration | Role |
| --- | --- |
| [Cloudflare Workers](https://workers.cloudflare.com) | Edge runtime for orchestration and scheduling |
| [D1](https://developers.cloudflare.com/d1/) | Edge SQLite for agent state and memory |
| [Workers Workflows](https://developers.cloudflare.com/workflows/) | Durable multi-step execution |
| [Browser Rendering](https://developers.cloudflare.com/browser-rendering/) | Headless Chrome for PDFs and screenshots |
| [Next.js](https://nextjs.org) | Application layer for agent-facing interfaces |
| [AnyDoc](https://github.com/firecrawl/anydoc) | Document parsing ahead of agent processing |

## Prior art

`agent-os` is a pattern library, not a runtime, and is complementary to the frameworks below.

| Project | Focus | Relationship |
| --- | --- | --- |
| [LangGraph](https://github.com/langchain-ai/langgraph) | Stateful graph execution | Use as the runtime; apply these patterns on top |
| [AutoGen](https://github.com/microsoft/autogen) | Conversational multi-agent orchestration | Overlapping goals, different abstraction |
| [CrewAI](https://github.com/crewAIInc/crewAI) | Role-based agent crews | Similar specialization model, no verification layer |

The distinguishing choice here is treating **verification as the primary abstraction** rather than orchestration.

## Contributing

Read the [patterns](./docs) first, then open an issue describing the change before submitting a pull request. Pattern documents should include the failure mode being addressed, not only the recommended approach.

## License

MIT © [Project Alpha Tech](https://projectalpha.tech)
