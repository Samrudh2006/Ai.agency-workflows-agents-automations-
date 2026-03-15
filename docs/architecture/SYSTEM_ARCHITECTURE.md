<div align="center">

# 🏛️ Full System Architecture
**The Master Diagram for Nexus AI Agency Client Deployments**

</div>

<br />

The following architecture defines how we build robust, scalable, and responsive AI Platforms (like Web Apps, Automated Dashboards, and Voice Assistants) for our clients.

---

## 🗺️ Master Architecture Flow

```text
┌─────────────────────────────────────────────────────────────────────┐
│                          CLIENTS                                     │
│    ┌──────────────┐   ┌──────────────┐   ┌──────────────┐          │
│    │   Client      │   │  Manager     │   │   Support    │          │
│    │   Browser     │   │  Browser     │   │   Browser    │          │
│    └──────┬───────┘   └──────┬───────┘   └──────┬───────┘          │
│           │                  │                   │                   │
│           │           ┌──────────────┐           │                   │
│           │           │    Admin     │           │                   │
│           │           │   Browser    │           │                   │
│           │           └──────┬───────┘           │                   │
└───────────┼──────────────────┼───────────────────┼──────────────────┘
            │                  │                   │
            ▼                  ▼                   ▼
┌─────────────────────────────────────────────────────────────────────┐
│                  FRONTEND (React + Vite + TypeScript)                 │
│                                                                       │
│  ┌─────────────┐ ┌──────────────┐ ┌────────────┐ ┌──────────────┐  │
│  │ Landing Page │ │ Client       │ │ Manager    │ │ Support      │  │
│  │ + Auth      │ │ Dashboard    │ │ Dashboard  │ │ Dashboard    │  │
│  │ + Login     │ │ + Analytics  │ │ + Team     │ │ + Chat Logs  │  │
│  │ + Register  │ │ + Settings   │ │ + Billing  │ │ + Escalation │  │
│  └─────────────┘ └──────────────┘ └────────────┘ └──────────────┘  │
│                                                                       │
│  ┌─────────────┐ ┌──────────────┐ ┌────────────┐ ┌──────────────┐  │
│  │ Admin       │ │ Chat +       │ │ Ticket +   │ │ Voice        │  │
│  │ Dashboard   │ │ Socket.IO    │ │ Helpdesk   │ │ Assistant    │  │
│  │ + Users     │ │ Real-time    │ │ Threads    │ │ (OpenAI LLM) │  │
│  │ + Analytics │ │ Messaging    │ │ Replies    │ │ STT + TTS    │  │
│  └─────────────┘ └──────────────┘ └────────────┘ └──────────────┘  │
│                                                                       │
│  ┌─────────────────────────────────────────────────────────────────┐ │
│  │ Shared: Analytics │ Webhooks │ API Logs │ Users │ Documents   │ │
│  │ Notifications │ Billing │ Settings │ Profile │ Training Data    │ │
│  └─────────────────────────────────────────────────────────────────┘ │
│                                                                       │
│  Tech: React 18 │ Radix UI + shadcn/ui │ Tailwind CSS │ TanStack   │
│  Query │ React Router DOM │ Framer Motion │ Recharts │ Zod          │
│                                                                       │
│  Hosted on: Vercel (SPA with rewrites)                               │
└──────────┬──────────────────┬──────────────────┬────────────────────┘
           │ HTTPS / REST     │ WebSocket        │ Direct Client
           │                  │ (Socket.IO)      │
           ▼                  ▼                  ▼
┌─────────────────────────────────────┐  ┌──────────────────────────┐
│    BACKEND API (Express + Node.js)   │  │  SUPABASE (Auth + DB)    │
│    Single Monolith Service           │  │                          │
│                                       │  │  - User authentication  │
│  ┌─────────────────────────────────┐ │  │  - Session management   │
│  │          API ROUTES             │ │  │  - Profile sync         │
│  │                                 │ │  │  - Role management      │
│  │  /api/auth/*     Auth + OTP     │ │  │  - RLS policies         │
│  │  /api/users/*    Profile + RBAC │ │  │                          │
│  │  /api/ai/*       AI advisory    │ │  │  PostgreSQL (managed)   │
│  │  /api/chat/*     Chat + Voice   │ │  │  Auth (managed)         │
│  │  /api/forum/*    Posts + Comment│ │  │                          │
│  │  /api/documents/*  Upload/Verify│ │  │  Free tier              │
│  │  /api/billing/*  Invoices       │ │  └──────────────────────────┘
│  │  /api/analytics/*  Dashboards   │ │
│  │  /api/notifications/*  Alerts   │ │  ┌──────────────────────────┐
│  │  /health          Health check  │ │  │  CURATED DATASETS        │
│  └─────────────────────────────────┘ │  │  (src/data/*)            │
│                                       │  │                          │
│  ┌───────────────────────────────────────┐  │  - App Settings         │
│  │            CORE MODULES               │  │  - Theme Constants      │
│  │                                       │  │  - Feature Flags        │
│  │  ┌───────────────┐ ┌───────────────┐ │  │  - Navigation links     │
│  │  │ AI Automation │ │ Chat Engine   │ │  │  - Standard Pricing     │
│  │  │ Engine        │ │               │ │  │  - Sys Prompt Templates │
│  │  │               │ │ - AI chat     │ │  │  - Static Asset URLs    │
│  │  │ - RAG Routing │ │ - Message     │ │  │  - Error Definitions    │
│  │  │ - Webhooks    │ │   history     │ │  │  - Supported Languages  │
│  │  │   Extraction  │ │ - Voice STT   │ │  │  - Country Dial Codes   │
│  │  │ - JSON Generat│ │ - Voice TTS   │ │  │                          │
│  │  │   Pipeline    │ │ - Session mgmt│ │  │  Resolved client-side   │
│  │  │ - Fallbacks   │ │               │ │  │  for fast lookups       │
│  │  │   Control     │ │               │ │  └──────────────────────────┘
│  │  └───────┬───────┘ └───────┬───────┘ │
│  │          │                 │          │
│  │  ┌───────┴───────┐ ┌──────┴────────┐ │
│  │  │ Document      │ │ Ticketing &   │ │
│  │  │ Management    │ │ Helpdesk      │ │
│  │  │               │ │               │ │
│  │  │ - Upload PDF  │ │ - Ticket CRUD │ │
│  │  │ - Chunking    │ │ - Notes       │ │
│  │  │ - Embeddings  │ │ - Agent Assign│ │
│  │  │ - Vector DB   │ │ - SLA Tracking│ │
│  │  └───────────────┘ └───────────────┘ │
│  │                                       │
│  │  ┌───────────────┐ ┌───────────────┐ │
│  │  │ Notification  │ │ Analytics     │ │
│  │  │ Engine        │ │ Engine        │ │
│  │  │               │ │               │ │
│  │  │ - Email       │ │ - Role-based  │ │
│  │  │   (Nodemailer)│ │   dashboards  │ │
│  │  │ - SMS (Twilio)│ │ - Activity    │ │
│  │  │ - In-app      │ │   tracking    │ │
│  │  │ - Socket.IO   │ │ - 7d/30d/90d  │ │
│  │  │   real-time   │ │   timeframes  │ │
│  │  └───────────────┘ └───────────────┘ │
│  └───────────────────────────────────────┘ │
│                                       │
│  ┌─────────────────────────────────┐ │
│  │         MIDDLEWARE STACK        │ │
│  │                                 │ │
│  │  Helmet → CORS → Rate Limiter  │ │
│  │  → JWT Auth → RBAC → Validator │ │
│  │  → Error Handler → Logger      │ │
│  └─────────────────────────────────┘ │
│                                       │
│  Tech: Express 4.18 │ Prisma ORM │ JWT │ Bcrypt │ Socket.IO    │
│  Winston Logger │ express-validator │ Multer │ UUID             │
│                                                                   │
│  Hosted on: Railway / VPS (~$5-10/mo)                            │
└──────────┬───────────────────┬──────────────────┬───────────────┘
           │                   │                  │
           ▼                   ▼                  ▼
┌──────────────────┐ ┌─────────────────┐  ┌────────────────────────┐
│  SQLite / Prisma │ │  Socket.IO      │  │  OpenAI / Anthropic    │
│  (Primary DB)    │ │  (Real-time)    │  │  (Core Engine)         │
│                  │ │                 │  │                        │
│  - Users         │ │  - Chat rooms   │  │  - Chat completions    │
│  - Client/       │ │  - Presence     │  │    (gpt-4o-mini)       │
│    Manager/      │ │  - Live notifs  │  │  - Speech-to-text      │
│    Support       │ │  - JWT auth     │  │    (Deepgram Nova-2)   │
│    Profiles      │ │  - User rooms   │  │  - Text-to-speech      │
│  - Documents     │ │                 │  │    (ElevenLabs)        │
│  - Chat Messages │ │  Embedded in    │  │  - Structured Outputs  │
│  - Tickets       │ │  Express server │  │    (JSON extractions)  │
│  - Embeddings    │ │                 │  │                        │
│  - Notifications │ │                 │  │  Pay-per-use APIs      │
│  - Activities    │ │                 │  │                        │
│  - Audit Logs    │ └─────────────────┘  └────────────────────────┘
│  - AI Recs       │
│  - OTP Codes     │  ┌─────────────────┐  ┌────────────────────────┐
│                  │  │  Twilio         │  │  Nodemailer            │
│  SQLite (dev)    │  │  (SMS/WhatsApp) │  │  (Email SMTP)          │
│  → Postgres      │  │                 │  │                        │
│  (production)    │  │  - OTP delivery │  │  - OTP delivery        │
│                  │  │  - Alerts       │  │  - Notifications       │
│  Free (SQLite)   │  │  Pay-per-use    │  │  Free / SMTP plan      │
└──────────────────┘  └─────────────────┘  └────────────────────────┘
```

## 🏗️ Architecture Decisions

### Monolith vs Microservices: MONOLITH
| Factor | Monolith (Chosen) | Microservices |
| :--- | :--- | :--- |
| **Team size** | ✅ Perfect for lean agency teams | ❌ Overkill |
| **Dev speed** | ✅ Fast iteration for clients | ❌ Inter-service overhead |
| **Deployment**| ✅ Single container | ❌ K8s/Docker Compose |
| **Debugging** | ✅ Single log stream (Winston) | ❌ Distributed tracing |
| **Cost** | ✅ ~$5-10/mo to host | ❌ $50-100/mo minimum |

### Database Strategy: SQLite → PostgreSQL
| Why SQLite (Dev) | Why PostgreSQL (Prod) |
| :--- | :--- |
| Zero-config local setup | ACID for concurrent writes |
| File-based, no server needed | Full-text search built-in |
| Fast for single-user development | Built-in Auth hooks via Supabase |

---
*(End of Architecture Overview)*
