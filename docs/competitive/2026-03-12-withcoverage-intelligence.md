# WithCoverage — Competitive Intelligence for agent-resolv

**Prepared:** March 2026 · **Purpose:** Team alignment on competitive learnings, MVP implications, and scaling insights

---

## Executive Summary

WithCoverage is a New York-based AI-augmented insurance brokerage that raised $42M in Series B (January 2026) from Sequoia Capital and Khosla Ventures. They've onboarded 700+ clients in 18 months, claim operational profitability, and position themselves as a replacement for traditional commission-based brokers.

**Why this matters for agent-resolv:** WithCoverage validates our core thesis — that AI can disrupt insurance brokerage — while operating in a fundamentally different market (US mid-market) with a different model (flat-fee, human-heavy). Their journey reveals what works, what's hard, and what we can borrow for the Portuguese SME market. This document also analyzes the critical strategic question of whether agent-resolv should go B2C, B2B, or B2B2C — with a recommendation based on Portuguese market dynamics and our funding constraints.

---

## Part 1: How WithCoverage Works

### The Product in a Nutshell

WithCoverage positions itself not as a broker but as an "outsourced Risk Management Team" — a combination of AI tools, a digital platform, and a team of specialized risk advisors and in-house attorneys. Their tagline: transparent pricing, better coverage, proactive risk management.

### The Customer Journey (High-Level)

**Step 1 — Free Risk Audit (AI-heavy)**
The customer shares their existing insurance policies. WithCoverage's AI Audit Engine parses policy documents and operational data to identify coverage gaps, inefficiencies, and overspending. The output is a line-by-line savings breakdown delivered on the first call.

This is their lead generation engine: a no-commitment, high-value entry point that quantifies potential savings before anyone signs anything.

**Step 2 — Risk Strategy Design (Human-led)**
A vertically specialized risk advisor designs a tailored risk management plan based on the audit findings. This is where deep domain expertise matters — understanding which coverage is essential for a hospitality company vs. a construction firm vs. a DTC brand.

**Step 3 — Competitive Carrier Shopping (Human-mediated)**
The internal team prepares submission packages and sends them to multiple A-rated carriers. Carriers' underwriters review, ask follow-up questions, and return quotes — a process that takes days to weeks. WithCoverage compares quotes, negotiates terms, and presents options.

**Critical insight: this step is NOT automated via APIs.** Even with $42M in funding in the US market (which has more developed insurance APIs than Portugal), the carrier interaction is still done through traditional channels — portals, email, and human relationships.

**Step 4 — Transition & Binding (Human-managed)**
WithCoverage manages the full transition from the previous broker, handles policy binding, and becomes the ongoing risk management partner.

**Step 5 — Ongoing Management (Platform + Human)**
Post-binding, their digital platform handles COI issuance, claims tracking, billing, renewals, and policy management. Their in-house attorneys proactively manage claims — a rare capability that acts as a strong retention mechanism.

### What They Actually Automated vs. What Stays Human

| Workflow Step | Automation Level | How |
|---|---|---|
| Policy document parsing | **High** | AI extracts structured data from PDFs (coverages, limits, exclusions) |
| Gap analysis & benchmarking | **High** | AI compares current coverage against risk profile and market benchmarks |
| Submission preparation | **Medium** | AMS pre-fills carrier applications from structured data |
| Carrier appetite matching | **Medium** | System knows which carriers are competitive for which risk types |
| Carrier communication | **Low** | Still human-mediated via portals and email — carriers respond on their own timeline |
| Negotiation & terms | **Low** | Requires human judgment and relationship leverage |
| Claims handling | **Low** | In-house attorneys manage full lifecycle |
| COI, billing, renewals | **High** | Digital platform automates routine administration |

**Key takeaway:** Their AI operates on the broker's side of the workflow, not the carrier's. The intelligence is in understanding policies better and faster than humans — not in having real-time API connections to insurers.

---

## Part 2: Company Profile & Trajectory

### Team & Founders

- **JD Ross** (Co-founder) — Co-founded Opendoor ($7B+ proptech), employee #5 at Addepar (joined at 19)
- **Max Brenner** (Co-founder & CEO) — Founding team at Compound ($1B+ AUM), ex-Bain & Company
- **Myles Hilliard** (Head of Risk) — 10+ years underwriting at Markel
- **Phil Wilusz** (Head of Claims) — Nationally recognized policyholder attorney
- Construction specialist from Marsh and Chubb

