<div align="center">

# 🗄️ Standardized Agency Client Data Models
**How we structure data for multitenant deployments and single-client handoffs.**

</div>

<br />

When an AI Automation Agency builds a custom web app, dashboard, or internal tooling suite for a client, we must establish a scalable database schema.

We default to **PostgreSQL** (often hosted via Supabase or Railway) managed by **Prisma ORM**. 

---

## 📊 The Core Agency deployment Schema

This schema is the foundational template we use when a client asks us to "build an AI-powered portal for my team."

### 1. Unified Authentication & Teams
Instead of just 'Users', we build for 'Workspaces/Organizations' so the client can invite their whole staff.

```prisma
model Organization {
  id            String    @id @default(uuid())
  name          String
  stripeData    Json?     // Used for agency billing/retainers
  subscriptions Subscription[]
  users         User[]
  apiKeys       ApiKey[]
  createdAt     DateTime  @default(now())
}

model User {
  id             String    @id @default(uuid())
  organizationId String
  email          String    @unique
  role           Role      @default(MEMBER) // OWNER, ADMIN, MEMBER
  organization   Organization @relation(fields: [organizationId], references: [id])
}
```

### 2. The AI RAG (Retrieval-Augmented Generation) Tables
When building a custom Knowledge Base (Vector DB) for a client using `pgvector`.

```prisma
// This extension must be enabled in Postgres
// CREATE EXTENSION IF NOT EXISTS vector;

model Document {
  id             String   @id @default(uuid())
  organizationId String
  title          String
  s3Url          String   // Link to the raw PDF/Text in Supabase Storage
  createdAt      DateTime @default(now())
  chunks         Chunk[]
}

model Chunk {
  id          String   @id @default(uuid())
  documentId  String
  content     String   @db.Text
  embedding   Unsupported("vector(1536)") // Compatible with text-embedding-ada-002 / v3
  document    Document @relation(fields: [documentId], references: [id])
}
```

### 3. AI Agent Analytics & Auditing
We charge clients a monthly retainer to "maintain and optimize" their AI. We cannot optimize what we do not measure. We log every interaction.

```prisma
model InteractionLog {
  id             String    @id @default(uuid())
  organizationId String
  agentType      String    // e.g., "VOICE_INBOUND", "WEB_CHATBOT"
  sessionId      String    // Groups messages together
  userInput      String    @db.Text
  aiResponse     String    @db.Text
  latencyMs      Int
  promptTokens   Int
  completionTokens Int
  costEstimateCents Float
  createdAt      DateTime  @default(now())
}
```

---

## 🌍 Data Sovereignty strategy

As an agency, we face two deployment scenarios regarding client data:

### Strategy A: The "Agency Hosted" Model (Most Common)
- We host the client's database on our Agency's centralized Supabase cluster.
- **Implementation:** We enforce strict `organizationId` Row-Level Security (RLS) policies.
- **Benefit:** Easy for us to push updates and manage. High profit margins.

### Strategy B: The "Client Hosted" / Handoff Model (Enterprise)
- We provision a brand new Supabase/Railway project directly under the client's credit card.
- **Implementation:** We apply this exact schema to their empty database.
- **Benefit:** Zero liability for the agency regarding HIPAA/SOC2 compliance. Total data ownership rests with the client.
