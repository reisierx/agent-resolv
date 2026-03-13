# agent-resolv.com — User & Node Flows

> Persistent artifact. Iterate here. Last updated: 2026-02-22.
> Open questions are marked ❓. Decisions are marked ✅.

---

## Flow 1 — Customer Journey

```mermaid
flowchart TD
    A([Customer]) --> B{How do they arrive?}
    B -->|❓ Broker referral| C[Invitation link from Rolando]
    B -->|❓ Organic / marketing| D[agent-resolv.com homepage]

    C & D --> E[Register & consent\nWhat data will be used, how]

    E --> F{Setup type?}
    F -->|Tech-savvy| G[Desktop vault\nDocs stay local on device]
    F -->|Everyone else| H[Web vault\nDocs hosted, encrypted]

    G & H --> I[State a need\ne.g. 'I need a mortgage'\n'Renew car insurance']

    I --> J[Client agent\nProcesses need + extracts\nrelevant data from docs]

    J --> K[Routing agent\nagent-resolv\nMatches to broker nodes]

    K --> L{Match found?}
    L -->|No| M[❓ Waitlist?\nExpand geography?]
    L -->|Yes| N[Request sent to broker\nPre-packaged context\nno raw docs]

    N --> O[Broker reviews\nand accepts]
    O --> P[Broker engages client\nvia platform]
    P --> Q[Transaction closes]
    Q --> R[Outcome logged\nReputation updated]
    R --> S{Client stays?}
    S -->|✅ Goal| T[Client relationship\nownd by platform\nBroker is a node]
    S -->|❓ Risk| U[Client follows broker\nif broker leaves]
```

---

## Flow 2 — Broker Onboarding (Joining as a Node)

```mermaid
flowchart TD
    A([Broker]) --> B{How do they join?}
    B -->|❓ Invitation only?| C[Invite from ReisierX]
    B -->|❓ Open application?| D[Apply at agent-resolv.com]

    C & D --> E[License verification\nASF / Banco de Portugal check\nAutomatic or manual?]

    E --> F{Approved?}
    F -->|No| G[Rejected / pending info]
    F -->|Yes| H[Configure node]

    H --> I[Products & services\ne.g. mortgage, car insurance\nlife insurance, leasing]
    I --> J[Geography\ne.g. Sardoal, Santarém district\nNational?]
    J --> K[Capacity\nMax requests/month]
    K --> L[❓ Fee / commission model\nHow does broker get paid?\nHow does ReisierX get paid?]
    L --> M[Go live as node]

    M --> N[Routing agent\ncan now match to this broker]
    N --> O[Receives routed request\nwith pre-packaged context]

    O --> P{Accept request?}
    P -->|No| Q[Declined\nRe-routed to next broker]
    P -->|Yes| R[Engage client\nvia platform interface]

    R --> S[Close transaction]
    S --> T[Outcome logged\nReputation score updates\nFeeds future routing priority]
```

---

## Flow 3 — Routing Logic (agent-resolv core)

```mermaid
flowchart TD
    A[Incoming request\nfrom client agent] --> B[Parse need\nproduct type + parameters]

    B --> C[Match against\nbroker node registry]
    C --> D{Filters}
    D --> E[License type covers product?]
    E --> F[Geography match?]
    F --> G[Capacity available?]
    G --> H[❓ Reputation score\nthreshold met?]

    H --> I[Ranked shortlist\nof broker nodes]
    I --> J{Routing strategy?}
    J -->|❓ Best match only| K[Single broker notified]
    J -->|❓ Top 3 compete| L[Multiple brokers notified\nfirst to accept wins]

    K & L --> M[Broker accepts]
    M --> N[Client agent\nand broker connected]
```

---

## Open Questions (surface for Feb 25 meeting)

| # | Question | Why it matters |
|---|----------|----------------|
| 1 | How does the first customer arrive? Broker referral or organic? | Defines the GTM motion entirely |
| 2 | Who owns the client relationship? Platform or broker? | Core defensibility question |
| 3 | What happens when a broker leaves — does the client stay? | Determines if you're building a platform or a tool |
| 4 | Invitation-only or open broker application? | Supply growth strategy |
| 5 | How does ReisierX make money? Per transaction? SaaS? | Revenue model not yet defined |
| 6 | How does the routing agent know which provider accepts which credential format? | Knowledge base build/maintenance problem |
| 7 | Best match vs. competing brokers — routing strategy? | UX + broker incentives |
| 8 | How does license verification work? Manual check or API? | Ops complexity |

---

## Iteration Log

| Date | What changed |
|------|-------------|
| 2026-02-22 | First draft — three flows, 8 open questions |
