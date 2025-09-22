# Project Workflow — Single Source of Truth

## Purpose and Scope
- This is the **top governance rule**. All development activities must conform.
- **Docs-first mandate:** **Generate or update required docs _before_ generating code.** Code must **follow** the documented contracts, topology, framework roles, and SLOs.
- **Stage gating & human review:** At **each stage**, the assistant **generates the required file(s)** and **stops for human review/approval** before moving to the next stage.
- **Cursor Rule Compliance:** Each stage must follow its corresponding cursor rule format and standards.


## Authoritative Paths & Naming
- **PRD** → `docs/prds/prd-YYYYMMDD.md` (new dated file for significant business changes; do **not** edit old; maintain `docs/prds/index.md`)
- **SPEC (TECH SPEC)** → `docs/specs/spec-YYYYMMDD-<spec>.md` (no subdirs; `<spec>` in `{system, api, frontend, llm, ...}`; maintain `docs/specs/index.md`)
  - **Architecture section contains only:** (1) **comprehensive diagram** (frameworks + relationships) and (2) **component inventory table**.
  - Minor edits → update + **Changelog**; material contract/SLO/framework/topology change → **new dated snapshot** (prior marked **Superseded**).
- **ADR** → `docs/adrs/adr-YYYYMMDD-<slug>.md` (non-trivial decisions; lifecycle: Draft → Accepted | Rejected | Superseded)
- **FEATURE** → `docs/features/ft-<ID>-<slug>.md` (+ keep `docs/features/schedule.md`)
- **OP-NOTE (Runbook)** → `docs/op-notes/op-<ID>-<slug>.md` **or** `docs/op-notes/op-release-<semver>.md` (+ `docs/op-notes/index.md`)
- **Trace IDs:** use ticket/feature ID like `ft-123` in files/branches/PRs/commits.


## End-to-End Pipeline

### Stage A — **Initiate**
- **Input:** Problem or idea.
- **Action:** If scope affects product goals/metrics → create/point to **PRD** (`prd-YYYYMMDD.md`). Minor bug/chores can skip to Stage D with a short note in PR.
- **Output (generate file):** `docs/prds/prd-YYYYMMDD.md` (or link existing).
- **Cursor Rule Compliance:** Generated PRD must follow 01-prd.md format and standards.
- **Exit Gate:** PRD exists **or** justified exemption noted in PR.
- **REVIEW STOP:** **Agent must stop here and wait for human review/approval before proceeding to Stage B.**

### Stage B — **Specify (TECH SPECS)**
- **Action:** Create/update relevant **SPECs** (system/api/frontend/llm). Use `spec-YYYYMMDD-<spec>.md`.
- **Architecture (required):** add **diagram (frameworks+relationships)** and **component inventory**; link latest PRD.
- **Output (generate file):** relevant SPEC(s) `spec-YYYYMMDD-<spec>.md` with:
  - **Architecture:** comprehensive diagram (frameworks + relationships) **and** component inventory table.
  - Links to latest PRD; Changelog entry.
- **Cursor Rule Compliance:** Generated SPECs must follow 02-tech_spec.md format and standards.
- **Exit Gate:** For any change to **contracts/topology/framework roles/SLOs**, a **new dated SPEC** is present and listed as **Current** in `docs/specs/index.md`.
- **REVIEW STOP:** **Agent must stop here and wait for human review/approval before proceeding to Stage C.**

### Stage C — **Decide (ADRs)**
- **Action:** For any non-trivial choice (new dependency, auth model, storage pattern, schema versioning, SLO shifts), add **ADR**.
- **Output (generate file):** `docs/adrs/adr-YYYYMMDD-<slug>.md` for non-trivial choices (deps, storage, auth, versioning, SLO moves).
- **Cursor Rule Compliance:** Generated ADRs must follow 03-adr.md format and standards.
- **Exit Gate:** PR references ADR(s); code relying on the decision cannot merge without an **Accepted** ADR.
- **REVIEW STOP:** **Agent must stop here and wait for human review/approval before proceeding to Stage D.**

### Stage D — **Plan (FEATURE)**
- **Action:** Create **FEATURE** spec `ft-<ID>-<slug>.md`; update `docs/features/schedule.md`.
- **Must include:** Acceptance criteria, design diffs (UI/API/schema), test & eval plan (goldens + thresholds for LLM paths), telemetry, rollout/rollback, risks.
- **Output (generate file):** `docs/features/ft-<ID>-<slug>.md` (+ update `docs/features/schedule.md`) with:
  - Acceptance criteria; design diffs (UI/API/schema); test & eval plan; telemetry; rollout/rollback; risks.
- **Cursor Rule Compliance:** Generated FEATURE must follow 04-feature.md format and standards.
- **Exit Gate (DoR):** FEATURE file present with all required sections + links to SPEC.
- **REVIEW STOP:** **Agent must stop here and wait for human review/approval before proceeding to Stage E.**

