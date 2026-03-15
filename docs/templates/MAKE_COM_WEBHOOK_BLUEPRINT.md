<div align="center">

# ⚡ Make.com Webhook Blueprint
**The standard payload layout for linking Voice Agents to CRM Platforms.**

</div>

<br />

Make.com (formerly Integromat) is our preferred no-code automation engine. This reference shows the standard JSON blueprint we build for clients to route leads from a Voice Agent (like Vapi) into a CRM (like HubSpot or GoHighLevel).

---

## 🗺️ The Workflow Architecture

```text
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│  1. Custom      │    │ 2. Router       │    │ 3. HTTP Request │
│  Webhook (Catch)│───►│ (Intent Check)  │──┬►│ (Add to CRM)    │
└─────────────────┘    └─────────────────┘  │ └─────────────────┘
                                            │
                                            │ ┌─────────────────┐
                                            ├►│ 4. Slack App    │
                                            │ │ (Send Alert)    │
                                            │ └─────────────────┘
                                            │
                                            │ ┌─────────────────┐
                                            └►│ 5. Gmail / Send │
                                              │ (Follow up Email│
                                              └─────────────────┘
```

## 📦 1. The Custom Webhook Module (Catch Hook)

When Vapi triggers the `end_call_and_send_data` function, it sends a payload to the Make.com Custom Webhook URL.

**Expected Incoming JSON Payload (Configure Make to 'Determine Data Structure'):**
```json
{
  "message": {
    "toolCalls": [
      {
        "function": {
          "name": "end_call_and_send_data",
          "arguments": {
            "caller_name": "John Smith",
            "caller_phone": "+15551239876",
            "qualifier_data": "Needs roof repaired ASAP",
            "call_summary": "John called about a leaking roof. Needs someone out tomorrow morning."
          }
        }
      }
    ]
  }
}
```

## 🔀 2. The Router (Intent Check)
Use a Router module to split paths based on standard logic.
- **Path A (Qualified Lead):** Filter: `qualifier_data` exists and is not empty.
- **Path B (Spam/Handoff):** Filter: `call_summary` contains "spam" or "wrong number". (Routes to an auto-archive module).

## 🚀 3. HTTP Request / CRM Module (Add to CRM)
Map the variables from Module 1 into a standard Add Contact module (e.g., HubSpot / GoHighLevel).
- `First Name` = `get(split(caller_name; " "); 1)`
- `Last Name`  = `get(split(caller_name; " "); 2)`
- `Phone`      = `caller_phone`
- `Notes`      = `call_summary`

## 🔔 4. Slack Notification Module
Format a clean markdown message for the client's `#new-leads` channel.

**Slack Message Template:**
```markdown
*🚨 New AI Voice Lead!*
*Name:* {{1.caller_name}}
*Phone:* {{1.caller_phone}}

*Situation:* {{1.qualifier_data}}
*Transcription Summary:* {{1.call_summary}}

_Added to HubSpot automatically. Needs human follow-up tomorrow morning._
```
