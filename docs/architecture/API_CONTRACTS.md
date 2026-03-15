<div align="center">

# 🌐 API Contracts & Core Endpoints
**Standardized REST and Webhook contracts for our Agency builds.**

</div>

<br />

This document outlines the standard Express REST API architecture we use to connect Frontend Dashboards and external No-Code Workflow tools (Make/Zapier) into our core platform.

---

## 📡 Base Endpoint map

```text
┌────────────────────────────────────────────────────────┐
│             BACKEND API (Express.js)                   │
│                                                        │
│  /api/v1/                                              │
│  ├── auth/                                             │
│  │   ├── POST /signup      (Register new client)       │
│  │   ├── POST /login       (Return JWT)                │
│  │   └── POST /refresh     (Refresh token)             │
│  │                                                     │
│  ├── ai/                                               │
│  │   ├── POST /chat        (Text-based AI inference)   │
│  │   ├── POST /voice/ask   (Accepts audio, returns TTS)│
│  │   └── POST /extract     (OCR + LLM JSON extract)    │
│  │                                                     │
│  ├── webhooks/                                         │
│  │   ├── POST /vapi        (Handles Voice Call states) │
│  │   ├── POST /stripe      (Billing logic)             │
│  │   └── POST /crm         (Lead sink from Zapier)     │
│  │                                                     │
│  └── data/                                             │
│      ├── GET  /metrics     (Analytics for Dashboard)   │
│      └── POST /upload      (Upload RAG documents)      │
└────────────────────────────────────────────────────────┘
```

## 🔄 Webhook Processing Flow (N8N/Make.com)

When designing webhooks to trigger external automations, we standardize our JSON payloads to make it easy to map data visually.

### Example: Outbound Trigger `POST /webhook/new-lead`
```json
{
  "event_type": "lead_captured",
  "timestamp": "2026-03-15T12:00:00Z",
  "data": {
    "lead_id": "usr_9438jf23",
    "name": "Jane Doe",
    "phone": "+15550198234",
    "intent_score": 85,
    "ai_summary": "User is interested in a 3-bedroom apartment in downtown area."
  },
  "metadata": {
    "source_agent": "RealEstateVoiceBot",
    "call_duration_seconds": 124
  }
}
```

### Response Standard Wrapper
Every API response returned to the frontend or an external tool must follow this standard format:
```json
{
  "success": true,
  "data": { ... payload here ... },
  "error": null,
  "meta": {
    "processing_time_ms": 142
  }
}
```
