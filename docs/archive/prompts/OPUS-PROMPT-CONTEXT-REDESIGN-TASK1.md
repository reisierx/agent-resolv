# Opus Prompt — CONTEXT.md Structure Design (Task 1 of 2)
*Date: 2026-02-27*

---

## Before anything else

Run `git pull` in `/Users/goncaloreis/Projects/reisierx-workspace` to get the latest from GitHub.

---

## Background

Fred is an AI co-founder helping build agent-resolv (an AI-native insurance and credit broker for Portugal). `projects/agent-resolv/CONTEXT.md` is Fred's working memory for the project — loaded at the start of every agent-resolv session.

The problem: CONTEXT.md has grown to ~18,700 chars. It's a monolith. The decision has been made to apply the same mechanism as MEMORY.md: hard char budget, weekly compression, archive for displaced content. Before implementing anything, the structure needs to be designed properly. That's this task.

---

## Read these files first

| File | Why |
|------|-----|
| `projects/agent-resolv/CONTEXT.md` | The current monolith — understand every section |
| `MEMORY.md` | The reference model — structure, budget discipline |
| `guides/memory.md` | The compression logic — read carefully, same mechanism will apply to CONTEXT.md |
| `guides/projects.md` | CONTEXT.md's stated design intent |
| `AGENTS.md` | How CONTEXT.md is loaded at session start |
| `skills/agent-resolv/SKILL.md` | What triggers a CONTEXT.md read |
| `projects/agent-resolv/INVESTMENT-MEMO.md` | The canonical investor narrative — so you know what CONTEXT.md must NOT duplicate |
| `projects/agent-resolv/research/2026-02-27-licensing-research.md` | Full licensing research — so you know how much CONTEXT.md can safely compress the licensing section |

---

## The Task

Design the structure of a new CONTEXT.md. This is a design question, not a migration task.

CONTEXT.md is Fred's **session-start snapshot** for agent-resolv. Like MEMORY.md, it should contain distilled essence — not full reasoning, not raw notes, not reference documentation. The full content lives in research files, the investment memo, and the archive.

### Work through each current section

For each section, decide: **does this belong in a session-start snapshot, or is it reference material?**

**Section 1 — Thesis (~800 chars)**
Core positioning. Probably stays. Can it compress to 3–5 sentences?

**Section 2 — Decisions Table (~3,500 chars)**
15 locked decisions. Fred needs these at session start. But some rows are very long. Is there a denser format that preserves the signal without losing any decision? Note: locked decisions don't compress — they archive when superseded. The only way to shrink this section is denser formatting per row.

**Section 3 — Product Vision (~4,000 chars)**
Design rationale, channel stack table, customer account model. Does Fred need this at session start, or only when doing product work? If the latter: compress to a 2-line summary + pointer to a separate `PRODUCT.md` (loaded on demand). Which is right?

**Section 4 — Plan (~600 chars)**
Current phase and milestones. Belongs. But pre-BuL call checklist items are stale — the call happened Feb 27. Needs updating.

**Section 4b — BuL Call Notes (~700 chars)**
Rui's raw framing from the Feb 27 call. This is circumstantial session output — it belongs in `memory/2026-02-27.md`, not CONTEXT.md. The *decisions* from the call belong in the decisions table. Does anything from 4b need to stay in CONTEXT.md, or does it migrate fully to the daily log + decisions table?

**Section 4c — Licensing Summary (~700 chars)**
Summary of agente vs corretor. The full research is in `research/2026-02-27-licensing-research.md`. Does CONTEXT.md need more than: the conclusion (corretor), the bottleneck (responsável técnico), a pointer to the research? Probably 3 lines, not 25.

**Section 5 — Status & Open Threads (~1,500 chars)**
Status dashboard + thread tracker table. Clearly belongs — it's the rolling "where are we?" snapshot. But some threads are resolved and should be cleared. The market numbers block (~400 chars) is in the investment memo — does it need to be here too?

**Section 6 — Key Artifacts (~400 chars)**
A file index. Being replaced by `projects/agent-resolv/research/INDEX.md` (created in Task 2). Should this section become a pointer or be removed?

### What's missing

The current CONTEXT.md has no **stakeholders section**. As the project enters execution, Fred needs at session start: who the key people are, their role, and the current status/next action. This is active project state. Consider whether a Stakeholders section belongs (Rolando, Luís Cervantes, BuL/Rui+JP, lawyer TBD, responsável técnico TBD, potential investors).

### The budget

Propose a hard char budget. MEMORY.md is 14,000 chars. CONTEXT.md should be leaner — it loads on top of an already ~30K startup context. What's the right number — lean enough to matter, large enough to be useful? Justify it.

---

## Output

Produce a proposed CONTEXT.md structure as `projects/agent-resolv/CONTEXT-DRAFT.md`.

Format:

```markdown
# CONTEXT.md — Proposed Structure
*Proposed by Opus, 2026-02-27. Pending Gonçalo approval before implementation.*

## Proposed budget: X,XXX chars hard ceiling

## Section structure

### [Section name]
**Purpose:** one sentence — what question does this section answer?
**Budget:** XXX chars
**Rule:** what goes here vs. elsewhere?
**Content displaced:** where does removed content live instead?

[repeat per section]

## What's changing vs. current structure
[brief summary of the key structural decisions and rationale]

## Draft content
[the actual proposed CONTEXT.md content, respecting the budget]
```

Commit message: `docs: propose CONTEXT.md structure — CONTEXT-DRAFT.md (Task 1 of 2)`

**Stop here. This is all that's needed now. Task 2 is a separate prompt.**
