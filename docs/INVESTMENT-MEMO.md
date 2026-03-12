# Investment Memorandum — agent-resolv

> **Living document.** Created: 2026-02-27 (post-BuildUp Labs call). Updated continuously as the project evolves. This is the canonical source of truth for the project narrative.
>
> **Company:** agent-resolv (a ReisierX venture)
> **Founder:** Gonçalo Reis
> **Stage:** Pre-seed / Pre-revenue
> **Classification:** Confidential
>
> **RENDERING NOTE:** This document uses an internal/external convention. Blocks marked `<!-- INTERNAL ... INTERNAL -->`, inline comments `<!-- rationale: ... -->`, and the Decision Log section are stripped when rendering /memo HTML for external audiences. The bare narrative (sections 1–12, Appendix A) is the investor-facing pitch. Source .md retains full reasoning and lock history.

---

## Executive Summary

**agent-resolv is the AI-native insurance and credit broker for Portugal's underserved market.**

The problem is structural: 74% of Portuguese SMEs carry inadequate coverage, 88% cannot explain what their professional liability insurance covers, and the traditional intermediary network is collapsing (down 37% since 2019). Fewer than 1% of insurance premiums are sold online. Simultaneously, credit intermediation has exploded from 9% to 57% of mortgages in five years, but compliance enforcement is nonexistent (93.9% irregularity rate).

agent-resolv inverts the industry. We start with the customer's actual needs—not product catalogs. The regulatory path is being defined: options include ASF corretor de seguros (independent broker, legal duty to act in client interest) and BdP credit intermediary — each with distinct attributes, requirements, and timelines. The AI agent handles omni-channel intake (WhatsApp, Telegram, email, voice, API), builds a persistent customer account, routes to all providers independently, and returns a recommendation. Humans hold regulatory accountability.

**Why this, why now, why us:**

- **Timing convergence:** IDD consolidation is eliminating weak intermediaries; FiDA (2027–28) will standardize insurance data access; the mortgage boom is generating cross-sell demand; digital channel infrastructure (WhatsApp, voice AI, email automation) is production-ready; AI capability has crossed the threshold for autonomous intermediation.
- **No independent AI broker in Portugal or Iberia.** MUDEY (the closest analogue) holds an agent license only and has struggled with D2C. The market for a licensed, independent AI broker is a blank canvas.
- **Structural defensibility:** The corretor model (working thesis) carries legal duty to act in client interest — a trust moat no AI-only platform or tied agent can replicate. Persistent customer account compounds LTV. Compliance-first positioning in a market where 93.9% of credit intermediaries operate irregularly.
- **Three-layer revenue:** Direct commissions (€1.3B commission market), embedded platform fees, and future data/analytics. Four segments: Individuals, Businesses, Brokers & Platforms, AI Agents.

**Core team locked. Completing the bench through the raise.** Gonçalo (founder) = strategy, regulatory, fundraising. BuildUp Labs confirmed as Co-Founder (2026-02-28): JP (BuL CTO) = de facto CTO; Rui Gouveia (BuL MD) = network + GTM. The raise completes the founding team — investors fill gaps in legal expertise, market relationships, and capital.

**The ask:** €250K–300K pre-seed (licensing, pilot build, provider integration, working capital). Pre-money valuation ~€1.4M–1.7M (implying ~15% dilution). We are not just raising capital — we are completing the founding team with investors who close specific gaps: legal, network, and cash.

---

## 1. The Problem

The credit and insurance industry is built backwards.

Suppliers design pre-packaged products. Brokers present what earns the most commission. Customers receive policy documents designed for compliance, not clarity. They are left to decipher, compare, and hope they chose correctly. The customer's actual needs—what they want to protect, how much coverage, at what price—are an afterthought.

In Portugal, this structural dysfunction is compounded by acute market failure:

<!-- rationale: Market failure quantification validates the scale of the opportunity and regulatory urgency. | source: consumers-pt.md, providers-pt.md | locked: 2026-02-25 -->

**74% of SMEs carry inadequate coverage.** 88% cannot explain what their professional liability insurance covers—the highest rate in Europe. One in three SME owners has not reviewed their policies in over three years. 55% lack basic general liability or property insurance. 54% suffered a cyberattack in the past year yet fewer than 15% carry cyber insurance.

**The intermediary network is collapsing.** Registered intermediaries fell 37% from 16,783 (2019) to 10,489 (2023). Surviving individual agents average just €11,717/year in commissions—below the national minimum wage. The predicted trajectory is a further halving. The gap between SME demand and intermediary supply is widening.

**Fewer than 1% of insurance premiums are sold online.** Only 4% of Portuguese consumers have purchased insurance through a comparison site (vs. 11% EU-wide). No insurer has achieved significant end-to-end digital sales. Customer satisfaction is poor: no Portuguese insurer has achieved a "high satisfaction" rating in DECO PROteste surveys. ASF received 28,939 formal complaints in 2024.

**Credit intermediation is scaling but chaotic.** Mortgage intermediation exploded from 9% to 57% of all mortgage volume in five years (9% in 2019 → €10.2B/57% in 2024). Banco de Portugal's compliance audits found near-universal irregularities among credit intermediaries. Compliance is the exception, not the rule.

The country has just **68 licensed independent brokers** across a €14.3 billion premium market—serving 1.1 million SMEs. That is a structural vacuum.

<!-- INTERNAL
The 68-corretor figure and 16,783 → 10,489 mediator collapse are the core empirical anchors of the market failure narrative. These justify why a licensed, AI-native broker is timely. The 93.9% credit intermediary irregularity rate is the strongest signal that compliance-first positioning is defensible (Section 5.3). The underinsurance statistics (74%, 88%, 54%) come from ASF and DECO PROteste sources and are stable across documents.
INTERNAL -->

---

## 2. The Solution

**agent-resolv is the broker agent.** It sits between the consumer—human or AI agent—and the provider (insurer or bank). It ingests the ask, compiles information, routes to providers, analyzes responses, and returns a recommendation. It is not a chatbot. It is not a comparison site. It is the broker—a complete agentification of brokerage as software.

The regulatory path is being defined. The working thesis is corretor de seguros (ASF) + credit intermediary (BdP)—the combination that enables fully independent, client-first advice across both insurance and lending. Final path subject to legal counsel and regulatory roadmap definition.

### 2.1 Customer-In, Product-Out

agent-resolv inverts the industry entirely. We start with the customer:

1. What do you want to protect?
2. How much coverage?
3. For what specifically?
4. At what price point?

Only then do we find products that match—across all providers, independently, without bias. The customer defines the requirement; we execute the search.

Under the corretor model (working thesis), independence is contractual — not marketing. That structural trust, once established, enables us to serve the customer's full financial life: every insurance line, every credit need, at every life stage.

### 2.2 "A Gente Resolve"

Portuguese for "we sort it out." Also: *agent* + *resolv*. Triple meaning: the AI agent, the technical resolve, and the human promise. The customer doesn't need to become an expert—they tell us what they need, we take it from there. Trust is the product. Everything else is execution.

<!-- rationale: Brand thesis drives positioning. Independent agent + AI scale + Portuguese cultural resonance. Not a translation; an identity. | source: creative synthesis; strategy memo | locked: 2026-02-15 -->

### 2.3 Autopilot, Not Copilot

Two categories of AI company are emerging. A **copilot** sells the tool — it puts AI in the hands of a professional and lets them decide what to do with it. Every improvement to the model makes the professional more productive, but the professional remains the product. An **autopilot** sells the work — the customer buys the outcome directly. Every improvement to the model makes the service faster, cheaper, and harder to compete with.

