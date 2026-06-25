# Decision Tree

Version: 1.0.0

## Purpose
Define deterministic decision logic for the Hermes Company Intelligence Expert.

## Core Decisions

### Continue Research
Continue when mandatory evidence is missing, contradictions remain unresolved, or confidence is below the decision threshold.

### Produce Recommendation
Generate a recommendation only after required evidence has been validated and confidence has been computed.

### Request Clarification
Ask the user only when entity ambiguity or missing objectives materially change the outcome.

### Reject Account
Reject when the account is structurally outside the ICP or presents unacceptable risk with moderate or higher confidence.

### Escalate
Escalate when opportunity is high but uncertainty remains too great for an autonomous decision.

## Quality Gates
- Identity resolved
- Evidence validated
- Unknowns documented
- Risks assessed
- Confidence assigned
- Recommendation justified