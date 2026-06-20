
# Arquitectura Institucional v1.0 — Plataforma Multiagente Financiera

## 1. Principios de Arquitectura

La plataforma debe diseñarse como un sistema distribuido local-first con capacidad de evolucionar posteriormente hacia una arquitectura cloud híbrida.

### Objetivos Estratégicos

| Objetivo             | Prioridad |
| -------------------- | --------- |
| Modularidad          | Crítica   |
| Escalabilidad        | Crítica   |
| Tolerancia a fallos  | Crítica   |
| Coste operativo      | Alta      |
| Observabilidad       | Alta      |
| Seguridad            | Alta      |
| Aprendizaje continuo | Alta      |

---

# 2. Arquitectura Global

```text
┌─────────────────────────────┐
│         WhatsApp            │
└─────────────┬───────────────┘
              │
              ▼
┌─────────────────────────────┐
│     WhatsApp Gateway        │
│ (Evolution API / Baileys)   │
└─────────────┬───────────────┘
              │
              ▼
┌─────────────────────────────┐
│         FastAPI API         │
│        API Gateway          │
└─────────────┬───────────────┘
              │
              ▼
┌─────────────────────────────────────────┐
│            SupervisorAgent              │
│        LangGraph Orchestrator           │
└────┬───────┬────────┬────────┬──────────┘
     │       │        │        │
     ▼       ▼        ▼        ▼

 Macro   Fundamental Quant   News
 Agent     Agent      Agent  Agent

     ▼       ▼        ▼       ▼

 Portfolio Agent
 Risk Agent
 Research Agent
 Learning Agent
 Execution Agent

              │
              ▼

     Intelligent LLM Router

 Gemini → DeepSeek → Qwen → Mistral → Ollama

              │
              ▼

 ┌────────────────────────────┐
 │      Memory Layer          │
 ├────────────────────────────┤
 │ PostgreSQL                 │
 │ Qdrant                     │
 │ Object Storage             │
 └────────────────────────────┘

              │
              ▼

 ┌────────────────────────────┐
 │ Observability Layer        │
 ├────────────────────────────┤
 │ Prometheus                 │
 │ Grafana                    │
 │ Loki                       │
 └────────────────────────────┘
```

---

# 3. Componentes Principales

## API Gateway

Tecnología:

* FastAPI

Responsabilidades:

* Exposición REST
* Webhooks WhatsApp
* Autenticación
* Rate Limiting
* Auditoría

Ventajas:

* Muy rápido
* Asíncrono
* Excelente integración Python

Complejidad:

Baja

---

# 4. Motor Multiagente

## LangGraph

Razón:

Los flujos financieros rara vez son lineales.

Necesitamos:

* Estados persistentes
* Checkpoints
* Branching
* Human in the Loop
* Recuperación de errores

LangGraph es superior a cadenas simples de LangChain para este caso.

---

# 5. Diseño de Agentes

## SupervisorAgent

Responsable:

```text
- Asignación de tareas
- Coordinación
- Validación cruzada
- Resolución de conflictos
- Quality scoring
```

Salida:

```python
class SupervisorDecision(BaseModel):
    confidence: float
    consensus_score: float
    final_recommendation: str
    dissenting_opinions: list[str]
```

---

## MacroAgent

Inputs:

```text
FED
ECB
BOJ
PBOC
Inflación
Yield Curve
M2
Liquidez Global
```

Outputs:

```python
class MacroReport(BaseModel):
    liquidity_score: float
    recession_probability: float
    inflation_trend: str
    macro_bias: str
```

---

## FundamentalAgent

Analiza:

```text
10-K
10-Q
Balance Sheet
Cash Flow
Income Statement
```

Métricas:

```text
ROE
ROIC
FCF
P/E
EV/EBITDA
Margins
```

---

## QuantAgent

Capacidades:

```text
Factor Models
Correlations
Monte Carlo
Backtesting
Regime Detection
```

Bibliotecas:

```text
numpy
pandas
polars
statsmodels
scipy
vectorbt
```

---

## RiskAgent

Calcula:

```text
VaR
CVaR
Expected Shortfall
Stress Testing
Max Drawdown
Risk Contribution
```

---

## PortfolioAgent

Funciones:

