# Decision Tree Specification

## Version: 2.0.0

## Purpose

This document contains the deterministic decision trees used by the Company Intelligence Expert. Each tree maps input variables to outputs through conditional logic (IF/THEN/ELSE blocks) to remove ambiguity and ensure consistent decision-making.

---

## Decision Trees Directory

### DT-001: Source Selection

- **Objective**: Determine which source to query and in what order to resolve a specific research variable.
- **Inputs**: Target variable (e.g., "Annual Revenue"), active source database, source tier list.
- **Outputs**: Target source, query parameter.
- **Decision Logic**:
  ```
  IF Target Variable is represented in Tier 1 (SEC Filings, Court Registries) AND data is within temporal bounds:
      USE Tier 1 Source.
  ELSE IF Target Variable is represented in Tier 2 (Earnings transcripts, Investor presentations):
      USE Tier 2 Source.
  ELSE IF Target Variable is represented in Tier 3 (D&B, ZoomInfo, job boards) AND Tier 1/2 are unavailable:
      USE Tier 3 Source.
  ELSE IF Target Variable is represented in Tier 4 (Company blogs, press releases):
      USE Tier 4 Source AND flag as "Self-Reported".
  ELSE:
      USE Tier 5 (Social media, forums) AND initiate corroboration loop.
  ```
- **Failure Mode & Recovery**: If selected source fails to return data, mark source as "failed", update query budget, and select the next available source in the tier hierarchy.

---

### DT-002: Evidence Sufficiency

- **Objective**: Determine whether sufficient evidence has been gathered to conclude research on a company.
- **Inputs**: Scoped objectives checklist, collected facts, query budget status.
- **Outputs**: Boolean (True = Stop and report; False = Continue research).
- **Decision Logic**:
  ```
  IF all mandatory research variables have at least one validated evidence item with Confidence >= 0.75:
      RETURN True (Sufficiency Met).
  ELSE IF Query Budget is Exhausted (max query limit reached):
      RETURN True (Stop Research, report with documented evidence gaps).
  ELSE:
      RETURN False (Continue Research).
  ```
- **Failure Mode & Recovery**: If sufficiency is False and budget is exhausted, do not continue querying. Compile the profile but reduce the overall confidence rating and explicitly list the missing variables in the gaps section.

---

### DT-003: Confidence Assignment

- **Objective**: Assign an initial confidence score to a claim.
- **Inputs**: Source Tier, Corroboration status, Recency.
- **Outputs**: Confidence Score (0.0 to 1.0).
- **Decision Logic**:
  ```
  IF Claim is backed by a single Tier 1 source AND data is <12 months old:
      SET Confidence = 0.80.
  ELSE IF Claim is backed by a single Tier 2 source:
      SET Confidence = 0.70.
  ELSE IF Claim is backed by Tier 3/4/5 source with no independent corroboration:
      SET Confidence = 0.40.
      
  IF Claim has independent corroboration (different publisher and domain):
      ADD Corroboration Bonus (+0.10 per independent source, capped at +0.20).
      
  IF Claim is older than temporal thresholds:
      SUBTRACT Stale Penalty (-0.15).
      
  IF Claim has unresolved contradictions:
      SET Confidence = 0.50 (Cap).
      
  RETURN Confidence (Bound between 0.0 and 1.0).
  ```
- **Failure Mode & Recovery**: If calculated confidence is <0.40 for a critical profile claim, flag the claim as "Hypothesis Only" and do not present it as a fact.

---

### DT-004: Conflict Resolution

- **Objective**: Resolve conflicting evidence from multiple sources.
- **Inputs**: Fact A (Source A, Tier A, Timestamp A), Fact B (Source B, Tier B, Timestamp B).
- **Outputs**: Canonical Fact, updated Confidence Score.
- **Decision Logic**:
  ```
  IF Tier A > Tier B (Tier A is higher authority, e.g., Tier 1 vs Tier 3):
      RESOLVE to Fact A.
      SET Confidence = Confidence(Fact A) - 0.10 (Contradiction Penalty).
  ELSE IF Tier A == Tier B:
      IF Timestamp A > Timestamp B (Fact A is more recent):
          RESOLVE to Fact A.
          SET Confidence = Confidence(Fact A) - 0.15.
      ELSE IF Timestamp A == Timestamp B:
          RESOLVE to "Unresolved Contradiction".
          SET Confidence = 0.30.
          PRESERVE both Fact A and Fact B in report.
  ```
- **Failure Mode & Recovery**: If conflicts cannot be resolved, elevate the contradiction to the "Gaps and Limitations" section of the output schema.

---

### DT-005: Scope Boundary

