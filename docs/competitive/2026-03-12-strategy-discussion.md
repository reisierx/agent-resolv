# Strategy Discussion — 2026-03-12
**Context:** agent-resolv//BUL Telegram group · Participants: Gonçalo, JP, Fred
**Topics:** Sequoia thesis alignment, WithCoverage analysis, market approach, copilot vs autopilot

---

## Sequoia "Services: The New Software" — Key Alignment

Published March 5, 2026. Full article: https://sequoiacap.com/article/services-the-new-software/

**Core thesis:** The next $1T company sells the work, not the tool. AI-native "autopilots" capture the services budget ($6 for every $1 of software spend) by selling outcomes directly to buyers.

**Framework:**
- **Copilot** = sells to the professional (makes them more productive). Customer is the practitioner.
- **Autopilot** = sells to the company that needs the outcome. Captures the work budget from day one.

**Insurance brokerage** is Sequoia's #1 priority market by dollar size ($140–200B). Described as "pure intelligence work" with fragmented distribution and no single incumbent controlling the customer relationship. WithCoverage and Harper named as autopilot plays.

**Implication for agent-resolv:** We are building an autopilot. We sell placed credit/insurance, not a productivity tool. This is the investor narrative — and it's now backed by Sequoia, a16z, and YC converging simultaneously.

---

## WithCoverage Analysis — Key Takeaways

Full competitive intelligence: `2026-03-12-withcoverage-intelligence.md`
Fred's analysis: `2026-03-12-withcoverage-analysis-fred.md`

**What's validated:**
- AI-native insurance brokerage is attracting serious capital ($42M Series B, Sequoia + Khosla)
- Licensed broker model (not MGA or carrier) is correct
- Email-mediated carrier integration is industry standard — even at $50M in funding, WithCoverage doesn't have real-time carrier APIs. This is not a limitation; it's the market reality.
- Schema/data layer is the defensible IP, not the AI model
- Vertical-first GTM (dominate one segment, then expand)
- Free audit as lead gen engine

**Where we diverge (advantages for PT market):**
- WhatsApp-first > dashboard (Portuguese SMEs don't want another login)
- Commission model fits SME premium sizes (flat fee doesn't)
- Dual ASF + BdP license = unique moat (no global comp offers both)
- Lean by necessity → forces deeper automation earlier (target 50–100 clients/human vs WithCoverage's ~10)

**Open gap flagged:** Credit side underdeveloped. Dual license is called a moat everywhere but the integration into the mediador workflow isn't mapped. Needs to be resolved before MVP design.

---

## Market Approach Decision: "Copilot Entry, Autopilot by Design"

**Sharpened recommendation (JP, 2026-03-12):** Enter via mediadores (copilot phase) but structure everything for direct-to-SME from day one (autopilot by design).

### Is the copilot phase necessary?

**Yes — but it's a capital constraint, not a strategic preference.**

Direct autopilot from day one requires:
1. Own ASF license already active
2. Marketing budget to acquire SMEs directly
3. Brand trust sufficient for a Portuguese SME to switch broker without a warm intro

None of these are true at €250K pre-seed. The copilot entry via mediadores solves for all three: Rolando's network provides distribution, warm introductions, and the bridge to operate while the license application is in progress.

The Sequoia framework itself supports this: *"the right place to start is where outsourcing already exists."* In Portugal, mediadores ARE the outsourcing layer for SME insurance.

### The critical structural condition

The mediador phase only makes sense if we're structured to exit it cleanly. Three non-negotiables:

1. **We hold the ASF license** — not Rolando. He's the bridge, not the endpoint. The license must transfer to the entity as soon as it's granted.
2. **Customer data stays with us** — not in the mediador's head or personal CRM. Every SME that onboards goes into our schema layer.
3. **SME onboarding happens through our interface** — WhatsApp/email flows we own. The mediador recommends us; the channel is ours.

If these three conditions are met, the mediador is just a distribution channel we use early. If any slip, we've built a structural dependency we can't unwind.

**"Mediador as distribution" ≠ "mediador as operating model."** This distinction is what sophisticated angels will probe. It needs to be explicit in the GTM section of the investment memo.

### The Diagnóstico de Seguros as first autopilot experiment

The free insurance health check is simultaneously:
- Lead gen engine (SME sends existing policies via WhatsApp, gets a coverage/savings report within hours)
- Data bootstrapping (every audit feeds the schema)
- First direct autopilot touchpoint (SME interacts with us, not through a mediador)

This works without carrier integration and generates the structured data that feeds everything else. Build this before quoting flows.

---

## Tech Layer (JP / BuL)

Full tech PRD and research now in `docs/tech/`. Key decisions from the tech layer:

- **Mastra** as agent orchestration framework
- **Resend** for email (chosen over agentmail after evaluation — agentmail evaluation kept in `docs/tech/research/agentmail-evaluation.md` for future reference)
- **Email-first interface** as primary carrier integration channel
- WhatsApp intake spike completed (`docs/tech/research/ai-spikes/whatsapp-intake.md`)
- PDF parsing spike completed (`docs/tech/research/ai-spikes/pdf-parsing.md`) — answers feasibility question on Portuguese insurance PDF parsing
- Rolando session prep kit in `docs/tech/research/onboarding/rolando-session-prep-kit.md` — to be evolved into a reusable mediador discovery template for future agent onboarding

---

## Open Questions for Next Sessions

1. **ASF licensing timeline** — what can we do before license is granted? Francisco Machado to answer. Determines go-to-market sequence entirely.
2. **Mediador adoption** — what are the real objections beyond Rolando? Need 5–10 conversations with mediadores.
3. **Carrier relationships in PT** — do Fidelidade, Ageas, Tranquilidade have appetite for an AI-native intermediary, or do they see us as a threat to tied agent networks?
4. **Real unit economics** — Rolando's actual book: average premium per SME, commission rates per product type, renewal rate.
5. **Credit integration model** — how does BdP-licensed credit intermediation fit into the mediador workflow? When does credit come up in the SME journey?
6. **Mediador discovery template** — evolve Rolando's session prep kit into a reusable format before the next agent onboarding session.
