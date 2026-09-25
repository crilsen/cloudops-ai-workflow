# Requirements

Product requirements describe what to build and why. PRDs are the source of truth for scope; agents treat them as requirements, not suggestions. Keep this file as a short index and store full PRDs under `docs/prd/`.

## Rules

- One PRD per feature or initiative, stored as `docs/prd/NNN-<slug>.md`.
- Status lifecycle: `Draft → Approved → Implemented → Superseded`.
- Only an **Approved** PRD drives implementation; use `.ai/workflows/feature.md`.
- Acceptance criteria must be concrete and testable, and validation maps back to them.
- Do not duplicate requirements here; link to the PRD.
- Record technical choices that a PRD depends on as ADR/TDR entries in `.ai/DECISIONS.md`.
- For large or risky work, a PRD can feed an optional spec-driven flow (`.ai/SPECS.md`, `docs/spec/`) that produces design, plan, and tasks before implementation.

## PRD format

```text
# PRD-NNN — Title
Status: Draft | Approved | Implemented | Superseded
Owner: <who>
Related decisions: ADR-NNN, TDR-NNN

Problem / Context:
...

Goals:
...

Non-goals:
...

Requirements:
- [ ] <requirement>

Acceptance criteria:
- [ ] <testable criterion>

Risks / Open questions:
...
```

## Index

| ID | Title | Status | Owner | File |
| --- | --- | --- | --- | --- |
| PRD-001 | CloudOps AI Workflow MVP | Approved | solicitante | `docs/plano-mvp.md` |

## Current requirements

- Entrada de alerta + 4 alertas sintéticos demo (ver `docs/plano-mvp.md` §1).
- Workflow n8n exportável com retry, logs, correlation_id (idem §2).
- RAG local `knowledge/` + busca semântica com origem rastreável (idem §3).
- MCP somente-leitura, 6 tools, dados sintéticos (idem §4).
- Agente com prompt versionado + JSON validado por Pydantic (idem §5).
- Tela de resultado + revisão humana (idem §6); Observability (idem §7); Uso seguro da IA (idem §8).
- Estrutura de pastas, evaluation com `report.md` e README pt-BR (idem §9–11).
- Restrições: sem "enterprise"; comentários de código em inglês; sem dados/credenciais reais.
