<div align="center">

# ⚡ n8n Workflow Blueprint
**Self-hosted automation blueprint for processing inbound communications.**

</div>

<br />

When clients require strict data sovereignty (Healthcare, Enterprise) we use **n8n** instead of Make.com to host automations on our own secure infrastructure (Supabase/Railway).

This blueprint outlines a core AI-email parsing sequence.

---

## 🗺️ The n8n Workflow Architecture

```text
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│  1. IMAP Node   │    │ 2. OpenAI Node  │    │ 3. Postgres/DB  │
│  (Read Inbox)   │───►│ (Extract Data)  │──┬►│ (Save Lead)     │
└─────────────────┘    └─────────────────┘  │ └─────────────────┘
                                            │
                                            │ ┌─────────────────┐
                                            └►│ 4. Gmail Node   │
                                              │ (AI Draft Reply)│
                                              └─────────────────┘
```

## 📤 1. The Trigger: Email Read (IMAP)
We connect this node to a specific sales inbox (e.g. `inquiries@clientcompany.com`).
- **Trigger Condition:** On all *Unread* emails.
- **Output:** Raw Email Body (`$json.textPlain`) and Sender Email (`$json.from.value[0].address`).

## 🧠 2. The AI Processing Node: OpenAI (Structured Output)

Pass the Email Body to an OpenAI node configured to extract JSON using Function Calling (or JSON Mode).

**System Prompt for n8n OpenAI Node:**
```text
You are an expert data extraction assistant. Read the provided email body.
Extract the following information into a strict JSON object:
1. 'intent': categorization of the email (Support, Sales, Spam, or Refund).
2. 'urgency': integer from 1 to 10.
3. 'name': sender's name if available.
4. 'summary': a 1-sentence summary of the request.
```

**Expression mapping in n8n:**
`{{ $json["textPlain"] }}`

## 🗄️ 3. The Database Node: Postgres (Supabase)

Insert the extracted JSON array into the client's lead tracker database.

- **Table:** `inbound_leads`
- **Mapping:**
  - `email` = `{{ $('IMAP Node').item.json.from.value[0].address }}`
  - `intent_category` = `{{ $('OpenAI Node').item.json.intent }}`
  - `ai_summary` = `{{ $('OpenAI Node').item.json.summary }}`

## ✉️ 4. The Response Node (Auto-Drafting)

If `intent == 'Sales'`, trigger a secondary OpenAI node to draft a personalized response based on the `summary`, then use the **Gmail Node** to save that output into the *Drafts* folder for a human to review.

**Drafting Prompt:**
```text
Write a polite, 3-sentence reply to this prospective client acknowledging their request: {{ $('OpenAI Node').item.json.summary }}.
Mention that a human sales rep will reach out shortly. Do not include subject lines.
```
