# Opus Research Prompt: Insurance Mediation Licensing in Portugal — Agente vs. Corretor

**Date:** 2026-02-27
**Requested by:** Gonçalo / Fred
**Output destination:** `research/2026-02-27-licensing-research.md`

---

**Research Task: Insurance Mediation Licensing in Portugal — Agente vs. Corretor**

**Context:** agent-resolv is a planned AI-native insurance and credit intermediary in Portugal, targeting individuals, SMEs, and eventually B2B2C embedded distribution. The company intends to operate as a fully independent broker (corretor de seguros), not an agent. This research will inform the licensing strategy, timeline, and product design constraints.

**Primary legal sources to anchor all findings:**
- Decreto-Lei n.º 170/2019, de 4 de dezembro (transposes IDD — Insurance Distribution Directive 2016/97/EU)
- Norma Regulamentar ASF n.º 13/2020-R, de 30 de dezembro (organisational and financial requirements for mediators)
- Norma Regulamentar ASF n.º 6/2019-R, de 3 de setembro (training and qualification requirements)
- Decreto-Lei n.º 144/2006, de 31 de julho (prior regime — relevant for historical context and transitional provisions)
- Any ASF circulares or orientações issued since 2020 relevant to corretor licensing

---

## Section 1: Legal distinction — Agente vs. Corretor

Map the exact legal definitions of each category under Portuguese law:
- What is an *agente de seguros*? Who do they legally represent? What conflicts of interest are built into the structure?
- What is a *corretor de seguros*? What is the legal duty of independence? Who does the corretor legally serve?
- What are the legal implications of acting as an agent vs. a corretor when giving advice to a client? (duty of care, liability, suitability obligations)
- What does "independência face às seguradoras" mean in practice — what is the corretor legally prohibited from doing that an agent can do, and vice versa?
- Under the IDD, what additional obligations apply to distributors giving "advice" (aconselhamento) vs. mere distribution? How does this interact with the corretor category?
- Is there a legal concept of "independent advice" (aconselhamento independente) distinct from standard corretor activity? If so, what does it require?

---

## Section 2: Licensing requirements — Corretor de Seguros

Detail the full requirements to obtain and maintain a corretor de seguros license from ASF, distinguishing between *pessoa singular* (individual) and *pessoa coletiva* (company):

**For a new company (pessoa coletiva):**
- Minimum share capital (€50,000 — confirm and detail)
- Legal form requirements (Lda, SA — any restrictions?)
- Object clause requirements (exclusive financial sector activity — confirm scope)
- Corporate governance requirements (who must be a "responsável técnico"?)
- Responsável técnico qualification requirements:
  - Minimum 5 years experience (confirm the exact rule — consecutive vs. interpolated, in which roles, over what reference period)
  - What counts as qualifying experience? (mediador, PDEADS, board member of insurer/mediator, etc.)
  - Academic requirements?
- PDEADS (Pessoas Diretamente Envolvidas na Atividade de Distribuição de Seguros) — who qualifies, what training is required, minimum hours (80h Vida / 90h Não Vida / 120h both — confirm), and who provides recognised training
- Professional indemnity insurance: minimum €1,300,380 per claim / €1,924,560 per year (confirm current figures — they are indexed)
- Guarantee bond (seguro de caução): minimum €19,510 first year, then 4% of annual premiums received (confirm)
- Portfolio diversification rules: max 50% from one insurer, 80% from two, 90% from three, 95% from four (confirm these thresholds and whether they apply from day one or after a ramp period)
- ASF registration process: what documents are required, what is the typical timeline, is there a formal review period, can it be refused and on what grounds?

**Ongoing compliance:**
- Annual accounts disclosure obligations (NR 13/2020-R)
- Reporting obligations to ASF
- Continuing professional development (CPD) requirements for responsável técnico and PDEADS
- What triggers a license suspension or revocation?

---

## Section 3: Credit intermediation (intermediário de crédito)

agent-resolv also intends to intermediate credit products (mortgages, personal loans, consumer credit). This is regulated separately by BdP (Banco de Portugal):
- What are the categories of credit intermediary under DL 81-C/2017 (transposing MCD and CCD)?
- What is the distinction between *intermediário vinculado* and *intermediário não vinculado* (independent)?
- What are the licensing requirements for an independent credit intermediary?
- Can the same legal entity hold both an ASF corretor license and a BdP credit intermediary license? What are the compatibility requirements?
- What are the ongoing compliance obligations (conduct rules, disclosure requirements, BdP oversight)?

---

## Section 4: EU AI Act implications

agent-resolv will use AI agents to analyse, recommend, and intermediate insurance and credit products:
- Which AI Act risk classification applies to AI systems used for credit scoring or insurance risk assessment? (Annex III — high risk?)
- Does an AI system that recommends insurance products to consumers fall under the high-risk classification?
- What obligations apply to high-risk AI systems under the EU AI Act (August 2026 deadline for high-risk provisions)?
- How do the AI Act obligations interact with the IDD suitability and advice obligations?
- Is there any ASF or BdP guidance on AI use in insurance/credit distribution?
- What does "human oversight" mean in a practical compliance sense for an AI-driven broker?

---

## Section 5: Practical implications and strategic flags for agent-resolv

Based on the above, synthesise:

1. **Licensing path:** What is the realistic timeline to obtain a corretor de seguros license from scratch? What is the critical path item (likely: finding a qualified responsável técnico with 5 years experience)?

2. **Responsável técnico risk:** This is likely the single biggest constraint. Who qualifies? Is there a market for hiring qualified responsáveis técnicos? What happens if the responsável técnico leaves?

3. **Capital and bond requirements:** Total minimum capital outlay to launch (share capital + PII + guarantee bond). Estimate the first-year cost of the PII premium (not just the minimum coverage).

4. **Portfolio diversification rule:** How does the 50/80/90/95% rule work in practice at launch when the book is small? Is there a ramp period or exemption for new entrants?

5. **Agent first, corretor later?** Is it legally viable and strategically sensible to launch as an agente de seguros (lower bar) and upgrade to corretor later? What would the upgrade path look like and what would it cost in terms of product/legal repositioning?

6. **MUDEY comparison:** MUDEY holds a corretor license. What can we infer about how they obtained it and what their compliance posture looks like?

7. **White-label / platform model:** If agent-resolv operates as the platform and partners with existing corretores (B2B2C), does agent-resolv itself need a license? Or can it operate as a technology provider without a mediação license? What are the legal boundaries?

8. **Cross-border passporting:** Can a PT corretor license be passported to operate in Spain or other EU markets under the IDD? What is the notification process?

---

**Output format:**
- Structured by section, with subsections matching the above
- Each factual claim anchored to the specific legal source (article number, NR number, or ASF document)
- Flag clearly where the law is ambiguous or where practice diverges from the letter of the law
- Strategic flags in Section 5 should be direct and opinionated — this is for a founder making go/no-go decisions, not a legal disclaimer document
- Approximate length: comprehensive but tight — prioritise precision over exhaustiveness
- Save output to: `research/2026-02-27-licensing-research.md`