The team is roughly 50–70 people, hiring 75+ more in 2026. Engineering leadership comes from UC Berkeley, Apple, and NASA backgrounds.

### Funding Path

| Round | Amount | Lead Investors | Implied Total |
|---|---|---|---|
| Early rounds | ~$8.5M | Antifund (Nik Sharma), Crystal Venture Partners | $8.5M |
| Series B (Jan 2026) | $42M | Sequoia Capital (Roelof Botha), Khosla Ventures (Keith Rabois) | $50.5M |

They claim operational profitability — rare for insurtech at this stage. Keith Rabois (Khosla) co-founded Opendoor with JD Ross, so there's a strong relationship-driven component to the funding.

### Go-to-Market: Vertical-First Strategy

Rather than going broad, WithCoverage builds deep expertise in specific verticals before expanding:

- **Consumer brands/CPG** — Initial beachhead (GoPuff, Bombas, Chomps, Eight Sleep)
- **Hospitality** — Dedicated landing page, specialist team (Blank Street Coffee)
- **Construction** — Launched 2026 with AI camera analysis and subcontractor tracking
- **3PL/Logistics** — Partnership with Fulfill.com
- **Aerospace/Defense** & **Auto/Transportation** — 2026 expansion targets
- **Private Equity portfolio companies** — B2B2B channel play

### Business Model: Flat Fee, Not Commission

This is their most distinctive strategic choice. Traditional brokers earn 10–30% of premiums (incentivized to sell expensive policies). WithCoverage charges a transparent flat fee and claims to save clients 30–40% on premiums, with some reporting $300K+ in annual savings.

However, job postings mention maintaining a "commissions and fee ledger" — suggesting they still receive carrier commissions but offset them against or pass them through to clients. The flat fee is the client-facing model; the backend economics may be hybrid.

### Market Impact

In February 2026, insurance broker stocks dropped 4–12% after AI comparison tools launched. Bank of America estimates $15B in "low complexity" insurance commissions are at risk from AI. WithCoverage is part of a broader wave that includes Harper ($46.8M), Panta (YC W26), and the $900M acquisition of Newfront by WTW.

---

## Part 3: Alignment with agent-resolv

### Where We Align

| Shared Thesis | Details |
|---|---|
| AI disrupts traditional brokerage | Both believe AI can automate analysis, comparison, and recommendation workflows |
| Licensed broker model | Both operate as licensed brokers (not MGAs or carriers) |
| Business customers, not consumers | Both target companies, not individual insurance buyers |
| Customer-aligned incentives | Both emphasize transparency over traditional broker conflicts |
| Email/human-mediated carrier integration | Neither relies on real-time carrier APIs — the industry doesn't support it |
| Proprietary data layer as IP | Both build structured schemas from unstructured insurance data |

### Where We Diverge

| Dimension | WithCoverage | agent-resolv |
|---|---|---|
| **Market** | US (multi-state, $1.4T market) | Portugal (single jurisdiction, €14.3B market) |
| **Target customer** | Mid-market, high-growth ($100K+ insurance spend) | SMEs |
| **Revenue model** | Flat fee (non-commission) | Commission-based (10–25% of premiums) |
| **Automation depth** | AI-augmented humans (50–70 employees for 700 clients) | AI-native (targeting much higher automation) |
| **Scope** | Insurance only | Insurance + credit (dual ASF + BdP license) |
| **Customer channel** | Web platform + human advisors | Omni-channel (WhatsApp, email, voice, Telegram) |
| **Team model** | Heavy specialist headcount | Lean, 2-person studio + domain expert |
| **Funding** | $50.5M raised | Targeting €250K pre-seed |

### What's Better About Our Approach for Portugal

**Omni-channel > Dashboard.** WithCoverage built a full web app because US mid-market companies expect SaaS platforms. Portuguese SMEs don't want another login. WhatsApp is the right primary interface — it's where they already communicate. The backend data layer matters, but the customer never needs to see a dashboard.

**Dual license (ASF + BdP) creates a moat.** No US competitor offers both insurance and credit brokerage. Becoming the single financial intermediary for SME risk and capital needs is unique.

