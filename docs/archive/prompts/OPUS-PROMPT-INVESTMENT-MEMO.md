# Opus Prompt — Create INVESTMENT-MEMO.md
*Drop this in the claude.ai project with reisierx/workspace + reisierx/agent-resolv loaded.*
*Date: 2026-02-27*

---

## Context

agent-resolv is a regulated AI broker for insurance and credit in Portugal. Over the past week, research, strategic discussions, and a live accelerator meeting (BuildUp Labs, Feb 27) have produced 4 sources that each contain unique content. None is complete on its own. Your job is to merge them into a single canonical `INVESTMENT-MEMO.md` — the living source of truth for the project narrative.

This document will be read by investors and accelerators. It will be updated continuously as the project evolves. It must be honest about gaps, not paper over them.

---

## Your Sources (read ALL before writing a single word)

### 1. `reisierx/agent-resolv` → `memo.html`
The /memo website. **Highest authority for recently locked decisions** — it was updated under meeting pressure the evening before the BuL call (Feb 26) and contains key strategic choices that may not appear fully in the other sources:
- Omni-channel architecture (channel = product, not distribution)
- Persistent customer account (the compounding asset)
- Customer-in positioning ("what do you need?" vs industry's product-out approach)
- Human/Agent toggle framing
- "A gente resolve" brand promise

Do not ignore content here just because it's in HTML. Extract and use it.

### 2. `reisierx/workspace` → `research/2026-02-25-investment-memo.md`
The Opus-generated investment memo from Feb 25. **Richest content and base structure.** Has market sizing, competitive analysis, business model depth, and narrative that the website compressed or omitted. Use this as the structural backbone.

### 3. `reisierx/workspace` → `research/2026-02-26-memo-vc-assessment.md`
A VC-lens assessment of the Feb 25 memo. Contains the hardest questions the memo doesn't answer well. Where we now have answers (from CONTEXT.md or the website), incorporate them. Where we don't — **flag explicitly as `[GAP — needs decision]`**. Never paper over a gap with vague language.

### 4. `reisierx/workspace` → `projects/agent-resolv/CONTEXT.md`
The operational brain of the project. Contains locked architectural and product decisions. **If anything across the other sources contradicts CONTEXT.md, CONTEXT.md wins.** Pay particular attention to:
- Corporate/legal structure thinking (agente vs corretor path)
- Channel strategy decisions
- A2A/agent economy positioning

### 5. `reisierx/workspace` → `research/2026-02-27-licensing-research.md`
Latest Opus research on PT insurance mediation licensing (Lei 7/2019). **Use this to rewrite the regulatory/legal section entirely.** The earlier memo's regulatory content is less accurate. Key findings to incorporate:
- Agente vs corretor distinction and strategic implications
- Corretor critical path: 9–16 months, bottleneck = responsável técnico
- Minimum capital outlay ~€85K–€105K for corretor
- MUDEY holds agente license (ASF #420558967) — strongest signal on difficulty
- Portfolio diversification rule: needs ASF pre-engagement
- Dual-entity structure required for full credit independence
- EU AI Act: credit scoring = definitively high-risk (August 2026)
- Passporting: corretor passports EU-wide under IDD; agente does not

---

## Output

**File:** `projects/agent-resolv/INVESTMENT-MEMO.md`
**Commit to:** `reisierx/workspace`, branch `main`
**Commit message:** `docs: create canonical INVESTMENT-MEMO.md — merged from 4 sources`

### Format requirements

- Written for a sophisticated investor / accelerator audience (not a first-time founder pitch)
- Lead with insight, not preamble
- Honest about what's locked vs what's still open
- **Source-tag every major section** with an HTML comment: `<!-- source: research/2026-02-25-investment-memo.md -->`  so we can trace what came from where
- Flag all genuine gaps as `[GAP — needs decision]` — do not soften them
- Living document header: include date created, note it is updated continuously

### Suggested structure (adapt if a better structure emerges from the sources)

1. **The Problem** — what's broken about how people buy insurance/credit in Portugal
2. **The Solution** — agent-resolv, customer-in approach, "a gente resolve"
3. **Market** — PT landscape, sizing, why now
4. **Product** — omni-channel architecture, persistent account, channel = product
5. **Business Model** — commissions, fees, embedded B2B2C layer
6. **Competitive Landscape** — why PT is a near-blank canvas, key analogues
7. **Regulatory Path** — agente vs corretor, licensing timeline, EU AI Act
8. **Go-to-Market** — `[GAP — needs decision]` if not clearly answered in sources
9. **Team** — what exists, what's missing (responsável técnico)
10. **Ask** — investment round, use of funds
11. **Risks & Open Questions** — honest list

---

## Critical rules

- Do not hallucinate facts. If something isn't in the sources, either flag it as a gap or omit it.
- Do not merge conflicting information silently — surface the conflict in a comment if needed.
- The memo should reflect the state of the project as of **2026-02-27**, post-BuL call.
- Keep it investor-grade: dense, honest, structured. Not a deck. Not a blog post.
