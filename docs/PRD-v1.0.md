# agent-resolv — Product Requirements Document

> **Version:** 1.0
> **Date:** March 2, 2026
> **Status:** Draft — internal review

---

## 1. Executive Summary

**TL;DR:** agent-resolv is an AI co-pilot for Portuguese insurance brokers. It automates the back-office grind — extracting structured data from leads, requesting quotes from insurers via email, parsing PDF responses, and generating comparisons — so brokers can serve more customers without hiring more people. Brokers keep their existing customer channels; agent-resolv slots into their workflow. Pre-seed stage, targeting launch Q3–early Q4 2026.

**MVP Scope Note:** The specific insurance product type(s) for MVP launch will be selected after Rolando shadow sessions (end of March 2026). The architecture is product-type-agnostic — schemas cover auto, home, life, and health — but implementation and validation start with the selected MVP types based on Rolando's real workflow volume, PDF availability, and competitive landscape.

### 1.1 What Is agent-resolv?

agent-resolv is an AI-powered back-office for licensed insurance brokers (mediadores de seguros). It processes incoming customer leads from whatever channel the broker already uses (website contact form, email, phone, WhatsApp), automates quote collection from insurers, parses responses, and generates structured comparisons for the broker to review and deliver.

Gonçalo's personal company is ReisierX Unipessoal Lda. The operating entity for agent-resolv is TBD. Build Up Labs is the technical co-founder. No ASF/BdP licenses are held yet — the MVP (broker co-pilot tooling) does not require its own license since it operates as software used by already-licensed brokers. Own licensing becomes necessary if/when agent-resolv operates as a broker directly (D2C channels, Phase 4+).

### 1.2 Why It Exists

Portugal's insurance brokerage market is manual. Brokers collect customer data over phone/email, request quotes by emailing each insurer individually, wait days for responses, manually compare PDFs, and email recommendations back. A single customer journey takes 3–7 days of elapsed time across dozens of emails.

There are no standardized APIs. Every insurer integration is bilateral. Existing digital players (MUDEY, ComparaJa) took years to build integrations and still don't cover post-purchase or claims. MUDEY's pivot to MudeyPRO (B2B2C tooling for small mediators) confirms: the market needs broker infrastructure, not just another consumer brand.

agent-resolv skips the integration queue by starting with the same channel brokers use today — email — but automates every step with AI. Brokers keep their existing customer relationships and channels. agent-resolv handles the back-office.

**Why broker-first, not consumer-first?** MUDEY spent 3+ years building a D2C brand, then pivoted to B2B2C tooling for small mediators. Customer acquisition is the hardest, most capital-intensive part — the technology is easier. We start where MUDEY ended up: broker tooling. Once the AI pipeline is proven on real broker workflows, we expand to direct customer channels (embeddable widget, WhatsApp) and eventually a full platform.

### 1.3 A Day in the Broker's Life

**Today:** Rolando receives a call from a customer who needs auto insurance. He takes notes on paper, opens his email, and sends separate quote requests to 5 insurers — each with slightly different formatting requirements. Over the next 3–5 days, PDFs trickle back. He opens each one, manually extracts pricing and coverage details into a spreadsheet, builds a comparison, writes a recommendation email to the customer, and waits for approval before binding the policy.

**With agent-resolv:** Rolando forwards the customer's email (or pastes his phone notes) to the system. The AI extracts a structured profile, sends quote requests to all 5 insurers simultaneously, parses the PDF responses as they arrive, generates a comparison with reasoning, and emails it to Rolando for review. He clicks an approval link, and the recommendation goes to the customer. What took 3–5 days of back-and-forth now takes hours of elapsed time with minutes of Rolando's attention.

```
Customer contacts broker → Broker forwards lead to agent-resolv via email → AI extracts structured profile → System sends quote requests to insurers via email → Insurers respond with PDF quotes → AI parses PDFs, compares quotes → Broker reviews comparison via email, approves via secure link
```

### 1.4 What the MVP Does

The MVP handles insurance back-office end-to-end for the selected MVP product types: a broker receives a lead through their existing channels, feeds it into agent-resolv (forwarded email, contact form webhook, pasted phone notes), the system extracts a structured profile, requests quotes from multiple insurers via email, parses the PDF responses, generates a comparison, and the broker reviews and delivers the recommendation to the customer.

The primary broker interface is **email** — brokers forward customer leads to agent-resolv, receive extracted profiles and comparisons via email, and approve recommendations via a secure link. Low onboarding friction: forward leads and click approval links. A web dashboard and MCP server follow in Phase 3.

### 1.5 Key Research Findings

**PDF Parsing (95%+ accuracy on tested products):** Claude reliably extracts structured data from Portuguese insurance quote PDFs across multiple insurers and formats. No per-insurer template needed. Tested on 5 real PDFs from 4 insurers (vida credito and multirriscos). PDF parsing for the selected MVP product types remains untested pending sample PDFs from Rolando shadow sessions — this is the top execution risk.

**Email Thread Parsing (90–93% accuracy on 3 threads):** Full workflow state can be inferred from email threads — participant identification, document tracking, action items, and insurance cross-sell moments. Tested on 3 real multi-party mortgage/insurance threads (56 messages). Insurer-to-broker email format is untested — critical gap to be resolved during Rolando sessions.

**Conversational Intake (viable for auto, challenging for health):** Conversational profiling works well for auto insurance (10–14 turns). Home insurance is blocked by rebuild cost estimation. Health insurance is high-complexity with regulatory constraints (GDPR Art. 9, EU AI Act). This research validates a future embeddable widget for broker websites.

**Integration Landscape (no APIs, email is the path):** No standardized PT insurance APIs. Email/portal automation is how every broker works today. Fidelidade has the best API story (20+ REST endpoints) for future direct integration.

**What's risky:** PDF parsing untested for some product types (pending sample PDFs); health insurance QIS errors create legal exposure; home insurance rebuild cost estimation is unreliable without lookup tables; EU AI Act likely classifies insurance AI recommendation as high-risk (Aug 2026 deadline); Mastra framework has a fast-moving API surface with frequent breaking changes.

### 1.6 Competitive Moat

The moat is not any single component — it's the accumulation of four stacking layers:

1. **Regulatory license** (table stakes) — ASF + BdP licensing creates a barrier to entry, but is replicable by any well-funded competitor.
2. **Broker relationships** (medium defensibility) — Trusted relationships with brokers and insurers take time to build. First-mover advantage in a market with no AI broker tooling.
3. **Operational data flywheel** (high defensibility) — Every quote request processed generates structured insurer response data: PDF formats, email patterns, response times, pricing patterns per insurer per product type per customer profile. This corpus grows with each transaction and cannot be replicated without processing equivalent volume.
4. **Normalized insurer data corpus** (highest defensibility) — The schema layer normalizes chaotic, insurer-specific data into consistent structures. Over time, this becomes the most comprehensive structured dataset of Portuguese insurance pricing and coverage across providers.

### 1.7 Unit Economics — LLM Cost Model

> **Status: Envelope estimate.** Exact costs require validation against real-world PDFs and insurer email formats from Rolando shadow sessions.

**Envelope calculation (auto insurance, 5 providers):** Pricing basis: Claude Sonnet 4.5 ($3/$15 per MTok input/output), Claude Haiku 4.5 ($1/$5 per MTok input/output). March 2026.

| Pipeline Step               | Model  | Input Tokens | Output Tokens | Est. Cost  |
| --------------------------- | ------ | ------------ | ------------- | ---------- |
| Lead extraction (×1)        | Sonnet | 2,000        | 800           | $0.02      |
| Quote email generation (×5) | Haiku  | 7,500        | 2,500         | $0.02      |
| PDF parsing (×5)            | Sonnet | 50,000       | 7,500         | $0.26      |
| Comparison + reasoning (×1) | Sonnet | 5,000        | 2,000         | $0.05      |
| Validations/formatting (×3) | Haiku  | 3,000        | 1,500         | $0.01      |
| **Total per journey**       |        | **67,500**   | **14,300**    | **~$0.36** |

