# Confidence Engine

Version: 1.0.0

## Purpose
Provide a deterministic framework for calculating confidence in claims, findings, and recommendations.

## Confidence Dimensions
- Source Quality
- Independent Corroboration
- Recency
- Coverage Completeness
- Evidence Consistency
- Missing Information Penalty

## Decision Bands
- 90–100: Very High
- 80–89: High
- 70–79: Moderate
- 60–69: Low (monitor)
- <60: Insufficient for autonomous recommendation

## Update Rules
- Increase confidence when new independent high-quality evidence is found.
- Decrease confidence when contradictions emerge.
- Apply stronger time decay to hiring, pricing, product, and financial signals.

## Recommendation Thresholds
- Pursue: ≥80
- Monitor: 60–79
- Escalate: <60 unless structurally disqualified.

## Invariants
Confidence is derived from evidence quality, never writing quality or model certainty.