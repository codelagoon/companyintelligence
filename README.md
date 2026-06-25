# Hermes Company Intelligence

This repository houses the production-grade knowledge base, operating manuals, schemas, and skill ecosystem for the **Hermes Company Intelligence** agent platform.

---

## 1. Repository Navigation Directory

Below is the canonical index of all active architectural and GTM documents in this repository.

### Core Architectural Documents
- [README.md](file:///Users/george/companyintelligence/README.md): Repository navigation directory.
- [GOALS.md](file:///Users/george/companyintelligence/GOALS.md): Primary and secondary mission goals, success metrics, and anti-goals.
- [INVARIANTS.md](file:///Users/george/companyintelligence/INVARIANTS.md): Non-negotiable system constraints and truth requirements.
- [PRINCIPLES.md](file:///Users/george/companyintelligence/PRINCIPLES.md): Core first principles and design implications.
- [STATE_MACHINE.md](file:///Users/george/companyintelligence/STATE_MACHINE.md): Deterministic execution state transitions.
- [SOURCE_HIERARCHY.md](file:///Users/george/companyintelligence/SOURCE_HIERARCHY.md): Authoritative hierarchy and ranking of information sources.
- [OUTPUT_SCHEMA.md](file:///Users/george/companyintelligence/OUTPUT_SCHEMA.md): Markdown and JSON output structure contracts.
- [TOOLS.md](file:///Users/george/companyintelligence/TOOLS.md): Tool orchestration, rate-limiting, and error-handling policies.

### Core Implementation Documents
- [SKILL.md](file:///Users/george/companyintelligence/SKILL.md): Operating manual and core execution lifecycle for the Expert.
- [HEURISTICS.md](file:///Users/george/companyintelligence/HEURISTICS.md): Validated decision rules library for enterprise B2B and financial diagnostics.
- [WORKFLOW.md](file:///Users/george/companyintelligence/WORKFLOW.md): Step-by-step procedures, entry/exit criteria, and execution gates.
- [THINKING_POLICIES.md](file:///Users/george/companyintelligence/THINKING_POLICIES.md): Cognitive policies, reasoning constraints, and bias mitigations.
- [DECISION_TREE.md](file:///Users/george/companyintelligence/DECISION_TREE.md): Deterministic decision trees (DT-001 through DT-010) for execution control.
- [CONFIDENCE_ENGINE.md](file:///Users/george/companyintelligence/CONFIDENCE_ENGINE.md): Mathematical scoring algorithms and calibration procedures.
- [SOURCE_VALIDATION.md](file:///Users/george/companyintelligence/SOURCE_VALIDATION.md): Integrity checks, bias detection, and freshness decay models.
- [BUYING_SIGNALS.md](file:///Users/george/companyintelligence/BUYING_SIGNALS.md): Catalog of validated enterprise buying signals and action patterns.
- [RED_FLAGS.md](file:///Users/george/companyintelligence/RED_FLAGS.md): Catalog of risk factors, severity levels, and false positive metrics.
- [MEMORY.md](file:///Users/george/companyintelligence/MEMORY.md): Storage schemas and retrieval algorithms for persistent knowledge.

### System Integration
- [HERMES.md](file:///Users/george/companyintelligence/HERMES.md): Foundational multi-agent constitution and orchestration integration rules.

---

## 2. Hermes Skill Ecosystem (skills/)

This directory houses the modular, reusable Hermes skills that dynamically select, compose, and orchestrate GTM and corporate intelligence tasks.

- [skills/company-intelligence/SKILL.md](file:///Users/george/companyintelligence/skills/company-intelligence/SKILL.md): Resolves target company profiles, technographics, and signals.
- [skills/meeting-prep/SKILL.md](file:///Users/george/companyintelligence/skills/meeting-prep/SKILL.md): Compiles attendee profiles and conversation guides.
- [skills/buyer-discovery/SKILL.md](file:///Users/george/companyintelligence/skills/buyer-discovery/SKILL.md): Scans registries to qualify targets matching an ICP.
- [skills/buying-committee/SKILL.md](file:///Users/george/companyintelligence/skills/buying-committee/SKILL.md): Maps stakeholders, roles, and reporting lines.
- [skills/account-prioritization/SKILL.md](file:///Users/george/companyintelligence/skills/account-prioritization/SKILL.md): Ranks target pipeline accounts by buying signal density and risk.
- [skills/cold-outreach/SKILL.md](file:///Users/george/companyintelligence/skills/cold-outreach/SKILL.md): Generates hyper-personalized cold outreach copy without boilerplate.
- [skills/warm-outreach/SKILL.md](file:///Users/george/companyintelligence/skills/warm-outreach/SKILL.md): Coordinates relational hooks for mutual connections and ex-users.
- [skills/executive-brief/SKILL.md](file:///Users/george/companyintelligence/skills/executive-brief/SKILL.md): Renders scanned findings into high-density briefing documents.
- [skills/competitive-intelligence/SKILL.md](file:///Users/george/companyintelligence/skills/competitive-intelligence/SKILL.md): Audits competitor tech stacks, pricing, and positioning.
- [skills/pilot-qualification/SKILL.md](file:///Users/george/companyintelligence/skills/pilot-qualification/SKILL.md): Scores integration feasibility and procurement barriers.
- [skills/stakeholder-research/SKILL.md](file:///Users/george/companyintelligence/skills/stakeholder-research/SKILL.md): Conducts targeted OSINT profiling of corporate stakeholders.
- [skills/account-monitoring/SKILL.md](file:///Users/george/companyintelligence/skills/account-monitoring/SKILL.md): Tracks monitored portfolios for dynamic executive and tech stack changes.
- [skills/osint-expert/SKILL.md](file:///Users/george/companyintelligence/skills/osint-expert/SKILL.md): Collects, validates, and indexes public data across 12 intelligence domains.
- [skills/evidence-acquisition/SKILL.md](file:///Users/george/companyintelligence/skills/evidence-acquisition/SKILL.md): Canonical GTM data engine for collecting, normalizing, and indexing public evidence.

---

## 3. Project Administration & Guidelines

Core guidelines, guidelines, and repository administrative parameters for open-source developers and maintainers:

- [ARCHITECTURE.md](file:///Users/george/companyintelligence/ARCHITECTURE.md): System block diagrams, module ownership, and integration flows.
- [ONBOARDING.md](file:///Users/george/companyintelligence/ONBOARDING.md): Onboarding guide, environment setup, and maintenance protocols.
- [CONTRIBUTING.md](file:///Users/george/companyintelligence/CONTRIBUTING.md): Standard developer contribution rules, PR templates, and validation checklists.
- [CHANGELOG.md](file:///Users/george/companyintelligence/CHANGELOG.md): History of revisions, updates, and releases.
- [ROADMAP.md](file:///Users/george/companyintelligence/ROADMAP.md): Strategic roadmap for V1.1 and V2.0 developments.
- [LICENSE](file:///Users/george/companyintelligence/LICENSE): MIT Open Source License.
- [CODEOWNERS](file:///Users/george/companyintelligence/CODEOWNERS): Code ownership registry.