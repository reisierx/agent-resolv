# agent-resolv.com — Architecture Thinking

> Persistent artifact. Iterate here. Last updated: 2026-02-22.
> Companion to `agent-resolv-flows.md`.

---

## The Question

Can we use OpenClaw as the implementation framework for agent-resolv.com?

---

## What OpenClaw Actually Is

Based on the docs and running it:

- **Single-tenant personal assistant framework.** One Gateway per host, designed around one (or a small number of) users.
- **Multi-agent = multi-persona, not multi-customer.** The multi-agent feature routes different channels/accounts to different agents. It's designed for one person across multiple channels (or a small team) — not for serving hundreds of customers from one instance.
- **VPS-deployable.** Can run in the cloud (Railway, Fly, Hetzner, GCP). State + workspace lives on the server.
- **Nodes.** Physical/logical devices connecting to a Gateway with declared capabilities. Closest thing to a "broker node" in OpenClaw vocabulary.
- **MCP/Skills.** External tools connect via mcporter. This is how broker CRMs, insurance platforms, product catalogs would plug in.
- **Sub-agents.** `sessions_spawn` creates isolated workers. Could be used for routing logic.
- **Open source.** Can be forked, extended, productised.

---

## The Honest Assessment

### Is it possible? Yes.
The agent loop (memory + tools + sessions + channels) is exactly the right pattern for both the customer agent and the broker agent. Fred is proof of concept.

### Is it feasible? Partially — depends on the layer.

OpenClaw maps cleanly to two of the three layers. It does not map to the third.

```
┌─────────────────────────────────────────────────────────┐
│  LAYER 1: Customer Agent                                 │
│  OpenClaw instance per customer                          │
│  — Personal memory, document vault, need articulation   │
│  — Channel: Telegram / WhatsApp / web                   │
│  VERDICT: ✅ OpenClaw fits well                          │
└─────────────────────────────────────────────────────────┘
           │ sends structured request
           ▼
┌─────────────────────────────────────────────────────────┐
│  LAYER 2: Routing Agent (agent-resolv core)              │
│  Broker registry + matching + reputation                 │
│  — License filter, geography, capacity, score            │
│  — Dispatch to broker nodes                              │
│  VERDICT: ⚠️  OpenClaw is NOT the right container        │
│  This is a backend service / API, not a chat agent.      │
│  Build purpose-built. Can use AI model internally.       │
└─────────────────────────────────────────────────────────┘
           │ routes to matched node(s)
           ▼
┌─────────────────────────────────────────────────────────┐
│  LAYER 3: Broker Node Agent                              │
│  OpenClaw instance per broker                            │
│  — Configured with their products, geography, capacity  │
│  — Skills: CRM, product catalog, insurer APIs           │
│  — Receives routed requests, engages client              │
│  VERDICT: ✅ OpenClaw fits well                          │
└─────────────────────────────────────────────────────────┘
```

### Is it the right approach? At MVP stage, yes. At scale, you'll diverge.

---

## Why OpenClaw (Arguments For)

1. **Fred is the PoC.** The customer agent does exactly what Fred does: reads docs, holds context, interprets needs, calls tools, communicates via channels. Zero new primitives needed.

2. **Speed.** Not building an agent runtime from scratch. Memory, sessions, tool calling, channel integration — all done. Go directly to the thesis.

3. **Nodes concept maps to broker nodes.** OpenClaw Nodes (physical/logical devices connecting to a Gateway with capabilities) is the exact same primitive as a broker node in agent-resolv.com. The conceptual model is already there.

4. **MCP = broker tool integration.** Broker's CRM, insurer's product API, document validation — all connect via mcporter. Already tested. No new infrastructure.

5. **VPS-deployable.** One OpenClaw instance per customer on a cheap VPS (Fly, Railway) is operationally feasible at small scale.

6. **Open source.** Can be forked. Where OpenClaw doesn't fit, extend it.

---

## Why Not OpenClaw (Arguments Against)

1. **Not designed for multi-tenancy.** Running one OpenClaw instance per customer is operationally complex at scale — provisioning, billing, isolation, updates across N instances. This is a real ops burden.