- **Objective**: Determine whether to pursue a discovered lead or variable that falls outside the initial research scope.
- **Inputs**: Discovered finding, Scoped objectives, Impact score (1-5).
- **Outputs**: Action (Ignore, Note, or Expand).
- **Decision Logic**:
  ```
  IF Discovered finding directly contradicts a core assumption of the primary objective:
      RETURN Expand (requires immediate scope adjustment and investigation).
  ELSE IF Discovered finding has Impact score >= 4 on the target company's business model:
      RETURN Note (document briefly in the appendix).
  ELSE:
      RETURN Ignore (do not expend query budget).
  ```
- **Failure Mode & Recovery**: If scope expansion is triggered, log the decision and deduct the expansion cost from the remaining query budget.

---

### DT-006: Output Completeness

- **Objective**: Validate draft deliverables against the output contract.
- **Inputs**: Draft JSON/Markdown report, [OUTPUT_SCHEMA.md](file:///Users/george/companyintelligence/OUTPUT_SCHEMA.md).
- **Outputs**: Pass/Fail status.
- **Decision Logic**:
  ```
  IF any required field in OUTPUT_SCHEMA.md is missing or null:
      RETURN Fail (Missing Fields).
  IF any factual claim in the report lacks a corresponding entry in the evidence_index:
      RETURN Fail (Unreferenced Claims).
  IF the overall profile confidence score is missing:
      RETURN Fail (No Confidence Score).
  IF all checks pass:
      RETURN Pass.
  ```
- **Failure Mode & Recovery**: If Fail status is returned, locate the missing index or field, extract the missing data from research history, and re-render. Do not transmit.

---

### DT-007: Entity Disambiguation

- **Objective**: Resolve name collisions and select the correct target company profile.
- **Inputs**: Query target name, list of matching registry records.
- **Outputs**: canonical Resolved Entity Record.
- **Decision Logic**:
  ```
  FOR each matching record:
      IF website domain matches the company's public domain:
          MATCH score = 1.0.
      ELSE IF HQ address country matches query parameters AND industry classification matches:
          MATCH score = 0.8.
      ELSE:
          MATCH score = 0.2.
          
  IF highest MATCH score >= 0.8:
      SELECT matching record.
      RETURN Resolved Entity Record.
  ELSE:
      RETURN Ambiguous (Flag for clarification).
  ```
- **Failure Mode & Recovery**: If all matches score <0.8, execute a targeted web search for the target company name + "incorporation location" to gather more context, then rerun disambiguation.

---

### DT-008: Signal Classification

- **Objective**: Classify a company behavior or event into a signal class.
- **Inputs**: Fact (source, category, description).
- **Outputs**: Signal Class (Buying Signal, Red Flag, Neutral).
- **Decision Logic**:
  ```
  IF Fact matches any ID in BUYING_SIGNALS.md (e.g. executive hire, database migration):
      RETURN Buying Signal.
  ELSE IF Fact matches any ID in RED_FLAGS.md (e.g. revenue decline, security breach):
      RETURN Red Flag.
  ELSE:
      RETURN Neutral.
  ```
- **Failure Mode & Recovery**: If a signal matches both taxonomies (e.g., an acquisition that expands market share but increases debt), classify as both a Buying Signal and a Red Flag, and evaluate their impacts independently.

---

### DT-009: Pain Point Inference

- **Objective**: Map observable operational proxy indicators to latent business pain.
- **Inputs**: Set of observable indicators, Pain Inference Table.
- **Outputs**: Inferred Pain, Confidence Score.
- **Decision Logic**:
  ```
  IF Indicators match the required proxies in Pain Inference Table:
      RETRIEVE Inferred Pain.
      SET Base Confidence = default table confidence.
      IF indicators are backed by corroboration:
          ADD Corroboration Bonus (+0.15).
      RETURN Inferred Pain AND Confidence.
  ELSE:
      RETURN "No Pain Inferred" AND Confidence = 0.0.
  ```
- **Failure Mode & Recovery**: If pain is inferred with confidence <0.50, reclassify it as a "Hypothesis" and do not include it in the executive summary.

---

### DT-010: Opportunity Qualification

- **Objective**: Qualify an account and generate a strategic outreach recommendation.
- **Inputs**: Buying signals, risk matrix, confidence scores.
- **Outputs**: Strategic Recommendation (Pursue, Monitor, or Disqualify) and urgency score.
- **Decision Logic**:
  ```
  IF (Active Buying Signal exists with Confidence >= 0.70)
     AND (Critical Red Flags count == 0)
     AND (Overall Opportunity Confidence >= 0.75):
      RETURN Pursue (High Urgency).
      
  ELSE IF (Emerging Buying Signal exists) OR (High Red Flags exist but carry clear mitigations):
      RETURN Monitor (Medium Urgency).
      
  ELSE IF (Cumulative Risk Score exceeds the threshold) OR (Overall Opportunity Confidence < 0.50):
      RETURN Disqualify (Low Urgency).
  ```
- **Failure Mode & Recovery**: If qualification results in "Disqualify" but the customer is a strategic target, log the disqualification reasons and suggest specific validation queries to re-evaluate the account.