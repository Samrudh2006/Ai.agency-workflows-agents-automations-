<div align="center">

# 🚨 Incident Response Plan
**What to do when an API goes down or an Automation breaks.**

</div>

<br />

In the automation business, external APIs (OpenAI, Make, HubSpot) *will* go down eventually. Our job is to handle it gracefully before the client notices.

---

## 🔔 Monitoring & Alerts
All core webhooks and APIs are monitored. 
- A failure in Make.com/n8n triggers an automated ping to the `#critical-alerts` Slack channel.
- If Vapi / OpenAI exhibits >2000ms latency, an alert is triggered.

## 🛠️ The 4-Step Response Protocol

### Step 1: Containment (0-15 Minutes)
If the AI is hallucinating wildly or hallucinating discounts:
- **Kill Switch:** Immediately deactivate the active phone number routing or toggle the website widget to `hidden`.
- Ensure the bleeding stops before trying to diagnose the code.

### Step 2: Client Communication (15-30 Minutes)
Never hide downtime. Own it.
- **Template:** "Hi [Client], our monitoring system caught an issue with the upstream provider [e.g., OpenAI]. The agent has been temporarily paused to prevent sending incorrect info. We expect it to be back online shortly and will update you in 30 minutes."

### Step 3: Diagnosis & Fallback (30-60 Minutes)
- Check external status pages (`status.openai.com`, `status.make.com`).
- If OpenAI is down, can we route to Anthropic Claude via our fallback logic?
- Check webhook logs. Was the payload schema altered by the CRM without warning?

### Step 4: Post-Mortem (24 Hours Later)
After the fix is deployed, the Lead Dev must write a brief Post-Mortem:
- What happened?
- Why wasn't it caught earlier?
- How do we update our templates to ensure this never breaks again? (e.g., adding an error-handler module to Make.com that catches empty arrays).
