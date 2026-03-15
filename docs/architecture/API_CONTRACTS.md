<div align="center">

# 🌐 API Contracts & Webhook Interfaces
**How we connect AI Agents to CRMs reliably every time.**

</div>

<br />

The core of an AI Automation Agency is making different tools talk to each other. We use standardized JSON "Contracts". When the Voice Agent finishes a call, it MUST output data in this exact format to trigger the next step.

---

## 📡 The Universal Lead API Contract

Whether the lead comes from a website form, an inbound phone call (Vapi), or a WhatsApp bot, the AI must synthesize the conversation into this exact JSON schema before hitting our No-Code Webhooks.

### The "Qualification Payload"
**Endpoint:** `POST https://hook.us1.make.com/abc123xyz`  
**Trigger:** Fired immediately after the AI concludes a conversation with a prospect.

```json
{
  "agency_metadata": {
    "client_id": "org_9f823jd",
    "source_channel": "INBOUND_VOICE",
    "session_duration_seconds": 184
  },
  "extracted_lead": {
    "first_name": "Jane",
    "last_name": "Doe",
    "phone_number": "+15550198234",
    "email": "jane.doe@example.com",
    "intent_category": "SALES_INQUIRY", 
    "urgency_score": 8 
  },
  "ai_analysis": {
    "call_summary": "Jane called to ask if we handle commercial roof repairs. She has a 5000 sq ft warehouse that is leaking.",
    "next_action_recommended": "Call back ASAP to schedule a site visit."
  }
}
```

## 🔄 Standard Mapping in No-Code Tools (Make/n8n)

Because we enforce the strict JSON contract above, our Make.com developers can build standardized blueprints in minutes.

### 1. The CRM Routing Logic
We use a Router based on `extracted_lead.intent_category`:
- **If `SALES_INQUIRY`:** Route to `GoHighLevel -> Create Opportunity -> Pipeline Stage: New Lead`.
- **If `SUPPORT_ISSUE`:** Route to `Zendesk -> Create Ticket -> Priority: High`.
- **If `SPAM/WRONG_NUMBER`:** Stop the scenario. Do not pollute the client's CRM.

### 2. The Slack Alert Formatting
We generate a standardized notification for the client's team:

```markdown
*🚨 New AI Qualified Lead!*
*Name:* {{extracted_lead.first_name}} {{extracted_lead.last_name}}
*Phone:* {{extracted_lead.phone_number}}
*Intent:* {{extracted_lead.intent_category}} (Score: {{extracted_lead.urgency_score}}/10)

*AI Summary:*
> {{ai_analysis.call_summary}}

*Next Action:* {{ai_analysis.next_action_recommended}}
```

## ⏳ Handling Missing Data
AI is non-deterministic. Sometimes the caller refuses to give their last name.
**The Contract Rule:** The AI must return `null` for any missing fields, rather than hallucinating or leaving the key out of the JSON. 

Our Make.com workflows are configured with `ifempty({{value}}; "Unknown")` formulas to safely process null variables without failing.
