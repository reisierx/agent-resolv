# Claude Code Implementation Guide

> **Version:** 1.0
> **Last updated:** March 2, 2026
> **Status:** Draft -- internal review

> **Note:** This document is designed for direct consumption by Claude Code during implementation. It is not intended for non-technical stakeholder review. The content is intentionally verbose, structured, and explicit — optimized for LLM consumption, not human reading. Each task is self-contained with code patterns, testing strategy, and acceptance criteria so that an AI coding agent can execute each task independently with minimal human clarification.

> **TL;DR:** 13 self-contained implementation tasks for building the agent-resolv MVP and Phase 3 interface expansion. Turborepo monorepo following the BuL venture-studio-template. Mastra agents in `packages/agents/`, Zod schemas in `packages/contracts/`, business logic in `packages/core/` with Effect-TS. API via Hono + Node.js with `@mastra/hono`. Durable workflows via `@mastra/inngest`. Postgres + Drizzle ORM. The primary interface is email. Each task includes code patterns, testing strategy, and acceptance criteria. Tasks 1-5 are Phase 1 (foundation), Tasks 6-9 are Phase 2 (email brokerage + workflow), Task 10 is API server setup (Mastra instance + Hono), Task 11 is Inngest engine (inbound email processing), Task 12 is email broker interface (Phase 2), Task 13 is MCP server (Phase 3).

## Prerequisites

```bash
# Scaffold monorepo from BuL template
npx create-turbo@latest agent-resolv
cd agent-resolv
```

**Package manager:** pnpm (check `packageManager` field in root `package.json`).

**.npmrc** -- pin exact versions:
```
save-exact=true
```

**Core dependencies by package:**

```bash
# packages/agents
pnpm --filter agents add @mastra/core @mastra/pg @mastra/inngest @mastra/memory @ai-sdk/anthropic zod
pnpm --filter agents add -D mastra

# packages/contracts
pnpm --filter contracts add zod

# packages/core
pnpm --filter core add effect

# packages/db
pnpm --filter db add drizzle-orm postgres
pnpm --filter db add -D drizzle-kit

# packages/env
pnpm --filter env add @t3-oss/env-core zod

# packages/auth
pnpm --filter auth add better-auth

# apps/api
pnpm --filter api add hono @hono/node-server @mastra/hono inngest
pnpm --filter api add -D tsx

# apps/engine
pnpm --filter engine add inngest @hono/node-server hono
pnpm --filter engine add -D tsx
```

**Phase 3-only dependency (Task 13):**

```bash
# apps/mcp
pnpm --filter mcp add @mastra/mcp zod
```

**Required environment variables:**

```
# LLM
ANTHROPIC_API_KEY=

# Database (Neon)
DATABASE_URL=postgresql://...

# Email
RESEND_API_KEY=
INBOUND_EMAIL_WEBHOOK_SECRET=

# Auth
BETTER_AUTH_SECRET=

# Analytics
POSTHOG_API_KEY=
POSTHOG_HOST=

# Inngest
INNGEST_EVENT_KEY=          # production only
INNGEST_SIGNING_KEY=        # production only

# Feature flags
ENABLE_WEB_CHAT=false
ENABLE_WHATSAPP=false
```

## Task Overview

| Task                       | Phase | Effort       | Description                     |
| -------------------------- | ----- | ------------ | ------------------------------- |
| 1. Monorepo Structure      | 1     | S (1-2 days) | Scaffold from BuL template      |
| 2. Schema Layer            | 1     | M (3-4 days) | Zod schemas for all types       |
| 3. Database Schema         | 1     | S (1-2 days) | Drizzle ORM tables              |
| 4. Lead Processing Agent   | 1     | M (3-4 days) | Mastra agent for extraction     |
| 5. Compliance Tools        | 1     | S (1-2 days) | Audit log + validator           |
| 6. Provider Email Tools    | 2     | M (3-4 days) | Email composition + PDF parsing |
| 7. Quote Comparison Agent  | 2     | M (3-4 days) | Comparison generation           |
| 8. Brokerage Workflow      | 2     | L (4-5 days) | 6-step Inngest workflow         |
| 9. Orchestrator            | 2     | S (2-3 days) | Agent network routing           |
| 10. API Server             | 1-2   | M (2-3 days) | Hono + Mastra adapter           |
| 11. Inngest Engine         | 2     | M (3-4 days) | Inbound email processing        |
| 12. Email Broker Interface | 2     | L (4-5 days) | Action links, reply parsing     |
| 13. MCP Server             | 3     | S (2-3 days) | MCP tool wrappers               |

Effort: S = 1-2 days, M = 3-4 days, L = 4-5 days

## Task 1: Monorepo Structure

Scaffold the full monorepo from the venture-studio-template. Create all workspace packages with `package.json`, `tsconfig.json`, and entry points.

**Phase 2 scope guard:** Keep `apps/mcp/` as a placeholder only (or skip it entirely) during Phase 2. Implement and wire MCP only in Task 13 (Phase 3).

```
agent-resolv/
  apps/
    api/                    # Hono + Node.js + @mastra/hono
      src/
        index.ts            # Server entry, mounts Mastra + custom routes
        routes/             # Custom route groups (webhooks, admin, broker)
      package.json
      tsconfig.json

    engine/                 # Inngest worker process
      src/
        index.ts            # Inngest serve endpoint
        functions/          # Inngest function definitions
      package.json
      tsconfig.json

    mcp/                    # MCP server (Phase 3)
      src/
        index.ts
      package.json
      tsconfig.json

    web/                    # Broker dashboard (Phase 3)
      package.json
      tsconfig.json

  packages/
    agents/                 # Mastra instance, agents, tools, memory
      src/
        index.ts            # Mastra instance export (for mastra dev)
        agents/
          lead-processor.ts
          quote-comparison.ts
          orchestrator.ts
        tools/
          providers/
            registry.ts
            email-quote-request.ts
            email-response-parser.ts
          compliance/
            audit-log.ts
            profile-validator.ts
        workflows/
          insurance-quote.ts
      package.json          # "dev": "mastra dev --dir src/"
      tsconfig.json

    contracts/              # Zod schemas — single source of truth
      src/
        index.ts
        schemas/
          customer-profile.ts
          normalized-quote.ts
          quote-request.ts
          comparison-result.ts
        validation/
          nif.ts
          license-plate.ts
      package.json
      tsconfig.json

    core/                   # Business logic (Effect-TS for domain logic; plain TS in apps/ — ADR-006)
      src/
        index.ts
        email-matching/     # Multi-signal email matching
        provider/           # Provider adapter interface
      package.json
      tsconfig.json

    db/                     # Drizzle ORM schema + client
      src/
        index.ts            # db client export
        schema.ts           # Drizzle table definitions
      drizzle/              # Generated migrations
      drizzle.config.ts
      package.json
      tsconfig.json

    env/                    # Type-safe env vars
      src/
        server.ts           # Server-only env (DATABASE_URL, secrets)
        client.ts           # Client-safe env (VITE_ vars)
      package.json
      tsconfig.json

    auth/                   # Better Auth
      src/
        index.ts            # Server auth (betterAuth())
        client.ts           # Client auth (createAuthClient())
      package.json
      tsconfig.json

    logging/                # Pino structured logging
      src/
        index.ts            # createLogger() factory
      package.json
      tsconfig.json

    analytics/              # PostHog
      src/
        server.ts
        browser.ts
      package.json
      tsconfig.json

  tooling/
    typescript/             # Shared tsconfig variants
      base.json
      react.json
      server.json
    tailwind/               # Shared Tailwind config (Phase 3)

  turbo.json
  pnpm-workspace.yaml
  biome.jsonc
  .npmrc
```

