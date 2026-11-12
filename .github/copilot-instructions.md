# Copilot instructions for Azure Language OpenAI Conversational Agent Accelerator

## Project overview
This repository is a sample Azure AI accelerator for a deterministic conversational agent experience that combines Azure AI Language, Azure OpenAI, and Azure AI Search. The project is designed to route user requests through CLU, CQA, orchestration, or a fallback RAG flow rather than relying on freeform LLM behavior alone.

Key concepts in this codebase:
- `src/frontend/` contains the React + Vite client UI.
- `src/backend/src/` contains the Python FastAPI services and orchestration logic.
- `src/backend/src/router/` contains routing strategies such as `CLU`, `CQA`, `ORCHESTRATION`, `FUNCTION_CALLING`, and `TRIAGE_AGENT`.
- `src/backend/src/agents/` contains custom business agents for order status, refund, and cancellation.
- `infra/` contains deployment scripts and sample product knowledge data used for the demo scenario.
- `docs/` contains deployment and environment setup guidance.

## Architecture and coding expectations
- Keep the project behavior deterministic and explainable: prefer explicit route selection, confidence checks, and structured responses.
- Preserve the existing routing model. Changes to route types, confidence thresholds, or fallback behavior should be intentional and aligned with the README and `src/backend/src/README.md`.
- Use environment variables for Azure endpoints and deployment names; do not hardcode secrets or resource IDs.
- Maintain compatibility with both orchestration patterns used here:
  - `semantic_kernel_app.py` / `semantic_kernel_orchestrator.py`
  - `unified_app.py` / `unified_conversation_orchestrator.py`
- When adding or changing app behavior, keep the frontend and backend contract in sync.

## Repository layout
- Root-level docs and deployment setup are in `README.md`, `docs/`, and `azure.yaml`.
- Backend environment variables are documented in `src/backend/src/README.md`.
- Frontend package scripts live in `src/frontend/package.json`.
- Python dependencies live in `src/backend/requirements.txt`.
- Tests live under `src/backend/src/test/` and exercise the chat endpoints.

## Validation workflow
Use the smallest relevant validation command for the change.

### Frontend
From `src/frontend`:
- `npm install`
- `npm run build`

### Backend
From `src/backend`:
- `pip install -r requirements.txt`
- `cd src`
- `python -m pytest test/test_unified_chat.py -q` when Azure-backed services are configured and the app environment is ready

If a change affects only Python logic or routing, prefer a focused backend check over a full repo-wide test suite.

## Implementation guidance
- Prefer minimal, targeted edits over broad refactors.
- Keep new code consistent with the naming and structure already used in the router and agent modules.
- Preserve the distinction between:
  - routing logic
  - fallback logic
  - Azure service integration
  - UI concerns
- For Azure AI Language features, honor confidence thresholds and fail gracefully when no intent or answer is found.
- For business actions (order status, refund, cancellation), keep the response format consistent with existing patterns and test expectations.
- If a feature affects deployed behavior, update the relevant docs in `README.md`, `src/backend/src/README.md`, or `docs/` where appropriate.

## Security and safety
- Do not commit secrets, keys, or environment files containing credentials.
- Treat customer or user data as sensitive; do not log full prompt payloads or confidential data beyond what is necessary for debugging.
- Respect the sample data and fictionalized demo scenario; avoid introducing company-specific or production assumptions into the sample code.

## When making changes
- Check whether the task affects frontend, backend, routing, or infra; validate only the impacted area.
- Keep changes aligned with the repo’s demo scenario: Contoso Outdoors product support, order management, and Azure AI Language orchestration.
- Avoid unrelated cleanup or refactors in the same patch.
- If you add configuration, document the required environment variables and defaults clearly.

## Source of truth
When uncertain, use the project’s README and backend README as the source of truth for architecture, routing strategies, and deployment behavior before making changes.
