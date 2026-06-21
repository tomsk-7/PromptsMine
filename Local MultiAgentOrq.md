
# AI MULTI-AGENT ORCHESTRATOR (DOCKER LOCAL-FIRST)
## ROADMAP + MASTER PROMPT (PRODUCTION-READY DESIGN)

---

# 1. SYSTEM OVERVIEW

Sistema local-first containerizado con Docker para orquestación de IA multi-modelo con:

- Router de LLMs (Ollama + APIs fallback opcionales)
- Memoria persistente (Postgres + Qdrant)
- Orquestación tipo grafo (LangGraph / state machine)
- Ejecución de tareas async (Celery + Redis)
- Integración WhatsApp (webhook externo vía túnel)
- Pipeline de análisis financiero (news + crypto + Polymarket)
- Sistema de nodos de memoria evolutiva

---

# 2. ARCHITECTURE (DOCKER LOCAL-FIRST)

```

WhatsApp → Tunnel (Cloudflared / Ngrok)
↓
┌──────────────────────┐
│   DOCKER STACK       │
│──────────────────────│
│ FastAPI Orchestrator │
│ LangGraph Engine     │
│ Celery Worker        │
│ Redis (Queue)        │
│ Postgres (State)     │
│ Qdrant (Memory)      │
│ Ollama (Local LLMs)  │
└──────────────────────┘
↓
External APIs (optional fallback)

```

---

# 3. CORE DESIGN PRINCIPLES

- Local-first execution (Ollama priority)
- Stateless API layer / stateful memory layer
- Event-driven architecture
- Incremental memory graph growth
- Cost-zero reasoning policy (no APIs unless needed)
- Docker reproducibility
- Async task separation (API vs Worker)

---

# 4. TECH STACK (OPEN SOURCE ONLY)

## Core Runtime
- Python 3.11
- FastAPI
- Docker + Docker Compose

## Orchestration
- LangGraph / custom state machine

## LLM Layer
- Ollama
- Models: Llama 3, Mistral, Qwen, Phi-3

## Memory
- Qdrant (vector DB)
- PostgreSQL (structured state)

## Async
- Celery
- Redis

## Tunneling
- Cloudflared (recommended)

---

# 5. ROADMAP

---

## PHASE 0 — INFRA LOCAL (1–2 DAYS)

### Objective:
Bring up full local stack with Docker.

### Tasks:
- Install Docker + Compose
- Deploy:
  - Postgres
  - Redis
  - Qdrant
  - Ollama
- Load base LLMs (Llama / Mistral)

### Output:
✔ Local inference + DB + vector memory working

---

## PHASE 1 — BASIC ORCHESTRATOR (3–5 DAYS)

### Build:
- FastAPI webhook
- WhatsApp endpoint (receives messages)
- Basic LLM call (Ollama only)
- Postgres state storage

### Output:
✔ Chatbot functional in WhatsApp (local backend)

---

## PHASE 2 — LLM ROUTER (3–6 DAYS)

### Add:
- Multi-model routing system
- Priority inference:
  - Qwen/Mistral (fast tasks)
  - Llama 3 (reasoning)
  - Phi-3 (fallback)
- External API fallback optional

### Output:
✔ Intelligent model switching system

---

## PHASE 3 — MEMORY GRAPH SYSTEM (5–10 DAYS)

### Add:
- Qdrant vector memory
- Node system:
  - event nodes
  - insight nodes
  - decision nodes
  - summary nodes
- embedding pipeline (sentence-transformers)

### Output:
✔ Persistent evolving AI memory system

---

## PHASE 4 — FINANCIAL INTELLIGENCE LAYER (5–10 DAYS)

### Add:
- Crypto data ingestion (CoinGecko / Binance public)
- Polymarket scraping
- RSS news ingestion
- Sentiment analysis (local transformers optional)

### Output:
✔ Market intelligence engine local

---

## PHASE 5 — ASYNC AUTONOMY SYSTEM (3–7 DAYS)

### Add:
- Celery workers separation
- Night jobs:
  - daily market summary
  - memory compression
  - insight generation
- Scheduler (Celery beat or APScheduler)

### Output:
✔ Autonomous offline intelligence cycles

---

## PHASE 6 — WHATSAPP PRODUCTION HARDENING (2–5 DAYS)

### Add:
- Cloudflared tunnel
- retry queue system
- message buffering
- rate limiting
- webhook resilience

### Output:
✔ Stable production chat interface

---

## PHASE 7 — OPTIMIZATION & SCALING (ONGOING)

- Model quantization (gguf / llama.cpp)
- GPU acceleration (optional)
- caching embeddings
- memory pruning strategies
- latency optimization
- worker separation scaling

---

# 6. MASTER SYSTEM PROMPT (ORCHESTRATOR)

---

## ROLE

You are a local-first AI orchestration engine operating inside a Dockerized multi-service system.

You manage:
- LLM routing (Ollama-first)
- memory graph evolution
- tool execution (financial + data ingestion)
- structured reasoning pipelines
- asynchronous task delegation

---

## CORE RULES

### 1. LOCAL-FIRST POLICY
Always prefer local models before external APIs.

### 2. MEMORY-FIRST POLICY
Always retrieve relevant memory nodes before reasoning.

### 3. ZERO-COST DEFAULT
External APIs are fallback only when local inference fails.

---

## LLM ROUTING LOGIC

Priority order:

1. Qwen / Mistral → fast execution
2. Llama 3 → reasoning tasks
3. Phi-3 → lightweight fallback
4. External APIs → last resort

---

## MEMORY SYSTEM

Each interaction can generate:

- event node
- insight node
- decision node
- summary node

Each node must include:
- timestamp
- embedding vector
- confidence score
- relational edges

---

## TOOL USAGE

Use tools when:
- financial data is required
- external validation is required
- real-time data ingestion is needed

---

## OUTPUT FORMAT (STRICT)

Return structured JSON:

{
  "intent": "",
  "memory_used": [],
  "llm_selected": "",
  "tools_used": [],
  "analysis": "",
  "final_answer": "",
  "new_nodes": []
}

---

## AUTONOMY MODE (NIGHT CYCLE)

When scheduled task is triggered:

- ingest external data
- aggregate market signals
- compress memory graph
- generate structured insights
- store summary nodes

---

## FINANCIAL MODE

If financial intent detected:

- retrieve correlated assets
- analyze sentiment
- evaluate macro context
- generate directional bias (non-advisory)
- assign confidence score

---

# 7. SYSTEM DESIGN PRINCIPLES

- deterministic orchestration layer
- modular microservices via Docker Compose
- separation of API / Worker / Memory layers
- incremental graph-based memory evolution
- event-driven async execution
- reproducible local-first deployment
- external API minimization strategy

---

# END OF DESIGN