**turbo.json:**
```json
{
  "$schema": "https://turbo.build/schema.json",
  "ui": "tui",
  "globalDependencies": [".env"],
  "tasks": {
    "build": { "dependsOn": ["^build"] },
    "dev": { "cache": false, "persistent": true },
    "check": { "dependsOn": ["^check"] },
    "lint": {},
    "test": {}
  }
}
```

**pnpm-workspace.yaml:**
```yaml
packages:
  - "apps/*"
  - "packages/*"
  - "tooling/*"
```

**Test:** `pnpm install` succeeds. `pnpm check` runs typecheck across all packages. `pnpm --filter agents dev` starts Mastra playground at localhost:4111.

## Task 2: Schema Layer (packages/contracts)

All schemas use Zod. All monetary values in euros. All dates as ISO strings. All IDs as UUID (`crypto.randomUUID()`).

**Scope note:** Schemas are defined for all 4 product types (auto, home, life, health) to keep the architecture product-type-agnostic. MVP implementation and testing focus on the product type(s) selected after Rolando shadow sessions. Schemas for non-MVP products are defined but not actively tested or used until those products are built.

### CustomerProfile -- Discriminated Union

```typescript
// packages/contracts/src/schemas/customer-profile.ts
import { z } from 'zod';
import { nifSchema } from '../validation/nif';
import { licensePlateSchema } from '../validation/license-plate';

const baseProfile = z.object({
  id: z.string().uuid(),
  nif: nifSchema,
  name: z.string().min(1),
  email: z.string().email(),
  phone: z.string(),
  dateOfBirth: z.string().date(),
  address: z.object({
    street: z.string(),
    postalCode: z.string(),
    city: z.string(),
    district: z.string(),
  }),
  createdAt: z.string().datetime(),
  updatedAt: z.string().datetime(),
});

export const autoProfile = baseProfile.extend({
  productType: z.literal('auto'),
  vehicle: z.object({
    make: z.string(),
    model: z.string(),
    year: z.number().int().min(1900).max(2030),
    licensePlate: licensePlateSchema,
    fuelType: z.enum(['gasoline', 'diesel', 'electric', 'hybrid', 'lpg']),
    usage: z.enum(['personal', 'professional', 'mixed']),
    parkingType: z.enum(['garage', 'street', 'parking_lot']),
    estimatedKmYear: z.number().optional(),
    cilindrada: z.number().int().optional(),
    potencia: z.number().optional(),
    municipioCirculacao: z.string().optional(),
    valorVeiculo: z.number().optional(),
  }),
  driverHistory: z.object({
    licenseDate: z.string().date(),
    claimsLast5Years: z.number().int().min(0),
    bonusMalus: z.number().optional(),
    atFaultClaims: z.number().int().min(0).optional(),
  }),
  desiredCoverage: z.enum(['rc_only', 'rc_furto_incendio', 'all_risk']),
});

export const homeProfile = baseProfile.extend({
  productType: z.literal('home'),
  property: z.object({
    type: z.enum(['apartment', 'house', 'villa']),
    ownership: z.enum(['owner', 'tenant']),
    constructionYear: z.number().int().optional(),
    area: z.number().positive(),
    floors: z.number().int().positive(),
    capitalEdificio: z.number().positive(),
    capitalRecheio: z.number().optional(),
    hasPool: z.boolean(),
    hasGarden: z.boolean(),
    alarmSystem: z.boolean(),
    constructionType: z.string().optional(),
    roofType: z.string().optional(),
    floodRiskZone: z.boolean().optional(),
  }),
  address: baseProfile.shape.address.extend({
    parish: z.string().optional(),
  }),
});

export const lifeProfile = baseProfile.extend({
  productType: z.literal('life'),
  health: z.object({
    smoker: z.boolean(),
    preExistingConditions: z.array(z.string()),
    height: z.number().optional(),
    weight: z.number().optional(),
  }),
  coverage: z.object({
    capitalDesired: z.number().positive(),
    duration: z.number().int().positive(),
    linkedToMortgage: z.boolean(),
    mortgageBank: z.string().optional(),
  }),
});

export const healthProfile = baseProfile.extend({
  productType: z.literal('health'),
  members: z.array(z.object({
    name: z.string(),
    dateOfBirth: z.string().date(),
    relationship: z.enum(['self', 'spouse', 'child', 'parent']),
    qisResponses: z.record(z.string(), z.unknown()).optional(),
  })),
  preferences: z.object({
    dentalCoverage: z.boolean(),
    networkPreference: z.enum(['open', 'closed', 'mixed']).optional(),
    existingConditions: z.array(z.string()),
  }),
  gdprConsent: z.object({
    explicitHealthDataConsent: z.boolean(),
    consentTimestamp: z.string().datetime(),
    consentMethod: z.enum(['whatsapp', 'web', 'email']),
  }),
});

export const customerProfileSchema = z.discriminatedUnion('productType', [
  autoProfile, homeProfile, lifeProfile, healthProfile,
]);

export type CustomerProfile = z.infer<typeof customerProfileSchema>;
```

### NormalizedQuote, QuoteRequest, ComparisonResult

Follow the schemas defined in [03-technical-architecture.md](./03-technical-architecture.md). All fields must have Zod schemas with appropriate validation (positive numbers for premiums, 0-1 range for confidence, valid date strings).

**Test:** Valid and invalid inputs for each schema. Edge cases: missing optional fields, boundary values, invalid NIF format, future dates.

## Task 3: Database Schema (packages/db)

Define Drizzle ORM tables for all application data. Mastra manages its own schema (`mastra` Postgres schema) via `@mastra/pg` -- do not define Mastra tables manually.

