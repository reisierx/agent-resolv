# agent-resolv.com — Project Context

> **Maintained by Fred.** Updated during session when decisions are made or status changes.
> Last updated: 2026-02-27

---

## 1. The Thesis

Three self-evident, inevitable dynamics converge on a single opportunity:

1. **Agentic AI permeation** — deterministic workflows executed by humans are being absorbed by AI agents. Credit and insurance intermediation — heavily human, partially deterministic — is directly exposed.
2. **On-chain asset layer** — every asset class (cash, instruments, identity, contracts) is being tokenised, transacted and settled on-chain. AI becomes a direct economic actor in this layer.
3. **Compliance as moat** — regulatory compliance, fit & proper processes, and reputation are durable assets and effective deterrents for opportunistic entrants.

**The conclusion (+1):** Aggregation is the winning model — already proven globally. The broker agent is the right position in the stack.

**agent-resolv is the broker agent.** It sits between the consumer (human or AI agent) and the provider (insurer/bank). It ingests the ask, compiles information, routes to providers, analyses responses, and returns a recommendation. It is not a customer-facing runtime — that's OpenClaw/OpenAI/Meta's problem. It ingests from humans or AI agents equally.

**"Service as software":** agent-resolv is the complete agentification of a brokerage. Not a product customers subscribe to — a service that executes end-to-end. The broker agent is the service.

---

## 2. Decisions (Locked)