**Estimated range: EUR 0.30–1.50 per journey** depending on number of providers (3–7), PDF complexity, and retry loops. Against commission revenue (auto EUR 80–150 per policy), AI costs represent <1–2% of commission revenue even at worst case. LLM API pricing has declined ~50% YoY consistently — margin improves automatically.

### 1.8 Team

| Person             | Role                                                                         |
| ------------------ | ---------------------------------------------------------------------------- |
| Gonçalo Reis       | CEO — business, domain expertise, regulatory, insurer relationships          |
| Rolando            | Qualified Director (ASF) — reviews all AI recommendations, domain validation |
| Build Up Labs / JP | Technical co-founder — architecture, implementation, AI feasibility          |

---

## 2. Market & Integrations

**TL;DR:** Portugal's insurance market has zero standardized APIs. Every integration is bilateral. Day-1 strategy: email automation (works with any insurer that accepts email quote requests — no commercial dependency). MVP targets minimum 3 validated providers. Progression: email → direct API (Fidelidade first) → middleware (lluni/i2S). Long-term play: build a unified integration layer for Portuguese insurance data.

### 2.1 PT Insurance Market Landscape

The Portuguese insurance market is dominated by a handful of large insurers, none of which offer standardized broker APIs. Integration is always bilateral and negotiated per-insurer.

Two middleware platforms — **lluni** and **i2S Brokers** — dominate the broker software layer, each serving 380–400+ broker installations. They provide some insurer connectivity, but access requires commercial agreements and platform lock-in.

There is no Portuguese equivalent of an Open Insurance standard. EIOPA's Open Insurance initiative is exploratory and not imminent. DORA (Digital Operational Resilience Act) applied January 17, 2025, adding ICT risk management requirements.

### 2.2 Competitor Analysis

| Player              | Model                      | Strengths                                    | Weaknesses                                                                             | Relevance                                             |
| ------------------- | -------------------------- | -------------------------------------------- | -------------------------------------------------------------------------------------- | ----------------------------------------------------- |
| **MUDEY**           | Digital insurance mediator | 20K users, 18+ insurer partners, first-mover | 3+ years to build integrations, pivoted to B2B2C (MudeyPRO) — signals D2C is expensive | Direct competitor. Their pivot validates our strategy |
| **ComparaJa**       | Lead-gen / affiliate       | 21M USD raised, SEO-dominant                 | No AI advisory, no post-purchase, pure comparison                                      | Not a tech competitor — different model               |
| **Doutor Finanças** | Human-assisted comparison  | Strong brand, credit + insurance             | Manual processes, not tech-driven                                                      | Not a tech competitor                                 |
| **DECO Proteste**   | Consumer rights org        | High trust                                   | Not a commercial competitor                                                            | Don't compete on trustworthiness                      |

**Key gap agent-resolv fills:** No Portuguese player combines AI-powered intake, automated multi-insurer quoting, structured comparison with reasoning, and human-reviewed recommendations. Post-purchase/claims experience is a consistent gap across all players.

### 2.3 Integration Ladder Strategy

**Tier 1 is not a fallback — it's how every PT broker works today.** Email-based quoting can work with any insurer that accepts email quote requests, with zero commercial dependencies. MVP targets minimum 3 providers with validated email templates and quote email addresses (from Rolando shadow sessions).

```
Tier 1 — MVP Launch: Email / Portal Automation
  → Works with any email-accepting insurer
  → LLM for email/PDF parsing

Tier 2 — Direct APIs
  → Caravela Seguros (Gonçalo has contact)
  → Fidelidade (best API docs, ~35% market share)

Tier 3 — Middleware Partnership
  → lluni WiP API (100+ endpoints, 833 EUR/mo)
  → i2S GIS (no public API)

Long Term — Unified Integration Layer
  → Unified schema across insurers
  → Open to other brokers
```

### 2.4 Provider Assessment

| Insurer                      | Market Share | API Access                              | Priority                   |
| ---------------------------- | ------------ | --------------------------------------- | -------------------------- |
| **Fidelidade**               | ~35%         | 20+ REST APIs on MuleSoft portal, OAuth | #1 for API pilot           |
| **Caravela Seguros**         | Small        | Unknown                                 | #1 for relationship-based  |
| **Tranquilidade / Generali** | Large        | OAuth 2.0 webservice                    | Medium                     |
| **Ageas**                    | Large        | Webservice live                         | Medium                     |
| **Allianz**                  | Large        | Webservice exists                       | Low                        |
| **Lusitania**                | Medium       | Via middleware                          | Low (middleware-dependent) |

### 2.5 Middleware Analysis

**lluni:** B2B insurance management ERP. 380+ mediators, 500M+ EUR portfolio under management. WiP integrates with ~13 insurers. WiP REST API at `wip.lluni.com/Help` — 100+ endpoints. API access only on Premium plan (833 EUR/mo). Risk: platform dependency, unknown if API is available to third-party integrators outside their ERP.

**i2S Brokers:** GIS Mediadores — modular broker management software. 400+ installations. Integrates with 10+ insurers. No public API or developer portal. Integration requires direct commercial agreement. Risk: black box.

**Middleware verdict:** Evaluate after direct insurer integrations prove value. Avoid platform dependency in MVP.

### 2.6 Strategic Positioning

agent-resolv's competitive advantage is not in any single integration — it's in the **normalization layer**. By building standardized schemas (CustomerProfile, NormalizedQuote, ComparisonResult) that work across all insurers regardless of integration method, agent-resolv creates a unified integration layer for Portuguese insurance data.

The near-term product is a broker co-pilot that helps mediators handle more customers without hiring more people. The long-term vision (3–5 years) is a data normalization layer for Portuguese insurance — but that vision is a byproduct of the broker co-pilot generating real transaction data at scale, not a standalone goal.

**Market timing:** FiDA (Financial Data Access) regulation is expected to force open APIs in financial services, including insurance, around 2027–2028. agent-resolv's email-first approach is a deliberate bridge strategy: build the normalization layer now using the same channel brokers already use, so that when APIs arrive, the schema standard is already established and battle-tested.

---

## 3. AI Feasibility

**TL;DR:** AI can reliably parse PT insurance PDFs (95%+ on vida credito and multirriscos — 5 PDFs tested), reconstruct workflow state from email threads (90–93% on 3 threads), and handle conversational intake for auto insurance (10–14 turns). Health insurance is high-risk due to legal exposure and regulatory constraints. Expected overall human handoff rate: ~25–30%.

> **Model pricing context:** LLM API costs have dropped approximately 50% year-over-year across major providers. Current cost estimates should be treated as conservative upper bounds. Model routing — using cheaper models (Haiku) for simple tasks and reserving Sonnet for complex extraction — further reduces per-journey costs.

### 3.1 Feasibility Matrix

| Capability               | Product                 | Confidence  | Notes                                  |
| ------------------------ | ----------------------- | ----------- | -------------------------------------- |
| PDF quote extraction     | Vida Credito            | 95–98%      | Tested on 4 APRIL + Real Vida PDFs     |
| PDF quote extraction     | Multirriscos            | 93%         | Tested on 1 Tranquilidade PDF          |
| PDF quote extraction     | Auto                    | Untested    | No sample PDFs yet                     |
| PDF quote extraction     | Health                  | Untested    | No sample PDFs yet                     |
| Email thread parsing     | Mortgage/insurance      | 90–93%      | Tested on 3 real multi-party threads   |
| Email workflow inference | Brokerage state machine | 90%         | State transitions reliably detected    |
| Conversational intake    | Auto                    | High        | 10–14 turns, best automation candidate |
| Conversational intake    | Home                    | Medium-high | Blocked by rebuild cost estimation     |
| Conversational intake    | Health (estimate)       | Medium      | 2–4 turns for estimate only            |
| Conversational intake    | Health (full QIS)       | Low-medium  | 8–15 turns/person, legal exposure risk |

