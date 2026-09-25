# Project

## Identity

- **Name:** CloudOps AI Workflow
- **Objective:** Fluxo de IA para acelerar investigações operacionais em cloud: recebe um alerta técnico, reúne contexto de runbooks/fontes sintéticas, executa ferramentas controladas de consulta e gera diagnóstico estruturado para revisão humana.
- **Repository purpose:** Portfólio. MVP funcional, não plataforma complexa. Interface e documentação em português.
- **Status:** Bootstrap/adotado em 2026-09-25; nenhuma implementação ainda (só `AGENTS.md` + `.ai/`).

## Observed

- Diretório contém apenas `AGENTS.md`, `.ai/` e `.DS_Store`; sem código, sem `docker-compose.yml`, sem `README.md`, sem git (confirmado via `git status`: "not a git repository").
- Escopo definido pelo solicitante em 2026-09-25: n8n + FastAPI + MCP Server Python + SQLite + frontend Next.js/TS/Tailwind (ou Streamlit) + Docker Compose + LLM com provider abstraído + ChromaDB/pgvector + n8n workflow JSON + evaluation.
- Restrições explícitas: sem dados/credenciais reais, sem acesso irrestrito à AWS, tudo sintético; sem termo "enterprise"; IA só apoia, nunca executa ação destrutiva/autônoma.

## Stack alvo (planejado, não observado)

- Orquestrador: n8n (workflow exportável `workflows/cloudops-investigation.json`).
- Backend: Python FastAPI; MCP Server Python somente-leitura; SQLite histórico.
- Frontend: Next.js + TypeScript + Tailwind (fallback Streamlit se tempo exigir — decisão pendente).
- RAG: base Markdown em `knowledge/` + busca semântica (ChromaDB ou pgvector).
- LLM: provider abstraído via env (Anthropic Claude / OpenAI / Gemini); `.env.example` sem chaves reais.