agent-resolv is an autopilot. The customer does not buy software to help them navigate insurance decisions — they buy cover in place and credit arranged. That distinction is structural. It determines pricing power (the work budget dwarfs the tool budget), competitive moat (the data compounds toward the service, not toward a licence key), and what "AI getting better" means for the business (tailwind, not disruption).

The reason autopilots can now win in insurance brokerage is the intelligence/judgement split. Brokerage intermediation — intake structuring, multi-provider quoting, comparison synthesis, eligibility routing — is primarily **intelligence work**: the rules are complex, but they are rules. AI has crossed the threshold where it can execute that work autonomously. What remains human is **judgement**: regulatory accountability, complex case decisions, final recommendation sign-off. agent-resolv is designed around that boundary — AI owns the intelligence layer; qualified human directors hold the judgement layer. The result is AI speed and scale with human accountability, in a licensed structure that satisfies IDD and EU AI Act requirements.

agent-resolv is not a product customers subscribe to — it is a service that executes end-to-end. The broker agent does what a broker does: information gathering, multi-provider quoting, comparison, recommendation, application processing, policy management. The human layer is retained through qualified human directors, not as a workaround but as the correct architecture for regulated financial services.

---

## 3. Why Now

Five converging factors create an exceptional timing window:

<!-- rationale: Timing convergence is the strongest justification for venture capital urgency. These five factors create a 18–36 month window where first movers capture position. | source: asf-license.md, providers-pt.md, consumers-pt.md, competitive-landscape.md | locked: 2026-02-25 -->

**1. IDD Consolidation.** The Insurance Distribution Directive's implementation in 2018 triggered compliance-driven consolidation. The intermediary network has contracted 37% since 2019 (see Section 1 for detail). Weak independent agents are exiting. The structural gap between SME demand for advice and intermediary supply is widening rapidly. That gap is a market.

**2. FiDA Standard-Setting (2027–28).** The Financial Data Access regulation will mandate standardized, API-based access to insurance data. Early movers who already have large customer bases and provider integrations will own the transition. Entrants after APIs are standardized compete on brand and execution only. The window to build customer lock-in before standardization narrows to 18–24 months.

**3. The Mortgage Boom.** Mortgage lending hit €23.3B in 2025—the highest since 2014 (€17.9B new origination in 2024, 57% via intermediaries). Every mortgage triggers simultaneous demand for insurance: life, property, mortgage protection. No incumbent combines mortgage and insurance advisory in a single conversation. The cross-sell opportunity is immediate and quantifiable.

**4. Digital Channel Infrastructure is Mature.** WhatsApp Cloud API, voice AI (ElevenLabs, Vapi), email automation, and A2A (agent-to-agent) protocols are all production-ready. Omni-channel intake is buildable today at a fraction of the cost three years ago. The infrastructure risk is eliminated.

**5. AI Capability Threshold Crossed.** The broker's value-add in standard commercial lines is essentially shopping across carriers and filling forms — intelligence work, not judgement. Large language models now handle 80–90% of that workflow (intake structuring, provider matching, comparison synthesis, edge case routing) autonomously. What remains human is judgement: regulatory accountability and complex case decisions. That boundary is not a limitation — it is the correct regulatory structure and a structural defensibility argument. The "why now for AI brokers?" question is answered by capability, not wishful thinking.

**6. Incumbent Transformation Lag.** Replacing an outsourcing contract with an AI-native provider is a vendor swap. Replacing internal headcount is a reorg. Insurance intermediation in Portugal is already outsourced — the SME is already buying an outcome from a third party. The switching cost to agent-resolv is structurally low: the customer's underlying need hasn't changed, only the delivery mechanism has. Meanwhile, incumbents — surviving individual agents, small brokerages, tied insurers — will be the last to transform their own workflows. The 37% collapse of the intermediary network since 2019 is not self-correcting; the incumbents who remain are running on legacy processes with no AI investment. That lag is the window.

These five factors compound. Each has an 18–24 month expiry window — miss the entry point, and the next opportunity is post-FiDA (2027–28), when data access is commoditized and incumbents have rebuilt defensibly.

<!-- INTERNAL
The five-factor framing is how Gonçalo sold BuL on urgency and how investors will evaluate risk of timing. Each factor has ~18–24 month expiration: IDD consolidation will slow once weak players exit, FiDA changes the game in 2027–28, the mortgage boom is a temporary demand spike, channel maturity accelerates competitive entry, and AI capability will be table-stakes by 2027. Scarcity of windows is the venture capital justification.
INTERNAL -->

---

## 4. Market

### 4.1 Market Sizing — Portugal

**Insurance (addressable via broker channel):**

| Segment | Annual Premiums | Broker Relevance |
|---|---|---|
| Non-life total | €7.4B (+10.4% YoY) | Brokers control 20–25% |
| Workers' comp (mandatory) | €1.1B | >95% via mediation |
| Health | €1.56B (+17.5%) | Fastest-growing; 4M covered |
| Multi-peril / property | ~€1.1B (+11%) | SME core product |
| Auto (mandatory) | €2.1B | High volume, competitive |
| Life (risk) | ~€0.8B | Independent advice valued |

Total mediation commissions across the market: **€1.3 billion** (2024, +17% YoY). Broker average commission rate: **18.1%**.

**Credit intermediation:**

| Segment | 2024 Volume | Intermediary Share |
|---|---|---|
| Mortgages | €17.9B new origination | 57% via intermediaries (~€10.2B) |
| Consumer credit | ~€8.4B new origination | 49.9% via intermediaries |
| Auto credit | — | 83.1% via intermediaries |

**Combined addressable market:** A licensed corretor intermediating insurance and credit across SME and personal lines in Portugal is addressing a market generating over **€1.4 billion in annual commissions**.

<!-- rationale: Market sizing is from ASF, Banco de Portugal, and industry reports. The €1.3B mediation commission figure is 2024 data and validates the scale of commissions available to brokers. | source: providers-pt.md | locked: 2026-02-25 -->

### 4.2 Customer Segments

agent-resolv serves four distinct customer segments. Prioritization across these segments is not yet locked — pilot and regulatory constraints will inform which to target first.

**🏠 Individuals**

Portugal's insurance density is €1,350 per capita (5.2% of GDP), below the EU average of ~7.5%. Bancassurance dominance (72.8% of life premiums flow through banks) creates captive distribution with limited carrier choice and conflicted advice. An independent AI broker is structurally differentiated. Within this segment, the foreign resident population is a notable sub-segment: 1.54 million residents (14.4% of population, growing ~18%/year), concentrated in Lisbon, Faro, Setúbal, and Porto, with acute underserved needs across health, property, motor, life, and mortgage intermediation. Only a handful of brokers offer English-language services today.

**🏢 Businesses**

1.1 million SMEs, 95.5% micro. Mandatory insurance creates a non-discretionary demand floor: every employer needs workers' compensation (~€1.1B market), every commercial vehicle needs motor insurance. Beyond mandatories, the underinsurance gap is structural — 74% of SMEs carry inadequate coverage, 88% cannot explain what their professional liability insurance covers. The intermediary network serving them is collapsing (see Section 1) — the gap between demand and supply is widening with no natural corrective mechanism.

**🤝 Brokers & Platforms**

Two related but distinct sub-segments. Traditional licensed intermediaries — individual agents and small brokerages — who lack the tooling to research, compare, and process at speed; agent-resolv as AI back-office lets them keep the client relationship while delegating the complexity. B2B2C platforms — Revolut (2.1M PT users), Rauva (SMB neobank), Coverflex (250K users), Idealista (38M monthly visitors) — that have user bases with insurance and credit needs but no licensed brokerage capability; agent-resolv as embedded infrastructure.

**🤖 Agents**

