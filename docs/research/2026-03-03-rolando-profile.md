# Rolando Ambrósio — Agent Profile

**Source:** Discovery conversation via Sebastião Fernandes (brother-in-law, AT Kearney), 2026-03-03.
**Status:** First-pass discovery. No direct conversation with Rolando yet.

---

## License & Registration

- **Insurance:** Licensed agent (mediador de seguros), vida + não vida. Registered with ISP (now ASF). Has a mediator number.
- **Real estate:** Has an imobiliária with an alvará. Registered with APEMIP.
- **Credit:** **No credit license.** For mortgages he acts as a Santander promotor — the bank originates the credit, Rolando refers clients. He does not hold a BdP credit intermediary license.

**Implication for agent-resolv:** Rolando can cover the insurance pilot. He cannot cover credit intermediation independently — that path requires a separate BdP license or a different structure.

---

## Distribution Setup

- Works through **Sabseg** (Sabseg Seguros — largest corretora in Portugal, ~€42M revenue, market leader, 38 offices). He is an agent operating under their umbrella — not a direct corretor.
- Works with all banks for mortgage referrals.
- Santander promotor specifically for mortgages.

**Implication:** Pilot mechanism needs to account for Sabseg. Any flow through Rolando goes through Sabseg's infrastructure — they are not a neutral party. Need to assess whether Sabseg would allow or block a tech-layer pilot on top of their agent network.

---

## Commissions

- **Mortgages:** 0.5% of loan value. >€500K loans: 0.75%.
- **Insurance:** Varies by product (auto, vida, etc.). Does not have percentages memorised — would need to sit down with commission tables to give specifics.

**Follow-up needed:** Full commission table by product. Key for unit economics validation.

---

## Daily Process

- **Insurance:** Everything by email. No calls required. Sends email to insurer (via Sabseg), gets response. Turn-around: send in the morning, response in the afternoon.
- **Banking/mortgages:** Same — email when all documentation is gathered. No need to phone anyone.

**Validates:** JP's email-first architecture. This is how real agents operate in Portugal today. Not a workaround — it's the standard workflow.

---

## Invoicing & Business Structure

- **Insurance:** Personal, recibos verdes.
- **Real estate:** Invoices via the imobiliária (company).

---

## Volume

- Approximately **2 processes/month**. Does not track client count — no CRM, no formal system.
- Characterisation: small operator. Not representative of a high-volume broker.

**Implication for pilot:** 2 processes/month is too low for volume validation. We would need to bring customers to him — use Rolando as a licensed node, not as a pipeline source. This is consistent with the B2B2C proxy mechanism we've been designing.

---

## Tools

- Uses the insurance management software provided by Sabseg (adapted from their standard tooling). No independent CRM or dedicated software.

**Implication:** No incumbent tooling lock-in to work around. Clean surface for a pilot layer.

---

## Pain Points

- **Primary:** Getting paid — payment delays are the main frustration, not process complexity.
- **Secondary (implied):** Low volume. The process itself is not the bottleneck — Rolando's scale limits income.

**Implication for product positioning:** The pain point for small agents is not "the process is too complex" — it's volume and payment. Our product pitch to brokers needs to address deal flow, not just workflow efficiency.

---

## RT Candidacy Assessment (Preliminary)

Rolando is an *agente de seguros* operating under Sabseg — not a *corretor* himself. The responsável técnico requirement for a corretor license requires 5 years of qualifying experience as a registered mediator, PDEADS, or board member of a mediator/insurer/reinsurer responsible for distribution.

Rolando has 20+ years as a licensed agent — the experience threshold is almost certainly met. However:
- His qualifying role is as an agente (not a corretor), and it's unclear if this counts under the specific regulatory definition
- His registration is under Sabseg's umbrella — his status as a PDEADS needs legal confirmation
- The "presença em permanência" requirement means he can't be a nominal appointment

**Verdict:** Promising candidate, not confirmed. Requires legal counsel review of his actual ASF registration type and experience classification.

---

## Open Questions (Follow-Up with Rolando)

1. Full commission table by product (needs his Sabseg tables)
2. ASF registration number and exact license category
3. How many active insurance clients approximately?
4. Would he be open to a pilot arrangement? What would he need?
5. His understanding of his role under Sabseg — is he free to test new tooling, or is there exclusivity?

---

## Summary for INVESTMENT-MEMO.md

Rolando operates as a licensed insurance agent under Sabseg (PT's largest corretora). Email-first workflow confirmed. ~2 processes/month — low volume, but he's a real licensed node. No credit license. Pilot feasibility depends on Sabseg's position and legal confirmation of the proxy mechanism. RT candidacy promising (20+ years experience) but requires legal review of his specific registration classification.

**See also:** `research/2026-02-27-licensing-research.md` for RT requirements detail.
