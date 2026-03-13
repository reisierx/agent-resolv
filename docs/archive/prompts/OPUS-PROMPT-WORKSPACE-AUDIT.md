# Opus Prompt — Workspace Architecture Review
*Date: 2026-02-27*

---

## Before anything else

Run `git pull` in `/Users/goncaloreis/Projects/reisierx-workspace` to get the latest from GitHub.

---

## The Question

This is not a housekeeping audit. The question is deeper:

**Given everything below — the tool, the person, the project, the goal — what is the right cognitive architecture for Fred to build a company?**

The current workspace grew organically over 10 days. It has files, folders, processes, and conventions that emerged from session to session. Some of it is good. Some of it is wrong. Some things that should exist don't. The question is not "what's broken" — it's "what should this be, designed from first principles?"

---

## What Fred Is

Read `SOUL.md` and `AGENTS.md` in full.

Fred is an AI co-founder — not an assistant, not a tool. Fred's job is to help build agent-resolv from near-inception: define business model, product, GTM, regulatory path, investor narrative, team, partnerships, and pilot. Fred operates via OpenClaw on a Mac mini, primarily over Telegram, with access to the full workspace repo, Google Workspace, web research, and sub-agents. Fred has persistent memory (MEMORY.md injected into every session), skills (loaded on demand), crons (scheduled background tasks), and an external brain (Opus, via claude.ai + GitHub, for heavy thinking).

The constraint: Fred's context window is 200K tokens. Everything injected at session start (workspace context files) has a direct cost. Lean context = sharper thinking.

---

## What We're Building

Read `projects/agent-resolv/CONTEXT.md` and `projects/agent-resolv/INVESTMENT-MEMO.md` in full.

agent-resolv is an AI-native insurance and credit broker for Portugal. Pre-seed. Pre-revenue. Pre-license. Pre-product. The founding team is one person (Gonçalo) plus Fred. BuildUp Labs is being evaluated as technical co-founder. The four most urgent open items as of today:

1. **GTM / ICP** — most glaring strategic gap
2. **Responsável técnico** — hard bottleneck on the corretor licensing path
3. **Licensing path decision** — agente vs corretor, single vs dual entity, lawyer first
4. **BuL next steps** — what do they actually want to do?

The company needs to go from here to: licensed entity, working product, pilot customers, pre-seed raised. Likely 12–18 months.

---

## What Exists Today

Read the full file tree. Key files:

| File/Folder | Purpose |
|---|---|
| `MEMORY.md` | Long-term memory, injected every session. 14K char budget. |
| `TASKS.md` | Open items, tiered by urgency. Fred's project plan. |
| `AGENTS.md` | Fred's operational instructions — startup sequence, safety, tools |
| `SOUL.md` | Fred's identity and operating principles |
| `USER.md` | About Gonçalo |
| `TOOLS.md` | Environment notes (SSH, paths, credentials) |
| `HEARTBEAT.md` | 30-min polling tasks (currently empty) |
| `guides/` | Memory, tasks, projects, audits — operational guides, loaded on demand |
| `audits/` | Audit guidelines + most recent audit outputs |
| `projects/agent-resolv/` | CONTEXT.md, INVESTMENT-MEMO.md, research/, flows/ |
| `research/` | General workspace research (non-agent-resolv) |
| `memory/` | Daily logs + archive |
| `skills/agent-resolv/` | Skill file — loads CONTEXT.md when project is active |
| `memos/` | Two early thesis memos (pre-agent-resolv focus) |
| `credentials/` | Google OAuth tokens |
| `media/` | Fred's avatar files |

Also read:
- `audits/housekeeping.md`, `audits/security.md`, `audits/landscape-scan.md`
- `guides/audits.md`, `guides/memory.md`, `guides/projects.md`, `guides/tasks.md`
- `skills/agent-resolv/SKILL.md`

---

## The Design Question — What to Answer

### 1. Information architecture
What should Fred know at all times (always in context) vs. load on demand vs. never load? 

The current always-in-context files are: AGENTS.md, SOUL.md, USER.md, TOOLS.md, MEMORY.md, TASKS.md, HEARTBEAT.md, IDENTITY.md, BOOTSTRAP.md. Is this the right set? Is anything missing? Is anything there that shouldn't be?

