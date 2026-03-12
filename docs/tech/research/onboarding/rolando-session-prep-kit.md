# Rolando Session Prep Kit

> **Version:** 1.0
> **Last updated:** March 2, 2026
> **Status:** Draft -- internal review
> **Audience:** Rolando (via Goncalo)

---

## 1. Context for Rolando

We're building a tool to automate the back-office work of insurance brokerage -- the parts that take the most time but don't require your expertise (data extraction, email drafting, PDF comparison). Your expertise and review remain essential -- the tool assists, you decide.

These sessions help us understand exactly how you work today so we can build something that fits your workflow, not force you to change it. We need 2-3 sessions of 1-2 hours each.

## 2. What to Bring / Have Ready

**Documents (critical -- bring digital copies if possible):**

- [ ] 5-10 recent quote PDFs from different insurers (mix of product types: auto, home, vida, health if available)
- [ ] 3-5 complete email threads with insurers (quote request to insurer response cycle)
- [ ] Your current comparison template (spreadsheet, document, or however you compare quotes today)
- [ ] A sample customer lead email (the kind of email you receive from a customer asking for insurance)
- [ ] Any examples of insurer portal screenshots (if you use portals for quoting)

**Lists:**

- [ ] Insurers you work with regularly and their contact methods (email address for quotes, portal URL, phone)
- [ ] Which product types generate the most volume in your practice
- [ ] Which insurers respond fastest vs. slowest to email quote requests

## 3. Session Topics & Questions

### Session 1: Your Current Workflow (observation)

- Walk us through a real case from start to finish -- customer contacts you, you collect info, request quotes, compare, recommend
- What does your current workflow look like with insurer portals?
- How fragmented are insurer quote/policy data formats in practice?
- How do you track which insurers have responded to a quote request?
- How many concurrent client cases do you typically manage?
- What's your email folder/label system for organizing client work?

*Why this matters: We need to understand the exact workflow so we can automate the right steps and keep the human-review steps where they belong.*

### Session 2: Documents & Data Formats (hands-on)

- How many distinct insurer PDF formats do you encounter regularly?
- Are any quote PDFs scanned rather than digital-native (i.e., photos or scans vs. computer-generated)?
- Password-protected PDFs -- how common are they?
- Do insurers ever respond with structured data (Excel, CSV) instead of PDF?
- What does a typical insurer email response look like? (just a PDF attachment, or text + attachment, or something else?)
- What other document types do you receive beyond quote PDFs? (proposals, conditions, policy documents)
- How do renewal quote formats differ from new business quotes?

*Why this matters: The AI needs to parse these documents accurately. We need real samples to test against and understand the full range of formats.*

### Session 3: Edge Cases & Product-Specific Details

- What percentage of customers know their bonus-malus class (auto)?
- How often do customers have their caderneta predial (property registry) ready (home)?
- How do you currently handle the rebuild cost estimation problem for home insurance?
- What's the typical turnaround for a complete auto insurance quote cycle?
- Which questions do customers find most confusing or refuse to answer?
- How do claims email threads differ from quoting threads?
- What's your relationship with Caravela Seguros -- tech stack and appetite for integration?
- Which insurers respond fastest to email quote requests?
- What does Fidelidade's commercial onboarding look like for new ASF-licensed brokers?

*Why this matters: Edge cases determine where AI can work autonomously vs. where it needs to hand off to you. Product-specific details drive which insurance type(s) we launch with first.*

## 4. What Happens After the Sessions

- We select the MVP product type(s) based on your real workflow volume and the data quality we've seen
- We test the AI on your actual PDFs and email threads
- We build the first version of the tool and you'll be the first user -- testing with real cases in a safe environment where you review everything before it reaches a customer
