# State Machine

Version: 1.0.0

## Purpose
Define the deterministic execution states for the Hermes Company Intelligence Expert.

## States
1. IDLE
2. INITIALIZE
3. PLAN
4. ENTITY_RESOLUTION
5. RESEARCH
6. SOURCE_VALIDATION
7. EVIDENCE_SYNTHESIS
8. HYPOTHESIS_GENERATION
9. RISK_ASSESSMENT
10. CONFIDENCE_SCORING
11. SELF_REVIEW
12. OUTPUT_GENERATION
13. MEMORY_UPDATE
14. COMPLETE
15. FAILED

## Transition Rules
- IDLE → INITIALIZE on new task.
- INITIALIZE → PLAN when objective is resolved.
- PLAN → ENTITY_RESOLUTION after research plan creation.
- ENTITY_RESOLUTION → RESEARCH when identity confidence is sufficient.
- RESEARCH → SOURCE_VALIDATION after evidence collection.
- SOURCE_VALIDATION → EVIDENCE_SYNTHESIS when mandatory evidence thresholds are met.
- EVIDENCE_SYNTHESIS → HYPOTHESIS_GENERATION after extracting facts.
- HYPOTHESIS_GENERATION → RISK_ASSESSMENT.
- RISK_ASSESSMENT → CONFIDENCE_SCORING.
- CONFIDENCE_SCORING → SELF_REVIEW.
- SELF_REVIEW → OUTPUT_GENERATION if quality gates pass; otherwise return to RESEARCH.
- OUTPUT_GENERATION → MEMORY_UPDATE.
- MEMORY_UPDATE → COMPLETE.
- Any state → FAILED upon unrecoverable error.

## Recovery
Recoverable failures return to the earliest state capable of resolving the missing evidence while preserving validated work.