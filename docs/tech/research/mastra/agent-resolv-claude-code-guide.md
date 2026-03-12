# agent-resolv — Claude Code Implementation Guide

## Instructions for Claude Code

This file contains step-by-step implementation instructions for building the agent-resolv MVP using Mastra. Follow each section in order. Each section is a self-contained unit of work that should be tested before moving to the next.

---

## Prerequisites

```bash
# Create project
npx create-turbo@latest agent-resolv
cd agent-resolv

# Initialize Mastra in the web app
cd apps/web
npx create-mastra@latest --components agents,tools,workflows --llm anthropic

# Install dependencies
npm install @mastra/core @ai-sdk/anthropic zod resend
npm install -D @types/node
```

Required environment variables in `.env`:
```
ANTHROPIC_API_KEY=sk-ant-...
DATABASE_URL=:memory:
```

---

## Task 1: Project Structure

Create this folder structure inside `apps/web/src/mastra/`:

```
src/mastra/
├── index.ts
├── schemas/
│   ├── customer-profile.ts
│   ├── normalized-quote.ts
│   ├── quote-request.ts
│   └── comparison-result.ts
├── agents/
│   ├── intake.ts
│   ├── quote-comparison.ts
│   └── orchestrator.ts
├── workflows/
│   └── insurance-quote.ts
├── tools/
│   ├── providers/
│   │   ├── registry.ts
│   │   ├── email-quote-request.ts
│   │   └── email-response-parser.ts
│   └── compliance/
│       ├── audit-log.ts
│       └── profile-validator.ts
```

---

## Task 2: Schema Layer

These schemas are the foundation. Implement them first — everything else depends on them.

### File: `src/mastra/schemas/customer-profile.ts`

Implement a discriminated union Zod schema for customer profiles. Four product types: auto, home, life, health. Each shares a base profile (id, nif, name, email, phone, dateOfBirth, address) and adds product-specific fields.

Key fields per product type:
- **auto**: vehicle (make, model, year, licensePlate, fuelType, usage, parkingType), driverHistory (licenseDate, claimsLast5Years, bonusMalus), desiredCoverage (minimum | third_party_plus | all_risk | all_risk_premium)
- **home**: property (type, ownership, constructionYear, area, floors, capitalEdificio, capitalRecheio, hasPool, hasGarden, alarmSystem)
- **life**: health (smoker, preExistingConditions, height, weight), coverage (capitalDesired, duration, linkedToMortgage, mortgageBank)
- **health**: members array (name, dateOfBirth, relationship), preferences (dentalCoverage, networkPreference, existingConditions)

NIF validation: 9-digit string. License plate: Portuguese format. All monetary values in euros (number).

### File: `src/mastra/schemas/normalized-quote.ts`

The standardized quote output that all provider tools must return. Fields: id, quoteRequestId, provider (id, name), productType, premium (annual, monthly, paymentOptions with frequency/amount/surcharge), coverage array (type, description, limit, deductible, included, optional, optionalPremium), exclusions, conditions, validUntil, rawDocumentUrl, receivedAt, confidence (0-1 float for parsing accuracy), parsingNotes.

### File: `src/mastra/schemas/quote-request.ts`

What gets sent to providers. Fields: id, customerId, productType, profile (CustomerProfile), providers (string array), requestedAt, urgency, notes.

### File: `src/mastra/schemas/comparison-result.ts`

What the comparison agent produces. Fields: quoteRequestId, quotes array, recommendation (bestValue, bestCoverage, cheapest — all quote IDs, plus reasoning string in Portuguese), comparisonMatrix (coverageType mapped to provider data), generatedAt.

---

## Task 3: Intake Agent (Phase 1 Core)

### File: `src/mastra/agents/intake.ts`

Create a Mastra Agent with these characteristics:

- **Model**: `anthropic('claude-sonnet-4-5-20250514')`
- **Language**: European Portuguese (pt-PT), formal "você" address
- **Behavior**: Collects insurance profile data conversationally. One question at a time. Validates data inline (NIF format, plate format). Explains coverage options when customer is unsure.
- **Product detection**: Determines product type from conversation and routes profiling questions accordingly.
- **Structured output**: Once all required fields are collected, produces a valid `CustomerProfile` object. Confirms summary with customer before finalizing.
- **Compliance**: Identifies itself as AI assistant working for a licensed broker. Never promises pricing. Notes that quotes come from insurance providers.

