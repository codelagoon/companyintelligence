# Repository Architecture Specification

## Version: 1.0.0

## Purpose

This document defines the structural relationship, data flow, and compositional boundaries of the **Hermes Company Intelligence** expert system. It serves as the canonical blueprint for developers and orchestration layers.

---

## 1. System Block Diagram

The system operates as a layered architecture where modular skills consume core engines, which are constrained by foundational invariants, tools, and memory layers.

```
+-----------------------------------------------------------------------------------+
|                              Downstream GTM Skills                                |
| (Outreach, Prioritization, Meeting Prep, Pilot Qual, Briefing, Buyer Discovery)   |
+-----------------------------------------------------------------------------------+
                                         |
                                         v
+-----------------------------------------------------------------------------------+
|                        Specialized Collection Skills                              |
|         (OSINT Expert, Company Intelligence, Evidence Acquisition)                |
+-----------------------------------------------------------------------------------+
                                         |
                                         v
+-----------------------------------------------------------------------------------+
|                                 Core Engines                                      |
|    (Confidence Engine, Source Validation, Heuristics, Decision Trees, Workflows)   |
+-----------------------------------------------------------------------------------+
                                         |
                                         v
+-----------------------------------------------------------------------------------+
|                              Foundational Layer                                   |
| (Invariants, Principles, Source Hierarchy, Output Schema, Tools, Memory, State)   |
+-----------------------------------------------------------------------------------+
```

---

## 2. Core Modules and System Ownership

Each file in this repository owns a single, non-overlapping responsibility:

| Document | Category | Operational Responsibility |
|---|---|---|
| [README.md](file:///Users/george/companyintelligence/README.md) | Navigation | Repository navigation and directory structure. |
| [GOALS.md](file:///Users/george/companyintelligence/GOALS.md) | Architecture | System objectives, success metrics, and optimization priorities. |
| [INVARIANTS.md](file:///Users/george/companyintelligence/INVARIANTS.md) | Architecture | Strict operational boundaries and truth assertions. |
| [PRINCIPLES.md](file:///Users/george/companyintelligence/PRINCIPLES.md) | Architecture | First principles and cognitive review questions. |
| [STATE_MACHINE.md](file:///Users/george/companyintelligence/STATE_MACHINE.md) | State | Canonical 15-state transitions and rollback rules. |
| [SOURCE_HIERARCHY.md](file:///Users/george/companyintelligence/SOURCE_HIERARCHY.md) | Sources | 5-tier source authority classifications. |
| [OUTPUT_SCHEMA.md](file:///Users/george/companyintelligence/OUTPUT_SCHEMA.md) | Output | Markdown and Draft-07 JSON Schema verification contracts. |
| [TOOLS.md](file:///Users/george/companyintelligence/TOOLS.md) | Tools | Orchestration constraints, rate-limits, and retry logic. |
| [SKILL.md](file:///Users/george/companyintelligence/SKILL.md) | Operating | Operating manual and state-to-workflow mapping for the Expert. |
| [HEURISTICS.md](file:///Users/george/companyintelligence/HEURISTICS.md) | Logic | Reusable, validated decision rules for corporate analysis. |
| [WORKFLOW.md](file:///Users/george/companyintelligence/WORKFLOW.md) | Workflow | 15-phase operational procedures with detailed checklists. |
| [THINKING_POLICIES.md](file:///Users/george/companyintelligence/THINKING_POLICIES.md) | Reasoning | 20 cognitive policies for bias control and disconfirmation. |
| [DECISION_TREE.md](file:///Users/george/companyintelligence/DECISION_TREE.md) | Logic | Conditional trees (DT-001 to DT-010) for pivot execution. |
| [CONFIDENCE_ENGINE.md](file:///Users/george/companyintelligence/CONFIDENCE_ENGINE.md) | Scoring | Scoring math, recency decay, and opportunity calibration. |
| [SOURCE_VALIDATION.md](file:///Users/george/companyintelligence/SOURCE_VALIDATION.md) | Validation | Freshness constants, bias matrix, and source checkers. |
| [BUYING_SIGNALS.md](file:///Users/george/companyintelligence/BUYING_SIGNALS.md) | Signals | Taxonomies, urgency ratings, and detection methods for buy-intent. |
| [RED_FLAGS.md](file:///Users/george/companyintelligence/RED_FLAGS.md) | Risks | Taxonomies, severity weights, and false positives for corporate risk. |
| [MEMORY.md](file:///Users/george/companyintelligence/MEMORY.md) | Persistence | Storage schemas and decay weights for long-term memory. |
| [HERMES.md](file:///Users/george/companyintelligence/HERMES.md) | Integration | Constitutions and multi-agent coordination contracts. |

---

## 3. Data Flow Diagram

The following sequence outlines how an incoming query transforms into a validated strategic decision:

```
[User Query] 
     │
     ▼
Phase 2: INITIALIZE ──► Phase 3: PLAN ──► Phase 4: ENTITY_RESOLUTION
                                                   │
                                                   ▼
                                           Phase 5: RESEARCH (Tools)
                                                   │
                                                   ▼
                                           Phase 6: SOURCE_VALIDATION (Checklists)
                                                   │
                                                   ▼
                                           Phase 7: EVIDENCE_SYNTHESIS
                                                   │
                                                   ▼
[Hypothesis] ◄── [Heuristics] ◄── [Signals/Risks] ◄───┘
     │
     ▼
Phase 10: CONFIDENCE_SCORING (Math Formulas)
     │
     ▼
Phase 11: SELF_REVIEW (Quality Gates) ──► Phase 12: OUTPUT_GENERATION ──► [Output]
```

---

## 4. Multi-Skill Composition Rules

Downstream skills compose their functionality by cascading triggers to upstream collections:

1.  **Reference Integrity**: Skills must not implement their own data extraction or validation. They execute calls to [skills/evidence-acquisition](file:///Users/george/companyintelligence/skills/evidence-acquisition/SKILL.md) and consume the structured JSON payload.
2.  **No Override**: Under no circumstances may a downstream skill override the confidence scoring calculated by the [CONFIDENCE_ENGINE.md](file:///Users/george/companyintelligence/CONFIDENCE_ENGINE.md).
3.  **Audit trail**: All skill compositions must append their execution parameters to the final `Execution Trace` output, maintaining a clear audit trail from raw tool calls to the final outreach or briefing deliverable.
