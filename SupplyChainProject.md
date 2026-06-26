# Supply Chain Macro Platform — Roadmap

## Objetivo
Construir una plataforma de inteligencia macroeconómica basada en supply chain mapping, que conecte:
macro → sectores → industrias → empresas → inputs físicos → commodities → shocks

---

# FASE 0 — Diseño conceptual (Semana 1)

## Objetivo
Definir ontología y modelo de datos

### Entregables
- [ ] Definición de nodos:
  - MacroFactor
  - Sector
  - Industry
  - Company
  - Commodity
  - InputMaterial

- [ ] Definición de relaciones:
  - DEPENDS_ON
  - SUPPLIES
  - IMPACTED_BY
  - CORRELATED_WITH
  - DRIVES

- [ ] Diseño del grafo base (manual)

### Resultado
Mapa conceptual funcional del sistema

---

# FASE 1 — MVP de grafo (Semana 2–3)

## Objetivo
Construir sistema funcional sin UI compleja

### Stack
- Python
- NetworkX
- Pandas

### Entregables
- [ ] Grafo inicial (50–100 nodos)
- [ ] Dataset manual de:
  - NVIDIA
  - TSMC
  - ASML
  - Silicon
  - Energy

- [ ] Motor básico de propagación de impacto

### Funcionalidad
- Simular shocks:
  - subida petróleo
  - escasez chips
  - subida tipos interés

---

# FASE 2 — Data ingestion (Semana 4–5)

## Objetivo
Automatizar carga de datos externos

### Fuentes
- FRED (macro)
- Yahoo Finance (mercados)
- World Bank
- EIA (energía)

### Stack
- Python
- Prefect (ETL orchestration)

### Entregables
- [ ] Pipeline ETL básico
- [ ] Normalización de entidades
- [ ] Dataset dinámico inicial

---

# FASE 3 — Knowledge Graph real (Semana 6–7)

## Objetivo
Migrar de grafo en memoria a base de datos real

### Stack
- Neo4j (principal)
- PostgreSQL (metadata)

### Entregables
- [ ] Schema de grafos en Neo4j
- [ ] API de inserción de nodos
- [ ] API de relaciones

### Resultado
Sistema persistente y consultable

---

# FASE 4 — Motor de inferencia (Semana 8–9)

## Objetivo
Convertir grafo en sistema causal

### Funcionalidades
- propagación de shocks
- cálculo de exposición
- scoring de riesgo sistémico

### Entregables
- [ ] Engine de impacto ponderado
- [ ] Query engine:
  - “impacto de subida de petróleo”
  - “riesgo de semiconductores”

---

# FASE 5 — Capa IA (Semana 10–11)

## Objetivo
Añadir inteligencia semántica

### Stack
- Qdrant (vector DB)
- Embeddings open source

### Entregables
- [ ] RAG de noticias macroeconómicas
- [ ] Linking automático de eventos → nodos
- [ ] Contextualización de shocks

---

# FASE 6 — API y servicio (Semana 12)

## Objetivo
Exponer sistema como backend

### Stack
- FastAPI

### Endpoints
- /graph/query
- /impact/simulate
- /node/{id}
- /relations/{id}

---

# FASE 7 — UI (Semana 13–14)

## Objetivo
Visualización del sistema

### Opción A (MVP rápido)
- Obsidian + Juggl + Dataview

### Opción B (producto)
- React
- D3.js force graph

### Entregables
- [ ] Visualización del grafo
- [ ] filtros por sector
- [ ] simulación interactiva

---

# FASE 8 — Producto avanzado (Futuro)

## Objetivo
Evolución a sistema tipo Bloomberg macro supply chain

### Features
- simulación global de shocks
- alertas macro automáticas
- scoring de riesgo sistémico
- correlaciones dinámicas

---

# KPIs del sistema

- Cobertura de nodos (>1.000 entidades)
- Latencia de query (<300ms API)
- Precisión de propagación (>70% validación histórica)
- Eventos detectados correctamente (backtesting)

---

# Arquitectura final objetivo

- Graph DB (Neo4j)
- RAG (Qdrant)
- API (FastAPI)
- ETL (Prefect)
- Analytics (Python)
- UI (React / Obsidian bridge)

---

# Resultado final

Sistema de inteligencia macroeconómica basado en supply chain capaz de:

- mapear economía global como grafo
- simular shocks sistémicos
- analizar dependencias reales entre activos
- generar insights accionables