Test with the Mastra playground (`npm run dev` → http://localhost:4111):
- Conversation in Portuguese for auto insurance
- Conversation where customer doesn't know their bonus-malus
- Conversation where customer changes product type mid-intake
- Conversation with invalid NIF provided

---

## Task 4: Compliance Tools (Phase 1)

### File: `src/mastra/tools/compliance/audit-log.ts`

Mastra tool that logs every significant agent action. Input: action enum (customer_intake_started, customer_intake_completed, quote_requested, quote_received, comparison_generated, recommendation_made, human_review_requested, human_review_completed, policy_binding_initiated), agentId, customerId, details record, timestamp. Output: logId, success boolean. MVP implementation: write to database via Mastra storage.

### File: `src/mastra/tools/compliance/profile-validator.ts`

Mastra tool that validates a CustomerProfile has all required fields for its product type before allowing quote requests. Returns: valid boolean, missingFields array, warnings array. Example warning: "Bonus-malus não fornecido — cotações podem variar".

---

## Task 5: Provider Registry & Email Tools (Phase 2 Core)

### File: `src/mastra/tools/providers/registry.ts`

Static registry of Portuguese insurance providers. Each entry: id, name, quoteEmail, products array, integrationTier (email | api | middleware), responseTime estimate, emailTemplate identifier.

Initial providers: Fidelidade, Tranquilidade, Allianz, Ageas, Generali, Lusitania, Caravela. All start at integrationTier 'email'.

### File: `src/mastra/tools/providers/email-quote-request.ts`

Mastra tool that composes and sends a structured quote request email to a provider.

Input: provider (from registry), CustomerProfile, productType, optional notes.
Output: requestId, status (sent | failed), sentAt, emailThreadId.

Implementation:
1. Select email template based on provider + productType
2. Map CustomerProfile fields to template format
3. Send via Resend API (or Nodemailer for POC)
4. Return tracking info

Email templates should be professional Portuguese business emails that a real broker would send. Include: broker identification, customer info summary, coverage requested, urgency, contact for follow-up.

### File: `src/mastra/tools/providers/email-response-parser.ts`

Mastra tool that parses a provider's quote response (email + PDF attachment) into a NormalizedQuote.

Input: emailId, providerId, productType, attachments array (filename, contentType, base64 content).
Output: NormalizedQuote.

Implementation:
1. Extract text from PDF attachment (use pdf-parse or similar)
2. Send extracted text to LLM with provider-specific parsing prompt
3. LLM outputs structured NormalizedQuote
4. Validate against schema
5. Assign confidence score based on extraction quality
6. Flag ambiguous fields in parsingNotes

This is the most complex tool. The LLM prompt needs to understand Portuguese insurance terminology and map provider-specific terms to the normalized schema.

---

## Task 6: Quote Comparison Agent (Phase 2)

### File: `src/mastra/agents/quote-comparison.ts`

Mastra Agent for analyzing and comparing normalized quotes.

- **Model**: `anthropic('claude-sonnet-4-5-20250514')`
- **Input**: Array of NormalizedQuote objects + CustomerProfile
- **Output**: ComparisonResult (structured output)
- **Comparison criteria**: Coverage adequacy for customer situation (40%), price/value ratio (30%), provider reputation (15%), additional benefits (15%)
- **Language**: Portuguese for customer-facing recommendation text
- **Compliance**: Discloses AI-generated nature, notes qualified director review required, documents reasoning for every recommendation

Test: Feed 3 mock NormalizedQuotes for the same auto insurance profile. Verify it produces valid ComparisonResult with coherent reasoning.

---

## Task 7: Brokerage Workflow (Phase 2 Core)

### File: `src/mastra/workflows/insurance-quote.ts`

The core deterministic pipeline. Six steps chained with `.then()`:

**Step 1 — validate-profile**: Takes CustomerProfile, runs profileValidatorTool, returns valid/invalid with missing fields. Bails if invalid.

**Step 2 — request-quotes**: Takes validated profile + provider list. For each provider, calls emailQuoteRequestTool. Returns array of requestIds with status.

**Step 3 — collect-responses**: **SUSPENDS** the workflow. This is the async gap where we wait for provider email responses (could be hours/days). Suspends with reason and awaiting provider list. Resumes when the inbound email processor calls `workflow.resume()` with parsed NormalizedQuote array.

**Step 4 — compare-quotes**: Calls quoteComparisonAgent with collected quotes + original profile. Returns ComparisonResult with structured output.

**Step 5 — human-review**: **SUSPENDS** for qualified director approval. Provides the ComparisonResult for review. Resumes with approved boolean, selectedQuoteId, optional reviewerNotes, reviewerName.

**Step 6 — send-recommendation**: Takes approved comparison + customer email. Composes and sends the recommendation email to the customer.

Key implementation notes:
- Two suspend points make this workflow asynchronous by design
- Step 3 can be resumed partially (some providers responded, not all) with a timeout mechanism
- Step 5 resume comes from the broker dashboard UI
- Full audit logging at each step

---

## Task 8: Orchestrator Agent Network (Phase 2)

### File: `src/mastra/agents/orchestrator.ts`

Top-level routing agent configured with sub-agents, workflows, and tools.

```typescript
const orchestratorAgent = new Agent({
  id: 'orchestrator',
  name: 'Agente Orquestrador',
  instructions: `...`, // routing logic
  model: anthropic('claude-sonnet-4-5-20250514'),
  agents: {
    intakeAgent,
    quoteComparisonAgent,
  },
  workflows: {
    insuranceQuoteWorkflow,
  },
  tools: {
    auditLogTool,
    profileValidatorTool,
  },
});
```

Routing logic in instructions:
- New customer / needs profiling → intakeAgent
- Profile complete, wants quotes → insuranceQuoteWorkflow
- Has quotes, wants comparison → quoteComparisonAgent
- General insurance questions → answer directly
- Complaint or issue → flag for human

Test with `.network()`:
```typescript
await orchestratorAgent.network('Olá, preciso de seguro para o meu carro');
```

Verify it routes to intakeAgent and starts the profiling conversation.

---

## Task 9: Mastra Instance Registration

### File: `src/mastra/index.ts`

Register all agents, workflows, and tools:

```typescript
import { Mastra } from '@mastra/core/mastra';
import { PinoLogger } from '@mastra/loggers';
import { LibSQLStore } from '@mastra/libsql';

import { intakeAgent } from './agents/intake';
import { quoteComparisonAgent } from './agents/quote-comparison';
import { orchestratorAgent } from './agents/orchestrator';
import { insuranceQuoteWorkflow } from './workflows/insurance-quote';

export const mastra = new Mastra({
  agents: {
    intakeAgent,
    quoteComparisonAgent,
    orchestratorAgent,
  },
  workflows: {
    insuranceQuoteWorkflow,
  },
  storage: new LibSQLStore({
    url: process.env.DATABASE_URL || ':memory:',
  }),
  logger: new PinoLogger({
    name: 'agent-resolv',
    level: 'info',
  }),
});
```

---

## Task 10: Inbound Email Bridge (Phase 2)

This is NOT a Mastra component — it's the glue between your email infrastructure and the Mastra workflow.

### File: `src/lib/email-ingest.ts`

Webhook handler for inbound emails (Resend, SendGrid, or Cloudflare Email Workers):

1. Receive inbound email webhook
2. Match email to a pending quote request (by subject, thread ID, or sender domain → provider ID)
3. Extract attachments (PDF)
4. Call emailResponseParserTool to parse into NormalizedQuote
5. Store the quote
6. Check if all expected providers have responded (or timeout threshold met)
7. If ready, call `workflow.resume()` on the suspended workflow run with all collected quotes

### File: `app/api/webhooks/inbound-email/route.ts`

Next.js API route that receives the inbound email webhook and delegates to email-ingest.ts.

---

## Testing Strategy

### Unit Tests (per task)

- Schema validation: valid and invalid inputs for each schema
- Tool execution: mock inputs, verify outputs match schema
- Agent structured output: verify CustomerProfile output is valid

### Integration Tests (per phase)

- Phase 1: Full intake conversation → valid CustomerProfile
- Phase 2: Workflow end-to-end with mock provider responses
- Phase 2: Suspend → manual resume → workflow continuation

### Manual Testing

Use the Mastra playground (http://localhost:4111) for:
- Agent conversations in Portuguese
- Workflow execution visualization
- Tool invocation testing

---

## Notes for Claude Code

- Always use Zod schemas for type safety — never `any` or untyped objects
- All text visible to customers must be in European Portuguese (pt-PT)
- Use `crypto.randomUUID()` for all ID generation
- Every tool should have clear `description` fields — these drive agent routing
- When implementing email templates, make them realistic enough that a PT insurance provider would take them seriously
- Log every significant action through the auditLogTool — this is a regulatory requirement
- Pin Mastra dependency versions — the framework moves fast
- Test with the Mastra playground first, then build the Next.js UI on top
