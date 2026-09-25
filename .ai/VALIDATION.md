# Validation

## Completion rule

Before completion, run all applicable project validations that are available and safe. Report each as **Validated**, **Partially validated**, or **Not validated**, with the reason for anything not run. Never claim validation that did not occur.

## Recommended checks when the technology exists

### Terraform / OpenTofu

1. Run formatting (`terraform fmt -recursive` or `tofu fmt`).
2. Run `validate`.
3. Run `tflint` when configured.
4. Run `plan` only when credentials/backend are available and the task permits it.

### Kubernetes / Helm

1. Validate YAML.
2. Run `helm lint` and `helm template` when applicable.
3. Use client-side `kubectl` dry-run or configured policy tools when safe.

### Scripts and applications

1. Run applicable formatters, syntax checks, linters, and tests.
2. Run `shellcheck` for shell scripts when available.
3. Report untested runtime assumptions.

No project-specific validation commands have been identified yet.

## CloudOps AI Workflow MVP checks (planned)

1. `docker compose config` validates the compose file; `docker compose up -d --build` + demo flow validates runtime.
2. Backend: `pytest` (contracts: diagnosis JSON schema via Pydantic, guardrail/injection-detection tests, MCP tool read-only tests).
3. Frontend: `tsc --noEmit` + `next lint` (or Streamlit smoke run if fallback chosen).
4. Workflow: `python -c "import json; json.load(open('workflows/cloudops-investigation.json'))"` validates n8n export.
5. Evaluation: `python evaluation/run_evaluation.py` regenerates `evaluation/report.md` (schema validity, justification, evidence, human-approval, source citation, no log-as-instruction).
6. Language check: code comments in English; UI/docs in pt-BR; no forbidden term "enterprise" (grep).
