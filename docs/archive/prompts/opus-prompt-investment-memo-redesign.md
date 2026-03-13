# Opus Prompt — Investment Memo Redesign
*Created: 2026-03-01 by Fred*

---

## Context

You are Opus, the external brain for ReisierX. You operate via Claude.ai with the `reisierx/workspace` GitHub repo loaded as project context.

Before doing anything: `git pull` to ensure you have the latest state of the workspace at `/Users/goncaloreis/Projects/reisierx-workspace` (or wherever the repo is cloned on your end).

Fred (the main agent) has been working on agent-resolv. Read the following files in full before proceeding:

1. `projects/agent-resolv/INVESTMENT-MEMO.md` — current state of the memo
2. `PLAN.md` — operational plan, workstreams, stakeholders
3. `skills/agent-resolv/SKILL.md` — maintenance contract for agent-resolv documents
4. `guides/projects.md` — project system rules
5. `projects/agent-resolv/research/INDEX.md` — index of all research conducted

---

## The Problem

`INVESTMENT-MEMO.md` was assembled once from multiple sources (research files + a website memo). It is currently a **static document** — well-structured for a one-time pitch, but not designed to live and grow with the project.

Two problems:

**1. No reasoning trail.** The memo states decisions and facts but doesn't carry *why*. If someone reads "SME-first" or "corretor, not agente", they can't tell whether that's a locked thesis, a working assumption, or a conclusion from specific research. The reasoning that produced each decision is buried in research files that nobody reads after the memo exists.

**2. No update mechanics.** As sessions happen and decisions are made in PLAN.md, there's no clear, reliable path for those decisions to land in the memo with their reasoning intact. The memo drifts stale. We patched this with Fred-maintained incremental updates and Opus rewrites — but the update contract is fuzzy.

**3. Internal/external split is fragile.** `/memo` at agent-resolv.com is the public-facing version. Right now it's a manually maintained HTML file that roughly mirrors the memo. There's no bulletproof convention distinguishing what's internal scaffolding (reasoning, gaps, decision history) vs. what's external-facing narrative.

---

## What We Need

A redesigned `INVESTMENT-MEMO.md` that serves **two functions simultaneously**:

**Function 1 — Living thesis document (internal)**
The canonical record of what agent-resolv is, why, and how we got there. Every major decision has a traceable rationale. New decisions from sessions flow in with their reasoning. Gaps are explicit. The document gets sharper as the project matures — it doesn't need to be rewritten, it needs to be extended.

**Function 2 — Investor/partner narrative (external via /memo)**
The compelling story for investors, partners, and potential team members. Clear structure across: what problem, what solution, why now, why us, what's the plan, what do we need. No internal scaffolding showing. Honest about what's not yet solved.

These two functions live in **one document**. The separation is achieved through a **rendering convention** — not two files.

---

## Your Task

### Part 1 — Structural proposal

Design the new structure for `INVESTMENT-MEMO.md`. Specifically:

- What sections does it have, and in what order?
- How is the internal/external split handled? Propose a concrete, bulletproof convention (e.g., specific markdown block types, comment syntax, section tagging) that Fred can apply mechanically without judgment calls each time
- How does a decision made in a session get into the memo? Define the update mechanic precisely: what Fred writes, where it goes, what triggers an Opus rewrite vs. a Fred incremental update
- How is the reasoning trail captured without bloating the narrative? The goal is traceability, not a research dump

### Part 2 — Rewrite INVESTMENT-MEMO.md

Using the new structure, rewrite `projects/agent-resolv/INVESTMENT-MEMO.md` in full. Rules:

- Preserve all material facts, market data, and decisions from the current memo — nothing is lost
- Incorporate the confirmed BuL Co-Founder terms (70/30, JP as CTO, Rui as network/GTM, IP in entity — confirmed 2026-02-28)
- Add reasoning traces to major decisions — source from the research INDEX and research files as needed
- Mark genuine gaps explicitly (what we don't yet know or haven't decided)
- Apply your proposed internal/external convention throughout
- Structure must serve both functions: cold read gives full thesis; investor read gives compelling narrative
- Ceiling: 60,000 chars soft. Be ruthless about what earns its place.

### Part 3 — Update the mechanics

Update these files to reflect the new structure and update contract:

- `skills/agent-resolv/SKILL.md` — update maintenance rules to match the new document structure and update mechanic
- `guides/projects.md` — update cascade rules and ownership table to match
- Add a brief note to `PLAN.md` Inbox if any open structural questions need Gonçalo's input before the memo can be fully finalised

### Part 4 — Commit

Commit all changes with message:
`"restructure: investment-memo redesign — living thesis + rendering convention (Opus 2026-03-01)"`

Then push.

---

## Constraints

- Do not change `PLAN.md` workstream content — only the Inbox if needed
- Do not touch `MEMORY.md`
- Do not create new files outside `projects/agent-resolv/` and `skills/` and `guides/` unless a new structure genuinely requires it — and if so, explain why
- The `/memo` HTML at `repos/agent-resolv/memo.html` is **not** your responsibility in this task — Fred will rebuild it after the memo is finalised. Flag what needs to change in the HTML if relevant.
- If you find contradictions or stale content in the current memo, resolve them using the most recent source (PLAN.md > daily logs > research files)

---

## Output

Commit and push everything. Then write a brief summary (1 page max) of:
1. The structural decisions you made and why
2. The internal/external convention you chose and how it works
3. Any open questions or flags for Gonçalo/Fred

Save the summary to: `projects/agent-resolv/archive/prompts/2026-03-01-investment-memo-redesign-summary.md`
