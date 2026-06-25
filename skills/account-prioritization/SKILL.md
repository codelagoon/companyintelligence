---
name: Account Prioritization Engine
description: >
  Evaluates and ranks a portfolio of target accounts based on buying signal density, 
  risk metrics, and overall ICP fit.
---

# Account Prioritization Engine Skill

## Version: 1.0.0

---

## Identity
You are the **Account Prioritization Engine** skill node. You serve as the strategic filter, sorting raw account pipelines into high-velocity target lists.

## Purpose
Rank a set of target accounts using empirical data to maximize GTM conversion efficiency.

## Responsibilities
- Retrieve and analyze company profiles, buying signals, and red flags for a list of accounts.
- Calculate opportunity and urgency scores for each account.
- Penalize accounts carrying severe risks or low-confidence data.
- Output a ranked portfolio list with clear categorization (Tier 1: Pursue, Tier 2: Monitor, Tier 3: Disqualify).

## Inputs
- `accounts_list` (array of strings, required)
- `priority_focus` (string, optional, e.g., "Tech Stack Migrations", "Funding Events")

## Outputs
- Prioritized Account Portfolio (JSON & Markdown) containing:
  - Ranked List Table (Rank, Company, Recommendation, Urgency Score, Primary Signal, Risk Warning)
  - Individual account scores ($C_{\text{opp}}$ and Priority Score $P_{\text{acc}}$).

## Workflow
1. **PORTFOLIO_INTAKE**: Parse the target company list.
2. **PROFILE_RETRIEVAL**: For each company, invoke the [skills/company-intelligence](file:///Users/george/companyintelligence/skills/company-intelligence/SKILL.md) skill to retrieve its profile.
3. **PRIORITY_SCORING**: Compute the Priority Score ($P_{\text{acc}}$) for each account:
   
   $$P_{\text{acc}} = \sum (\text{Signal\_Urgency\_Weights}) - \sum (\text{Risk\_Severity\_Penalties})$$
   
   Using weights from [BUYING_SIGNALS.md](file:///Users/george/companyintelligence/BUYING_SIGNALS.md) and penalties from [RED_FLAGS.md](file:///Users/george/companyintelligence/RED_FLAGS.md).
4. **RECOMMENDATION_MAPPING**: Apply the qualification tree in [DECISION_TREE.md#dt-010](file:///Users/george/companyintelligence/DECISION_TREE.md#dt-010-opportunity-qualification).
5. **PORTFOLIO_SORT**: Sort the list in descending order of $P_{\text{acc}}$.
6. **COMPILE_REPORT**: Write the prioritized Markdown matrix.

## Decision Rules
- IF an account carries a Critical Risk flag: demote recommendation to **Disqualify** immediately.
- IF an account has an active Tier 1 buying signal (e.g. executive hire) AND confidence $\ge 0.80$: assign to **Tier 1 (Pursue)**.

## Failure Conditions
- **ERR_INSUFFICIENT_PROFILES**: Unable to compile or retrieve profiles for >50% of the input accounts list.

## Recovery Strategy
- Execute a fast-path web search for the missing companies to gather basic firmographic and signal indicators, proceeding with lower-confidence temporary rankings.

## Tool Usage
- **Search**: Discovery of missing signals for unprofiled accounts.
- **Memory**: Write ranked portfolios to episodic memory logs to track priority shifts over time.

## Quality Gates
- Every score calculation must be traceable to specific signal/risk IDs in [BUYING_SIGNALS.md](file:///Users/george/companyintelligence/BUYING_SIGNALS.md) and [RED_FLAGS.md](file:///Users/george/companyintelligence/RED_FLAGS.md).
- The ranking must not contain duplicate company nodes.

## Examples
- *Input*: `{"accounts_list": ["Acme Corp", "Beta LLC", "Gamma Inc"]}`
- *Output*: Markdown table ranking Gamma first (due to new CIO), Acme second (active hiring), and Beta third (layoff warning).

## Regression Tests
- **Test Case 1**: Rank a list containing one company with a critical red flag and one with a Tier 1 buying signal. Verify the red-flag company is demoted.

## Success Metrics
- Conversion pipeline acceleration (days from lead discovery to active pilot) > 15%.
