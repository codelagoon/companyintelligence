# Developer Onboarding & Maintenance Guide

## Version: 1.0.0

---

## 1. Developer Onboarding

Welcome to the **Company Intelligence Expert** repository. This guide helps developers set up, navigate, and maintain this expert knowledge base.

### 1.1 Local Workspace Setup
1.  Clone the repository to your local workspace:
    ```bash
    git clone https://github.com/codelagoon/companyintelligence.git
    cd companyintelligence
    ```
2.  Configure your Markdown editor (we recommend VS Code with Markdown All in One and Markdown Links extensions).
3.  Set up local pre-commit hooks to automate basic styling audits (see V1.1 Roadmap objectives).

### 1.2 Repository Navigation
- Start with [README.md](file:///Users/george/companyintelligence/README.md) to locate active documents.
- Review [ARCHITECTURE.md](file:///Users/george/companyintelligence/ARCHITECTURE.md) to understand data flows and compositional boundaries.
- Read [SKILL.md](file:///Users/george/companyintelligence/SKILL.md) to learn how the core operating manual maps execution phases.

---

## 2. Maintenance Protocols

To preserve the production-grade quality of this repository, maintainers must enforce these rules:

### 2.1 Terminology Enforcement
When editing files, verify that all references to the following core systems match the canonical specifications:
1.  **State Machine**: Must refer to the 15 states in [STATE_MACHINE.md](file:///Users/george/companyintelligence/STATE_MACHINE.md) using exact names (e.g. `ENTITY_RESOLUTION`, `CONFIDENCE_SCORING`).
2.  **Source Tiers**: Must refer to the 5-tier classification in [SOURCE_HIERARCHY.md](file:///Users/george/companyintelligence/SOURCE_HIERARCHY.md).
3.  **Confidence Bands**: Must use the 5 ranges defined in [CONFIDENCE_ENGINE.md](file:///Users/george/companyintelligence/CONFIDENCE_ENGINE.md#confidence-scale) (Speculative, Low, Moderate, High, Very High).

### 2.2 Eviction and Deprecation Policy
Information and heuristics decay over time. Every 180 days, maintainers should review:
1.  **Stale Heuristics**: Audit [HEURISTICS.md](file:///Users/george/companyintelligence/HEURISTICS.md) for rules that have produced false positives or have been superseded by platform shifts. Retire or update them.
2.  **Freshness Decay Constants**: Adjust the volatility decay constants ($\gamma$) in [SOURCE_VALIDATION.md](file:///Users/george/companyintelligence/SOURCE_VALIDATION.md#2-freshness-decay-functions) if telemetry indicates that stacks or org structures are shifting at different rates.

### 2.3 Quality Gate Enforcement
No pull request may be merged into `main` unless it satisfies the Public Release Checklist:
- All absolute file links resolve correctly.
- No placeholder text or TODO comments remain.
- The modified files are free of generic marketing prose.
- Version history updates are logged in the headers and [CHANGELOG.md](file:///Users/george/companyintelligence/CHANGELOG.md).
