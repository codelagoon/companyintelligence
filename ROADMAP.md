# Repository Roadmap

## Version: 1.0.0

---

This document outlines the strategic engineering objectives for **Hermes Company Intelligence** Version 2.0 and beyond.

## V1.1 — Automated Validation & CI/CD Integration
- **Executable Schema Validation**: Implement a CI pipeline that runs output payload checks against the JSON Schema in [OUTPUT_SCHEMA.md](file:///Users/george/companyintelligence/OUTPUT_SCHEMA.md).
- **Broken Link Checker**: Integrate automated pre-commit hooks to verify that all absolute file links (`file:///Users/george/companyintelligence/...`) resolve correctly.
- **Trigger Alignment Checks**: Create automated linting rules to verify that skill descriptions under `/skills/` are unique and optimize router dispatching.

## V2.0 — Reference Execution SDK & Sandboxing
- **Python Execution Engine**: Develop a lightweight, reference implementation SDK that imports the markdown decision trees, thinking policies, and heuristics, executing them locally via mock scraper APIs.
- **Mock Scenarios Harness**: Author a compliance harness containing 10 static company portfolios (with conflicting statements and decayed source timestamps) to evaluate agent compliance against the confidence formulas.
- **Target Rate-Limiting Engine**: Transition the hardcoded tool rules in [TOOLS.md](file:///Users/george/companyintelligence/TOOLS.md) to a dynamic, domain-aware back-off calculator.

## V2.5 — Regulated Industry Specializations
- **Healthcare Stack Profiling**: Expand [SOURCE_VALIDATION.md](file:///Users/george/companyintelligence/SOURCE_VALIDATION.md) and [RED_FLAGS.md](file:///Users/george/companyintelligence/RED_FLAGS.md) to support specialized healthcare compliance filters (HIPAA, HITECH, FDA clearances).
- **Public Sector & FedRAMP qualification**: Add dedicated heuristics for evaluating public sector software sales viability, incorporating FedRAMP status, government contract registries, and security clearances.
