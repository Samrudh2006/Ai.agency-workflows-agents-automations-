<div align="center">

# 🔒 Security Model & Architecture
**Protecting AI payloads, Client PI, and API endpoints.**

</div>

<br />

When delivering AI solutions to clients, security is our main selling point. The following layers ensure we meet compliance standards (HIPAA, GDPR) where applicable.

---

## 🛡️ The 6-Layer Security Architecture

```text
┌─────────────────────────────────────────────────────┐
│              SECURITY LAYERS                         │
│                                                       │
│  Layer 1: TRANSPORT                                  │
│    - HTTPS everywhere (Vercel + Railway enforce)    │
│    - Helmet.js security headers                     │
│    - CORS whitelist (FRONTEND_URL only)             │
│                                                       │
│  Layer 2: AUTHENTICATION                             │
│    - bcrypt password hashing (12 rounds)            │
│    - JWT tokens with expiry                         │
│    - OTP-based 2FA for Admin/Support roles          │
│    - Supabase RLS (Row-Level Security) policies     │
│                                                       │
│  Layer 3: AUTHORIZATION                              │
│    - Role-based middleware (authorize([roles]))      │
│    - Resource ownership checks                      │
│    - Socket.IO room isolation (user-{userId} only)  │
│                                                       │
│  Layer 4: INPUT VALIDATION & PROMPT INJECTION        │
│    - express-validator on all REST endpoints        │
│    - Zod schemas on frontend                        │
│    - System messages strictly separate instructions  │
│    - SQL injection prevention (Prisma ORM)          │
│                                                       │
│  Layer 5: RATE LIMITING & COST CONTROL               │
│    - Per-IP rate limits (4 tiers)                   │
│    - Auth endpoint hardening (20 req/15min)         │
│    - AI endpoint cost control (Usage limits)        │
│                                                       │
│  Layer 6: ERROR HANDLING & LOGGING                   │
│    - Global error boundary (React)                  │
│    - Express error middleware (no stack traces)      │
│    - Winston structured logging                     │
│                                                       │
└─────────────────────────────────────────────────────┘
```

## 🚨 Rate Limiting Strategy
To protect from DDoS and API credit scraping (billing attacks):

| Tier | Window | Max Limit | Target Routes | Purpose |
| :--- | :--- | :--- | :--- | :--- |
| **Tier 1: Global API** | 15 minutes | 100 / IP | `/api/*` | General flood protection |
| **Tier 2: Auth Endpoints** | 15 minutes | 20 / IP | `/api/auth/` | Prevent brute-force |
| **Tier 3: AI / LLM Calls** | 15 minutes | 40 / IP | `/api/ai/*` | Control OpenAI/Anthropic costs |
| **Tier 4: Chat Messages** | 5 minutes | 60 / User| `/api/chat/*` | Prevent spam/abuse |

*Response on limit: `429 Too Many Requests` + Retry-After headers.*
