# Opus Audit — INVESTMENT-MEMO.md

**Date:** 2026-03-02
**Auditor:** Claude Opus (prompted by Gonçalo + Fred)
**Document version audited:** 1.1 (Major overhaul, 2026-03-02)

---

## Audit Findings

### Remaining Inconsistencies

**1. Executive Summary says "Team is complete" — the document disagrees.**

The exec summary states: *"Team is complete: BuildUp Labs confirmed as Co-Founder."* Section 10.2 immediately undermines this: Rolando's roles are unconfirmed, the RT backup doesn't exist, and no functioning prototype exists (INTERNAL 10.3). Section 11.1 opens with "We're Not Just Raising — We're Hiring." You cannot simultaneously say the team is complete and that you're hiring the rest of the founding team. This will be noticed by any investor who reads past page 1.

**Fix:** Change the exec summary to reflect reality. Something like: "Core team locked: BuildUp Labs confirmed as Co-Founder (2026-02-28). Completing the founding bench through the raise."

**2. "Working thesis" vs. definitive language on corretor — the document oscillates.**

Section 7.2 correctly frames corretor as a "working thesis." But:
- Section 2 opens with "agent-resolv is the broker agent" — definitive.
- Section 2.1 says "the corretor license makes independence contractual... that trust is structurally grounded once the license is granted" — this reads as a locked decision, not a thesis.
- Section 6.5 differentiator table says "Independent corretor license (working thesis)" — good.
- Section 6.5 customer view says "Structurally obligated to act in the customer's interest — this is the corretor model we are pursuing" — definitive again.
- Section 7.2 INTERNAL rationale says "Locked: corretor is the right call" — directly contradicts the external "working thesis" framing.

The document needs to pick one voice. If it's a working thesis externally, every section must be consistent with that. Right now, Sections 2 and 6 write as if the license is decided; Section 7 and the exec summary hedge. An investor reading end to end will notice the drift.

**3. Decision Log entry "Rolando = primary RT path" is marked Locked — but the rest of the document says it's unconfirmed.**

Decision Log (2026-02-28): "Rolando (20+ year licensed agent) = primary responsável técnico path" — **Status: Locked.**

Meanwhile:
- Section 10.2: "Rolando's roles unconfirmed"
- Section 12.1: "Both pilot mechanism and RT candidacy depend on confirming his actual license type(s). Neither role is locked."
- Risk Register: Rolando uncertainty rated Critical (High/High)

A decision cannot be simultaneously "Locked" and "unconfirmed." The Decision Log entry should be changed to Status: **Working thesis** or **Conditional**, with a note that confirmation is pending Mar 20.

**4. €1.3B / €1.4B addressable market figure is sloppy.**

Executive Summary: "Direct insurance/credit commissions (€1.3B market)." Section 4.1 says total mediation commissions are €1.3B (insurance only), then "Combined addressable market" is "over €1.4 billion in annual commissions" (insurance + credit). But the credit commission figure is never stated separately. The math is: €1.3B insurance + [unstated] credit = €1.4B? That's only €100M in credit commissions from a €17.9B mortgage market at 57% intermediary share? The figure doesn't add up or isn't explained transparently.

Either show the credit commission calculation explicitly or remove the combined figure. A sophisticated investor will do the math.

**5. Section 8.5 Cost Structure assumes corretor path without acknowledging conditionality.**

The Year 1 cost structure includes €85K–105K for licensing — entirely corretor-specific. If the agent-first fallback is taken (Section 7.8), these costs drop to perhaps €10K–20K. The cost structure doesn't acknowledge this. This matters because the ask (€250K–300K) is sized against the corretor path. If an investor funds the raise and you take the agent path, over €60K of the budget is freed — and the investor should know that.

**6. Section 4.1 says "Broker-addressable non-life premiums: approximately €3–4 billion" but the broker share is stated as 20–25%.**

20–25% of €7.4B non-life is €1.5–1.85B, not €3–4B. The €3–4B figure appears to be "segments where brokers are relevant" (summing the lines), not "broker-addressable." The labeling is misleading. This is exactly the kind of market-sizing inflation that sophisticated investors catch.

