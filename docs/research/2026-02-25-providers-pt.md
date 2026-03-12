# Portugal Provider Landscape: Insurance & Credit Intermediation

> **Date:** 2026-02-25
> **Context:** agent-resolv provider-side research — assessing integration path and time-to-revenue for an AI-powered insurance + credit intermediary in Portugal.

---

## Executive Summary

Portugal presents a compelling entry window for an AI-powered intermediary. The insurance market reached **€14.3 billion** in gross written premiums in 2024, yet digital distribution accounts for less than 3% of non-life sales. No dominant AI-native intermediary bridges insurance and credit products. The regulatory framework permits fully digital brokers — MUDEY and Lovys have already obtained ASF registration — and the credit intermediation regime explicitly allows distance-only operation. Meanwhile, insurer technical integration remains stuck in a portal-and-webservice era with no standardized APIs, creating a modernization gap that a tech-native entrant can exploit.

The market's structural features favor disruption: **69 registered brokers** serve the entire country, the agent population has halved since 2012 (from ~22,000 to ~10,400), and health insurance is booming at 19% annual growth. Banks are actively expanding intermediary partnerships for credit products during a mortgage boom that hit **€23.3 billion** in new lending in 2025 — the highest since 2014.

---

## 1. Insurance Market Structure

### Market Concentration

Portugal's insurance sector is controlled by a tight oligopoly:

| Insurer Group | Ownership | Total Market Share | Primary Segments |
|---|---|---|---|
| **Fidelidade** | Fosun / CGD | **30.6%** | All lines — dominant in life, auto, health, workers' comp |
| **Ageas Portugal** | Ageas / Millennium BCP JV | **~18%** | Life (Ocidental Vida), non-life (Ageas Seguros) |
| **Generali / Tranquilidade** | Generali (absorbed Liberty 2024) | **~13%** | Non-life strong; 2,500+ points of sale |
| **Allianz** | Allianz SE | **~5.8%** | Non-life; agent/broker focused |
| **BPI Vida e Pensões** | CaixaBank / BPI | **~5%** | Life — exclusive bancassurance |
| **Santander Group** | Santander | **~4-5%** | Life — exclusive bancassurance |
| **Zurich** | Zurich Group | **~3%** | Non-life; broker-friendly |
| **CA Seguros / Vida** | Crédito Agrícola | **~2-3%** | Bancassurance-captive |
| **Real Vida** | Independent | **~1-2%** | 100% mediator channel — no bank partner |

Top three groups control ~62% of total premiums. Top five ≈ 70%.

### Segment Breakdown (2024)

- **Life insurance:** €6.9 billion (+35% YoY) — dominated by bank-distributed savings/investment products benefiting from higher interest rates.
- **Non-life insurance:** €7.4 billion (+10.4%) — the largest growth in 25 years.
  - Motor: ~€1.8 billion
  - Health: ~€1.7 billion (19% annual growth, 4 million beneficiaries)
  - Workers' compensation: ~17.7% of non-life
  - Fire/property/multiriscos: significant
  - Travel, liability, other: growing

### Distribution Channel Split

| Channel | Life share | Non-life share | Total market share |
|---|---|---|---|
| **Bancassurance** | 72.8% | ~16% | 42.4% (€6.1B) |
| **Agents** | ~15% | ~55-60% | ~35% (€5.0B) |
| **Brokers** | ~5% | 20-25% | ~15% (€2.15B) |
| **Direct / digital** | ~7% | ~7.6% | ~7% |

Key insight: **banks dominate life; agents/brokers dominate non-life**. Digital remains <3% of non-life — a massive underpenetrated channel.

---

## 2. Openness to Independent Intermediation

### Broker-Friendly Insurers (Priority Targets)

- **Fidelidade** — Voted "Best Insurer for Brokers" (APROSE 2023-24). Serves ~3,000 external mediators via proprietary platform. Despite CGD bancassurance backbone, actively invests in independent distribution.
- **Generali/Tranquilidade** — 2,500+ points of sale: 80 brokers, 2,100 multi-brand agents, 350 exclusive agents. Voted "Best Insurer for Agents." Liberty acquisition (2024) expanded broker network.
- **Allianz** — Agent/broker-focused; no major bank partner. Offers "AllianzNet" mediator portal with webservice integration.
- **Zurich** — Consistently top-rated by brokers; focused on independent distribution.
- **Real Vida** — **100% mediator channel** (no bank partner). Voted "Best Life Insurer" by APROSE.
- **Lusitânia / Victoria** — Mid-tier, mediator-dependent, likely receptive to new digital brokers.

