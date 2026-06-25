# Confidence Engine Specification

## Version: 2.0.0

## Purpose

This document defines the mathematical models, scoring algorithms, and calibration procedures used by the Company Intelligence Expert to calculate confidence scores for claims, findings, and recommendations. 

---

## 1. Mathematical Scoring Models

The confidence score ($C$) for any individual claim is a calculated value bounded between $0.0$ and $1.0$. It is computed using the following formula:

$$C = \text{MIN}\left(1.0, \text{MAX}\left(0.0, B + K - P_t - P_c\right)\right)$$

Where:
*   $B$ = Base Score (determined by the highest-tier source).
*   $K$ = Corroboration Bonus.
*   $P_t$ = Recency Decay Penalty.
*   $P_c$ = Contradiction Penalty.

---

## 2. Parameter Definitions and Weights

### 2.1 Base Score ($B$)
The base score is determined by the highest-tier source supporting the claim, as defined in [SOURCE_HIERARCHY.md](file:///Users/george/companyintelligence/SOURCE_HIERARCHY.md):

| Source Tier | Description | Base Score ($B$) |
|---|---|---|
| **Tier 1** | Regulatory filings, court records, government registries | $0.80$ |
| **Tier 2** | Audited financial reports, earnings calls, official investor decks | $0.70$ |
| **Tier 3** | Firmographic databases, job posting aggregators, analyst reports | $0.50$ |
| **Tier 4** | Company blogs, press releases, self-reported case studies | $0.40$ |
| **Tier 5** | Social media, forums, Glassdoor, community discussions | $0.20$ |

---

### 2.2 Corroboration Bonus ($K$)
Corroboration requires independent sources (different publishers, authors, or domains) confirming the same fact.

*   **Tier 1 or Tier 2 Corroboration**: $+0.10$ per independent source.
*   **Tier 3 or Tier 4 Corroboration**: $+0.05$ per independent source.
*   **Tier 5 Corroboration**: $+0.02$ per independent source.
*   **Cap**: The cumulative corroboration bonus $K$ cannot exceed $+0.20$.

---

### 2.3 Recency Decay Penalty ($P_t$)
Information value decays based on the variable class:

| Variable Class | Fresh Range (No Penalty) | Stale Range ($P_t = 0.10$) | Stale Limit ($P_t = 0.25$) |
|---|---|---|---|
| **Hiring / Personnel** | $< 90$ days | $90 - 180$ days | $> 180$ days |
| **Technology Stack** | $< 180$ days | $180 - 360$ days | $> 360$ days |
| **Financial / Margins** | $< 90$ days | $90 - 360$ days | $> 360$ days |
| **Corporate Registration** | $< 365$ days | $365 - 1095$ days | $> 1095$ days |

---

### 2.4 Contradiction Penalty ($P_c$)
When conflicting evidence is detected during synthesis (Step 8 of the workflow):

*   **Lower-Tier Contradiction**: If a lower-tier source contradicts the claim: $P_c = 0.15$.
*   **Same-Tier Contradiction**: If a source of the same tier contradicts the claim: $P_c = 0.30$.
*   **Cap Rule**: If there is any unresolved contradiction, the final score $C$ is capped at $0.60$, regardless of corroboration or base score.

---

### 2.5 Self-Reporting Cap
If a claim is solely backed by company self-reported marketing documents (Tier 4 or Tier 2 marketing decks) and has zero independent third-party corroboration, the final confidence score $C$ is capped at $0.50$.

---

## 3. Opportunity Calibration Algorithm

To qualify an account for engagement, the overall opportunity confidence score ($C_{\text{opp}}$) is calculated as a weighted average of its core components:

$$C_{\text{opp}} = w_1 C_{\text{identity}} + w_2 C_{\text{model}} + w_3 C_{\text{financial}} + w_4 C_{\text{tech}} + w_5 C_{\text{signals}} + w_6 C_{\text{risks}}$$

Where the weights ($w_i$) are defined as:

*   $w_1$ (Entity Identity) = $0.15$
*   $w_2$ (Business Model) = $0.20$
*   $w_3$ (Financial Health) = $0.20$
*   $w_4$ (Tech Stack) = $0.15$
*   $w_5$ (Buying Signals) = $0.15$
*   $w_6$ (Risks / Red Flags) = $0.15$

---

## 4. Worked Calibration Examples

### Example 1: High-Confidence Technology Usage Claim
- **Claim**: Company X uses Snowflake as their primary data warehouse.
- **Evidence Gathered**:
  1. A case study on Snowflake's website (Tier 4) published 45 days ago.
  2. A job posting on Company X's website seeking a "Snowflake Data Engineer" (Tier 3) published 10 days ago.
  3. A LinkedIn profile of a Senior Data Engineer at Company X (Tier 5) listing Snowflake as their primary stack, updated 30 days ago.
- **Calculations**:
  - Highest source tier = Tier 3 (Job posting). Base $B = 0.50$.
  - Corroboration:
    - Snowflake case study (Tier 4): $+0.05$
    - LinkedIn profile (Tier 5): $+0.02$
    - Cumulative $K = 0.07$.
  - Recency: All data is fresh. $P_t = 0.0$.
  - Contradiction: None. $P_c = 0.0$.
  - **Raw Score**: $C = 0.50 + 0.07 = 0.57$.
  - *Wait!* The case study (Tier 4) is self-reported, but we have independent corroboration from the job board and LinkedIn, so the self-reporting cap does not apply.
  - **Final Score**: $0.57$ (Moderate).

---

### Example 2: Financial Health Contradiction
- **Claim**: Company Y has positive operating cash flow.
- **Evidence Gathered**:
  1. SEC 10-Q filing from 14 days ago (Tier 1) showing negative operating cash flow of -$5M.
  2. A press release from Company Y from 5 days ago (Tier 4) stating "Strong cash performance and growth".
- **Calculations**:
  - Highest source tier = Tier 1 (SEC filing). Base $B = 0.80$.
  - Corroboration: None. $K = 0.0$.
  - Recency: Fresh. $P_t = 0.0$.
  - Contradiction: Tier 4 press release contradicts the Tier 1 filing. Lower-Tier Contradiction penalty: $P_c = 0.15$.
  - **Raw Score**: $C = 0.80 - 0.15 = 0.65$.
  - *Conflict Resolution Rule*: Under [DECISION_TREE.md](file:///Users/george/companyintelligence/DECISION_TREE.md#dt-004-conflict-resolution), Tier 1 prevails over Tier 4. The claim "Company Y has positive cash flow" is rejected, and the canonical fact is resolved to "Negative operating cash flow".
  - **Final Score for resolved fact (Negative Cash Flow)**: $0.80$ (High confidence in negative cash flow, based on SEC filing).

---

## 5. Edge Cases and Failure Modes

- **Edge Case 1: Private Company Lack of Tier 1/2 Financials**
  - *Problem*: Private companies do not file 10-Ks. Base score for financials is capped at Tier 3 or 4 ($0.50$ or $0.40$).
  - *Mitigation*: You must seek multiple independent Tier 3/4/5 indicators (e.g., funding press release + venture capital portfolio site listing + hiring rate). Apply the corroboration bonus to raise the score, but cap the maximum confidence at $0.70$ for private company financial claims.
  
- **Edge Case 2: Multi-Cloud Overlaps**
  - *Problem*: Finding job postings for AWS and Azure developers concurrently.
  - *Mitigation*: Do not assume multi-cloud. Under [HEURISTICS.md](file:///Users/george/companyintelligence/HEURISTICS.md#h-tech-002-multi-cloud-overlap-as-migration-phase), treat this as an active migration. Split the confidence score between the two clouds (e.g., AWS usage confidence = 0.50, Azure usage confidence = 0.50) until a primary landing zone is identified.