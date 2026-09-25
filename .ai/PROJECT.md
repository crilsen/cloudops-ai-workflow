# Project

## Identity

- **Name:** CloudOps AI Workflow
- **Objective:** An AI flow that speeds up operational investigations in cloud environments: it receives a technical alert, gathers context from runbooks and synthetic sources, runs controlled query tools, and produces a structured diagnosis for human review.
- **Repository purpose:** Portfolio. Functional MVP, not a complex platform. UI strings in Portuguese; all Markdown docs in English.
- **Status:** Bootstrapped/adopted on 2026-09-25; no implementation yet (only `AGENTS.md` + `.ai/` + docs + `.gitignore`; git on `main` with a public remote).

## Observed

- The directory holds a git repo (`main`, remote `https://github.com/crilsen/cloudops-ai-workflow`, public); no app code, no `docker-compose.yml`, no `README.md` yet.
- Scope defined by the requester on 2026-09-25: n8n + FastAPI + Python MCP Server + SQLite + Next.js/TS/Tailwind frontend + Docker Compose + abstracted-provider LLM + ChromaDB + n8n workflow JSON + evaluation.
- Explicit restrictions: no real data/credentials, no unrestricted AWS access, everything synthetic; no term "enterprise"; AI only assists, never performs destructive/autonomous actions.

## Target stack (planned, not observed)

- Orchestrator: n8n (exportable workflow `workflows/cloudops-investigation.json`).
- Backend: Python FastAPI; read-only Python MCP Server; SQLite history.
- Frontend: Next.js + TypeScript + Tailwind (confirmed 2026-09-25).
- RAG: Markdown base in `knowledge/` + semantic search (ChromaDB, confirmed 2026-09-25).
- LLM: env-abstracted provider (Anthropic Claude / OpenAI / Gemini); `.env.example` with no real keys.
