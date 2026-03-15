<div align="center">

# 🎙️ Voice & Chat Pipeline
**How we process audio and text into intelligent agent responses.**

</div>

<br />

This document outlines the exact orchestration flow we use to handle multi-modal inputs (Text and Voice) using external APIs (like OpenAI, Sarvam, or Vapi).

---

## 🌊 Pipeline Orchestration Flow

```text
┌─────────────────────────────────────────────────────────┐
│              CHAT & VOICE PIPELINE                        │
│                                                           │
│  Input: {user_id, message|audio, mode, language}         │
│                                                           │
│  ┌─────────────────────────────────────────────┐        │
│  │ Mode 1: TEXT CHAT (LLM Routing)             │        │
│  │                                               │        │
│  │  1. Receive user message                     │        │
│  │  2. Build role-aware system prompt:          │        │
│  │     - User: Simple language, practical       │        │
│  │     - Admin: Technical, verbose              │        │
│  │  3. Include chat history (last 10 messages)  │        │
│  │  4. Call AI API (OpenAI / Anthropic)         │        │
│  │     - Temperature: 0.3 (deterministic)       │        │
│  │  5. Store message + response in DB           │        │
│  │  6. Return AI response to client             │        │
│  └───────────────────┬─────────────────────────┘        │
│                      ▼                                    │
│  ┌─────────────────────────────────────────────┐        │
│  │ Mode 2: VOICE INPUT (Speech-to-Text)        │        │
│  │                                               │        │
│  │  1. User records audio in browser            │        │
│  │  2. Audio sent to POST /api/chat/voice       │        │
│  │  3. Forward to Whisper/Deepgram STT API      │        │
│  │  4. Return text transcription                │        │
│  │  5. Feed into text chat pipeline             │        │
│  └─────────────────────────────────────────────┘        │
│                                                           │
│  ┌─────────────────────────────────────────────┐        │
│  │ Mode 3: VOICE OUTPUT (Text-to-Speech)       │        │
│  │                                               │        │
│  │  1. AI response text generated               │        │
│  │  2. Call TTS API (ElevenLabs / OpenAI)       │        │
│  │  3. Configure: voice_id, pace, emphasis      │        │
│  │  4. Return audio stream/base64 to frontend   │        │
│  │  5. Auto-play in browser                     │        │
│  └─────────────────────────────────────────────┘        │
│                                                           │
│  ┌─────────────────────────────────────────────┐        │
│  │ Mode 4: REAL-TIME WEBSOCKET (Socket.IO)     │        │
│  │                                               │        │
│  │  1. JWT authenticated WebSocket handshake    │        │
│  │  2. User joins room: session-{sessionId}     │        │
│  │  3. Stream tokens live as they generate      │        │
│  │  4. Send function-call indicators to UI      │        │
│  └─────────────────────────────────────────────┘        │
└─────────────────────────────────────────────────────────┘
```

## ⚡ Integration Strategy: API-Based, Not Self-Hosted
For 95% of clients, we do not fine-tune or self-host models simply because it is too expensive and complex to maintain. 

Instead, we use heavily prompted, external APIs with **RAG (Retrieval-Augmented Generation)** to ground the model in the client's custom data.

### Cost Example (1000 Users)
- **STT:** Deepgram = ~$0.0043/min
- **LLM:** GPT-4o-mini = ~$0.15 / 1M Input Tokens
- **TTS:** ElevenLabs = ~$0.06/1000 chars

*Average interaction cost: < $0.05. You can markup this cost 10x when billing clients on a retainer.*