### 3.2 AI vs Human Intervention

**AI handles autonomously:** Customer greeting & routing, auto insurance profiling (10–14 turns), home insurance profiling (standard apartments), PDF quote parsing (digital PDFs, known formats), quote comparison & recommendation generation, email response matching & document tracking.

**AI with human review:** Home rebuild cost estimation from conversation, health insurance initial estimate, complex auto cases (multi-driver, claims history), final recommendation approval by qualified director.

**Human required:** Health QIS with 2+ pre-existing conditions, classic/unusual vehicles (pre-1981, modified), rural properties (quintas, agricultural), mixed-use properties (home office, commercial), claims handling, policy binding & payment processing.

### 3.3 PDF Quote Parsing

LLM extraction from digital Portuguese insurance quote PDFs is production-viable. No per-insurer template is needed — the model handles format variability across insurers natively.

**Test results across 5 PDFs from 4 insurers:**

| PDF               | Insurer       | Product                            | Accuracy | Key Challenge                                                            |
| ----------------- | ------------- | ---------------------------------- | -------- | ------------------------------------------------------------------------ |
| april.pdf         | APRIL         | Vida Credito (consumer)            | 98%      | Second insured has DOB but no name                                       |
| april2.pdf        | APRIL         | Vida Credito (distributor ITP 60%) | 97%      | Completely different layout from consumer version                        |
| april3.pdf        | APRIL         | Vida Credito (distributor IAD)     | 97%      | Coverage table structure changes by selected option                      |
| realvida.pdf      | Real Vida     | Vida Habitação                     | 95%      | Unique "Idade Actuarial Comum" concept, Euribor-linked declining capital |
| tranquilidade.pdf | Tranquilidade | Multirrisco Casa                   | 93%      | Dual imóvel/recheio capital structure, mixed franchise types             |

Key findings: No two insurers use the same format. Even within APRIL, consumer vs distributor layouts differ completely. A template-based parser would need N templates per insurer per layout variant — LLM extraction avoids this entirely. European numeric formatting (1.234,56), Portuguese abbreviations, and irregular table structures are handled reliably.

**What's untested:** Scanned/image-based PDFs (OCR pathway), password-protected PDFs, auto insurance quote PDFs, health insurance quote PDFs, multi-product bundle PDFs, renewal/amendment PDFs.

**Production pipeline design:**

```
PDF → Text extraction → LLM structured extraction → Zod validation → Confidence scoring → Human review (if low confidence)
```

### 3.4 Email Thread Parsing

Claude can extract structured workflow data from multi-party Portuguese mortgage/insurance email threads and reconstruct the brokerage state machine.

**Test results across 3 real email threads (56 messages total):**

| Thread                      | Messages | Parties | Accuracy | Key Capability                                                       |
| --------------------------- | -------- | ------- | -------- | -------------------------------------------------------------------- |
| PHB-391-2024                | 14       | 5       | 93%      | Insurance cross-sell detection, bank requirement extraction          |
| RE_ 1033499                 | 24       | 3       | 91%      | Full mortgage lifecycle reconstruction, out-of-band action inference |
| Solicitação de documentação | 18       | 3       | 92%      | Credit broker workflow, 28-attachment categorization                 |

Key findings: The three test threads tell one complete story — same mortgage transaction across different parties. LLM reconstructs the full timeline from fragmented threads. Insurance cross-sell moments are reliably detected. Document request/fulfillment tracking works across messages. Participant disambiguation works despite name/case/accent variations. 74 total attachments across 3 threads — categorization is reliable.

**Inferred workflow state machine:**

```
DOCUMENT_COLLECTION → BANK_SUBMISSION → INSURANCE_QUOTING → MORTGAGE_APPROVAL → ESCRITURA_PREPARATION → COMPLETED
```

**Accuracy challenges:** Sentiment detection ~80% (urgency signals in Portuguese formal email are subtle). Out-of-band action inference ~85% (some actions happen via SMS/phone). Insurer-to-broker email threads **not tested** — critical gap.

**Email matching strategy:** Deterministic-first with probabilistic fallback. Primary signal: correlation token (REF-AR-XXXX) in subject and body for 100% match. Fallback signals: subject line fuzzy match, sender domain lookup, LLM body extraction, timestamp window. Insurer email behavior is untested — critical gap for Rolando sessions.

### 3.5 Conversational Intake (WhatsApp / Widget)

**Auto insurance — best MVP candidate:** 10–14 conversational turns. Matrícula (license plate) auto-fills ~6 fields via VIS/DUA database lookup. Clearest field structure, lowest regulatory risk. Expected handoff: 5–10% standard, 15–20% multi-driver/complex.

**Home insurance — viable with caveats:** 14–18 turns. Blocked by rebuild cost estimation: capital do edifício ≠ market value, can differ 3–5× in Lisbon. MVP approach: INE construction cost lookup table (~1,200–1,500 EUR/m² standard construction) as proxy, flagged for human review. Expected handoff: 15–20% standard apartment, 40–50% moradia/quinta/unusual.

**Health insurance — high complexity:** Two-phase flow: initial estimate (2–4 turns) + full QIS (8–15 turns per insured). Family of 4 with conditions: 28–55 messages total. QIS responses are legal declarations — false declarations can void contract. GDPR Article 9 (special category health data) requires explicit consent. EU AI Act classifies AI for health insurance risk assessment as high-risk (Annex III, 5(b)). Expected handoff: 10–15% individual no conditions, 50–70% individual 2+ conditions.

> Health insurance feasibility is documented here for completeness. MVP scope may exclude health entirely — the regulatory complexity, high handoff rates, and QIS legal exposure make health a candidate for a dedicated post-MVP phase with its own legal review.

**PT Portuguese language design:** The intake agent must use colloquial Portuguese, not insurance jargon ("seguro do carro" not "seguro automóvel", "o que pago por mês" not "prémio"). ~300,000+ Brazilian residents in Portugal — agent must detect PT-BR patterns.

**Human handoff rates:**

| Scenario                         | Handoff Rate |
| -------------------------------- | ------------ |
| Auto standard                    | 5–10%        |
| Auto multi-driver/claims         | 15–20%       |
| Home standard apartment          | 15–20%       |
| Home moradia/quinta/unusual      | 40–50%       |
| Health individual, no conditions | 10–15%       |
| Health individual, 2+ conditions | 50–70%       |
| Health family 4+                 | 30–45%       |
| **Overall weighted average**     | **~25–30%**  |

### 3.6 Cross-Spike Insights

1. **Email threads reference the same PDFs** tested in the PDF spike — the credit broker distributed APRIL and Tranquilidade quotes that appear in both test sets. Validates end-to-end data flow.
2. **Insurance cross-sell moments in email threads** create opportunities for conversational intake — when a bank requires insurance for mortgage approval, agent-resolv can proactively initiate the intake flow.
3. **Coverage naming varies across insurers:** "IDPAC60" (Tranquilidade) vs "ITP 60%" (APRIL) = same concept. The normalization schema must map insurer-specific names to canonical coverage types.
4. **Document dependency is the biggest flow-breaker** across all products — ~30% of fields require documents the customer doesn't have at hand. The "send a photo, I'll extract the data" pattern is critical for reducing drop-off.

### 3.7 Gap Analysis — What Still Needs Testing

| Gap                                       | Impact                                            | When to Test            |
| ----------------------------------------- | ------------------------------------------------- | ----------------------- |
| Insurer-to-broker email format            | Critical for email matching in brokerage workflow | Rolando shadow sessions |
| Quote PDFs for selected MVP product types | Needed for MVP                                    | Rolando shadow sessions |
| Health insurance quote PDFs               | Needed for health product launch                  | Post-MVP                |
| Scanned/image-based PDFs                  | Determines if OCR pathway is needed               | Rolando shadow sessions |
| Insurer automated email responses         | May differ from human-written emails              | Phase 2 testing         |
| Claims email thread structure             | Needed for claims feature                         | Post-MVP                |

