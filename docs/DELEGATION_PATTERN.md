# Delegation Brief — Pattern

Every substantial delegation follows a structured contract. This ensures:

- The specialist has enough context to act correctly
- The specialist knows what success looks like
- The specialist knows when to stop and return control
- The orchestrator can verify the output

---

## The contract

```markdown
PROJECT: <canonical project name/ID>

WORKSPACE: <verified absolute filesystem path>

MODE: <IMPLEMENTATION | PLANNING | RESEARCH | COMMERCIAL | ESCALATION>

OBJECTIVE: <precise desired outcome>

CONTEXT: <relevant architecture, existing state, why this matters>

REQUIREMENTS:
- <concrete behavior or feature>
- <concrete behavior or feature>

CONSTRAINTS:
- <technical constraint>
- <business constraint>
- <security constraint>

SELECTED SKILLS:
- <skill relevant to this task>

ALLOWED SCOPE:
- <files/modules that may be modified>

PROTECTED SCOPE:
- <files that should NOT be changed>

ACCEPTANCE CRITERIA:
- <observable success condition>
- <observable success condition>

VALIDATION:
- <command: build, test, lint, browser check>
- <command: build, test, lint, browser check>

STOP AND RETURN IF:
- workspace appears wrong
- requirement conflicts with architecture
- destructive change becomes necessary
- credentials are missing
- external blocker prevents completion
- architecture decision exceeds scope

RETURN:
- summary of changes
- files changed
- validation performed + results
- remaining risks
- blockers if any
```

---

## Delegation classes

### LIGHTWEIGHT
Small task with low risk. Minimal brief required.
- Summaries, simple reasoning, text transformations
- Usually: orchestrator handles directly or delegates to lightweight agent

### RESEARCH
External discovery or evidence gathering.
- Requires: research question, scope, freshness requirement, expected output
- Usually: researcher agent

### COMMERCIAL
Marketing, sales, advertising, commercial intelligence.
- Requires: business question, available data, timeframe, uncertainty constraints
- Usually: commercial analyst agent

### IMPLEMENTATION
Software modification.
- Requires: full execution contract (see above)
- Usually: builder agent

### ESCALATION
Difficult technical reasoning beyond the builder's current capability.
- Requires: isolated hard problem, evidence, previous attempts, actual failure
- Usually: architect/escalation agent (escalation-only)

---

## Research brief example

```markdown
TARGET: <what to investigate>

CONTEXT: <why this matters, what we already know>

SCOPE:
- Freshness: <how recent must evidence be>
- Depth: <how thorough>
- Sources: <preferred source types>

QUESTIONS:
1. <specific question>
2. <specific question>

EXPECTED OUTPUT:
- Findings with evidence
- Sources cited
- Options and tradeoffs
- Risks identified
- Recommendation if justified

RESEARCH PRINCIPLE:
Distinguish between FACT / SUPPORTED INFERENCE / OPINION / UNKNOWN
Never fabricate sources or present uncertain information as fact.
```

---

## Commercial brief example

```markdown
QUESTION: <what commercial question are we answering>

CONTEXT: <business context, what we know>

DATA AVAILABLE:
- <what data exists>
- <what's missing>

TIMEFRAMEFRAME: <relevant period>

EXPECTED OUTPUT:
- What is happening
- Why it matters
- Financial/commercial impact
- Opportunities and risks
- Recommended actions
- Important metrics

UNCERTAINTY CONSTRAINTS:
- Do not fabricate numbers
- Label estimates clearly
- Unknown remains unknown
```

---

## One milestone at a time

For complex projects:

```
PLAN → MILESTONE 1 → BUILD → VALIDATE → REVIEW → MILESTONE 2 → ...
```

NOT:

```
PLAN → BUILD EVERYTHING → HOPE IT WORKS
```

Each milestone has: objective, dependencies, scope, acceptance criteria, validation.
