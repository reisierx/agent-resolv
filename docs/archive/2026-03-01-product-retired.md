# agent-resolv — Product Vision & Design Philosophy

> Displaced from CONTEXT.md during 2026-02-28 redesign. This is the full product vision document.
> Load on demand when doing product work. The compressed summary lives in CONTEXT.md Section 7.

---

## Core Philosophy: Customer-In, Product-Out

The credit and insurance industry is built backwards. Current flow: suppliers design pre-packaged products → brokers present what they earn commission on → customers receive 80-page PDFs designed for compliance, not clarity → customers are left to decipher, compare, and hope they chose correctly.

**agent-resolv inverts this entirely.** We start with the customer:
1. What do you want to protect?
2. How much coverage?
3. For what specifically?
4. At what price point?

Only then do we find products that match — across all providers, independently, without bias. The customer defines the requirement; we execute the search. This is not a UX improvement. It is a fundamentally different business model.

Because we are licensed and independent, that trust is contractual — not marketing. And because the customer trusts us, we can serve their full financial life: every insurance line, every credit need, forever. "A gente resolve" — we sort it out — is the brand promise and the business model.

---

## "A Gente Resolve"

Portuguese for "we sort it out." Also: *agent* + *resolv*. Triple meaning: the AI agent, the technical resolve, and the human promise. The customer doesn't need to become an expert — they tell us what they need, we take it from there. Trust is the product. Everything else is execution.

---

## Value Dimensions

- **Independent:** No insurer bias. No commission pressure. Legally obligated to act in the customer's interest (corretor license).
- **Transparent:** Reasoning visible — not just an answer, but the why behind it.
- **Secure & private:** Customer data used only for their benefit. Never sold, never shared.
- **Fast:** Initial recommendation in seconds, not days with a broker.
- **Humane:** Plain language. No jargon. No compliance theatre.

---

## Customer Segments

| Segment | Pain | Value |
|---------|------|-------|
| **Individuals** | No one starts with what they actually need — just product catalogues | We start with their life, not the shelf |
| **SMEs** | 74% underinsured; mandatory products bought blind; broker conflicts of interest | Right coverage across all lines; we absorb the complexity |
| **Brokers (B2B2C)** | Manual research, compliance burden, limited product range | AI back office — we do the research, they keep the relationship |
| **AI Agents** | Need structured financial services with compliant, machine-readable I/O | First-class API client: structured intake, structured output, A2A-ready |

---

## Channel Stack — Omni-Channel Architecture

The channel is not a distribution decision. It is a product principle: the customer chooses how they come in, and they get the same broker agent regardless. One broker, any channel, consistent experience.

Every channel normalises to the same structured intake request before touching the broker agent. The broker agent never knows which channel it came from.

| Channel | Segment | Role |
|---------|---------|------|
| **WhatsApp** | Consumer, tech-savvy SME, expat | Primary async messaging; ~90% PT penetration |
| **Telegram** | AI agents, tech-savvy users | Agent economy primary channel |
| **Email** | Traditional SME professionals (accountants, lawyers, architects) | Permanent channel for this segment — not a fallback |
| **Voice** (ElevenLabs / Vapi) | Traditional SME, older demographics, complex intake | Core infrastructure; qualifying call → structured handoff to broker agent |
| **Web** | Inbound, long-form intake | agent-resolv.com |
| **API / A2A** | AI agents, B2B2C platforms | Machine-readable; structured protocol |

Voice intake is core infrastructure from day one — not a Phase 2 add-on. Traditional SME professionals will always prefer phone/email. Designing voice in late means rebuilding the intake layer.

---

## Customer Profiling & The agent-resolv Account

**Profiling is a first-class product step.** Before the broker agent routes to any provider, it needs a structured customer profile. This is built conversationally during the first intake — channel-adaptive (message-by-message on WhatsApp, async on email, spoken on voice). It is never a form. Partial profiles are saved; intake resumes exactly where it left off on the next contact. Returning customers are never re-profiled from scratch.

**Every customer gets a persistent account on first contact** — regardless of which channel they came in through. The account is the structural foundation of everything else:
- **Structured customer profile** — business type, sector, headcount, turnover, risk appetite, budget
- **Current insurance policies and credit facilities** — full financial picture, always up to date
- **Ongoing processes and open requests** — quotes in progress, applications pending, documents outstanding
- **Preference settings** — coverage priorities, preferred providers, communication channel
- **Opportunities** — coverage gaps, renewal optimisations, cross-sell moments the broker agent has identified

The account is what makes omni-channel coherent: a customer who first contacts via email, then calls six months later, is recognised immediately — the voice agent picks up from the same account, no re-profiling. The account is the compounding asset: the broker agent gets better per customer over time. Each interaction enriches the profile; each renewal is a retention moment; each cross-sell multiplies LTV. No comparison site has this. No traditional broker has it structured and accessible to an AI system. This is a durable moat.

---

## Machine-Readable Endpoint (Agent API)

agent-resolv publishes a structured, machine-readable service description — a toggle on the landing page and a dedicated endpoint. Tells AI agents exactly: what we do, what we accept, what we return, which channels are live, what our licensing status is.

AI agents are first-class clients. Not an afterthought — a design principle. A2A-ready architecture from day one.
