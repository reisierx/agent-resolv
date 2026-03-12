# Integration Landscape Research

Session 1 deliverable. Research conducted 2026-02-27.

## Executive Summary

The PT insurance market has **no standardized broker APIs**. Every integration is bespoke and bilaterally negotiated. Two middleware platforms (lluni, i2S Brokers) dominate the broker software layer and already hold insurer integration credentials.

**Day-1 path: email/portal automation.** This is how every PT broker already works — emails, portal logins, PDF downloads. The AI layer automates what Rolando does manually. Zero commercial dependencies, works with any insurer from launch. Direct API integrations are a phase-2 optimization earned through volume.

**Integration ladder:**
1. Email/portal automation (day 1 — any insurer, no dependencies)
2. Caravela Seguros direct integration (Goncalo has contact with Luis Cervantes)
3. Fidelidade direct API (best-documented, 20+ REST APIs, OAuth-based)
4. lluni/i2S partnership (evaluate after direct contact, avoid platform dependency)

**Long-term possibility (Plaid model):** Build internal abstraction layer that normalizes insurer data (quotes, policies, claims) into a unified schema. If this proves robust across enough insurers, it could open up to other brokers — similar to how Plaid started as an internal banking integration tool and became the platform. Not a launch priority; let it emerge organically from the broker's operational needs.

**Biggest risk:** Format fragmentation across insurers. Each has different quote/policy data structures. Session 2 AI spikes will test how well LLMs handle this variability.

---

## Broker Software Platforms (Middleware Layer)

### lluni

- **What:** B2B insurance management ERP (SaaS). Dominant broker software in PT.
- **Scale:** 380+ mediators, 500M EUR+ portfolio under management
- **Founded:** 2013, Braga. CEO: Leandro Fernandes. ~16+ employees. Bootstrapped.
- **Products:**
  - lluni Seg — flagship ERP (portfolio, CRM, compliance, claims, finance, BPM)
  - Portal do Cliente — consumer-facing portal (all policies across insurers)
  - Portal do Agente — for distributed agent/sub-agent networks (launched Nov 2023)
  - WiP (Web Insurance Platform) — multi-insurer quote aggregation and policy issuance
- **Insurer integrations (~13):** Ageas, Allianz, Fidelidade, Generali, Zurich, Lusitania, Caravela, MetLife, MGEN, Ocidental, Una, ASISA, Prevoir, Liberty, Victoria
- **Integration methods:** webservice/REST (real-time) for ~10 insurers, file import fallback for others
- **API:** WiP REST API at `wip.lluni.com/Help` — ASP.NET WebAPI, 100+ endpoints (InsurerMatch, Client, Quote, Document, Auth, etc.). **API access only on Premium plan (833 EUR/mo).** Third-party access unclear — may be locked to lluni Seg subscribers.
- **Pricing:** Tatico 125 EUR/mo (3 users, 3 insurers) | Estrategico 166 EUR/mo (5/5) | Premium 833 EUR/mo (25/20, API, portals, SIBS payments)
- **Tech:** .NET backend, OVH cloud, WordPress marketing site
- **Key contacts:** Miguel Pinheiro (commercial), info@lluni.com

**Flags:**
- API exists but access model unclear for third parties — requires direct contact
- Insurer webservice activation goes through the insurer's commercial structure, not lluni
- Infrastructure dependency risk — 380 mediators on one platform = significant market power
- Could be partner, competitor (if they launch own broker), or acquisition target

### i2S Brokers

- **What:** GIS Mediadores — modular broker management software. Self-described market leader.
- **Scale:** 400+ installations, 40-year installed base
- **Entity:** I2S II - Brokers Software Solutions, S.A., Matosinhos (Porto). **Separate from Cleva/Inetum** (the original i2S was acquired by Inetum in 2019 and rebranded Cleva; i2S Brokers operates independently).
- **Products:**
  - GIS CORE — portfolio, billing, document management, alerts, BI (mandatory base)
  - GIS+ modules — claims, CRM, insurer webservices (Ligacao Seguradoras), banking (SEPA/SIBS), accounting export (Primavera, Sage, Dynamics, SAP)
  - GIS APPs — web portal for multi-role access (manager/agent/employee/client)
- **Insurer integrations:** Ageas, Allianz, Asisa, Caravela, Fidelidade, Liberty, Lusitania, Seguradoras Unidas, Victoria, Zurich
- **API:** No public API or developer portal. Integration requires direct commercial agreement.
- **Pricing:** Quote-based enterprise licensing. No public pricing.
- **Tech:** Web-based, service-oriented architecture, no mobile-native app

**Flags:**
- No self-serve integration path — negotiated commercial engagement only
- Quiet operationally — no press releases or product launches found 2023-2025
- Ligacao Seguradoras module already solves insurer connectivity for GIS users — agent-resolv could inherit these connections indirectly if operating alongside a GIS-enabled mediator

