# Source Validation

Version: 1.0.0

## Purpose
Define how Hermes determines whether evidence is admissible, trustworthy, and suitable for influencing recommendations.

## Validation Criteria
1. Authority
2. Directness
3. Recency
4. Independence
5. Internal consistency
6. Business relevance

## Admissibility Rules
- Primary sources may support factual claims directly.
- Secondary sources require corroboration for material claims.
- Weak-signal sources generate hypotheses, not conclusions.

## Contradiction Handling
- Preserve conflicting evidence.
- Prefer newer primary evidence.
- Lower confidence until conflicts are resolved.

## Rejection Rules
Reject evidence that is:
- Unsupported
- Stale for dynamic facts
- Anonymous without corroboration
- Internally inconsistent
- Irrelevant to the research objective

## Validation Outcome
Every claim should be marked as:
- Accepted
- Accepted with caveats
- Hypothesis only
- Rejected