---

### Remaining Redundancies

**1. Rolando — four separate treatments, no canonical description.**

Rolando's situation is described in: Section 7.4 (INTERNAL, detailed), Section 10.2 (external, summary), Section 12.1 (Risk Register), and Section 12.2 (Assumptions — twice). Each version is slightly different in emphasis. This should be one canonical description (Section 10, probably) with cross-references from Risk and Regulatory.

**2. "74% of SMEs carry inadequate coverage" / "88% cannot explain" — three times.**

Executive Summary, Section 1, Section 4.2. The stats are strong but triple repetition dulls them. Use them once with full force (Section 1), reference them in the exec summary, drop the repetition from 4.2 or replace with segment-specific data.

**3. "93.9% irregularity rate" — three times.**

Executive Summary, Section 1, Section 6.5 (differentiator table). Same issue. Exec summary + one body section is enough.

**4. MUDEY analysis — spread across three sections.**

Section 5.1 (detailed competitive), Section 7.2 (implied comparison on corretor), Section 12.2 (Key Assumptions: "MUDEY's 5-year failure to upgrade is the strongest evidence"). Each adds something, but the MUDEY story could be told once (Section 5.1) and referenced elsewhere.

**5. Intermediary collapse figures (16,783 → 10,489) — three times.**

Section 1, Section 3 (Why Now), and both use full figures. Section 3 should reference Section 1, not repeat the same stat with the same framing.

---

### Structural Issues

**1. Section 7 numbering is broken in external view.**

External rendering shows: 7.1, 7.2, 7.7, 7.8, 7.9. Sections 7.3–7.6 are INTERNAL and stripped. The visible gaps (jumping from 7.2 to 7.7) signal to any reader that content has been removed. This undermines the entire internal/external convention. Either re-number for external rendering or restructure so INTERNAL subsections don't create visible gaps.

**2. Appendix A — contradictory rendering convention.**

The rendering note says "sections 1–12, Appendix A" are the investor-facing pitch — making Appendix A external. But the first line of Appendix A is an INTERNAL comment saying "This section is internal-only." These directly contradict. Decide: is the platform vision external or not? If external, remove the INTERNAL comment. If internal, wrap the whole section.

Current state: the INTERNAL comment is stripped but the content below it renders — so investors DO see the EU expansion, agent economy, and data layer vision. If that's intentional, fine. But the document contradicts itself about whether it's intentional.

**3. Section 9 (GTM) is too thin for external consumption.**

