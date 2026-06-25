# Memory Management Specification

## Version: 2.0.0

## Purpose

This document defines the storage schemas, retrieval algorithms, and decay policies used by the Company Intelligence Expert to maintain persistent knowledge across research sessions.

---

## 1. Storage Schemas

The memory layer is structured into four hierarchical classes. Each class is represented by a JSON schema.

### 1.1 Permanent Memory Schema
Stores structural variables that do not change under ordinary operating conditions.
```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "PermanentMemory",
  "type": "object",
  "properties": {
    "canonical_name": { "type": "string" },
    "global_ultimate_owner": { "type": "string" },
    "incorporation_date": { "type": "string", "format": "date" },
    "jurisdiction": { "type": "string" },
    "cik_code": { "type": "string" },
    "historical_log": {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "field": { "type": "string" },
          "old_value": { "type": "string" },
          "new_value": { "type": "string" },
          "update_timestamp": { "type": "string", "format": "date-time" },
          "citation_url": { "type": "string" }
        }
      }
    }
  },
  "required": ["canonical_name", "incorporation_date"]
}
```

### 1.2 Semi-Permanent Memory Schema
Stores dynamic structural variables that evolve over multi-year cycles.
```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "SemiPermanentMemory",
  "type": "object",
  "properties": {
    "technologies": {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "name": { "type": "string" },
          "first_observed": { "type": "string", "format": "date" },
          "last_observed": { "type": "string", "format": "date" },
          "status": { "enum": ["active", "migrating", "deprecated"] },
          "source_url": { "type": "string" }
        }
      }
    },
    "buying_committee": {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "role": { "type": "string" },
          "name": { "type": "string" },
          "tenure_start": { "type": "string", "format": "date" },
          "last_verified": { "type": "string", "format": "date" }
        }
      }
    }
  },
  "required": ["technologies", "buying_committee"]
}
```

### 1.3 Short-Lived Memory Schema
Stores highly volatile tactical observations.
```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "ShortLivedMemory",
  "type": "object",
  "properties": {
    "active_buying_signals": {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "signal_id": { "type": "string" },
          "observation_date": { "type": "string", "format": "date" },
          "raw_text": { "type": "string" },
          "source_url": { "type": "string" }
        }
      }
    },
    "identified_risks": {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "flag_id": { "type": "string" },
          "severity": { "enum": ["Critical", "High", "Medium", "Low"] },
          "observation_date": { "type": "string", "format": "date" }
        }
      }
    }
  },
  "required": ["active_buying_signals", "identified_risks"]
}
```

---

## 2. Retrieval Algorithm and Relevance Weighting

When initializing research (S2 state), retrieve historical records using the following algorithm:

1.  **Select Target Keys**: Generate query vector based on the resolved canonical name and primary research variables.
2.  **Calculate Relevance Weight ($W_m$)**: For each retrieved record in the database, calculate its weight:

    $$W_m = \text{Match\_Score}(\text{canonical\_name}) \cdot e^{-\lambda_c \cdot t}$$

    Where:
    *   $\text{Match\_Score}$: Exact match on canonical name = $1.0$; phonetic match = $0.5$; alias/subsidiary match = $0.7$.
    *   $t$: Age of the memory record in days.
    *   $\lambda_c$: Class decay rate.
3.  **Filter**: Discard records where $W_m < 0.40$.

---

## 3. Decay Policies and Expiration

Memory records decay based on their class parameters:

| Memory Class | Decay Rate ($\lambda_c$) | Half-Life ($t_{1/2}$) | Expire Action |
|---|---|---|---|
| **Permanent** | $0.0$ | $\infty$ | Never expire. Update only on structural corporate registry changes. |
| **Semi-Permanent** | $0.0006$ | $\approx 1155$ days (3 years) | Flag for re-verification. Do not delete. |
| **Short-Lived** | $0.0057$ | $\approx 120$ days | Evict from memory store if $t > 180$ days. |

---

## 4. Conflict Resolution and Update Policy

When updating memory blocks upon task completion (S9 state):

```
IF Incoming Data Tier > Existing Memory Source Tier:
    OVERWRITE existing memory.
    WRITE old value to historical_log array.
ELSE IF Incoming Data Tier == Existing Memory Source Tier:
    IF Incoming Timestamp > Existing Memory Timestamp:
        OVERWRITE existing memory.
        WRITE old value to historical_log.
    ELSE:
        IGNORE incoming data.
ELSE:
    IGNORE incoming data (do not downgrade memory accuracy).
```