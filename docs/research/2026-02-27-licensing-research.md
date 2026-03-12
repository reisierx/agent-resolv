# Insurance mediation licensing in Portugal: agente vs. corretor for agent-resolv

**Date:** 2026-02-27
**Research by:** Fred (Opus)
**Prompt:** `research/2026-02-27-opus-prompt-licensing-research.md`

---

**The corretor de seguros license is the right target but comes with a hard bottleneck — a responsável técnico with five years of qualifying experience — that will define agent-resolv's launch timeline.** Every other requirement (€50,000 capital, PII, guarantee bond) is solvable with money; this one requires finding a specific person. The realistic path to a corretor license from scratch is **9–16 months**, with the responsável técnico search as the critical path item. Meanwhile, MUDEY — the closest Portuguese insurtech comparator — chose the faster **agente** route and has not upgraded, which tells you something about the practical trade-offs.

⚠️ **Legislative reference correction:** The prompt cites "Decreto-Lei n.º 170/2019, de 4 de dezembro" as the primary law. This is incorrect for insurance distribution. **The governing statute is Lei n.º 7/2019, de 16 de janeiro**, which approved the **Regime Jurídico da Distribuição de Seguros e de Resseguros (RJDS)** as an annex. DL 170/2019 concerns public contracts. All article references below are to the RJDS (annex to Lei 7/2019) unless otherwise noted. The prior regime (DL 144/2006) was revoked by this law.

---

## Section 1: The legal line between agente and corretor

### Who they serve and why it matters

Portuguese law draws a clean structural distinction between two categories of insurance mediator (Art. 9(1) RJDS):

The **agente de seguros** acts *"por conta de uma ou várias empresas de seguros"* — on behalf of one or several insurers (Art. 16(1)(a) RJDS). The agent must hold a mediation contract with each insurer it represents. Its primary legal relationship is with the insurer, not the client. The agent is a sales channel for the insurer, even though it faces the consumer.

The **corretor de seguros** exercises distribution activity *"de forma independente face às empresas de seguros"* — independently of insurers (Art. 9(1)(b) RJDS). The corretor does not act "por conta de" any insurer. Its privileged contractual relationship is with the **client** (tomador de seguros), typically formalized through a broker-client contract that may include consultancy services, risk analysis, and a power of attorney (*procuração*) granting the broker authority to represent the client before insurers.

The categories are **mutually exclusive** — a person cannot be simultaneously registered as both (Art. 15 RJDS on incompatibilities). Art. 68 RJDS governs category changes.

### Built-in conflicts and structural independence

The agent structure carries inherent conflicts: the agent is paid by the insurer, creating incentives aligned with the insurer's product push rather than the client's best interest. A non-exclusive agent working with multiple insurers faces additional competing-principal conflicts. The RJDS mitigates this through Art. 43 (conflicts of interest rules) and a prohibition on sales-target-based incentive mechanisms that would steer recommendations toward specific products.

The corretor's **independence** means it cannot hold mediation contracts in the agent sense, cannot act on behalf of insurers, and cannot allow its corporate structure to compromise its impartiality (NR 13/2020-R, Art. 8(1)(c)). In practice, corretores are still typically remunerated through insurer commissions embedded in premiums — a practical tension the law addresses through transparency requirements (Art. 31 RJDS) rather than structural prohibition. Portuguese legal scholarship (Luís Poças, Almedina 2021) notes an unresolved question about whether the commission arrangement between corretor and insurer constitutes, in substance, a mediation contract despite not being legally required.

### Duty of care, liability, and suitability

All mediators owe general duties under Art. 24 RJDS (diligence, neutrality, loyalty) and client-specific duties under Art. 30 (objective product information, personalized recommendation, suitability assessment). Art. 103 RJDS makes all mediators civilly liable to policyholders, insured persons, and beneficiaries for acts attributable to them.

The key differences in liability exposure:

For **agents**, the insurer bears primary liability for acts performed within the scope of the agency relationship. The agent's PII requirements are lower, and no guarantee bond is required. For **corretores**, enhanced personal liability follows from their independence and direct client relationship. Art. 35 RJDS imposes additional duties exclusively on corretores — enhanced care toward the client beyond the general mediator duties. The corretor faces higher PII thresholds (€1,300,380/claim vs. lower agent minimums) and must maintain a guarantee bond (seguro de caução) to protect clients against fund mishandling.

