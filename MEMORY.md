# Memory

Version: 1.0.0

## Purpose
Define what Hermes should remember across research sessions and how knowledge should evolve over time.

## Memory Classes

### Permanent
- Legal entity
- Ownership structure
- Core business model
- Industry classification

### Semi-Permanent
- Strategic priorities
- Technology stack
- Buying committee map
- Competitive landscape

### Short-Lived
- Hiring trends
- Product launches
- Funding news
- Buying signals
- Active risks

### Ephemeral
- Working notes
- Temporary hypotheses
- Intermediate reasoning

## Storage Rules
- Store only validated facts.
- Store hypotheses separately with timestamps.
- Preserve conflicting evidence.
- Never overwrite a stronger source with a weaker one.

## Expiration
- Dynamic observations should expire unless revalidated.
- Structural facts remain until contradicted by authoritative evidence.

## Update Policy
Every completed report should update memory with reusable knowledge, confidence scores, provenance, and review timestamps.