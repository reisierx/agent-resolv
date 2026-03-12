# agent-resolv — Technical Roadmap & Implementation Guide

## Document Purpose

This document serves as the technical specification and phased roadmap for building the agent-resolv MVP using the Mastra AI agent framework. It is designed to be fed into Claude Code or similar AI coding tools to guide implementation of POCs and incremental feature development.

**Company**: agent-resolv (operating entity TBD)
**Product**: agent-resolv — AI-native licensed insurance broker for Portugal  
**Framework**: Mastra (TypeScript AI agent framework)  
**Stack**: Next.js, Turborepo, Vercel, TypeScript, Better Auth, PostHog  
**Stage**: Pre-seed, pre-revenue, pre-license — technical feasibility phase

---

## Architecture Overview

### Core Principle

agent-resolv is a multi-agent system that agentifies the insurance brokerage workflow. The architecture follows a layered approach where customer-facing AI agents orchestrate deterministic workflows, which in turn call provider integration tools through a standardized schema layer.

### System Layers

```
┌─────────────────────────────────────────────────────┐
│  CHANNEL LAYER (Email only for MVP)                 │
│  Thin transport — routes messages to agents          │
├─────────────────────────────────────────────────────┤
│  AGENT LAYER (Mastra Agents + Agent Network)        │
│  LLM-powered routing, conversation, recommendations │
├─────────────────────────────────────────────────────┤
│  WORKFLOW LAYER (Mastra Workflows)                  │
│  Deterministic brokerage pipeline with HITL          │
├─────────────────────────────────────────────────────┤
│  TOOL LAYER (Mastra Tools / MCP)                    │
│  Provider integrations behind standardized schemas   │
├─────────────────────────────────────────────────────┤
│  SCHEMA LAYER (Zod — the core IP)                   │
│  CustomerProfile, QuoteRequest, NormalizedQuote      │
└─────────────────────────────────────────────────────┘
```

### Agent-to-Agent Coordination Strategy

The system uses a **hybrid pattern** combining two Mastra primitives:

- **Agent Network** (`.network()`) as the top-level customer-facing orchestrator. Uses LLM reasoning to interpret customer requests and route to the right sub-agents or workflows dynamically. This handles conversational ambiguity (e.g., "I need insurance for my new house and car").

- **Deterministic Workflows** underneath for actual brokerage steps. These follow predefined execution graphs with `.then()`, `.branch()`, `.parallel()`, and `suspend()`/`resume()` for human-in-the-loop. This is auditable and ASF-compliant.

```
Customer message
  → Agent Network (LLM routes dynamically)
    → autoInsuranceWorkflow (deterministic)
      → tools (provider integrations)
      → suspend() for human approval
    → or homeInsuranceWorkflow (deterministic)
    → or intakeAgent (profiling conversation)
```

### MCP & Provider Integration Strategy

Every provider integration sits behind a **standardized tool interface**. The agent never knows whether a tool uses email, a direct API, or middleware underneath. This enables progressive enhancement:

- **Tier 1 — Email** (MVP): Compose structured quote request → send via SMTP → parse response PDF
- **Tier 2 — Direct API**: Replace email with provider-specific API call. Same tool interface.
- **Tier 3 — Middleware** (i2S, lluni): One tool fans out to multiple providers via gateway API.

Start with plain Mastra tools at MVP. Wrap them as MCP servers later when integrations need to be reusable outside the Mastra instance.

---

## Standardized Schema Layer (Core IP)

These schemas are the normalization layer that makes plug-and-play provider integration possible. **Every tool takes `CustomerProfile` as input and returns `NormalizedQuote` as output**, regardless of the underlying transport.

### CustomerProfile Schema