### The advice regime under the IDD transposition

The RJDS distinguishes between advice (*aconselhamento*) and mere distribution primarily for **insurance-based investment products (PIBS)**:

**With advice** (Art. 41): the distributor must ensure the recommended PIBS is the most adequate to the client's preferences, objectives, risk tolerance, and loss-bearing capacity — requiring detailed information gathering on knowledge, experience, financial situation, and investment objectives. **Without advice** (Art. 42): the distributor must still assess appropriateness by testing the client's knowledge and experience.

For **non-life/general insurance**, the advice framework is less formalized but Art. 30 requires all distributors to provide a *recomendação personalizada* (personalized recommendation) explaining why a product best meets the client's demands and needs.

**There is no separate legal concept of "aconselhamento independente" (independent advice) in Portuguese insurance law.** Unlike MiFID II (Art. 24(7)), which creates an explicit, regulated independent advice category with specific requirements (sufficient range analysis, prohibition on third-party inducements), the IDD and its Portuguese transposition address independence through **the mediator category itself**. The corretor's structural independence (Art. 9(1)(b)) and specific duties (Art. 35) serve as the functional equivalent. A founder seeking to position as an independent advisor must register as a corretor — there is no additional "independent advice" classification available.

---

## Section 2: What it takes to become a corretor de seguros

### Capital, legal form, and corporate structure

**Minimum share capital: €50,000**, fully paid up at incorporation (NR 13/2020-R, Art. 9(5)). Equity (*capital próprio*) must remain above 50% of share capital at all times (i.e., ≥ €25,000). The company must also maintain ongoing financial ratios: **autonomia financeira ≥ 15%** (equity/assets), **solvabilidade ≥ 20%** (equity/liabilities), **liquidez geral ≥ 100%** (current assets/current liabilities) — all per NR 13/2020-R, Art. 9(3)–(4). These thresholds are higher than for agents (10%/15%/100%).

The RJDS does not restrict legal form — both **Lda and SA** are permitted. However, a ROC (Revisor Oficial de Contas) must be designated for statutory audit regardless of company size thresholds that would normally trigger this requirement. The corporate object (*objeto social*) must be **exclusively financial sector activities** (*"ter como objeto social exclusivo atividades incluídas no setor financeiro"*) — confirmed by APROSE's analysis of Art. 18 RJDS. For an individual (*pessoa singular*), the equivalent restriction is not exercising any profession that could diminish independence.

At least **two members** of the board (*órgão de administração*) must be responsible for distribution activity or PDEADS per public-facing establishment, with one in permanent presence (NR 13/2020-R, Art. 9(1)(h)). Qualified holdings are assessed under RJDS Art. 63, with ASF evaluating whether holders guarantee sound and prudent management.

### The responsável técnico — the hard constraint

For a *pessoa coletiva*, at least one board member responsible for distribution activity must demonstrate **5 years of qualifying experience, consecutive or interpolated, within the 7-year period preceding the registration application** (RJDS Art. 13). This is the person colloquially known as the *responsável técnico*.

Qualifying roles (confirmed under Art. 13):

- Insurance or reinsurance mediator (registered)
- PDEADS (person directly involved in insurance distribution activity)
- Board member of a mediator, insurer, or reinsurer **responsible for distribution activity**

**Academic requirements:** minimum compulsory schooling (12th grade in Portugal) plus completion of an ASF-recognized insurance course. No specific university degree is required, though holders of a degree whose curriculum covers the minimum RJDS content can substitute the training requirement (Art. 13(1)(b)).

Whether one person can serve as responsável técnico for multiple entities is **not explicitly addressed** in the legislation. Incompatibility rules (Art. 15) prohibit corretor board members from sitting on insurer/reinsurer boards or working for ASF, but do not explicitly cover serving multiple corretores. ASF may informally restrict this on independence grounds — recommend verifying directly with the regulator.

### Training requirements: mediators vs. PDEADS

**For the corretor registration itself** (or board members responsible for distribution) — per NR 6/2019-R, Art. 3(1):

