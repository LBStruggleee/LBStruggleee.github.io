### KnowFlow · Traceable RAG Knowledge Base

<p class="project-meta"><strong>Solo developer (full-stack)</strong> · 2024.09.15 – 2026.02.20 · Python 3.13 · FastAPI · SQLAlchemy · SQLite (WAL + FTS5) · Chroma · DashScope (Qwen / text-embedding-v3) · Vue 3 + Vite + Element Plus · pytest / Vitest / GitHub Actions</p>

A traceable RAG knowledge base for teams and individuals that keeps model answers grounded in the user's own documents instead of letting them improvise, and makes the whole retrieval process auditable. Users can create and organise multiple knowledge bases and upload txt, md, pdf, docx and pptx files; the system parses each document asynchronously, builds a Section tree from heading levels, splits text into paragraph-aware chunks, and writes them to both the fact database and the vector index.

Retrieval combines three channels — FTS5 lexical search, semantic vectors and RRF fusion — and can be switched between vector / lexical / hybrid in settings. Matching chunks, similarity scores and section paths are handed to Qwen, which produces answers with inline `[资料 N]` citations and a retrieval trace. A unified refusal-threshold gate runs before every answer, so the system declines instead of calling the LLM when evidence is insufficient. Five study modes (question / explain / compare / summarize / exercise), three privacy modes (local / hybrid / cloud), multi-turn conversations with reference resolution, and one-click note / exercise / mistake records round out the product.

<p class="project-label">Key Contributions</p>

- **Architecture boundaries**: Established the three-layer split — SQLite for fact data, Chroma for a rebuildable vector index, cloud models as a degradable external capability. Without an API key the system falls back to deterministic local embeddings and extractive answers, and the index can be rebuilt at any time without losing uploaded documents.
- **Hybrid retrieval**: Implemented FTS5 Chinese bigram tokenisation, a vector channel and RRF fusion, together with a unified refusal-threshold semantic.
- **Documents and prompts**: Built structured document parsing and Section-aware chunking, and added prompt-injection defences.
- **Reliability**: Handled WAL concurrency, persisting messages before calling the LLM, re-entrancy guards and failure compensation for index rebuilds, and orphan-vector reconciliation at startup.
- **Engineering and quality**: Built a CI suite of 176 pytest and 47 Vitest tests; reduced the front-end main bundle from 1.17 MB to 473 KB.
- **Operations and delivery**: Added status statistics, a provider health panel, streaming data export and confirm-before-cleanup; the backend also serves the front-end assets, with Docker and docker-compose deployment.

[[Code]](https://github.com/LBStruggleee/KnowFlow)

---

### DataQuery Copilot · Natural-Language Data Querying

<p class="project-meta"><strong>Solo developer (full-stack)</strong> · 2025.03.12 – 2025.06.26 · Python 3.10+ · FastAPI · SQLite (WAL) · DeepSeek API (OpenAI-compatible) · pandas · matplotlib · React + TypeScript + Vite · Playwright · Docker / Render</p>

A data-querying tool that connects the full pipeline — natural language → SQL generation → safety validation → execution → data cleaning → visual output. Users ask questions in natural language, DeepSeek generates SQLite statements from the schema, the query passes layered safety checks before execution, results are cleaned (missing-value filling, format normalisation) and visualised as bar, line, pie or scatter charts chosen from the shape of the data.

The system ships in two forms: a CLI (interactive Q&A, single queries, batch queries with a summary report) and an HTTP API + React workbench (sidebar for switching and uploading datasets, key entry, paginated results and chart previews). The platform includes API-key issuance and revocation, per-key sliding-window rate limits and daily quotas, a unified error-code contract and request_id tracing, making the API layer safe to expose. The data layer imports CSV / Excel (.xlsx) into SQLite with automatic type inference, protected by 50 MB and dataset-count limits.

<p class="project-label">Key Contributions</p>

- **Data ingestion**: CSV / Excel import with automatic type inference, three CLI modes (interactive / single / batch) and a FastAPI + React workbench.
- **SQL safety**: A validation layer that strips strings and comments, applies a word-boundary keyword blacklist and rejects multiple statements; an execution layer using `sqlite3` set_authorizer table allow-lists and set_progress_handler timeouts; plus automatic SQL-repair retries and an LRU query cache.
- **API and deployment**: API-key issuance and revocation, sliding-window rate limiting with daily quotas, unified error codes and request_id tracing; a non-root container image, a Render deployment blueprint (persistent disk, sample-database seeding, unauthenticated health probe) and an SQLite online-backup script.
- **Quality**: A CI suite of 252 pytest, 24 Vitest and 14 Playwright end-to-end tests.

[[Code]](https://github.com/LBStruggleee/DataQuery-Copilot)