```typescript
import { z } from 'zod';

// Base profile shared across all product types
const BaseProfile = z.object({
  id: z.string().uuid(),
  nif: z.string().regex(/^\d{9}$/),
  name: z.string(),
  email: z.string().email(),
  phone: z.string().optional(),
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

// Auto insurance specifics
const AutoProfile = BaseProfile.extend({
  productType: z.literal('auto'),
  vehicle: z.object({
    make: z.string(),
    model: z.string(),
    year: z.number(),
    licensePlate: z.string(),
    fuelType: z.enum(['gasoline', 'diesel', 'electric', 'hybrid', 'lpg']),
    usage: z.enum(['personal', 'professional', 'mixed']),
    parkingType: z.enum(['garage', 'street', 'parking_lot']),
    estimatedKmYear: z.number().optional(),
  }),
  driverHistory: z.object({
    licenseDate: z.string().date(),
    claimsLast5Years: z.number(),
    bonusMalus: z.number().optional(), // Portuguese bonus-malus scale
  }),
  desiredCoverage: z.enum(['minimum', 'third_party_plus', 'all_risk', 'all_risk_premium']),
});

// Home insurance specifics
const HomeProfile = BaseProfile.extend({
  productType: z.literal('home'),
  property: z.object({
    type: z.enum(['apartment', 'house', 'villa', 'studio']),
    ownership: z.enum(['owner', 'tenant', 'landlord']),
    constructionYear: z.number(),
    area: z.number(), // m²
    floors: z.number(),
    capitalEdificio: z.number(), // building reconstruction value
    capitalRecheio: z.number().optional(), // contents value
    hasPool: z.boolean(),
    hasGarden: z.boolean(),
    alarmSystem: z.boolean(),
  }),
  address: z.object({
    street: z.string(),
    postalCode: z.string(),
    city: z.string(),
    district: z.string(),
    parish: z.string().optional(), // freguesia
  }),
});

// Life insurance specifics
const LifeProfile = BaseProfile.extend({
  productType: z.literal('life'),
  health: z.object({
    smoker: z.boolean(),
    preExistingConditions: z.array(z.string()),
    height: z.number(), // cm
    weight: z.number(), // kg
  }),
  coverage: z.object({
    capitalDesired: z.number(),
    duration: z.number(), // years
    linkedToMortgage: z.boolean(),
    mortgageBank: z.string().optional(),
  }),
});

// Health insurance specifics
const HealthProfile = BaseProfile.extend({
  productType: z.literal('health'),
  members: z.array(z.object({
    name: z.string(),
    dateOfBirth: z.string().date(),
    relationship: z.enum(['self', 'spouse', 'child', 'parent']),
  })),
  preferences: z.object({
    dentalCoverage: z.boolean(),
    networkPreference: z.enum(['open', 'network_only', 'no_preference']),
    existingConditions: z.array(z.string()),
  }),
});

// Discriminated union — validates correct fields per product
export const CustomerProfile = z.discriminatedUnion('productType', [
  AutoProfile,
  HomeProfile,
  LifeProfile,
  HealthProfile,
]);

export type CustomerProfile = z.infer<typeof CustomerProfile>;
```

### QuoteRequest Schema

```typescript
export const QuoteRequest = z.object({
  id: z.string().uuid(),
  customerId: z.string().uuid(),
  productType: z.enum(['auto', 'home', 'life', 'health']),
  profile: CustomerProfile,
  providers: z.array(z.string()), // which providers to request from
  requestedAt: z.string().datetime(),
  urgency: z.enum(['standard', 'urgent']).default('standard'),
  notes: z.string().optional(), // agent notes for the provider
});
```

### NormalizedQuote Schema

```typescript
export const NormalizedQuote = z.object({
  id: z.string().uuid(),
  quoteRequestId: z.string().uuid(),
  provider: z.object({
    id: z.string(),
    name: z.string(),
    logoUrl: z.string().optional(),
  }),
  productType: z.enum(['auto', 'home', 'life', 'health']),
  premium: z.object({
    annual: z.number(),
    monthly: z.number().optional(),
    paymentOptions: z.array(z.object({
      frequency: z.enum(['annual', 'biannual', 'quarterly', 'monthly']),
      amount: z.number(),
      surcharge: z.number().optional(), // fracionamento surcharge %
    })),
  }),
  coverage: z.array(z.object({
    type: z.string(),
    description: z.string(),
    limit: z.number().optional(),
    deductible: z.number().optional(),
    included: z.boolean(),
    optional: z.boolean().default(false),
    optionalPremium: z.number().optional(),
  })),
  exclusions: z.array(z.string()),
  conditions: z.array(z.string()),
  validUntil: z.string().date(),
  rawDocumentUrl: z.string().optional(), // link to original PDF
  receivedAt: z.string().datetime(),
  confidence: z.number().min(0).max(1), // how confident the parser is in extraction accuracy
  parsingNotes: z.array(z.string()).optional(), // any ambiguities during PDF parsing
});

export type NormalizedQuote = z.infer<typeof NormalizedQuote>;
```

### ComparisonResult Schema

```typescript
export const ComparisonResult = z.object({
  quoteRequestId: z.string().uuid(),
  quotes: z.array(NormalizedQuote),
  recommendation: z.object({
    bestValue: z.string(), // quote ID
    bestCoverage: z.string(), // quote ID
    cheapest: z.string(), // quote ID
    reasoning: z.string(), // agent's explanation in Portuguese
  }),
  comparisonMatrix: z.array(z.object({
    coverageType: z.string(),
    byProvider: z.record(z.string(), z.object({
      included: z.boolean(),
      limit: z.number().optional(),
      deductible: z.number().optional(),
    })),
  })),
  generatedAt: z.string().datetime(),
});
```

---

## Phased Implementation Roadmap

The roadmap follows three phases, each delivering standalone value while building toward the full platform. Every phase is designed to be testable independently with POCs before moving to the next.

---

## PHASE 1 — Foundation (Weeks 1-3)

**Goal**: Get a single agent working that can have a conversation, collect customer data, and produce a structured profile. No provider integrations yet. Pure AI feasibility testing.

### 1.1 Project Setup

**POC Target**: Mastra running inside Next.js with the local playground working.

```
agent-resolv/
├── apps/
│   └── web/                      # Next.js frontend
│       └── src/
│           └── mastra/
│               ├── index.ts      # Mastra instance registration
│               ├── agents/
│               ├── workflows/
│               ├── tools/
│               └── schemas/
├── packages/
│   ├── schemas/                  # Shared Zod schemas (from above)
│   └── config/                   # Shared config
├── turbo.json
└── package.json
```

Dependencies:
```bash
npm install @mastra/core @ai-sdk/anthropic @ai-sdk/openai zod
```

