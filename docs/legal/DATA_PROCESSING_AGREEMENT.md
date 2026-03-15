<div align="center">

# 🗄️ Data Processing Agreement (DPA)
**Crucial for EU (GDPR) or California (CCPA) Clients.**

</div>

<br />

A DPA legally defines how your agency handles personal data on behalf of your client. As an agency, you are the **"Data Processor"** and your client is the **"Data Controller"**.

---

### 1. Subject Matter of Processing
The Data Processor (Nexus AI Agency) will process personal data (e.g., customer names, phone numbers) solely for the purpose of executing the AI Autonmations specified in the Master Service Agreement.

### 2. Sub-Processors
The Data Controller authorizes the Data Processor to engage the following sub-processors to fulfill the services:
- **OpenAI, LLC** (For LLM processing. We utilize their Enterprise API strictly with Zero Data Retention).
- **Supabase, Inc.** (For encrypted PostgreSQL database hosting).
- **Make.com / Celonis** (For webhook routing).
- **Vapi.ai / ElevenLabs** (For text-to-speech and speech-to-text generation).

Processor shall notify Controller 30 days in advance of adding any new Sub-Processors.

### 3. Security of Processing
The Processor shall implement appropriate technical and organizational measures (e.g., encryption at rest, environment variable isolation, RBAC) to ensure a level of security appropriate to the risk.

### 4. Data Deletion
Upon termination of the Services, the Processor shall, at the choice of the Controller, delete or return all personal data to the Controller, unless local law requires storage of the personal data.
