# ⚙️ Automation Blueprints
This directory contains fully functional, exportable `.json` blueprints that you can immediately import into your Make.com or n8n accounts to start automating.

## Included Blueprints
1. **`make_lead_routing_webhook.json`** - A fast, reliable Make.com flow that catches inbound Voice AI payloads (from Vapi or Bland), extracts them via OpenAi, logs the data to HubSpot, and alerts the team in Slack.
2. **`n8n_email_support_routing.json`** - An n8n workflow designed to intercept Zendesk tickets, use GPT-4o-mini to triage if human intervention is required, and auto-reply using RAG.
3. **`make_data_extraction.json`** - A Make.com data-layer blueprint showing how to take unstructured WhatsApp messages, force OpenAI to output strict JSON, and log the variables into Airtable.

## How to use
- **Make.com:** Open a new Scenario -> Click the `...` menu at the bottom -> "Import Blueprint" -> Select the `.json` file.
- **n8n:** Go to Workflows -> "Import from File" -> Select the `.json` file.