Mastra instance:
```typescript
// src/mastra/index.ts
import { Mastra } from '@mastra/core/mastra';
import { PinoLogger } from '@mastra/loggers';
import { LibSQLStore } from '@mastra/libsql';
import { intakeAgent } from './agents/intake';

export const mastra = new Mastra({
  agents: {
    intakeAgent,
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

### 1.2 Intake Agent

**POC Target**: An agent that can have a natural conversation in Portuguese, ask the right questions for insurance profiling, and output a valid `CustomerProfile` as structured output.

```typescript
// src/mastra/agents/intake.ts
import { Agent } from '@mastra/core/agent';
import { anthropic } from '@ai-sdk/anthropic';

export const intakeAgent = new Agent({
  id: 'intake-agent',
  name: 'Agente de Atendimento',
  instructions: `
    You are a licensed insurance broker assistant for agent-resolv, operating in Portugal.
    Your role is to collect customer information needed for insurance quotes.

    LANGUAGE: Always respond in European Portuguese (pt-PT). Use formal "você" address.

    BEHAVIOR:
    - Be warm, professional, and efficient
    - Ask one question at a time — never overwhelm the customer
    - If the customer provides partial info, acknowledge it and ask for what's missing
    - Validate data as you go (e.g., NIF must be 9 digits, license plate format)
    - Explain why you need each piece of information when it's not obvious
    - If the customer is unsure about coverage levels, briefly explain the options

    PRODUCT ROUTING:
    - Auto insurance: Ask about vehicle, driver history, desired coverage
    - Home insurance: Ask about property type, ownership, building/contents values
    - Life insurance: Ask about health basics, coverage amount, mortgage link
    - Health insurance: Ask about family members, preferences

    COMPLIANCE:
    - Always identify yourself as an AI assistant working for a licensed broker
    - Never make promises about pricing or coverage
    - Always note that final quotes come from insurance providers

    OUTPUT:
    - Once you have all required fields for the product type, produce a structured
      CustomerProfile object. Confirm the summary with the customer before finalizing.
  `,
  model: anthropic('claude-sonnet-4-5-20250514'),
});
```

**What to test in POC**:
- Can the agent hold a natural Portuguese conversation?
- Does it ask the right questions for each product type?
- Does it produce valid `CustomerProfile` structured output?
- How does it handle edge cases (customer doesn't know their bonus-malus, gives partial info)?
- Token usage and latency per conversation turn

### 1.3 Compliance Tools

**POC Target**: Basic tools that log interactions and validate completeness.

```typescript
// src/mastra/tools/compliance/audit-log.ts
import { createTool } from '@mastra/core/tools';
import { z } from 'zod';

export const auditLogTool = createTool({
  id: 'audit-log',
  description: 'Logs every agent action for ASF compliance audit trail',
  inputSchema: z.object({
    action: z.enum([
      'customer_intake_started',
      'customer_intake_completed',
      'quote_requested',
      'quote_received',
      'comparison_generated',
      'recommendation_made',
      'human_review_requested',
      'human_review_completed',
      'policy_binding_initiated',
    ]),
    agentId: z.string(),
    customerId: z.string().optional(),
    details: z.record(z.unknown()),
    timestamp: z.string().datetime(),
  }),
  outputSchema: z.object({
    logId: z.string(),
    success: z.boolean(),
  }),
  execute: async ({ context }) => {
    // MVP: Write to database
    // Later: Structured audit log storage meeting ASF requirements
    console.log('[AUDIT]', context.action, context.details);
    return {
      logId: crypto.randomUUID(),
      success: true,
    };
  },
});

// src/mastra/tools/compliance/profile-validator.ts
export const profileValidatorTool = createTool({
  id: 'profile-validator',
  description: 'Validates a CustomerProfile has all required fields for the product type before requesting quotes',
  inputSchema: z.object({
    profile: CustomerProfile,
  }),
  outputSchema: z.object({
    valid: z.boolean(),
    missingFields: z.array(z.string()),
    warnings: z.array(z.string()),
  }),
  execute: async ({ context }) => {
    const { profile } = context;
    const missing: string[] = [];
    const warnings: string[] = [];

    // Product-specific validation
    if (profile.productType === 'auto') {
      if (!profile.driverHistory.bonusMalus) {
        warnings.push('Bonus-malus não fornecido — cotações podem variar');
      }
    }

    return {
      valid: missing.length === 0,
      missingFields: missing,
      warnings,
    };
  },
});
```

### 1.4 Memory Setup

**POC Target**: Persistent customer threads across sessions.

```typescript
// Mastra memory is thread-based
// Each customer gets a persistent thread for conversation continuity

