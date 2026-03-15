<div align="center">

# 🤝 Contributing to Nexus AI Agency & Automations
**Your guide to contributing AI workflows, custom agents, and voice scripts.**

[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=for-the-badge)](http://makeapullrequest.com)

</div>

<br />

First off, **thank you** for considering contributing to our repository! It's people like you that make the AI and Automations community such a great place to learn, inspire, and create.

Whether you're fixing a bug, adding a new Make.com blueprint, tweaking a system prompt for a Voice Agent, or adding a completely new custom Chatbot template, your contributions are highly valued!

---

## 🧭 How to Contribute

We follow a standard GitHub flow. Here’s how you can make your first contribution:

### 1️⃣ Fork the Repository
Click the **Fork** button at the top right of the repository page. This creates your own copy of the project where you can freely make changes.

### 2️⃣ Clone Your Fork
Clone your newly created fork to your local machine:
```bash
git clone https://github.com/YOUR-USERNAME/ai-agency-blueprint.git
cd ai-agency-blueprint
```

### 3️⃣ Create a Branch
Always create a new branch for your specific feature or fix. Use a descriptive name:
```bash
git checkout -b feature/hubspot-sync-workflow
# OR
git checkout -b fix/auth-token-refresh
```

### 4️⃣ Make Your Changes
Make your changes to the codebase, JSON blueprints, or markdown prompts. Make sure your changes:
- Are well documented.
- Do NOT expose any API keys or secrets (check your `.env` files!).
- Follow the existing formatting and structure of the repository.

### 5️⃣ Commit & Push
Commit your changes with a clear and descriptive message:
```bash
git add .
git commit -m "feat(workflow): Add new HubSpot lead sync blueprint for Make.com"
git push origin feature/hubspot-sync-workflow
```

### 6️⃣ Open a Pull Request (PR)
Go back to the original repository on GitHub. You should see a prompt to open a **Pull Request**. Provide a detailed description of what you changed, why you changed it, and if it resolves any open issues.

---

## 🛠️ What Can You Contribute?

Here are some of the areas where we'd love your help:

| Contribution Area | What We Need | Location |
| :--- | :--- | :--- |
| **⚡ Next.js / Code** | UI improvements for the agency dashboard, new API routes for webhooks. | `/src/`, `/api/` |
| **🗄️ Automations (No-Code)** | Exported `.json` workflows from Zapier, n8n, or Make.com solving specific business problems. | `/workflows/blueprints/` |
| **🎙️ Voice Prompts** | Better system prompts (`system_prompt`), function-calling definitions, or edge-case handling for Voice Agents. | `/playbooks/voice/` |
| **📚 Documentation** | Typos, clearer instructions, or adding new tutorials/case studies. | `/docs/` and `README.md` |

---

## 📝 Code & Workflow Standards

To keep the repository clean and maintainable, please follow these guidelines:

### For Code (Node.js/Python/React):
- **Use clear variable names**: `fetchHubspotLead()` instead of `doThing()`.
- **Environment variables**: Never hardcode keys. Use `process.env.API_KEY` and provide a `.env.example`.
- **Formatting**: We use Prettier / ESLint. Please ensure your code lints cleanly before committing.

### For No-Code Workflows (JSON Blueprints):
- **Remove Auth Data**: Before exporting a blueprint from Make.com or n8n, make sure you UNLINK your actual API connections/credentials.
- **Node Naming**: Rename the nodes in your automation to be descriptive (e.g., "Parse Stripe Webhook" instead of "Webhook 1").
- **Readme file**: Include a small `README.md` alongside your `.json` file explaining what the automation does and what apps are required.

---

## 🚨 Reporting Bugs & Requesting Features

If you find a bug or have a great idea for a new AI workflow or agent:
1. Check the **[Issues](../../issues)** tab to see if it has already been reported.
2. If not, open a new Issue using the appropriate template (Bug Report or Feature Request).
3. Provide as much context as possible (screenshots, error logs, or specific use-cases).

---

<div align="center">
<b>Thank you for helping us build the ultimate AI Agency toolkit! 🚀</b>
</div>