AI agents acting on behalf of humans are first-class clients. As the agent economy matures, autonomous agents will need to procure insurance, arrange credit, and manage risk on behalf of the humans and businesses they serve — requiring a licensed, machine-readable counterparty with structured I/O. agent-resolv is designed for this from day one: A2A-ready architecture, machine-readable endpoint, Telegram as the agent economy's primary channel.

### 4.3 Ideal Customer Profile — Working Thesis

The ICP is being sharpened through discovery. Working thesis as of 2026-03-03:

**B2B2C primary channel: real estate brokers with associated insurance and/or credit intermediary operations.** These operators are already in the room at the moment of the life event trigger (property transaction), carry warm client pipelines, and are under-tooled — no CRM, manual email workflows, payment delays, corretora-provided software with no automation layer. First-pass discovery with Rolando and Tiago Assunção (2026-03-03) surfaces this pattern clearly: both are real estate agents with associated insurance/credit operations. They are not just pilot nodes — they are the B2B2C ICP.

**D2C secondary channel:** Individuals and SMEs at life event moments (mortgage, business formation, property purchase). Particularly strong for the expat segment (1.54M foreign residents, underserved). Complements B2B2C, not a substitute.

The full strategic logic behind this ICP is documented in `reflections/2026-03-03-b2b2c-thesis.md`.

<!-- rationale: ICP updated from "deliberately open" to working thesis based on first-pass discovery with Rolando and Tiago (2026-03-03). Real estate broker as B2B2C prime channel is the structural insight: credit is the trigger, insurance is the margin. | updated: 2026-03-03 -->

---

## 5. Competitive Landscape

### 5.1 Direct Competitors in Portugal: Effectively None

No AI-native insurance or credit broker exists in Portugal—or Iberia. The market is a near-blank canvas.

**MUDEY**—Portugal's first digital insurance mediator. Holds only an **agente de seguros** license (ASF #420558967), *not* a corretor. Has raised €720K, employs 10–15 people, ~30,000 registered users against a target of 60,000. MUDEY's pivot to MudeyPRO (B2B tooling for micro-mediators) signals difficulty in D2C acquisition. The fact that MUDEY has not upgraded to corretor after 5+ years of operation is instructive—it is the strongest possible market signal about the practical trade-offs of the agente-first path.

**Doutor Finanças**—Dominates credit intermediation (€21M revenue, €918M in intermediated mortgages, 100+ franchise stores) but treats insurance as an afterthought: just 6,500 policies and ~€1.8M in insurance premiums in 2023. That gap is the target.

**Adjacent platforms:** Coverflex (250K users, agent license only, not independent broker), Weecover (embedded insurance tech, no ASF license, potential technology partner—not competitor), ComparaJá (comparison site, no AI advisory).

### 5.2 International Analogues Validating the Thesis

| Company | Market | Model | Traction |
|---|---|---|---|
| Harper (YC W25) | US | AI-native commercial broker | $6M+ annualized premiums since Oct 2024 |
| WithCoverage ($42M Series B, Sequoia/Khosla) | US | AI-powered risk management team; sells to CFO, not broker; replaces commission model | $42M raised Jan 2026; Sequoia partner Julien Bek cited as autopilot-native exemplar |
| Flow Specialty ($15.6M Series A) | US | AI wholesale specialty broker | 260% growth in 2024 |
| Meshed (£950K pre-seed) | UK | FCA-regulated SME AI broker | 51 insurer agreements |
| Wopta (€8M raised) | Italy | AI agent "Anna", 200K+ customers | 4,000+ intermediaries |

These companies validate the thesis that AI-native insurance brokerage works. None operates in Portugal or Iberia. agent-resolv is applying a proven playbook to a market with zero incumbents.

<!-- rationale: Harper, WithCoverage, Flow, Meshed, and Wopta demonstrate that the AI-native broker model works globally and attracts venture capital. Their absence from Portugal validates the vacuum. Wopta's Italy presence is nearest competitive threat but no cross-border offer. WithCoverage added 2026-03-06: $42M Series B (Sequoia/Khosla, Jan 2026) — named by Sequoia partner Julien Bek as the autopilot-native exemplar in insurance. Sequoia is explicitly calling insurance brokerage the largest autopilot opportunity ($140-200B global); strong external validation for investor conversations. | source: competitive-landscape.md, research/2026-03-06-julien-bek-sequoia-autopilot-thesis.md | updated: 2026-03-06 -->

### 5.3 Structural Gaps Defining the Opportunity

The competitive analysis points to six gaps — none of which any existing player is positioned to close:

1. **No AI-native insurance broker** in Portugal or Iberia. First mover owns the position.
2. **No cross-product integration** — no single platform combines insurance and credit advisory despite life events triggering simultaneous needs.
3. **SME insurance digitally unaddressed** — online share under 1%. No incumbent has achieved significant end-to-end digital sales.
4. **Expat segment unserved** — 1.54M foreign residents, growing 18%/year, served by a handful of manual English-language brokers.
5. **Bancassurance dominance is captive distribution** — 72.8% of life premiums flow through banks with conflicted advice. An independent broker is structurally differentiated.
6. **FiDA (2027–28) will commoditize data access** — first movers with established relationships and integrations win the transition; late entrants compete on brand alone.

---

## 6. Product

### 6.1 Omni-Channel Architecture

**The channel is not a distribution decision—it is a product principle.** The customer chooses how they come in; agent-resolv meets them there. Every channel normalizes to the same structured intake request before touching the broker agent. The broker agent never knows which channel it came from.

Different customer segments have **permanent channel preferences**—these are not transitional. Designing for one channel and adding others later means rebuilding the intake layer. agent-resolv is omni-channel from day one.

| Channel | Segment | Role |
|---------|---------|------|
| **WhatsApp** | Consumer, tech-savvy SME, expat | Primary async messaging; ~90% PT penetration, 98% open rate |
| **Telegram** | AI agents, tech-savvy users | Agent economy primary channel |
| **Email** | Traditional SME professionals (accountants, lawyers, architects) | First-class front door—permanent, not transitional |
| **Voice** (ElevenLabs / Vapi) | Traditional SME, older demographics, complex intake | Core infrastructure—qualifying call → structured handoff to broker agent |
| **Web** | Inbound, long-form intake | agent-resolv.com |
| **API / A2A** | AI agents, B2B2C platforms | Machine-readable; structured protocol |

Voice intake is core infrastructure from day one. Traditional SME professionals will always prefer phone/email — a channel built late means rebuilding the intake layer.

<!-- rationale: VC assessment flagged omni-channel as Day 1 overengineering risk. Decision locked: "Channel is not a distribution decision—it is a product principle." Execution prioritizes email + WhatsApp + voice in Phase 1 (matching ICP), with Telegram, web, and API/A2A live but not primary focus. | source: vc-assessment.md, strategy memos | locked: 2026-02-24 -->

### 6.2 Customer Profiling — First-Class Product Step

Before the broker agent routes to any provider, it builds a structured customer profile through conversational intake: business type, sector, employee count, turnover, existing coverage, claims history, budget. Profiling is channel-adaptive—spoken on a voice call, message-by-message on WhatsApp, async on email. It is never a form.

Critically: **partial profiles are saved** to the customer account and intake resumes exactly where it left off on the next contact. Returning customers are never re-profiled from scratch.

### 6.3 Persistent Customer Account — The Compounding Asset

**Every customer gets a persistent account on first contact**—regardless of which channel they arrived through. The account holds:

- Structured customer profile—business type, sector, headcount, turnover, risk appetite, budget
- Current insurance policies and credit facilities—full financial picture, always up to date
- Ongoing processes and open requests—quotes in progress, applications pending, documents outstanding
- Preference settings—coverage priorities, preferred providers, communication channel
- Opportunities—coverage gaps, renewal optimisations, cross-sell moments the broker agent has identified