// When a customer returns, retrieve their thread:
const response = await intakeAgent.generate('Olá, quero atualizar o meu seguro', {
  memory: {
    resource: 'customers',
    thread: `customer-${customerId}`, // persistent across sessions
  },
});
```

**What to test in POC**:
- Does the agent remember context from previous sessions?
- Can it pick up an intake conversation that was interrupted?
- Memory token management — does it stay within context limits on long threads?

### Phase 1 Deliverables

- [ ] Mastra instance running in Next.js with local playground
- [ ] Intake agent that profiles customers in Portuguese
- [ ] Validated `CustomerProfile` structured output for auto insurance
- [ ] Basic audit logging tool
- [ ] Persistent memory per customer thread
- [ ] Latency and token usage benchmarks

---

## PHASE 2 — Email-Based Brokerage Pipeline (Weeks 4-7)

**Goal**: End-to-end brokerage workflow using email as the provider integration channel. Customer submits profile → agent composes quote request emails → sends to providers → parses responses → generates comparison. Human-in-the-loop for approval.

### 2.1 Email Integration Tools

**POC Target**: Send a structured quote request email and parse a provider response.

```typescript
// src/mastra/tools/providers/email-quote-request.ts
import { createTool } from '@mastra/core/tools';
import { z } from 'zod';

export const emailQuoteRequestTool = createTool({
  id: 'email-quote-request',
  description: `
    Sends a structured insurance quote request to a provider via email.
    Composes professional Portuguese email from CustomerProfile data.
    Used when no direct API integration exists for the provider.
  `,
  inputSchema: z.object({
    provider: z.object({
      id: z.string(),
      name: z.string(),
      quoteEmail: z.string().email(),
    }),
    profile: CustomerProfile,
    productType: z.enum(['auto', 'home', 'life', 'health']),
    notes: z.string().optional(),
  }),
  outputSchema: z.object({
    requestId: z.string(),
    status: z.enum(['sent', 'failed']),
    sentAt: z.string().datetime(),
    emailThreadId: z.string().optional(),
  }),
  execute: async ({ context }) => {
    const { provider, profile, productType, notes } = context;

    // Compose email using a template per product type
    // MVP: Use Resend or Nodemailer
    // Email template maps CustomerProfile fields to provider-expected format

    const requestId = crypto.randomUUID();

    // TODO: Implement email sending
    // - Use product-specific email template
    // - Attach any required documents (NIF copy, vehicle docs)
    // - Track email thread ID for response matching

    return {
      requestId,
      status: 'sent' as const,
      sentAt: new Date().toISOString(),
      emailThreadId: 'thread-placeholder',
    };
  },
});
```

```typescript
// src/mastra/tools/providers/email-response-parser.ts
export const emailResponseParserTool = createTool({
  id: 'email-response-parser',
  description: `
    Parses a provider's quote response email (usually with PDF attachment)
    and extracts structured NormalizedQuote data.
    Uses AI to extract data from unstructured PDF documents.
  `,
  inputSchema: z.object({
    emailId: z.string(),
    providerId: z.string(),
    productType: z.enum(['auto', 'home', 'life', 'health']),
    attachments: z.array(z.object({
      filename: z.string(),
      contentType: z.string(),
      content: z.string(), // base64
    })),
  }),
  outputSchema: NormalizedQuote,
  execute: async ({ context }) => {
    // Step 1: Extract text from PDF attachment
    // Step 2: Use LLM to parse unstructured text into NormalizedQuote
    // Step 3: Validate extracted data against schema
    // Step 4: Flag low-confidence extractions for human review

    // This is where AI adds real value — turning unstructured provider
    // PDFs into standardized comparable data

    // TODO: Implement PDF extraction + LLM parsing
  },
});
```

### 2.2 Provider Registry

```typescript
// src/mastra/tools/providers/registry.ts
export const providerRegistry = {
  fidelidade: {
    id: 'fidelidade',
    name: 'Fidelidade',
    quoteEmail: 'cotacoes@fidelidade.pt', // placeholder
    products: ['auto', 'home', 'life', 'health'],
    integrationTier: 'email', // email | api | middleware
    responseTime: '24-48h',
    emailTemplate: 'fidelidade', // maps to template
  },
  tranquilidade: {
    id: 'tranquilidade',
    name: 'Tranquilidade',
    quoteEmail: 'mediadores@tranquilidade.pt',
    products: ['auto', 'home', 'life'],
    integrationTier: 'email',
    responseTime: '24-48h',
    emailTemplate: 'tranquilidade',
  },
  allianz: {
    id: 'allianz',
    name: 'Allianz',
    quoteEmail: 'cotacoes@allianz.pt',
    products: ['auto', 'home', 'life'],
    integrationTier: 'email',
    responseTime: '24-48h',
    emailTemplate: 'default',
  },
  caravela: {
    id: 'caravela',
    name: 'Caravela Seguros',
    quoteEmail: '', // TBD — Luís Cervantes contact
    products: ['auto', 'home'],
    integrationTier: 'email', // potential fast-track to API via partnership
    responseTime: '24h',
    emailTemplate: 'caravela',
  },
  // Add providers as partnerships are established
} as const;
```

### 2.3 Quote Comparison Agent

```typescript
// src/mastra/agents/quote-comparison.ts
export const quoteComparisonAgent = new Agent({
  id: 'quote-comparison-agent',
  name: 'Agente de Comparação',
  instructions: `
    You are an insurance quote comparison specialist for the Portuguese market.

    ROLE:
    - Analyze normalized quotes from multiple providers
    - Generate clear, objective comparisons
    - Make recommendations based on customer profile and needs
    - Explain trade-offs in simple Portuguese

    COMPARISON CRITERIA (weighted):
    1. Coverage adequacy for customer's specific situation (40%)
    2. Price/value ratio (30%)
    3. Provider reputation and claims handling (15%)
    4. Additional benefits and services (15%)

    OUTPUT FORMAT:
    - Summary table comparing key coverages and prices
    - Top recommendation with reasoning
    - Alternative option and why it might suit specific cases
    - Clear disclosure that this is an AI-generated recommendation
      subject to review by a qualified director

    COMPLIANCE:
    - Never guarantee coverage or pricing
    - Always note that recommendations are subject to qualified director review
    - Document the reasoning for every recommendation (audit trail)
    - Mention any coverage gaps or concerns proactively
  `,
  model: anthropic('claude-sonnet-4-5-20250514'),
});
```

### 2.4 Brokerage Workflow (The Core Pipeline)

```typescript
// src/mastra/workflows/insurance-quote.ts
import { createWorkflow, createStep } from '@mastra/core/workflows';
import { z } from 'zod';