```text
Asset Allocation
Risk Parity
Exposure Analysis
Rebalancing
```

---

## NewsAgent

Fuentes:

* RSS
* SEC
* ECB
* FED
* Reuters
* FRED
* Yahoo Finance

Funciones:

```text
Clasificación
Resumido
Impacto
Sentimiento
```

---

## ResearchAgent

Fusiona:

```text
Macro
Fundamental
Quant
Risk
News
```

Produce:

```text
Institutional Research Report
```

---

## LearningAgent

Responsable:

```text
Tracking predicciones
Medición de precisión
Análisis de errores
Aprendizaje continuo
```

---

## ExecutionAgent

No ejecuta órdenes reales inicialmente.

Simula:

```text
VWAP
TWAP
Icebergs
Liquidity Sweeps
```

---

# 6. Sistema de Memoria

## PostgreSQL

Almacena:

```text
Usuarios
Conversaciones
Análisis
Predicciones
Resultados
Métricas
Configuraciones
```

### Tablas

```sql
users
conversations
research_reports
predictions
prediction_results
portfolio_snapshots
risk_reports
agent_runs
model_metrics
```

---

## Qdrant

Almacena:

```text
PDFs
Libros
Research
Noticias
Memorias
Estrategias
```

Colecciones:

```text
research_docs
books
market_news
internal_reports
lessons_learned
```

Embeddings:

```text
bge-large
multilingual-e5-large
```

---

# 7. RAG Institucional

Pipeline:

```text
Documento
    ↓
Chunking Semántico
    ↓
Embeddings
    ↓
Qdrant
    ↓
Hybrid Search
    ↓
Reranker
    ↓
Context Builder
    ↓
LLM
```

Recomendación:

```text
Qdrant Hybrid Search
+
BGE Reranker
```

---

# 8. Router Inteligente de Modelos

## Objetivo

Optimización automática.

### Métricas

```python
class ProviderMetrics(BaseModel):
    latency_ms: float
    token_cost: float
    success_rate: float
    quality_score: float
```

---

### Política

```text
Gemini
↓
DeepSeek
↓
Qwen
↓
Mistral
↓
Ollama
```

---

### Selección dinámica

Score:

```text
Quality 40%
Latency 25%
Cost 20%
Availability 15%
```

---

# 9. Flujo de Investigación

Comando:

```text
/ research AAPL
```

Workflow:

```text
Supervisor
      ↓
MacroAgent
      ↓
FundamentalAgent
      ↓
QuantAgent
      ↓
NewsAgent
      ↓
RiskAgent
      ↓
PortfolioAgent
      ↓
ResearchAgent
      ↓
Supervisor Validation
      ↓
Report Generator
```

---

# 10. Metodología Institucional

Cada análisis debe contener:

```text
1 Contexto Macro
2 Contexto Fundamental
3 Contexto Cuantitativo
4 Contexto Técnico
5 Liquidez Institucional
6 Riesgo
7 Escenario Alcista
8 Escenario Bajista
9 Escenario Probable
10 Invalidación
11 Conclusión
```

Formato JSON obligatorio:

```python
class InstitutionalResearch(BaseModel):
    macro: dict
    fundamental: dict
    quantitative: dict
    technical: dict
    liquidity: dict
    risk: dict
    bullish_case: dict
    bearish_case: dict
    base_case: dict
    invalidation: dict
    conclusion: dict
```

---

# 11. Aprendizaje Continuo

## Ciclo

```text
Investigación
      ↓
Predicción
      ↓
Registro
      ↓
Resultado Real
      ↓
Comparación
      ↓
Evaluación
      ↓
Aprendizaje
```

---

### Métricas

```text
Accuracy
Hit Rate
Sharpe
Sortino
Alpha
Beta
Max DD
```

---

### Knowledge Evolution

LearningAgent generará:

```text
Errores recurrentes
Sesgos
Factores predictivos
Factores inválidos
```

---

# 12. Observabilidad

## Prometheus

Recolecta:

```text
CPU
RAM
GPU
Disco
Network
Tokens
Coste
Errores
Latencia
```

---

## Grafana Dashboards

### Sistema

```text
Infraestructura
```

### IA

```text
Tokens
Costes
LLM
```

### Agentes