```typescript
// packages/db/src/schema.ts
import { pgTable, uuid, varchar, text, jsonb, integer, real, boolean, timestamp } from 'drizzle-orm/pg-core';

export const customers = pgTable('customers', {
  id: uuid('id').primaryKey().defaultRandom(),
  nif: varchar('nif', { length: 9 }).notNull(),
  name: varchar('name', { length: 255 }).notNull(),
  email: varchar('email', { length: 255 }).notNull(),
  phone: varchar('phone', { length: 20 }),
  createdAt: timestamp('created_at').defaultNow().notNull(),
  updatedAt: timestamp('updated_at').defaultNow().notNull(),
});

export const customerProfiles = pgTable('customer_profiles', {
  id: uuid('id').primaryKey().defaultRandom(),
  customerId: uuid('customer_id').references(() => customers.id, { onDelete: 'cascade' }).notNull(),
  productType: varchar('product_type', { length: 20 }).notNull(),
  profileData: jsonb('profile_data').notNull(),
  version: integer('version').default(1).notNull(),
  createdAt: timestamp('created_at').defaultNow().notNull(),
});

export const quoteRequests = pgTable('quote_requests', {
  id: uuid('id').primaryKey().defaultRandom(),
  customerId: uuid('customer_id').references(() => customers.id, { onDelete: 'cascade' }).notNull(),
  productType: varchar('product_type', { length: 20 }).notNull(),
  providers: jsonb('providers').notNull(),
  status: varchar('status', { length: 30 }).notNull().default('pending'),
  requestedAt: timestamp('requested_at').defaultNow().notNull(),
  notes: text('notes'),
});

export const quoteRequestProviders = pgTable('quote_request_providers', {
  id: uuid('id').primaryKey().defaultRandom(),
  quoteRequestId: uuid('quote_request_id').references(() => quoteRequests.id, { onDelete: 'cascade' }).notNull(),
  providerId: varchar('provider_id', { length: 50 }).notNull(),
  status: varchar('status', { length: 30 }).notNull().default('pending'),
  emailSentAt: timestamp('email_sent_at'),
  responseReceivedAt: timestamp('response_received_at'),
});

export const normalizedQuotes = pgTable('normalized_quotes', {
  id: uuid('id').primaryKey().defaultRandom(),
  quoteRequestId: uuid('quote_request_id').references(() => quoteRequests.id, { onDelete: 'cascade' }).notNull(),
  providerId: varchar('provider_id', { length: 50 }).notNull(),
  premiumData: jsonb('premium_data').notNull(),
  coverageData: jsonb('coverage_data').notNull(),
  confidence: real('confidence').notNull(),
  rawDocumentUrl: text('raw_document_url'),
  receivedAt: timestamp('received_at').defaultNow().notNull(),
});

export const comparisons = pgTable('comparisons', {
  id: uuid('id').primaryKey().defaultRandom(),
  quoteRequestId: uuid('quote_request_id').references(() => quoteRequests.id, { onDelete: 'cascade' }).notNull(),
  recommendation: jsonb('recommendation').notNull(),
  comparisonMatrix: jsonb('comparison_matrix').notNull(),
  generatedAt: timestamp('generated_at').defaultNow().notNull(),
});

// NOTE: `user` table is managed by Better Auth (not defined here).
// Better Auth creates its own `user`, `session`, `account` tables on init.
// See packages/auth/ for config. reviewerId stores the Better Auth user ID
// but is NOT a DB-level FK — enforced at app layer since Better Auth owns the table.
export const reviews = pgTable('reviews', {
  id: uuid('id').primaryKey().defaultRandom(),
  comparisonId: uuid('comparison_id').references(() => comparisons.id, { onDelete: 'cascade' }).notNull(),
  reviewerId: varchar('reviewer_id', { length: 64 }).notNull(), // Better Auth user ID (format depends on BA config — not necessarily UUID), app-layer validation (must have qualified_director role)
  approved: boolean('approved').notNull(),
  selectedQuoteId: uuid('selected_quote_id'),
  notes: text('notes'),
  reviewedAt: timestamp('reviewed_at').defaultNow().notNull(),
});

// NOTE: Deletion strategy (hard delete with cascades vs. pseudonymization/soft-delete)
// requires legal counsel input before launch. Regulated flows may require pseudonymization
// to preserve audit integrity while removing PII. See 04-compliance-and-risk.md.
export const auditLogs = pgTable('audit_logs', {
  id: uuid('id').primaryKey().defaultRandom(),
  action: varchar('action', { length: 50 }).notNull(),
  agentId: varchar('agent_id', { length: 100 }).notNull(),
  customerId: uuid('customer_id').references(() => customers.id, { onDelete: 'set null' }), // retained for legal obligation
  details: jsonb('details'),
  createdAt: timestamp('created_at').defaultNow().notNull(),
});
```

**Test:** `drizzle-kit push` applies schema to local Postgres. Insert/query operations work via Drizzle client. Verify foreign key constraints enforce referential integrity.

## Task 4: Lead Processing Agent

The lead processing agent is the core of the broker co-pilot. It receives unstructured input from the broker (forwarded emails, contact form submissions, pasted phone notes) and extracts a structured CustomerProfile.

```typescript
// packages/agents/src/agents/lead-processor.ts
import { Agent } from '@mastra/core/agent';
import { anthropic } from '@ai-sdk/anthropic';

export const leadProcessorAgent = new Agent({
  id: 'lead-processor',
  model: anthropic('claude-sonnet-4-5-20250514'),
  instructions: `You are an AI back-office assistant for a licensed Portuguese insurance broker (mediador de seguros autorizado pela ASF).

Your role: extract structured customer profile data from unstructured broker inputs. The broker feeds you leads from their existing channels — you extract, validate, and organize the data.

INPUT TYPES you will receive:
1. FORWARDED EMAIL: Customer emailed the broker requesting a quote.
2. CONTACT FORM: Structured form submission from broker's website.
3. PASTED NOTES: Broker's free-text notes from a phone call or WhatsApp conversation.
4. DOCUMENT: Photo of existing policy, caderneta predial, or other document.

