# Source Hierarchy

## Version: 2.0.0

## Purpose

Define the canonical ranking of evidence sources used by the Company Intelligence Expert. This ranking determines source authority and is used deterministically during conflict resolution and confidence scoring.

---

## Tier 1 — Regulatory and Legal (Highest Authority)
- SEC filings (10-K, 10-Q, 8-K, proxy statements)
- Government company registries (e.g., UK Companies House, state registries)
- Patent grants and filings (e.g., USPTO)
- Court records and active litigation filings
- Regulatory body enforcement actions and databases

---

## Tier 2 — Audited and Investor Relations Sources
- Audited annual reports
- Earnings call transcripts
- Official investor presentations
- Bond offering documents
- Official corporate governance reports

---

## Tier 3 — Structured Business and Industry Data
- Business registries and firmographic databases (e.g., D&B, ZoomInfo)
- Job posting aggregators (e.g., Indeed)
- Independent industry analyst reports (e.g., Gartner, Forrester)
- Trade association data and registries

---

## Tier 4 — Semi-Structured Public and Self-Reported Sources
- Company official websites and blogs
- Product documentation and API portals
- Official press releases
- Technical case studies and partner announcements
- Engineering blogs and open-source repositories (e.g., GitHub)

---

## Tier 5 — Unstructured and Social Signals (Lowest Authority)
- Professional networking profiles (e.g., LinkedIn)
- Discussion forums (e.g., Reddit, community forums)
- Employer reviews and employee feedback (e.g., Glassdoor)
- Social media posts (e.g., X)
- Press rumors and third-party news blogs

---

## Conflict Resolution Rules

When data points from multiple sources contradict, resolve using the rules in [DECISION_TREE.md](file:///Users/george/companyintelligence/DECISION_TREE.md#dt-004-conflict-resolution):
1. **Tier Superiority**: The source belonging to the higher tier prevails by default.
2. **Temporal Precedence**: If sources are in the same tier, the source with the more recent verified timestamp prevails.
3. **Contradiction Penalty**: Apply a confidence penalty ($P_c$) to the resolved claim as specified in [CONFIDENCE_ENGINE.md](file:///Users/george/companyintelligence/CONFIDENCE_ENGINE.md#24-contradiction-penalty-p_c).