This is the compounding asset: each interaction enriches the profile, each renewal is a retention moment, each cross-sell multiplies LTV. A customer who first contacts via email and calls six months later is recognized immediately — no re-profiling. No comparison site or traditional broker has this structured and accessible to an AI system.

### 6.4 Machine-Readable Endpoint (Agent API)

agent-resolv publishes a structured, machine-readable service description—a toggle on the landing page and a dedicated endpoint. Tells AI agents exactly: what we do, what we accept, what we return, which channels are live, what our licensing status is. AI agents are first-class clients. Not an afterthought—a design principle. A2A-ready architecture from day one.

### 6.5 Value Dimensions

**Investor view—competitive differentiators:**

No licensed, independent AI insurance and credit broker exists in Portugal or Iberia. The six differentiators that define the position:

| Differentiator | Why it matters |
|---|---|
| Autopilot, not copilot | We sell the outcome (cover in place, credit arranged), not the interface. Every model improvement is a tailwind. The work budget dwarfs the tool budget. |
| Independent corretor model (working thesis) | Legal duty to act in client's interest — a promise no agente can legally make. Only 68 entities in Portugal hold this position today. |
| AI-native, not AI-augmented | Built for autonomous execution. Incumbents retrofit AI onto manual workflows; we design for it from day one. |
| Insurance + credit combined | Life events trigger simultaneous needs — one conversation, full bundle. No incumbent combines both. |
| Omni-channel by design | WhatsApp, Telegram, email, voice, API/A2A — unified intake abstraction. Channel is a product principle, not a distribution channel. |
| Persistent customer account | Every interaction enriches the profile. Compounding LTV over the customer's financial lifetime. |
| Compliance-first architecture | EU AI Act compliance and audit-readiness from day one, in a market where 93.9% of credit intermediaries operate irregularly. |

**Customer view—what the product promises:**

- **Independent:** No insurer bias. No commission pressure. Structurally obligated to act in the customer's interest — this is the corretor model we are pursuing.
- **Transparent:** Reasoning visible—not just an answer, but the why behind it.
- **Secure & private:** Customer data used only for their benefit. Never sold, never shared.
- **Fast:** Initial recommendation in seconds, not days with a broker.
- **Humane:** Plain language. No jargon. No compliance theatre.

### 6.6 What the Product Promises Each Segment

| Segment | What we do for them |
|---------|---------------------|
| **🏠 Individuals** | Start with their life, not the shelf. One conversation covers every insurance line and credit need — omni-channel, plain language, no jargon. |
| **🏢 Businesses** | Absorb the complexity. Mandatory compliance handled, coverage gaps identified, multi-provider quoting done autonomously. Owner gets a recommendation, not a spreadsheet. |
| **🤝 Brokers & Platforms** | AI back office for intermediaries — they keep the client relationship, we do the research and processing. Embedded licensed infrastructure for platforms needing brokerage capability. |
| **🤖 Agents** | First-class machine-readable endpoint: structured intake, structured output, A2A-ready. The only licensed brokerage counterparty designed for the agent economy. |

---

## 7. Regulatory Path

### 7.1 The Only Viable EU Model

No precedent exists in any EU jurisdiction for AI as a licensed entity. IDD fit-and-proper requirements are anthropocentric: they require natural persons with professional qualifications, clean criminal records, and demonstrated competence. The only viable compliance structure is a **licensed entity with AI tools**—a properly licensed brokerage with qualified human directors, deploying AI as operational tooling under human oversight. This is how Harper, Meshed, Flow Specialty, and every other AI-native broker globally is structured.

### 7.2 Agente vs Corretor de Seguros — The Strategic Choice

Portuguese law draws a clean structural distinction (Lei n.º 7/2019, RJDS):

**Agente de seguros**—acts on behalf of one or several insurers. The agent's primary legal relationship is with the insurer, not the client. The agent is a sales channel for the insurer. ~10,199 in Portugal. MUDEY, Coverflex, most digital players hold this.

**Corretor de seguros**—exercises distribution activity independently of insurers. The corretor's privileged contractual relationship is with the **client**. Legal duty to act in the client's best interest (Art. 35 RJDS). Only **68 in Portugal** across a €14.3B premium market.

The categories are **mutually exclusive** (Art. 15 RJDS).

**Working thesis: corretor de seguros.** "A gente resolve" as a brand promise—and the customer-in model—is most strongly grounded with the corretor license. An agente cannot make the same legal promise. The corretor license also passports EU-wide under IDD (Arts. 4 and 6 of Directive 2016/97); an agente license does not. The final path is subject to legal counsel confirmation and regulatory roadmap definition.

<!-- rationale: Corretor vs. agente is a binary strategic choice. Corretor is harder (higher capital, responsável técnico bottleneck, diversification rules) but defensible (independent status, legal duty of care, EU passporting). Agente-first path is faster but limits positioning and growth. Working thesis: corretor is the right call despite timeline cost — pending legal counsel confirmation and regulatory roadmap. | source: licensing-research.md | updated: 2026-03-02 -->

<!-- INTERNAL
### 7.3 Corretor Requirements and Critical Path

**Capital and corporate:**
- Minimum share capital: **€50,000**, fully paid up at incorporation
- Equity must remain above 50% of share capital at all times (≥€25,000)
- Ongoing financial ratios: autonomia financeira ≥15%, solvabilidade ≥20%, liquidez geral ≥100%
- Corporate object must be exclusively financial sector activities
- ROC (statutory auditor) must be designated regardless of company size
- At least two board members responsible for distribution activity

**Professional indemnity insurance (PII):**
- €1,300,380 per claim / €1,924,560 per year aggregate
- Must cover entire EU territory
- Must not exclude liability from acts of PDEADS (staff directly involved in distribution)

**Guarantee bond (seguro de caução):**
- Year 1: **€19,510** (statutory minimum)
- Subsequent years: greater of €19,510 or 4% of total funds handled

**Training:**
- Vida + Não Vida scope: **120 hours** of ASF-recognized training
- Final exam (in-person, written) worth ≥75% of overall grade
- Providers: APROSE, SABFORMA, MOFP, Distance Learning Consulting, GPA Academy
- CPD: 15 hours/year minimum for all persons involved in distribution

**The responsável técnico—THE hard constraint:**
At least one board member must demonstrate **5 years of qualifying experience, consecutive or interpolated, within the 7-year period preceding the application** (RJDS Art. 13). Qualifying roles: registered mediator, PDEADS, or board member of a mediator/insurer/reinsurer responsible for distribution. This person must be in *presença em permanência* (permanent presence)—not a nominal appointment.

**Total minimum capital outlay: ~€85,000–€105,000**

| Item | Amount |
|---|---|
| Share capital (fully paid) | **€50,000** |
| Guarantee bond (Year 1) | **€19,510** |
| PII annual premium (estimate) | **€10,000–€25,000** |
| ASF fees + legal/incorporation | **€5,000–€10,000** |
| **Total minimum** | **~€85,000–€105,000** |

<!-- rationale: These figures come from Lei n.º 7/2019 (RJDS), ASF regulations, and preliminary legal quotes. They are locked figures based on statutory requirements, not estimates. | source: licensing-research.md | locked: 2026-02-27 -->

### 7.4 Realistic Licensing Timeline: 9–16 Months

The critical path runs through five sequential gates:

1. Identify and recruit a responsável técnico—**1–3 months**
2. Complete ASF-recognized training (can overlap with recruitment)—**1–3 months**
3. Incorporate, capitalize at €50K, secure PII and guarantee bond commitments—**1–2 months**
4. Prepare and submit application dossier including three-year business plan—**1 month**
5. ASF review and processing—**2–6 months** (legal max: 60 business days; negative silence applies—no response = refusal)

