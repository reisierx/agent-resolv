# Opus Prompt — Full System Audit v2
# Created: 2026-03-02
# Purpose: Exhaustive audit of Fred's operational system — closing loops, knowledge management, memory hygiene, SOUL.md adherence

---

## Context

Fred is an AI co-founder (Claude Sonnet via OpenClaw) operating for ReisierX. The workspace at `reisierx/workspace` is his knowledge infrastructure. It has been built incrementally over ~2 weeks, with rules, guides, crons, and memory systems added reactively as problems emerged.

The concern: we've been patching symptoms. Today's example — PLAN.md had completed items still listed as open. Fred didn't catch it. Opus didn't catch it in a previous audit. The ops-scan cron didn't catch it. The housekeeping cron didn't catch it. Four independent systems all missed it because the root cause — no session-close discipline that reconciles PLAN.md against the daily log — was never formalised. A session-close routine was added tonight, but the question is: what else is like this?

This is not a request for reassurance. It's a request for a complete, adversarial audit.

---

## What Opus Has Access To

The full workspace repo. Key files:

- `AGENTS.md` — session startup + close routines, operating rules
- `SOUL.md` — Fred's identity, principles, communication style, how he thinks
- `USER.md` — Gonçalo's profile, preferences, working style
- `MEMORY.md` — long-term memory (compressed, ~14K char budget)
- `PLAN.md` — active operating plan
- `guides/memory.md` — memory write/compress/prune rules
- `guides/plan.md` — PLAN.md structure and maintenance protocol
- `guides/audits.md` — audit process, cron criteria
- `guides/projects.md` — project system rules
- `audits/housekeeping.md` — housekeeping cron prompt
- `audits/security.md` — security cron prompt
- `audits/ops-scan.md` — ops-scan cron prompt
- `memory/2026-03-02.md` — today's daily log (full session record)
- `memory/archive/` — past daily logs
- `skills/agent-resolv/SKILL.md` — project context skill

---

## The Audit

This is a systems audit, not a content audit. Do not review whether the investment memo is well-written or whether the licensing strategy is correct. Review whether the operational system is coherent, complete, and self-enforcing.

### 1. Closing Loop Audit

For each information flow between system components, answer:
- Is there a mechanism that keeps them in sync?
- What breaks if the mechanism fails silently?
- Is the failure detectable without Gonçalo manually catching it?

Components to audit:
- **PLAN.md ↔ Daily log** — does completed work reliably close items in PLAN.md?
- **PLAN.md ↔ MEMORY.md** — do decisions in MEMORY.md match the current state in PLAN.md? Are there contradictions?
- **Daily log ↔ MEMORY.md** — do important lessons/decisions from the log make it to long-term memory? What's the mechanism? Is it reliable?
- **Cron outputs ↔ PLAN.md** — do cron findings ever result in PLAN.md updates? Or do they sit in brief files and get forgotten?
- **Opus commits ↔ Fred's session** — is the startup git check sufficient to catch Opus changes? What happens if Opus commits something that contradicts an in-session decision Fred already made?
- **AGENTS.md rules ↔ actual behaviour** — for each rule in AGENTS.md, is there any enforcement mechanism, or is it purely honour-system?

### 2. Knowledge Management Audit

- Is there a clear, unambiguous answer to: "where does X live?" for every category of information (decisions, lessons, project status, people, rules, preferences, research)?
- Are there overlaps where the same information lives in multiple places and can diverge?
- Is there information that should be in long-term memory (MEMORY.md) but is only in the daily log (ephemeral)?
- Is there information in MEMORY.md that is stale, redundant, or no longer accurate?
- Are the guides (`guides/memory.md`, `guides/plan.md`, etc.) actually complete and current, or do they have gaps that cause inconsistent behaviour?

### 3. Memory System Audit

- Does the daily log format (per `guides/memory.md`) produce logs that are actually useful for session recovery? What information is typically missing?
- Is the MEMORY.md compression process well-defined enough that two different agents would compress it the same way? Or is it ambiguous?
- Is the 14K char budget for MEMORY.md the right size? What's being sacrificed to stay within it?
- Are there categories of information that consistently fall through the cracks (not making it to either daily log or MEMORY.md)?
- `memory_search` is currently non-functional (sqlite-vec failure). MEMORY.md is injected at session start as a workaround. What are the implications of this for memory reliability?

### 4. SOUL.md Adherence Audit

Read SOUL.md carefully. For each principle, assess:
- Is there any mechanism that enforces it, or is it purely aspirational?
- Based on the daily log and session history, are there patterns where Fred visibly fails to apply this principle?
- Are there principles that are internally inconsistent or that conflict with AGENTS.md rules?

Focus especially on:
- **Dissent as obligation** — does Fred actually push back, or does he social-comply?
- **Lead with insight, conclusion first** — or does Fred bury leads in preambles?
- **Co-founder, not executor** — does Fred bring things proactively, or only respond?
- **Via negativa** — when adding rules/files/processes, does Fred first ask what to remove?
- **Implement-then-reason** — does Fred evaluate → state view → get approval → build, or does he build first?

### 5. Cron System Audit

- Are the three crons (ops-scan, security, housekeeping) collectively covering what they should?
- Are there blind spots — things that should be checked regularly but aren't?
- Do cron outputs feed back into the system (PLAN.md, MEMORY.md, AGENTS.md), or do they produce briefs that get read once and forgotten?
- Is the cron frequency right for each task?
- Sub-agent output routing rules — are they being followed? Are there edge cases where they'd be violated?

### 6. Gap Analysis

What operational failure modes exist that no current mechanism catches? Give specific examples of the form:
> "If X happens, the system will silently fail because Y. The first sign of failure would be Z, which Gonçalo would have to catch manually."

Be exhaustive. This is the most valuable section.

---

## Output Format

Save your findings to: `projects/agent-resolv/research/2026-03-02-system-audit-v2.md`

Wait — this is a workspace-wide audit, not agent-resolv specific. Save to: `research/2026-03-02-system-audit-v2.md`

Structure:
1. Executive summary — top 5 most critical findings
2. Closing loop audit — findings per component pair
3. Knowledge management audit
4. Memory system audit
5. SOUL.md adherence audit
6. Cron system audit
7. Gap analysis (failure modes)
8. Recommended fixes — prioritised, specific, implementable

For each finding: be direct. Don't soften. If something is broken, say it's broken. If something is working, say so and move on. The value is in the gaps, not the validation.

Commit the output to the repo when done.
