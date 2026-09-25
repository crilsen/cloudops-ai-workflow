# Decision Records

Durable, meaningful choices. Two types:

- **ADR** — architecture decision: how the system is structured.
- **TDR** — technology decision: stack, library, provider, protocol, or tooling.

Status lifecycle: `Proposed → Accepted → Superseded | Deprecated`. ADR and TDR use separate ID sequences.

## Modes

Choose one mode per project and record the choice at adoption. The index below is used in both modes.

- **Simple (default):** entries live inline in this file. Best for small projects and up to roughly 15–20 active records.
- **Scale:** one file per record under `docs/decisions/`, named `ADR-NNN-<slug>.md` or `TDR-NNN-<slug>.md`. This file stays as the index only.

Migrate from simple to scale when the inline log gets unwieldy. Keep IDs stable during migration.

## Entry format

```text
## ADR-NNN / TDR-NNN — Title
Type: ADR | TDR
Status: Proposed | Accepted | Superseded | Deprecated
Date: YYYY-MM-DD
Owners: <who>
Supersedes: <ID or none>

Context:
...

Decision:
...

Reasoning:
...

Consequences:
...
```

## Index

| ID | Type | Title | Status | Date | File |
| --- | --- | --- | --- | --- | --- |
| ADR-001 | ADR | n8n as primary orchestrator | Accepted | 2026-09-25 | inline |
| TDR-001 | TDR | MVP stack | Accepted | 2026-09-25 | inline |
| ADR-002 | ADR | Read-only + human decides | Accepted | 2026-09-25 | inline |
| Language-001 | TDR | Code comments in English | Accepted | 2026-09-25 | inline |
| Language-002 | TDR | All Markdown in English | Accepted | 2026-09-25 | inline |

## Records

### ADR-001 — n8n as primary orchestrator
Type: ADR
Status: Accepted
Date: 2026-09-25
Owners: requester

Context: MVP needs a visible, exportable workflow with retry/logs for portfolio purposes.
Decision: n8n orchestrates (webhook → validate → context → agent → MCP → diagnosis → database → human review); workflow versioned in `workflows/cloudops-investigation.json`.
Consequences: requires n8n in Docker Compose + import doc.

### TDR-001 — MVP stack
Type: TDR
Status: Accepted
Date: 2026-09-25
Owners: requester

Context: Fast functional MVP, no real data.
Decision: FastAPI + Python MCP + SQLite + Docker Compose; Next.js+TS+Tailwind frontend; RAG with ChromaDB; LLM via env (Claude/OpenAI/Gemini).
Consequences: stack confirmed on 2026-09-25 (no longer pending).

### ADR-002 — Read-only + human decides
Type: ADR
Status: Accepted
Date: 2026-09-25
Owners: requester

Context: AI assists investigation, never performs destructive/autonomous actions.
Decision: read-only MCP tools with synthetic data; risky actions require human approval; logs/runbooks treated as untrusted data (prompt-injection detection).
Consequences: evaluation tests cover these guarantees; no credentials in the repo.

### Language-001 — Code comments in English
Type: TDR
Status: Accepted
Date: 2026-09-25
Owners: requester (direct request)

Context: Explicit request ("comentarios todos em ingles").
Decision: All code comments in English, in every source file and language.
Consequences: validation includes a comment-language check.

### Language-002 — All Markdown in English
Type: TDR
Status: Accepted
Date: 2026-09-25
Owners: requester (direct request)

Context: Explicit request ("faca todos os mds em ingles"); supersedes the earlier Portuguese-docs rule for Markdown files.
Decision: Every `.md` file in the repo is written in English. Code comments stay in English (Language-001). UI strings in frontend code (not Markdown) remain Portuguese (pt-BR) per the original spec.
Consequences: `README.md` and `evaluation/report.md` will be written in English.

Do not backfill invented history. Record decisions that are observed, expressly documented, or approved during future work.

The design rationale for this template itself lives in [`docs/design.md`](../docs/design.md).