---

## Insurer Direct Integration

### Fidelidade (largest PT insurer, ~35% market share, Fosun group)

- **Broker portal:** `mymediador.fidelidade.pt`, `plataformacomercial.fidelidade.pt`
- **Developer portal:** Public MuleSoft portal at `eu1.anypoint.mulesoft.com/exchange/portals/fidelidade/` — **20+ REST APIs** with OAS/RAML specs:
  - `common-brokers-b2b-xapi` — broker operations
  - `health-claims-b2b-xapi` — claims lifecycle
  - `health-invoices-b2b-xapi` — invoice management
  - `vdmotor-policies-b2b-xapi` — motor policy operations
  - `pt-fid-casual-insurancesubscription-b2b-xapi` — casual insurance
  - `pt-fid-life-partnersubscription-b2b-xapi` — life insurance
  - `pt-vd-motor-proposals-b2b-xapi` — motor proposals
- **Access:** Requires Nr ASF + ClientID + ClientSecret. Provisioned through Fidelidade commercial structure. Not self-serve but well-documented.
- **Auth:** OAuth-based (new API, replacing old system per i2S migration notice Nov 2023)
- **Data:** Policies, receipts, collections, claims, documents

**Verdict: Best API access story in PT market. Priority #1 for pilot integration.**

### Tranquilidade / Generali (formerly Liberty Seguros, Generali group)

- **Broker portal:** `sigms.tranquilidade.pt`
- **API:** OAuth 2.0 webservice confirmed (manual found on Scribd: "Manual WebServices V1_07"). Endpoints: customers, policies, receipts, claims.
- **Access:** No public developer portal. Docs issued only to certified broker software vendors.
- **Network:** ~450 exclusive agents, ~2,100 multi-brand agents, 80 brokers
- **Integration confirmed:** Both lluni and i2S integrate with this insurer

**Verdict: API exists and is in production use. Access likely requires being an approved mediator + going through lluni/i2S or direct negotiation.**

### Ageas Portugal (Ocidental brand, ~2,264 brokers, 1.8M customers)

- **Broker portal:** `ageasonmais.pt` (AgeasOn+)
- **API:** Webservice live, confirmed by lluni and i2S documentation
- **Access:** Mediators request activation via insurer commercial structure
- **Startup program:** Ageas Insure (`ageasinsure.com`) — 15K EUR PoC funding, no equity, ~6-9 months. **Program is closed as of Feb 2026.**

**Verdict: Production webservice, gated access. Ageas Insure program closed — not a viable path.**

### Allianz Portugal

- **Broker portal:** `e-pacallianz.com/ngx-azs-epac/public/home` (ePac)
- **API:** Webservice exists (confirmed via broker integrations, gemese.pt reference). No public developer portal or documentation.
- **Integration:** Both lluni and i2S integrate

**Verdict: Webservice exists but no public-facing API program. Access requires becoming a registered mediator.**

### Generali Portugal (direct brand)

- No separate broker portal found. Treat as part of Generali Tranquilidade ecosystem post-merger.
- Investigate further during Rolando shadow sessions.

---

## Competitors

### MUDEY (Portugal's first fully digital insurance mediator)

- **Founded:** July 2020, Vila Nova de Gaia. Sisters Ana Teixeira (CEO) and Sonia Teixeira (CCO).
- **License:** Mediador de seguros (not corretor). ASF: 420558967.
- **Funding:** 720K EUR seed (2019, angels from PT/BR/US). No subsequent rounds disclosed.
- **Team:** ~11 people (extremely lean for 18+ insurer integrations)
- **Users:** 20,000 active by Feb 2023 (ahead of 3-year target)
- **Products:** Auto, health, dental, life, travel, home, bicycle, work accident, group life. "MUDEY Wallet" for policy management.
- **Insurer partners (18+):** Allianz, Liberty, Lusitania, Europ Assistance, April, Asisa, Real Vida, Tranquilidade, Prevoir, Mapfre, Saude Prime, Innovarisk, Victoria, RNA, Domestic&General, MGEN, Arag, MetLife
- **MudeyPRO (Feb 2024):** White-label platform for small mediators. 100 onboarded by April 2024. Signals D2C is expensive and B2B2C is the play.
- **Tech:** WordPress marketing site + outsourced MERN/React Native app (Classic Informatics). No public APIs.
- **Integration approach:** Bespoke per insurer. No standardized APIs. Growth from 7 to 18+ partners in 3 years without apparent API infrastructure suggests operationally heavy integrations.
- **Reviews:** 4.6/5 on reviews.io (369 reviews). Easy to buy, weak on post-purchase support and claims.