| Scope | Hours |
|---|---|
| Vida (Life) | **80h** |
| Não Vida (Non-Life) | **90h** |
| Vida + Não Vida | **120h** |
| Vida excl. PIBS + Não Vida | 100h |

**For PDEADS** (staff directly involved in distribution at the service of the corretor) — per NR 6/2019-R, Art. 3(3):

| Scope | Hours |
|---|---|
| Vida | **55h** |
| Não Vida | **60h** |
| Vida + Não Vida | **80h** |

The prompt correctly cited the mediator/board figures (80/90/120h) — these apply to the registration and the responsável técnico, **not** to PDEADS who have lower thresholds. Training must be delivered by ASF-recognized entities (evaluated by a technical commission per NR 6/2019-R, Arts. 4–6), and candidates must pass a written in-person final exam worth at least **75% of the overall grade** (NR 6/2019-R, Art. 2(1)(g)). Recognized providers include APROSE, SABFORMA, MOFP, Distance Learning Consulting, and GPA Academy.

**CPD requirement:** 15 hours per year minimum for all persons involved in distribution (RJDS Art. 25; NR 6/2019-R, Art. 11).

### Professional indemnity insurance and guarantee bond

**PII minimums** (RJDS Art. 18(1)(c)):
- **€1,300,380 per claim**
- **€1,924,560 per year** (aggregate, regardless of number of claims)
- Must cover the entire EU territory
- Claims-made basis permitted with a minimum **1-year extended reporting period** after policy cessation (NR 13/2020-R, Art. 23(2))
- Must not exclude liability arising from acts of PDEADS

These amounts derive from the IDD (Art. 10(4) of Directive 2016/97), which sets EU minimums of €1,250,618/€1,875,927 reviewed every 5 years against Eurostat HICP. The Portuguese figures (€1,300,380/€1,924,560) represent the most recent indexed update, with further adjustments delegated to ASF via Art. 18(4) RJDS.

**Guarantee bond (seguro de caução)** (NR 13/2020-R, Art. 10):
- **Year 1: €19,510** (statutory minimum)
- **Subsequent years:** the greater of €19,510 or **4% of total funds** handled (premiums received from clients for transfer to insurers, and funds from insurers for transfer to clients/beneficiaries)
- Funds for which the insurer has granted the corretor power to receive in its own name are excluded from the 4% calculation
- The guarantee covers creditor claims from policyholders/beneficiaries and clients for premiums mishandled
- Remains claimable for 1 year after expiry (NR 13/2020-R, Art. 11(1))

### Portfolio diversification (dispersão de carteira)

The thresholds are confirmed (NR 13/2020-R; RJDS Art. 35(b)):

| Concentration | Max % of total commissions |
|---|---|
| 1 insurer | **50%** |
| 2 insurers | **80%** |
| 3 insurers | **90%** |
| 4 insurers | **95%** |

The metric is *remunerações recebidas* (commissions received) per insurer as a percentage of total portfolio remuneration. Assessment is per financial year. **No explicit ramp period or exemption for new entrants** was found in the current legislation. The prior regime (NR 17/2006-R under DL 144/2006) typically allowed grace for start-ups, but the current framework is silent. In practice, ASF may grant tolerance given the mathematical impossibility of diversification in the first months. This must be verified with ASF directly.

### ASF registration process and timeline

**Required documents** (NR 13/2020-R, Arts. 7–8): registration form (Annex II), commercial registry certificate or draft statutes, qualified holdings documentation (Annex IV), ID, qualification certificates, criminal records, and experience proof for all board members and PDEADS, annual accounts (if applicable), a **three-year business plan** (*programa de atividades*) including PDEADS training program, fair treatment principles, client funds procedures, and adequacy of accounting structure, plus proof of PII insurance and guarantee bond commitments.

**Timeline:** ASF must decide within **60 business days** of receiving a complete application or additional requested information (RJDS Art. 19). If ASF does not decide within this period, the registration is deemed **refused** (*silêncio negativo* — negative silence). In practice, expect **3–6 months** from complete submission, potentially longer if documentation gaps trigger requests for additional information.

