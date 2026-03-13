# Opus Prompt — guides/projects.md Update
*Date: 2026-02-27*

---

## Before anything else

Run `git pull` in `/Users/goncaloreis/Projects/reisierx-workspace` to get the latest from GitHub.

---

## Background

The project system for agent-resolv now has four key documents:

| File | Purpose |
|------|---------|
| `projects/agent-resolv/CONTEXT.md` | Session-start snapshot. 8,000 char budget. Memory-like compression. |
| `projects/agent-resolv/INVESTMENT-MEMO.md` | Canonical investor narrative. Source of truth for agent-resolv.com/memo. |
| `projects/agent-resolv/PRODUCT.md` | Full product vision reference. Loaded on demand when doing product work. |
| `projects/agent-resolv/research/` | All research outputs. Indexed in research/INDEX.md. |

`guides/projects.md` was recently updated with the CONTEXT.md memory mechanism (budget, Reflection Process, decay rates). But it says nothing about when and how INVESTMENT-MEMO.md and PRODUCT.md are updated — that logic lives nowhere. Fred has no written rules for when to touch these files, who updates them, or what the trigger is.

---

## Read these files first

| File | Why |
|------|-----|
| `guides/projects.md` | What's already there — understand the full current content before adding |
| `projects/agent-resolv/CONTEXT.md` | How decisions are tracked |
| `projects/agent-resolv/INVESTMENT-MEMO.md` | What it contains — understand its scope and sections |
| `projects/agent-resolv/PRODUCT.md` | What it contains — understand its scope |
| `AGENTS.md` | Fred's operating rules — external brain pattern, sub-agent routing |
| `guides/memory.md` | The reference model for how update/compression logic is documented |

---

## The Task

Add a section to `guides/projects.md` that documents the update logic for INVESTMENT-MEMO.md and PRODUCT.md. This is operational guidance for Fred — clear enough that Fred knows exactly what to do without asking.

### What to document

**INVESTMENT-MEMO.md — when and how it's updated**

Think through and document:
- What triggers an update? (strategic decision that shifts the narrative, a `[GAP]` section gets filled, market data becomes stale, investor feedback surfaces a hole)
- Who updates it? (Opus — it's narrative, heavy lifting, not line edits by Fred)
- What's the process? (Fred flags the need with a specific description of what changed → Opus prompt → Opus rewrites affected sections → Fred reviews + commits)
- How does Fred flag it? (a clear, lightweight mechanism — perhaps a `[MEMO-UPDATE-NEEDED: reason]` annotation in CONTEXT.md status section, or a TASKS.md item)
- What's the relationship to CONTEXT.md? (a decision lands in CONTEXT.md first; if it has investor narrative implications, INVESTMENT-MEMO.md gets flagged for update)
- What's the relationship to agent-resolv.com/memo? (website is a rendered view of INVESTMENT-MEMO.md — when INVESTMENT-MEMO.md is updated, the website needs rebuilding)
- How often should it be reviewed even if nothing triggered an update? (propose a cadence)

**PRODUCT.md — when and how it's updated**

Think through and document:
- What triggers an update? (a product decision lands in CONTEXT.md decisions table that has architectural implications; a new channel is added; the account model evolves)
- Who updates it? (Fred, mid-session — it's technical, not narrative)
- What's the process? (product decision → CONTEXT.md decisions table first → check if PRODUCT.md needs updating → update in same commit if yes)
- What stays in PRODUCT.md vs CONTEXT.md? (CONTEXT.md carries the decision outcome in one line; PRODUCT.md carries the full design logic, channel stack, segment details, value dimensions)
- What's the relationship to INVESTMENT-MEMO.md? (INVESTMENT-MEMO.md has a product section — when PRODUCT.md changes significantly, flag INVESTMENT-MEMO.md for update too)

**The cascade rule**

Document the full update cascade so Fred always knows the propagation path:

```
New product/strategic decision
  → CONTEXT.md decisions table (always, immediately)
  → PRODUCT.md (if architectural implications — Fred, same session)
  → INVESTMENT-MEMO.md (if narrative implications — flag for Opus, next cycle)
  → agent-resolv.com/memo (rebuild when INVESTMENT-MEMO.md is updated)
```

---

## Output

Update `guides/projects.md` in place. Add the new section cleanly — don't restructure what's already there, just add. Increment the version number if there is one.

Commit message: `docs: update guides/projects.md — INVESTMENT-MEMO.md + PRODUCT.md update logic`
