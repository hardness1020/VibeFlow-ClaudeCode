# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**VibeFlow-ClaudeCode** is a Claude Code development environment repository configured with a comprehensive documentation-first workflow and development governance system.

## Development Workflow

This repository implements a **10-stage docs-first development pipeline** with mandatory stage gating and human review at each step. All development must follow the workflow defined in `rules/00-workflow.md`.

### Required Documentation Pipeline

1. **Stage A (Initiate)**: Create PRD (`docs/prds/prd-YYYYMMDD.md`)
2. **Stage B (Specify)**: Create Tech Specs (`docs/specs/spec-YYYYMMDD-<spec>.md`)
3. **Stage C (Decide)**: Create ADRs (`docs/adrs/adr-YYYYMMDD-<slug>.md`)
4. **Stage D (Plan)**: Create Feature specs (`docs/features/ft-<ID>-<slug>.md`)
5. **Stage E (Implement)**: Implement with proper branching
6. **Stage F (Validate)**: Add tests and evaluations
7. **Stage G (Review)**: Create PR with all doc links
8. **Stage H (Release Prep)**: Create OP-NOTE (`docs/op-notes/op-<ID>-<slug>.md`)
9. **Stage I (Deploy)**: Follow OP-NOTE procedures
10. **Stage J (Close Loop)**: Update indexes and close issues


## File Structure and Naming Conventions

### Documentation Paths (Mandatory)
- **PRDs**: `docs/prds/prd-YYYYMMDD.md`
- **Tech Specs**: `docs/specs/spec-YYYYMMDD-<spec>.md` (spec types: system, api, frontend, llm)
- **ADRs**: `docs/adrs/adr-YYYYMMDD-<slug>.md`
- **Features**: `docs/features/ft-<ID>-<slug>.md`
- **OP-NOTEs**: `docs/op-notes/op-<ID>-<slug>.md`

### Branching and Commits
- **Branches**: `feat/<ID>-<slug>`, `fix/<ID>-<slug>`, `chore/<ID>-<slug>`
- **Commits**: Conventional Commits format: `<type>(scope): subject (#<ID>)`
- **PR Titles**: `[<ID>] <title>`

## Key Governance Rules

### Change Control Triggers
Creating new dated SPEC snapshots is required when changing:
- **Contracts**: API/GraphQL schemas, DB schemas, model I/O
- **Topology**: Components, protocols, trust boundaries
- **Framework roles**: Django/DRF, React, Redis usage patterns
- **SLOs**: Availability, latency, error rate targets

### Documentation Requirements
- **Architecture sections** must include: comprehensive diagram (frameworks + relationships) + component inventory table
- **PRDs** focus on business problems, users, metrics - NOT implementation
- **ADRs** required for non-trivial decisions (dependencies, auth, storage, versioning)
- **Feature specs** must include acceptance criteria, design diffs, test plans, telemetry

## Development Environment

### Container Setup
- Uses Node.js 20-slim base image with Claude Code pre-installed
- Development container configured in `.devcontainer/`
- Workdir: `/workspace`

### Current State
- **Repository**: Git-initialized, main branch
- **Language**: No source code files present yet (template repository)
- **Documentation**: Comprehensive Cursor rules for governance
- **Build/Test**: No build system configured (will depend on chosen tech stack)

## Critical Workflow Enforcement

1. **Docs-first mandate**: Generate/update required docs BEFORE code
2. **Stage gating**: Must stop and wait for human review at each stage
3. **No auto-advance**: Forbidden to proceed without explicit approval
4. **Cursor rule compliance**: Each file must follow corresponding cursor rule format
5. **Traceability**: Use feature IDs (`ft-123`) in all files, branches, PRs, commits

This repository prioritizes documentation quality, change control, and systematic development practices over rapid prototyping.