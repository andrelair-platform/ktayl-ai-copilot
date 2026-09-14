# AGENTS.md — ktayl-ai-copilot

Tiny, evidence-based context for AI agents working in this repo. The org-level rules
(`minicloud-gitops/.claude/rules/*`) still apply; this only adds what's repo-specific.

## Policy
- **This is a ktayl-solution IS product** (the insurer's internal tool), IS Foundations initiative.
- **NEVER involve Retrieva.** Retrieva is a separate product (RNCP cert / DORA third-party-risk) that
  only *runs on* ktayl infra. This copilot must not call it, read its data, or depend on it.
- **The differentiation rule:** every capability must be one **Open WebUI cannot do** (structured
  decisions / authorized actions / multi-agent workflows). If OWUI can already do it, it belongs in the
  sister product **ktayl Knowledge Assistant** (board #18), not here. Chat-over-docs is out of scope.
- Deploy via the GAP wrapper-chart + Kargo + CODEOWNERS prod gate (see gitops rules). Never commit CLAUDE.md.

## What it is
An insurance **decision & action** copilot: makes validated, authorized, audited underwriting (v1)
then claims (v2) decisions, and acts through ktayl's own systems (policy-service live; claims/UW/CRM
as they deploy). Built on the shared AI Platform (LiteLLM, Qdrant/RAG-ingest, Langfuse/Ragas,
Presidio, agent frameworks). Surface: OWUI for read-only knowledge Q&A (via #18); standalone
Authentik-OIDC UI + API for decisions/actions.

## Conventions / pitfalls
- Planning artefacts (BMAD): `docs/planning/` (committed as evidence). `_bmad/` + `_bmad-output/` are
  gitignored scratch for the `/bmad-*` skills.
- Per-user authz is application-layer (identity ≠ user claim); actions require confirmation + audit.
- LLMOps: own Langfuse project, versioned prompts, Ragas eval, per-feature experiment card.

## Status
BMAD Path C. Product Brief drafted (`docs/planning/brief.md`) — awaiting sign-off. Board **#19**.
