# VibeFlow-ClaudeCode

⭐ **Star this repo** | 🍴 **Fork to get started** | 📖 **Use for your projects**

A comprehensive documentation-first development workflow template for systematic software engineering with Claude Code.

## Overview

VibeFlow-ClaudeCode provides a battle-tested, 10-stage development pipeline that enforces documentation-first practices, stage gating, and human review checkpoints. Perfect for teams and individuals who want to build software systematically with proper governance, traceability, and quality controls.

## What's Inside

- **📋 10-Stage Development Pipeline**: From problem identification to deployment and closure
- **📚 Documentation Templates**: PRDs, Tech Specs, ADRs, Feature specs, and OP-NOTEs
- **🎯 Stage Gating**: Mandatory human review at each stage prevents rushing ahead
- **🔄 Change Control**: Automatic triggers for documentation updates when contracts/topology change
- **📈 Traceability**: Feature IDs flow through files, branches, PRs, and commits
- **⚡ Claude Code Optimized**: Designed specifically for AI-assisted development workflows

## Quick Start

1. **Fork & Star This Repository** ⭐
   - Click the **Fork** button to create your own copy
   - Click the **Star** button to bookmark and support the project
   - Clone your fork to get started

2. **Initialize Your Project**
   ```bash
   git clone https://github.com/YOUR-USERNAME/VibeFlow-ClaudeCode.git
   cd VibeFlow-ClaudeCode
   ```

3. **Start with a Problem**
   - Have an idea? Begin at Stage A by creating a PRD in `docs/prds/`
   - Small bug fix? Skip to Stage D with a note in your PR

4. **Follow the Workflow**
   - Each stage has clear entry/exit criteria
   - Human review required before advancing
   - Documentation must be created before code

## Stage Overview

| Stage | Name | Output | Gate |
|-------|------|--------|------|
| **A** | Initiate | PRD document | Business impact validated |
| **B** | Specify | Tech Specs with architecture | Contracts/topology documented |
| **C** | Decide | ADRs for key decisions | Non-trivial choices recorded |
| **D** | Plan | Feature specs with criteria | Implementation plan approved |
| **E** | Implement | Code + tests | Implementation complete |
| **F** | Validate | Test results + evals | Quality gates passed |
| **G** | Review | PR with doc links | Code review approved |
| **H** | Release Prep | OP-NOTEs | Deployment plan ready |
| **I** | Deploy | Live system | Production verified |
| **J** | Close Loop | Updated indexes | Retrospective complete |

## Documentation Structure

```
docs/
├── prds/           # Product Requirements (business goals)
├── specs/          # Technical Specifications (architecture)
├── adrs/           # Architectural Decision Records (choices)
├── features/       # Feature Specifications (implementation)
└── op-notes/       # Operational Notes (deployment/runbooks)
```

## Key Principles

### 🎯 **Docs-First Mandate**
All documentation must exist before code is written. No exceptions.

### 🚦 **Stage Gating**
Human review and approval required at every stage transition.

### 📝 **Change Control**
Automatic documentation snapshots triggered by:
- API/schema changes (contracts)
- Component/protocol changes (topology)
- Framework role changes
- SLO modifications

### 🔗 **Full Traceability**
Feature IDs (`ft-123`) appear in:
- File names: `ft-123-user-auth.md`
- Branches: `feat/ft-123-user-auth`
- PRs: `[ft-123] Add user authentication`
- Commits: `feat(auth): add JWT validation (#ft-123)`

## Rules Reference

The `rules/` directory contains the complete governance framework:

- `00-workflow.md` - Main workflow and stage definitions
- `01-prd.md` - Product Requirements authoring guide
- `02-tech_spec.md` - Technical specification standards
- `03-adr.md` - Architectural decision record format
- `04-feature.md` - Feature specification template
- `05-op_note.md` - Operations and runbook guidelines
- `06-markdown.md` - Markdown formatting standards

## Example: Implementing a New Feature

```bash
# 1. Create PRD (Stage A)
docs/prds/prd-20250922.md

# 2. Create Tech Spec (Stage B)
docs/specs/spec-20250922-api.md

# 3. Record key decisions (Stage C)
docs/adrs/adr-20250922-auth-provider.md

# 4. Plan feature (Stage D)
docs/features/ft-001-user-login.md

# 5. Implement (Stage E)
git checkout -b feat/ft-001-user-login
# ... code with tests ...

# 6. Validate (Stage F)
npm test && npm run lint

# 7. Create PR (Stage G)
# PR links all docs: PRD, SPEC, ADR, FEATURE

# 8. Deploy (Stages H-I)
docs/op-notes/op-001-user-auth-deploy.md

# 9. Close (Stage J)
# Update indexes, close issues
```

## Benefits

✅ **Reduced Technical Debt**: Documentation requirements prevent shortcuts \
✅ **Improved Handoffs**: Complete context for any team member \
✅ **Change Impact Analysis**: Clear dependency tracking \
✅ **Audit Trail**: Full history of decisions and rationale \
✅ **Quality Assurance**: Multiple review checkpoints \
✅ **Knowledge Retention**: Tribal knowledge captured in docs

## When to Use This Workflow

**Perfect for:**
- Production systems with multiple stakeholders
- Teams that need governance and compliance
- Projects requiring change management
- Long-term maintainable codebases
- AI-assisted development workflows

**Consider alternatives for:**
- Quick prototypes or experiments
- Solo learning projects
- Throwaway code or demos

## Getting Help

- Read the `rules/` documentation for detailed guidance
- Each rule file includes examples and templates
- Follow the stage-by-stage process for best results
- Remember: documentation quality over speed

## Contributing & Community

🌟 **Found this useful?** Star the repository to show your support!

🍴 **Want to customize?** Fork it and make it your own!

🔧 **Have improvements?** Pull requests are welcome!

📢 **Share with others** who might benefit from systematic development workflows

---

**Built for systematic software development with Claude Code** 🤖

*This template enforces best practices while maintaining development velocity through AI assistance and clear governance structures.*

### Ready to get started? [⭐ Star](../../stargazers) • [🍴 Fork](../../fork) • [📖 Use it now!](../../generate)