**This is substantially longer than the 3–6 months estimated in earlier documents.** The responsável técnico search is the binding constraint. Only ~69 corretoras operate in Portugal. The pool of individuals with 5+ years of qualifying experience who are available, willing, and not conflicted is inherently small.

**Rolando (Gonçalo's father-in-law, 20+ years licensed agent)** plays two potential roles. First, as a pilot enabler: his existing license could allow agent-resolv to run the MVP brokerage flow through his operation—functioning as a B2B2C node or proxy arrangement—before the entity's own license is granted. The exact mechanism depends on confirming what licenses he currently holds. Second, as a future director candidate: if his license type and experience meet the 5-year qualifying threshold under the specific regulatory path chosen, he could be invited onto the board as responsável técnico. Both roles are conditional on license confirmation (pending his return from Mar 20). A single RT creates a structural dependency—mitigation is to ensure at least two board members independently meet the experience requirement.

<!-- rationale: The 9–16 month timeline is the honest critical path. Earlier 3–6 month estimates underestimated the RT search and ASF processing variability. This is now locked and must be communicated to investors. | source: licensing-research.md | locked: 2026-02-27 -->

### 7.5 Portfolio Diversification — A Mathematical Problem at Launch

Corretor diversification rules (NR 13/2020-R; RJDS Art. 35(b)):

| Concentration | Max % of total commissions |
|---|---|
| 1 insurer | **50%** |
| 2 insurers | **80%** |
| 3 insurers | **90%** |
| 4 insurers | **95%** |

These thresholds are mathematically impossible to meet with a small, new book. **No explicit ramp period or new-entrant exemption exists in the current legislation.** Must be raised directly with ASF during pre-application. Launch strategy should begin placing business with at least 5 insurers from day one.

<!-- GAP: ASF guidance on new-entrant tolerance for diversification thresholds | owner: Legal counsel + ASF pre-application consultation | urgency: 🔴 -->
INTERNAL -->

<!-- INTERNAL
### 7.6 Credit Intermediation — Dual-Entity Structure Required

DL 81-C/2017 establishes three mutually exclusive categories of credit intermediary. The **independente (não vinculado)** category—which aligns with agent-resolv's independence positioning—requires **exclusive corporate purpose**: credit intermediation activity only. This directly conflicts with the corretor's requirement to include insurance distribution in its corporate object.

**A corretor de seguros cannot simultaneously be an intermediário de crédito não vinculado.** However, a corretor can potentially also be an intermediário de crédito **vinculado** (tied)—operating under lenders' binding agreements and liability.

**Strategic choice:** if agent-resolv wants to be genuinely independent in both insurance and credit, it needs a **two-entity corporate structure**—one for the corretor license (ASF), one for the non-tied credit intermediary license (BdP).

<!-- GAP: Tied vs. untied credit intermediary path and single-entity vs. dual-entity structure requires a definitive decision with legal counsel. | owner: Gonçalo + legal counsel | urgency: 🔴 -->
INTERNAL -->

### 7.3 EU AI Act — August 2026 Deadline

**Credit scoring AI: definitively high-risk** (Annex III, 5(b)). **Life/health insurance pricing AI: high-risk** (Annex III, 5(c)). **Non-life insurance product recommendation AI: probably not high-risk**—unless it performs profiling of natural persons (Art. 6(3) override).

**Compliance approach (risk-tiered):**
- Standard non-life recommendations: HOTL (human-on-the-loop) with exception flagging
- Life/health insurance and credit scoring: enhanced HOTL or HITL (human-in-the-loop)
- All systems: human override capability, interpretability tools, minimum 6-month log retention, FRIA for credit AI, transparency notices to affected persons

<!-- INTERNAL
### 7.8 Passporting — EU Expansion

A Portuguese corretor can passport its license to any EU member state under the IDD. The process: notify ASF → ASF communicates to host state authority within 1 month → begin operating 1 month after notification. For **Spain**: Portuguese corretor notifies ASF → ASF notifies DGSFP → ~2 months total → can operate in Spain.
INTERNAL -->

### 7.4 Agente-First Path — Viable Alternative

The corretor path requires a *responsável técnico* — a qualified director with 5+ years of licensed intermediary experience, mandated by ASF. If that person cannot be secured within 2–3 months: launch as an agente with a clear internal roadmap to upgrade within 18–24 months. This eliminates the €50,000 capital requirement, the guarantee bond, the experience requirement, and the diversification rules. Gets to market 6–12 months faster. MUDEY took exactly this path.

The costs: no legal independence, reduced duty of care, transition friction when upgrading, and reputational risk if marketing as "independent" while holding an agent license.

<!-- rationale: Agente-first is a real alternative if RT search stalls. This is not a pivot; it's a contingency with clear exit criteria (18–24 month upgrade path). Keeping this option open is prudent. | updated: 2026-03-02 -->

### 7.5 Portugal FinLab

Portugal FinLab is a communication channel co-managed by ASF, BdP, and CMVM. Not a formal sandbox, but enables structured dialogue with all three financial regulators simultaneously. agent-resolv should engage FinLab pre-launch.

---

## 8. Business Model

### 8.1 Revenue Architecture

Three-layer monetization:

**Layer 1—Direct commissions.** Insurance and credit intermediation commissions earned from providers. The traditional broker's revenue stream, executed at AI speed and scale.

| Product | Commission Rate | Avg Annual Premium | Revenue per Policy |
|---|---|---|---|
| Workers' comp | 17.5% | €800–2,500 | €140–438 |
| Professional liability | 20% | €500–3,000 | €100–600 |
| SME multi-peril | 20% | €1,000–5,000 | €200–1,000 |
| Health (group, <50 lives) | 12% | €400/person | €48/person |
| Auto | 18% | €450 | €81 |
| Home | 17% | €250 | €43 |
| Mortgage referral | 0.5–1.0% | €142,800 deal size | €714–1,428 |
| Consumer credit | 1.0–2.0% | €10,000 deal size | €100–200 |

Note: Commission rates are volume-dependent and negotiated per insurer. Year 1 rates modeled at 60–80% of market average, ramping to full by Year 2–3.

**Layer 2—B2B2C embedded platform fees.** Platforms like Revolut, Rauva, Coverflex, and Idealista integrate agent-resolv's API. Revenue split: agent-resolv earns commission, platform earns a referral fee. Base platform integration fees (€2,000–10,000/month) plus per-API-call fees for high-volume integrations. Benchmark: 60–80% gross margins per bolttech and Cover Genius.

**Layer 3—Data/analytics.** Future revenue stream from portfolio analytics, risk intelligence, and market data. Not modeled in projections.

### 8.2 Revenue Wedge

Credit intermediation—specifically mortgage referrals—serves as the customer acquisition wedge, leveraging proven demand. Doutor Finanças proved credit at scale but treats insurance as an afterthought (see Section 5.1). **That gap is the target.** Credit attracts the customer; insurance is where margin expands and LTV compounds.

<!-- rationale: Doutor Finanças is the model case: massive credit volume, minimal insurance penetration. This validates that SMEs understand credit but neglect insurance, and that the wedge strategy (credit → insurance) is proven. | source: competitive-landscape.md | locked: 2026-02-25 -->

### 8.3 Revenue Projections (€K)

| Scenario | Year 1 | Year 2 | Year 3 |
|---|---|---|---|
| Conservative | €25K | €165K | €625K |
| Base | €41K | €290K | €1,213K |
| Upside | €50K | €450K | €2,750K |

*Base case trajectory: €41K (Y1) → €290K (Y2) → €1,213K (Y3). Base case assumptions: 75 SME clients Y1, 400 Y2, 1,500 Y3. 2.5 policies/client. €275 avg commission/policy. 50 mortgage deals Y2+. 3 B2B2C partnerships Y3 at €100K/yr each. 85% renewal rate.*

### 8.4 Target Unit Economics (Year 2 Steady-State)

*These are target economics based on market benchmarks, not validated actuals. Pilot will test and refine all assumptions.*

| Metric | Target |
|---|---|
| Revenue per client (year 1) | €688 (2.5 policies × €275 avg. commission) |
| Revenue per client (renewal years) | €585 (85% renewal × €688) |
| Client lifetime value (5-year, 10% discount) | **€2,500** |
| Customer acquisition cost (direct marketing, scale) | €150–300 |
| LTV:CAC ratio | **8–17x** |
| Payback period | **1–3 months** |

### 8.5 Cost Structure (Year 1)

| Item | Annual Cost |
|---|---|
| Licensing (ASF + BdP): PII, surety, training, capital | €85,000–105,000 |
| Core team (including Founder, full loaded cost) | ~€123,000 |
| AI infrastructure (API costs, hosting) | €12,000–24,000 |
| Legal and compliance | €15,000 |
| Office (covered by BuildUp Labs) | €0 |
| **Total Year 1** | **~€235,000–267,000** |

<!-- rationale: Core team cost is fully loaded (consulting contracts, social charges included). BuL team (JP, Rui) and any investor-directors (e.g. Zeev) receive no salary or contract — sweat equity only. Office space provided by BuL as part of their contribution. | source: strategy discussion 2026-03-02 | locked: 2026-03-02 -->

*Note: The €50,000 share capital figure is a statutory requirement for corretor registration and applies if that path is confirmed.*

---

## 9. Go-to-Market

### 9.1 Where We Are

The go-to-market strategy is being actively designed. This is not an omission — it is the honest state of a pre-pilot company making decisions in the right order.

The immediate priority is defining the pilot/MVP: what we test, with whom, through which mechanism, and what success looks like. That design depends on several open questions being resolved in parallel:

- **Regulatory path:** Which license(s), in what sequence, and what pilot mechanism is available before the entity's own license is granted (e.g., operating through Rolando's existing license as a B2B2C or proxy node — pending confirmation of his registrations)
- **Product scope:** What does the MVP actually do end-to-end? JP (BuL CTO) is leading this; alignment session scheduled for Wednesday Mar 4
- **Team and investor structure:** Whether Zeev joins (legal workstream, potential investor) shapes the timeline and resources available
- **ICP:** Which customer segment to target first — driven by regulatory and technical constraints, not just market attractiveness