// Step 1: Validate profile completeness
const validateProfile = createStep({
  id: 'validate-profile',
  description: 'Validates customer profile has all required fields',
  inputSchema: z.object({ profile: CustomerProfile }),
  outputSchema: z.object({
    valid: z.boolean(),
    profile: CustomerProfile,
    missingFields: z.array(z.string()),
  }),
  execute: async ({ inputData }) => {
    // Use profileValidatorTool internally
    return {
      valid: true,
      profile: inputData.profile,
      missingFields: [],
    };
  },
});

// Step 2: Fan out quote requests to providers
const requestQuotes = createStep({
  id: 'request-quotes',
  description: 'Sends quote requests to all relevant providers in parallel',
  inputSchema: z.object({
    profile: CustomerProfile,
    providers: z.array(z.string()),
  }),
  outputSchema: z.object({
    requestIds: z.array(z.object({
      providerId: z.string(),
      requestId: z.string(),
      status: z.enum(['sent', 'failed']),
    })),
  }),
  execute: async ({ inputData }) => {
    // For each provider, call emailQuoteRequestTool
    // In future, route to API tool if provider supports it
    // All requests fire in parallel
    return {
      requestIds: inputData.providers.map(p => ({
        providerId: p,
        requestId: crypto.randomUUID(),
        status: 'sent' as const,
      })),
    };
  },
});

// Step 3: Wait for responses (SUSPEND — this is the async email gap)
const collectResponses = createStep({
  id: 'collect-responses',
  description: 'Suspends workflow until provider responses are received',
  inputSchema: z.object({
    requestIds: z.array(z.object({
      providerId: z.string(),
      requestId: z.string(),
    })),
  }),
  outputSchema: z.object({
    quotes: z.array(NormalizedQuote),
  }),
  resumeSchema: z.object({
    quotes: z.array(NormalizedQuote),
  }),
  suspendSchema: z.object({
    reason: z.string(),
    awaitingProviders: z.array(z.string()),
    requestedAt: z.string(),
  }),
  execute: async ({ inputData, resumeData, suspend }) => {
    if (!resumeData) {
      // First execution: suspend and wait for email responses
      return await suspend({
        reason: 'Aguardando respostas dos provedores de seguros',
        awaitingProviders: inputData.requestIds.map(r => r.providerId),
        requestedAt: new Date().toISOString(),
      });
    }
    // Resumed with parsed quotes
    return { quotes: resumeData.quotes };
  },
});

// Step 4: Generate comparison using the comparison agent
const compareQuotes = createStep({
  id: 'compare-quotes',
  description: 'AI agent analyzes and compares all received quotes',
  inputSchema: z.object({
    quotes: z.array(NormalizedQuote),
    profile: CustomerProfile,
  }),
  outputSchema: ComparisonResult,
  execute: async ({ inputData, mastra }) => {
    const agent = mastra.getAgent('quoteComparisonAgent');
    const response = await agent.generate(
      `Compare these insurance quotes for the customer profile provided.
       Quotes: ${JSON.stringify(inputData.quotes)}
       Profile: ${JSON.stringify(inputData.profile)}`,
      {
        structuredOutput: {
          schema: ComparisonResult,
        },
      }
    );
    return response.object;
  },
});

// Step 5: Human approval (qualified director review)
const humanReview = createStep({
  id: 'human-review',
  description: 'Suspends for qualified director (Rolando) to review recommendation',
  inputSchema: ComparisonResult,
  outputSchema: z.object({
    approved: z.boolean(),
    selectedQuoteId: z.string(),
    reviewerNotes: z.string().optional(),
    reviewerName: z.string(),
  }),
  resumeSchema: z.object({
    approved: z.boolean(),
    selectedQuoteId: z.string(),
    reviewerNotes: z.string().optional(),
    reviewerName: z.string(),
  }),
  suspendSchema: z.object({
    reason: z.string(),
    comparison: ComparisonResult,
  }),
  execute: async ({ inputData, resumeData, suspend }) => {
    if (!resumeData) {
      return await suspend({
        reason: 'Revisão do diretor qualificado necessária antes do envio ao cliente',
        comparison: inputData,
      });
    }
    return resumeData;
  },
});

