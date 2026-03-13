# Opus Prompt — Investment Memorandum (agent-resolv)

> **Status:** Ready to send. Queue in the morning (full Opus session).
> **Expected output:** `research/2026-02-25-investment-memo.md`
> **Note:** Most research-loaded prompt to date. Allow 30-40 min extended thinking.

---

**Context:** You are producing an investment memorandum for agent-resolv, a company being built by ReisierX (Gonçalo Reis, founder). The immediate audience is BuildUp Labs (BuL), a Lisbon accelerator considering incubating agent-resolv. The call is Friday February 27. This memo is the long-form, evidence-dense foundation — a shorter brief will be extracted for the actual meeting, but this document needs to be thorough enough to stand alone.

**What agent-resolv is:** An AI-native broker agent for credit and insurance intermediation in Portugal. It IS the broker — sitting between consumers (human or AI agent) and providers (insurers, banks). It ingests requests via WhatsApp, Telegram, email, voice, or API, compiles information, routes to providers, analyses responses, and returns recommendations. It is not a consumer-facing runtime (that's OpenClaw/OpenAI/Meta's problem) — it ingests from humans or AI agents equally, treating both as first-class clients.

**Core design philosophy — customer-in, product-out:** The entire credit and insurance industry is built backwards. Suppliers design pre-packaged products. Brokers present what earns them the most commission. Customers receive 80-page PDFs designed for compliance, not clarity, and are left to decipher and compare alone. agent-resolv inverts this: we start with the customer (what do you want to protect? how much coverage? for what? at what price?) and then go find what fits — independently, across all providers, without bias. This is not a UX improvement. It is a fundamentally different business model. Because we are licensed and independent, that trust is contractual. And because customers trust us, we can serve their full financial life.

**"A gente resolve":** Portuguese for "we sort it out." Also: *agent* + *resolv*. Triple meaning — the AI agent, the technical resolve, and the human promise. This is the brand story and the business model: trust as product, execution as differentiator.

The thesis rests on three inevitable dynamics: (1) agentic AI will absorb deterministic human workflows — credit/insurance intermediation is directly exposed; (2) all asset classes are moving on-chain, making AI a direct economic actor; (3) regulatory compliance and fit-and-proper processes are durable moats. The conclusion: aggregation is the winning model, and the broker agent is the right position. Rui Gouveia (BuL MD) introduced the framing "service as software" — agent-resolv IS this: a complete agentified brokerage service, not a subscription product.

**Read all of the following files before writing. They are your primary sources:**

- `projects/agent-resolv/CONTEXT.md` — thesis, locked decisions, current plan and status. Your synthesis anchor.
- `research/2026-02-25-competitive-landscape.md` — global competitive scan, 50+ companies, failure modes, business model economics, regulatory posture, A2A infrastructure.
- `research/2026-02-25-brokers-pt.md` — Portuguese broker/intermediary landscape: 10,289 mediators, 68 corretores, Doutor Finanças profile, Coverflex (agent license), Weecover (tech partner), B2B2C opportunity map.
- `research/2026-02-25-providers-pt.md` — Portuguese provider landscape: market structure, commission rates, integration path, incumbent AI posture, licensing requirements.
- `research/2026-02-25-consumers-pt.md` — Portuguese consumer/demand landscape: 1.1M SMEs, protection gap, digital behaviour, WhatsApp penetration, expat segment, credit intermediation growth.

**Produce a structured investment memorandum with the following sections:**

---

**0. Executive Summary (1 page)**
Stand-alone overview: the opportunity in one paragraph, the solution in one paragraph, the go-to-market in one paragraph, the ask in one paragraph. Someone who reads only this should understand what agent-resolv is, why the timing is right, and what BuL's involvement enables. Dense, no filler.

**1. Market & Opportunity**
- Why is this a compelling opportunity? Lead with the structural dynamics (agentic AI permeation, on-chain asset layer, compliance as moat).
- Market sizing: total addressable market in Portugal across insurance (€14.3B GWP, relevant broker-addressable segments) and credit intermediation (mortgage boom, €10.2B intermediated in 2024). Be specific and cite figures from the research.
- Customer segments: who are the customers? Profile each — SMEs (primary: 1.1M, 74% underinsured, mandatory insurance demand), personal lines (secondary), expat segment (1.54M, underserved, high willingness to pay). For each: size, current behaviour, pain points, willingness to adopt digital intermediation.
- Ideal Customer Profile (ICP): which specific segment to target first and why.
- Current suppliers (competition): who is doing this today? What are the analogues (Doutor Finanças, MUDEY, Coverfy, Harper, Flow Specialty)? What does the existing intermediary landscape look like?
- Structural gaps: what specifically does the market lack that agent-resolv provides?
- **Corollary — The Opportunity:** What is the precise opportunity, and why now? Include the three dynamics, the timing argument (IDD consolidation eliminating weak operators, FiDA incoming, mortgage boom, WhatsApp infrastructure mature, AI capability threshold crossed).

