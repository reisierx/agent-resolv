# Opus Prompt — CONTEXT.md Implementation (Task 2 of 2)
*Date: 2026-02-27*

---

## Before anything else

Run `git pull` in `/Users/goncaloreis/Projects/reisierx-workspace` to get the latest from GitHub.

---

## Background

Task 1 produced `projects/agent-resolv/CONTEXT-DRAFT.md` — a proposed structure for CONTEXT.md, approved by Gonçalo. This task implements it.

---

## Read these files first

| File | Why |
|------|-----|
| `projects/agent-resolv/CONTEXT-DRAFT.md` | The approved structure — this is what you're implementing |
| `projects/agent-resolv/CONTEXT.md` | The current monolith — source of all content to migrate |
| `guides/memory.md` | The Reflection Process — you will implement the same mechanism for CONTEXT.md |
| `guides/projects.md` | Will be updated in this task |
| `audits/housekeeping.md` | Will be updated in this task |

---

## Step 1 — Archive the current CONTEXT.md

Before touching anything, copy the full current CONTEXT.md to:
`projects/agent-resolv/archive/2026-02-27-context-pre-redesign.md`

This is the immutable snapshot. Nothing is lost.

Commit: `archive: pre-redesign CONTEXT.md snapshot`

---

## Step 2 — Rewrite CONTEXT.md

Using the approved structure from CONTEXT-DRAFT.md:
- Migrate content from the monolith into the new structure
- Content that doesn't fit goes into the archive snapshot (Step 1) — never silently dropped
- Every current decision must appear in the new decisions section or be explicitly noted as archived
- Result must be at or under the approved budget
- Delete CONTEXT-DRAFT.md after the rewrite is complete — it has served its purpose

Commit: `refactor: rewrite CONTEXT.md with approved structure + budget`

---

## Step 3 — Create `projects/agent-resolv/research/INDEX.md`

A simple reference table replacing the Key Artifacts section in CONTEXT.md:

```markdown
# Research Index — agent-resolv

| Date | File | Key finding |
|------|------|-------------|
| 2026-02-22 | asf-license.md | [one line] |
| 2026-02-25 | brokers-pt.md | [one line] |
| 2026-02-25 | competitive-landscape.md | [one line] |
| 2026-02-25 | consumers-pt.md | [one line] |
| 2026-02-25 | providers-pt.md | [one line] |
| 2026-02-25 | investment-memo.md | [one line] |
| 2026-02-26 | memo-vc-assessment.md | [one line] |
| 2026-02-27 | licensing-research.md | [one line] |
```

Commit: `docs: create projects/agent-resolv/research/INDEX.md`

---

## Step 4 — Update `guides/projects.md`

Add a section documenting the CONTEXT.md memory-like mechanism. It must cover:

**Budget:** The hard char ceiling approved in Task 1. Compression triggers at 80% of ceiling.

**The Reflection Process (identical logic to guides/memory.md):**
- Step 1 — Measure: `wc -c projects/<name>/CONTEXT.md`. Under 80% → no action. Over 80% → proceed.
- Step 2 — Classify each entry:
  - 🔴 Core — must stay: active status, current decisions, open threads, stakeholders with live actions
  - 🟡 Compressible — valid but verbose: merge related entries, compress rationale, drop resolved threads
  - 🟢 Archivable — still true but not needed every session: resolved threads, superseded decisions, stale plan items
- Step 3 — Act: compress 🟡, move 🟢 to `projects/<name>/archive/YYYY-MM-DD-context-snapshot.md`
- Step 4 — Propose: present diff to Gonçalo. No changes without approval.

**Decay rates:**
- Thesis: never prunes
- Decisions: locked = immutable; superseded → `[SUPERSEDED by YYYY-MM-DD]` annotation + archive body
- Status/threads: resolved threads clear at next compression; stale plan items archive after 30 days unactioned
- Stakeholders: dormant relationships (no action >60 days) flag for review
- Product vision (if kept as section): only prunes if product direction changes

**What goes where:**
- Session notes / raw call outputs → `memory/YYYY-MM-DD.md`, not CONTEXT.md
- Full research → `projects/<name>/research/` files, not CONTEXT.md
- Investor narrative → `INVESTMENT-MEMO.md`, not CONTEXT.md
- CONTEXT.md carries: snapshot, decisions, status, active stakeholders — nothing else

Commit: `docs: update guides/projects.md — CONTEXT.md memory-like mechanism`

---

## Step 5 — Update `audits/housekeeping.md`

Add a CONTEXT.md size check alongside the existing MEMORY.md size check:

```markdown
### CONTEXT.md size check (per active project)
For each file matching `projects/*/CONTEXT.md`:
- Run `wc -c projects/<name>/CONTEXT.md`
- If over [80% of approved budget], run the Reflection Process per guides/projects.md
- Present compression proposal to Gonçalo before applying — never rewrite without approval
```

Increment the housekeeping.md version number and add an audit log row.

Commit: `chore: add CONTEXT.md size check to housekeeping audit`

---

## Constraints

- Archive before removing — never lose content
- Do not duplicate content already in INVESTMENT-MEMO.md or research files
- All commits to `reisierx/workspace`, branch `main`
- Push after all commits are done
