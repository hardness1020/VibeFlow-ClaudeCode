# FEATURE — Executable Spec per Deliverable

## When to use
- For every ticket/deliverable before starting branch/PR; update during development.

## Key Rules

### Path & Naming (mandatory)
- **Path:** `docs/features/`
- **Naming:** `ft-<ID>-<slug>.md` (e.g., `ft-001-hover-badge.md`)
- **Schedule file:** `docs/features/schedule.md` (IDs, titles, status; keep in sync)

### Linking & Traceability
- Each feature file must link its upstream artifacts:
  - **TECH-SPECs:** `docs/specs/spec-YYYYMMDD-<spec>.md`

### Required sections (must include)
  - **Header:** ID, file, owner, TECH-SPECs
  - **Acceptance Criteria** (clear, testable; Gherkin or checklist)
  - **Design Changes** (UI/API/schema diffs; examples)
  - **Test & Eval Plan** (unit/integration + AI eval thresholds & goldens)
  - **Telemetry & Metrics to watch** (dashboards, alerts)
  - **Rollout/Canary & Rollback**
  - **Edge Cases & Risks**
- **Traceability:** Use `<ID>` in branch (`feat/<ID>-slug`), PR title `[<ID>]`, and commits `... (#<ID>)`.


## Example
```md
# Feature — 001 hover-badge
**File:** docs/features/ft-001-hover-badge.md  
**Owner:** Marcus 
**TECH-SPECs:** `spec-20250822-frontend.md`, `spec-20250822-backend.md`

## Acceptance Criteria
- [ ] Badge shows G/Y/R ≤200ms with cache hit
- [ ] Tooltip lists top-3 rationales
- [ ] Works on en/zh pages; degrades gracefully if scores missing

## Design Changes
Endpoint `/v1/score` + new UI component `<CredBadge />`  
Schema diff: add `rationales[]: string[≤3]`

## Test & Eval Plan
- Unit: score mapper, UI renders states  
- Integration: end-to-end DOM test  
- AI eval: goldens v0.4, F1≥0.72 must pass

## Telemetry
Dashboards: badge latency p95, error rate; alert >2% errors

## Rollout
Enable 10% canary; rollback by disabling `feature.hover_badge`

## Edge Cases & Risks
Very long articles; unsupported langs → fallback "Check Article"
```