---

## 4. Technical Architecture

**TL;DR:** Turborepo monorepo following the BuL template. Mastra agents in `packages/agents/`, Zod schemas in `packages/contracts/`, business logic in `packages/core/` with Effect-TS. API via Hono + Node.js with `@mastra/hono` adapter. Durable workflow orchestration via `@mastra/inngest` (crash-safe steps, suspend/resume, retries, observability). Postgres + Drizzle ORM (Neon in production). Mastra stores workflow snapshots in a separate `mastra` Postgres schema. Brokers interact via email interface; dashboard and MCP server follow in Phase 3.

### 4.1 Architecture Decision Records

#### ADR-001: Monorepo Package Structure

**Decision:** Follow the BuL template monorepo layout with `apps/`, `packages/`, and `tooling/` directories. Enforce dependency boundaries with dependency-cruiser.

**Rationale:** Enforced dependency boundaries (packages never import from apps), independent testing (each package has its own test suite), and reusability (`packages/contracts/` is consumed by API, dashboard, MCP server, and agents independently).

**Trade-offs:** (+) Clean separation, testable in isolation, scales to multiple apps. (-) More boilerplate for initial setup (mitigated by template).

#### ADR-002: Database — Postgres + Drizzle ORM

**Decision:** PostgreSQL (Neon in production) with Drizzle ORM for application data. Mastra stores workflow state in a separate `mastra` Postgres schema via `@mastra/pg` PostgresStore.

**Rationale:** No migration tax (no SQLite-to-Postgres migration later). Drizzle is the BuL standard — schema-as-code, type inference, push-based dev workflow. Neon branching for testing migrations and preview deployments.

**Trade-offs:** (+) Production-ready from day one, Neon branching. (-) Requires a Postgres instance for local dev.

#### ADR-003: Workflow Engine — @mastra/inngest

**Decision:** Use `@mastra/inngest` (official Mastra package, v1.1.1) as the single workflow orchestration layer. Replace Mastra's native execution engine with Inngest's durable runtime.

**Rationale:** agent-resolv's 6-step pipeline has 2 suspend points that may persist for days (waiting for insurer email responses, human director review). Crash-safe durability (crash at step 4 replays from step 4, not step 1). Inngest Cloud dashboard provides tamper-evident run history satisfying ASF audit trail and EU AI Act Art. 12 record-keeping. Built-in flow control: concurrency limits, rate limiting, throttle, debounce. Cron scheduling for email polling, audit cleanup, monitoring.

**Trade-offs:** (+) Crash-safe, durable, observable, retriable. (-) `@mastra/inngest` is young (v1.1.1) — pin version, test suspend/resume thoroughly. (-) Adds Inngest as an infrastructure dependency.

#### ADR-004: API Runtime — Hono + Node.js

**Decision:** Use Hono as the HTTP framework running on Node.js, with `@mastra/hono` as the server adapter. Not Elysia + Bun (the BuL default).

**Rationale:** `@mastra/hono` auto-exposes agents, workflows, and tools as HTTP endpoints. No `@mastra/elysia` exists. Foresight (the only BuL project running Mastra in production) chose Hono + Node.js. Bun has multiple resolved friction issues with Mastra.

**Trade-offs:** (+) Official Mastra adapter, zero custom glue. (-) Departure from BuL standard (Elysia + Bun).

#### ADR-005: Mastra as Standalone Package

**Decision:** Place Mastra agents, tools, memory, and the Mastra instance in `packages/agents/` as a standalone workspace package.

**Rationale:** Clean import boundary (`apps/api/`, `apps/mcp/`, `apps/engine/` all import from `@agent-resolv/agents`). Independent playground via `mastra dev`. Testable in isolation.

#### ADR-006: AI-First Development — Hybrid Effect-TS

**Decision:** Use Effect-TS for business logic in `packages/core/` (error-heavy services, domain logic) and plain TypeScript for `apps/` and UI-facing code.

**Rationale:** The codebase is designed to be generated and maintained by AI coding tools (Claude Code). Effect-TS typed errors in core give the AI explicit failure paths. Plain TypeScript in apps keeps the app layer simple. Strict package boundaries (dependency-cruiser) prevent circular dependencies. Zod schemas as contracts give ground truth for validation.

**Trade-offs:** (+) Stronger type safety where it matters most. (-) Two coding styles in one codebase. (-) Effect-TS has a smaller community. If too cumbersome, `packages/core/` can be refactored to plain TypeScript with a custom Result type.

### 4.2 System Architecture

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  Input Channels                                                             │
│  ┌──────────────┐ ┌──────────────┐ ┌──────────────┐ ┌──────────────┐       │
│  │ Email Forward │ │ Contact Form │ │ Manual Paste │ │ Intake Widget│       │
│  │ (broker fwd)  │ │ Webhook      │ │ (phone notes)│ │ (Phase 3)    │       │
│  └──────┬───────┘ └──────┬───────┘ └──────┬───────┘ └──────┬───────┘       │
│         └────────────────┴────────────────┴────────────────┘               │
│                                    │                                        │
│  ┌─────────────────────────────────▼──────────────────────────────────────┐ │
│  │  API Layer — Hono + @mastra/hono                                      │ │
│  └──────────────────────┬─────────────────────────────────────────────────┘ │
│                         │                                                   │
│  ┌──────────────────────▼──────────────────────────────────────────────────┐│
│  │  Agent Layer — Mastra Agents                                           ││
│  │  ┌─────────────────┐ ┌──────────────────┐ ┌──────────────────────────┐ ││
│  │  │ Orchestrator    │ │ Lead Processing  │ │ Quote Comparison Agent  │ ││
│  │  │ (routes intent) │ │ Agent            │ │                          │ ││
│  │  └────────┬────────┘ └──────────────────┘ └──────────────────────────┘ ││
│  └───────────┼────────────────────────────────────────────────────────────┘│
│              │                                                              │
│  ┌───────────▼────────────────────────────────────────────────────────────┐ │
│  │  Workflow Layer — @mastra/inngest (6 Steps, 2 Suspend Points)         │ │
│  │  1. validate-profile → 2. request-quotes → 3. collect-responses       │ │
│  │  [SUSPEND: wait for emails] → 4. compare-quotes → 5. human-review    │ │
│  │  [SUSPEND: qualified director] → 6. send-recommendation               │ │
│  └───────────┬────────────────────────────────────────────────────────────┘ │
│              │                                                              │
│  ┌───────────▼──────────┐  ┌─────────────────┐  ┌────────────────────────┐ │
│  │ Tool Layer           │  │ Schema Layer     │  │ Data Layer             │ │
│  │ emailQuoteRequestTool│  │ CustomerProfile  │  │ PostgreSQL / Neon      │ │
│  │ emailResponseParser  │  │ NormalizedQuote  │  │ public: app data       │ │
│  │ auditLogTool         │  │ QuoteRequest     │  │ mastra: workflow state │ │
│  │ profileValidatorTool │  │ ComparisonResult │  │                        │ │
│  │ Provider Registry    │  │                  │  │                        │ │
│  └──────────────────────┘  └─────────────────┘  └────────────────────────┘ │
└─────────────────────────────────────────────────────────────────────────────┘

Phase 3 additions: Broker Dashboard (Vite + React SPA), MCP Server (demos + AI-native consumers)
```

### 4.3 Monorepo Structure

```
agent-resolv/
  apps/
    api/              # Hono + Node.js + @mastra/hono adapter
    engine/           # Inngest background job workers
    mcp/              # MCP server (Phase 3)
    web/              # Vite + React — broker dashboard (Phase 3)

  packages/
    agents/           # Mastra instance, agents, tools, memory
    contracts/        # Zod schemas — single source of truth for all types
    core/             # Business logic (Effect-TS)
    db/               # Drizzle ORM schema + client (Postgres/Neon)
    env/              # Type-safe env vars (@t3-oss/env-core)
    auth/             # Better Auth configuration
    analytics/        # PostHog (server + browser)
    logging/          # Pino structured logging
    ui/               # Shared React components (Phase 3)
    api-client/       # Typed fetch wrapper for frontends

  tooling/
    typescript/       # Shared tsconfig variants
    tailwind/         # Tailwind factory/preset