Once these are resolved, the pilot design follows naturally: target segment, channel, success criteria, and scale path. We will define those explicitly before any external go-to-market begins.

### 9.2 What We Know

While the pilot design is open, several structural conclusions are already locked:

**Distribution will be dual — direct and embedded.** Direct omni-channel (WhatsApp, email, voice, web) is fastest to market and gives us full control over the customer relationship and data. B2B2C embedded — platforms like Revolut (2.1M PT users), Rauva, Coverflex, Idealista — is the scaling mechanism. The sequence and timing are TBD, but both are in scope.

**Provider integration is a critical path dependency.** Multi-insurer quoting requires either middleware (Habit Analytics API, lluni/i2S) or direct integrations. JP is assessing feasibility and integration costs. Manual fallback for non-integrated insurers is available in the short term but not a sustainable model.

**Warm pipeline exists but is not yet quantified.** Rolando's existing client network and Gonçalo's professional contacts represent a potential pilot source — but the size, conversion likelihood, and accessibility of that pipeline have not been formally assessed.

<!-- rationale: GTM section rewritten to reflect honest pre-pilot state. Previous version presented three phases with specific timelines, targets, and success metrics — none of which had been validated or even formally decided. That framing was premature and misleading. The honest position is that pilot design is the immediate work, and it depends on several open questions being resolved. | source: strategy discussion 2026-03-02 -->

---

## 10. Team

### 10.1 Current Team

**Gonçalo Reis (Founder/CEO)**—Strategy, fundraising, regulatory engagement, initial sales. Master's in Management with VC/PE fluency, philosophy-trained analytical rigor.

**BuildUp Labs (Co-Founder, confirmed 2026-02-28):**
- **JP (BuL CTO):** de facto CTO for agent-resolv. Owns product/engineering completely. Will deliver v1 system architecture.
- **Rui Gouveia (BuL MD):** network, GTM know-how. Co-founder of CoMon. Bridge to Zeev (LxLaunch + Fresh Legal).
<!-- INTERNAL - **Terms:** IP lives in the entity. Equity split and dilution terms available on request. INTERNAL -->

<!-- rationale: BuL Co-Founder terms confirmed in call 2026-02-28. This is the binding commitment that unblocks product development and unlocks VC conversations. 70/30 split is structured to preserve dilution flexibility post-formation while aligning incentives. | locked: 2026-02-28 -->

**Fred (friedr.eth)**—AI co-founder operating as research and strategic intelligence system.

<!-- INTERNAL
**Key relationships:**
- **Rolando** (Gonçalo's father-in-law)—20+ year licensed insurance agent. Two potential roles: (1) pilot enabler — run MVP brokerage flow through his existing license as a B2B2C or proxy node; (2) director/RT candidate for the corretor license if his license type and experience qualify. Both conditional on confirming his actual registrations (Mar 20+). **First-pass discovery completed 2026-03-03** (via Sebastião): licensed agent (vida + não vida) under Sabseg (PT's largest corretora); no credit license; email-first workflow confirmed; ~2 processes/month (low volume — pilot must bring customers to him); RT candidacy promising but requires legal review; Sabseg relationship may constrain pilot design. Full profile: `research/2026-03-03-rolando-profile.md`.
- **Luís Cervantes**—CEO, Caravela Seguros. Direct access to the insurer side of the market.
- **Luís Santos**—Alpac Capital. Strategic thinking partner.
- **Rui Gouveia network**—Rui (BuL MD) brings an extensive network of relevant relationships; details available on request.
INTERNAL -->

### 10.2 What's Missing

**Rolando's roles unconfirmed**—Both his pilot role (B2B2C/proxy mechanism) and his RT candidacy depend on confirming his actual license type(s) and ASF/BdP registration. Neither role is locked until that confirmation happens (Mar 20+).

**Responsável técnico backup**—If Rolando qualifies as RT, a single-person dependency remains a structural risk. Need at least two board members independently meeting the 5-year experience requirement.

<!-- GAP: Rolando's license type(s) and current registration not confirmed. Blocks pilot design and licensing path. | owner: Gonçalo | urgency: 🔴 -->
<!-- GAP: Rolando's commitment structure (FT/PT, equity, formal agreement) not documented. | owner: Gonçalo | urgency: 🔴 -->

<!-- INTERNAL
### 10.3 What Is Already Built

| Asset | Status |
|---|---|
| agent-resolv.com domain | Live. Triple meaning: AI agent · resolve technically · "a gente resolve" |
| Landing page with Human/Agent toggle | Live at agent-resolv.com |
| Four research tracks (50+ sources) | Complete |
| Licensing research | Complete |
| ReisierX AI infrastructure (OpenClaw + Fred) | Operational |
| Thesis defined, decisions locked | Regulatory pathway mapped. ICP defined. Go-to-market sequenced. |

<!-- GAP: No functioning prototype of the core brokerage loop. No demo of: conversational intake → structured profile → multi-provider quote request → comparison output. | owner: JP (BuL CTO) | urgency: 🔴 -->
INTERNAL -->

---

## 11. The Ask

### 11.1 We're Not Just Raising — We're Hiring

We have a founder. We have a co-founder in BuildUp Labs. The team covers strategy, product, and engineering.