**Grounds for refusal** (RJDS Arts. 17/19): failure to meet any access condition, inadequate *idoneidade* (fitness and propriety) of board members or shareholders, corporate structure posing risk to independence, inadequate financial structure or business plan, prior regulatory sanctions or disqualifications.

### Ongoing compliance and revocation triggers

Annual accounts must be submitted to ASF via Portal ASF by **15 April** each year, in the format specified in Annex VII of NR 13/2020-R. Changes to registration conditions (board changes, qualified holdings, address) must be notified per RJDS Art. 60. PII and guarantee bond renewals must be maintained continuously.

**License suspension** (Art. 65 RJDS) may be triggered by non-compliance with registration conditions, mediator request, or ASF decision pending investigation. **Cancellation** (Art. 66) follows from loss of registration conditions (e.g., failure to maintain PII, guarantee bond, or financial ratios), failure to commence activity within the specified period, failure to meet CPD requirements, material breach of RJDS obligations, loss of *idoneidade*, or application of cancellation as a sanction for grave administrative offences under RJDS Arts. 112–114.

---

## Section 3: Credit intermediation — a dual-licensing puzzle

### Three categories, one sharp incompatibility

DL 81-C/2017 (transposing MCD and CCD into Portuguese law) establishes three mutually exclusive categories of credit intermediary (Art. 4(3)):

**Intermediário de crédito vinculado** (tied): acts under a binding agreement (*contrato de vinculação*) in the name of and under the full liability of one or more lenders. May be a natural or legal person. Paid by lenders only. The bound lenders must not represent a majority (>50%) of the market.

**Intermediário de crédito a título acessório** (ancillary): a supplier of goods/services that arranges credit to sell those goods/services, under a binding agreement with lender(s). Same rules as tied. Classic example: car dealer arranging finance.

**Intermediário de crédito não vinculado** (untied/independent): a **legal entity only** (*pessoa coletiva*) that acts without a binding agreement with any lender. Enters directly into an intermediation contract with the consumer. Paid only by the consumer. Enhanced duties of impartiality and independence.

### The independence tax: requirements for IC não vinculado

The untied credit intermediary faces the heaviest requirements (Art. 18(2) DL 81-C/2017):

- **Exclusive corporate purpose** (*objeto social exclusivo*): credit intermediation activity only
- **Capital isolation**: no credit institutions, financial companies, payment institutions, e-money institutions, or tied/ancillary intermediaries may hold participations in the entity's capital — and vice versa
- **PII insurance**: for housing credit, **€460,000/claim and €750,000/year** (EU Delegated Reg. 1125/2014); for consumer credit, **€500,000/claim and year** for legal entities (Portaria 385-E/2017)
- No minimum share capital beyond general Portuguese company law
- Training/qualification per Portaria 385-B/2017, with knowledge requirements applying to all board members (or a designated *responsável técnico* for consumer-credit-only intermediaries per Art. 11(6))
- BdP authorization timeline: max **90 days** from complete application, extendable to **180 days** if additional information is requested. Post-authorization registration within 30 days.

### The dual-licensing incompatibility

**A corretor de seguros cannot simultaneously be an intermediário de crédito não vinculado.** Art. 18(2)(a) of DL 81-C/2017 requires exclusive corporate purpose for untied credit intermediaries. The corretor's corporate object must include insurance distribution (required by Lei 7/2019), which directly conflicts with the credit-only exclusivity rule.

**However, a corretor de seguros can potentially also be an intermediário de crédito vinculado (tied).** Tied and ancillary intermediaries face no corporate purpose exclusivity requirement. The entity would need to amend its articles to include credit intermediation, meet all BdP requirements, enter binding agreements with lender(s), and satisfy both ASF and BdP fitness criteria for board members. There is confirmed market precedent: "Decisões e Soluções" appears on both ASF and BdP registries, operating across insurance mediation and tied credit intermediation.

