<div align="center">

# 🧠 Chatbot Knowledge Base Formatting
**How to structure data before feeding it to RAG (Vector Databases).**

</div>

<br />

When building custom ChatGPT clones for websites or internal company usage, the AI is only as good as the documents you feed it.

Garbage in = Garbage out. Do not just upload messy PDFs. Use this markdown format to clean client data before vectorizing.

---

## 📐 The Standard Q&A Format Document
*Upload a text/markdown file looking exactly like this to your Supabase Vector DB, Pinecone, or OpenAI Assistant.*

```markdown
# Knowledge Base: [Company Name] Support & Policies
**Last Updated:** [Date]

## Category: Pricing & Refunds

**Question:** What is your refund policy?
**Answer:** We offer a 30-day money-back guarantee on all software subscriptions. Physical goods must be returned unopened within 14 days. Custom enterprise builds non-refundable once the Statement of Work is signed.

**Question:** Do you offer discounts for non-profits?
**Answer:** Yes, registered 501(c)(3) non-profits receive a flat 20% discount on all monthly retainers. They must email verification to non-profits@company.com to apply.

## Category: Technical Support

**Question:** How do I reset my API key if I lose it?
**Answer:** Log into the dashboard, navigate to Settings > Developer > API Keys. Click the red "Revoke & Regenerate" button. Note that your old key will immediately stop working and any active integrations will break until updated.

**Question:** What are your server IP addresses so I can whitelist them?
**Answer:** Our outbound webhook IP addresses are: 
1. 192.168.1.100
2. 10.0.0.55
(Note: Only provide these if the customer explicitly asks for IP whitelisting).
```

## 🛠️ Tips for Cleaning Client Data (The Process)

If a client hands you a messy 50-page PDF manual:
1. Parse the PDF using a script (or OpenAI's extraction tool).
2. Ask an LLM: *"Rewrite this entire manual into pairs of 'Question:' and 'Answer:' using concise, professional language."*
3. Save the output as a clean `.md` file.
4. Upload that parsed `.md` file to your Vector DB.

**Why?** Vector databases search by similarity. A clean "Question and Answer" chunk has a much higher retrieval accuracy than a random paragraph buried on page 34 of a dense manual.