What this raise does is close the remaining gaps — and those gaps are not just cash. We are looking for people who bring something we need and who invest instead of drawing a salary. Legal expertise and regulatory network. Market relationships and distribution access. Financial credibility and early validation.

In that sense, the pre-seed round is less a fundraise and more a founding team completion exercise. The €250K–300K matters — it funds the pilot, licensing process, and runway. But who the money comes from matters just as much.

### 11.2 Pre-seed Raise

**Target: €250,000–300,000**
**Pre-money valuation: ~€1.4M–1.7M** (implying ~15% dilution at the midpoint of the range)

| Use of Funds | Amount | Purpose |
|---|---|---|
| Licensing and compliance setup | €85,000–105,000 | Regulatory path definition, entity formation, licenses (path TBC) |
| Technical development | €40,000–60,000 | AI infrastructure, pilot build |
| Provider integration | €20,000–40,000 | Middleware licensing, direct integration |
| Operations and overhead | €20,000–30,000 | Tools, insurance, admin |
| Working capital and contingency | €45,000–75,000 | Buffer for timeline variance |

**Proposed structure:** SAFE note. Cap and discount to be confirmed with legal counsel — dependent on entity formation and regulatory path decision.

<!-- rationale: Round size revised to €250K–300K (from €250K–500K) to reflect honest pre-pilot scope. "Post-Pilot" framing removed — this raise funds the pilot itself. Dilution of 15–20% is the honest expectation at this stage and risk profile. Named investors and specific relationships moved to INTERNAL. | source: strategy discussion 2026-03-02 -->

### 11.3 Investor Landscape

**Strategic investors** — Portuguese insurers with innovation mandates, fintech platforms that could become B2B2C partners, and sector operators with distribution access are natural pre-seed partners given the regulated, relationship-driven nature of the market.

**Financial investors** — Portuguese and Iberian seed-stage funds with fintech or regulated market exposure. European insurtech-focused funds.

<!-- INTERNAL
### Named Pipeline
- **Zeev / LxL Ventures** — legal workstream + potential investor. Rui speaking with him Tue Mar 3. Decision window 2 weeks from Wed Mar 4.
- **Luís Santos** (Alpac Capital) — strategic thinking partner, potential investor
- **Luís Cervantes** (CEO Caravela Seguros) — insurer-side access; Luís Santos is a Caravela shareholder — double relationship
- **Rui Gouveia network** — additional relationships via BuL; details available on request

<!-- GAP: Legal structure and cap table not documented. | owner: Gonçalo + legal counsel | urgency: 🔴 -->
INTERNAL -->

---

## 12. Risks & Mitigations

### 12.1 Risk Register

Ranked by criticality (probability × severity). Critical risks are existential — they block the business entirely if unresolved. High risks are serious but manageable with active mitigation. Medium risks are real but do not threaten the thesis.

**Critical**

| Risk | Probability | Severity | Mitigation |
|---|---|---|---|
| **Licensing delay** — corretor critical path is 9–16 months. Responsável técnico search is the binding constraint; pool of qualified, available, unconflicted candidates is inherently small. | High | High | Engage lawyer immediately. Confirm Rolando's license type on return (Mar 20). Agente-first alternative (6–12 months faster) if RT search stalls beyond 2–3 months. |
| **Rolando uncertainty** — Both pilot mechanism and RT candidacy depend on confirming his actual license type(s). Neither role is locked until then. Single-person RT dependency is a structural fragility. | High | High | Confirm registrations on return (Mar 20). Recruit second RT-eligible candidate in parallel. Formalise commitment and role before signing anything. |

**High**

| Risk | Probability | Severity | Mitigation |
|---|---|---|---|
| **Provider integration complexity** — No standardized APIs until FiDA (2027–28). Multi-insurer quoting requires middleware or direct SOAP/XML integrations. | High | Medium | Assess Habit Analytics / lluni middleware in parallel with pilot design. Manual fallback viable short-term, not at scale. JP leading feasibility assessment. |
| **Commission rate ramp** — New broker will not achieve market-average rates from day one. Insurers negotiate rates against volume. | High | Medium | Revenue model already accounts for this: Year 1 at 60–80% of market average, ramping to full by Year 2–3. Monitor actual rate-card in first provider conversations. |
| **Regulatory interpretation risk** — Dual-entity structure (corretor + non-tied credit intermediary) or EU AI Act compliance interpreted more strictly than anticipated. | Low-Medium | High | Engage FinLab pre-application. Commission legal assessment from established counsel before entity formation. Compliance-first architecture from day one. |

**Medium**

| Risk | Probability | Severity | Mitigation |
|---|---|---|---|
| **Well-funded entrant** — Coverfy (€16M, Barcelona), Weecover, or an international player enters Portugal before we are licensed. | Low-Medium | High | Speed to license is the primary defense. The corretor barrier is 9–16 months for any entrant — same clock applies to everyone. |
| **Portfolio diversification at launch** — Corretor concentration rules are mathematically impossible to meet with a small new book. No new-entrant exemption exists in current legislation. | High | Medium | Raise directly with ASF in pre-application consultation. Request written transitional tolerance. Place business with 5+ insurers from day one. |
| **Market traction slower than modeled** — Warm lead conversion below 30%, or broader market slower to adopt. | Medium | Medium | Pilot validates assumptions before committing to scale. Fallback to B2B2C embedded if D2C acquisition proves harder than expected. |
| **D2C acquisition gap** — Between warm leads (pilot) and B2B2C partnerships (scale), no defined acquisition channel exists. | Medium | Medium | Active gap. Rui Gouveia (BuL) owns GTM; acquisition strategy is a Wednesday alignment session agenda item. |

<!-- rationale: Risk register reordered by criticality (probability × severity). "No technical co-founder" removed — resolved risks belong in the team section, not here. "Renewal/retention" removed — relevant at scale, premature at pilot stage. | source: strategy discussion 2026-03-02 -->

### 12.2 Key Assumptions Requiring Validation

Three tiers: thesis-level assumptions (if wrong, the business case changes fundamentally), pilot-level assumptions (if wrong, the pilot design changes), and financial assumptions (if wrong, the model needs revision).

**Thesis-level**

| Assumption | Why It Matters | How We Validate |
|---|---|---|
| The corretor license is achievable within 9–16 months | The entire independent positioning depends on it. If the RT bottleneck proves unsolvable, the agente-first alternative changes the competitive story. | Engage lawyer immediately. Confirm Rolando's eligibility. Pre-application consultation with ASF/FinLab. |
| Corretor is worth the cost over agente | €85–105K capital outlay and 9–16 month delay vs. faster agente path. The bet: independent status is a durable moat, not just a marketing claim. | MUDEY's 5-year failure to upgrade is the strongest evidence. Working thesis — final confirmation requires legal counsel and regulatory roadmap. |
| AI handles 80%+ of brokerage workflow under human oversight | The core product thesis. If AI capability is insufficient for the Portuguese market specifics (language, document formats, insurer systems), the unit economics break. | JP's PRD shows 90–93% email parsing accuracy on limited data. Production validation is the pilot's primary technical objective. |
| The market gap is structural, not cyclical | 74% SME underinsurance and the intermediary collapse are real and not self-correcting. | ASF and BdP data confirms the trend is multi-year and accelerating. IDD consolidation is ongoing. |

**Pilot-level**

