<div align="center">

# 🤝 Client Onboarding SOP
**Standard Operating Procedure for launching a new client project.**

</div>

<br />

The first 48 hours after a client signs the MSA and pays the invoice are crucial. This SOP ensures a smooth, professional handoff from Sales to Development.

---

## 🛠️ Step 1: The Administrative Setup (Day 1)
*Completed by: Account Manager / Founder*

- [ ] Verify 50% deposit has cleared via Stripe.
- [ ] Verify Master Services Agreement (MSA) is signed.
- [ ] Create a dedicated Slack/Teams/Discord channel: `#client-company-name`.
- [ ] Create a shared Google Drive folder with Sub-folders: `/Assets`, `/Contracts`, `/Transcripts`.
- [ ] Send the "Welcome & Onboarding Questionnaire" email to the client.

## 📝 Step 2: The Onboarding Questionnaire
The client must fill this out before we write a single line of code.

**Key Questions to ask:**
1. **Brand Voice:** Do you want the AI to sound extremely professional, casual, empathetic, or enthusiastic?
2. **The "Hand-Off" Goal:** If the AI does not know an answer, exactly what should it do? (e.g. Say "Let me email the team" vs transfer to a phone number).
3. **Primary Tech Stack:** What CRM do you currently use? (HubSpot, GoHighLevel, Salesforce?)
4. **Data Upload:** Please drop all your standard operating procedures, pricing PDFs, and FAQs into the shared Google Drive folder.

## 📞 Step 3: The Kickoff Call (Day 3-4)
*Goal: Finalize the technical architecture.*

- [ ] Review the Questionnaire answers live on a 30-minute Zoom call.
- [ ] Present a simple visual data flow (e.g. Lead → AI → CRM).
- [ ] Request API Keys or OAuth sharing for their CRM. *(Use secure vaults, NEVER ask for keys over plain email).*
- [ ] Set expectations for the testing phase (e.g. "On Friday, we will send you a phone number. I want you to call it and try to break the AI").

## 💻 Step 4: The Internal Handoff
- [ ] Account manager creates a highly detailed prompt requirements document.
- [ ] Lead developer forks the standard Voice Agent / Automation repository.
- [ ] Kickoff the Build Phase.