```

Dependency direction: Apps import from packages. Packages never import from apps. Enforced by dependency-cruiser at lint time. Package manager: pnpm with workspaces. Build system: Turborepo with `concurrency: 20`.

### 4.4 Email Interface — Primary (MVP)

The email interface is agent-resolv's primary broker-facing channel. Brokers forward leads to an intake address, receive extracted profiles and comparisons via email, and take all state-changing actions via authenticated action links embedded in those emails (opaque token, consumed on POST only). Email replies handle edits and questions only.

**Components:**

- **Inbound processing:** Dedicated intake address (`intake@agent-resolv.com`) receives forwarded leads. Lead processing agent extracts structured profile.
- **Outbound comparison emails:** Profile confirmations, progress updates, and comparison results sent via Resend.
- **Email replies (draft-level only):** Broker replies are parsed by LLM for profile edits and questions. Never change workflow state.
- **Authenticated action links (all state changes):** Profile confirmation, comparison trigger, cancellation, and binding approval require opaque-token links with Better Auth session verification. Tokens are server-side, 24h TTL. GET is non-destructive (safe from email scanner prefetch); token consumed on authenticated POST only.
- **Reply matching:** Primary: high-entropy per-request reply-to alias. Secondary: Message-ID/In-Reply-To headers. Tertiary: subject token `[AR-XXXX]`.
- **Expired-token recovery:** Explicit expiry state, allow link reissue to broker's registered address only. Rate-limited (max 3 per request per hour) and logged.

### 4.5 MCP Server — Secondary Interface (Phase 3)

The MCP server exposes the brokerage pipeline as tools for investor/partner demos and AI-native consumers. It wraps the same agents and workflows as the email interface.

| Tool                      | Description                                                                | Who Uses It      |
| ------------------------- | -------------------------------------------------------------------------- | ---------------- |
| `process_lead`            | Extract structured profile from unstructured input                         | Broker           |
| `submit_quote_request`    | Validate profile and send quote requests                                   | Broker           |
| `check_quote_status`      | Check which insurers have responded                                        | Broker           |
| `get_comparison`          | Retrieve AI-generated quote comparison                                     | Broker           |
| `approve_recommendation`  | Qualified director approves/rejects (requires `qualified_director` role)   | Broker (QD only) |
| `request_insurance_quote` | End-to-end: provide details, get quotes, receive comparison. Asynchronous. | Consumer         |
| `list_providers`          | List available insurance providers and supported products                  | Both             |

### 4.6 Hybrid Orchestration Pattern

**Agent Network** handles the broker-facing layer — routing incoming leads and broker actions to the right sub-agent or workflow. **Durable Workflows via @mastra/inngest** handle the brokerage pipeline — the 6-step quote-to-recommendation process where order matters and two steps require external input. This hybrid avoids the problems of pure agent orchestration (unpredictable routing, hard to audit) while keeping the broker-facing layer flexible.

### 4.7 Schema Layer (Core IP)

The Zod schema layer is agent-resolv's core intellectual property. It normalizes data across insurers regardless of integration method, making the system provider-agnostic. Schemas live in `packages/contracts/` — the single source of truth.

**CustomerProfile** — Discriminated union by product type. All types share BaseProfile (id, NIF, name, email, phone, dateOfBirth, address). AutoProfile extends with vehicle details, driver history, desired coverage. HomeProfile extends with property details. LifeProfile extends with health data and coverage preferences. HealthProfile extends with member QIS responses and GDPR consent tracking.

**NormalizedQuote** — Provider-agnostic quote representation: premium (annual/monthly/payment options), coverage array (type, description, limit, deductible, included/optional), exclusions, conditions, validity, confidence score, parsing notes.

**QuoteRequest** — Customer ID, product type, profile, selected providers, urgency, notes.

**ComparisonResult** — Quotes array, recommendation (bestValue, bestCoverage, cheapest, reasoning in Portuguese), comparison matrix by coverage type.

### 4.8 6-Step Brokerage Workflow

Orchestrated by `@mastra/inngest`. Every step is a durable operation.

```
[Start] → 1. ValidateProfile → 2. RequestQuotes → 3. CollectResponses
          [SUSPEND POINT 1: Inngest function completes. Snapshot stored in Postgres.
           Resumes via new Inngest event when emails arrive or timeout.]
→ 4. CompareQuotes → 5. HumanReview
          [SUSPEND POINT 2: Qualified director reviews via authenticated action link (Phase 2)
           or broker dashboard (Phase 3). Inngest run history = audit trail.]
→ 6. SendRecommendation → [End]
```

**Suspend Point 1 (collect-responses):** Workflow suspends after sending quote request emails. Inbound email webhook handler matches responses, parses PDF attachments, calls `workflow.resume()`. No polling, no long-running connections, survives server restarts.

**Suspend Point 2 (human-review):** Workflow suspends for qualified director review. Resume payload: `{ approved, selectedQuoteId?, reviewerNotes?, reviewerId }` (authenticated, must have `qualified_director` role).

### 4.9 Inbound Email Processing

Provider: Resend (webhook-based inbound email).

```
Inbound email webhook → Deduplicate (by Message-ID) → Inngest event
→ Match to pending request → Extract PDF attachments → emailResponseParserTool (LLM)
→ Store NormalizedQuote (idempotent upsert) → Check completion threshold
→ workflow.resume() (idempotent)
```

**Idempotency controls:** Three dedup layers: (1) Webhook dedup by Message-ID header, (2) Quote storage dedup by (quote_request_id, provider_id), (3) Resume dedup via Inngest event deduplication key.

**Email matching — insurer responses:** Correlation-token-first, probabilistic fallback. Uncertain-match rule: fuzzy/LLM matches are never auto-routed — quarantined for manual review.

**Inbound trust controls (must ship for Phase 2):** Verify webhook signatures, enforce sender allowlist per broker, evaluate SPF/DKIM/DMARC, quarantine unknown/failed-authentication emails, apply inbound rate limits.

### 4.10 Database Schema

Postgres with Drizzle ORM. Two schemas: `public` (application data), `mastra` (Mastra workflow state via `@mastra/pg`).

```
-- public schema (Drizzle ORM)
customers (id, nif, name, email, phone, created_at, updated_at)
customer_profiles (id, customer_id FK CASCADE, product_type, profile_data JSONB, version, created_at)
quote_requests (id, customer_id FK CASCADE, product_type, providers JSONB, status, requested_at, notes)
quote_request_providers (id, quote_request_id FK CASCADE, provider_id, status, email_sent_at, response_received_at)
normalized_quotes (id, quote_request_id FK CASCADE, provider_id, premium_data JSONB, coverage_data JSONB, confidence, raw_document_url, received_at)
comparisons (id, quote_request_id FK CASCADE, recommendation JSONB, comparison_matrix JSONB, generated_at)
reviews (id, comparison_id FK CASCADE, reviewer_id, approved, selected_quote_id, notes, reviewed_at)
audit_logs (id, action, agent_id, customer_id FK SET NULL, details JSONB, created_at)