```text
Tiempo ejecución
Errores
Calidad
```

### Research

```text
Predicciones
Rentabilidad
Precisión
```

---

# 13. Seguridad

## Secretos

```text
Docker Secrets
.env
Vault (fase futura)
```

---

## Control de acceso

RBAC:

```text
admin
analyst
viewer
```

---

## Auditoría

Loki

Registros:

```text
Prompt
Respuesta
Modelo
Coste
Usuario
```

---

# 14. Docker Compose

Servicios iniciales:

```yaml
postgres
qdrant

api

supervisor-agent
macro-agent
fundamental-agent
quant-agent
risk-agent
portfolio-agent
news-agent
research-agent
learning-agent
execution-agent

prometheus
grafana
loki

ollama

whatsapp-gateway
```

---

# 15. Estructura de Carpetas

```text
financial-platform/

├── apps
│   ├── api
│   └── whatsapp
│
├── agents
│   ├── supervisor
│   ├── macro
│   ├── fundamental
│   ├── quant
│   ├── risk
│   ├── portfolio
│   ├── news
│   ├── research
│   ├── learning
│   └── execution
│
├── core
│   ├── llm_router
│   ├── rag
│   ├── memory
│   ├── workflows
│   ├── observability
│   └── security
│
├── infrastructure
│   ├── postgres
│   ├── qdrant
│   ├── grafana
│   ├── prometheus
│   └── docker
│
├── reports
├── tests
├── docs
└── scripts
```

---

# 16. Estrategia de Despliegue

## Fase Local

Equipo personal.

```text
Docker Compose
Ollama
PostgreSQL
Qdrant
```

Coste:

≈ 0 €/mes

---

## Fase Homelab

Mini servidor dedicado.

Ejemplo:

* Ryzen 9
* 128 GB RAM
* RTX 4090

---

## Fase Enterprise

Cluster:

* Kubernetes
* PgBouncer
* Redis
* Kafka
* MinIO

---

# 17. Roadmap de Ejecución

## Fase 1 — Fundación

Duración:

2-3 semanas

Objetivos:

* FastAPI
* PostgreSQL
* Qdrant
* Docker
* WhatsApp
* Observabilidad básica

Entregables:

```text
Arquitectura operativa mínima
```

Riesgo:

Bajo

---

## Fase 2 — Memoria y RAG

Duración:

2 semanas

Objetivos:

* Ingesta PDFs
* Embeddings
* Qdrant
* Retrieval

Entregables:

```text
Sistema RAG funcional
```

Riesgo:

Medio

---

## Fase 3 — Router LLM

Duración:

1 semana

Objetivos:

* Gemini
* DeepSeek
* Qwen
* Ollama

Entregables:

```text
Fallback automático
```

Riesgo:

Bajo

---

## Fase 4 — Multiagente

Duración:

3-4 semanas

Objetivos:

* LangGraph
* Supervisor
* Macro
* Fundamental
* Quant
* News

Entregables:

```text
Investigación automática
```

Riesgo:

Alto

---

## Fase 5 — Riesgo y Portfolio

Duración:

2 semanas

Objetivos:

* RiskAgent
* PortfolioAgent

Entregables:

```text
Análisis institucional completo
```

Riesgo:

Medio

---

## Fase 6 — Learning System

Duración:

3 semanas

Objetivos:

* Tracking
* Evaluación
* Feedback Loop

Entregables:

```text
Aprendizaje continuo real
```

Riesgo:

Alto

---

## Fase 7 — Producción Institucional

Duración:

4 semanas

Objetivos:

* Hardening
* Auditoría
* Backup
* Optimización

Entregables:

```text
Plataforma v1.0
```

Riesgo:

Medio

---

## Recomendación CTO

No construir inicialmente los 10 agentes como microservicios independientes. La mejor relación complejidad/beneficio en un entorno local es:

```text
FastAPI
+
LangGraph
+
Agentes como módulos Python
+
PostgreSQL
+
Qdrant
+
Ollama
+
WhatsApp Gateway
```

Y evolucionar a microservicios únicamente cuando existan problemas reales de escalabilidad. Esto reduce entre un 60% y un 70% la complejidad operativa inicial manteniendo la capacidad de crecimiento hacia una arquitectura institucional.


