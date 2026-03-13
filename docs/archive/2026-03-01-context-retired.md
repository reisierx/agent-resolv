# agent-resolv.com — Project Context

> **Maintained by Fred.** Updated during session when decisions are made or status changes.
> **Budget: 8,000 chars hard ceiling.** Compression trigger: 7,000. Archive: `archive/context/`.
> Last updated: 2026-02-28

---

## 1. Thesis

agent-resolv is the broker agent — an AI-native, licensed insurance and credit broker for Portugal. It sits between the consumer (human or AI agent) and the provider (insurer/bank), ingesting asks, compiling information, routing to providers, and returning independent recommendations. Customer-in, product-out: the customer defines the need, we execute the search. "A gente resolve" — the brand promise and the business model.

---

## 2. Decisions (Locked)

| Decision | Outcome |
|----------|---------|
| Positioning | agent-resolv IS the broker agent. Direct intermediation, not infra/DePIN/routing. |
| Regulatory structure | Licensed entity (ASF + BdP) deploying AI tools. Human directors required. |
| License type | Corretor de seguros (ASF) + credit intermediary (BdP). Only 68 corretores in PT. |
| Primary segment | SME insurance first — highest commissions (10–20%+), most AI-compressible. |
| Revenue wedge | Credit attracts SMEs; insurance expands margin. Doutor Finanças gap: €21M rev, 6,500 policies. |
| Revenue model | Commission (direct) + B2B2C platform fees + data/analytics (three layers). |
| Channel architecture | Omni-channel, unified intake abstraction. Customer chooses channel; broker agent is channel-agnostic. Voice (ElevenLabs/Vapi) = core infrastructure from day one. |
| Customer profiling | First-class intake step. Conversational, channel-adaptive. Partial saves; never re-profiled for returning customers. |
| Customer account | Persistent, channel-agnostic, created on first contact. Holds profile/policies/processes/preferences/opportunities. The compounding asset. |
| Weecover | Technology partner, not competitor. They have embedded tech; we have license + insurer relationships. |
| Provider integration Phase 1 | Habit Analytics API or lluni/i2S middleware. No standardized REST APIs in PT until FiDA (2027–28). |
| Priority insurers | Fidelidade, Tranquilidade/Generali, Allianz, Zurich, Real Vida. Bancassurance-captive = inaccessible. |
| Licensing cost | ~€85–100K year-1 (€50K capital + PII + bond + legal). Responsável técnico = hard constraint. |
| Do NOT build | Full-stack carrier. Burns $1B+. Intermediation only. |
| Do NOT do | D2C only (unsustainable CAC) · Multi-country early (Wefox cautionary tale) · Agent license (cannot promise independence). |

---

## 3. Stakeholders

| Person | Role | Status / Next |
|--------|------|---------------|
| Rolando | Father-in-law. Licensed agent 20+ yr. Responsável técnico candidate + first pilot node. | Off-grid until Mar 20. Engage on return. |
| Luís Cervantes | CEO Caravela Seguros. Insurer-side access. | Key relationship — activate for provider intros. |
| Rui Gouveia + JP | MD + CTO, BuildUp Labs. Accelerator track. | BuL next steps pending — what do they want to do? |
| Lawyer TBD | Portuguese lawyer for licensing review. | Not yet engaged. 🔴 |
| Responsável técnico | 5yr ASF-qualified person for corretor license. Rolando is primary candidate. | Confirm eligibility when Rolando returns. |

---

## 4. Plan

**Current phase:** Post-BuL call. Thesis validated. Entering execution prep.

→ Full execution plan with workstreams, milestones, and dependencies: `PLAN.md`

---

## 5. Status & Open Threads

**As of 2026-02-28:** BuL confirmed Co-Founder mode (70/30, JP as de facto CTO, Rui as network/GTM). Thesis validated. Three research tracks complete. Licensing research complete. Investment memo live at agent-resolv.com/memo. Biggest gaps: sign incubation agreement, engage lawyer, JP alignment on v1 architecture, Zeev intro via Rui.

| Thread | Status | Urgency |
|--------|--------|---------|
| GTM / ICP / acquisition + conversion | Not defined | 🔴 |
| Licensing path — Rolando vs broker in cap table | Open. Sabseg / Doutor Finanças / Deco Proteste as alternatives | 🔴 |
| Lawyer engagement | Not started | 🔴 |
| Funding round — €250K | Soft discussions. Investors named: Zeev/LxL, Caravela, Luís Santos, Rui Bento. | 🟡 |
| BuL next steps | ✅ Co-Founder mode confirmed (2026-02-28). Next: sign incubation agreement (IP clause explicit), Zeev intro via Rui. | 🔴 |
| ASF + BdP licensing timeline | Not yet researched | 🟡 |
| Business model design | Queued | 🟡 |

---

## 6. Licensing

**Target:** Corretor de seguros (ASF) + credit intermediary (BdP). **Bottleneck:** Responsável técnico — board member with 5 years qualifying insurance experience. **Timeline:** 9–16 months from start. **Cost:** ~€85–100K year-1. Full research: `research/2026-02-27-licensing-research.md`.

---

## 7. Product Vision

Customer-in, product-out. The industry starts with supplier products; we start with "what do you need?" Independent (corretor license = contractual trust), transparent, fast, humane. AI agents are first-class clients (A2A-ready from day one).

→ Full vision: `PRODUCT.md` · Segments, channels, account model, agent API.

---

## 8. Pointers

- **Investment memo:** `INVESTMENT-MEMO.md` (canonical narrative, market numbers)
- **Licensing research:** `research/2026-02-27-licensing-research.md`
- **Research index:** `research/INDEX.md`
- **Product vision:** `PRODUCT.md`
