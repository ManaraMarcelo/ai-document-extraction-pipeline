# AI-Powered Structured Extraction Pipeline

![Python](https://img.shields.io/badge/Python-3.11%2B-3776AB?logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-ETL-150458?logo=pandas&logoColor=white)
![OpenAI](https://img.shields.io/badge/LLM-OpenAI-412991?logo=openai&logoColor=white)
![Google AI](https://img.shields.io/badge/LLM-Gemini-4285F4?logo=google&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-Data%20Layer-4169E1?logo=postgresql&logoColor=white)
![SQLAlchemy](https://img.shields.io/badge/SQLAlchemy-ORM-D71F00?logo=sqlalchemy&logoColor=white)

> **Production AI Engineering** — turning unstructured documents into reliable, traceable, operational data at scale.

---

## Overview

This project implements a **modular, AI-driven document processing pipeline** that transforms unstructured text into standardized tabular outputs ready for operational and analytical consumption.

Designed for high-volume document scenarios where raw text needs to be:
- Ingested from a persistent source
- Enriched before inference to improve LLM accuracy
- Processed by language models with **automatic provider fallback**
- Normalized to a stable data contract
- Persisted in a relational database
- Continuously evaluated with assertiveness metrics

The core problem this solves: **how to go from chaotic text to reliable, traceable, operationalizable data — without manual intervention on each item.**

---

## Architecture

The pipeline follows a **modular staged architecture** with clear separation between ingestion, enrichment, inference, normalization, persistence, and evaluation.

```
flowchart TD
    A[Reprocessing Trigger] --> B[Data Ingestion]
    B --> C[Pre-processing & Enrichment]
    C --> D[LLM Inference]
    D --> E[Key & Type Normalization]
    E --> F[Result Persistence]
    F --> G[Quality Evaluation]
    G --> H[Report & Metrics]

    D --> I[Fallback to Alternative Provider]
    I --> E
```

### Pipeline Flow

1. **Orchestrator** initializes execution, times each stage, and controls interruption on critical failure
2. **Ingestion** downloads and materializes input data for batch processing
3. **Enrichment** replaces low-quality content with a better version before inference — reducing noise and improving model precision
4. **Inference** sends batches to an LLM with automatic fallback between providers on failure, quota limits, or unavailability
5. **Normalization** applies semantic key resolution, synonym mapping, and heuristics to match the output contract
6. **Persistence** stores structured results in PostgreSQL
7. **Evaluation** measures assertiveness metrics per batch and generates a quality report

---

## Key Technical Challenges

### 1. Reliability in an AI-dependent environment
The pipeline implements **automatic fallback between LLM providers**, with explicit error handling and dynamic strategy switching — preventing a single provider's downtime from halting the entire process.

### 2. Unreliable semi-structured output
LLMs don't always return labels that exactly match the internal contract. A **semantic normalization layer** with synonyms, heuristic fallback, and human-label resolution ensures consistency before writing the final key.

### 3. Input quality impacting inference quality
Instead of blindly sending raw text, the pipeline runs an **enrichment step** that substitutes content when a higher-quality version is available — significantly improving extraction accuracy.

### 4. Batch operation with full traceability
The orchestrator **measures time per stage**, standardizes logs, propagates return codes, and terminates predictably when needed — enabling reliable debugging and maintenance.

### 5. Large, type-sensitive data contract
Field configuration, semantic mapping, and type definitions are **separated from business logic**, reducing coupling between stages and making the schema easier to evolve and validate.

---

## Tech Stack

| Layer | Technology | Why |
|---|---|---|
| Core language | Python 3.11+ | Mature ecosystem for automation, data IO, and AI integration |
| Data processing | Pandas + OpenPyXL | Batch operations, column validation, Excel IO for legacy compatibility |
| AI inference | OpenAI + Gemini | Dual-provider strategy for operational resilience and fallback |
| Persistence | PostgreSQL + SQLAlchemy + psycopg2 | Reliable relational storage with fine-grained control |
| Configuration | dotenv | Environment-based config, no hardcoded credentials |

---

## Architecture Decisions

**Central orchestrator over standalone scripts** — a single pipeline with return codes and time measurement improves predictability, automation, and observability.

**Enrichment before inference, not just post-correction** — model output quality depends heavily on input quality. Pre-processing with intelligent content substitution reduces ambiguity upstream.

**Explicit normalization instead of trusting LLM literals** — semantic synonym layers decouple prompt design from persistence, making the system tolerant to natural LLM variability.

**Type contract separated from logic** — isolating field definitions and type mappings makes the project easier to evolve, validate, and version without cascading changes.

**Persistence decoupled from inference** — an intermediate artifact is generated before final insertion, creating an inspection and recovery layer for troubleshooting and selective reprocessing.

---

## Results

- ~70% reduction in manual document reading and structuring time
- ~60% reduction in human rework through semantic normalization
- Increased operational stability via automatic LLM provider fallback
- Improved output reliability with explicit field contract and type enforcement
- Full execution traceability with per-stage metrics and integrated evaluation

---

## What I'd Do Differently

1. **Replace spreadsheet-based flow with queues** — for larger scale, migrate to event-driven storage and versioned artifacts
2. **Add semantic regression tests** — test suite for prompts, synonyms, and normalization to catch prompt changes that degrade critical fields
3. **Richer observability** — per-item tracing, per-batch metrics, fallback rate, cost-per-inference, and near-real-time quality dashboards
4. **Formalize output contract with typed models** — strong input/output validation to reduce inter-stage inconsistency risk
5. **Hybrid extraction strategy** — at scale, combine LLM with deterministic rules, auxiliary classifiers, and domain validations to reduce cost and latency

---

## What I'd Do Differently

1. **Replace spreadsheet-based flow with queues** — for larger scale, migrate to event-driven storage and versioned artifacts instead of Excel intermediaries
2. **Add semantic regression tests** — a test suite for prompts, synonyms, and normalization to catch prompt changes that silently degrade critical fields
3. **Richer observability** — per-item tracing, per-batch metrics, fallback rate, cost-per-inference, and near-real-time quality dashboards
4. **Formalize output contract with typed models** — strong input/output validation (e.g. Pydantic) to reduce inter-stage inconsistency risk
5. **Hybrid extraction strategy** — at scale, combine LLM with deterministic rules, auxiliary classifiers, and domain validations to reduce cost and latency

---

## Author

**Marcelo Manara** — Software Engineer | AI Systems · Cloud · Python · AWS

[![LinkedIn](https://img.shields.io/badge/LinkedIn-marcelo--manara-0077B5?logo=linkedin&logoColor=white)](https://linkedin.com/in/marcelo-manara)
[![GitHub](https://img.shields.io/badge/GitHub-ManaraMarcelo-181717?logo=github&logoColor=white)](https://github.com/ManaraMarcelo)
[![Portfolio](https://img.shields.io/badge/Portfolio-portifolio--peach--beta.vercel.app-000000?logo=vercel&logoColor=white)](https://portifolio-peach-beta.vercel.app)