### Stage E — **Branch & Implement**
- **Rules:** Do **not** change public contracts and code without SPEC/ADR updates already in place.
- **Branch:** `feat/<ID>-<slug>` (or `fix/…`, `chore/…`).
- **Commits:** Conventional Commits → `<type>(scope): subject (#<ID>)`.
- **Docs-first enforcement:** If contracts change, first **propose SPEC snapshot + ADR stub**, then code.
- **Output:** code diffs/snippets, **unit & integration tests**, commit messages (Conventional Commits with `(#<ID>)`).
- **REVIEW STOP:** **Agent must stop here and wait for human review/approval before proceeding to Stage F.**

### Stage F — **Validate (Tests & Evals)**
- **Action:** Add/update **unit & integration tests**. For LLM/prompt/model changes, add/update **goldens + eval harness**; enforce thresholds in CI.
- **Output:** runnable tests, goldens, eval harness, thresholds; CI snippet.
- **Exit Gate:** Tests green; evals meet thresholds.
- **REVIEW STOP:** **Agent must stop here and wait for human review/approval before proceeding to Stage G.**

### Stage G — **Review & Merge**
- **PR must include:** links to **PRD**, **SPEC(s)** (exact filenames), **ADR(s)**, and **FEATURE**; status updates to `docs/features/schedule.md`.
- **Output:** PR description linking **PRD / SPEC(s) / ADR(s) / FEATURE**; schedule status updated.
- **Exit Gate (DoD):** Docs updated (SPEC snapshot if material), ADR accepted, tests/evals pass, schedule updated.
- **REVIEW STOP:** **Agent must stop here and wait for human review/approval before proceeding to Stage H.**

### Stage H — **Release Preparation (OP-NOTE)**
- **Action:** Write **OP-NOTE** (per feature or release). Include preflight (migrations/env/flags), deploy steps, monitoring, playbooks, rollback, post-deploy checks.
- **Output (generate file):** `docs/op-notes/op-<ID>-<slug>.md` or `op-release-<semver>.md` with preflight, deploy steps, monitoring, playbooks, rollback, post-deploy checks.
- **Cursor Rule Compliance:** Generated OP-NOTE must follow 05-op_note.md format and standards.
- **Exit Gate:** OP-NOTE approved.
- **REVIEW STOP:** **Agent must stop here and wait for human review/approval before proceeding to Stage I.**

### Stage I — **Deploy & Verify**
- **Action:** Follow OP-NOTE; canary if specified; run smoke tests; watch dashboards/alerts.
- **Exit Gate:** Post-deploy checks passed; toggle flags as per OP-NOTE; `docs/op-notes/index.md` updated.
- **REVIEW STOP:** **Agent must stop here and wait for human review/approval before proceeding to Stage J.**

### Stage J — **Close Loop**
- **Action:** Update `docs/specs/index.md` (mark Current); ensure PRD/FEATURE/ADR links are reciprocal; tag release; **close issues with "Closes #<ID>"**.
- **Exit Gate:** Schedule shows **Done**; retrospective TODOs captured (if any).
- **REVIEW STOP:** **Agent must stop here and wait for human review/approval before completing the workflow.**


## Stage Gating & Review Requirements
- **MANDATORY:** At **every stage**, the agent **MUST stop and wait for human review/approval** before proceeding to the next stage.
- **Cursor Rule Compliance:** Each generated file must follow its corresponding cursor rule format and standards.
- **No Auto-Advance:** The agent is **forbidden** from automatically proceeding to the next stage without explicit human approval.
- **Review Checkpoint:** Each stage ends with a **REVIEW STOP** instruction that the agent must follow.


## Change-Control Tripwires (snapshot triggers)
- **Contracts:** API/GraphQL/webhook/event schemas, headers, status/error taxonomy, DB schemas exposed to others, model/prompt I/O.
- **Topology:** components/paths/protocols/trust boundaries (e.g., adding Redis/Celery, async flows).
- **Framework roles:** Django/DRF, React, Redis (cache vs broker), Celery, Postgres, gateways, SSR/SPA changes.
- **SLOs:** availability/latency/error-rate/freshness targets.
→ If any tripwire is hit, **create a new dated SPEC**, update index, and reference ADRs.


## Traceability (IDs everywhere)
- Files: `ft-123-<slug>.md`, `op-123-<slug>.md`
- Branch: `feat/ft-123-<slug>` • PR title: `[ft-123] <title>` • Commits: `<type>(scope): subject (#ft-123)`


## Enforcement (automatic blockers)
- Code generated **before** required docs exist/updated → **block** and generate the missing doc file(s) first.
- Public **contracts** changed with no SPEC update → **block merge**.
- Material changes with no new **dated SPEC** snapshot → **block merge**.
- Non-trivial choices with no **ADR** → **block merge**.
- Feature work with no **FEATURE** file → **block merge**.
- Production-impacting changes with no **OP-NOTE** → **block deployment**.
- PR missing links (PRD/SPEC/ADR/FEATURE) → request changes.
- **Stage skipping without review** → **block and return to previous stage**.
- **Non-compliant file formats** → **block and regenerate following cursor rules**.