| Decision | Outcome | Rationale |
|----------|---------|-----------|
| Positioning | agent-resolv IS the broker agent | Not infra, not DePIN, not a routing layer. Direct intermediation. |
| Regulatory structure | Licensed entity (ASF + BdP) deploying AI tools | Only viable EU model. No precedent for AI-as-licensee. Human directors required. |
| License type | Corretor de seguros (ASF) + independent credit intermediary (BdP) | Only 68 corretores in PT — genuinely scarce. Agent license (like MUDEY, Coverflex) = tied to insurer, cannot give independent advice. |
| Primary segment | SME insurance first | Highest commissions (10–20%+), most AI-compressible, most digitally underserved in PT. |
| Revenue wedge | Credit intermediation attracts SMEs; insurance is where margin expands | Doutor Finanças proved credit at scale (€21M rev) but neglected insurance (6,500 policies). The gap to attack. |
| Revenue model | Commission (direct) + B2B2C embedded platform fees | Three-layer: direct commission → embedded/API fees → data/analytics. |
| Channel architecture | Omni-channel with unified intake abstraction | The customer chooses their front door. Every channel (WhatsApp, Telegram, email, voice, web, API/A2A) normalises to the same structured request before reaching the broker agent. The broker agent never knows which channel it came from. Channel is not a distribution decision — it is a product principle. Different segments have permanent channel preferences: traditional SME professionals = email + phone; tech-savvy / expats = WhatsApp / Telegram; platform users = embedded API; AI agents = A2A protocol. Voice intake (ElevenLabs / Vapi) is core infrastructure, not a Phase 2 add-on. |
| Customer profiling | Explicit first-class intake step, not plumbing | Before the broker agent can route to providers, it needs a structured customer profile: business type, sector, employee count, turnover, existing coverage, claims history, budget. Profiling is conversational and channel-adaptive — voice-based for callers, async for email, message-by-message for WhatsApp. Partial profiles are saved to the account; intake resumes on next contact. Profiling is never re-done from scratch for returning customers. |
| Customer account | Persistent, channel-agnostic, created on first contact | Every customer gets an account on first intake — regardless of channel. The account holds: structured customer profile, current policies and credit facilities, ongoing processes, preference settings, coverage gaps and opportunities. This is the compounding asset: the broker agent gets better per customer over time; LTV multiplies with each renewal and cross-sell. No comparison site or traditional broker has this. The account is also what makes omni-channel coherent — a customer who calls six months after emailing is recognised, not re-profiled. |
| Weecover | Technology partner, not competitor | Needs licensed entities per market. Structural complement — they have the embedded tech stack, we have the license and insurer relationships. |
| Provider integration Phase 1 | Habit Analytics API or lluni/i2S middleware | No standardized REST APIs in PT. Portal + SOAP/XML webservices is the reality until FiDA (2027-28). Direct webservices with Fidelidade/Tranquilidade/Allianz/Zurich in Phase 2. |
| Priority insurer targets | Fidelidade, Tranquilidade/Generali, Allianz, Zurich, Real Vida | All broker-friendly. Bancassurance-captive (BPI Vida, Santander/Aegon, CA Seguros) = inaccessible via broker channel. |
| Licensing constraint | ~€50K year-1 + 5-year experienced person required | ASF corretor: PII ≥€1.3M, surety bond ≥€19.5K, 120h coursework, qualified director. BdP credit: professional cert + liability insurance. **Rolando (Gonçalo's father-in-law, 20+ yr licensed agent) is the short-term path for the qualified director requirement.** |
| Do NOT build | Full-stack carrier | Burns $1B+. Lemonade/Root proved it. Intermediation only. |
| Do NOT do | D2C only | Unsustainable CAC. B2B2C embedded layer required early. |
| Do NOT do | Multi-country expansion early | Wefox cautionary tale. Portugal first, Spain second. |

---

## 3. Product Vision & Design Philosophy

### Core Philosophy: Customer-In, Product-Out

The credit and insurance industry is built backwards. Current flow: suppliers design pre-packaged products → brokers present what they earn commission on → customers receive 80-page PDFs designed for compliance, not clarity → customers are left to decipher, compare, and hope they chose correctly.

**agent-resolv inverts this entirely.** We start with the customer:
1. What do you want to protect?
2. How much coverage?
3. For what specifically?
4. At what price point?

Only then do we find products that match — across all providers, independently, without bias. The customer defines the requirement; we execute the search. This is not a UX improvement. It is a fundamentally different business model.

Because we are licensed and independent, that trust is contractual — not marketing. And because the customer trusts us, we can serve their full financial life: every insurance line, every credit need, forever. "A gente resolve" — we sort it out — is the brand promise and the business model.

### "A Gente Resolve"

Portuguese for "we sort it out." Also: *agent* + *resolv*. Triple meaning: the AI agent, the technical resolve, and the human promise. The customer doesn't need to become an expert — they tell us what they need, we take it from there. Trust is the product. Everything else is execution.

### Value Dimensions

- **Independent:** No insurer bias. No commission pressure. Legally obligated to act in the customer's interest (corretor license).
- **Transparent:** Reasoning visible — not just an answer, but the why behind it.
- **Secure & private:** Customer data used only for their benefit. Never sold, never shared.
- **Fast:** Initial recommendation in seconds, not days with a broker.
- **Humane:** Plain language. No jargon. No compliance theatre.

### Customer Segments

| Segment | Pain | Value |
|---------|------|-------|
| **Individuals** | No one starts with what they actually need — just product catalogues | We start with their life, not the shelf |
| **SMEs** | 74% underinsured; mandatory products bought blind; broker conflicts of interest | Right coverage across all lines; we absorb the complexity |
| **Brokers (B2B2C)** | Manual research, compliance burden, limited product range | AI back office — we do the research, they keep the relationship |
| **AI Agents** | Need structured financial services with compliant, machine-readable I/O | First-class API client: structured intake, structured output, A2A-ready |

### Channel Stack — Omni-Channel Architecture

The channel is not a distribution decision. It is a product principle: the customer chooses how they come in, and they get the same broker agent regardless. One broker, any channel, consistent experience.

Every channel normalises to the same structured intake request before touching the broker agent. The broker agent never knows which channel it came from.

| Channel | Segment | Role |
|---------|---------|------|
| **WhatsApp** | Consumer, tech-savvy SME, expat | Primary async messaging; ~90% PT penetration |
| **Telegram** | AI agents, tech-savvy users | Agent economy primary channel |
| **Email** | Traditional SME professionals (accountants, lawyers, architects) | Permanent channel for this segment — not a fallback |
| **Voice** (ElevenLabs / Vapi) | Traditional SME, older demographics, complex intake | Core infrastructure; qualifying call → structured handoff to broker agent |
| **Web** | Inbound, long-form intake | agent-resolv.com |
| **API / A2A** | AI agents, B2B2C platforms | Machine-readable; structured protocol |

Voice intake is core infrastructure from day one — not a Phase 2 add-on. Traditional SME professionals will always prefer phone/email. Designing voice in late means rebuilding the intake layer.

### Customer Profiling & The agent-resolv Account

**Profiling is a first-class product step.** Before the broker agent routes to any provider, it needs a structured customer profile. This is built conversationally during the first intake — channel-adaptive (message-by-message on WhatsApp, async on email, spoken on voice). It is never a form. Partial profiles are saved; intake resumes exactly where it left off on the next contact. Returning customers are never re-profiled from scratch.

**Every customer gets a persistent account on first contact** — regardless of which channel they came in through. The account is the structural foundation of everything else:
- **Structured customer profile** — business type, sector, headcount, turnover, risk appetite, budget
- **Current insurance policies and credit facilities** — full financial picture, always up to date
- **Ongoing processes and open requests** — quotes in progress, applications pending, documents outstanding
- **Preference settings** — coverage priorities, preferred providers, communication channel
- **Opportunities** — coverage gaps, renewal optimisations, cross-sell moments the broker agent has identified

The account is what makes omni-channel coherent: a customer who first contacts via email, then calls six months later, is recognised immediately — the voice agent picks up from the same account, no re-profiling. The account is the compounding asset: the broker agent gets better per customer over time. Each interaction enriches the profile; each renewal is a retention moment; each cross-sell multiplies LTV. No comparison site has this. No traditional broker has it structured and accessible to an AI system. This is a durable moat.

### Machine-Readable Endpoint (Agent API)

agent-resolv publishes a structured, machine-readable service description — a toggle on the landing page and a dedicated endpoint. Tells AI agents exactly: what we do, what we accept, what we return, which channels are live, what our licensing status is.

AI agents are first-class clients. Not an afterthought — a design principle. A2A-ready architecture from day one.

---

## 4. Plan

**Current phase:** Research + BuL accelerator track

### Immediate (by Friday Feb 27)
- [ ] Three Opus research tracks: broker/intermediary landscape PT, provider landscape PT, consumer landscape PT
- [ ] Enrich CONTEXT.md as research lands
- [ ] Build stakeholder/flow visual (Rui's ask pre-call)

### BuL Exploratory Call — Friday Feb 27, 12:00 Lisbon
- Rui Gouveia (MD) + João Pereira/JP (CTO), BuildUp Labs
- Gonçalo runs call; Opus loaded via GitHub as live research assistant
- Objective: understand BuL's thesis, their incubation offer, alignment on direction

### Post-Friday
- Remaining research tracks: ASF+BdP licensing timeline, carrier APIs + commission structures, MUDEY acquisition potential, Spain AI sandbox
- Business model design (Opus prompt queued)
- CONTEXT.md updated from BuL call output

---

## 4c. Licensing: Agente vs Corretor de Seguros

**Agente de seguros** — represents the *insurer*. Tied to one or more insurers, can only offer their products, cannot give independent advice. ~10,199 in Portugal. MUDEY, Coverflex, most digital players hold this.

**Corretor de seguros** — represents the *client*. Independent, full-market access, legally obligated to act in client's best interest. Only **68 in Portugal** across a €14.3B premium market. This is the license agent-resolv needs.

Why it matters: "A gente resolve" as a brand promise — and the customer-in model — is only legally grounded with the *corretor* license. An *agente* cannot make that promise.

**Requirements:** PII ≥€1.3M + surety bond ≥€19.5K + 120h coursework + qualified director with 5+ years licensed insurance experience (the hard constraint).

**Two paths to the qualified director requirement:**
1. **Rolando** (Gonçalo's father-in-law, 20+ yr licensed agent) — internal director, aligned, no governance complications
2. **Broker in cap table** (Sabseg, Doutor Finanças, Deco Proteste) — licensed entity as strategic investor providing the credential. Faster path + potential distribution upside, but governance and alignment considerations apply.

*Portuguese lawyer review pending. This is preliminary based on Opus's research.*

---

## 4b. BuL Call — Feb 27 2026 (Rui Gouveia's Notes)

**Riskiest assumptions (Rui's framing):**
- Licensing and regulatory overhead — specific requirements + feasibility. Alternative path raised: **broker in cap table** (Sabseg, Doutor Finanças, Deco Proteste) as a shortcut to the qualified director / licensed entity requirement.
- Provider integrations — start with email automation; integrate APIs as they become available (pragmatic sequencing, aligns with our Phase 1 decision).
- GTM / ICP / acquisition and conversion — not yet defined. Open thread.

**Moat / Defensibility (Rui's framing):**
- Structural low churn if "always best price guaranteed" model (reference: Mannie.pt).
- No legacy systems to retrofit — greenfield advantage over incumbents.

**MVP (Rui's framing):**
- Start with insurance only.
- Value prop framing: *"OpenClaw for insurance agents, with persistent customer data."*
- Business model: service-as-software.
- Partnerships: Rolando (licensing path) + Caravela (Luís Cervantes — insurer side).

**Funding discussed:**
- Target: **€250K**
- Potential investors named: Zeev / LxL Ventures, Caravela, Luís Santos, Rui Bento
- "No questions asked" 🤔 — *needs clarification: does this mean committed/soft-circled, or was this said with doubt?*

**Exit:**
- Scenario raised: Spanish competitor entering PT market.

---

## 5. Status & Open Threads

**Status as of 2026-02-27 (session end):** BuL call done. Thesis validated. Rui's notes logged (Section 4b). Agente vs Corretor summary shared with Rui + JP (Section 4c). Lawyer review on licensing pending. Biggest open holes: GTM/ICP, responsável técnico (corretor bottleneck), and BuL next steps. €250K funding target soft-circled but Rui sceptical without MVP + first customers through Rolando.

**Key market numbers (PT):**
- 1.1M SMEs (95.5% micro) — primary target. 74% underinsured. 88% can't explain professional liability.
- Workers' comp alone: €1.1B, >95% via mediation. Total non-life: €5.6B.
- Mortgage intermediation: 9% → 57% in 5 years (9% in 2019 → €10.2B/57% in 2024) — rapid adoption proven.
- BdP found 93.9% irregularity rate among credit intermediaries — compliance as moat, confirmed.
- WhatsApp: ~90% PT penetration (~8.4M users), 98% open rate — channel choice validated.
- Expat segment: 1.54M foreign residents (+18%/yr), concentrated in 4 districts, no unified insurance journey exists.

| Thread | Status | Urgency |
|--------|--------|---------|
| Broker/intermediary landscape PT | ✅ Done — `research/2026-02-25-brokers-pt.md` | — |
| Provider landscape PT | ✅ Done — `research/2026-02-25-providers-pt.md` | — |
| Consumer landscape PT | ✅ Done — `research/2026-02-25-consumers-pt.md` | — |
| BuL call | ✅ Done — notes in Section 4b | — |
| GTM / ICP / acquisition + conversion | ❌ Not defined | 🔴 Next priority |
| Licensing path — Rolando vs broker in cap table | Open — Sabseg / Doutor Finanças / Deco Proteste as alternatives | 🔴 Pre-revenue |
| Funding round — €250K | Soft discussions; "no questions asked" needs clarification | 🟡 Active |
| BuL next steps | Pending — what does BuL want to do next? | 🟡 Active |
| ASF + BdP licensing timeline | Not yet researched | 🟡 Pre-revenue |
| Mannie.pt — pricing model reference | Worth a closer look as moat framing | 🟢 Research |
| Coverflex: agent license confirmed, not broker | ✅ Resolved — B2B2C window exists but closing | — |
| Weecover PT: tech partner, not competitor | ✅ Resolved | — |
| Old project files (flows, diagrams) | To be archived | 🟢 Low urgency |

---

## 6. Key Artifacts

| File | What it is |
|------|-----------|
| `research/2026-02-25-competitive-landscape.md` | Full competitive landscape (50+ companies, Iberian gap, business model economics, regulatory posture). Primary reference. |
| `research/2026-02-25-brokers-pt.md` | Broker/intermediary landscape PT — 10,289 mediators, 68 corretores, Doutor Finanças profile, Coverflex (agent license), Weecover (tech partner), B2B2C map (Revolut 2.1M users) |
| `research/2026-02-25-providers-pt.md` | Provider landscape PT — market structure, commission rates (18% broker avg), integration path (Habit Analytics / lluni middleware → direct webservices), incumbent AI posture (ops only, not distribution), licensing costs |
| `research/2026-02-25-consumers-pt.md` | Consumer landscape PT — 1.1M SMEs, 74% underinsured, WhatsApp 90% PT, mortgage intermediation 9→57% in 5yr, 1.54M expats underserved, 93.9% BdP irregularity rate |
| `projects/agent-resolv/archive/` | Old flows, diagrams, architecture docs — prior thesis (DePIN/routing layer). Superseded. Do not use. |
