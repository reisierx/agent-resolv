# Opus Prompt — Full System Audit
*Date: 2026-02-27*

---

## Before anything else

Run `git pull` in `/Users/goncaloreis/Projects/reisierx-workspace` to get the latest from GitHub.

---

## The Question

The workspace has been significantly restructured today. Before adding anything new, the question is: **does the whole system actually work?**

Not "is it tidy?" Not "are the files in the right folders?" The question is: is this a coherent, operational, MECE system that allows Fred to build a company — and does it hold up under scrutiny?

This is a holistic audit, not a checklist. Think like a systems architect stress-testing a production environment before go-live.

---

## Read Everything First

Read the full workspace. Every file that matters. Don't skim.

**Core system files:**
- `SOUL.md` — who Fred is, operating principles
- `AGENTS.md` — how Fred operates, session startup, routing rules
- `USER.md` — who Gonçalo is
- `TOOLS.md` — environment config
- `MEMORY.md` — long-term memory
- `TASKS.md` — open items
- `IDENTITY.md` — Fred's identity
- `HEARTBEAT.md` — heartbeat config
- `RECOVERY.md` — disaster recovery

**Project system:**
- `projects/agent-resolv/CONTEXT.md` — project working memory
- `projects/agent-resolv/INVESTMENT-MEMO.md` — canonical investor narrative
- `projects/agent-resolv/PRODUCT.md` — product vision reference
- `projects/agent-resolv/research/INDEX.md` — research index

**Guides (the rules):**
- `guides/memory.md` — memory system rules
- `guides/projects.md` — project system rules + CONTEXT.md mechanism
- `guides/tasks.md` — task management rules
- `guides/audits.md` — audit process rules

**Audit infrastructure:**
- `audits/housekeeping.md` — housekeeping checklist
- `audits/security.md` — security checklist
- `audits/landscape-scan.md` — landscape scan guidelines
- Most recent audit outputs in `audits/`

**Skills:**
- `skills/agent-resolv/SKILL.md`

---

## What to Audit

### 1. Loop closure — are all update paths defined and closed?

Every piece of information in the system has a lifecycle: it gets created, updated, compressed, and eventually archived. Trace every major document:

- **MEMORY.md**: When does it get written? Who writes it? When does it compress? Where does displaced content go? Is the compression rule operational (housekeeping cron now fixed)?
- **CONTEXT.md**: When does it get updated? What triggers an update? When does it compress? Where does displaced content go? Is the 8,000 char budget + 7,000 trigger documented and enforced?
- **INVESTMENT-MEMO.md**: When does it get updated? Who updates it (Fred or Opus)? What triggers an update? Is there a defined process, or is this a gap?
- **PRODUCT.md**: Same questions.
- **TASKS.md**: When do items get added? When do they get cleared? What's the housekeeping cadence?
- **Daily logs** (`memory/YYYY-MM-DD.md`): When are they written? What goes in them vs MEMORY.md?
- **Research files**: When do they get created? When do they get archived? Is INDEX.md kept current — by what mechanism?

Flag any loop that isn't closed. A loop is closed when: the trigger is defined, the actor is defined, the destination is defined, and the cadence is defined.

### 2. MECE check — does every piece of information have exactly one home?

The system should be MECE: every piece of information belongs somewhere specific, and nowhere twice.

Test with real examples:
- A new strategic decision is made mid-session. Where does it go? CONTEXT.md decisions table? MEMORY.md decisions section? Both? Is there a rule?
- A key person is introduced (e.g. a new investor). Where does it go? MEMORY.md People? CONTEXT.md Stakeholders? Both? What's the rule for when someone moves from one to the other?
- A product architecture decision is made. CONTEXT.md decisions table, PRODUCT.md, and INVESTMENT-MEMO.md all have product content. Which gets updated, in what order, by whom?
- A research finding lands from Opus. It goes to `projects/agent-resolv/research/`. Does INDEX.md get updated automatically, or is there a gap?
- The BuL call produced raw notes (Section 4b in old CONTEXT.md). These were removed from CONTEXT.md. Where did they go? Are they still accessible? Is the daily log sufficient, or is there a risk of losing call context?

Flag any overlap, ambiguity, or gap in information ownership.

### 3. Coherence — does the system reflect SOUL.md's operating principles?

