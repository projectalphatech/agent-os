# Examples — Delegation Briefs in Practice

These are concrete examples of delegation briefs for different scenarios.

---

## Example 1: Research Brief

```
TARGET: Investigate the GitHub repository "react-query" — specifically its current maintenance status, community adoption, and whether it's still recommended for new projects.

CONTEXT: We're evaluating whether to use react-query (TanStack Query) for a new dashboard project. Need current facts, not outdated blog posts.

SCOPE:
- Freshness: Within 30 days
- Depth: Stars, forks, open issues, last commit, maintenance signals, community sentiment
- Sources: GitHub API, npm, recent discussions

QUESTIONS:
1. What's the current star count and growth trend?
2. When was the last meaningful commit/release?
3. How many open issues? Any critical unaddressed bugs?
4. Is the maintainer team active?
5. Is it recommended for new projects in 2026?

EXPECTED OUTPUT:
- Facts with sources (URLs)
- Maintenance status: ACTIVE / DECLINING / ABANDONED
- Recommendation with justification
- Risks identified

RESEARCH PRINCIPLE:
Distinguish between FACT / SUPPORTED INFERENCE / OPINION / UNKNOWN
Never fabricate star counts or dates.
```

---

## Example 2: Implementation Brief

```
PROJECT: Sharm Trips (travel booking platform)

WORKSPACE: /home/adham/projects/sharm-trips/

MODE: IMPLEMENTATION

OBJECTIVE: Add tokenized public PDF links for driver dispatch — the driver can open the dispatch manifest without logging in.

CONTEXT: Currently, dispatch PDFs are only accessible through the admin panel (requires login). Drivers need a public link they can tap on their phone. The link should be unguessable (UUID token), accessible without auth, and display inline in the browser.

REQUIREMENTS:
- Add `pdf_token` column to `dispatch_groups` table (UUID, unique)
- When generating dispatch PDF, create/reuse token
- Public route: `/api/d/<token>/` returns the PDF with `Content-Disposition: inline`
- WhatsApp message sent to driver includes the full public URL

CONSTRAINTS:
- No authentication on the public route (token is the access control)
- Token must be UUID v4 — unguessable
- PDF generation uses Cloudflare Browser Rendering API
- Must not break existing admin PDF download

SELECTED SKILLS:
- nextjs-cloudflare-deploy (for Cloudflare Workers patterns)
- systematic-debugging (if issues arise)

ALLOWED SCOPE:
- src/app/api/admin/dispatch/[groupId]/pdf/handlers.ts
- src/app/api/d/[token]/route.ts
- src/db/schema.ts
- migrations/

PROTECTED SCOPE:
- src/lib/auth.ts (don't touch auth system)
- src/app/admin/(console)/ (don't break existing admin routes)

ACCEPTANCE CRITERIA:
- `pdf_token` column exists in D1
- GET /api/d/<token> returns HTTP 200 with `application/pdf`
- GET /api/d/<token> works without any session/cookie
- GET /api/d/<invalid-token> returns HTTP 404

VALIDATION:
- `npx next build` passes
- `curl -sI https://<worker>/api/d/<valid-token>` returns 200
- `curl -sI https://<worker>/api/d/invalid-uuid` returns 404

STOP AND RETURN IF:
- Database migration conflicts with existing schema
- Cloudflare Browser Rendering format issue
- Unable to deploy for verification

RETURN:
- Summary of changes
- Files changed
- Migration SQL
- Live URL for testing
- Remaining risks
```

---

## Example 3: Planning Brief

```
PROJECT: New Client CRM

MODE: PLANNING

OBJECTIVE: Produce a technical plan for a custom CRM system for a client who manages 500+ customers, needs lead tracking, sales pipeline, and WhatsApp integration.

CONTEXT: The client currently uses Excel. They have a sales team of 5 people. Need mobile-friendly access. Must support Arabic and English. Integration with their existing WhatsApp Business number.

REQUIREMENTS (high-level):
- Lead management (create, update, assign, track)
- Sales pipeline (stages: new → contacted → qualified → proposal → won/lost)
- WhatsApp integration (send message to lead from the CRM)
- Mobile-friendly (team uses phones primarily)
- Bilingual: Arabic (primary) and English
- User roles: admin and sales rep

CONSTRAINTS:
- Must run on Cloudflare Workers (existing infrastructure)
- D1 database (SQLite at edge)
- No monthly SaaS fees — client owns everything
- Auth: simple credentials (no OAuth complexity)

EXPECTED OUTPUT:
- Recommended architecture (pages, routes, components)
- Data model (tables, relationships)
- Component structure
- Key workflows (how a lead moves through the system)
- Integration approach (WhatsApp — wa.me links or API?)
- Implementation milestones (6 phases)
- Major risks and unknowns
```

---

## Example 4: Commercial Brief

```
QUESTION: How are our Meta ads performing for the Sharm Trips campaign? What needs fixing?

CONTEXT: We've been running Facebook/Instagram ads for 3 days targeting Egypt and Saudi Arabia. Budget is ~$20/day. We need to know if the creative is working and where waste is.

DATA AVAILABLE:
- Meta ad account ID: act_XXXXXXXXX
- Campaign: "Sharm Trips - Landing Page Views"
- Creative: Single image (Naama Bay beach), Arabic copy, CTA "Book Now"
- Landing page: sharm-trips.adhamelsharkawy996.workers.dev/en/book

EXPECTED OUTPUT:
- Performance summary (impressions, clicks, CTR, CPC, spend)
- Audience breakdown (age, gender, country, placement)
- Creative assessment (is the image working? is the copy clear?)
- Landing page assessment (does the page deliver on the ad promise?)
- Top 3 recommended actions with expected impact
- Any disapproved ads or policy issues

TIMEFRAME: Last 7 days

UNCERTAINTY CONSTRAINTS:
- Do not fabricate numbers
- If data is inaccessible, say so and explain why
- Estimates must be labeled as such
```

---

## Example 5: Escalation Brief

```
OBJECTIVE: Diagnose why the booking endpoint returns 400 "Invalid envelope" for encrypted requests.

CONTEXT: The booking endpoint was recently changed to accept AES-256-GCM encrypted payloads. The client encrypts correctly (verified with a standalone test), but the server rejects valid envelopes. The error is generic — no stack trace, just "Invalid envelope".

PREVIOUS ATTEMPTS:
1. Verified client encryption works (can decrypt locally with same key)
2. Checked that nonce/iv/ciphertext are all present in the request body
3. Verified Content-Type: application/json header is set

RELEVANT ARCHITECTURE:
- Server: Next.js 15 route handler on Cloudflare Worker
- Decryption: HKDF(AES-GCM) with server-side secret + client nonce
- Token: `sharm-booking-2026` (hardcoded, not from env — potential mismatch?)

ACTUAL FAILURE:
- Request body: `{"nonce":"...","iv":"...","ciphertext":"..."}`
- Response: `{ "error": "Invalid envelope" }` (400)

EXPECTED REASONING OUTPUT:
- Root cause hypothesis
- Which component is failing (key derivation? decryption? parsing?)
- Recommended fix
- Why previous attempts didn't catch it