### Bancassurance-Captive (Not Accessible via Broker Channel)

- **BPI Vida e Pensões** — Exclusive CaixaBank/BPI distribution.
- **Santander/Aegon** — Exclusive Santander branch distribution.
- **CA Seguros/Vida** — Exclusive Crédito Agrícola distribution.
- **Ocidental Vida** (Ageas life arm) — Primarily via Millennium BCP. Non-life Ageas Seguros is broker-accessible.

### AI-Powered Intermediaries Active in Portugal

No insurer currently works with a pure AI-powered intermediary. The closest players are:

- **Habit Analytics** (Lisbon) — Digital broker providing a single API for distributing multiple insurance products. Powers embedded insurance for NOS (telecom), Ergovisão (optical), Luz Saúde (healthcare). Risk underwritten by Allianz Partners, MetLife.
- **MUDEY** — Digital insurance comparison/purchase platform. ASF-registered.
- **Lovys** — Digital neo-insurer (French-origin, PT operations). ASF-registered. €20M+ raised.
- **MOVA Seguros** — AI-powered digital broker.
- **Indie Seguros** — Insurance-as-a-service for platforms.

---

## 3. Technical Integration Maturity

### Current State: Portal + Webservices (No APIs)

Portuguese insurers **do not offer modern RESTful APIs** for quote/bind workflows. Distribution technology sits in a portal-and-webservice paradigm:

- Each major insurer operates a **proprietary mediator portal** (web login).
- Data exchange uses **SOAP/XML webservices**, mediated by broker management software.
- No standardized API format or open insurance initiative exists in Portugal today.

### Broker Management Platforms (De Facto Middleware)

| Platform | Market Position | Insurer Integrations |
|---|---|---|
| **lluni Seg** | 380+ mediator installations, cloud SaaS | Webservice integration with major insurers |
| **GIS Mediadores (i2S)** | Market leader since 1984, used by top brokerages | Deepest integrations |
| **Gemese** | Smaller mediators | Selected insurers |
| **Anybroker** | Launched 2023, multi-insurer quotation | Growing; single-interface quoting |

Insurers confirmed with webservice integrations: Ageas, Allianz, Caravela, Fidelidade, Generali/Liberty, Lusitânia, Victoria, Zurich.

### Comparison / Aggregator Platforms

- **ComparaJá.pt** — Works with 15+ insurers. Form submission → human advisor callback → phone contracting. **No real-time API-driven quote-and-bind.**
- **CompareOMercado.pt**, **Comparador.pt** — Similar model.

### Insurtech Middleware

- **Habit Analytics** — Single API for multi-insurer product distribution. The most technically advanced option for embedding.
- **Weecover** (Spanish, operating in PT) — Full-lifecycle SaaS with API-based quote-issue-administer-claims.

### Integration Path for a New Broker

1. **Near-term (0-12 months):** Connect through existing broker management platforms (lluni, i2S/GIS) or partner with middleware insurtechs (Habit, Weecover). Portal scraping as fallback for non-integrated insurers.
2. **Medium-term (12-24 months):** Build direct webservice connections with priority insurers (Fidelidade, Tranquilidade, Allianz, Zurich).
3. **Long-term (2027-2028):** EU FiDA Regulation will force standardized API access for non-life insurance data. But cannot wait for it.

### Credit Side: Open Banking

PSD2-compliant open banking APIs exist at major banks (CGD, BCP, Santander, BPI, Novobanco) for account information and payment initiation. **No standardized credit product distribution APIs** exist. Credit intermediary-bank integration relies on:

- **eGO CRM** — Building proprietary bank connections for credit intermediation.
- **CrediDesk** — CRM platform for credit intermediaries with bank integrations.
- **Adnew** — CRM with specific credit intermediation module.

---

## 4. Commission Structures

### Overall Market

Total mediation remuneration: **~€1.3 billion** (2024, +17% YoY). Overall average commission rate: **9.1%** (declining slightly as low-commission life savings products surge). Brokers average **~18.1%** across portfolios; agents average **~8.9%**.