The honesty is admirable, but the section is 15 lines of body text. For an investor, the question isn't whether you have a GTM plan — it's whether you have the *capability* to build one. Section 9 demonstrates neither. It lists dependencies without showing strategic thinking about how to resolve them. "What We Know" (9.2) is better but still reads as a list of obvious truisms (direct + embedded, providers are a dependency, warm pipeline exists but isn't quantified).

What's missing: a "How we'll figure it out" subsection — the process, the decision criteria, the timeline. Not a fabricated plan, but evidence of GTM thinking capability.

**4. Section 6.6 partly duplicates Section 4.2 and Section 6.5.**

Three places tell the reader "here's what we do for each segment": Section 4.2 (segment descriptions), Section 6.5 (customer view), Section 6.6 (segment promise table). These should be consolidated. The cleanest structure: describe segments once (Section 4.2), describe differentiators once (Section 6.5), and cut 6.6 — or merge 6.6 into 4.2.

**5. Executive Summary is too long (~400 words).**

An exec summary should be 150–200 words and make the reader want to continue. This one repeats body content (the four segments, the commission figure, unit economics). A busy investor reads the exec summary and decides whether to read the rest. Currently it tries to be a miniature version of the whole document instead of a hook.

---

### Simplification Opportunities

**1. Section 2.1 — meta-commentary adds nothing.**

"This is not a UX improvement. It is a fundamentally different business model." The description already makes this clear. Telling the reader how to interpret what they just read is a sign of insecurity in the writing. Same with Section 6.1: the defensive paragraph about omni-channel not being overengineering draws attention to the criticism it's rebutting. Present the architecture confidently.

**2. Section 6.3 (Persistent Customer Account) — verbose, repetitive self-congratulation.**

"This is the compounding asset" ... "This is a durable moat" ... "No comparison site has this. No traditional broker has it." Three mid-paragraph declarations of strategic importance in one subsection. Trust the reader to see the value. One sentence at the end is enough.

**3. Section 3 (Why Now) closing paragraph is unnecessary.**

"These five factors compound — they do not trade off. Miss the window, and the next entry point is post-FiDA (2027–28)..." This restates what the five factors already implied. The urgency is in the factors themselves.

**4. The differentiator table (Section 6.5) includes a "Who else has it" column that says "No one in Portugal" four times.**

The repetition weakens rather than strengthens. Consider: one bold statement about first-mover position, then a table focused on what the differentiators are and why they matter.

---

### INTERNAL/EXTERNAL Boundary Issues

**1. Fred listed as external team member.**

Section 10.1: "Fred (friedr.eth)—AI co-founder operating as research and strategic intelligence system." This is externally visible. Presenting an AI system as a "co-founder" to investors is a choice that will divide your audience. Some will find it interesting; others will find it unserious. At minimum, consider whether this is the right framing for the external narrative. If you keep it, it needs more explanation — one line isn't enough for something this unconventional.

**2. Appendix A boundary (see Structural Issues #2).**

The INTERNAL comment says internal-only but the content renders externally. This needs resolution.

**3. Section 7.2 INTERNAL rationale contradicts external framing.**

The rationale comment says "Locked: corretor is the right call despite timeline cost." The external text says "Working thesis... Final path subject to legal counsel." If anyone reads the .md source (investors doing due diligence on a GitHub-hosted document, for instance), the contradiction is visible. Update the rationale comment to match the external framing.

**4. Decision Log — confirm it actually strips.**

The rendering note says "the Decision Log section" is stripped. But unlike INTERNAL blocks which use marker comments, the Decision Log relies on section-name-based stripping. Confirm the rendering pipeline actually handles this — because the content includes equity split references ("70% ReisierX / 30% BuL") and named investor pipeline that should absolutely not be external.

---

## Strategic Assessment

### External Narrative Coherence

Stripping all INTERNAL content, the external narrative is **70% there**. The thesis is clear, the market data is strong, and the regulatory analysis is sophisticated. But three things break coherence:

1. **Section 7 numbering gaps** (7.1 → 7.2 → 7.7) signal redacted content. This is a trust issue — the document visibly has holes.

2. **The GTM section** is so thin it raises more questions than it answers. An investor's takeaway isn't "they're honest" — it's "they haven't done the work yet on how to sell."

3. **The exec summary overpromises** ("Team is complete") and the body text corrects it (Rolando unconfirmed, hiring through the raise). First impression and detail impression conflict.

The strongest sections externally are: Problem (Section 1), Why Now (Section 3), Competitive Landscape (Section 5), and Regulatory Path (Section 7). These are well-researched, well-argued, and differentiated. The weakest are GTM (Section 9) and Team (Section 10).

### Honest-but-Compelling Balance

The balance is mostly right. The document succeeds at avoiding the two common traps — overclaiming (most pre-seed memos) and underselling (some first-time founders).

**Where honesty is a strength:** ICP unlocked (Section 4.3), regulatory path as working thesis (Section 7), GTM in design (Section 9.1). These signal maturity.

**Where honesty tips into "not ready":**

- Section 9.1 lists four open dependencies without showing how any of them get resolved. Honesty without a resolution path reads as paralysis.
- Section 10.2 ("What's Missing") is entirely about Rolando. The external reader's conclusion: "the entire plan depends on one person whose participation is unconfirmed." That's an accurate reading — and it's devastating.
- The ICP being "To Be Defined" as a **subsection header** (4.3) is too blunt. The content explains the reasoning, but a skimming investor sees the header and moves on.

**Suggested calibration:** Keep the honesty but add resolution mechanisms. "ICP is open because X, Y, Z — and here's how we'll decide by [date]." "Rolando is unconfirmed — here's what happens in each scenario." Turn open questions into decision trees, not just lists of unknowns.

### Why Us / Why Now Strength

**Why Now: 8/10.** The five-factor convergence is the strongest section of the document. Well-argued, time-bounded, and it creates genuine urgency. Minor critique: the FiDA factor (2027–28) is speculative — regulation timelines slip. Acknowledge that.

**Why Us: 4/10.** This is the weakest part of the document, as you predicted.

- **Gonçalo's bio is generic.** "Master's in Management with VC/PE fluency, philosophy-trained analytical rigor." What has he *done*? What specific experience or insight makes him the right founder for an AI insurance broker? The bio reads like a LinkedIn summary, not a founder pitch. A sophisticated investor will ask: "Why is this person the one who sees what the market doesn't?"
- **JP gets one sentence.** "De facto CTO" — no credentials, no track record, no indication of why he can build this.
- **Rui gets one sentence.** "Network + GTM know-how" — no specifics on what network, what GTM experience.
- **BuildUp Labs has no track record cited.** What have they built before? What's their hit rate? Why should an investor trust their co-founder status?
- **Fred as AI co-founder** is interesting but unexplained and potentially confusing (see boundary issues).

The team section reads as a roster, not a pitch. Investors fund people, not theses. This section needs to make the reader believe these specific humans can execute.

### Credibility Risks

**1. LTV:CAC range of 8–50x.**

The range is so wide it's meaningless. 8x and 50x are fundamentally different businesses. The warm-lead CAC of €50 is aspirational and pre-validation. Any investor who sees "50x LTV:CAC" at pre-seed will either laugh or lose trust. Report the direct marketing range (8–17x) and acknowledge the warm-lead number is unvalidated.

**2. "Zero competition" claim with MUDEY in the market.**

Section 5.1 says "No AI-native insurance or credit broker exists in Portugal." MUDEY is literally described in the same section as "Portugal's first digital insurance mediator" with AI-adjacent positioning. Yes, MUDEY is an agente not a corretor, and yes, it's struggling. But "zero competition" is overclaiming when a direct analogue exists. "No independent AI broker" is defensible. "Zero competition" is not.

**3. Revenue projections for a pre-product company.**

The base case (€41K → €290K → €1.2M) looks reasonable on paper but presupposes: a license being granted, a product being built, providers being integrated, and customers being acquired — none of which have started. At pre-seed, sophisticated investors discount revenue projections to roughly zero. The projections aren't harmful, but don't lean on them. They're a proof of financial thinking, not a forecast.

**4. "AI handles 80–90% of broker intermediation workflow."**

Stated as established fact in Section 3 and Section 12.2. This is untested for Portuguese market specifics (language, document formats, insurer systems, regulatory quirks). JP's PRD shows "90–93% email parsing accuracy on limited data" — which is exactly the kind of early signal you should cite, not a blanket claim about 80–90% workflow automation. Cite the evidence you have; don't generalize beyond it.

**5. Unit economics assume Year 2 steady-state at a company that doesn't exist yet.**

€2,500 LTV over 5 years, 85% renewal, 2.5 policies/client — these are perfectly reasonable assumptions for a mature broker. They are fiction at pre-seed. Label them as target economics, not expected economics.

### What's Missing

**1. Founder story / personal motivation.**

Why is Gonçalo building this? What personal experience, obsession, or insight drives the company? Every great pre-seed memo has a "why this founder" narrative. This one has none. The closest is "philosophy-trained analytical rigor" — which is not a story, it's a CV line.

**2. Any form of customer validation.**

Zero evidence that any potential customer wants this. Not five conversations, not a waitlist, not a survey, not a letter of intent. The market data is strong, but market data proves the problem exists — not that this solution is wanted. Even three quotes from SME owners saying "I'd use this" would materially strengthen the pitch.

**3. Technical architecture overview.**

The product section describes capabilities but not architecture. Even a one-paragraph overview (LLM orchestration → structured intake → provider routing → comparison engine → recommendation output, with human oversight at defined checkpoints) would help an investor understand what's being built. JP's PRD exists — reference it or include a high-level extract.

**4. Competitive response scenarios.**

What if MUDEY upgrades to corretor? What if Doutor Finanças adds AI insurance advisory? What if Coverfy (€16M, Barcelona) enters Portugal? The competitive landscape section describes the current state but not how it might evolve. Investors think in scenarios, not snapshots.

**5. Use of funds timeline.**

Section 11.2 shows a use-of-funds table but no timeline. When does each category of spending happen? What are the 6-month and 12-month milestones? "Licensing, pilot build, provider integration, working capital" is a budget, not a plan.

**6. Exit / liquidity narrative.**

Not expected at pre-seed but increasingly common. Even one sentence: "Exits in AI insurance brokerage range from [X] (Optic Insurance Data, $50M+ acquisition) to [Y]" — signal that you've thought about the investor's return path.

---

## Priority Recommendations

Ranked by impact on the document's effectiveness for its three audiences (investors, internal alignment, Wednesday prep).

**1. Fix the team section. (Sections 10.1, Executive Summary)**

This is the single highest-impact change. Investors fund people. The current team section is a roster with one-line descriptions. Write 2–3 sentences per person that answer: what have they done, what specific capability do they bring, and why are they credible for this specific company. Add a Gonçalo founder story — even 3 sentences about why he's building this.

**2. Fix the Section 7 numbering for external rendering.**

The visible gaps (7.1 → 7.2 → 7.7) are an immediate trust break. Re-number or restructure so that external rendering shows consecutive sections. This is a 10-minute fix with outsized impact.

**3. Resolve the "Team is complete" contradiction in the exec summary.**

Change to language that's honest about the current state while conveying momentum. The exec summary is the first thing an investor reads — it cannot contradict the body.

**4. Add a "How we'll figure it out" paragraph to GTM (Section 9).**

Not a fabricated plan — a decision process. What questions need answers, in what order, by when, and who owns each. Turn the open questions into a decision framework. This is also directly useful for Wednesday prep.

**5. Tighten the executive summary to 200 words max.**

Cut the segment detail, the unit economics, and the commission figures. The exec summary should be: problem, solution, why now, team, ask. Make the reader want to continue — don't front-load the entire document.

**6. Normalize the corretor framing across all sections.**

Every external reference to the corretor license should use the same language: "working thesis, subject to legal counsel confirmation." Sections 2 and 6 currently write as if it's a done deal. Pick the framing from Section 7.2 and apply it everywhere.

**7. Add 1–2 customer validation data points.**

Even pre-product: "In conversations with [N] SME owners, [finding]." Or: "Rolando's existing clients have expressed [X]." Or: waitlist numbers from agent-resolv.com. Any evidence that the demand side has been tested, not just modeled.

**8. Add a one-paragraph technical architecture overview to Section 6.**

Reference JP's PRD or describe the high-level system: intake → profiling → routing → comparison → recommendation → human oversight checkpoints. Investors need to know what the technical artifact is.

**9. Resolve the Appendix A boundary contradiction.**

Either make it explicitly external (remove the INTERNAL comment) or wrap the content in INTERNAL blocks. Current state is contradictory.

**10. Narrow the LTV:CAC range and label unit economics as targets.**

Report direct marketing economics (8–17x) as the primary figure. Acknowledge warm-lead economics are unvalidated. Label Section 8.4 as "Target Unit Economics" not "Unit Economics — Base Case."

---

*Audit complete. Document is substantially improved from previous iterations. The thesis is strong. The market data is strong. The regulatory sophistication is genuine and differentiating. The gaps above are fixable — most in a single editing session. The team section and GTM section are the two areas that need the most work before this document is investor-ready.*