**Commission model fits SMEs.** A flat fee makes sense when you're saving a company $100K+/year. For Portuguese SMEs with €2K–€5K annual premiums, a flat fee would feel expensive. Commission-based is the right model for this segment.

**Leaner by necessity = faster to software economics.** WithCoverage has ~10 clients per employee. Agent-resolv needs to target 50–100+ clients per human from the start, which forces deeper automation earlier.

---

## Part 4: Learnings for Our MVP

### 1. Start with the Audit, Not the Quote

WithCoverage's most powerful product insight: the sale begins with analysis, not quoting. Their free risk audit creates a no-risk entry point and immediately demonstrates value.

**What we can do:** Build a "Diagnóstico de Seguros" — the customer sends photos or PDFs of existing policies via WhatsApp. AI extracts coverage details, identifies gaps, benchmarks against market. Deliver a health check report within hours. This works without any provider integration and generates the structured data that feeds everything else.

### 2. The Schema Is the IP, Not the AI Model

WithCoverage built a proprietary Agency Management System from scratch. The reason: traditional systems weren't designed for machine consumption. Their AMS is the normalized data layer that makes AI agents possible.

**What we can do:** Our CustomerProfile, QuoteRequest, and NormalizedQuote schemas are the Portuguese equivalent. Every policy parsed, every quote received, every provider email processed feeds this schema. Over time, this structured representation of an unstructured market becomes our defensible asset. Prioritize schema design over AI sophistication in MVP.

### 3. Pick One Vertical, Go Deep

WithCoverage started with DTC/CPG brands, then expanded to hospitality, then construction. Each vertical has different risk profiles, policy types, and carrier preferences.

**What we can do:** Choose one Portuguese SME vertical for MVP. Hospitality (restauração/alojamento local) is compelling — large segment in Portugal, well-defined insurance needs (responsabilidade civil, multirisco, acidentes de trabalho), and the owners are WhatsApp-native. Build everything for that vertical first: schemas, agent prompts, provider email templates. Expand after we've nailed it.

### 4. Email-Based Integration Is Validated, Not a Compromise

Even with $50M in funding in the US market, WithCoverage uses human-mediated carrier relationships. They don't have real-time API connections to insurers.

**What we can do:** Stop treating email-based integration as a limitation. Frame it as: "Even the best-funded US insurtech uses traditional carrier channels. We automate the mediation layer with AI — turning email into a structured API." The AI agent that sends structured quote requests and parses incoming email responses is a natural fit for the async nature of carrier quoting.

### 5. Build the Back Office First, Customer-Facing Second

WithCoverage's platform manages COIs, claims tracking, billing, proposals. The internal tooling enables the team to operate efficiently before the customer-facing experience scales.

**What we can do:** Build the AMS (internal tool for Rolando and operations) before the customer-facing WhatsApp flows. The internal system is where structured data lives, where human-in-the-loop approvals happen, where compliance audit trails are maintained. The customer interface is just a channel layer on top.

### 6. Free Audit as GTM Engine

WithCoverage's free insurance analysis is their primary acquisition channel. It's promoted everywhere — website, LinkedIn, PR coverage. The audit sells itself because it quantifies waste.

**What we can do:** This could be our primary GTM motion. Portuguese SMEs rarely review their insurance. A free, AI-powered audit that says "you're paying €800/year too much on your multirisco, and you have a gap in responsabilidade civil profissional" is immediately actionable and shareable.

---

## Part 5: Learnings for Post-MVP Scaling

### 1. Vertical Expansion Playbook

WithCoverage's pattern: dominate one vertical → document the playbook → hire a specialist for the next vertical → repeat. Each vertical adds schema extensions, carrier appetite knowledge, and referral networks.

For agent-resolv, after hospitality: retail → professional services → construction. Each vertical in Portugal has a manageable number of relevant insurance products and a finite set of carriers competitive for that segment.

### 2. Claims Handling as Retention Moat

WithCoverage's in-house claims attorneys are their biggest differentiator. Claims moments are where trust is built or destroyed.

For agent-resolv, full claims management is out of MVP scope, but a claims tracker via WhatsApp ("O seu sinistro está em fase de peritagem — previsão: 15 dias") would be differentiated in Portugal where claims processes are opaque.

