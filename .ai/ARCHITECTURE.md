# Architecture

## Current repository architecture

No components implemented yet (only `AGENTS.md` + `.ai/` + `docs/` + `.gitignore`; git on `main` with a public remote). The architecture below is **planned** from the approved scope (2026-09-25), not observed.

## Planned MVP architecture (target)

```text
[web: Next.js]
  → POST /investigations (alert) → [api: FastAPI + SQLite]
  → triggers [n8n workflow: webhook → validate → context fetch (RAG knowledge/ + MCP tools) → AI agent → diagnosis JSON → DB write → human review]
  → [mcp-server: 6 read-only tools, synthetic data] ← [knowledge/: .md runbooks] + [sample-data/: alerts, logs, metrics, waf-events]
  → [web: investigation screen + Observability + Safe AI Use]
```

- Planned data flow: alert → correlation_id → RAG (relevant excerpts with traceable origin) → MCP (queries) → versioned prompt `prompts/cloudops_investigator.md` → Pydantic-validated JSON → SQLite → human-in-the-loop (approve/reject/edit/comment/close).
- Target layout: `apps/{api,web,mcp-server}/`, `workflows/cloudops-investigation.json`, `knowledge/{runbooks,service-catalog,architecture}/`, `prompts/`, `sample-data/{alerts,logs,metrics,waf-events}/`, `evaluation/{test-cases.json,run_evaluation.py,report.md}`, `docker-compose.yml`, `.env.example`, `README.md` (in English).
- Real networking/deploy/infra: unknown; `docker-compose.yml` runs all services locally. No real AWS, no credentials.

## Context-layer layout

```text
Tool-specific adapter (optional)
            ↓
        AGENTS.md
            ↓
          .ai/
  project · architecture · conventions · decisions
  tasks · handoff · tools · validation · workflows · prompts
```

`.ai/` is the portable source of truth. Tool-specific adapters must only route agents to it.

## Recommended convention

Document the architecture that exists, not an idealized future design. Mark facts as observed, inferences as inferred, proposed patterns as recommended, and unavailable facts as unknown.