Fred's operating principles are load-bearing, not decorative. Test the system against them:

- **Via negativa:** Is anything in the system that doesn't earn its place? Any file, rule, or process that adds complexity without clear value?
- **Knowledge infrastructure is existential:** Is every important decision, direction, and lesson actually recorded with date and context? Or are there gaps where information lives only in session memory?
- **Durability over cleverness:** Is the system robust? What breaks if Fred has a bad session, makes a mistake, or loses context? What's the recovery path?
- **Don't trust, verify:** Are there any assumptions baked into the system that haven't been tested? Any rules that sound good but might not work in practice?

### 4. Operational test — simulate a real session

Walk through a realistic session scenario and check whether the system actually works:

**Scenario A — Standard session:**
Gonçalo opens a conversation: "Let's work on the GTM strategy for agent-resolv."
- Fred runs startup sequence (AGENTS.md). Does it produce the right context?
- Fred loads the agent-resolv skill. Does CONTEXT.md give Fred what it needs?
- A decision is made: "ICP is micro-enterprises in regulated professions in Lisbon." Where does this go? How does it propagate?
- Session ends. What does Fred write, where, in what order?

**Scenario B — External brain handoff:**
Opus commits a research file to `projects/agent-resolv/research/`.
- Fred finds it via git check (step 5 in startup). Fred reads it.
- The research changes the GTM thinking. What updates?
- INDEX.md — does it get updated? By whom, when?

**Scenario C — Compression trigger:**
CONTEXT.md hits 7,000 chars.
- What does Fred do? Is the process documented clearly enough to execute without re-reading guides/projects.md?
- What gets compressed vs archived? Is there a risk of losing important information?

Flag any step in these scenarios where the system breaks, is ambiguous, or requires Fred to make a judgment call that should be a rule.

### 5. OpenClaw capabilities — is the system using the platform well?

Fred runs on OpenClaw. Check whether the current setup is aligned with what OpenClaw actually offers:

- **Crons:** Three weekly crons (landscape, security, housekeeping) + daily integrity check. Are these the right tasks for cron? Is there anything that should be a cron but isn't? Anything that is a cron but shouldn't be?
- **Skills:** One skill (agent-resolv). Is the skill doing the right thing? Is there anything else that warrants a skill?
- **Memory injection:** MEMORY.md is injected into every session. Given the 200K context window and the current startup context size, is this still the right architecture? When memory_search becomes functional, what changes?
- **Sub-agents:** The routing rules exist. Are they sufficient? Any gaps?
- **Heartbeat:** Currently empty/inactive. Is there anything that should be a heartbeat task?

### 6. Security posture — is anything exposed?

- Are credentials, tokens, or sensitive paths referenced in any file that loads into context?
- Are there any files that shouldn't be in the git repo?
- Does the current setup follow the security guidelines in `audits/security.md`?

### 7. The INVESTMENT-MEMO.md + PRODUCT.md gap

This was explicitly identified before this audit was commissioned: there are no documented rules for when/how these files get updated, who updates them, and how changes cascade (decision → CONTEXT.md → PRODUCT.md → INVESTMENT-MEMO.md → website).

Document what the rules should be, and whether there are any other undocumented update paths in the system.

---

## Output

Commit a single file: `research/2026-02-27-system-audit.md`

Structure:

```
# System Audit — 2026-02-27

## Verdict
One paragraph. Does the system work? What's the most important finding?

## Closed loops ✅
What's working correctly.

## Open loops ❌
Loops that aren't closed — trigger/actor/destination/cadence undefined.

## MECE violations
Overlaps, ambiguities, or gaps in information ownership.

## Coherence issues
Where the system contradicts SOUL.md's principles.

## Operational test results
What broke or was ambiguous in the scenario walkthroughs.

## OpenClaw alignment
Is the platform being used well? What's missing or wrong?

## Security findings
Anything exposed or misaligned with security guidelines.

## Recommended fixes (prioritised)
Concrete, specific. What Fred can fix in one session. What requires Gonçalo's input. What requires Opus.

## The update cascade rule (INVESTMENT-MEMO + PRODUCT)
The documented rule for how updates propagate through the system.
```

Be brutal. The goal is a system that actually works — not one that looks good on paper.

Commit message: `research: full system audit — 2026-02-27`