### 3. The "Service-as-Software" Scaling Reality

Despite the AI vision, WithCoverage has ~10 clients per employee. The "breaking scaling laws" ambition is real but unproven. The human-in-the-loop requirements in insurance are persistent — regulatory compliance, carrier negotiations, complex claims.

For agent-resolv, design the system so AI handles 80% of the workflow and humans handle the 20% that requires judgment. The metric isn't "zero humans" — it's clients per human. Target 50–100+ to achieve viable unit economics with our lean model.

### 4. Pricing Model Evolution

WithCoverage's flat-fee model works for mid-market companies with large insurance spend. But their backend likely still involves carrier commissions.

For agent-resolv, start with commission-based (aligns with Portuguese SME expectations and smaller premiums). As we grow, consider a hybrid: commission + optional premium advisory tier for larger clients who want proactive risk management beyond basic brokerage.

### 5. Adjacent Revenue Opportunities

WithCoverage is "exploring opportunities beyond insurance in response to customer demand." Our dual license (insurance + credit) means we already have this built in from day one. Cross-selling credit products to insured SMEs is a natural expansion that no pure insurance play can match.

### 6. Investor Narrative Strengthened

WithCoverage's $42M Series B from Sequoia and Khosla, Harper's $46.8M raise, Newfront's $900M acquisition by WTW, and BofA's estimate of $15B in at-risk commissions — this is powerful validation material for our investor conversations. The global wave is real; we're the Portuguese play in a market with <1% online penetration.

---

## Part 6: Market Approach — B2C vs. B2B vs. B2B2C

WithCoverage is purely B2B: they sell directly to companies, replacing the company's existing broker. This works in the US because the market is massive, mid-market companies have dedicated finance teams who evaluate vendors rationally, and switching brokers is a business decision, not a personal one.

Portugal is different. The insurance market is relationship-driven, fragmented, and dominated by a mediation layer (~25,000 mediadores + 68 independent brokers) that controls distribution. How agent-resolv enters this market is arguably the most consequential strategic decision we face. There are three viable approaches, each with fundamentally different implications.

### Option A: B2C — Direct to Individual Consumers

Sell insurance directly to individuals (auto, home, health, life) through AI-powered comparison and recommendation.

**How it would work:** Consumer visits agent-resolv via web or WhatsApp → provides personal details → AI compares quotes across insurers → recommends and helps bind the best policy.

**Pros:**
- Large addressable market (~10M potential customers in Portugal)
- Simpler insurance products (auto, home) are easier to automate end-to-end
- Consumer-facing brand could achieve viral growth via WhatsApp sharing
- Comparadores like Compareamais have proven there's demand for price comparison

**Cons:**
- Low margins per customer (individual premiums are small, €300–€1,500/year typical)
- High customer acquisition cost (competing against established brands like Fidelidade, Ageas, Tranquilidade with massive ad budgets)
- MUDEY attempted this in Portugal and only raised €720K — weak investor signal
- AI adds limited value because personal insurance decisions are relatively simple
- Requires significant marketing spend we don't have at €250K funding
- Price-sensitive customers churn easily at renewal (no loyalty to a platform)
- Commission per policy is too small to build viable unit economics without massive volume

**Unit economics sketch:**
- Average personal lines premium: ~€500/year
- Commission at 15%: €75/year per customer
- Estimated CAC (digital): €30–€50
- LTV (assuming 3-year retention): €225
- LTV:CAC ratio: ~4.5–7.5×

**Verdict:** Feasible but capital-intensive, low-margin, and hard to differentiate. Not aligned with our funding level or the AI-native thesis. The AI is overkill for simple products, and the volume required to make the economics work demands marketing budgets we don't have.

### Option B: B2B — Direct to SMEs (WithCoverage Model)

Sell insurance brokerage services directly to small and medium businesses, replacing or capturing their existing mediador relationship.

**How it would work:** SME receives a free insurance audit via WhatsApp/email → AI identifies gaps and savings → agent-resolv becomes their broker of record → manages all commercial insurance (responsabilidade civil, multirisco, acidentes de trabalho, frota, etc.).

