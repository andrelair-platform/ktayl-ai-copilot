# Product Brief — ktayl AI Ops Copilot

> **BMAD artefact:** Product Brief (Path C, artefact #1). Status: **DRAFT — awaiting owner sign-off.**
> Next in chain: PRD → UX → Architecture (← governance gate) → SPEC/epics → readiness gate.
> Owner (PM/PO): AndreLiar. Board: **#19 ktayl AI Ops Copilot** (IS Foundations). Repo: `ktayl-ai-copilot`.

## 1. What this is (one paragraph)

An **internal AI operations copilot for the ktayl insurance organisation** that **makes and
executes validated, authorized, audited insurance business decisions** — starting with
**underwriting**. It is **not** a chat-over-documents tool: *that* need (ask a policy/guideline
question, get a cited answer) is already met by **Open WebUI** and is delivered by the sister
project **ktayl Knowledge Assistant** (board #18, config-only). This copilot earns its existence
**only** where Open WebUI structurally cannot go — structured decisions, authorized actions into
ktayl's transactional systems, and multi-agent insurance workflows.

## 2. The two-layer positioning (do not conflate)

- This is a **ktayl-solution IS** product (the insurer's own internal tool) — **IS Foundations**
  initiative, built on the shared **AI Platform**.
- It **excludes Retrieva entirely.** Retrieva is a *separate product* (RNCP cert deliverable, DORA
  third-party-risk) that merely *runs on* ktayl's infrastructure; it is **not** a ktayl org system.
  This copilot does not call, share data with, or depend on Retrieva. Different layer, different
  domain (insurance business ops vs third-party-risk compliance).

## 3. The problem

ktayl's underwriters (and later claims handlers) make judgment-heavy decisions that require pulling
together **appetite, underwriting guidelines, policy wordings, prior submissions, exposure/accumulation
data, and regulatory constraints**, then producing a **defensible, structured decision** (accept /
decline / refer, terms, pricing indication, referral reasons) — and acting on it in ktayl's systems.
Today that is manual, inconsistent, slow, and leaves a weak audit trail. A chatbot over documents does
not solve it (it answers questions; it does not *decide* or *act* or *prove why*).

## 4. Why Open WebUI is NOT the answer (the differentiation test — mandatory)

Every capability below is one Open WebUI **structurally cannot** provide. If a proposed feature *is*
coverable by Open WebUI, it belongs in **ktayl Knowledge Assistant (#18)**, not here.

| Capability (this project) | Why Open WebUI can't |
|---|---|
| **Structured, schema-validated decisions as an API** (risk_assessment, coverage_determination) | OWUI returns prose, not validated JSON another system consumes |
| **Authorized ACTIONS into ktayl systems** (bind, create claim, endorsement) with per-user authz + confirmation + audit | OWUI has no enterprise authz/trust boundary; identity ≠ user claim |
| **Multi-agent insurance workflows** (research → risk → pricing/coverage → recommendation) with planning + state | OWUI is single-turn chat |
| **Cross-system reasoning** (join policy + submission + exposure + billing + regulatory) | OWUI RAG is document retrieval only |
| **Decision discipline** (rules over LLM guessing, wording-version conflict resolution, mandatory grounding/refusal/escalation) | not expressible in OWUI |
| **Insurance-domain evaluation + AI-decision auditability** ("who decided, on what evidence" — ACPR) | OWUI has no eval framework or decision audit |

## 5. Anchor value chain (v1) + roadmap

- **v1 = Underwriting & Pricing** — knowledge/judgment-heavy → strongest "AI adds judgment" business
  case; the multi-agent **underwriting risk assessment** is the sharpest beyond-OWUI showcase;
  `ktayl-policy-service` is live for early tool integration.
- **v2 = Claims** (FNOL triage → coverage determination → fraud/SIU signals → settlement recommendation
  → create claim). High-volume, automation-heavy.
- Later: distribution/CRM, finance/billing, compliance actions — as those ktayl systems deploy.

## 6. Target users

Underwriters, underwriting assistants, referral/senior underwriters (v1); claims handlers/adjusters
(v2); risk/compliance for the audit trail. **Not** customer-facing.

## 7. Goals & success metrics (to be made measurable in the PRD)

- **Decision quality:** structured recommendations match senior-UW judgment on a gold test set (target
  set in PRD); referral flags catch the cases that should be referred.
- **Cycle time:** submission → structured recommendation materially faster than manual.
- **Auditability:** 100% of copilot decisions carry the evidence + rules + prompt version used.
- **Safety:** zero unauthorized actions; passes the L9 red-team suite.
- **Beyond-OWUI:** every shipped capability fails the "could OWUI do this?" test (i.e., OWUI can't).

## 8. Scope

**In (v1):** submission intake + structured extraction; retrieval over appetite/guidelines/wordings
(grounded, cited, conflict-aware); multi-agent underwriting risk assessment; structured risk + pricing
+ referral recommendation (validated schema); **read + limited authorized write** into
`ktayl-policy-service`; per-user authz; decision audit log; evaluation harness; the standalone
Authentik-authed copilot UI + API (introduced when actions arrive).

**Out:** general document Q&A (→ #18 Knowledge Assistant / OWUI); anything touching **Retrieva**;
customer-facing surfaces; full claims (→ v2); autonomous binding without human confirmation.

## 9. High-level approach

Dedicated **orchestrator backend** on the shared AI Platform (LiteLLM gateway, Qdrant/RAG-ingest,
Langfuse/Ragas, agent frameworks, Presidio PII, MLflow). Surface phasing: **Open WebUI covers
read-only knowledge Q&A (via #18, config)**; the copilot backend starts at **structured decisions +
authorized actions**, fronted by a **standalone Authentik-OIDC UI + API** the moment actions/authz
appear (L7). The 10-level prompt-engineering framework = the **epic roadmap**; the per-feature
**experiment card** = the LLMOps discipline (ties into the platform LLMOps standard: own Langfuse
project, versioned prompts, Ragas eval).

## 10. Constraints

Resource-constrained 5-node cluster + tight per-namespace quotas + €10/mo/provider cloud cap; EU
data residency + PII governance (Presidio, governed egress netpol); GAP wrapper-chart deployment +
Kargo promotion + CODEOWNERS prod gate; ESM/existing platform conventions; regulated-insurer
auditability (ACPR) from day one.

## 11. Key risks

- **Scope creep back into OWUI territory** → mitigated by the §4 differentiation test on every feature.
- **Decision correctness / hallucinated coverage** → grounding discipline + rules + eval harness + human
  confirmation on actions.
- **Transactional systems not yet deployed** (claims/UW workbench) → v1 uses docs corpus + live
  policy-service; deploy real system tools per milestone.
- **Authz boundary** → application-layer authz, real user identity (not OWUI pass-through), red-team.

## 12. Delivery path & gates

**Path C.** Governance gates: **architecture + security review** (before build) and the **readiness
gate** (before the sprint). This brief must be **signed off** before the PRD. Cert mapping: BC02
(concevoir/développer), BC03 (déployer/sécuriser) — but note this is a **ktayl-IS** product; the
cert deliverable remains Retrieva.
