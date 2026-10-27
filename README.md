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

| Agent / Node | Primary Responsibility | Backing Tech / Tools |
|---|---|---|
| **🛡️ Travel Guardrail** | Validates intent, sanitizes inputs, and filters prompt injections before agent handoff. | Groq Classification Node |
| **🛫 Flight Agent** | Extracts routes, dates, and origin/destination constraints to resolve flight segments. | `airportsdata`, `pycountry` |
| **🏨 Hotel Agent** | Conducts web discovery for stay recommendations matching requested budget tiers. | Tavily Search API |
| **🗓️ Itinerary Agent** | Combines flight and hotel contexts to construct realistic day-by-day travel blocks. | Groq LLM Planner |
| **📝 Synthesis Agent** | Formats final combined responses, sets metadata, and renders markdown structures. | LangGraph State Reducer |

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

## 🚀 Quick Start (Authorized Access Setup)

### Prerequisites

- **Python 3.11+**
- **PostgreSQL Database** (e.g., Supabase, Neon, or local instance)
- **API Keys:** Groq Console & Tavily Search

### 1. Environment Setup

```bash
# Clone the repository (Access required)
git clone https://github.com/Shubh9966/TripMakerAi---A-multi-agent-travel-planner-with-langgraph.git
cd TripMakerAi---A-multi-agent-travel-planner-with-langgraph

# Create and activate virtual environment
python -m venv venv
source venv/bin/activate       # Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

### 2. Configure Environment Variables

Create a `.env` file in the root directory:

```env
GROQ_API_KEY=your_groq_api_key_here
DATABASE_URL=postgresql://user:password@host:port/dbname?sslmode=require
TAVILY_API_KEY=your_tavily_api_key_here

# Optional Configurations
GUARDRAIL_MODEL=openai/gpt-oss-120b        # Model used by the travel guardrail
RATE_LIMIT_MAX_REQUESTS=2                  # Allowed requests per IP per window
RATE_LIMIT_WINDOW_SECONDS=3600             # Window length (1 hour)
RATE_LIMIT_CONTACT_EMAIL=shubham.shivare9112@gmail.com
TRUSTED_PROXY_HOPS=1                       # Reverse proxy hop count (e.g., Cloudflare/Render)
```

### 3. Run Application

```bash
uvicorn app:app --reload
```

- **Web UI:** Visit `http://127.0.0.1:8000`
- **Interactive Swagger Docs:** Visit `http://127.0.0.1:8000/docs`

---

## 🐳 Docker Deployment

Run containerized without local Python dependencies:

```bash
# Build Docker image
docker build -t tripmaker-ai .

# Run container with environment bindings
docker run -d -p 8000:8000 --env-file .env --name tripmaker-app tripmaker-ai
```

---

## 🔌 API Reference

### Execute Travel Graph

`POST /api/travel`

Runs the multi-agent orchestration pipeline and checkpointing lifecycle.

#### Request Body
```json
{
  "message": "Plan a 4 days budget trip to Vietnam from Mumbai",
  "thread_id": "optional-thread-session-id"
}
```

#### Response (`200 OK`)
```json
{
  "success": true,
  "thread_id": "user_20261026_x89a",
  "answer": "# Complete 4-Day Vietnam Itinerary\n...",
  "flight_results": "...",
  "hotel_results": "...",
  "itinerary": "...",
  "llm_calls": 4
}
```

#### Error Responses

| Status | `error_code` | Reason |
|---|---|---|
| `400` | – | Empty message body |
| `400` | `message_too_long` | Message exceeds 1500 characters |
| `400` | `not_travel_request` | Guardrail identified non-travel prompt |
| `429` | `rate_limited` | Quota exceeded (2 requests/hour per IP) |
| `500` | – | Internal execution error |

```json
{
  "success": false,
  "error_code": "rate_limited",
  "error": "Rate limit reached (2 requests per hour per IP). Please try again in 30 minutes. Reach out at shubham.shivare9112@gmail.com to request access.",
  "retry_after": 1800,
  "limit": 2,
  "contact_email": "shubham.shivare9112@gmail.com"
}
```

---

## 🛡️ Production Guardrails & Rate Limiting

- **Intent Classifier:** Every request is first evaluated by a zero-temperature classification guardrail. Non-travel requests exit cleanly before triggering any external tools or LLM nodes.
- **Sliding-Window Limiter:** Tracks requests by IP address within a sliding 1-hour window, persisted directly in PostgreSQL (`rate_limit_hits`). An automatic in-memory fallback engages if the database is unreachable.
- **Reverse Proxy Handling:** Inspects the `X-Forwarded-For` header using trusted hop counts to reliably resolve client IPs behind proxies such as Render or Cloudflare.

---

## 📬 Contact & Inquiries

For demonstrations, code audits, or technical collaboration:
- **Email:** [shubham.shivare9112@gmail.com](mailto:shubham.shivare9112@gmail.com)
- **GitHub:** [@Shubh9966](https://github.com/Shubh9966)
