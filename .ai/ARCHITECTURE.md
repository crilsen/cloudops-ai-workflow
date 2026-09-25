# Architecture

## Current repository architecture

Nenhum componente implementado ainda (só `AGENTS.md` + `.ai/`). Arquitetura abaixo é **planejada** a partir do escopo aprovado em 2026-09-25, não observada.

## Planned MVP architecture (target)

```text
[web: Next.js ou Streamlit]
  → POST /investigations (alerta) → [api: FastAPI + SQLite]
  → dispara [n8n workflow: webhook → valida → busca contexto (RAG knowledge/ + MCP tools) → agente IA → diagnóstico JSON → grava banco → revisão humana]
  → [mcp-server: 6 tools somente-leitura, dados sintéticos] ← [knowledge/: runbooks .md] + [sample-data/: alerts, logs, metrics, waf-events]
  → [web: tela investigação + Observability + Uso seguro da IA]
```

- Data flow planejado: alerta → correlation_id → RAG (trechos relevantes com origem) → MCP (consultas) → prompt versionado `prompts/cloudops_investigator.md` → JSON validado por Pydantic → SQLite → human-in-the-loop (aprovar/rejeitar/editar/comentar/concluir).
- Estrutura alvo: `apps/{api,web,mcp-server}/`, `workflows/cloudops-investigation.json`, `knowledge/{runbooks,service-catalog,architecture}/`, `prompts/`, `sample-data/{alerts,logs,metrics,waf-events}/`, `evaluation/{test-cases.json,run_evaluation.py,report.md}`, `docker-compose.yml`, `.env.example`, `README.md` (pt-BR).
- Networking/deploy/infra reais: desconhecido; `docker-compose.yml` sobe todos os serviços localmente. Sem AWS real, sem credenciais.

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