// Step 6: Send recommendation to customer
const sendRecommendation = createStep({
  id: 'send-recommendation',
  description: 'Sends the approved recommendation to the customer via email',
  inputSchema: z.object({
    comparison: ComparisonResult,
    approval: z.object({
      selectedQuoteId: z.string(),
      reviewerNotes: z.string().optional(),
    }),
    customerEmail: z.string().email(),
  }),
  outputSchema: z.object({
    sent: z.boolean(),
    emailId: z.string(),
  }),
  execute: async ({ inputData }) => {
    // Compose and send comparison email to customer
    // Include: comparison summary, recommendation, next steps
    return {
      sent: true,
      emailId: crypto.randomUUID(),
    };
  },
});

// Assemble the workflow
export const insuranceQuoteWorkflow = createWorkflow({
  id: 'insurance-quote-workflow',
  inputSchema: z.object({
    profile: CustomerProfile,
    providers: z.array(z.string()),
    customerEmail: z.string().email(),
  }),
  outputSchema: z.object({
    sent: z.boolean(),
    emailId: z.string(),
  }),
})
  .then(validateProfile)
  .then(requestQuotes)
  .then(collectResponses) // SUSPENDS — waits for email responses
  .then(compareQuotes)
  .then(humanReview) // SUSPENDS — waits for Rolando's approval
  .then(sendRecommendation)
  .commit();
```

### 2.5 Inbound Email Processing

The workflow suspends at `collectResponses` until provider emails arrive. You need a webhook/polling system to detect responses and resume the workflow.

```typescript
// src/lib/email-ingest.ts
// This is NOT a Mastra component — it's the bridge between
// your email system and the Mastra workflow