**2. Solution & Product**
- What is agent-resolv? Describe the broker agent clearly: the actors (consumer, broker agent, provider), the flow, the channels.
- **Produce Mermaid diagrams** for: (a) the actor/stakeholder map showing all parties and their relationships; (b) the primary user journey (SME customer initiates → broker agent → provider → recommendation returned); (c) the B2B2C embedded flow (platform embeds agent-resolv → end user interacts → providers connected).
- Product components: what does the system consist of? Describe each layer — ingestion (WhatsApp, Telegram, email, voice, API), broker agent (AI reasoning, information compilation, routing), provider integration layer (insurer/bank connections), advisory output, persistent customer account.
- **The customer account:** Every customer has a persistent account — not just a chat session. It shows: current insurance policies and credit facilities, ongoing processes, preference settings, and opportunities (upsell, renewal optimisation, cross-sell). This is the compounding asset where loyalty builds and LTV multiplies.
- **Channel stack:** WhatsApp (primary consumer, ~90% PT penetration); Telegram (primary for AI agents and tech-savvy users — the agent economy runs on Telegram); Email; Voice; API (Q3 2026). Each channel is a first-class entry point.
- **Machine-readable service endpoint:** agent-resolv publishes a structured, machine-readable service description for AI agents — intake schema, output format, channel availability, licensing status. A toggle on the website exposes a "Human" and "Agent" view. AI agents are first-class clients from day one, not an afterthought.
- Technology architecture: MCP as the internal interface layer between AI and integrations. Phase 1 integration via Habit Analytics API or lluni/i2S middleware. WhatsApp Cloud API. OpenClaw as initial runtime. A2A/A2P readiness for the agent economy.
- Design principles: what are the non-negotiables in how this is built? (customer-in not product-out; privacy; compliance-first; human+agent ingestion; transparency of reasoning; auditability)
- User journeys: walk through 2-3 key journeys in detail (SME getting workers' comp + liability bundle via WhatsApp; individual getting mortgage + life insurance; AI agent querying via Telegram or API for a structured recommendation).
- **Unique value proposition:** Apply Luís's framework — "from what is important to my customers, what only I can offer." Be specific. The answer should reference: independent corretor license (only 68 in PT), AI-native vs manual incumbents, insurance+credit combined (no one does this), WhatsApp-native, compliance-ready from day one.

**3. Regulatory & Licensing**
- The regulatory structure: licensed entity with AI tools — why this is the only viable EU model, and why it's a feature not a burden.
- ASF corretor de seguros: requirements, timeline, ~€50K year-1 cost, the 5-year experienced person requirement. Solution: Rolando (licensed agent, 20+ years) as the qualified director path.
- BdP independent credit intermediary: requirements, digital-only operation permitted.
- EU AI Act: high-risk classification for credit scoring and life/health insurance. August 2026 compliance deadline. How agent-resolv structures for compliance.
- Portugal FinLab: the regulatory dialogue channel.
- Why compliance = moat: 93.9% of credit intermediaries found irregular by BdP. 88% of SMEs can't explain their professional liability insurance. Incumbents are not audit-ready. Agent-resolv is.

**4. Go-to-Market**
- Distribution strategy: direct (WhatsApp, own app) vs B2B2C embedded (neobanks, HR platforms, proptech). Prioritise and justify.
- Phase 1 — Stealth pilot: who is the first customer? What does success look like? What is being proved? (Suggested: SME workers' comp + liability bundle via WhatsApp. Rolando as licensed anchor. Measure: time-to-quote, conversion, NPS.)
- Phase 2 — Market entry: expand to a defined customer segment with a repeatable playbook. Define the metrics that signal readiness to move here.
- Phase 3 — Scale: B2B2C embedded layer. Platform partnerships (Revolut PT, Rauva, Coverflex, Idealista). What does scale velocity look like?
- **Corollary:** What combination of channel + customer segment proves the model first? And what is the path to scale velocity?

**5. Financial Projections**
Label clearly as model projections (pre-revenue). Build a 3-year P&L model grounded in the research data:
- Revenue drivers: commission rates by product (auto 15-22%, SME property/liability 18-25%, health 10-18%, workers' comp 15-20%, mortgage referral 0.3-1.5%), average deal size, conversion assumptions, B2B2C platform fee structure.
- Key assumptions: customer acquisition rate, average policies per SME client, renewal rates, platform partnership timeline.
- Cost structure: fixed (licensing ~€50K yr-1, team, infrastructure) vs variable (per-transaction, compliance).
- Scenario analysis: conservative, base, upside. What are the key sensitivities?
- Unit economics: revenue per client, CAC, LTV, payback period.
- Milestones that de-risk the model.

**6. Resourcing & The Ask**
- Team required: what roles are needed and when? (Phase 1 vs Phase 2.) Who does what? What's missing?
- **BuL as Tech Co-Founder:** If BuL accepts to incubate agent-resolv, the expectation is that JP (BuL's CTO, João Pereira) would function as the de facto Tech Co-Founder — not merely as an advisor or a resource BuL lends to the portfolio company, but as an equity-holding co-founder with full ownership of the technical execution. This reframes the ask: it is not purely a capital or acceleration pitch, it is a co-founder proposal with a capital and network wrapper. The memo should make this explicit. Describe how the Gonçalo (business/domain/regulatory) + JP (technical) pairing maps to the execution plan, what JP would own in Phase 1 and Phase 2, and why this structure is preferable to hiring a CTO externally.
- Capital required: how much to reach the first meaningful milestone (proof of model)? What does it get spent on?
- Pre-seed terms: what is a reasonable investment size and structure for a regulated fintech pre-seed in Portugal/Europe? Equity, convertible note, SAFE? Valuation range?
- Ideal investors: who should be on the cap table? Strategic (insurers, banks, fintech platforms)? Financial (seed VCs, angels)? BuL's role?
- What does BuL's incubation specifically enable that capital alone wouldn't? Given the Tech Co-Founder framing, the answer is more than mentorship and network — it is the technical co-founder that makes the company viable at Phase 1.

**7. Risks & Mitigants**
Identify the top 5-7 risks honestly. For each: what is the risk, what is the probability/severity, what is the mitigation. Include: regulatory risk, provider integration risk (no APIs yet), distribution risk (D2C CAC), competitive risk (well-funded Spanish entrant), team/execution risk, timing risk.

**8. Why Us**
- Founder-market fit: why is Gonçalo the right person to build this? (Background, relationships, the ReisierX infrastructure already built, Rolando as licensed anchor, the external brain coordination pattern already operational.)
- What is already built: OpenClaw infrastructure, Google Workspace MCP, Gmail pipeline, agent identity (friedr.eth), agent-resolv.com domain (triple meaning — AI agent, resolve technically, *a gente resolve* — "we sort it out" in Portuguese), landing page live at agent-resolv.com with human/agent toggle.
- **The platform vision:** The beachhead is /insurance and /credit — regulated, licensed, hard, defensible, and confirmed by research. But the underlying thesis is not specific to those categories. The thesis is: any market where suppliers structure information to their advantage and consumers navigate complexity alone is attackable with the same model. The arc is agent-resolv as a platform of categories: agent-resolv/insurance → agent-resolv/credit → agent-resolv/irs-declaration → agent-resolv/energy → agent-resolv/phone-internet → and beyond. The moat built in regulated financial services (trust, license, track record, customer relationships) is the platform on which each new category is launched. The beachhead is not the product — it is the proof of model and the trust infrastructure. The 5-year vision is a platform where "a gente resolve" applies to any complex, provider-biased market a consumer or agent faces. Make this vision explicit in the memo, while being clear that the path there runs through insurance and credit first — focus and sequencing are the discipline.
- **A note on agent-economy categories:** There is a separate and distinct track for agent-to-agent services (e.g., a research/intelligence service where AI agents pay for structured analysis on a specific domain). This is architecturally different from consumer intermediation — it is a knowledge marketplace rather than a licensed advisory service — and should be positioned as a parallel track, not conflated with the consumer platform. The memo can acknowledge this as part of the longer-term agent economy positioning without mixing it into the core beachhead narrative.

---

**Style:** Dense and evidence-driven. Every claim should be grounded in data from the research files. Avoid corporate filler. Write as if presenting to a sophisticated audience that will fact-check every number. English throughout. Diagrams in Mermaid format.

**Output:** Save to `research/2026-02-25-investment-memo.md` in the `reisierx/workspace` repo and commit to main.
