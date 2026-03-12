# agent-resolv — Briefing Memo
**Prepared for:** Bruno Dinis
**Meeting context:** Gonçalo Reis, Founder / agent-resolv
**Date:** March 2026
**Classification:** Confidential

---

## What This Is

agent-resolv is being built as an AI-native independent broker for Portugal — insurance and credit intermediation. Pre-revenue, pre-license.

This memo captures what we currently believe. The goal of our meeting is to find the holes in it.

---

## What We Think We Know

### 1. Insurance

**€14.3B in gross written premiums (2024). ~€1.3B in annual broker commissions at ~18%.** Non-life growing at ~10% YoY, driven by health, workers' comp, and multi-peril. Life dominated by bancassurance (72.8% of premiums through bank channels — captive distribution, conflicted advice).

The intermediary network is collapsing: 16,783 registered in 2019, 10,489 in 2023. Individual agents average €11,717/year in commissions — below minimum wage. IDD is filtering out undercapitalised operators and the gap isn't self-correcting.

Of the ~10,200 remaining intermediaries, only **68 are corretores** — the only category legally obligated to act in the client's interest, not the insurer's. Everything else is captured distribution.

On economics: we understand headline rates (17–20%) and the ramp for a new entrant (we've modeled Year 1 at 60–80% of market average). What we have limited visibility into is the layer underneath — retrocessions, profit commissions, volume bonuses, preferred-distributor deals. That's where the real distributor P&L lives, and we're working from the outside.

New corretor diversification rules (NR 13/2020-R) require no single insurer to exceed 50% of commissions — mathematically impossible with a new book. We intend to raise this directly with ASF.

One structural friction that compounds at scale: ASF mandatory ongoing training to maintain licences is recurring, deadline-driven, and grows with agent count. For anyone managing a network, it's a material overhead. It's also something we can systematise more naturally than any incumbent.

### 2. Credit

**Mortgage intermediation: 9% of total volume in 2019 → 57% (€10.2B) in 2024.** Consumer credit ~50% intermediated, auto at 83%. A standard mortgage referral generates 0.5–1.0% of loan value (€714–1,428 at the Portuguese average).

The volume growth is real. The compliance picture is not: BdP audits found a 93.9% irregularity rate across credit intermediaries. The market scaled faster than it professionalised.

On structure: the genuinely non-tied credit intermediary (independente) requires exclusive corporate purpose — credit only. That conflicts directly with holding an insurance license. Our working thesis is a **dual-entity structure**: one corretor (ASF), one non-tied intermediary (BdP). Subject to legal confirmation — but we want to pressure-test whether this creates commercial friction in practice that the regulations don't capture.

### 3. Why We're Combining Them

Life events don't separate insurance and credit. A property transaction creates the credit moment; the mortgage creates the insurance moment. The operator already in the room at the trigger event is the natural distribution node for both.

Credit is the acquisition wedge — high-intent customer, proven demand. Insurance is where margin expands and LTV compounds. Doutor Finanças proved the credit channel at scale (€21M revenue, 57% mortgage volume) and left the insurance gap entirely open (€1.8M in insurance premiums). That gap is the target.

### 4. Our Model

Sequoia's Julien Bek recently called insurance brokerage the largest autopilot opportunity in financial services — a $140–200B market where "the broker's value-add is essentially shopping across carriers and filling forms — pure intelligence work." The distinction he draws: a **copilot** sells the tool; an **autopilot** sells the work. We are building an autopilot. We are not building software for brokers. We close the deal.

Standard intermediation — intake, profiling, multi-provider quoting, comparison, recommendation — follows complex rules, but they are rules. That is intelligence work. AI has crossed the threshold to execute it autonomously. What remains human is judgement: regulatory accountability, complex cases, final sign-off. We design explicitly around that boundary. Whether AI actually handles 80%+ of the workflow in Portuguese market conditions is the central assumption our pilot will test.

The corretor + independente path is the only architecture consistent with the positioning. "A gente resolve" — we work for the customer, full stop — is a promise only those two structures can legally make.

**Why the economics compound in a way a manual broker cannot replicate:** Every interaction enriches a persistent customer account that never forgets. A manual broker's knowledge walks out the door when they retire. Ours compounds in software. No capacity constraint: the marginal cost of the 500th client is a fraction of the 50th. The renewal and cross-sell conversations are richer because the full history persists — coverage, life events, prior quotes, preferences. A customer who comes in for a mortgage becomes a multi-line insurance client over time. The LTV compounds without proportional cost increase. No manual operator can execute this systematically. We do it by design.

---

## What We Haven't Solved

**Why would a customer choose us?**

The structural argument is sound — corretor is the only independent position in the market. But legal obligation and customer behaviour are different things. Most customers have never experienced genuinely independent advice and don't know the distinction matters. We don't yet have a clear thesis for how customers discover, trust, and act on it — particularly in the early stages without an established brand or distribution network.

**Why would a distribution operator switch to us?**

A franchise operator in the S&D model pays an upfront franchise fee plus 10% of every transaction for brand, playbook, and pre-negotiated contracts. That arrangement is durable — but the value it delivers compounds slowly, expensively, and inconsistently across operators. The contracts don't get smarter. The operator's economics don't improve with scale.

The right question isn't how to partner with that structure — it's: under what conditions does an operator switch their outsourcing provider? Not because we pitch a better tool, but because we deliver better outcomes at a margin the incumbent arrangement can't match.

One hypothesis: **perpetual revenue share on renewals**. Most referral arrangements today are transactional — one-time fee, done. If a distribution partner earns on every renewal for the lifetime of a referred client, they're not sending a transaction — they're building a passive book. That changes the conversation. We haven't validated whether it's commercially sustainable or regulatorily clean. But it's the kind of structural move that makes switching genuinely attractive rather than marginally better.

---

## Questions for You

**1. Supply or demand failure?**
We believe both markets are supply-constrained — not enough qualified independent advisors, not enough compliance, not enough tooling. The alternative reading: the failure is demand-side. SMEs don't know what they're missing, and even with excellent independent brokerage in front of them, they won't engage or pay for it. Which failure mode is more accurate — for insurance, and separately, for credit?

**2. Will insurers cooperate with a truly independent broker?**
A corretor routes business to whoever best serves the client — which means directing 40–60% of potential customers elsewhere on any given case. Do insurers in Portugal want a genuinely independent AI broker to exist? Under what conditions is that relationship commercially functional?

**3. Why only 68 corretores?**
Barrier-to-entry: responsável técnico scarcity, capital, diversification rules? Or is there something structurally less attractive about the corretor commercial model — rate access, insurer relationship dynamics, distribution leverage — that keeps experienced operators from upgrading? We've assumed the former.

**4. The real first-year P&L for a new entrant.**
We have headline rates and a ramp model. We have limited visibility into retrocessions, profit commissions, volume bonuses, and the informal preferred-distributor dynamics that determine actual margins. Same question on the credit side — bank cooperation and the practical commercial penalties of independence. What does the actual P&L look like in years one and two?

**5. The dual-entity structure — viable in practice?**
Legally possible in principle. What we don't know: does it create commercial friction with insurers, banks, or customers that the regulations don't surface? Is there a practical reason nobody has done this?

**6. What are we missing entirely?**

---

*agent-resolv is a ReisierX venture. This document is confidential and prepared solely for the purpose of this conversation.*
