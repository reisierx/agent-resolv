# Product Overview -- agent-resolv

> **Version:** 1.0
> **Last updated:** March 2, 2026
> **Status:** Draft -- internal review
> **Audience:** Goncalo (CEO)

---

## 1. What agent-resolv Does (Scenario Walk-Through)

### Today (without agent-resolv)

- A customer emails Rolando asking for auto insurance
- Rolando reads the email, manually extracts: name, NIF, date of birth, license plate, vehicle details, driving history
- Rolando composes individual emails to 4 insurers (Fidelidade, Allianz, Tranquilidade, Caravela) requesting quotes
- Over 2-3 days, insurers respond with PDF quotes attached to emails
- Rolando opens each PDF, manually compares premiums, coverages, deductibles, franchises
- Rolando creates a comparison (mental or spreadsheet), picks a recommendation
- Rolando emails the customer with the recommendation
- **Total time: 3-7 days, dozens of emails, hours of manual work per customer**

### With agent-resolv

- Same customer emails Rolando
- Rolando forwards the email to `intake@agent-resolv.com`
- The system extracts the customer profile automatically and sends Rolando a confirmation email with the extracted data
- Rolando clicks "Confirm Profile" (a link in the email) -- quote requests are automatically sent to 4 insurers
- As insurers respond with PDF quotes, the system parses each one and builds a comparison
- Rolando receives an email with the AI-generated comparison and recommendation
- Rolando reviews it, clicks "Approve" -- the customer gets a professional comparison
- **Total time: hours instead of days, Rolando's involvement is review and approval only**

## 2. What the MVP Looks Like for a Broker

- No software to install -- everything works through email
- Forward leads, receive structured profiles, confirm, receive comparisons, approve
- All state-changing actions (confirm, approve, cancel) via secure links in emails
- Rolando must review and approve every AI recommendation before it reaches the customer (regulatory requirement and quality control)

## 3. What Goncalo Needs to Decide

| Decision                                   | When                | Context                                                                           |
| ------------------------------------------ | ------------------- | --------------------------------------------------------------------------------- |
| Operating entity (new company vs ReisierX) | Pre-funding close   | Investor structure, equity allocation                                             |
| Legal counsel engagement                   | ASAP                | GDPR legal basis, AI Act classification, DPA templates needed before build starts |
| Insurer relationship strategy              | Pre-Phase 2 (April) | Which insurers to prioritize for email quoting -- Goncalo's network is key        |
| GTM one-pager (ICP, pricing, channels)     | Pre-Phase 2         | How we acquire brokers beyond Rolando                                             |
| ASF license timing                         | Q3-Q4 2026          | When to start the application process for D2C expansion                           |

## 4. What Goncalo Needs to Facilitate

| Action                                         | When                | Why It Matters                                                             |
| ---------------------------------------------- | ------------------- | -------------------------------------------------------------------------- |
| Rolando shadow sessions                        | End of March        | Blocks MVP scope -- we can't build without understanding the real workflow |
| Send Rolando the session prep kit              | Mid-March           | So he has sample PDFs, email threads, and insurer contacts ready           |
| Caravela Seguros introduction (Luis Cervantes) | Phase 2 (April-May) | Priority insurer for direct API integration                                |
| Legal counsel selection + engagement           | ASAP                | Multiple legal questions block Phase 2 start                               |
| Investor conversations                         | Starting March 3    | Funding one-pager and memo ready                                           |

## 5. Decision Calendar

```
March 2-7:    Investor conversations begin
Mid-March:    Send Rolando session prep kit
End of March: Rolando shadow sessions
Early April:  MVP product type(s) selected, scope finalized
April 7:      Legal basis for data processing confirmed (hard blocker for Phase 2)
April 14:     DPA template + sub-processor register complete
April 28:     DPA signed with Rolando's brokerage, Phase 2 begins
Q3 2026:      MVP live with Rolando
```