**Pros:**
- Higher value per customer (SME premiums range from €2,000–€20,000+/year)
- More complex insurance needs = more value from AI (gap analysis, multi-policy optimization, compliance)
- Commission per customer is meaningful (€250–€2,500+/year)
- The "free audit" GTM motion works because there's real money to save
- Dual license (insurance + credit) creates unique cross-sell opportunity
- Validated globally by WithCoverage ($42M), Harper ($46.8M), and others
- Aligns with "service-as-software" thesis that excites investors

**Cons:**
- SMEs in Portugal are relationship-driven — trust matters more than technology
- We're a new, unknown brand competing against mediadores they've known for 15 years
- Requires ASF license before we can operate (timeline risk)
- Sales cycle is longer than B2C — need to convince a business owner to switch
- The 68 independent brokers and mediador network could become actively hostile if we position as a replacement
- Smaller total number of target customers than B2C (~400,000 SMEs in Portugal, but only a fraction are addressable)

**Unit economics sketch:**
- Average SME insurance spend: ~€5,000/year (across multiple policies)
- Commission at 15%: €750/year per customer
- Credit cross-sell adds ~€200–€500/year
- Estimated CAC: €100–€200 (outbound + referral)
- LTV (assuming 5-year retention): €4,750–€6,250
- LTV:CAC ratio: ~24–62×

**Verdict:** Strongest unit economics and highest alignment with AI-native thesis. But requires building trust in a relationship-driven market from scratch, and risks antagonizing the existing mediation ecosystem.

### Option C: B2B2C — Platform for Mediadores (Recommended Hybrid)

Build agent-resolv as the AI-powered backbone that makes existing mediadores and brokers 10× more productive, while agent-resolv holds the license and controls the customer relationship over time.

**How it would work:** Mediadores like Rolando use agent-resolv's platform as their operating system. AI handles intake (via WhatsApp), policy analysis, quote comparison, submission prep, and ongoing management. The mediador handles relationship, judgment calls, and human-in-the-loop approvals. The SME customer interacts via WhatsApp and gets exceptional service — powered by AI, delivered through their trusted mediador.

**Phase 1 (MVP):** Tool for 5–10 mediadores. Agent-resolv holds the ASF license, mediadores operate under it. Revenue = commission split on policies managed through the platform.

**Phase 2 (Scale):** Expand the mediador network. Build the brand with SMEs. Start acquiring SME customers directly in verticals where mediadores are weak or absent.

**Phase 3 (Own the relationship):** With enough data, brand recognition, and proven service quality, shift toward direct-to-SME in parallel with the mediador channel. The platform becomes the industry standard; mediadores depend on it.

