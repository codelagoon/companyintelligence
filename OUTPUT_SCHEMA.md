# Output Schema Specification

## Version: 2.0.0

## Purpose

This document defines the structured output formats produced by the Company Intelligence Expert. All deliverables must conform to the specifications below.

---

## 1. Required Sections (Markdown Contract)

Every Markdown report must contain the following headers in order:

1.  **# Executive Summary & Strategic Recommendation**: The primary recommendation (Pursue, Monitor, Disqualify), overall opportunity confidence score ($C_{\text{opp}}$), and high-level reasoning.
2.  **## Company Identity**: Legal name, primary domain, CIK code, headquarters, and Global Ultimate Owner (GUO).
3.  **## Business Model & Revenue Engine**: Analysis of how the target creates, packages, and captures value.
4.  **## Technology Stack & Infrastructure**: Active tech footprint, observed platforms, and migration indicators.
5.  **## Buying Committee Map**: Identified Economic Buyers, Technical Evaluators, and potential Champions.
6.  **## Active Buying Signals**: List of detected signals with their IDs, confidence, and urgency ratings.
7.  **## Risk and Red Flag Assessment**: Identified risks, severity ratings, and mitigations.
8.  **## Gaps and Unknowns**: explicit gaps, unverified claims, and missing research variables.
9.  **## Next Actions**: Actionable, prioritized steps for sales or engagement teams.
10. **## Evidence Index**: Source citations mapped to citation keys.

---

## 2. Executable JSON Schema Contract

The following JSON Schema (Draft-07) defines the output format for automated validations:

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "CompanyIntelligenceProfile",
  "type": "object",
  "properties": {
    "metadata": {
      "type": "object",
      "properties": {
        "entity_name": { "type": "string" },
        "canonical_domain": { "type": "string" },
        "research_timestamp": { "type": "string", "format": "date-time" },
        "overall_opportunity_confidence": { "type": "number", "minimum": 0.0, "maximum": 1.0 },
        "query_budget_consumed": { "type": "integer" }
      },
      "required": ["entity_name", "canonical_domain", "research_timestamp", "overall_opportunity_confidence"]
    },
    "recommendation": {
      "type": "object",
      "properties": {
        "status": { "enum": ["Pursue", "Monitor", "Disqualify"] },
        "urgency": { "enum": ["Immediate", "High", "Medium", "Low"] },
        "rationale": { "type": "string" }
      },
      "required": ["status", "urgency", "rationale"]
    },
    "company_identity": {
      "type": "object",
      "properties": {
        "legal_name": { "type": "string" },
        "global_ultimate_owner": { "type": "string" },
        "headquarters_address": { "type": "string" },
        "cik": { "type": "string" }
      },
      "required": ["legal_name", "global_ultimate_owner"]
    },
    "business_model": {
      "type": "object",
      "properties": {
        "revenue_engine": { "type": "string" },
        "target_segments": { "type": "array", "items": { "type": "string" } },
        "cost_drivers": { "type": "array", "items": { "type": "string" } }
      },
      "required": ["revenue_engine", "target_segments"]
    },
    "technology_stack": {
      "type": "object",
      "properties": {
        "cloud_providers": { "type": "array", "items": { "type": "string" } },
        "data_infrastructure": { "type": "array", "items": { "type": "string" } },
        "core_tooling": { "type": "array", "items": { "type": "string" } }
      },
      "required": ["cloud_providers", "data_infrastructure"]
    },
    "buying_signals": {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "signal_id": { "type": "string" },
          "description": { "type": "string" },
          "confidence": { "type": "number", "minimum": 0.0, "maximum": 1.0 },
          "urgency": { "enum": ["Immediate", "High", "Medium", "Low"] },
          "citation_keys": { "type": "array", "items": { "type": "string" } }
        },
        "required": ["signal_id", "description", "confidence", "urgency", "citation_keys"]
      }
    },
    "red_flags": {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "flag_id": { "type": "string" },
          "severity": { "enum": ["Critical", "High", "Medium", "Low"] },
          "description": { "type": "string" },
          "mitigation": { "type": "string" },
          "citation_keys": { "type": "array", "items": { "type": "string" } }
        },
        "required": ["flag_id", "severity", "description", "citation_keys"]
      }
    },
    "gaps_and_unknowns": {
      "type": "array",
      "items": { "type": "string" }
    },
    "next_actions": {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "priority": { "type": "integer" },
          "action": { "type": "string" },
          "target_stakeholder": { "type": "string" }
        },
        "required": ["priority", "action"]
      }
    },
    "evidence_index": {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "key": { "type": "string" },
          "url": { "type": "string" },
          "publish_date": { "type": "string", "format": "date" },
          "source_tier": { "type": "integer", "minimum": 1, "maximum": 5 }
        },
        "required": ["key", "url", "publish_date", "source_tier"]
      }
    }
  },
  "required": [
    "metadata",
    "recommendation",
    "company_identity",
    "business_model",
    "technology_stack",
    "buying_signals",
    "red_flags",
    "gaps_and_unknowns",
    "next_actions",
    "evidence_index"
  ]
}
```

---

## 3. Validation Rules

1.  **Strict Reference Integrity**: Every entry in `buying_signals.citation_keys` and `red_flags.citation_keys` must match a corresponding `key` in the `evidence_index`. Unreferenced citations or broken keys will fail validation.
2.  **No Placeholders**: Text fields must not contain default filler strings (e.g., "TBD", "N/A", "Unknown"). If a value is unknown, it must be listed as an array item in the `gaps_and_unknowns` section, and the corresponding property value must be omitted or represented as an empty structure.
3.  **Overall Confidence Verification**: `metadata.overall_opportunity_confidence` must equal the exact value computed using the calibration formula in [CONFIDENCE_ENGINE.md](file:///Users/george/companyintelligence/CONFIDENCE_ENGINE.md#3-opportunity-calibration-algorithm).