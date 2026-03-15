<div align="center">

# 🎙️ Ultra-Low Latency Voice Agent Pipeline
**The architecture required to make an AI sound human on the phone.**

</div>

<br />

The biggest friction point in selling Voice AI to clients is latency. If the AI takes 3 seconds to respond, the human caller will interrupt it, get confused, and hang up. 

Our agency guarantees **sub-800ms response times** by utilizing the following distributed architecture.

---

## ⚡ The Sub-800ms Architecture Flow

To achieve human-like conversation, we decouple the process into three distinct, specialized microservices managed via WebSockets.

```text
┌─────────────────────────────────────────────────────────────┐
│                 ULTRA-LOW LATENCY PIPELINE                  │
│                                                             │
│  1. Human      (Continuous Audio Stream)                    │
│     Speaks ─────────────────────────┐                       │
│                                     ▼                       │
│                           ┌──────────────────┐              │
│                           │ Speech-to-Text   │              │
│                           │ (Deepgram Nova-2)│              │
│                           └────────┬─────────┘              │
│                                    │  < 150ms               │
│                                    ▼                        │
│                           ┌──────────────────┐              │
│                           │ Engine / LLM     │              │
│                           │ (gpt-4o-mini /   │              │
│                           │  Groq Llama 3)   │              │
│                           └────────┬─────────┘              │
│   (Tokens streamed                 │  < 300ms (TTFT*)       │
│    the moment they                 │                        │
│    are generated)                  ▼                        │
│                           ┌──────────────────┐              │
│  4. AI Voice              │ Text-to-Speech   │              │
│     Plays  ◄──────────────┤ (ElevenLabs /    │              │
│                 < 300ms   │  Play.ht Turbo)  │              │
│                           └──────────────────┘              │
└─────────────────────────────────────────────────────────────┘
* TTFT = Time To First Token
```

## 🧠 Core Engineering Principles for Voice

### 1. Streaming vs. Batching (Crucial)
We NEVER wait for the LLM to write the whole paragraph before generating audio.
- The LLM streams tokens exactly like ChatGPT does on the web.
- The moment the LLM types the first punctuation mark (e.g., "Hello,"), that single word is immediately sent to ElevenLabs to render the audio.
- While ElevenLabs is playing "Hello," the LLM is finishing the rest of the sentence in the background.

### 2. Provider Selection for Speed over Brilliance
A phone receptionist does not need to write python code. It needs to say "Yes, we are open until 5 PM" instantly.
- **LLM:** We use `gpt-4o-mini` or Groq (Llama-3). We do *not* use `gpt-4` for Voice Agents due to the latency penalty.
- **STT:** Deepgram is preferred over OpenAI Whisper due to its real-time streaming capabilities and endpointing (detecting when the user stops talking).

### 3. Interruption Handling (Barge-in)
Humans talk over each other. The system must support "Barge-in".
- When Deepgram detects user audio, a kill-signal is sent via WebSocket to stop the ElevenLabs audio playback immediately.
- The LLM context window is updated to reflect that the bot was cut off mid-sentence.

## 🛠️ The Agency Stack Recommendation
While it's possible to build this websocket orchestration from scratch using Node.js, **we highly advise utilizing a managed Voice platform** to handle the telephony and websocket orchestration.

**Agency Preferred Stack:**
- **Telephony / Orchestration:** Vapi.ai or Bland AI
- **Phone Numbers:** Twilio (Imported via SIP trunk to Vapi)
- **Functions/Webhooks:** Make.com (Triggered by Vapi on call completion)
