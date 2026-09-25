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
| ADR-001 | ADR | n8n como orquestrador principal | Accepted | 2026-09-25 | inline |
| TDR-001 | TDR | Stack do MVP | Accepted | 2026-09-25 | inline |
| ADR-002 | ADR | Somente leitura + humano decide | Accepted | 2026-09-25 | inline |
| Language-001 | TDR | Comentários em inglês | Accepted | 2026-09-25 | inline |

## Records

### ADR-001 — n8n como orquestrador principal
Type: ADR
Status: Accepted
Date: 2026-09-25
Owners: solicitante

Context: MVP precisa de workflow visível, exportável e com retry/logs para portfólio.
Decision: n8n orquestra (webhook → valida → contexto → agente → MCP → diagnóstico → banco → revisão humana); workflow versionado em `workflows/cloudops-investigation.json`.
Consequences: requer n8n no Docker Compose + doc de importação.

### TDR-001 — Stack do MVP
Type: TDR
Status: Accepted
Date: 2026-09-25
Owners: solicitante

Context: MVP funcional rápido, sem dados reais.
Decision: FastAPI + MCP Python + SQLite + Docker Compose; frontend Next.js+TS+Tailwind (Streamlit só como fallback); RAG com ChromaDB ou pgvector; LLM via env (Claude/OpenAI/Gemini).
Consequences: frontend ainda pendente de escolha final.

### ADR-002 — Somente leitura + humano decide
Type: ADR
Status: Accepted
Date: 2026-09-25
Owners: solicitante

Context: IA apoia investigação, nunca executa ação destrutiva/autônoma.
Decision: MCP tools somente-leitura com dados sintéticos; ações de risco exigem aprovação humana; logs/runbooks tratados como dados não confiáveis (detecção de prompt injection).
Consequences: testes de avaliação cobrem essas garantias; sem credenciais no repo.

### Language-001 — Comentários em inglês
Type: TDR
Status: Accepted
Date: 2026-09-25
Owners: solicitante (pedido direto)

Context: Pedido explícito "comentarios todos em ingles".
Decision: Todos os comentários de código em inglês; UI e docs voltadas ao usuário em pt-BR.
Consequences: validação inclui checagem de idioma dos comentários.

Do not backfill invented history. Record decisions that are observed, expressly documented, or approved during future work.

The design rationale for this template itself lives in [`docs/design.md`](../docs/design.md).
