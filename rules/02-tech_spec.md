# TECH-SPEC — System/Component Engineering Plan

## Purpose and Scope
- Before building a new system/component or changing **interfaces, data flow, storage, frameworks, or SLOs**.
- Whenever a PR changes **contracts** (API/event/schema), **topology**, **framework roles** (e.g., Django, Redis, React), or **security posture**.

## Key Rules

### Path & Naming (mandatory)
- **Path:** `docs/specs/`
- **Filename pattern:** `spec-YYYYMMDD-<spec>.md`
  - `<spec>` is a short slug describing the scope. Common values:
    - `system` (end-to-end overview)
    - `api` (backend: Django/DRF, DB, Redis, Celery)
    - `frontend` (React/Vite)
    - `llm` (models/prompts/evals)
    - others allowed (e.g., `data`, `infra`) when needed
- **Examples:**  
  - `docs/specs/spec-20250822-system.md`  
  - `docs/specs/spec-20250822-api.md`  
  - `docs/specs/spec-20250822-frontend.md`  
  - `docs/specs/spec-20250822-llm.md`

### Versioning policy
- **Minor editorial fixes:** update the current file + add to **Changelog**.
- **Material changes** (contracts/SLOs/framework roles/topology): create a **new dated snapshot** and mark prior as **Superseded**.
- **Scope changes:** use a new `<spec>` slug (e.g., split `system` into `api`/`frontend`/`llm`).

### Required sections (must include)
- **Header** — version, file, status, FEATUREs (link its upstream artifacts), Contract Versions
- **Overview & Goals** (link Latest PRD)  
- **Architecture (Detailed)** — must meet *Architecture Requirements* below  
- **Interfaces & Data Contracts** (endpoints/events, schemas, error taxonomy, versioning)  
- **Data & Storage** (tables/indexes/migrations/retention)  
- **Reliability & SLIs/SLOs** (timeouts/retries/backpressure/limits)  
- **Security & Privacy** (authn/z, PII, secrets, logging)  
- **Evaluation Plan** (datasets, metrics, thresholds, test harness)  
- **Rollout & Ops Impact** (flags, canary, dashboards)  
- **Risks & Rollback; Open Questions**  
- **Changelog** (dated bullets of changes)

### Architecture Requirements (be explicit)
- **Comprehensive diagram** that explicitly **names each framework** and shows their **relationships** (edges, protocols, sync/async if relevant) and **trust boundaries** (public/private).  
   *Call out frameworks like: Django/DRF, Redis, Celery, Postgres, React, Nginx/CDN, LLM provider, etc.*
- **Component inventory (table)** listing, for every box in the diagram: **framework/runtime**, purpose, **interfaces (in/out)**, direct dependencies, scale/HA notes, and owner.

## Example
``` md
# Tech Spec — api
**Version**: v1.0.0
**File:** docs/specs/spec-20250822-api.md  
**Status:** Accepted
**PRD:** `prd-20250820.md`  
**Contract Versions:** API v1.3  •  Schema v1.1  •  Prompt Set v1.4

## Overview & Goals
Serve article credibility scores with p95 ≤ 400ms on cache hit, ≥99.9% availability.

## Architecture (Detailed)

### Topology (frameworks)


### Component Inventory
| Component  | Framework/Runtime        | Purpose                           | Interfaces (in/out)                            | Depends On        | Scale/HA            | Owner |
|------------|--------------------------|-----------------------------------|------------------------------------------------|-------------------|---------------------|-------|
| Frontend   | React (Vite, TS)         | SPA UI                            | In: HTTPS via CDN; Out: `GET/POST /api/*`      | CDN/Ingress       | Edge cached         | FE    |
| API        | Django DRF (Gunicorn)    | REST endpoints, auth, policy      | In: `/api/v1/*`; Out: Redis, Postgres, Celery  | Redis, Postgres   | 3 replicas, HPA     | BE    |
| Cache/Broker| Redis 7                 | Low-latency cache + Celery broker | `GET/SETEX score:*`, Celery queues             | —                 | Primary+replica     | SRE   |
| Workers    | Celery (Python)          | Async scoring/enrichment          | In: queue; Out: LLM HTTP, Redis, Postgres      | Redis, LLM, DB    | Autoscale by depth  | BE    |
| DB         | Postgres 15              | Durable storage                   | SQL                                            | —                 | Primary + PITR      | SRE   |
| LLM API    | HTTP client (requests)   | External scoring                  | HTTPS to provider                              | Provider          | External            | BE    |

## Interfaces & Data Contracts
POST `/api/v1/score` body `{url|string, lang?}` → `ScoreV1` | 202 w/ `task_id`  
Errors: 400 invalid, 422 unsupported, 429 throttled, 500 server.

## Data & Storage
Table `scores(id, url_hash, label, scores_json, created_at idx)`. Migration `20250822_add_scores.sql`.

## Reliability & SLOs
SLIs: availability, p95 latency; SLOs: 99.9% / 400ms. Circuit breaker on LLM ≥5% errors.

## Security & Privacy
No PII stored; redact URLs; keys via env; structured audit logs.

## Evaluation Plan
Goldens v0.4; F1 ≥ 0.72 gate in CI. Regression suite on PR.

## Rollout & Ops
Flag `feature.scorer.v1`; canary 10%; dashboards: latency, error rate.

## Risks & Rollback
LLM drift → pin model vX; high Redis miss → increase TTL or pre-warm.

## Changelog
- 2025-08-22: Accepted; API v1.3; Redis TTL 24h; Celery retry policy added.
- 2025-08-19: Draft; initial endpoints and cache keys.
```