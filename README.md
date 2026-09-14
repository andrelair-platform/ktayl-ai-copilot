# ktayl-ai-copilot

> Insurance operations copilot for the **ktayl-solution IS** — an AI system that **makes and executes
> validated, authorized, audited insurance business decisions** (underwriting first, then claims).
> It is **not** a chat-over-documents tool: that need is served by **Open WebUI** via the sister
> project *ktayl Knowledge Assistant* (board #18). This copilot exists only for what Open WebUI
> **structurally cannot do** — structured decisions, authorized actions into ktayl systems, and
> multi-agent insurance workflows.

**Layer:** ktayl-solution IS · IS Foundations · board **#19**. **Excludes Retrieva** (a separate product).

## Status

BMAD **Path C**. Currently at the first artefact — see [`docs/planning/brief.md`](docs/planning/brief.md)
(Product Brief, awaiting owner sign-off). Chain: Brief → PRD → UX → Architecture (governance gate) →
SPEC/epics → readiness gate → build.

## The differentiation test (mandatory)

Every capability must be one **Open WebUI cannot provide**. If OWUI already does it (doc Q&A, cited
RAG), it belongs in *ktayl Knowledge Assistant* (#18), not here. See the brief §4.

## Built on

The shared AI Platform: LiteLLM gateway · Qdrant + RAG-ingest · Langfuse + Ragas · agent frameworks ·
Presidio (PII) · MLflow. Deployed via the GAP wrapper-chart + Kargo (see `minicloud-gitops`).
