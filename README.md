<div align="center">

# ✈️ TripMaker AI
### Multi-Agent Travel Planner Orchestrated with LangGraph

[![Python 3.11+](https://img.shields.io/badge/Python-3.11+-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![LangGraph](https://img.shields.io/badge/Orchestration-LangGraph-FF6F00?style=for-the-badge)](https://langchain-ai.github.io/langgraph/)
[![FastAPI](https://img.shields.io/badge/Backend-FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com/)
[![Groq](https://img.shields.io/badge/LLM_Inference-Groq-F55036?style=for-the-badge)](https://groq.com/)
[![PostgreSQL](https://img.shields.io/badge/Database-PostgreSQL-4169E1?style=for-the-badge&logo=postgresql&logoColor=white)](https://www.postgresql.org/)
[![Repository: Private](https://img.shields.io/badge/Source_Code-Private-critical?style=for-the-badge&logo=github)](mailto:shubham.shivare9112@gmail.com)

<p align="center">
  An autonomous multi-agent travel orchestration engine that converts unstructured natural-language requests into verified, budget-aligned, end-to-end travel plans.
</p>

<!-- Optional Demo Links -->
<!-- [🌐 Live Web App](https://your-demo-url.com) • [📄 Project Documentation](https://shubh9966.github.io/repo-name/) -->

</div>

---

## 🏗️ Architecture & Orchestration

Every request is evaluated sequentially across specialized nodes within a LangGraph state graph:

```mermaid
flowchart LR
    classDef default fill:#1e1e2e,stroke:#89b4fa,stroke-width:2px,color:#cdd6f4;
    classDef terminal fill:#313244,stroke:#a6adc8,stroke-width:1.5px,color:#cdd6f4;

    START((Start)):::terminal --> G{🛡️ Travel Guardrail}
    G -- Not Travel / Abuse --> END((Exit)):::terminal
    G -- Valid Request --> A[🛫 Flight Agent]
    A --> B[🏨 Hotel Agent]
    B --> C[🗓️ Itinerary Agent]
    C --> D[📝 Final Synthesis Agent]
    D --> END((End)):::terminal
```

---

## 🔒 Source Code Access Notice

> **Note:** The core backend implementation, prompt pipelines, and specialized custom agent tools are hosted in an authenticated private repository.  
> If you are a recruiter, collaborator, or enterprise looking to review the source code or evaluate integration, please reach out via email: **[shubham.shivare9112@gmail.com](mailto:shubham.shivare9112@gmail.com)**.

---

## 🌟 System Overview & Key Capabilities

- **Autonomous Agentic Flow:** Chains live flight extraction, web-verified accommodations, and contextual sightseeing curation into a single deterministic execution cycle.
- **Persistent State Graphs:** Leverages `PostgresSaver` via `langgraph-checkpoint-postgres` to support resumeable sessions and thread-based memory across interactions.
- **Fast Inference Pipeline:** Powered by Groq-hosted LPUs for low-latency multi-agent decision steps.
- **Production Guardrails:** Incorporates a front-door zero-temperature classifier to detect and abort non-travel prompts and injection attempts before invoking expensive external APIs.
- **Rate Limiting Engine:** Per-IP sliding window rate limiting backed by PostgreSQL with an in-memory fallback mechanism to shield against API abuse.
- **Document Exporting:** Dynamic rendering to cleanly styled A4 PDFs and structured Markdown with active table preservation.

---

## 🛠️ Complete Tech Stack

| Domain | Technologies |
|---|---|
| **Core Framework** | Python 3.11+, FastAPI, Uvicorn, Pydantic |
| **AI & Agents** | LangGraph, LangChain, Groq (`langchain-groq`) |
| **Search & Discovery** | Tavily Search API, `airportsdata`, `pycountry` |
| **Storage & Checkpointing** | PostgreSQL, `psycopg`, `langgraph-checkpoint-postgres` |
| **Frontend & UI** | Jinja2 Templates, Vanilla JS/CSS, `marked.js`, `html2pdf.js` |
| **Containerization** | Docker (`python:3.11-slim`) |


---

## 🛡️ Production Guardrails & Rate Limiting

- **Intent Classifier:** Every request is first evaluated by a zero-temperature classification guardrail. Non-travel requests exit cleanly before triggering any external tools or LLM nodes.

---

## 📬 Contact & Inquiries

For demonstrations, code audits, or technical collaboration:
- **Email:** [shubham.shivare9112@gmail.com](mailto:shubham.shivare9112@gmail.com)
- **GitHub:** [@Shubh9966](https://github.com/Shubh9966)