### Estimated Commission Ranges by Product

| Product | Broker Commission | Agent Commission | Notes |
|---|---|---|---|
| **Auto insurance** | 15–22% | 10–15% | High volume, competitive; obligatory |
| **Home / multiriscos** | 15–20% | 10–15% | Often tied to mortgage |
| **Health insurance** | 10–18% (individual) / 3–5% (large group) | 5–10% | Fastest-growing; highly variable by size |
| **Life risk** | 20–30% | 15–25% | Higher for individual policies |
| **Travel** | 20–30% | 15–25% | Low premium, high margin % |
| **SME property / liability** | 18–25% | 12–20% | Broker-dominated for complex risks |
| **Workers' compensation** | 15–20% | 10–15% | Mandatory for employers |
| **Life investment / PPR** | 2–7% | 1–5% | Very low; bank-dominated |

### Volume Requirements & Incentives

- **No statutory minimum production thresholds.**
- Insurers practically expect a viable client portfolio and adequate technical infrastructure.
- Large brokers negotiate performance-based **override commissions** and **profit-sharing** tied to loss ratios below ~70%.
- Periodic **incentive campaigns** (*campanhas*) tied to volume targets and product pushes.
- A **2% stamp duty** (*Imposto do Selo*) applies to all insurance mediation commissions.

### Top Brokers by Revenue (Reference)

| Broker | Annual Commission Revenue |
|---|---|
| MDS Portugal | ~€42M |
| Sabseg | ~€42M |
| AON | ~€25.5M |
| Marsh | ~€23.5M |
| Verlingue | ~€16.7M |

Average broker firm: **€3.15 million/year** in commissions.

---

## 5. Credit / Lending Side

### Market Context

- New mortgage lending (2025): **€23.3 billion** — highest since 2014.
- New mortgage contracts (2024): **125,360** (+26.6% YoY).
- Young borrowers (18-35): **43%** of new contracts.
- Total outstanding mortgage stock: **€106.1 billion**.
- Government public guarantee scheme enabling 100% financing for young buyers.

### Key Banks & Intermediary Openness

| Bank | Intermediary Partnerships | Notes |
|---|---|---|
| **CGD** | Works with major intermediary networks | Largest bank; state-owned |
| **Millennium BCP** | Partners with intermediaries | Largest private bank |
| **Santander PT** | Active intermediary program | Strong digital agenda |
| **BPI** | Published intermediary remuneration policy | CaixaBank-owned; transparent commission model |
| **Novobanco** | Partners with D&S, Maxfinance, others | Aggressive in mortgage market |
| **Bankinter** | Works with intermediaries | Growing player |
| **Banco CTT** | Partners with intermediary networks | Postal bank; digital-forward |
| **ABANCA / UCI** | Key partner for Maxfinance | Specialist mortgage lender |
| **Crédito Agrícola** | Works with intermediaries | Cooperative; regional strength |
| **Consumer credit:** Cofidis, Credibom, BNP PF | Active intermediary programs | Consumer / auto loan specialists |

### Commission Structures (Credit)

- **Mortgage referral commissions:** estimated **0.3–1.5% of financed amount**, scaling with production volume.
- BPI's public remuneration policy: marginal rate schedule combining **quantitative** (cumulative volume) and **qualitative** (complaint ratios, compliance quality) criteria.
- Auto loan commissions: NPV-based calculations with annual **rappel** (volume bonuses).
- Commissions are paid by banks, not consumers.

### API Availability

- **PSD2 open banking APIs:** Available at CGD, BCP, Santander, BPI, Novobanco for account info and payment initiation.
- **Credit product distribution APIs:** None standardized. Integration relies on CRM platforms (eGO CRM, CrediDesk, Adnew) building proprietary bank connections.
- **Digital-only operation:** Explicitly permitted under DL 81-C/2017.

### Established Intermediary Networks (Incumbents)

| Network | Model | Scale | Bank Partners |
|---|---|---|---|
| **Maxfinance** | Franchise | Largest network | UCI/ABANCA, others |
| **Decisões e Soluções** | Franchise | 17+ years | Novobanco, CGD, Santander, BPI, Bankinter, Banco CTT |
| **Doutor Finanças** | Franchise + digital | 200+ staff, 39 units, 2M+ monthly web sessions | Multiple banks |
| **ComparaJá.pt** | Digital comparison | Registered as tied intermediary #375 | Multiple banks |