EXTRACTION RULES:
1. Extract every field you can identify. Mark confidence level (high/medium/low) for each.
2. Identify the insurance product type from context (auto, home, life, health).
3. For auto insurance: if matricula is present, note it for VIS/DUA auto-fill.
4. List all MISSING required fields that the broker needs to collect.
5. Flag inconsistencies (e.g., construction year doesn't match property description).
6. Detect PT-BR patterns ("placa" = matricula, CNH/CPF = note NIF is needed).

LANGUAGE: All field names and internal processing use English. Missing field descriptions and broker-facing notes use European Portuguese (pt-PT).

VALIDATION:
- NIF: 9 digits
- License plate: Portuguese format (XX-XX-XX)
- Postal code: Portuguese format (XXXX-XXX)
- Date of birth: reasonable age for insurance product

OUTPUT: Produce a LeadProcessingResult as structured output containing the extracted profile plus metadata.`,
  structuredOutput: {
    schema: leadProcessingResultSchema,
  },
});
```

The `LeadProcessingResult` schema wraps `CustomerProfile` with extraction metadata:

```typescript
// packages/contracts/src/schemas/lead-processing-result.ts
import { z } from 'zod';
import { autoProfile, homeProfile, lifeProfile, healthProfile } from './customer-profile';

// Uses deepPartial so the agent can return partially extracted profiles.
// profile is null only when input is too incomplete to even identify the product type.
// When non-null, missing fields will have undefined values — missingFields array
// lists what the broker still needs to collect from the customer.
// productType is derived from profile.productType when profile is non-null.
//
// Note: each partial variant is defined explicitly (not mapped from the union)
// because z.discriminatedUnion requires a readonly tuple, not a mapped array.
const partialAutoProfile = autoProfile.deepPartial().required({ productType: true });
const partialHomeProfile = homeProfile.deepPartial().required({ productType: true });
const partialLifeProfile = lifeProfile.deepPartial().required({ productType: true });
const partialHealthProfile = healthProfile.deepPartial().required({ productType: true });

export const partialCustomerProfileSchema = z.discriminatedUnion('productType', [
  partialAutoProfile, partialHomeProfile, partialLifeProfile, partialHealthProfile,
]);

export const leadProcessingResultSchema = z.object({
  profile: partialCustomerProfileSchema.nullable(),
  extractedFields: z.array(z.object({
    field: z.string(),
    value: z.unknown(),
    confidence: z.enum(['high', 'medium', 'low']),
  })),
  missingFields: z.array(z.string()),
  brokerNotes: z.array(z.string()),
  inputType: z.enum(['email', 'contact_form', 'phone_notes', 'document']),
});
```

**Test in Mastra playground (`pnpm --filter agents dev`):**
1. Forwarded customer email requesting insurance (MVP product type) -- verify field extraction accuracy
2. Contact form submission with partial data -- verify missing fields identified and returned in `missingFields`
3. Pasted phone notes in informal Portuguese -- verify extraction from unstructured text, `brokerNotes` populated
4. Email with PT-BR language patterns -- verify detection and NIF flag
5. Ambiguous product type -- verify `profile.productType` inferred when possible, or `profile` is null with explanation in `brokerNotes`

## Task 5: Compliance Tools

### auditLogTool

```typescript
// packages/agents/src/tools/compliance/audit-log.ts
import { createTool } from '@mastra/core/tools';
import { z } from 'zod';

export const auditLogTool = createTool({
  id: 'audit-log',
  description: 'Log a significant action for regulatory audit trail.',
  inputSchema: z.object({
    action: z.enum([
      'customer_intake_started',
      'quote_requested',
      'quote_received',
      'comparison_generated',
      'recommendation_made',
      'human_review_requested',
      'human_review_completed',
      'policy_binding_initiated',
    ]),
    agentId: z.string(),
    customerId: z.string().uuid(),
    details: z.record(z.string(), z.unknown()),
  }),
  outputSchema: z.object({
    logId: z.string().uuid(),
    success: z.boolean(),
  }),
  execute: async ({ context }) => {
    const logId = crypto.randomUUID();
    // Insert into audit_logs table via @agent-resolv/db
    return { logId, success: true };
  },
});
```

### profileValidatorTool

```typescript
// packages/agents/src/tools/compliance/profile-validator.ts
export const profileValidatorTool = createTool({
  id: 'profile-validator',
  description: 'Validate that a CustomerProfile has all required fields for requesting quotes.',
  inputSchema: z.object({
    profile: customerProfileSchema,
    productType: z.enum(['auto', 'home', 'life', 'health']),
  }),
  outputSchema: z.object({
    valid: z.boolean(),
    missingFields: z.array(z.string()),
    warnings: z.array(z.string()),
  }),
  execute: async ({ context }) => {
    const { profile, productType } = context;
    const missingFields: string[] = [];
    const warnings: string[] = [];

    if (productType === 'auto' && !profile.driverHistory?.bonusMalus) {
      warnings.push('Bonus-malus nao fornecido — cotacoes podem variar');
    }
    // ... additional validations per product type

    return { valid: missingFields.length === 0, missingFields, warnings };
  },
});
```

**Test:** Valid profile passes. Profile missing required field fails with specific field name. Warnings generated for optional but impactful missing fields.

## Task 6: Provider Registry & Email Tools

### Provider Registry

```typescript
// packages/agents/src/tools/providers/registry.ts
interface Provider {
  id: string;
  name: string;
  quoteEmail: string;
  products: ('auto' | 'home' | 'life' | 'health')[];
  integrationTier: 'email' | 'api' | 'middleware';
  responseTimeHours: number;
  emailTemplate: string;
}

export const providerRegistry: Provider[] = [
  {
    id: 'fidelidade',
    name: 'Fidelidade',
    quoteEmail: '', // To be filled from Rolando sessions
    products: ['auto', 'home', 'life', 'health'],
    integrationTier: 'email',
    responseTimeHours: 48,
    emailTemplate: 'standard',
  },
  // ... Tranquilidade, Allianz, Ageas, Generali, Lusitania, Caravela
];
```

### emailQuoteRequestTool

Composes and sends professional Portuguese business email with customer profile data and quote request. Uses Resend API. Every outbound email includes a unique **correlation token** (e.g., `REF-AR-XXXX`) in the subject line and body for deterministic inbound insurer response matching. (Broker reply matching uses a different strategy -- alias-first via per-request reply-to address. See Task 12.)

Input: provider ID, CustomerProfile, productType, notes.
Output: requestId, correlationToken, status, sentAt, emailThreadId.

### emailResponseParserTool

Parses inbound provider email + PDF attachment into NormalizedQuote.

Pipeline:
1. Extract text from PDF attachment
2. LLM structured extraction with provider-specific prompt context
3. Zod schema validation against NormalizedQuote
4. Confidence scoring (0-1) based on extraction certainty
5. Flag ambiguous fields in parsingNotes

**Test:** Mock email with PDF attachment. Verify NormalizedQuote output matches expected values. Test with real PDFs from research samples.

## Task 7: Quote Comparison Agent

```typescript
// packages/agents/src/agents/quote-comparison.ts
export const quoteComparisonAgent = new Agent({
  id: 'quote-comparison',
  model: anthropic('claude-sonnet-4-5-20250514'),
  instructions: `You are a quote comparison specialist for a licensed Portuguese insurance broker.

INPUT: An array of NormalizedQuote objects and the original CustomerProfile.

COMPARISON CRITERIA (weighted):
- Coverage adequacy for customer needs: 40%
- Price/value ratio: 30%
- Provider reputation and service quality: 15%
- Additional benefits and extras: 15%

OUTPUT: A ComparisonResult with:
- bestValue, bestCoverage, cheapest quote IDs
- reasoning in European Portuguese (pt-PT), customer-facing language
- comparisonMatrix mapping each coverage type to per-provider data

COMPLIANCE:
- Disclose that this comparison is AI-generated.
- Note that the recommendation will be reviewed by a qualified director.
- Never overstate provider quality claims.
- Be specific about coverage gaps.`,
  structuredOutput: {
    schema: comparisonResultSchema,
  },
});
```

**Test:** 3 mock NormalizedQuotes for the same auto profile. Verify valid ComparisonResult, different quotes recommended for bestValue vs cheapest, reasoning in Portuguese, coverage gaps identified.

## Task 8: Brokerage Workflow (@mastra/inngest)

The core 6-step pipeline, now durable via `@mastra/inngest`. Every step is crash-safe and independently retriable.

**Health product gate (future):** When health insurance is added, the workflow must include a GDPR Art. 9 consent verification step before `validate-profile`. Health data extraction, storage, and LLM processing are blocked until `gdprConsent.explicitHealthDataConsent === true` is verified. This is not needed for MVP but the step insertion point is here, before step 1.

```typescript
// packages/agents/src/workflows/insurance-quote.ts
import { init } from '@mastra/inngest';
import { Inngest } from 'inngest';
import { Step } from '@mastra/core/workflows';
import { z } from 'zod';

// Inngest client (shared across apps/engine and packages/agents)
export const inngest = new Inngest({ id: 'agent-resolv' });

const { createWorkflow, InngestWorkflow } = init(inngest);

const validateProfile = new Step({
  id: 'validate-profile',
  inputSchema: z.object({ profile: customerProfileSchema }),
  outputSchema: z.object({ valid: z.boolean(), issues: z.array(z.string()) }),
  execute: async ({ context }) => {
    // Call profileValidatorTool
    // Return valid/invalid with details
  },
});

const requestQuotes = new Step({
  id: 'request-quotes',
  execute: async ({ context }) => {
    // For each provider in request.providers:
    //   Call emailQuoteRequestTool
    //   Log via auditLogTool (quote_requested)
    // Return array of { providerId, requestId, status }
  },
});

const collectResponses = new Step({
  id: 'collect-responses',
  suspendSchema: z.object({ reason: z.literal('waiting_for_providers') }),
  resumeSchema: z.object({ quotes: z.array(normalizedQuoteSchema) }),
  // SUSPEND POINT 1
  // Inngest function completes. Snapshot stored in Postgres.
  // Resumes via new Inngest event when inbound emails arrive.
});

const compareQuotes = new Step({
  id: 'compare-quotes',
  execute: async ({ context }) => {
    // Call quoteComparisonAgent with collected quotes + profile
    // Log via auditLogTool (comparison_generated)
    // Return ComparisonResult
  },
});

const humanReview = new Step({
  id: 'human-review',
  suspendSchema: z.object({ reason: z.literal('director_review_required') }),
  resumeSchema: z.object({
    approved: z.boolean(),
    selectedQuoteId: z.string().uuid().optional(),
    reviewerNotes: z.string().optional(),
    reviewerId: z.string(), // Better Auth user ID (format depends on BA config), verified by RBAC middleware
  }),
  // SUSPEND POINT 2 — EU AI Act Article 14 compliance
  // Reviewer identity is authenticated via Better Auth, not a free-text field.
  // Inngest run history = tamper-evident audit trail
});

const sendRecommendation = new Step({
  id: 'send-recommendation',
  execute: async ({ context }) => {
    // Compose recommendation email in Portuguese
    // Send to customer via Resend
    // Log via auditLogTool (recommendation_made)
  },
});

export const insuranceQuoteWorkflow = createWorkflow({
  id: 'broker-pipeline',
  concurrency: { limit: 10 },
})
  .then(validateProfile)
  .then(requestQuotes)
  .then(collectResponses)
  .then(compareQuotes)
  .then(humanReview)
  .then(sendRecommendation);

insuranceQuoteWorkflow.commit();
```

**Test:**
- Full workflow with mock data: profile -> quotes -> comparison -> review -> recommendation
- Suspend at step 3, resume with mock NormalizedQuotes (verify Inngest event fired)
- Suspend at step 5, resume with approval
- Suspend at step 5, resume with rejection + notes -> verify re-comparison
- Partial comparison: only 3 of 5 providers responded before timeout
- Crash recovery: kill process during step 4, restart, verify replay from step 4

## Task 9: Orchestrator Agent Network

```typescript
// packages/agents/src/agents/orchestrator.ts
export const orchestratorAgent = new Agent({
  id: 'orchestrator',
  model: anthropic('claude-sonnet-4-5-20250514'),
  instructions: `You are the main routing agent for agent-resolv, an AI back-office for a Portuguese insurance broker.

You receive inputs from the broker (not from end customers). Route broker inputs to the appropriate sub-agent or workflow:
- New lead (forwarded email, form submission, pasted notes) -> Lead Processing Agent
- Profile complete, ready for quotes -> Insurance Quote Workflow
- Has quotes, needs comparison -> Quote Comparison Agent
- Broker question about a case or insurance topic -> Answer directly in Portuguese
- Edge case or complex product -> Flag for human specialist

Always respond in European Portuguese (pt-PT).`,
  agents: { leadProcessorAgent, quoteComparisonAgent },
  workflows: { insuranceQuoteWorkflow },
  tools: { auditLogTool, profileValidatorTool },
});
```

**Test with `.network()`:**
- Forwarded customer email about auto insurance -> routes to leadProcessorAgent
- `'O perfil do cliente esta completo, pode pedir cotacoes?'` -> routes to workflow
- `'Qual e a diferenca entre RC e danos proprios?'` -> answers directly
- `'Este cliente tem uma quinta com uso misto'` -> flags for human specialist

## Task 10: Mastra Instance + API Server

### Mastra Instance (packages/agents/src/index.ts)

```typescript
import { Mastra } from '@mastra/core';
import { PostgresStore } from '@mastra/pg';
import { PinoLogger } from '@mastra/loggers';

import { leadProcessorAgent } from './agents/lead-processor';
import { quoteComparisonAgent } from './agents/quote-comparison';
import { orchestratorAgent } from './agents/orchestrator';
import { insuranceQuoteWorkflow } from './workflows/insurance-quote';

export const mastra = new Mastra({
  agents: {
    leadProcessor: leadProcessorAgent,
    quoteComparison: quoteComparisonAgent,
    orchestrator: orchestratorAgent,
  },
  workflows: {
    insuranceQuote: insuranceQuoteWorkflow,
  },
  storage: new PostgresStore({
    connectionString: process.env.DATABASE_URL!,
    schemaName: 'mastra',  // separate from app schema
  }),
  logger: new PinoLogger({ level: 'info' }),
  bundler: {
    transpilePackages: ['@agent-resolv/contracts'],
  },
});
```

### Hono API Server (apps/api/src/index.ts)

```typescript
import { Hono } from 'hono';
import { serve } from '@hono/node-server';
import { MastraServer } from '@mastra/hono';
import { mastra } from '@agent-resolv/agents';
import { inngest } from '@agent-resolv/agents/workflows/insurance-quote';

const app = new Hono();

// --- Auth middleware (default-deny) ---
// All routes except /health and /webhooks/* require a valid Better Auth session.
// Mastra auto-exposes agents, workflows, and tools at /api/* — these MUST be behind auth
// to prevent unauthorized access to customer data and workflow actions.
app.use('/api/*', async (c, next) => {
  // Verify Better Auth session from cookie or Authorization header
  // Reject with 401 if no valid session
  // Attach session.user to context for downstream role checks
  await next();
});

// Mount Mastra (auto-exposes agents, workflows, tools at /api/*)
const mastraServer = new MastraServer({ app, mastra });
await mastraServer.init();

// Public routes (no auth required)
app.get('/health', (c) => c.json({ status: 'ok' }));

// Webhook routes (no auth — verified by webhook signature)
app.post('/webhooks/inbound-email', async (c) => {
  // 1. Verify INBOUND_EMAIL_WEBHOOK_SECRET (HMAC signature from Resend)
  // 2. Deduplicate by Message-ID header
  // 3. Inspect recipient address to determine event type:
  const recipientAddress = ''; // extracted from webhook payload
  if (recipientAddress.endsWith('@reply.agent-resolv.com')) {
    // Broker reply -> dispatch to handleBrokerReply (Task 12)
    await inngest.send({ name: 'email/broker-reply.received', data: { /* payload */ } });
  } else {
    // Insurer response -> dispatch to handleInboundEmail (Task 11)
    await inngest.send({ name: 'email/inbound.received', data: { /* payload */ } });
  }
  return c.json({ received: true });
});

// Action page routes (Task 12: authenticated action links)
import { actionRoutes } from './routes/action';
app.route('/', actionRoutes); // mounts GET/POST /action?t=<token>

serve({ fetch: app.fetch, port: 3000 });
```

**scripts (apps/api/package.json):**
```json
{
  "scripts": {
    "dev": "tsx watch src/index.ts",
    "build": "tsc",
    "start": "node dist/index.js"
  }
}
```

**Test:** `pnpm --filter api dev` starts server at localhost:3000. `/health` returns 200 (public). Mastra agent routes at `/api/*` return 401 without valid session, 200 with valid session. `pnpm --filter agents dev` starts Mastra playground independently.

## Task 11: Inngest Engine (apps/engine)

The background job worker that hosts Inngest functions.

```typescript
// apps/engine/src/index.ts
import { Hono } from 'hono';
import { serve as honoServe } from '@hono/node-server';
import { serve as inngestServe } from 'inngest/hono';
import { inngest } from '@agent-resolv/agents/workflows/insurance-quote';

// Import all Inngest functions
import { handleInboundEmail } from './functions/handle-inbound-email';
import { handleBrokerReply } from './functions/handle-broker-reply'; // Task 12: broker reply parsing
import { emailTimeoutChecker } from './functions/email-timeout-checker';
import { auditLogCleanup } from './functions/audit-log-cleanup';

const app = new Hono();

// Mount Inngest serve endpoint
app.on(['GET', 'POST', 'PUT'], '/api/inngest',
  inngestServe({ client: inngest, functions: [
    handleInboundEmail,    // insurer quote responses (Task 11)
    handleBrokerReply,     // broker email replies (Task 12)
    emailTimeoutChecker,
    auditLogCleanup,
  ]})
);

honoServe({ fetch: app.fetch, port: 3001 });
```

### Inbound Email Handler

```typescript
// apps/engine/src/functions/handle-inbound-email.ts
export const handleInboundEmail = inngest.createFunction(
  { id: 'handle-inbound-email', retries: 3 },
  { event: 'email/inbound.received' },
  async ({ event, step }) => {
    // 1. Match insurer email to pending quote request (correlation-token-first)
    //    NOTE: This handles insurer responses only. Broker reply matching
    //    uses alias-first strategy (Task 12: handleBrokerReply).
    const match = await step.run('match-to-request', async () => {
      // Primary: correlation token (REF-AR-XXXX) in subject/body -> deterministic match
      // Fallback: subject fuzzy match, sender domain lookup, LLM extraction, timestamp window
    });

    // 2. Extract and parse PDF attachments
    const quote = await step.run('parse-pdf', async () => {
      // Call emailResponseParserTool -> NormalizedQuote
    });

    // 3. Store NormalizedQuote
    await step.run('store-quote', async () => {
      // Insert into normalized_quotes table
    });

    // 4. Check completion and resume workflow if ready
    await step.run('check-completion', async () => {
      // If all providers responded or timeout met:
      // workflow.resume(workflowRunId, { quotes: collectedQuotes })
    });
  }
);
```

**Test:** Mock inbound email -> correctly matched to pending request -> PDF parsed -> NormalizedQuote stored -> workflow resumed.

## Task 12: Email Broker Interface

> **Scope trap warning:** This task contains five distinct subsystems: (1) outbound notification emails, (2) authenticated action link infrastructure (token generation/storage/expiry/consumption/RBAC), (3) reply-to alias routing, (4) LLM-based reply parsing, (5) token lifecycle management (expiry recovery, rate-limited reissue). If Phase 2 is slipping, **cut reply parsing (subtasks 12c-12d below) to Phase 3** — action links alone are sufficient for MVP. Brokers can confirm, approve, and cancel via links; profile edits can happen through a re-submission flow instead of email replies. Estimated effort: 5-8 days (not the 2 days originally allocated).

The email broker interface is agent-resolv's primary broker-facing channel for Phase 2. Brokers forward leads to an intake address, receive extracted profiles and comparisons via email, and take all state-changing actions via authenticated action links. Email replies handle draft edits and questions only.

### Outbound Notification Emails

```typescript
// packages/agents/src/tools/email/broker-notification.ts
import { createTool } from '@mastra/core/tools';
import { z } from 'zod';

export const brokerNotificationTool = createTool({
  id: 'broker-notification',
  description: 'Send a workflow notification email to the broker with action links.',
  inputSchema: z.object({
    quoteRequestId: z.string().uuid(),
    brokerId: z.string(),
    notificationType: z.enum([
      'profile_confirmation',
      'quotes_sent',
      'response_progress',
      'comparison_ready',
      'recommendation_ready',
    ]),
    data: z.record(z.string(), z.unknown()),
  }),
  outputSchema: z.object({
    emailId: z.string(),
    actionTokens: z.array(z.object({
      action: z.string(),
      token: z.string(),
    })),
    sentAt: z.string().datetime(),
  }),
  execute: async ({ context }) => {
    const { quoteRequestId, brokerId, notificationType, data } = context;

    // 1. Generate opaque action tokens for each CTA in this email
    // 2. Store tokens server-side with 24h TTL, linked to quoteRequestId + action type
    // 3. Compose email with per-request reply-to alias
    // 4. Send via Resend
    // 5. Return email ID and token metadata

    return { emailId: '', actionTokens: [], sentAt: new Date().toISOString() };
  },
});
```

### Authenticated Action Links

```typescript
// apps/api/src/routes/action.ts
import { Hono } from 'hono';

const actionRoutes = new Hono();

// GET /action?t=<token> -- non-destructive, loads the action page
// Safe from email link scanner prefetch (scanners only GET)
actionRoutes.get('/action', async (c) => {
  const token = c.req.query('t');
  if (!token) return c.text('Missing token', 400);

  // 1. Validate token exists and is not expired (24h TTL)
  // 2. If expired/consumed: show "Este link expirou ou ja foi utilizado"
  //    with "Reenviar link" button (rate-limited: max 3 per quote request per hour)
  // 3. If valid: require Better Auth login (redirect to login if no session)
  // 4. After auth: render the action page (confirm/cancel/approve/reject)
  //    based on the token's action type (resolved server-side from token)

  return c.html('<!-- action page -->');
});

// POST /action?t=<token> -- consumes the token, state-changing
actionRoutes.post('/action', async (c) => {
  const token = c.req.query('t');
  if (!token) return c.text('Missing token', 400);

  // 1. Validate token exists, not expired, not already consumed
  // 2. Verify Better Auth session (reject if not authenticated)
  // 3. Verify user has required role (e.g., qualified_director for approvals)
  // 4. Consume the token (mark as used, single-use)
  // 5. Execute the action: update workflow state via Inngest event
  //    - profile_confirmation -> start quote requests
  //    - comparison_trigger -> generate comparison from available quotes
  //    - cancellation -> terminate pipeline
  //    - approval/rejection -> resume human-review suspend point
  // 6. Log to audit trail (authenticated user ID, action, timestamp)
  // 7. Show confirmation page

  return c.html('<!-- confirmation -->');
});

export { actionRoutes };
```

### Reply-Based Draft Edits

```typescript
// apps/engine/src/functions/handle-broker-reply.ts
import { inngest } from '@agent-resolv/agents/workflows/insurance-quote';

export const handleBrokerReply = inngest.createFunction(
  { id: 'handle-broker-reply', retries: 3 },
  { event: 'email/broker-reply.received' },
  async ({ event, step }) => {
    // 1. Match reply to quote request via per-request reply-to alias (primary)
    //    Fallback: Message-ID/In-Reply-To headers, subject token [AR-XXXX]
    //    If all fail: quarantine for manual review (never auto-route fuzzy matches)
    const requestMatch = await step.run('match-to-request', async () => {
      // Primary: reply-to alias lookup (<random>@reply.agent-resolv.com) -> quoteRequestId
      // Secondary: In-Reply-To / References headers
      // Tertiary: subject token [AR-XXXX]
      // Fallback: LLM extraction + quarantine (human-reviewed)
    });

    // 2. Verify sender against broker's registered addresses
    const senderVerified = await step.run('verify-sender', async () => {
      // Check From address against broker's allowlist
      // Check SPF/DKIM/DMARC results from webhook payload
      // Apply disposition matrix (see strategy doc Section 8)
    });

    // 3. Parse reply intent via LLM
    const parsed = await step.run('parse-reply', async () => {
      // LLM parses: is this a profile edit, a question, or unclear?
      // Profile edits: extract field-level changes
      // Questions: extract question text for logging
      // Unclear: send clarification email
    });

    // 4. Apply changes (draft-level only -- no state changes from email)
    await step.run('apply-reply', async () => {
      // If profile edit: update draft profile, send confirmation email
      // If question: log and forward to case thread
      // If unclear: send clarification email
      // NEVER change workflow state from an email reply
    });
  }
);
```

### Per-Request Reply-To Alias Matching

```typescript
// packages/core/src/email-matching/request-alias.ts

// Each quote request gets one unique high-entropy reply-to address
// Format: <random-alphanumeric>@reply.agent-resolv.com
// Multiple emails for the same request (reminder, comparison) reuse the same alias
// High entropy prevents enumeration and accidental cross-request routing

export function generateReplyToAlias(): string {
  // Generate 10-char alphanumeric (a-z, 0-9) = ~52 bits of entropy
  // Uses crypto.getRandomValues for security-sensitive routing aliases
  const chars = 'abcdefghijklmnopqrstuvwxyz0123456789';
  const bytes = crypto.getRandomValues(new Uint8Array(10));
  const alias = Array.from(bytes, (b) => chars[b % chars.length]).join('');
  return `${alias}@reply.agent-resolv.com`;
}

// Lookup: inbound email to reply-to alias -> quoteRequestId
// Stored in DB: reply_to_aliases(alias, quote_request_id, created_at)
```

### Action Token Model

```typescript
// packages/db/src/schema.ts (additions)
import { pgTable, uuid, varchar, timestamp, boolean } from 'drizzle-orm/pg-core';

export const actionTokens = pgTable('action_tokens', {
  id: uuid('id').primaryKey().defaultRandom(),
  token: varchar('token', { length: 64 }).notNull().unique(),
  quoteRequestId: uuid('quote_request_id').references(() => quoteRequests.id, { onDelete: 'cascade' }).notNull(),
  actionType: varchar('action_type', { length: 50 }).notNull(),
  // e.g., 'profile_confirmation', 'comparison_trigger', 'cancellation', 'approval'
  consumed: boolean('consumed').default(false).notNull(),
  consumedAt: timestamp('consumed_at'),
  consumedByUserId: varchar('consumed_by_user_id', { length: 64 }),
  expiresAt: timestamp('expires_at').notNull(), // created_at + 24h
  createdAt: timestamp('created_at').defaultNow().notNull(),
});

export const replyToAliases = pgTable('reply_to_aliases', {
  id: uuid('id').primaryKey().defaultRandom(),
  alias: varchar('alias', { length: 64 }).notNull().unique(),
  quoteRequestId: uuid('quote_request_id').references(() => quoteRequests.id, { onDelete: 'cascade' }).notNull(),
  createdAt: timestamp('created_at').defaultNow().notNull(),
});
```

**Test:**
- **Rolando pilot gate:** Rolando completes 5 end-to-end cases via email (forward lead, confirm profile via action link, receive comparison, approve via action link)
- Outbound email: broker notification with action links sent for each notification type
- Action link GET: renders action page, does not consume token (safe from scanner prefetch)
- Action link POST: consumes token, updates workflow state, logged to audit trail
- Expired token: shows expiry message with reissue button
- Reply parsing: 10 broker email replies (edits + questions), measure correct parse rate >80%
- Request matching: inbound reply routed to correct request via reply-to alias
- Sender verification: emails from unregistered addresses are quarantined

## Task 13: MCP Server (Phase 3)

The MCP server wraps the Mastra agents and workflow as MCP tools. This task is deferred to Phase 3 -- the pipeline and email broker interface are validated first.

```typescript
// apps/mcp/src/index.ts
import { MCPServer } from '@mastra/mcp';
import { z } from 'zod';
import { mastra } from '@agent-resolv/agents';
import { providerRegistry } from '@agent-resolv/agents/tools/providers/registry';

export const mcpServer = new MCPServer({
  name: 'agent-resolv',
  version: '0.1.0',
  tools: {
    process_lead: {
      description: 'Extract a structured customer profile from unstructured input.',
      inputSchema: z.object({
        input: z.string().describe('Raw lead content: email body, form data, or phone notes'),
        inputType: z.enum(['email', 'contact_form', 'phone_notes', 'document']).optional(),
      }),
      execute: async ({ input, inputType }) => {
        // Call leadProcessorAgent
      },
    },

    submit_quote_request: {
      description: 'Validate a customer profile and send quote requests to selected insurers.',
      inputSchema: z.object({
        customerId: z.string().uuid(),
        providerIds: z.array(z.string()).optional(),
        notes: z.string().optional(),
      }),
      execute: async ({ customerId, providerIds, notes }) => {
        // Start insuranceQuoteWorkflow via Inngest
      },
    },

    check_quote_status: {
      description: 'Check which insurers have responded and the current workflow state.',
      inputSchema: z.object({ workflowRunId: z.string().uuid() }),
      execute: async ({ workflowRunId }) => {
        // Query workflow state
      },
    },

    get_comparison: {
      description: 'Get the AI-generated quote comparison for a completed request.',
      inputSchema: z.object({ workflowRunId: z.string().uuid() }),
      execute: async ({ workflowRunId }) => {
        // Return ComparisonResult
      },
    },

    approve_recommendation: {
      description: 'Qualified director approves or rejects an AI-generated recommendation. Requires authenticated user with qualified_director role.',
      inputSchema: z.object({
        workflowRunId: z.string().uuid(),
        approved: z.boolean(),
        selectedQuoteId: z.string().uuid().optional(),
        reviewerNotes: z.string().optional(),
      }),
      execute: async (input, { session }) => {
        // 1. Verify session.user exists and has qualified_director role
        //    (Better Auth session from auth middleware)
        // 2. Reject with 403 if role check fails
        // 3. Use session.user.id as reviewerId (not a free-text name)
        // 4. Call workflow.resume() on human-review step
      },
    },

    request_insurance_quote: {
      description: 'Request insurance quotes by providing your details (consumer-facing).',
      inputSchema: z.object({
        details: z.string().describe('Describe your insurance needs'),
        productType: z.enum(['auto', 'home', 'life', 'health']).optional(),
      }),
      execute: async ({ details, productType }) => {
        // 1. Extract profile via leadProcessorAgent
        // 2. If complete, start workflow
        // 3. If not, return missing fields
      },
    },

    list_providers: {
      description: 'List available insurance providers and supported products.',
      inputSchema: z.object({
        productType: z.enum(['auto', 'home', 'life', 'health']).optional(),
      }),
      execute: async ({ productType }) => {
        const providers = productType
          ? providerRegistry.filter(p => p.products.includes(productType))
          : providerRegistry;
        return providers.map(p => ({
          id: p.id, name: p.name, products: p.products,
          expectedResponseTime: `${p.responseTimeHours}h`,
        }));
      },
    },
  },
});
```

**Claude Desktop configuration** (`claude_desktop_config.json`):
```json
{
  "mcpServers": {
    "agent-resolv": {
      "command": "npx",
      "args": ["tsx", "/path/to/agent-resolv/apps/mcp/src/index.ts"]
    }
  }
}
```

**Test:**
- Broker flow: "I have a new lead, here's the email: [paste]" -> `process_lead` called
- Broker flow: "Submit quote request to Fidelidade and Allianz" -> `submit_quote_request` called
- Consumer flow: "I need auto insurance, plate AA-00-BB, age 35" -> `request_insurance_quote` called
- Status check -> `check_quote_status` returns current state
- List providers -> filtered list returned

## Definition of Done -- Phase Gates

**Phase 1 Complete When:**
- [ ] Monorepo builds and passes lint (`pnpm build`, `pnpm lint`)
- [ ] All Zod schemas pass validation with valid/invalid test data
- [ ] Drizzle schema pushes to Neon, CRUD operations work, FK cascades verified
- [ ] Better Auth: user creation, role assignment, session management verified
- [ ] Lead processing agent: 90%+ field extraction from 20 test inputs
- [ ] Audit log records all lead processing actions
- [ ] Maps to: Roadmap Phase 1 Decision Gate

**Phase 2 Complete When:**
- [ ] Full workflow completes end-to-end with mock provider data
- [ ] PDF parsing >90% accuracy on test corpus from Rolando
- [ ] Rolando rates recommendation quality as "acceptable" or better
- [ ] Email broker interface: Rolando completes 5 end-to-end cases
- [ ] Inbound email matching >99% on test data, uncertain matches quarantined
- [ ] All GDPR legal gates met (legal basis, DPA, retention policy, encryption)
- [ ] Maps to: Roadmap Phase 2 Decision Gate

**Phase 3 Complete When:**
- [ ] Broker dashboard: Rolando completes 5 reviews in <3 min each
- [ ] MCP server: broker + consumer flows work from Claude Desktop
- [ ] PDF parsing: per-field confidence scores, golden dataset started
- [ ] Observability: key metrics tracked in PostHog
- [ ] Maps to: Roadmap Phase 3 launch criteria

## Testing Strategy Summary

### Unit Tests (per task)

| Task                         | What to Test                                                       |
| ---------------------------- | ------------------------------------------------------------------ |
| 2 -- Schemas                 | Valid/invalid inputs, boundary values, discriminated union routing |
| 3 -- Database                | Drizzle schema push, insert/query operations, FK constraints       |
| 4 -- Lead Processor          | Field extraction accuracy from diverse input types                 |
| 5 -- Compliance              | Audit log creation, profile validation with missing fields         |
| 6 -- Email Tools             | Email composition format, PDF parsing accuracy                     |
| 7 -- Comparison Agent        | ComparisonResult validity, reasoning coherence                     |
| 8 -- Workflow                | Step transitions, suspend/resume via Inngest, crash recovery       |
| 9 -- Orchestrator            | Routing accuracy for different message types                       |
| 11 -- Inngest Engine         | Inbound email matching, PDF extraction, workflow resume            |
| 12 -- Email Broker Interface | Action link token lifecycle, reply parsing, request alias matching |
| 13 -- MCP Server (Phase 3)   | Tool invocation, broker and consumer flows                         |

### Integration Tests

| Test                                              | What it Covers        |
| ------------------------------------------------- | --------------------- |
| Full lead ingestion -> valid profile              | Tasks 2, 3, 4, 5, 10  |
| Workflow end-to-end (mock providers)              | Tasks 6, 7, 8, 10, 11 |
| Suspend -> Inngest event -> resume -> completion  | Tasks 8, 11           |
| Multi-provider partial response                   | Tasks 8, 11           |
| Email broker flow end-to-end                      | Tasks 4, 8, 11, 12    |
| Crash recovery (kill mid-workflow, verify replay) | Task 8                |
| MCP broker flow end-to-end (Phase 3)              | Tasks 4, 8, 13        |
| MCP consumer flow end-to-end (Phase 3)            | Tasks 4, 6, 8, 13     |

### Manual Testing

- Mastra playground (`pnpm --filter agents dev` at localhost:4111) for lead processing with sample broker inputs
- Inngest dev dashboard (localhost:8288) for workflow execution visualization, step-level inspection, retry history
- Tool invocation testing with real PDF samples from `/research/ai-spikes/samples/`
- Email broker interface: forward a test lead to intake address, verify notification emails, test action links (profile confirmation, approval), test reply parsing with sample broker emails
- MCP server testing via Claude Desktop (Phase 3)
- Test with real forwarded emails and contact form data from Rolando's workflow

## Notes for Claude Code

- Always use Zod schemas from `@agent-resolv/contracts` -- never `any` or untyped objects
- Broker-facing text in European Portuguese (pt-PT) -- the broker is the user
- Use `crypto.randomUUID()` for all ID generation
- Every tool must have clear `description` fields -- these drive agent routing quality
- Email templates must be realistic enough for PT insurance providers
- Log every significant action through auditLogTool -- regulatory requirement
- Pin Mastra dependency versions with `save-exact=true` in `.npmrc`
- Test workflow durability: kill the process mid-step, restart, verify Inngest replays correctly
- `packages/contracts/` is the core IP -- invest time in getting schemas right
- `packages/agents/` uses `transpilePackages` for workspace imports -- see ADR-005
- The Inngest dev server must run alongside the API for workflow testing (`npx inngest-cli dev`)

---

*Original implementation guide: [../research/mastra/agent-resolv-claude-code-guide.md](../research/mastra/agent-resolv-claude-code-guide.md)*
*Technical roadmap: [../research/mastra/agent-resolv-technical-roadmap.md](../research/mastra/agent-resolv-technical-roadmap.md)*
