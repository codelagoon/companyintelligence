# Contributing to Hermes Company Intelligence

Thank you for contributing to the **Hermes Company Intelligence** project. To maintain the rigorous, production-grade standards required for regulated enterprise GTM systems, all contributions must comply with these guidelines.

---

## 1. Development Standards

This repository functions as an engineering specification. Follow these rules when editing or creating files:

- **No Descriptive Fluff**: Avoid generic marketing or conversational AI language. Write concise, technical, and implementation-first text.
- **Strict Single Responsibility**: Every file owns a single area. Do not duplicate or redefine concepts. Refer to [ARCHITECTURE.md](file:///Users/george/companyintelligence/ARCHITECTURE.md) to locate the canonical owner.
- **Reference Integrity**: Always use absolute file links (`file:///Users/george/companyintelligence/FILENAME.md`) when linking documents.

---

## 2. Proposing a New Heuristic

When submitting a new heuristic to [HEURISTICS.md](file:///Users/george/companyintelligence/HEURISTICS.md):
1.  **Format Compliance**: The heuristic must follow the exact 10-field schema (ID, Category, Statement, Rationale, Evidence Pattern, Counterexamples, Failure Modes, Confidence, Applicability, Validation History).
2.  **Empirical Grounding**: Do not invent heuristics. The rule must be grounded in verified enterprise sales, forensic finance, procurement, or regulatory practices.
3.  **Unique Identifier**: Use the next sequential ID in the target category (e.g. `H-FIN-004`).

---

## 3. Creating a New Hermes Skill

When proposing a new skill under `/skills/`:
1.  **Directory Structure**: Create a subfolder `/skills/[skill-name]/` containing a `SKILL.md` file.
2.  **YAML Frontmatter**: The file must begin with trigger-matching metadata containing `name` and `description` (YAML block).
3.  **Under 500 Lines**: Keep the main instruction file under 500 lines for token efficiency. Put extensive references under `/skills/[skill-name]/references/`.
4.  **Composition Rules**: Ensure the skill uses the canonical root documents (e.g., `CONFIDENCE_ENGINE.md`, `SOURCE_VALIDATION.md`) and cascades research calls to [skills/evidence-acquisition](file:///Users/george/companyintelligence/skills/evidence-acquisition/SKILL.md).

---

## 4. Pull Request Requirements

Every pull request must:
- [ ] Pass the syntax audit (valid Markdown and JSON Schema).
- [ ] Maintain consistent terminology (e.g., matching the 15 states in [STATE_MACHINE.md](file:///Users/george/companyintelligence/STATE_MACHINE.md) and 5 tiers in [SOURCE_HIERARCHY.md](file:///Users/george/companyintelligence/SOURCE_HIERARCHY.md)).
- [ ] Include updated version numbers in modified file headers.
- [ ] Provide a worked example or regression test in the PR description.