export async function handleInboundEmail(email: InboundEmail) {
  // Step 1: Match email to a pending quote request
  // (by subject line, thread ID, or sender domain)
  const pendingRequest = await matchEmailToRequest(email);
  if (!pendingRequest) return;

  // Step 2: Parse the email and attachments into NormalizedQuote
  // Use the emailResponseParserTool (which uses AI for PDF parsing)
  const quote = await parseProviderResponse(email);

  // Step 3: Check if all expected provider responses are in
  const allQuotes = await getQuotesForRequest(pendingRequest.workflowRunId);
  allQuotes.push(quote);

  // Step 4: Resume the workflow when we have enough quotes
  // (either all providers responded, or timeout threshold met)
  if (allQuotes.length >= pendingRequest.minQuotesRequired) {
    const workflow = mastra.getWorkflow('insurance-quote-workflow');
    // Resume with collected quotes
    await workflow.resume({
      runId: pendingRequest.workflowRunId,
      step: 'collect-responses',
      resumeData: { quotes: allQuotes },
    });
  }
}
```

**Email infrastructure options for MVP**:
- **Resend** (send) + **Cloudflare Email Workers** (receive) — simplest
- **SendGrid** (send + inbound parse) — more mature
- **Gmail API** — if operating from a Gmail-based business email

### 2.6 Agent Network (Orchestrator)

```typescript
// src/mastra/agents/orchestrator.ts
export const orchestratorAgent = new Agent({
  id: 'orchestrator',
  name: 'Agente Orquestrador',
  instructions: `
    You are the main orchestrator for agent-resolv, a Portuguese insurance broker.
    You coordinate between specialized agents and workflows based on customer requests.

    ROUTING LOGIC:
    - New customer or needs profiling → delegate to intakeAgent
    - Profile complete, wants quotes → trigger insuranceQuoteWorkflow
    - Has quotes, wants to compare → delegate to quoteComparisonAgent
    - Wants to modify existing request → retrieve context and route appropriately
    - General insurance questions → answer directly from knowledge base
    - Complaint or issue → flag for human handling

    Always respond in European Portuguese.
    Always maintain the audit trail by calling the auditLogTool.
  `,
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

// Usage — the network handles routing automatically
const stream = await orchestratorAgent.network(
  'Olá, preciso de um seguro automóvel para o meu carro novo'
);
```

### Phase 2 Deliverables

- [ ] Email sending tool with product-specific templates
- [ ] Email response parser (PDF → NormalizedQuote via AI)
- [ ] Provider registry with initial provider data
- [ ] Full brokerage workflow with two suspend points (email wait + human review)
- [ ] Inbound email processing and workflow resume
- [ ] Quote comparison agent with structured output
- [ ] Orchestrator agent network routing to sub-agents and workflows
- [ ] End-to-end test: customer profile → email quotes → parsed responses → comparison → approved recommendation

---

## PHASE 3 — Production Hardening & Channel Expansion (Weeks 8-12)

**Goal**: Polish the MVP for real-world use. Add web UI for the broker dashboard (human review), improve PDF parsing accuracy, add customer-facing web chat, and prepare the architecture for API-based provider integrations.

### 3.1 Broker Dashboard (Human Review UI)

A Next.js web interface for Rolando and Gonçalo to:

- View pending quote requests and their status
- See suspended workflows awaiting human review
- Review AI-generated comparisons and recommendations
- Approve, modify, or reject recommendations
- Resume workflows after approval
- View full audit trail per customer

```typescript
// Example: API route to list suspended workflows awaiting review
// app/api/reviews/pending/route.ts
export async function GET() {
  const workflow = mastra.getWorkflow('insurance-quote-workflow');
  // Query for runs suspended at 'human-review' step
  const pendingReviews = await workflow.getRunsByStatus('suspended');
  return Response.json(pendingReviews);
}

// API route to approve a review
// app/api/reviews/[runId]/approve/route.ts
export async function POST(req, { params }) {
  const { approved, selectedQuoteId, reviewerNotes, reviewerName } = await req.json();
  const workflow = mastra.getWorkflow('insurance-quote-workflow');

  await workflow.resume({
    runId: params.runId,
    step: 'human-review',
    resumeData: {
      approved,
      selectedQuoteId,
      reviewerNotes,
      reviewerName,
    },
  });

  return Response.json({ success: true });
}
```

### 3.2 PDF Parsing Improvement

The AI-based PDF parsing from Phase 2 will need iteration. Build a feedback loop:

- Parser extracts data and assigns confidence scores per field
- Low-confidence fields are flagged for human review
- Human corrections are logged and used to improve prompts
- Provider-specific parsing templates emerge over time

```typescript
// Evaluation loop for PDF parsing accuracy
import { evaluate } from '@mastra/core/evals';

// Create golden dataset of manually parsed quotes
const goldenDataset = [
  {
    input: { pdfContent: '...', providerId: 'fidelidade' },
    expectedOutput: { /* manually verified NormalizedQuote */ },
  },
  // ... more examples per provider
];

// Run evals to measure parsing accuracy
await evaluate({
  agent: 'email-response-parser',
  dataset: goldenDataset,
  scorers: [
    fieldAccuracyScorer, // % of fields correctly extracted
    premiumAccuracyScorer, // price within 1% tolerance
    coverageCompletenessScorer, // all coverages captured
  ],
});
```

### 3.3 RAG for Insurance Knowledge

Index regulatory documents and product information for agent context:

```typescript
// src/mastra/rag/setup.ts
// Index ASF regulations, provider product sheets, internal compliance guides

import { embed } from '@mastra/core/rag';

// Documents to index:
// - ASF regulations for insurance brokers
// - EU AI Act requirements (Aug 2026 deadline)
// - Provider product documentation
// - Internal compliance checklists
// - Common insurance terms glossary (PT)

// The intake agent and comparison agent use RAG to:
// - Answer customer questions about coverage types
// - Validate recommendations against regulatory requirements
// - Reference specific policy conditions during comparison
```

### 3.4 Observability & Tracing

```typescript
// Mastra has built-in tracing — configure for production
export const mastra = new Mastra({
  // ... agents, workflows, tools
  logger: new PinoLogger({
    name: 'agent-resolv',
    level: 'info',
  }),
  // Enable OpenTelemetry for production tracing
  // Connect to PostHog for product analytics
});

// Track key metrics:
// - Conversation turns to complete intake (target: <10)
// - PDF parsing accuracy by provider
// - Quote request to response time by provider
// - Human review approval rate
// - End-to-end workflow completion time
// - Token usage and cost per workflow run
```

### 3.5 Web Chat Channel

Add a customer-facing web chat using Vercel AI SDK UI:

```typescript
// Thin transport layer — same agent, different channel
// app/api/chat/route.ts
import { mastra } from '@/mastra';

export async function POST(req) {
  const { messages, customerId } = await req.json();
  const agent = mastra.getAgent('orchestrator');

  const stream = await agent.stream(messages, {
    memory: {
      resource: 'customers',
      thread: `customer-${customerId}`,
    },
  });

  return stream.toDataStreamResponse();
}
```

### 3.6 Prepare for API Integrations

As provider partnerships mature, swap email tools for API tools:

```typescript
// src/mastra/tools/providers/fidelidade-api.ts
// Same interface as fidelidade-email.ts — agents don't change

export const fidelidadeApiTool = createTool({
  id: 'fidelidade-api-quote',
  description: 'Requests auto insurance quote from Fidelidade via direct API',
  inputSchema: z.object({
    profile: CustomerProfile,
    productType: z.enum(['auto', 'home', 'life', 'health']),
  }),
  outputSchema: NormalizedQuote, // SAME output schema
  execute: async ({ context }) => {
    // Direct API call instead of email
    // Map CustomerProfile to Fidelidade's API format
    // Parse response to NormalizedQuote
    // Instant instead of 24-48h
  },
});

// When ready, swap in the provider registry:
// integrationTier: 'email' → integrationTier: 'api'
// The workflow and agents remain unchanged
```

### Phase 3 Deliverables

- [ ] Broker dashboard for human review (approve/reject recommendations)
- [ ] Improved PDF parsing with confidence scores and eval pipeline
- [ ] RAG indexed with ASF regulations and provider documentation
- [ ] Production tracing and observability (PostHog + OpenTelemetry)
- [ ] Web chat channel using Vercel AI SDK UI
- [ ] Architecture ready for API-based provider integrations
- [ ] Provider registry supports dynamic routing (email vs API per provider)

---

## Future Phases (Post-MVP)

These are not part of the initial build but should inform architectural decisions now.

### Phase 4 — Multi-Channel & Advanced AI
- WhatsApp Business API integration (same agents, new transport)
- Telegram bot channel
- Voice channel (AI voice agent for phone intake)
- Multi-language support (English for expats in PT)
- Proactive renewal reminders based on policy expiry dates

### Phase 5 — Credit Intermediary (BdP License)
- Credit product schemas (mortgage, personal, business)
- Bank provider integrations
- Credit-specific compliance workflows
- Cross-sell: insurance customer → credit referral

### Phase 6 — Platform & Scale
- Self-service portal for customers (view policies, claims, documents)
- API for B2B partners (embed agent-resolv quoting in third-party apps)
- Provider MCP servers for ecosystem integration
- A2A protocol for cross-broker agent communication
- Multi-tenant architecture for white-label licensing
- AI-powered claims assistance

---

## Technical Decisions & Constraints

### Model Selection Strategy

Use Mastra's model routing to optimize cost vs quality:

| Task                              | Model               | Reason                                                   |
| --------------------------------- | ------------------- | -------------------------------------------------------- |
| Customer intake conversation      | Claude Sonnet       | Good balance of quality and cost for conversational flow |
| PDF parsing / data extraction     | Claude Sonnet       | Needs strong structured output and Portuguese language   |
| Quote comparison & recommendation | Claude Sonnet       | Reasoning-heavy task, needs high quality                 |
| Simple validations, formatting    | Haiku / GPT-4o-mini | Low complexity, optimize for speed and cost              |
| Orchestrator routing decisions    | Sonnet              | Needs to understand context and route correctly          |

### Database Schema (MVP)

```sql
-- Core entities
customers (id, nif, name, email, phone, created_at)
customer_profiles (id, customer_id, product_type, profile_data JSONB, created_at)
quote_requests (id, customer_id, profile_id, status, created_at)
quote_request_providers (id, request_id, provider_id, status, email_thread_id)
normalized_quotes (id, request_id, provider_id, quote_data JSONB, confidence, received_at)
comparisons (id, request_id, comparison_data JSONB, recommendation JSONB, created_at)
reviews (id, comparison_id, reviewer_name, approved, notes, reviewed_at)

-- Audit trail (ASF compliance)
audit_logs (id, action, agent_id, customer_id, details JSONB, timestamp)

-- Mastra internals
workflow_runs (managed by Mastra's LibSQLStore)
agent_memory (managed by Mastra's memory system)
```

### Environment Variables

```env
# LLM Providers
ANTHROPIC_API_KEY=
OPENAI_API_KEY=  # optional, for model routing flexibility

# Database
DATABASE_URL=

# Email
RESEND_API_KEY=
INBOUND_EMAIL_WEBHOOK_SECRET=

# Auth
BETTER_AUTH_SECRET=

# Analytics
POSTHOG_API_KEY=
POSTHOG_HOST=

# Feature Flags
ENABLE_WEB_CHAT=false
ENABLE_WHATSAPP=false
```

### Key Risk Mitigations

| Risk                                | Mitigation                                                                                                                                                  |
| ----------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| PDF parsing accuracy too low        | Start with structured email templates that guide providers toward standard responses. Build provider-specific parsing prompts. Human review catches errors. |
| Email delivery/reception issues     | Use transactional email service (Resend). Implement delivery tracking. Fallback to manual email forwarding.                                                 |
| Provider response time too slow     | Set expectations with customers. Allow partial comparisons (compare available quotes, note pending providers).                                              |
| ASF compliance gaps                 | Rolando reviews every recommendation. Full audit trail from day one. Conservative approach: over-document everything.                                       |
| AI hallucination in recommendations | Structured output with Zod validation. Comparison agent works only from NormalizedQuote data, not from memory. Human review layer.                          |
| Mastra framework instability        | Pin dependency versions. Keep custom tool logic decoupled from Mastra internals. Standard Zod schemas make migration possible.                              |

---

## POC Testing Checklist

### Phase 1 POC Tests

1. **Intake conversation test**: Start 10 conversations in Portuguese for auto insurance. Measure: turns to complete profile, data accuracy, schema validation pass rate.
2. **Product routing test**: Send ambiguous messages ("need insurance") and specific ones ("seguro automóvel para Honda Civic 2020"). Verify correct routing.
3. **Memory persistence test**: Start a conversation, stop mid-intake, resume hours later. Verify context retention.
4. **Edge case test**: Provide invalid NIF, refuse to answer questions, change product type mid-conversation. Verify graceful handling.

### Phase 2 POC Tests

5. **Email composition test**: Generate 5 quote request emails per product type. Have Gonçalo/Rolando validate they would be accepted by providers.
6. **PDF parsing test**: Collect 10 real provider quote PDFs. Parse each. Measure field extraction accuracy against manual extraction.
7. **Workflow end-to-end test**: Run the full workflow with mock provider responses. Verify suspend/resume at both points. Measure total execution time.
8. **Comparison quality test**: Give the comparison agent 3 real quotes. Have Rolando evaluate the recommendation quality.

### Phase 3 POC Tests

9. **Dashboard usability test**: Rolando performs 5 reviews through the dashboard. Measure time per review, error rate.
10. **Multi-channel test**: Same customer query via web chat and email. Verify consistent agent behavior and shared memory.
