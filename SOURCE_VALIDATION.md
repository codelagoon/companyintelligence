# Source Validation Specification

## Version: 2.0.0

## Purpose

This document defines the validation protocols, admissibility rules, and decay functions used by the Company Intelligence Expert to assess the reliability of evidence before it is admitted into the analysis engine.

---

## 1. Per-Source-Type Validation Checklists

Before admitting a source document, you must run it through the corresponding checklist. If a check fails, apply the designated penalty or reject the source.

### 1.1 Regulatory & Legal Sources (Tier 1)
- [ ] **Registration Verification**: Is the document retrieved from an official government registry website (e.g., `sec.gov`, `gov.uk`) or court system?
- [ ] **Signature Check**: Is the document signed by authorized corporate officers (e.g., CFO, CEO) under penalty of perjury?
- [ ] **Period Validation**: Does the document specify the exact period ended (e.g., Q3 ended September 30, 2025)?
- *Action on Failure*: Reject the source if it is a draft or unfiled version.

### 1.2 Investor Relations & Audited Financials (Tier 2)
- [ ] **Audit Attribution**: Are the financial figures accompanied by an independent auditor's report (e.g., Deloitte, EY, PwC, KPMG)?
- [ ] **GAAP Reconciliation**: Are non-GAAP metrics (e.g., Adjusted EBITDA) clearly reconciled to GAAP equivalents?
- [ ] **Transcript Verification**: If using an earnings transcript, is it sourced from an established financial news provider or the official company IR page?
- *Action on Failure*: Demote source to Tier 4 if it lacks independent auditor certification.

### 1.3 Structured Business Databases (Tier 3)
- [ ] **Data Provider Reputation**: Is the data provider established (e.g., ZoomInfo, Dun & Bradstreet)?
- [ ] **Update Timestamp**: Does the record contain a "last updated" or "last crawled" timestamp within the last 180 days?
- [ ] **Entity Alignment**: Does the company name and address in the record match the resolved entity record?
- *Action on Failure*: Reduce source reliability score ($R_s$) by $0.20$.

### 1.4 Company Public-Facing Sites (Tier 4)
- [ ] **Domain Check**: Does the document URL resolve to the canonical company domain?
- [ ] **Purpose Analysis**: Is the page an educational resource, documentation, or a pure marketing page?
- [ ] **Case Study Verification**: If a case study, is the customer named, and is the implementation date stated?
- *Action on Failure*: If pure marketing, cap the maximum admitted confidence at $0.50$ (Self-Reporting Cap).

### 1.5 Social & Community Sources (Tier 5)
- [ ] **Profile Veracity**: Does the author's profile show consistent employment history matching the target company?
- [ ] **Sentiment Calibration**: For Glassdoor/Reddit, is the post free of extreme emotional keywords that indicate individual bias (e.g., "worst company ever", "absolute disaster")?
- [ ] **Date Verification**: Is the post timestamped?
- *Action on Failure*: Admissibility is restricted to "Hypothesis generation only". Do not admit as fact.

---

## 2. Freshness Decay Functions

To prevent stale data from distorting current profiles, you must apply a mathematical decay multiplier ($D(t)$) to the source reliability score ($R_s$) based on the age ($t$) of the document in days:

$$D(t) = e^{-\gamma \cdot t}$$

Where $\gamma$ is the decay constant determined by the volatility of the target variable:

*   **High Volatility ($\gamma = 0.0077$)**: For hiring trends, open roles, executive tenures, and buying signals. (Half-life $\approx 90$ days).
*   **Medium Volatility ($\gamma = 0.0038$)**: For technology stack components, vendor partnerships, and pricing models. (Half-life $\approx 180$ days).
*   **Low Volatility ($\gamma = 0.0019$)**: For financial metrics, physical footprint, and legal registrations. (Half-life $\approx 360$ days).

### Decay Multiplication Rule:
Let $R_0$ be the initial reliability of the source tier (e.g., $1.0$ for Tier 1, $0.8$ for Tier 2). The decayed source reliability is:

$$R_s = R_0 \cdot D(t)$$

If $R_s < 0.30$, the source is classified as **Stale** and is excluded from factual analysis.

---

## 3. Bias Detection Algorithm

You must run this algorithm on all Tier 4 (Company blogs/marketing) and Tier 5 (Social media/analyst reviews) sources to detect bias.

### Bias Assessment Matrix:

| Bias Indicator | Detection Mechanism | Analytical Action |
|---|---|---|
| **Vendor-Sponsored Content** | Page contains disclosures like "sponsored by", "partner content", or "written in collaboration with [Vendor]". | Cap confidence at $0.40$. Exclude comparative claims. |
| **Executive PR Spin** | Document uses high-frequency buzzwords (e.g., "hyper-growth", "revolutionary") while omitting standard operational disclosures or details. | Demote source tier by 1. Extract only raw names and dates. |
| **Glassdoor Outliers** | A single review shows 1-star or 5-star ratings with extreme vocabulary while contradicting the 12-month average. | Flag as outlier. Exclude from synthesis unless corroborated by at least 3 independent reviews. |
| **Analyst Pay-to-Play** | Analyst report contains a disclosure showing the target company is a paying client of the analyst firm. | Exclude vendor rankings; extract only structural tech data. |

---

## 4. Admissibility and Outcomes

Based on the checklists, decay calculations, and bias checks, assign one of the following states to the evidence:

1.  **Accepted**: Admitted directly into the structured facts database.
2.  **Accepted with Caveats**: Admitted, but carrying a confidence penalty ($P_{\text{caveat}} = -0.15$) due to moderate recency decay or potential bias.
3.  **Hypothesis Only**: Admissible only as a proxy indicator for pain inference or to generate search vectors. Cannot support factual claims in the final report.
4.  **Rejected**: Deleted from the active dataset. Log the reason for audit.