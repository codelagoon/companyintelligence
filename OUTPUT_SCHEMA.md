# Output Schema

Version: 1.0.0

## Purpose
Define the canonical output formats produced by the Hermes Company Intelligence Expert.

## Required Sections
1. Executive Summary
2. Company Identity
3. Recommendation
4. Confidence Assessment
5. Evidence Summary
6. Business Model
7. Buying Signals
8. Pain Hypotheses
9. Risks
10. Unknowns
11. Next Actions
12. Sources

## JSON Contract
Top-level fields:
- company
- objective
- recommendation
- confidence
- facts
- hypotheses
- signals
- risks
- unknowns
- sources
- metadata
- next_actions

## Markdown Contract
Every report should:
- Begin with the recommendation.
- Clearly separate facts from hypotheses.
- Label confidence for major conclusions.
- Include evidence references.
- End with prioritized next actions.

## Validation Rules
- No unsupported material claims.
- No missing required sections.
- Unknowns must be explicit.
- Every recommendation must be evidence-backed.