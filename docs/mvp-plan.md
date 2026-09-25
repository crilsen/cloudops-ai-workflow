# CloudOps AI Workflow — MVP Plan

> Status: approved by the requester on 2026-09-25. This document is PRD-001 (index in `.ai/REQUIREMENTS.md`).
> Conventions: code comments in English; all Markdown in English; UI strings in pt-BR; never use the term "enterprise".

## 1. Pasted context (original request, no scope cuts)

### 1.1 Objective

Build an AI flow that speeds up operational investigations in cloud environments. The app
receives an alert or technical request, consults runbooks and operational context, runs
controlled query tools, and produces a structured diagnosis for human review.

The project must demonstrate: n8n · AI Agents · Context Engineering · RAG · MCP · API and
webhook integration · Observability · Guardrails · Human-in-the-loop · CloudOps and SRE.

### 1.2 Non-negotiable restrictions

- Portfolio; no real data, real credentials, or unrestricted AWS access.
- Synthetic alerts, logs, metrics, inventory, and runbooks.
- No term "enterprise" in titles, texts, names, README, or code.
- AI assists the investigation; never performs destructive or autonomous actions.
- UI strings in Portuguese; Markdown docs in English (Language-002, decided 2026-09-25).
- Functional MVP, not a complex platform.
- Code comments in English (requester, 2026-09-25).

### 1.3 Mandatory stack

- n8n as the primary orchestrator.
- Python backend with FastAPI.
- MCP Server in Python (controlled tools).
- SQLite for history.
- Next.js + TypeScript + Tailwind frontend (confirmed 2026-09-25; Streamlit fallback dropped).
- Docker Compose for all services.
- LLM with abstracted provider (Claude, OpenAI, or Gemini via env).
- `.env.example` with no real keys.

### 1.4 MVP scenario

Example alerts: "Elevated 5xx errors in payments-api", "High latency in checkout service",
"Unusual AWS WAF blocked requests", "Pod CrashLoopBackOff in production namespace".
The flow gathers synthetic context, proposes hypotheses, and suggests next steps with evidence.

### 1.5 Features (§2–§11 of the request)

1. **Alert intake** — page with title, service, severity (low/medium/high/critical),
   environment (development/staging/production), free-text description, timestamp,
   "Start investigation" button + 4 ready-made synthetic alerts.
2. **n8n workflow** — exportable JSON: webhook/manual → validation → context → agent →
   MCP → diagnosis → database → human review; with error handling, simple retry, basic logs,
   investigation correlation ID.
3. **Context Engineering + RAG** — local Markdown base in `knowledge/` (runbooks, catalog,
   simulated architecture, 5xx/latency/CrashLoopBackOff/WAF procedures, escalation,
   security); simple semantic search (embeddings + ChromaDB); the agent receives only
   relevant excerpts with traceable origin.
4. **MCP Server** — read-only: `get_service_catalog`, `search_runbooks`,
   `get_recent_logs`, `get_service_metrics`, `get_waf_events`, `get_kubernetes_status`;
   synthetic JSON responses; no writes, no arbitrary commands, no secrets, no unvalidated
   instructions from the LLM.
5. **AI agent** — versioned system prompt in `prompts/cloudops_investigator.md`;
   logs/runbooks are data, never instructions; no invented metrics/causes; fact ≠ hypothesis
   ≠ recommendation; cite origins; reversible safe actions; escalate risk to humans; never
   suggest production changes without approval; JSON output validated by Pydantic
   (`incident_summary`, `severity_assessment`, `observed_facts`, `hypotheses`,
   `recommended_actions`, `escalation_recommendation`, `limitations`).
6. **Result interface** — summary, severity, query timeline, observed facts, hypotheses with
   confidence, evidence/sources, recommended actions, approval status; approve, reject,
   comment, edit before approving, mark as done.
7. **Observability/LLMOps** — per investigation: ID, model, prompt_version, total time,
   per-step latency, tool-call count, estimated cost, JSON-validation status, human review;
   aggregate "Observability" page (total, mean time, errors, severity, approval rate).
8. **Security/guardrails** — "Safe AI Use" page (6 principles from the request) + simple
   prompt-injection detection in logs/runbooks with an alert in the result.
9. **Expected layout** — `apps/{api,web,mcp-server}/`, `workflows/`, `knowledge/`,
   `prompts/`, `sample-data/`, `evaluation/`, `docker-compose.yml`, `.env.example`, `README.md`.
10. **Evaluation** — synthetic cases validating valid JSON, justification, evidence/uncertainty,
    approval for risk, source citation, log-is-not-instruction; report in
    `evaluation/report.md`.
11. **README (English)** — problem, architecture, Mermaid diagram, n8n flow, local run,
    LLM provider setup, workflow import, demos, limitations, security, demonstrated skills.
12. **Build order** — synthetic data + backend/MCP first; then n8n workflow and interface;
    finally validate Compose, demos, and tests.

## 2. Decisions already made

See `.ai/DECISIONS.md` (ADR-001, TDR-001, ADR-002, Language-001, Language-002).
No stack choices pending (Next.js + ChromaDB confirmed).

## 3. Execution phases

| Phase | Deliverables | Validation |
| --- | --- | --- |
| 0 — Foundation (done) | Populated `.ai/`, this plan, public repo, `.gitignore`, all Markdown in English | Clean `git status`, push ok |
| 1 — Synthetic data | `sample-data/` (alerts/logs/metrics/waf-events) + `knowledge/` (.md, in English) | Versioned files; grep shows no "enterprise" |
| 2 — MCP Server | `apps/mcp-server/` 6 read-only tools + tests (English comments) | `pytest apps/mcp-server` |
| 3 — API + RAG | `apps/api/` FastAPI + SQLite + embeddings/ChromaDB + Pydantic schema + `prompts/cloudops_investigator.md` | `pytest apps/api`; valid JSON |
| 4 — n8n workflow | `workflows/cloudops-investigation.json` + import doc | Valid import into n8n |
| 5 — Web | Alert intake, investigation screen, Observability, Safe AI Use (UI strings in pt-BR) | `tsc --noEmit` + demo flow |
| 6 — Compose + demos | `docker-compose.yml` runs everything; 4 demo alerts end-to-end | `docker compose up --build`; demos ok |
| 7 — Evaluation + README | `evaluation/{test-cases.json,run_evaluation.py,report.md}` + English `README.md` | `run_evaluation.py` green; complete README |

## 4. Risks and limits

- No real credentials/services: all data synthetic by design.
- LLM cost/latency: abstract provider; log estimated cost; evaluation may run with mocks.
- n8n in Compose adds local weight; keep the workflow simple and documented.
- Scope locked to MVP: no multi-tenancy, real auth, or cloud deploy in this phase.

## 5. Next action

Start Phase 1 (synthetic data): English Markdown runbooks + JSON fixtures for the 4 demo alerts.
