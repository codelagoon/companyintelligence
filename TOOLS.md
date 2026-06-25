# Tool Orchestration Specification

## Version: 2.0.0

## Purpose

This document defines the rules, constraints, rate-limit policies, and retry algorithms governing the orchestration of external tools by the Company Intelligence Expert.

---

## 1. Tool Execution Principles

1.  **Planning Priority**: No tool call may be executed before a structured search plan is initialized in the active execution context (Phase 3 of the workflow).
2.  **Least-Cost Retrieval**: Always prioritize the cheapest, most targeted tool call (e.g., fetching a specific URL) over a broad search query.
3.  **Deduplication**: Keep an active registry of executed queries in episodic memory. Do not run identical queries or fetch the same URL twice in a single session.
4.  **Provenance Preservation**: Every tool output must be stored alongside its source URL, execution timestamp, and HTTP response headers.

---

## 2. Resource Constraints and Rate Limits

To prevent rate-limiting (HTTP 429) and network timeouts, you must enforce the following boundaries:

-   **Query Budget**: Maximum of 20 total search queries per target company research session.
-   **Page Fetch Limit**: Maximum of 5 pages fetched per single query vector.
-   **Search Page Depth**: Do not parse beyond the first 3 pages of search results.
-   **Rate-Limiting Pause**: Introduce a minimum delay of 1000ms between consecutive HTTP requests to the same target domain, unless overridden by target-specific rate limits.
-   **Payload Truncation**: Truncate page fetches exceeding 100,000 characters to prevent context window overflow. Extract only text blocks matching target keywords.

---

## 3. Tool Failure and Recovery Algorithms

When a tool call encounters an error (network timeout, HTTP 4xx/5xx, DNS failure), run the following recovery algorithm:

```
Inputs: failed_tool, query_parameters, retry_count
Outputs: execution_status, retrieved_payload

Preconditions: tool execution failed or timed out.

1. Check retry_count:
   IF retry_count >= 1:
       Log permanent failure.
       Increment error_counter by 1.
       RETURN Failure (ERR_TOOL_EXHAUSTED).
       
2. Classify error type:
   IF error is transient (HTTP 429 Rate Limit, HTTP 503 Service Unavailable, or Timeout):
       Wait 2000ms.
       Retry identical query with retry_count = 1.
       RETURN outcome.
   ELSE IF error is permanent (HTTP 404 Not Found, HTTP 403 Forbidden):
       Locate alternative source class in SOURCE_HIERARCHY.md.
       IF alternative source exists:
           Format query_parameters for alternative source.
           Execute query with retry_count = 0.
           RETURN outcome.
       ELSE:
           Log evidence gap.
           RETURN Failure (ERR_NO_ALTERNATIVE_SOURCE).
```

---

## 4. Safety and Compliance Boundaries

1.  **No Authentication Bypass**: Do not attempt to bypass login pages, paywalls, or premium gates. If content is behind a gate, mark the source as unavailable and seek alternatives.
2.  **User-Agent Compliance**: Identify all requests using the canonical system user-agent header. Do not spoof user-agents to bypass scraping blockers.
3.  **Strict File Sandbox**: All file writes and reads must occur within the authorized project directory or temp workspace. Do not execute commands or read files outside the workspace bounds.