| Assumption | Why It Matters | How We Validate |
|---|---|---|
| Rolando's existing license enables the pilot mechanism | If his license type doesn't support the B2B2C or proxy arrangement we need, the pilot mechanism changes entirely. | Confirm ASF/BdP registrations on return (Mar 20). Legal counsel to assess viable mechanism. |
| Provider integration is feasible at pilot scale | Without multi-insurer quoting, there is no brokerage. Manual fallback works for a handful of policies, not as a product. | JP assessing Habit Analytics / lluni middleware. Outcome expected post Wednesday alignment session. |
| Warm pipeline has sufficient density for a meaningful pilot | Rolando's network and Gonçalo's contacts are the assumed pilot source. If the pipeline is too thin, pilot design needs a different acquisition approach. | Quantify pipeline before pilot launch. Minimum viable cohort TBD based on pilot design. |
| Pilot design can be agreed and launched before pre-seed closes | We are raising to fund the pilot, not after it. If pilot design stalls, fundraise stalls. | Wednesday alignment session with JP and Rui is the forcing function. |

**Financial**

| Assumption | Why It Matters | How We Validate |
|---|---|---|
| Commission rates achievable at 60–80% of market average in Year 1 | Revenue model is built on this ramp. Lower rates compress the path to sustainability. | First provider conversations. Rate-card benchmarking before pilot launch. |
| 85% SME insurance renewal rate | Drives LTV and the compounding account thesis. If churn is higher, the unit economics weaken materially. | Industry benchmarking first; pilot data as the ground truth. |

---

## Appendix A: Platform Vision

> *Longer-term strategic thinking — not locked decisions.*

**The underlying thesis:** any market where suppliers structure information to their advantage and consumers navigate complexity alone is attackable with the same model. The beachhead is insurance + credit. But the arc is agent-resolv as a platform: /irs-declaration → /energy → /phone-internet → and beyond.

**Agent economy track:** A separate track for agent-to-agent services—a knowledge marketplace where AI agents pay for structured analysis. Architecturally distinct from consumer intermediation. Examples: one AI agent calls another to quote a specific risk scenario; agent-resolv is the counterparty providing licensed brokerage at machine speed and pricing. This track is not modeled in Y1–Y3 financials but is the long-term moat against aggregation.

**EU expansion (Year 3+):** If the corretor path is taken, the license passports EU-wide under IDD — Spain, France, Italy accessible via notification to ASF. Duplicate entity structure per market, shared AI infrastructure. Year 3 target: 2–3 EU markets, 10,000+ combined active clients.

**Data and analytics layer (Year 4+):** Portfolio risk analytics, market trend intelligence, premium optimization. Monetize the compounded customer data as a service layer to insurers, reinsurers, and brokers. Benchmark: Optic Insurance Data (acquisition $50M+).

---

## Appendix B: Decision Log

<!-- INTERNAL
Chronological record of locked decisions. This section is stripped in external /memo rendering.
INTERNAL -->

| Date | Decision | Rationale | Owner | Status |
|---|---|---|---|---|
| 2026-02-15 | Brand: "A Gente Resolve" (triple meaning: AI agent, technical resolve, Portuguese cultural resonance) | Brand thesis drives positioning and customer trust narrative. Not a translation; an identity. | Gonçalo | Locked |
| 2026-02-24 | Omni-channel by design (not sequential rollout) | VC flagged as Day 1 overengineering. Decision: Channel is a product principle, not a distribution choice. Execution prioritizes email + WhatsApp + voice in Phase 1. | Gonçalo + BuL | Locked |
| 2026-02-25 | Timing convergence (5-factor window: IDD, FiDA, mortgage boom, channel maturity, AI capability) | Scarcity of windows justifies venture urgency. Each factor has 18–24 month expiration. | Gonçalo + Fred | Locked |
| 2026-02-25 | SME Phase 1 ICP: regulated professions with mandatory insurance (lawyers, architects, accountants, engineers, medical, real estate agents) | Original working assumption. Superseded 2026-03-02: ICP deliberately unlocked — pilot design, regulatory path, and Rolando's license type must inform segment prioritization before committing. | Gonçalo | Superseded |
| 2026-02-27 | Corretor de seguros (not agente) as working thesis | "A gente resolve" positioning and customer-in model most strongly grounded with corretor. Agente-first alternative viable if RT search delays >2–3 months. Final path subject to legal counsel + regulatory roadmap. | Gonçalo + legal | Working thesis |
| 2026-02-27 | Licensing critical path: 9–16 months (not 3–6) | Responsável técnico search is binding constraint. ASF processing variance 2–6 months. Earlier estimates underestimated RT scarcity. | Gonçalo + licensing research | Locked |
| 2026-02-27 | Licensing capital: €85K–105K (not €50K) | €50K share capital is mandatory. Earlier docs omitted this hard requirement. Updated cost structure reflects actual statutory requirements. | Legal counsel | Locked |
| 2026-02-27 | Revenue projection baseline: €41K (Y1) → €290K (Y2) → €1.2M (Y3) | Modeled on 75 SME clients Y1, 400 Y2, 1,500 Y3; 2.5 policies/client; €275 avg commission; 85% renewal. Conservative rates (60–80% of market average) Y1, ramp to full by Y2–3. | Gonçalo + financial modeling | Locked |
| 2026-02-28 | BuildUp Labs Co-Founder (70% ReisierX / 30% BuL) | JP = de facto CTO. Rui Gouveia = network + GTM. IP in entity (70/30). Unblocks product dev and VC conversations. | Gonçalo + BuL | Locked |
| 2026-02-28 | Rolando = primary pilot enabler and RT candidacy path | Two conditional roles confirmed as thesis. Both gated on confirming actual license type(s) on return from Mar 20. Neither role is locked. | Gonçalo | Conditional |
| 2026-02-28 | Dual-entity structure (insurance corretor + credit intermediary) requires legal clarity | Tied vs. untied credit path not yet decided. Impact on corporate structure. Requires legal counsel consultation before final entity formation. | Gonçalo + legal | Open |

<!-- INTERNAL
Decision Log serves three purposes:
1. Accountability trail for investor review (what was locked, when, by whom)
2. Hypothesis record for founder retrospective (which assumptions held, which didn't)
3. Strategic anchor (what pivots would require revisiting these decisions)

This section is internal-only and stripped from /memo HTML rendering.
INTERNAL -->

---

## Research Index

<!-- INTERNAL
Reference-only section for tracing analysis sources. Stripped in /memo rendering.
INTERNAL -->

| Date | File | Key Finding |
|------|------|-------------|
| 2026-02-22 | asf-license.md | ASF/BdP licensing is accessible (2–6 months each), not a moat; real defensibility is trust networks + compliance track record. |
| 2026-02-25 | brokers-pt.md | 10,289 mediators, only 68 corretores; 0.28% online sales; IDD consolidation eliminating small operators — timing exceptional. |
| 2026-02-25 | competitive-landscape.md | 50+ AI intermediaries globally, Southern Europe near-blank; winning models are asset-light B2B2C + commission + platform fees. |
| 2026-02-25 | consumers-pt.md | 1.1M SMEs, 74% underinsured; credit intermediation 9%→57% in 5yr; 1.54M expats underserved; WhatsApp ~90% penetration. |
| 2026-02-25 | providers-pt.md | €14.3B market, <3% digital; no standardised APIs until FiDA (2027–28); agent population halved since 2012. |
| 2026-02-25 | investment-memo.md | Full investment memo for BuL accelerator track. Dual-license thesis, SME-first, three-layer revenue model. |
| 2026-02-26 | memo-vc-assessment.md | Consistency audit + VC pressure test of MD vs HTML memos. Versioning gap identified; neither is strict superset. |
| 2026-02-27 | licensing-research.md | Corretor license: 9–16 months; bottleneck = responsável técnico (5yr experience). MUDEY chose agente route. |

---

**Document version:** 1.3 (Incumbent lag thesis added, Levie/Bek — 2026-03-06)
**Next review:** Post BuL sync + Tiago call debrief
**Maintained by:** Gonçalo Reis, Fred (friedr.eth)
