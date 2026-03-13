# Opus Prompt — agent-resolv Investment Memo Audit

**Date:** 2026-03-02
**Requested by:** Gonçalo + Fred
**File to audit:** `projects/agent-resolv/INVESTMENT-MEMO.md`

---

## Context

agent-resolv is a pre-seed, pre-revenue AI-native insurance and credit broker being built in Portugal by Gonçalo Reis (founder) and BuildUp Labs (co-founder, confirmed 2026-02-28). The company is in the thesis/pilot design phase — no product, no license, no revenue yet.

INVESTMENT-MEMO.md is the canonical source of truth for the project. It serves two audiences from one file:

- **Internal (the .md):** Full reasoning trails, gap markers, decision log, internal commentary
- **External (/memo HTML):** Clean investor narrative — all `<!-- INTERNAL ... INTERNAL -->` blocks, `<!-- rationale: ... -->` comments, `<!-- GAP: ... -->` comments, Appendix B (Decision Log), and the Research Index are stripped at render time

The document has just undergone a major overhaul (today, 2026-03-02) that touched almost every section. Changes include:
- Section reordering (Competitive Landscape moved after Market; Regulatory after Product)
- Customer segmentation completely rewritten (4 segments: Individuals, Businesses, Brokers & Platforms, AI Agents)
- GTM section rewritten to reflect honest pre-pilot state (no fabricated phases or timelines)
- License language corrected: we are not "pursuing" licenses — we are evaluating the regulatory path; corretor is a working thesis, not a locked decision
- The Ask reframed: €250K–300K at ~€1.4M–1.7M pre-money, 15% dilution; we are completing the founding team, not just raising
- Risk register restructured by criticality (Critical / High / Medium)
- Key Assumptions restructured into thesis / pilot / financial tiers
- Several equity, relationship, and internal detail items moved to INTERNAL blocks
- A consistency pass (items A–N) that fixed cross-references, redundancies, and contradictions

The document is a living document and will continue to evolve post-Wednesday (2026-03-04 alignment session with BuildUp Labs).

---

## Current Section Structure (post-overhaul)

| # | Section |
|---|---------|
| — | Executive Summary |
| 1 | The Problem |
| 2 | The Solution (2.1 Customer-In Product-Out, 2.2 "A Gente Resolve", 2.3 Service as Software) |
| 3 | Why Now (5 converging factors) |
| 4 | Market (4.1 Sizing, 4.2 Customer Segments, 4.3 ICP — To Be Defined) |
| 5 | Competitive Landscape (5.1 Portugal, 5.2 International analogues, 5.3 Structural Gaps) |
| 6 | Product (6.1 Omni-channel, 6.2 Profiling, 6.3 Persistent Account, 6.4 Agent API, 6.5 Value Dimensions, 6.6 What the Product Promises Each Segment) |
| 7 | Regulatory Path (7.1 Only viable EU model, 7.2 Agente vs Corretor, 7.7 EU AI Act, 7.8 Agent-first fallback, 7.9 FinLab) — 7.3–7.6 + 7.8 INTERNAL |
| 8 | Business Model (8.1 Revenue Architecture, 8.2 Revenue Wedge, 8.3 Projections, 8.4 Unit Economics, 8.5 Cost Structure) |
| 9 | Go-to-Market (9.1 Where We Are — honest/open, 9.2 What We Know) |
| 10 | Team (10.1 Current Team, 10.2 What's Missing) |
| 11 | The Ask (11.1 We're Not Just Raising — We're Hiring, 11.2 Pre-seed Raise, 11.3 Investor Landscape) |
| 12 | Risks & Mitigations (12.1 Risk Register — tiered, 12.2 Key Assumptions — tiered) |
| A | Platform Vision |
| B | Decision Log (INTERNAL) |
| — | Research Index (INTERNAL) |

---

## What We're Asking For

### 1. Audit — Discrepancies, Redundancies, Simplification

Read the full INVESTMENT-MEMO.md. Identify:

- **Remaining inconsistencies** — places where the text contradicts itself, contradicts the "working thesis" framing for the regulatory path, or presents uncertain things as decided
- **Remaining redundancies** — content repeated across sections that could be consolidated or cross-referenced
- **Structural issues** — sections that feel out of place, subsections that would be stronger merged or split, flow problems
- **Simplification opportunities** — content that is more verbose than it needs to be without adding substance
- **INTERNAL/EXTERNAL boundary issues** — anything that appears in the external narrative that should be internal, or anything in INTERNAL that should actually be visible externally

### 2. Strategic Assessment — Is This Document Achieving Its Objectives?

The document has three objectives:

**A. Investor narrative (external /memo):** Should make a sophisticated early-stage investor want to take a meeting. The company is pre-product, pre-license, pre-revenue. The honest position is that this is a strong thesis with an open pilot design phase. The document should make the thesis compelling without overclaiming.

**B. Internal source of truth:** Should capture all strategic decisions, open questions, gaps, and reasoning trails so that anyone (including AI co-founders like Fred) can get full context by reading it.

**C. Wednesday prep:** The document should set the right foundation for the BuildUp Labs alignment session on 2026-03-04, where JP (BuL CTO) and Rui (BuL MD) will review the investment thesis alongside the PRD JP has been developing.

Assess:
- Does the external narrative (stripping all INTERNAL content) hold up as a coherent, honest, compelling pitch?
- Is the "honest but not weak" balance right? (We believe being honest about open questions is a strength, not a weakness — but it can tip into "this isn't ready")
- Does the document communicate why this is urgent and why this team? (The "why us" is the weakest part of most early-stage memos)
- Are there claims that will lose credibility with a sophisticated investor?
- What's missing that a seed investor would expect to see?

### 3. Output Format

Please structure your response as:

```
## Audit Findings
### Remaining Inconsistencies
### Remaining Redundancies  
### Structural Issues
### Simplification Opportunities
### INTERNAL/EXTERNAL Boundary Issues

## Strategic Assessment
### External Narrative Coherence
### Honest-but-Compelling Balance
### Why Us / Why Now Strength
### Credibility Risks
### What's Missing

## Priority Recommendations
(Top 5–10 things to fix, in order of impact)
```

Be direct. Gonçalo and Fred have no patience for diplomatic padding. If something is weak, say it's weak and say why.

---

## Important Notes

- Do NOT modify `INVESTMENT-MEMO.md` or any other existing file. Read only.
- Write your audit output to: `projects/agent-resolv/research/2026-03-02-opus-memo-audit.md`
- Commit and push when done: `git add projects/agent-resolv/research/2026-03-02-opus-memo-audit.md && git commit -m "audit: investment memo — Opus review 2026-03-02" && git push`
- The document uses `git pull` at `/Users/goncaloreis/Projects/reisierx-workspace` — make sure you have the latest version before starting.
- The document is intentionally honest about open questions. Do not flag "ICP is unlocked" or "regulatory path undecided" as weaknesses — these are deliberate. Flag it if the *framing* of those open questions is weak.
- Fred has already done one consistency pass (items A–N) today. Your job is to find what Fred missed and give an independent strategic view.