-- mastra schema (managed by @mastra/pg PostgresStore)
-- Workflow runs, agent memory, execution snapshots — do not modify directly
```

Dev workflow: `drizzle-kit push`. Production: `drizzle-kit generate` + `drizzle-kit migrate`.

### 4.11 Model Routing

| Task                              | Model         | Rationale                                         |
| --------------------------------- | ------------- | ------------------------------------------------- |
| Lead processing / data extraction | Claude Sonnet | Complex structured extraction from diverse inputs |
| PDF parsing / data extraction     | Claude Sonnet | Complex structured extraction                     |
| Quote comparison & recommendation | Claude Sonnet | Reasoning over multiple quotes                    |
| Simple validations, formatting    | Haiku         | Cost optimization for simple tasks                |
| Orchestrator routing              | Claude Sonnet | Needs to understand intent                        |

Single LLM provider (Anthropic) simplifies billing, API key management, and failure modes.

### 4.12 Tech Stack

| Component        | Choice                                   | Rationale                                    |
| ---------------- | ---------------------------------------- | -------------------------------------------- |
| Monorepo         | Turborepo + pnpm                         | BuL standard                                 |
| API framework    | Hono + Node.js                           | Official `@mastra/hono` adapter              |
| Frontend         | Vite + React 19                          | Phase 3 broker dashboard                     |
| AI framework     | Mastra (pinned)                          | TypeScript-native agents, workflows, tools   |
| Workflow engine  | `@mastra/inngest`                        | Durable execution, crash-safe suspend/resume |
| Auth + RBAC      | Better Auth                              | OSS, self-hosted, organization plugin        |
| Analytics        | PostHog                                  | Product analytics, feature flags             |
| Email (outbound) | Resend                                   | API-first, Vercel ecosystem                  |
| Email (inbound)  | Resend webhooks                          | Same provider, webhook-based                 |
| Database         | PostgreSQL (Neon)                        | Drizzle ORM, branching                       |
| ORM              | Drizzle                                  | Schema-as-TypeScript, type inference         |
| Validation       | Zod                                      | Runtime type checking, schema-first          |
| Business logic   | Effect-TS (core only)                    | Typed errors for domain logic                |
| Logging          | Pino                                     | Structured JSON                              |
| Env validation   | @t3-oss/env-core                         | Type-safe env split                          |
| Background jobs  | Inngest                                  | Durable functions, retries, cron             |
| Linting          | Biome                                    | Fast, single config                          |
| Deployment       | Vercel (frontend) + Docker/Coolify (API) | Static + server split                        |

### 4.13 Infrastructure Cost Estimate (MVP)

| Service         | Free Tier               | Production Estimate      | Notes                             |
| --------------- | ----------------------- | ------------------------ | --------------------------------- |
| Neon (Postgres) | 0.5 GB storage          | ~EUR 19/month            | Branching included                |
| Inngest         | 25K function runs/month | ~EUR 25/month at scale   | Free tier covers early production |
| Resend          | 3,000 emails/month      | ~EUR 20/month            | Outbound + inbound webhooks       |
| Vercel          | Hobby tier              | Free (dashboard)         | Upgrade to Pro if needed          |
| Anthropic API   | Pay-per-use             | TBD (cost spike pending) | Main variable cost                |
| PostHog         | 1M events/month         | Free                     | Cloud hosted                      |

**Total fixed infrastructure: under EUR 100/month for MVP** (excluding LLM API costs).

---

## 5. Compliance & Risk

**TL;DR:** Three regulatory frameworks apply: ASF (insurance regulator — audit trails, qualified director), GDPR (Article 9 for health data), and EU AI Act (likely high-risk classification pending formal assessment, Aug 2026 deadline). The technical architecture assumes high-risk and satisfies requirements through audit logging, human-review suspend points with RBAC-enforced approval, and explicit consent tracking. DORA adds ICT risk management requirements.

### 5.1 Regulatory Framework

#### ASF (Autoridade de Supervisão de Seguros e Fundos de Pensões)

The MVP operates as broker co-pilot software used by already-licensed brokers — no own ASF license required at this stage. Own license becomes necessary if/when agent-resolv operates as a broker directly (D2C, Phase 4+).

| Requirement                 | How We Satisfy It                                                                                                                                                                                                                                                                                                    |
| --------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Qualified Director**      | Rolando reviews all AI recommendations (human-review suspend point). Approval restricted to `qualified_director` role via Better Auth RBAC.                                                                                                                                                                          |
| **Audit Trail**             | Dual-layer: `auditLogTool` logs every action in Postgres + Inngest Cloud provides tamper-evident step-level execution history. Actions logged: intake_started, quote_requested, quote_received, comparison_generated, recommendation_made, human_review_requested, human_review_completed, policy_binding_initiated. |
| **Record Retention**        | All customer profiles, quotes, comparisons, reviews, and audit logs persisted. Retention period to be confirmed (minimum 5 years expected).                                                                                                                                                                          |
| **Customer Identification** | NIF (9-digit Portuguese tax number) validation at intake.                                                                                                                                                                                                                                                            |
| **Disclosure**              | Intake agent identifies itself as AI working for a licensed broker. Never promises pricing. Notes that quotes come from providers and recommendations require qualified director review.                                                                                                                             |

#### GDPR

| Requirement                           | How We Satisfy It                                                                                                                                                                                      |
| ------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Lawful Basis**                      | Candidate: legitimate interest for non-health (Art. 6(1)(f)); explicit consent for health (Art. 9). Final basis pending legal counsel.                                                                 |
| **Article 9 — Special Category Data** | HealthProfile includes `gdprConsent` object with explicitHealthDataConsent, consentTimestamp, consentMethod. Consent collected before any health questions.                                            |
| **Data Minimization**                 | Intake agent collects only fields required for the requested product type. Schema validation enforces required vs optional per product.                                                                |
| **Right to Access / Erasure**         | FK cascade ensures deletion propagates. Audit logs retained with customer ID set to null (legal obligation, Art. 17(3)(b)). Deletion strategy (hard delete vs pseudonymization) pending legal counsel. |
| **Transparency**                      | AI-generated recommendations labeled as such. Comparison reasoning in Portuguese. Human review disclosed.                                                                                              |

#### EU AI Act

Insurance recommendation engines likely fall under **high-risk classification** (Annex III) — formal assessment pending legal counsel. Architecture assumes high-risk from day one. Compliance deadline: **August 2026**.

| Requirement         | Article | How We Satisfy It                                                                                                  |
| ------------------- | ------- | ------------------------------------------------------------------------------------------------------------------ |
| **Human Oversight** | Art. 14 | Human-review suspend point (workflow step 5) via `@mastra/inngest`. Inngest records full review lifecycle.         |
| **Transparency**    | Art. 13 | Customers informed they're interacting with AI. Comparison includes reasoning. Confidence scores flag uncertainty. |
| **Risk Management** | Art. 9  | Documented risk matrix. Mitigation strategies per risk. PostHog monitoring.                                        |
| **Data Governance** | Art. 10 | No model fine-tuning on customer data. Structured output with Zod validation bounds hallucination risk.            |
| **Record Keeping**  | Art. 12 | Dual-layer: audit_logs + Inngest Cloud run history.                                                                |
| **Accuracy**        | Art. 15 | Confidence scores on PDF parsing. Human review catches errors. Eval loop (Phase 3) with golden dataset.            |
| **Robustness**      | Art. 15 | Zod validation at every boundary. Workflow suspend points prevent autonomous action on critical decisions.         |

> **Health insurance AI** has additional classification under Annex III, 5(b): extends to life and health insurance risk assessment. The health QIS intake and any health-related recommendation carry the highest regulatory burden.

#### DORA (Digital Operational Resilience Act)

Applied January 17, 2025. ICT risk management via managed services (Vercel, Neon, Inngest Cloud, Resend) with defined SLAs. PostHog alerting for anomalies. LLM provider dependency (Anthropic) documented. Mastra dependency mitigated by schema decoupling and version pinning.

### 5.2 Risk Matrix

**Critical — Mitigate Now (High Impact, Medium+ Probability):**
- Health QIS legal exposure
- EU AI Act non-compliance
- ASF compliance gaps
- Rebuild cost underinsurance

**Monitor (High Impact, Low Probability):**
- AI hallucination in recommendations
- PDF parsing accuracy too low
- Mastra API breaking changes
- Email delivery/reception issues

**Plan Mitigation (Medium Impact, Medium Probability):**
- Provider response time too slow

**Accept (Low Impact, High Probability):**
- PT-BR customer without NIF
- Document dependency breaks flow

### 5.3 Risk Mitigation Strategies

| Risk                                 | Impact | Probability | Mitigation                                                                                                                                                                       |
| ------------------------------------ | ------ | ----------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Health QIS legal exposure**        | High   | Medium      | Defer health to post-MVP. When implemented: explicit GDPR Art. 9 consent, human review for all health recommendations, QIS = legal declarations — offer phone for complex cases. |
| **EU AI Act non-compliance**         | High   | Medium      | Human-review suspend satisfies Art. 14. Audit trail satisfies Art. 12. Confidence scoring satisfies Art. 15. Built into architecture, not bolted on.                             |
| **ASF compliance gaps**              | High   | Low         | Rolando reviews every recommendation. Full audit trail from day one. Legal counsel review before launch.                                                                         |
| **Rebuild cost underinsurance**      | High   | High        | INE construction cost lookup table as proxy. All estimates flagged for human review. Caderneta predial photo extraction as secondary signal.                                     |
| **AI hallucination in recs**         | High   | Low         | Structured output with Zod validation. Comparison agent works only from NormalizedQuote data. Human review catches errors.                                                       |
| **PDF parsing accuracy too low**     | High   | Low         | Provider-specific parsing prompts. Human review. Phase 3: eval loop with golden dataset.                                                                                         |
| **Mastra API breaking changes**      | Medium | Low-Med     | Pin exact versions. Schemas decoupled from Mastra internals. `@mastra/inngest` provides durable workflows independent of native engine.                                          |
| **Rolando single-person dependency** | High   | Medium      | Document all domain knowledge from shadow sessions. Onboard 1–2 additional reviewers in Phase 2. Cross-train Gonçalo on review workflow.                                         |

### 5.4 Open Questions for Legal Counsel

**Pre-Phase 1:** (1) AI Act classification — formal assessment needed? (2) Cross-border data transfers — Anthropic is US-based, SCCs sufficient? DPIA needed? (3) Deletion strategy — hard delete vs pseudonymization vs soft-delete?

**Pre-launch:** (4) Record retention period. (5) Health data processing lawful basis. (6) QIS legal declarations — broker liability if AI-captured response is inaccurate? (7) DORA scope — proportionality thresholds for intermediaries of our size?

---

## 6. Implementation Roadmap

**TL;DR:** Three-phase MVP build (12–16 weeks). Phase 1: foundation (monorepo, schemas, auth, lead processing). Phase 2: durable brokerage workflow, email integration, email broker interface. Phase 3: broker dashboard, MCP server, PDF parsing improvements, intake widget. Post-MVP: WhatsApp, credit intermediary, B2B2C platform.

### 6.1 Timeline Assumptions & Flexibility

**Realistic range: 12–16 weeks.** The 12-week target assumes ideal conditions; 16 weeks accounts for likely delays.

Baseline assumptions: Rolando shadow sessions complete by end of March 2026, legal counsel responds within expected timeframes, no major technical surprises from real-world insurer data, AI model performance matches feasibility spike results.

The phased structure with decision gates absorbs delays: if Phase 1 takes 4 weeks instead of 3, Phase 3 scope compresses. If legal blockers slip by 2 weeks, Phase 2 starts later but scope is preserved. If PDF parsing accuracy is below threshold, additional parsing spike added, Phase 3 features deprioritized.

**The minimum viable launch** is the email broker interface + durable workflow + 3 working providers. Everything beyond that is additive.

### 6.2 Phase 1 — Foundation (Weeks 1–3)

**Goal:** Lead processing agent that can extract structured customer profiles from diverse broker inputs (forwarded emails, contact form submissions, pasted phone notes). Auth + RBAC foundation.

| Deliverable              | Description                                                                                                       | Success Criteria                                    |
| ------------------------ | ----------------------------------------------------------------------------------------------------------------- | --------------------------------------------------- |
| Monorepo structure       | Turborepo, `packages/agents/` (Mastra), `packages/contracts/` (Zod), `packages/db/` (Drizzle), `apps/api/` (Hono) | `pnpm --filter agents dev` starts Mastra playground |
| Schema layer             | Zod schemas for CustomerProfile (4 types), NormalizedQuote, QuoteRequest, ComparisonResult                        | All schemas pass valid/invalid tests                |
| Database schema          | Drizzle ORM tables, Postgres (Neon), Mastra PostgresStore in `mastra` schema                                      | `drizzle-kit push` applies. FK cascades verified.   |
| Auth + RBAC              | Better Auth with roles (qualified_director, broker, viewer). Organization plugin.                                 | Role gates approval actions.                        |
| Lead processing agent    | Extracts customer profiles from unstructured broker input                                                         | 90%+ field extraction accuracy                      |
| Compliance tools         | auditLogTool, profileValidatorTool                                                                                | Every lead logged, invalid profiles rejected        |
| Lead ingestion endpoints | Hono routes: email forward webhook, contact form webhook, manual paste                                            | All 3 input methods produce valid leads             |

**POC tests:** 10 forwarded emails, 5 form submissions, 5 pasted phone notes, edge cases (incomplete, mixed product, PT-BR, missing NIF).

**Decision gate:** Proceed to Phase 2 if lead processing agent produces valid output from 90%+ of test inputs including partial profiles with correct missing field identification.

### 6.3 Phase 2 — Email-Based Brokerage Pipeline + Email Broker Interface (Weeks 4–7)

**Goal:** End-to-end brokerage workflow using email as provider integration channel. Two suspend points. Broker receives AI-generated comparison to review, adjust, and deliver via email and authenticated action links.

| Deliverable                | Description                                                                           | Success Criteria                                                    |
| -------------------------- | ------------------------------------------------------------------------------------- | ------------------------------------------------------------------- |
| Provider registry          | Static registry for insurers (minimum 3, up to 7)                                     | 3+ providers with valid email templates and addresses               |
| Email tools                | emailQuoteRequestTool, emailResponseParserTool                                        | Professional Portuguese emails sent, PDFs parsed to NormalizedQuote |
| Quote comparison agent     | Generates ComparisonResult from NormalizedQuotes                                      | Valid structured output with Portuguese reasoning                   |
| Brokerage workflow         | 6-step durable workflow via `@mastra/inngest`, 2 suspend points                       | Full suspend/resume completes. Crash recovery verified.             |
| Inbound email bridge       | Webhook handler matching responses to requests, trust controls                        | >99% match rate. Uncertain matches quarantined.                     |
| Orchestrator               | Agent network routing to sub-agents/workflows                                         | Correct routing for all test types                                  |
| **Email broker interface** | Outbound notifications, authenticated action links, reply-based edits, alias matching | Rolando completes 5 end-to-end cases. Reply parsing >80%.           |

> **Scope trap warning:** The email broker interface is five subsystems: (1) outbound notification emails with templating, (2) authenticated action link infrastructure, (3) reply-to alias routing, (4) reply parsing via LLM, (5) token lifecycle management. If Phase 2 slips, **cut reply parsing to Phase 3** — action links alone are sufficient for MVP.

**POC tests:** 5 quote request emails per product type, 10 real PDF parsing tests, end-to-end workflow with mock responses, 3 real comparison quality evaluations, 5 end-to-end broker email flow tests with Rolando, 10 reply parsing accuracy tests, inbound trust control verification.

**Decision gate:** Proceed to Phase 3 if workflow completes end-to-end, PDF parsing >90%, Rolando rates recommendation quality as "acceptable", email interface completes 5 e2e cases, inbound matching >99%, and all GDPR legal gates are met (legal basis confirmed, DPA signed, sub-processor register complete, retention policy defined, PII minimization verified, encryption verified).

### 6.4 Phase 3 — Production Hardening & Widget (Weeks 8–12)

**Goal:** Broker dashboard for Rolando/Gonçalo. MCP server for demos. Improved PDF parsing with eval loop. Observability. Embeddable intake widget.

| Deliverable              | Description                                                                                  | Success Criteria                                                   |
| ------------------------ | -------------------------------------------------------------------------------------------- | ------------------------------------------------------------------ |
| Broker dashboard         | Vite + React SPA: leads, requests, workflows, comparison review, approve/reject, audit trail | Rolando completes 5 reviews in <3 min each                         |
| MCP server               | Broker + consumer tools wrapping the pipeline, demo-ready                                    | Process a lead from Claude Desktop                                 |
| PDF parsing improvements | Per-field confidence, low-confidence flagging, human correction logging                      | Golden dataset per provider started                                |
| Observability            | OpenTelemetry tracing, PostHog product analytics                                             | Key metrics tracked: accuracy, approval rate, e2e time, token cost |
| Embeddable intake widget | Customer-facing Portuguese conversational widget for broker websites                         | Valid profile in <14 turns                                         |
| RAG                      | Indexed ASF regulations, provider docs, PT insurance terms                                   | Agent answers regulatory questions accurately                      |
| API prep                 | Architecture ready for direct API integrations                                               | Tool interface documented, swap path clear                         |

**POC tests:** 5 dashboard reviews, 10 widget conversations, 5 MCP broker flows, MCP consumer e2e test.

### 6.5 Scope-Cut Protocol

If decision gates slip or timelines compress, features drop in this order:

| Priority          | What Stays                                                                                                       | What Drops First                         |
| ----------------- | ---------------------------------------------------------------------------------------------------------------- | ---------------------------------------- |
| P0 (must ship)    | Lead processing + email brokerage workflow + email broker interface (action links, notifications, alias routing) | Reply parsing (defer to Phase 3)         |
| P1 (should ship)  | PDF parsing improvements, broker dashboard (basic), reply parsing                                                | RAG, embeddable intake widget            |
| P2 (nice to have) | MCP server (demos), observability                                                                                | MCP consumer tools, API integration prep |

**Rule:** If Phase 2 slips, Phase 3 scope is reduced, not deferred entirely. The minimum viable launch is the email broker interface + durable workflow + 3 working providers.

### 6.6 Dependencies and Decision Gates

| Decision                              | When                        | Blocker For                  |
| ------------------------------------- | --------------------------- | ---------------------------- |
| Rolando shadow sessions complete      | End of March 2026           | MVP scope finalization       |
| MVP product types selected            | Early April 2026            | Phase 1 start                |
| AI Act classification + DPIA scoping  | Pre-Phase 1 (legal counsel) | Architecture assumptions     |
| Deletion strategy                     | Pre-Phase 1 (legal counsel) | Database schema design       |
| Sample PDFs obtained                  | Rolando sessions            | PDF parsing validation       |
| Insurer email format characterized    | Rolando sessions            | Email matching validation    |
| Legal basis confirmed                 | April 7                     | Phase 2 start (hard blocker) |
| DPA template + sub-processor register | April 14                    | Phase 2 start                |
| DPA signed with Rolando's brokerage   | April 28                    | Phase 2 start (hard blocker) |
| Retention policy defined              | April 21                    | Phase 2 start                |
| Operating entity decision             | Pre-Phase 1                 | Entity structure             |
| ASF license application               | Before Phase 4              | D2C operations only          |
| BdP license application               | Before Phase 5              | Credit intermediary          |

### 6.7 Non-PRD Deliverables (Tracked)

| Deliverable                                           | Owner              | When                   | Status                   |
| ----------------------------------------------------- | ------------------ | ---------------------- | ------------------------ |
| GTM one-pager (ICP, pricing hypothesis, channel plan) | Gonçalo            | Pre-Phase 2            | Not started              |
| Data retention / deletion policy                      | Legal counsel + JP | Pre-Phase 2 (April 21) | Blocked on legal counsel |
| Email matching validation protocol                    | JP                 | Phase 2 (post-Rolando) | Not started              |
| AI Act / DORA operational control matrix              | Legal counsel + JP | Pre-launch             | Not started              |

### 6.8 Consolidated Open Questions for Rolando Shadow Sessions

**Integration research:** (1) Current workflow with insurer portals? (2) How fragmented are insurer formats? (3) Caravela tech stack and appetite? (4) Which insurers respond fastest? (5) Fidelidade commercial onboarding?

**PDF parsing:** (6) Document types beyond quote PDFs? (7) Scanned PDFs vs digital? (8) How many distinct formats? (9) Password-protected PDFs common? (10) Renewal vs new business format differences? (11) Structured data (Excel, CSV) instead of PDF?

**Email parsing:** (12) How do you track insurer responses? (13) Typical insurer email response format? (14) How many concurrent threads? (15) Email folder/label system? (16) Claims threads vs quoting threads?

**Conversational intake:** (17) % of customers knowing bonus-malus class? (18) How often do customers have caderneta predial? (19) Typical auto quote turnaround? (20) Most confusing questions? (21) How do you handle rebuild cost estimation?

### 6.9 Recommended Next Spikes

| Spike                                              | Priority | Dependency                                                  |
| -------------------------------------------------- | -------- | ----------------------------------------------------------- |
| Insurer email matching                             | P0       | Rolando shadow sessions                                     |
| PDF parsing for selected MVP products              | P0       | Rolando shadow sessions                                     |
| Scanned PDF / OCR pathway                          | P1       | Rolando sessions                                            |
| Mastra `@mastra/inngest` suspend/resume durability | P1       | Run test workflow, suspend 48+ hours, resume, confirm state |
| VIS/DUA matrícula lookup API                       | P1       | Phase 1 (if auto is MVP product)                            |
| Health insurance PDF parsing                       | P2       | Post-MVP                                                    |

---

## 7. Vision Roadmap — Post-MVP

> The following phases represent the long-term product vision. Not in scope for the current funding round. Timing and scope defined based on MVP results, market feedback, and subsequent funding.

### Phase 4 — Direct Customer Channels

Once the broker co-pilot is proven (PDF parsing validated, email matching reliable, comparison quality approved by Rolando): WhatsApp Business API for customer interaction, widget expansion (multi-product, multi-language), voice channel (exploratory), proactive renewal reminders. **Key gate:** AI accuracy validated on 100+ real broker cases before customers interact directly.

### Phase 5 — Credit Intermediary (BdP License)

Credit product schemas (mortgage, personal loan, credit card), bank integration tools, credit-specific compliance workflows, cross-sell: insurance ↔ credit.

### Phase 6 — Full Autonomous Broker & Platform

Self-service customer portal (end-to-end without broker in the loop except regulatory requirements), B2B partner API (other brokers use our infrastructure), provider MCP servers, A2A (Agent-to-Agent) protocol, multi-tenant / white-label, AI claims assistance.

### Strategic Progression

| Phase         | Model                    | Customer Interaction                                      | Risk                                   |
| ------------- | ------------------------ | --------------------------------------------------------- | -------------------------------------- |
| **MVP (1–2)** | Broker co-pilot + email  | Broker forwards leads, reviews comparisons via email      | Lowest — broker catches errors         |
| **Phase 3**   | Dashboard + MCP + widget | Web dashboard, MCP demos, widget for self-service         | Low — broker reviews everything        |
| **Phase 4**   | Direct customer channels | WhatsApp/web, AI autonomous with oversight on exceptions  | Medium — AI errors can reach customers |
| **Phase 6**   | Full platform            | End-to-end AI broker, human oversight for regulatory only | Highest — requires proven accuracy     |

Each phase expands AI autonomy only after the previous phase has proven accuracy and reliability. The broker co-pilot MVP is not a compromise — it's the fastest path to the full vision because it generates real-world training data and validates the pipeline before consumers interact directly.

---

*This document consolidates: 00-executive-summary.md, 01-market-and-integrations.md, 02-ai-feasibility.md, 03-technical-architecture.md, 04-compliance-and-risk.md, and 05-implementation-roadmap.md.*
