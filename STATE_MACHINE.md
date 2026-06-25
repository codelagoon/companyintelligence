# State Machine

## Version: 1.1.0

## Purpose
Define the deterministic execution states and transition paths for the Company Intelligence Expert.

## States
1. **IDLE**: Waiting for incoming research request.
2. **INITIALIZE**: Parsing request, setting objectives and constraints.
3. **PLAN**: Constructing prioritized queries and tool sequence.
4. **ENTITY_RESOLUTION**: Disambiguating company identity and corporate structure.
5. **RESEARCH**: Querying search tools and caching raw page payloads.
6. **SOURCE_VALIDATION**: Assessing reliability, decay, and bias of source documents.
7. **EVIDENCE_SYNTHESIS**: Extracting structured facts and resolving contradictions.
8. **HYPOTHESIS_GENERATION**: Inferring pain points and cataloging buying signals.
9. **RISK_ASSESSMENT**: Evaluating red flags and severity metrics.
10. **CONFIDENCE_SCORING**: Calculating confidence values for claims and overall fit.
11. **SELF_REVIEW**: Auditing deliverables against Quality Gates.
12. **OUTPUT_GENERATION**: Serializing JSON and rendering Markdown reports.
13. **MEMORY_UPDATE**: Committing reusable knowledge to persistence folders.
14. **COMPLETE**: Finalizing metadata and reporting delivery status.
15. **FAILED**: Handling unrecoverable exceptions and formatting error reports.

## Transition Rules
- **IDLE → INITIALIZE** on new task intake.
- **INITIALIZE → PLAN** when objectives and variables checklist are resolved.
- **PLAN → ENTITY_RESOLUTION** after query list is populated.
- **ENTITY_RESOLUTION → RESEARCH** when corporate identity meets confidence threshold ($\ge 0.80$).
- **RESEARCH → SOURCE_VALIDATION** after search queries are executed.
- **SOURCE_VALIDATION → EVIDENCE_SYNTHESIS** when admitted evidence checklist is compiled.
- **EVIDENCE_SYNTHESIS → HYPOTHESIS_GENERATION** after structuring facts.
- **HYPOTHESIS_GENERATION → RISK_ASSESSMENT** once pain and buying signals are mapped.
- **RISK_ASSESSMENT → CONFIDENCE_SCORING** once risk matrix is compiled.
- **CONFIDENCE_SCORING → SELF_REVIEW** once confidence calculations are resolved.
- **SELF_REVIEW → OUTPUT_GENERATION** if all Quality Gates pass.
- **SELF_REVIEW → RESEARCH** if Quality Gates fail due to evidence gaps (and query budget remains).
- **SELF_REVIEW → FAILED** if Quality Gates fail and query budget is exhausted.
- **OUTPUT_GENERATION → MEMORY_UPDATE** when file writes are successful.
- **MEMORY_UPDATE → COMPLETE** when database writes are complete or cached.
- **COMPLETE → IDLE** after delivery confirmation is transmitted.
- **FAILED → IDLE** after error report transmission is finalized.
- **Any State → FAILED** upon throwing an unrecoverable exception.

## Recovery and Rollbacks
When a validation check or gate fails:
1. Rollbacks must return execution to the earliest state capable of resolving the failure (e.g., source validation failures roll back to `RESEARCH` to gather alternative evidence).
2. The system must preserve previously validated parameters in the cache to prevent redundant queries.
3. Every rollback operation decrements the remaining query budget and increments the error counter.