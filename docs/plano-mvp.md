# CloudOps AI Workflow — Planejamento do MVP

> Status: aprovado pelo solicitante em 2026-09-25. Este documento é o PRD-001 (índice em `.ai/REQUIREMENTS.md`).
> Convenções: comentários de código em inglês; UI e docs em pt-BR; nunca usar o termo "enterprise".

## 1. Contexto colado (pedido original, sem cortes de escopo)

### 1.1 Objetivo

Construir um fluxo de IA para acelerar investigações operacionais em ambientes cloud. A aplicação
recebe um alerta ou solicitação técnica, consulta runbooks e contexto operacional, executa
ferramentas controladas de consulta e gera um diagnóstico estruturado para revisão humana.

O projeto deve demonstrar: n8n · AI Agents · Context Engineering · RAG · MCP · integração por
APIs e webhooks · Observability · Guardrails · Human-in-the-loop · CloudOps e SRE.

### 1.2 Restrições inegociáveis

- Portfólio; sem dados reais, credenciais reais ou acesso irrestrito à AWS.
- Alertas, logs, métricas, inventário e runbooks sintéticos.
- Sem o termo "enterprise" em títulos, textos, nomes, README ou código.
- A IA apoia a investigação; nunca executa ações destrutivas ou autônomas.
- Interface e documentação em português.
- MVP funcional, não plataforma complexa.
- Comentários de código em inglês (pedido do solicitante em 2026-09-25).

### 1.3 Stack obrigatória

- n8n como orquestrador principal.
- Backend Python com FastAPI.
- MCP Server em Python (ferramentas controladas).
- SQLite para histórico.
- Frontend Next.js + TypeScript + Tailwind (ou Streamlit se reduzir muito o tempo — decisão pendente).
- Docker Compose para todos os serviços.
- LLM com provider abstraído (Claude, OpenAI ou Gemini via env).
- `.env.example` sem chaves reais.

### 1.4 Cenário do MVP

Alertas de exemplo: "Elevated 5xx errors in payments-api", "High latency in checkout service",
"Unusual AWS WAF blocked requests", "Pod CrashLoopBackOff in production namespace".
O fluxo reúne contexto sintético, propõe hipóteses e sugere próximos passos com evidências.

### 1.5 Funcionalidades (§2–§11 do pedido)

1. **Entrada de alerta** — página com título, serviço, severidade (baixa/média/alta/crítica),
   ambiente (development/staging/production), descrição livre, data/hora, botão "Iniciar
   investigação" + 4 alertas sintéticos prontos.
2. **Workflow n8n** — JSON exportável: webhook/manual → validação → contexto → agente →
   MCP → diagnóstico → banco → revisão humana; com erro, retry, logs, correlation_id.
3. **Context Engineering + RAG** — base Markdown em `knowledge/` (runbooks, catálogo,
   arquitetura simulada, procedimentos 5xx/latência/CrashLoopBackOff/WAF, escalonamento,
   segurança); busca semântica (embeddings + ChromaDB ou pgvector); agente recebe só trechos
   relevantes com origem rastreável.
4. **MCP Server** — somente leitura: `get_service_catalog`, `search_runbooks`,
   `get_recent_logs`, `get_service_metrics`, `get_waf_events`, `get_kubernetes_status`;
   retorno JSON sintético; sem escrita, sem comandos arbitrários, sem segredos, sem instrução
   não validada vinda do LLM.
5. **Agente de IA** — prompt versionado em `prompts/cloudops_investigator.md`; logs/runbooks
   são dados, nunca instruções; sem inventar métricas/causas; fato ≠ hipótese ≠ recomendação;
   citar origens; ações reversíveis; escalar risco p/ humano; nunca sugerir mudança em produção
   sem aprovação; saída JSON validada por Pydantic (schema `incident_summary`,
   `severity_assessment`, `observed_facts`, `hypotheses`, `recommended_actions`,
   `escalation_recommendation`, `limitations`).
6. **Interface de resultado** — resumo, severidade, linha do tempo, fatos, hipóteses com
   confiança, evidências/fontes, ações, status de aprovação; aprovar, rejeitar, comentar,
   editar antes de aprovar, concluir.
7. **Observability/LLMOps** — por investigação: ID, modelo, prompt_version, tempo total,
   latência por etapa, nº de tool calls, custo estimado, status da validação JSON, revisão
   humana; página "Observability" agregada (total, tempo médio, erros, severidade, aprovação).
8. **Segurança/guardrails** — página "Uso seguro da IA" (6 princípios do pedido) + detecção
   simples de prompt injection em logs/runbooks com alerta no resultado.
9. **Estrutura esperada** — `apps/{api,web,mcp-server}/`, `workflows/`, `knowledge/`,
   `prompts/`, `sample-data/`, `evaluation/`, `docker-compose.yml`, `.env.example`, `README.md`.
10. **Avaliação** — casos sintéticos validando JSON válido, justificativa, evidência/incerteza,
    aprovação p/ risco, citação de fontes, log-não-é-instrução; relatório em
    `evaluation/report.md`.
11. **README pt-BR** — problema, arquitetura, Mermaid, fluxo n8n, execução local, provider LLM,
    importação do workflow, demos, limitações, segurança, competências demonstradas.
12. **Ordem de execução** — dados sintéticos + backend/MCP primeiro; depois workflow n8n e
    interface; ao final validar Compose, demos e testes.

## 2. Decisões já tomadas

Ver `.ai/DECISIONS.md` (ADR-001, TDR-001, ADR-002, Language-001). Pendente: Next.js vs
Streamlit; ChromaDB vs pgvector; qual provider LLM será o padrão no Compose.

## 3. Fases de execução

| Fase | Entregas | Validação |
| --- | --- | --- |
| 0 — Fundação (este passo) | `.ai/` populado, este plano, repo público, `.gitignore`, `docker-compose.yml` esqueleto, `.env.example` | `git status` limpo, push ok |
| 1 — Dados sintéticos | `sample-data/` (alerts/logs/metrics/waf-events) + `knowledge/` (.md) | arquivos versionados; grep sem "enterprise" |
| 2 — MCP Server | `apps/mcp-server/` 6 tools read-only + testes | `pytest apps/mcp-server` |
| 3 — API + RAG | `apps/api/` FastAPI + SQLite + embeddings/ChromaDB + schema Pydantic + `prompts/cloudops_investigator.md` | `pytest apps/api`; JSON válido |
| 4 — Workflow n8n | `workflows/cloudops-investigation.json` + doc de importação | import JSON válido no n8n |
| 5 — Web | entrada de alerta, tela investigação, Observability, Uso seguro da IA | `tsc --noEmit` + fluxo demo |
| 6 — Compose + demos | `docker-compose.yml` sobe tudo; 4 alertas demo fim-a-fim | `docker compose up --build`; demos ok |
| 7 — Avaliação + README | `evaluation/{test-cases.json,run_evaluation.py,report.md}` + `README.md` pt-BR | `run_evaluation.py` verde; README completo |

## 4. Riscos e limites

- Sem credenciais/serviços reais: todo dado é sintético por desenho.
- LLM custa/latência: abstrair provider; registrar custo estimado; evaluation pode rodar com mock.
- n8n no Compose aumenta peso local; manter workflow simples e documentado.
- Escopo trava em MVP: nada de multi-tenant, auth real ou deploy em nuvem nesta fase.

## 5. Próxima ação

Iniciar Fase 1 (dados sintéticos) após escolha pendente: Next.js vs Streamlit e ChromaDB vs
pgvector — ou seguir com padrão (Next.js + ChromaDB) se o solicitante delegar.