**This means agent-resolv faces a strategic choice:** if it wants to intermediate credit independently (untied), it must use a **separate legal entity** from its corretor de seguros. If it accepts tied status for credit (operating under lenders' binding agreements and liability), it can use a single entity for both activities. The tied model sacrifices credit independence but simplifies corporate structure.

### Conduct rules and BdP oversight

All credit intermediaries must act with diligence, loyalty, and discretion (Art. 36). Non-tied intermediaries face enhanced impartiality duties. Intermediaries may **not** receive or handle any funds related to credit contracts (Art. 37). Disclosure obligations are extensive: in-premises display of identity, category, BdP registration, lender relationships, PII details, and services offered (Art. 39). Pre-contractual written disclosure to consumers is mandatory (Art. 41). Non-tied intermediaries must specify in the intermediation contract a minimum number of proposals to present and the consumer's 3-day withdrawal right (Art. 48).

BdP supervision is conducted by the *Departamento de Supervisão Comportamental*, including authorization processing, on-site/off-site inspections, complaints assessment, and sanctioning. Authorization revocation follows from serious or repeated breaches, or non-activity for 6 months. AML/CFT compliance (Lei 83/2017) and adherence to at least two ADR entities are mandatory.

---

## Section 4: EU AI Act — what applies and when

### Life/health pricing AI is high-risk; product recommendation AI probably is not

Annex III of the EU AI Act (Regulation 2024/1689) classifies the following financial services AI as **high-risk**:

- **5(b):** AI systems to evaluate creditworthiness of natural persons or establish their credit score (explicitly excludes fraud detection)
- **5(c):** AI systems for **risk assessment and pricing in relation to natural persons in the case of life and health insurance**

**Non-life insurance pricing/underwriting** is not explicitly listed. This is a deliberate scope limitation — only life and health insurance AI triggers Annex III, 5(c).

**An AI system that recommends insurance products to consumers** (distribution/brokerage) is also **not directly listed as high-risk**. Product recommendation is conceptually distinct from risk assessment and pricing. However, a critical override exists in Article 6(3): the exceptions that could de-classify Annex III systems **never apply if the AI performs profiling of natural persons.** An AI that analyzes a consumer's financial situation, health status, or risk profile to generate personalized insurance recommendations could constitute profiling, potentially triggering automatic high-risk classification regardless of whether the use case is explicitly listed.

For **credit scoring AI**, the classification is unambiguous: any AI evaluating creditworthiness of natural persons or establishing credit scores is **high-risk** (Annex III, 5(b)). The EBA has confirmed this covers AI used at the point of loan origination. Credit scoring of legal entities (businesses/SMEs as entities) is **not** high-risk. An AI that merely presents available credit products based on stated preferences (amount, term) without itself assessing ability to repay is likely not high-risk — but if it analyzes a consumer's financial data to determine likely qualification, it effectively constitutes creditworthiness evaluation and becomes high-risk.

### The August 2026 deadline and deployer obligations

**All Annex III high-risk obligations take effect on August 2, 2026.** As an AI-native broker deploying (and potentially developing) AI systems, agent-resolv faces obligations under both the **deployer** (Art. 26) and potentially the **provider** (Arts. 9–17) frameworks.

Core deployer obligations (Art. 26):

- **Use per instructions** and implement technical/organizational measures accordingly
- **Assign human oversight** to natural persons with necessary competence, training, and authority (Art. 26(2))
- Ensure **input data quality** — relevant and sufficiently representative (Art. 26(4))
- **Monitor operation**, inform providers of risks, suspend use if risk identified, report serious incidents
- Retain automatically generated **logs for at least 6 months** (Art. 26(6))
- **Inform affected persons** that they are subject to a high-risk AI system (Art. 26(11))
- Complete a **Fundamental Rights Impact Assessment (FRIA)** for credit scoring/creditworthiness AI (Art. 27)
- Cooperate with competent authorities

If agent-resolv develops proprietary AI (making it a provider), additional obligations include a formal risk management system (Art. 9), data governance framework (Art. 10), technical documentation (Art. 11), quality management system (Art. 17), and conformity assessment. For financial services AI, **internal conformity assessment** (Annex VI) is permitted — no notified body is required.

Financial institutions get a partial carve-out: monitoring obligations can be met through existing internal governance processes, and logs can be maintained as part of financial services documentation (Art. 26(5)–(6)).

### IDD and AI Act work together, not against each other

The IDD's technology-neutral obligations mean that advice delivered via automated systems does not reduce the intermediary's legal responsibilities. The AI Act adds **additional** requirements on top of existing IDD obligations: formal risk management processes, mandatory logging, explicit transparency about AI use, and structured human oversight. There is no direct conflict between the regimes — they create a cumulative compliance burden.

EIOPA's August 2025 Opinion on AI Governance (EIOPA-BoS-25-360) addresses AI in insurance that falls outside the high-risk category, establishing principles of fairness, data governance, transparency, and human oversight. The EBA's November 2025 mapping exercise concluded that EU financial services law already addresses many AI Act requirements, and **no new EBA guidelines** are needed at this stage. European Commission guidelines on high-risk classification (due February 2026 per Art. 96) will provide critical practical examples for financial services.

### Human oversight does not mean human approval of every output

Article 14 requires high-risk AI to be **designed for effective human oversight** — not that every output must be individually approved. The law is deliberately flexible: oversight measures must be *"commensurate with the risks, level of autonomy and context of use"* (Art. 14(3)). Both human-in-the-loop (HITL, pre-approval) and human-on-the-loop (HOTL, exception-based monitoring) can satisfy the requirements depending on risk level.

Practical compliance for agent-resolv should follow a **risk-tiered model**: HOTL with exception flagging for standard non-life recommendations, enhanced HOTL or HITL for life/health insurance and credit scoring decisions, with designated qualified human overseers trained specifically on automation bias (Art. 14(4)(b)), override/stop capability built into every system, and interpretability tools that make AI reasoning transparent to overseers.

### Regulatory landscape is immature but forming

**ASF** has not issued specific guidance on AI in insurance distribution but is developing a *"Zona Livre Tecnológica"* (regulatory sandbox) for insurance technology. **BdP** has no dedicated AI/credit intermediation guidance. **Portugal FinLab** (joint BdP-ASF-CMVM initiative) is the primary channel for agent-resolv to engage regulators on AI compliance questions. The Commission guidelines on high-risk classification (due February 2026) will be the next major clarifying event.

---

## Section 5: Strategic flags for agent-resolv

### 1. Realistic licensing timeline: 9–16 months for corretor

The critical path runs through five sequential gates: (1) identify and recruit a responsável técnico with 5 years of qualifying experience within the last 7 years, **1–3 months**; (2) complete ASF-recognized training for any persons not yet certified, **1–3 months** (can overlap with recruitment); (3) incorporate the company with exclusive financial sector object, capitalize at €50,000, secure PII and guarantee bond commitments, **1–2 months**; (4) prepare and submit the application dossier including a three-year business plan, **1 month**; (5) ASF review and processing, **2–6 months**.

The legal maximum for ASF decision is 60 business days from complete application, but documentation gaps extend this. Negative silence applies — no response means refusal. An agente license, by contrast, could potentially be obtained in **2–4 months** given the substantially lower bar.

### 2. The responsável técnico is the single biggest constraint

Only ~69 corretoras de seguros operate in Portugal. The pool of individuals with 5+ years of qualifying insurance distribution experience who are available, willing, and not conflicted is inherently small. Major corretoras (MDS, SABSEG, AON Portugal, Marsh Portugal) employ the most experienced professionals, and recruiting from them will require competitive compensation.

If the responsável técnico leaves, the corretor's registration is at risk. The law requires *"presença em permanência"* (permanent presence) of at least one qualified board member responsible for distribution. No specific statutory replacement timeline was found, but ASF would expect prompt notification and remediation — likely measured in weeks, not months. **Having a single responsável técnico creates a single point of failure.** The mitigation is to ensure at least two board members independently meet the 5-year experience requirement.

The role requires genuine involvement — the *presença em permanência* language precludes a purely nominal appointment. However, it need not be a full-time employment contract; the person must be a board member and genuinely available, but some structuring flexibility exists (executive director vs. full-time employee).

### 3. Total minimum capital outlay: ~€85,000–€100,000+

| Item | Amount |
|---|---|
| Share capital (fully paid) | **€50,000** |
| Guarantee bond (Year 1) | **€19,510** |
| PII annual premium (estimate) | **€10,000–€25,000** |
| ASF fees + legal/incorporation | **€5,000–€10,000** |
| **Total minimum** | **~€85,000–€105,000** |

The PII premium is an estimate — actual cost depends on the insurer, coverage scope, and the broker's risk profile. For a startup with no claims history, expect the higher end. The PII minimum coverage (€1,300,380/claim, €1,924,560/year) is substantial, and not all Portuguese insurers offer this product. The guarantee bond premium is typically 1–3% of the bond amount, so the first-year €19,510 bond might cost **€200–€600** in premium, though the full bond amount must be available as a commitment.

### 4. Portfolio diversification at launch: a mathematical problem

The 50/80/90/95% thresholds are mathematically impossible to meet with a small, new book. If a new corretor places its first 10 policies with 2 insurers, it automatically breaches the 80% threshold. **No explicit ramp period or new-entrant exemption exists in the current legislation.** The prior regime (NR 17/2006-R under DL 144/2006) reportedly allowed transitional tolerance, but the current NR 13/2020-R is silent.

Practical advice: raise this directly with ASF during the pre-application phase. Request written confirmation of transitional tolerance. In parallel, structure the launch strategy to begin placing business with at least **5 insurers from day one** — even if the initial volumes are tiny with some. The metric is commissions received per insurer per financial year, so the first partial year may provide natural tolerance.

### 5. Agent first, corretor later: viable and proven — but has real costs

**This is exactly what MUDEY did.** MUDEY (ASF #420558967) launched in 2020 as an **agente de seguros** — not a corretor as previously assumed. Despite marketing itself in broker-like terms ("independent," works with 15+ insurers), MUDEY has not upgraded to corretor as of the latest available information. This is the strongest possible market signal about the practical trade-offs.

The agent-first strategy eliminates: the €50,000 capital requirement, the guarantee bond, the 5-year experience requirement for the responsável técnico, the portfolio diversification rules, and the mandatory ROC appointment. It gets agent-resolv to market **6–12 months faster** with significantly lower capital outlay.

The costs of this approach:

- **No legal independence:** the agent acts on behalf of insurers, not clients. This undermines the "independent broker" positioning agent-resolv wants.
- **Reduced duty of care:** the agent's legal obligation to the client is structurally weaker than the corretor's Art. 35 duties.
- **Transition friction:** upgrading later requires meeting all corretor requirements, restructuring insurer relationships from "agency" to "independence," rewriting all client disclosures, and amending the corporate object.
- **Reputational risk:** if agent-resolv markets as "independent" while holding an agent license, it risks ASF scrutiny and consumer trust issues. MUDEY navigates this by using the generic term "mediador" rather than explicitly claiming corretor status.

**Strategic recommendation:** if the responsável técnico cannot be secured within 2–3 months, launch as an agent with a clear internal roadmap to upgrade within 18–24 months. Use the agent phase to build the technology, establish insurer relationships, and accumulate the track record that demonstrates independence.

### 6. MUDEY is not what we assumed — and that is instructive

MUDEY holds an **agente de seguros** license (ASF #420558967), not a corretor license. The company was founded by Ana Teixeira (CEO, economist/MBA, extensive international insurtech experience) and Sónia Teixeira (legal/compliance). It raised €720,000 from business angels in 2020, operates with 7–11 employees, and serves ~30,000 users through relationships with 15–20 insurers.

MUDEY's evolution is instructive for agent-resolv: it started B2C (consumer platform), added **B2B2C** in February 2024 with MudeyPRO (white-label technology for small/micro mediadores), and has grown to aggregate 100+ mediadores on its platform. Each MudeyPRO mediador must hold their own ASF registration and PII insurance — MUDEY provides the technology layer.

The key inference: **MUDEY chose speed and capital efficiency over independence.** The agent license let them launch fast with minimal capital. The fact that they have not upgraded to corretor after 5+ years of operation suggests either the benefits of corretor status are not worth the costs for their model, or the responsável técnico requirement is a binding constraint even for an established company.

### 7. The platform-without-a-license model has a narrow window

Under RJDS Art. 2, the insurance distribution regime does **not** apply to the "mere supply" of information about insurance products or potential policyholders, provided the provider *"does not take additional steps to assist in the conclusion of a contract."*

This exclusion is narrow. The definition of *distribuição de seguros* (Art. 4 RJDS) is broad: any activity consisting of providing advice, proposing or practicing preparatory acts for contract conclusion, concluding contracts, or assisting in management/execution of contracts. **A platform that uses algorithms to recommend "best" products, pre-fills forms, or facilitates the purchasing flow is almost certainly conducting mediação de seguros and needs a license.**

If agent-resolv operates as a pure technology provider connecting consumers with existing licensed corretores (B2B2C), the license question turns on who is the mediator in the consumer's eyes:

- **Lower risk:** the licensed corretor is the primary interface, uses agent-resolv's tools internally, and the consumer's contractual relationship is with the corretor. Agent-resolv is a technology vendor.
- **Higher risk:** agent-resolv is the consumer-facing platform, and the corretor functions as a background license holder. ASF would likely view agent-resolv as the de facto distributor.

No specific ASF guidance on technology platforms exists. Abreu Advogados (March 2024) confirms this is a regulatory gap. MUDEY's MudeyPRO model is the closest precedent: MUDEY holds its own license and provides technology to other licensed mediadores, structuring the relationship as a technology service rather than sub-delegation of mediation.

**Bottom line:** if the AI generates personalized recommendations to consumers, agent-resolv needs a license regardless of the corporate structure. A pure B2B technology play (selling tools to licensed brokers) could potentially avoid licensing, but only if agent-resolv is genuinely invisible to the end consumer.

### 8. Cross-border passporting works and is straightforward

A Portuguese corretor can passport its license to any EU member state under the IDD (Arts. 4 and 6 of Directive 2016/97, transposed in Lei 7/2019). The process: notify ASF of intent to operate in the host state (via establishment or freedom to provide services), specifying scope of activities → ASF communicates to the host state authority within **1 month** → the intermediary may begin operating **1 month after** the host authority receives the notification.

Confirmed precedent: Costa Duarte (Portuguese corretor) notified Germany, Belgium, France, and Luxembourg for non-life distribution. C1 Broker (Spanish corretor) operates in Portugal via branch. The mechanism is well-established and routinely used.

For **Spain** specifically: Portuguese corretor notifies ASF → ASF notifies DGSFP (Spain's regulator) → ~2 months total → can operate in Spain under freedom to provide services. Must comply with Spanish host-state conduct rules (*general good provisions*). Spanish language capability is practically essential.

**Strategic note:** the corretor license's passporting value is a significant advantage over the agent license for agent-resolv's medium-term EU ambitions. This alone may justify pursuing corretor from the start rather than accepting the agent-first path.

---

## Conclusion: three decisions that shape everything

**First, the responsável técnico.** Start searching immediately. This person defines the timeline, and every week spent searching is a week the license is delayed. The ideal candidate has 5+ years at a Portuguese corretora, holds current ASF-recognized qualifications, and is willing to join a startup board. Recruit two if possible — redundancy against the single-point-of-failure risk.

**Second, the agent-vs-corretor choice.** The corretor license is the right strategic target for an independent AI broker with EU ambitions, but the 9–16 month timeline may be intolerable. The agent-first path (2–4 months) is proven by MUDEY and viable, but compromises the independence claim that is central to agent-resolv's value proposition. The right answer likely depends on how fast agent-resolv needs to be in market vs. how much the independence positioning matters for early customer acquisition and investor narrative.

**Third, the credit intermediation structure.** An independent (não vinculado) credit intermediary requires a **separate legal entity** from the corretor due to exclusive corporate purpose requirements. A tied intermediary can share the entity but sacrifices credit independence. If agent-resolv wants to be genuinely independent in both insurance and credit, it needs a two-entity corporate structure — one for the corretor license (ASF), one for the non-tied credit intermediary license (BdP). This adds complexity and cost but preserves the independence positioning across both product lines.

The EU AI Act deadline of **August 2, 2026** is the next hard regulatory milestone. Credit scoring AI is definitively high-risk; insurance recommendation AI probably is not (unless it involves profiling or life/health pricing). The practical compliance work — human oversight framework, logging infrastructure, transparency notices, FRIA for credit AI — should begin now. Portugal FinLab is the right channel to engage ASF and BdP on AI compliance questions before the deadline.
