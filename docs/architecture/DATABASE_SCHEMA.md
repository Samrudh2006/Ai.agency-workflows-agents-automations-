<div align="center">

# 🗄️ Database Schema & Data Models
**Standard schema templates for SaaS Client Deployments**

</div>

<br />

For our agency builds, we typically start with SQLite for local development and migrate seamlessly to PostgreSQL via Prisma ORM when pushing to production.

---

## 📊 Core Data Model (Prisma ORM)

```text
┌─────────────────────────────────────────────────────────┐
│           DATA MODEL (Prisma ORM + SQLite)               │
│                                                           │
│  Table: users                                            │
│  ┌──────────────────┬────────────┬─────────────────────┐│
│  │ id               │ UUID (PK)  │                     ││
│  │ email            │ STRING     │ unique              ││
│  │ password         │ STRING     │ bcrypt hashed       ││
│  │ name             │ STRING     │                     ││
│  │ phone            │ STRING?    │                     ││
│  │ role             │ ENUM       │ ADMIN/USER/SUPPORT  ││
│  │ isVerified       │ BOOLEAN    │ default: false      ││
│  │ createdAt        │ TIMESTAMP  │                     ││
│  └──────────────────┴────────────┴─────────────────────┘│
│                                                           │
│  Table: user_profiles                                    │
│  ┌──────────────────┬────────────┬─────────────────────┐│
│  │ userId           │ FK → users │ 1:1                 ││
│  │ companyName      │ STRING     │                     ││
│  │ industry         │ STRING     │                     ││
│  │ latitude         │ FLOAT?     │ geo-location        ││
│  │ longitude        │ FLOAT?     │                     ││
│  └──────────────────┴────────────┴─────────────────────┘│
│                                                           │
│  Table: documents (For RAG / Knowledge Bases)            │
│  ┌──────────────────┬────────────┬─────────────────────┐│
│  │ id               │ UUID (PK)  │                     ││
│  │ userId           │ FK → users │                     ││
│  │ fileName         │ STRING     │                     ││
│  │ filePath         │ STRING     │ S3/Supabase Storage ││
│  │ embedding        │ VECTOR     │ pgvector            ││
│  │ size             │ INT        │ bytes               ││
│  └──────────────────┴────────────┴─────────────────────┘│
│                                                           │
│  Table: chat_sessions                                    │
│  ┌──────────────────┬────────────┬─────────────────────┐│
│  │ id               │ UUID (PK)  │                     ││
│  │ userId           │ FK → users │                     ││
│  │ agentId          │ STRING     │ Voice/Chat Agent ID ││
│  │ transcript       │ TEXT       │                     ││
│  │ summary          │ TEXT       │ LLM generated       ││
│  └──────────────────┴────────────┴─────────────────────┘│
└─────────────────────────────────────────────────────────┘
```

## 🧠 Curated Client-Side Data Layer
To minimize API latency and save costs on backend queries, we bundle static contextual data directly into the frontend build.

### Data Examples `src/data/*`
- `industryBenchmarks.ts` (Industry statistics for dashboards)
- `geofencingLimits.ts` (Location boundaries and constraints)
- `pricingTiers.ts` (Available SaaS plans and feature flags)

*POST-MVP: Migrate SQLite → PostgreSQL (Railway/Supabase) for concurrent writes + full-text search + production reliability.*
