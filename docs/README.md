# agent-resolv — Project Docs

Internal knowledge base. Not rendered publicly.

---

## Structure

```
docs/
├── INVESTMENT-MEMO.md       ← Canonical source of truth: thesis, product, market, decisions
├── PRD-v1.0.md              ← Product Requirements Document v1.0
│
├── research/                ← Research files (date-prefixed)
│   ├── INDEX.md             ← One-line summary of every research file
│   └── YYYY-MM-DD-topic.md
│
├── competitive/             ← Competitive intelligence and analysis
│   ├── 2026-03-12-withcoverage-intelligence.md   ← JP's WithCoverage deep-dive
│   └── 2026-03-12-withcoverage-analysis-fred.md  ← Fred's analysis + open questions
│
└── memos/                   ← Meeting prep, investor briefs, external memos
    └── bruno-dinis-meeting-prep.md
```

---

## Maintenance

- **INVESTMENT-MEMO.md** — Primary author: Opus (major rewrites); Fred (incremental updates). 60K char ceiling.
- **Research files** — Created by Fred or sub-agents. Always update `research/INDEX.md` when adding a file.
- **Competitive** — Competitive intelligence drops + analysis. Date-prefix all files.
- **Memos** — Meeting prep and external documents. Keep originals; add a `-fred.md` companion for analysis.

See `INVESTMENT-MEMO.md` Appendix B for the full decision log.
