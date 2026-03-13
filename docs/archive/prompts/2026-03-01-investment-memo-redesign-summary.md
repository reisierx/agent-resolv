# Investment Memo Redesign — Summary

**Date:** 2026-03-01
**Author:** Opus
**Triggered by:** `opus-prompt-investment-memo-redesign.md` (Gonçalo)

---

## 1. Structural Decisions

**Why Now as standalone section.** Pulled timing convergence from old §3.4 (Market) into its own §3. Timing is the strongest VC urgency argument — it was buried. Now it stands alone between Solution and Market, answering the investor's second question after "what is this?"

**Executive Summary added.** The old memo had no executive summary. Cold readers (investors, partners) need a 2-minute pitch before diving into 50K+ of detail. Added as an unnumbered section after the header.

**Decision Log (Appendix B).** New internal-only section. Chronological record of 11 locked decisions with dates, owners, rationale. Serves three purposes: accountability trail for investor diligence, hypothesis record for founder retrospective, strategic anchor for pivot evaluation.

**Section renumbering.** Old: Problem (1) → Solution (2) → Market (3) → Product (4) → Business Model (5) → Competitive (6) → Regulatory (7) → GTM (8) → Team (9) → Ask (10) → Risks (11). New: Problem (1) → Solution (2) → **Why Now (3)** → Market (4) → Product (5) → Business Model (6) → Competitive (7) → Regulatory (8) → GTM (9) → Team (10) → Ask (11) → Risks (12). Cross-references in SKILL.md and guides/projects.md updated accordingly.

---

## 2. Internal/External Convention

**The problem:** One document needs to serve as both a living thesis (with reasoning trails, gaps, decision history) and an investor-facing narrative (clean, compelling, no scaffolding). Two files means drift. One file means exposing internal content.

**The solution:** HTML comment markers that are invisible in rendered markdown and machine-strippable for /memo HTML.

Three marker types:

1. **`<!-- INTERNAL ... INTERNAL -->`** — Block-level internal content. Reasoning trails, decision context, analytical notes. Stripped entirely in /memo rendering.

2. **`<!-- rationale: [reason] | source: [file] | locked: [date] -->`** — Inline reasoning trace, co-located with the claim it supports. Stripped in /memo rendering.

3. **`<!-- GAP: [description] | owner: [who] | urgency: 🔴🟡🟢 -->`** — Open gap marker. Explicit about what's not yet known or decided. Stripped in /memo rendering.

**Rendering rule for /memo HTML:** Strip all three marker types. Strip Appendix B (Decision Log) and Research Index entirely. Everything remaining is the external narrative.

**Why this works:** Fred can apply the convention mechanically. "Is this reasoning or internal context? Wrap it in `<!-- INTERNAL ... INTERNAL -->`. Is this a rationale for a specific claim? Add `<!-- rationale: ... -->` after the paragraph. Is this an open gap? Add `<!-- GAP: ... -->`." No judgment calls about what's internal vs. external — the markers handle it.

---

## 3. Open Questions / Flags

**For Gonçalo:**
- Decision Log (Appendix B) includes 11 entries — verify dates and ownership are accurate. One entry (dual-entity structure) is flagged as "Open" rather than "Locked" because the tied vs. untied credit intermediary path hasn't been decided.
- The memo now uses section numbers shifted by +1 from §3 onward (Why Now inserted). Any external references to "Section 7 — Regulatory" should now point to "Section 8."

**For Fred:**
- `repos/agent-resolv/memo.html` needs to be rebuilt using the new rendering convention. The Inbox note in PLAN.md captures this.
- When making incremental updates, always add both the `<!-- rationale: ... -->` tag AND a Decision Log entry for new locked decisions. This is the traceability contract.

**No structural questions remain that block the memo from being considered final.** All open items are operational (HTML rebuild, Gonçalo review of Decision Log accuracy) rather than structural.

---

## Files Changed

| File | Change |
|------|--------|
| `projects/agent-resolv/INVESTMENT-MEMO.md` | Complete rewrite with new structure, rendering convention, reasoning trails, Decision Log |
| `skills/agent-resolv/SKILL.md` | Updated maintenance rules, section map, convention reference, update mechanics |
| `guides/projects.md` | Updated cascade rules, ownership table, maintaining section, new project template |
| `PLAN.md` | Inbox items added (HTML rebuild + Gonçalo review) |
| `projects/agent-resolv/archive/prompts/2026-03-01-investment-memo-redesign-summary.md` | This file |