**Pros:**
- Works WITH the Portuguese market structure instead of against it
- Mediadores provide instant distribution — no need to build trust from scratch
- Dramatically lower CAC (mediadores bring their existing clients onto the platform)
- Rolando is the perfect pilot user — he's our qualified director AND a practicing mediador
- De-risks the licensing timeline (we're enabling licensed professionals, not replacing them)
- Avoids antagonizing the mediation ecosystem — we're their ally, not their threat
- Builds the data moat faster (multiple mediadores feeding the same schema layer)
- Investor story is stronger: "We're not fighting the market — we're capturing it through existing distribution, then owning the customer relationship over time"
- Proven playbook: Mannie.pt reference model uses a similar distribution partnership approach

**Cons:**
- Revenue is split with mediadores (lower margin per policy than direct B2B)
- Mediadores may resist adoption (technology skepticism, fear of disintermediation)
- Dependent on mediador quality and engagement — if they don't use the tool, it doesn't work
- Risk of mediadores leaving with "their" clients once they've learned the value
- Slower path to owning the end-customer relationship
- More complex product (need both mediador-facing tools AND customer-facing WhatsApp flows)
- The 68 independent brokers is a small addressable market for broker-as-customer SaaS; the ~25,000 mediadores are larger but fragmented

**Unit economics sketch:**
- Average SME insurance spend: ~€5,000/year
- Commission at 15%: €750/year gross
- Split with mediador (e.g., 60/40 or 50/50): €375–€450/year net to agent-resolv
- Credit cross-sell adds ~€100–€250/year net
- Estimated CAC per mediador: €200–€500 (but each mediador brings 20–50+ clients)
- Effective CAC per SME: €5–€25
- LTV per SME (5-year, net after split): €2,375–€3,500
- LTV:CAC ratio: ~95–700× (extraordinarily efficient due to mediador-as-channel)

**Verdict:** Best risk-adjusted path for a pre-seed startup in the Portuguese market. Lower margin per customer than direct B2B, but dramatically lower CAC, faster distribution, and a path that evolves toward direct-to-SME over time.

### Side-by-Side Comparison

| Dimension | B2C (Consumers) | B2B (Direct to SMEs) | B2B2C (Via Mediadores) |
|---|---|---|---|
| **Target customer** | Individuals | SMEs directly | SMEs via mediadores |
| **Revenue per customer/year** | ~€75 | ~€750–€950 | ~€475–€700 |
| **CAC** | €30–€50 | €100–€200 | €5–€25 (effective) |
| **LTV:CAC** | 4.5–7.5× | 24–62× | 95–700× |
| **AI value-add** | Low (simple products) | High (complex needs) | High (complex needs) |
| **Time to first revenue** | 6–9 months | 6–12 months | 3–6 months |
| **Trust barrier** | Medium (brand needed) | High (replacing relationship) | Low (mediador provides trust) |
| **Market friction** | Competing with insurers | Competing with mediadores | Partnering with mediadores |
| **Scaling capital needed** | High (marketing) | Medium (sales team) | Low (platform adoption) |
| **Dual license value** | Low | High | High |
| **Investor appeal** | Weak (MUDEY precedent) | Strong (WithCoverage comp) | Strong (platform + distribution) |
| **Defensibility** | Low (price competition) | Medium (schema + brand) | High (network + schema + data) |
| **Path to €1M ARR** | ~13,000 customers | ~1,100–1,300 customers | ~1,500–2,100 SMEs via 30–50 mediadores |

### Our Recommendation: Start B2B2C, Evolve to B2B

The B2B2C model via mediadores is the most capital-efficient entry strategy for a €250K pre-seed round in Portugal. It works with the market instead of against it, leverages Rolando as both qualified director and pilot mediador, and builds the data and schema moat that makes everything else possible.

The evolution path is clear:

1. **Months 1–6:** Build the AI-powered toolkit. Pilot with Rolando and 3–5 mediadores. Prove the workflow automation works. Collect data. Earn commissions through the mediador channel.

2. **Months 6–12:** Expand to 15–25 mediadores. Refine the schema. Start building brand recognition with SMEs who experience the service. Launch the "Diagnóstico de Seguros" as a public-facing lead gen tool.

3. **Months 12–18:** Begin direct-to-SME acquisition in verticals where the platform has proven traction. Mediador channel continues in parallel. The platform becomes the default operating system for independent insurance mediation in Portugal.

This is not a compromise — it's a deliberate strategy that acknowledges the Portuguese market reality while building toward the same end state as WithCoverage: owning the customer relationship with superior technology.

---

## Key Takeaways for the Team

1. **The thesis is validated.** AI-driven brokerage disruption is attracting serious capital globally. We're not early to an unproven idea — we're applying a proven model to an underserved market.

2. **Start B2B2C, evolve to B2B.** Enter the market through mediadores (starting with Rolando), not against them. Build the platform that makes them 10× productive, then expand to direct-to-SME as the brand and data compound.

3. **Start with the audit.** The "free insurance health check" is both our lead gen engine and our data bootstrapping strategy. Build this before anything else.

4. **The schema is the product.** The normalized data layer that makes Portuguese insurance machine-readable is our core IP. Invest in getting this right.

5. **Email integration is the industry standard.** Don't apologize for it. Even $50M doesn't buy carrier APIs.

6. **Go vertical-first.** Pick hospitality, nail it, then expand.

7. **Omni-channel is our edge.** WhatsApp-first beats dashboard-first for Portuguese SMEs.

8. **Dual license is a unique moat.** No comparable company globally offers both insurance and credit brokerage through a single AI-native platform.

---

*Sources: Sequoia Capital, Pulse 2.0, Reinsurance News, Crowdfund Insider, InsurTech Analyst, FinTech Global, Beinsure, WithCoverage website and app, Greenhouse job postings, ZoomInfo, Bloomberg, Fortune, McKinsey*
