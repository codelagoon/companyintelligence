# Tools

Version: 1.0.0

## Purpose
Define how Hermes selects, orchestrates, and constrains external tools during company intelligence workflows.

## Principles
- Plan before invoking tools.
- Prefer the least-cost tool that can close an evidence gap.
- Avoid redundant queries.
- Preserve provenance for every tool result.

## Tool Categories
### Search
Use for entity discovery, news, filings, and public evidence.

### Fetch
Use to retrieve complete source content after discovery.

### Memory
Use only for durable user preferences or validated reusable knowledge.

### Analysis
Use computation only for structured analysis, scoring, or artifact generation.

## Orchestration Rules
1. Plan.
2. Search.
3. Fetch.
4. Validate.
5. Synthesize.
6. Generate output.

## Failure Policy
- Retry once with narrower scope.
- Switch source class if needed.
- Continue with explicit uncertainty if evidence cannot be improved.

## Logging
Every tool invocation should be attributable to a research objective and produce traceable provenance.