**None are truly AI-native.** Doutor Finanças comes closest with proprietary tech but franchise royalties (8-30%) indicate significant cost structures.

---

## 6. Incumbents Moving on AI

### Insurance Sector

| Player | AI Activity | Threat Level to Independent AI Broker |
|---|---|---|
| **Fidelidade** | Center for AI & Analytics; GAMA system processes 50%+ of accident declarations automatically. Won Portugal Digital Awards 2024. Considering selling GAMA externally. | **Low** — AI is internal ops, not distribution. Actively dependent on broker network. |
| **MAPFRE** (Spain/PT) | 115+ AI initiatives globally. MIA GPT assistant serves 3,000+ intermediaries. IRIS deep learning for vehicle damage. 40%+ of ops handled by virtual assistants. | **Low** — MIA GPT explicitly *empowers* brokers, not replaces them. |
| **Generali/Tranquilidade** | Early stage; leadership described autonomous AI as "too big an ethical risk" currently. | **Very low** |
| **Ageas** | Actively hiring for AI roles; no major deployments announced. | **Very low** |

### Banking Sector

| Bank | AI Activity | Impact on Credit Intermediation |
|---|---|---|
| **Santander** | OpenAI partnership; €200M+ AI-generated savings (2024); AI-driven Next Best Offer models power 85% of sales for top products. | **Medium** — Strong internal AI but continues expanding intermediary partnerships. |
| **BPI** | Centro de Excelência em IA; ML models drive 70%+ of commercial outreach. | **Medium** — AI-augmented internal sales, still using intermediaries. |
| **Novobanco** | Quantexa + Microsoft partnership; 50+ operational AI models. | **Medium** — Enterprise data/AI transformation focused internally. |
| **CGD** | AI-powered virtual assistant for 2M+ digital customers. | **Low** — Operational; no intermediary displacement. |
| **Millennium BCP** | AI investments; details less public. | **Low** |

### Key Insight

**Incumbent AI deployments enhance rather than threaten the intermediary model.** No insurer or bank in Portugal is building an AI-powered independent distribution capability. The greater risk is a well-funded Spanish insurtech (Coverfy, Weecover) or international player entering first.

### Insurtech Ecosystem (PT + Iberia)

**Portugal:** Habit Analytics, MUDEY, Coverflex (€29.4M raised), Lovys (€20M+), MOVA Seguros, Indie Seguros.

**Spain (cross-border risk):** 250+ startups, 65 pure insurtechs. Key: Coverfy (AI insurance manager), Life5/Getlife (digital life, planning PT entry), Weecover (SaaS, already in PT).

---

## 7. Strategic Implications for agent-resolv

### Priority Entry Segments

1. **Non-life insurance** — Brokers control 20-25% of distribution, earn ~18% commissions. Health insurance (19% growth) and auto (mandatory, high volume) are the entry wedge.
2. **Mortgage credit** — Booming at €23.3B/year, banks actively recruiting intermediary partners, no AI-native player.
3. **SME products** — Higher commissions (18-25%), less price-sensitive, broker-dominated.

### Integration Strategy

- **Phase 1:** Connect via lluni/i2S middleware or Habit Analytics API for insurance. Use eGO CRM / CrediDesk for credit bank connections.
- **Phase 2:** Direct webservice integrations with Fidelidade, Tranquilidade, Allianz, Zurich.
- **Phase 3:** Leverage FiDA (2027-28) for standardized API access.

### Licensing Requirements

- **Insurance broker (corretor):** ASF registration. PII ≥€1.3M, surety bond ≥€19,510, 120h coursework, **5-year experienced person required**. ~€50K year-1 cost.
- **Credit intermediary (vinculado):** Banco de Portugal registration. Professional certification (IFB/ESAI), liability insurance. Digital-only permitted.

### Competitive Moat

- **No player combines AI-powered insurance + credit intermediation** in a single platform.
- Portuguese-language AI is an acknowledged gap (Fidelidade's AI lead noted this publicly).
- Agent population halving creates a distribution vacuum.
- The FiDA regulation will eventually commoditize API access — first movers with established client relationships win.