**Key signals:**
- 3+ years to build 18 integrations with 11 people — confirms core technical risk
- MudeyPRO pivot = D2C acquisition is expensive, mediator channel is viable
- Post-purchase/claims experience is the gap (consistent across all review platforms)
- WordPress + outsourced dev = architectural debt opportunity for greenfield build

### Doutor Financas (Protege)

- **Model:** Human-assisted insurance comparison. Customer fills form, specialists negotiate with insurers, present 3-5 proposals. Not automated.
- **License:** Mediador. ASF: 418460081.
- **Insurers:** Allianz, Mapfre, Zurich, MetLife, Generali Tranquilidade, Victoria, Una, Asisa, Prevoir, Real Vida, Saude Prime, Habit
- **Positioning:** Insurance is cross-sell from mortgage/finance lead-gen funnel. Not a tech competitor.

### DECO Proteste (Proteste Seguros)

- **Model:** Consumer rights org with insurance comparison simulators + human brokerage for members.
- **License:** Mediador. ASF: 415421691/3.
- **Positioning:** Trust and independence brand. Publishes annual insurer rankings. Not a tech competitor.
- **Strategic note:** Don't compete on trustworthiness against DECO. Compete on speed and AI personalization.

### ComparaJa.pt (Compare European Group)

- **Model:** Pure lead-gen / affiliate comparison. User compares, clicks through to insurer. No contract intermediation.
- **Funding:** 21M USD raised.
- **Tech:** Vue.js / Nuxt.js. Modern SPA.
- **Positioning:** SEO-dominant for generic insurance comparison queries in PT. Pure top-of-funnel, no post-purchase relationship.
- **Strategic note:** Expect ComparaJa to outrank agent-resolv on generic search. They have no AI advisory or post-purchase engagement.

---

## Industry Context

### API Standardization

- **No standardized PT insurance API protocol exists.** Every integration is bilateral.
- **EIOPA Open Insurance initiative:** Discussion paper published, no mandatory standard yet. Exploratory, not imminent.
- **No ACORD adoption** found in PT market. No industry-wide EDI standard in use.
- **APS** (Associacao Portuguesa de Seguradores) coordinates inter-insurer protocols (e.g., IDS for auto claims) but no API standardization program found.

### Regulatory Digital Requirements

- **DORA** (Digital Operational Resilience Act): Applied Jan 17 2025. Insurers must report ICT third-party arrangements. Portugal implemented via Law No. 73/2025.
- **ASF ICT incident reporting:** Circular No 3/2025.
- **ASF audit trail obligations:** Record retention required for mediation activities.
- **ASF digital finance:** Participant in EU cross-border testing framework — signals receptiveness to digital-native actors.
- **EU AI Act:** High-risk classification for insurance recommendation engines. Aug 2026 deadline. EIOPA has published guidance.

---

## Strategic Conclusions

### Integration strategy

**Phase 1 — Email/portal automation (MVP launch):**
- Automates existing broker workflow: portal logins, email exchanges, PDF downloads
- Works with any insurer from day 1, zero commercial negotiation
- AI parsing layer handles format variability (validated in Session 2 spikes)
- Playwright for portal interaction, LLM for email/PDF parsing
- This is not a "fallback" — it's how every PT broker works today. We just make it 10x faster.

**Phase 2 — Direct API integrations (post-launch, demand-driven):**
- Caravela first (Goncalo direct contact with Luis Cervantes)
- Fidelidade second (best-documented APIs, largest market share)
- Add insurers based on portfolio volume and API availability
- Each direct integration replaces a portal automation with a reliable, real-time connection

**Phase 3 — Evaluate middleware partnerships:**
- lluni WiP API and/or i2S — only if the value exceeds the platform dependency risk
- By this point we'll have our own internal abstraction layer and can evaluate whether middleware adds anything we don't already have

**Long-term (Plaid model):** The internal normalization layer that handles format chaos across insurers becomes the valuable asset. If it proves robust, it could open up to other brokers — similar to how Plaid started as an internal banking tool before becoming the platform. Not a launch goal; let it emerge from operational reality.

### What MUDEY's journey tells us

- Expect 6-12 months per insurer for bespoke API integrations (but email/portal automation bypasses this entirely at launch)
- D2C acquisition in PT is capital-intensive — B2B2C (enabling mediators) may be faster
- Post-purchase (claims, renewals) is the unserved gap across all players
- Small team (sub-15) can manage 18+ integrations but likely with significant operational overhead

### Open questions for Session 2+

1. What does Rolando's current workflow look like with insurer portals? (shadow session priority — directly informs email/portal automation design)
2. How fragmented are insurer quote/policy data formats? (Session 2 AI spikes will test this)
3. Caravela Seguros — what's their tech stack and appetite for direct integration? (Goncalo intro)
4. Is lluni WiP API available to third-party integrators? (requires direct contact)
5. What does Fidelidade's commercial onboarding look like for new ASF-licensed brokers?