2. **The routing layer doesn't fit.** The broker registry, matching algorithm, and reputation scoring are backend service logic — not a conversational agent. Forcing it into OpenClaw would be wrong abstraction.

3. **Vendor dependency.** Your core product's runtime depends on a third-party framework still in active development. API changes break things. Roadmap is not yours to control.

4. **Cost model mismatch.** OpenClaw is designed for single-user cost structures. Each customer running a persistent agent costs money. Token cost per interaction needs a billing layer you'd build from scratch anyway.

5. **No customer onboarding automation.** Provisioning a new OpenClaw instance per customer, setting up their workspace, configuring their agent — none of this is automated in OpenClaw. You'd build the automation layer.

---

## The Recommended Approach

**Stage 1 (MVP / Rolando pilot): Use OpenClaw directly**

- Broker node (Rolando) = one OpenClaw instance, configured with his products + geography
- Customer agent = one OpenClaw instance per customer (or Fred acting as the customer agent initially)
- Routing = manual / Fred orchestrating
- Goal: validate the thesis, not build the platform

**Stage 2 (First 5–10 broker nodes): OpenClaw as the agent layer**

- Automate OpenClaw provisioning per broker node (script: `openclaw onboard`, configure workspace, push SOUL.md/AGENTS.md for the broker persona)
- Build the routing service separately (lightweight API: broker registry, match logic, dispatch)
- Customer agent still OpenClaw-based, per customer
- Goal: prove the three-layer model works end-to-end

**Stage 3 (Scale): Assess and diverge where needed**

- If OpenClaw's multi-tenant ops burden becomes the bottleneck → build a purpose-built agent runtime (informed by OpenClaw's loop, not shackled to it)
- If the routing service grows complex → invest in it independently
- The customer-facing agent may migrate to a lighter runtime or a web-native experience
- Goal: decouple what needs decoupling, keep what works

---

## Architecture Diagram (Current Thinking)

```
Customer                 ReisierX                    Broker (Rolando)
────────                 ────────                    ────────────────

[Customer's             [agent-resolv.com            [Broker's
 OpenClaw                Routing Service]              OpenClaw
 instance]                                             instance]
    │                         │                             │
    │  structured request      │                             │
    │ ──────────────────────► │                             │
    │                         │  match: license +           │
    │                         │  geo + capacity + score     │
    │                         │ ──────────────────────────► │
    │                         │                             │
    │  ◄─────────────────────────── broker accepts ─────────│
    │                         │                             │
    │  connected to broker ◄──│                             │
    │ ─────────────────────────────────────────────────────►│
    │                                                        │
    │  ◄──── conversation ─────────────────────────────────►│
    │                                                        │
    │                                                        │
    Transaction closes                                       │
    Outcome logged ─────────► Routing Service               │
                              (reputation update) ─────────►│
```

---

## Open Architecture Questions

| # | Question | Why it matters |
|---|----------|----------------|
| A | How does a new customer get an OpenClaw instance? Who provisions it? | Ops model for customer onboarding |
| B2 | OpenClaw's WhatsApp = Baileys only (no Cloud API support). Customer agent layer can't use native OpenClaw WhatsApp channel. Path A: bridge Cloud API webhook → OpenClaw. Path B: purpose-built customer agent runtime. Which? | Core architecture fork |
| B | ✅ Customer channel: WhatsApp Cloud API (Meta official). Broadest reach, production-grade from day one. Baileys ruled out — ToS risk incompatible with trust-based product. | Decided 2026-02-22 |
| C | What's the routing service built on? (Python/Node API, simple DB registry?) | Tech stack for the core |
| D | How does the routing service authenticate broker nodes? | Security model |
| E | How does document processing work? Customer uploads → agent extracts → what format goes to routing? | Data model for the request packet |
| F | At what point does OpenClaw get forked vs. replaced? | Build vs. buy line |
| G | Who manages broker node OpenClaw instances? ReisierX or broker self-serve? | Ops burden allocation |

---

## Iteration Log

| Date | What changed |
|------|-------------|
| 2026-02-22 | First draft — feasibility analysis + three-layer architecture + staged approach |