What is the right structure for a growing company where the number of projects, decisions, people, and open threads will multiply? How does the workspace scale without the context window becoming unusable?

### 2. Project system
agent-resolv has `projects/agent-resolv/` with CONTEXT.md + INVESTMENT-MEMO.md + research/ + flows/. 

Is this the right structure for a company that needs to track: strategic decisions, product architecture, regulatory path, investor narrative, GTM, pilot execution, team/hiring, partnerships, financial model? What's missing? Should CONTEXT.md carry all of this or should it be decomposed? What is the right granularity?

What happens when a second project starts? Is the current project system (described in `guides/projects.md`) fit for purpose?

### 3. Knowledge management
Research compounds. Today there are 10 research files in `projects/agent-resolv/research/`. In 6 months there will be 50. How should research be organised, indexed, and made findable without loading everything into context? 

Is the current pattern (Opus produces → Fred reads → key findings absorbed into MEMORY.md or CONTEXT.md → raw file stays in research/) correct? What's missing?

### 4. Execution tracking
TASKS.md is a flat markdown file with tiers. It works today with ~10 items. It will not work with 50. What is the right task/execution tracking system for a company-building AI operating without external PM tools? How does it handle: multi-week workstreams, dependencies, milestones, sub-agent work, external dependencies (waiting on Gonçalo, waiting on BuL, waiting on lawyer)?

### 5. External brain coordination
Opus (Claude Pro, claude.ai) is the external brain for heavy thinking. The current pattern: Gonçalo triggers Opus manually, Opus commits to workspace, Fred picks it up on session start via git check. 

Is this the right pattern? What breaks at scale? How should Opus outputs be integrated — immediately absorbed, staged for review, or something else? Should there be a cleaner handoff protocol?

### 6. Cadence and rhythm
What recurring processes should Fred run and at what frequency? Currently: daily integrity check (09:00), weekly landscape scan (Mon 06:00 UTC — currently erroring), weekly security + housekeeping (Mon 18:00 UTC — housekeeping erroring). 

Are these the right cadences for a company in this stage? What's missing — e.g. weekly strategic review, monthly investor update drafting, quarterly memory compression, regulatory monitoring, competitor tracking? What should be cron vs. heartbeat vs. manual?

### 7. Cron errors
Two crons are erroring:
- **Landscape Intelligence** (Mon 06:00 UTC, last ran 5 days ago)
- **Housekeeping Audit** (Mon 18:00 UTC, last ran 4 days ago)

Find their prompts/scripts in the workspace, identify the likely cause of failure (broken paths, references to moved files, outdated instructions), and flag specifically what needs fixing.

### 8. Structural clutter
As a byproduct of the above: flag anything that is clearly wrong — duplicate folders, misplaced files, orphaned documents, naming inconsistencies, files that have no clear owner or purpose. But only after answering the design questions above. Cleanup follows design; it doesn't replace it.

---

## Output

Commit a single file to `reisierx/workspace` at `research/2026-02-27-workspace-architecture-review.md`.

This is a design document, not a bug report. Structure it as:

```
# Workspace Architecture Review — 2026-02-27

## Executive Summary
3–5 sentences. What is the core finding? What is the most important thing to change?

## Current State Assessment
What's working. What isn't. What's missing entirely.

## Recommended Architecture
The proposed system — information architecture, project structure, knowledge management, 
execution tracking, external brain coordination, cadence. Be specific and opinionated.
If the current system is right, say so and explain why. If it needs redesign, propose it.

## Migration Path
How to get from current state to proposed state. What Fred can do in one session.
What requires Gonçalo's input. What can wait.

## Cron Error Analysis
Specific findings on the two failing crons. What's broken, where, what to fix.

## Structural Issues (cleanup list)
The janitor work — specific files/folders to move, rename, delete, or create.
```

Be opinionated. If something is wrong, say it's wrong. If the current approach is correct, defend it. The goal is a workspace that makes Fred genuinely better at building agent-resolv — not a tidy filing cabinet.

Commit message: `research: workspace architecture review — 2026-02-27`
