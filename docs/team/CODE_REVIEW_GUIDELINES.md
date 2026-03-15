<div align="center">

# 🔍 Code & Workflow Review Guidelines
**How we ensure high-quality AI deployments before launch.**

</div>

<br />

Unlike traditional software engineering, an "AI Agency" review process involves reviewing system prompts, Make.com blueprints, and Vector Data just as strictly as Node.js code.

---

## 1. Submitting a Review (The Developer)

When you believe a custom agent or workflow is ready, you must document it in the `#code-reviews` channel for another engineer to review.

**Your Submission Must Include:**
1. **The Goal:** What does this agent/workflow do? (e.g. "Inbound Voice Agent to book cleaning appointments.")
2. **The Components involved:** Vapi + Make.com + GoHighLevel.
3. **The Loom Video:** A 2-minute recording showing a successful test run from start to finish.

## 2. Reviewing the Build (The Reviewer)

The reviewer is responsible for deliberately trying to break the system.

### A. The System Prompt Review
- [ ] Is it concise? (Tokens cost money. Remove fluff).
- [ ] Are the security guardrails present? ("Do not discuss competitors, do not offer discounts").
- [ ] Is the JSON schema for function-calling strictly defined?

### B. The No-Code (Make/n8n) Review
- [ ] Are all modules explicitly renamed so they are readable? (e.g., "Parse Stripe Payment" instead of "Webhook 1").
- [ ] Is there an Error Handler attached to critical external API calls? (e.g. if the CRM API is down, does the lead get saved to a backup Google Sheet or does it just vanish?)
- [ ] Have all Auth/Credentials been properly decoupled using Environment Variables?

### C. The Cost Reality Check
- [ ] Did we over-engineer this? (e.g. using `gpt-4o` for a simple JSON extraction when `gpt-4o-mini` would cost 10x less and run 3x faster?)

## 3. Approval & Merge
Once the Reviewer adds a ✅ emoji to the Slack thread, the Dev is authorized to transition the project from the Staging environment to Production and notify the Account Manager for Client Handoff.
