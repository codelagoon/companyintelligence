# Red Flags Specification

## Version: 2.0.0

## Purpose

This document catalogs the adverse signals used by the Company Intelligence Expert to evaluate corporate stability, partnership risk, fit, and implementation obstacles.

---

## Red Flag Schema

Every red flag must follow this structure:
- **Flag ID**: F-[CATEGORY]-[NNN]
- **Category**: Domain classification.
- **Description**: What the risk indicates.
- **Detection Method**: Query parameters and source locations used to find it.
- **Severity Level**: Critical, High, Medium, Low.
- **Recommended Action**: Steps for the analysis engine to take if detected.
- **False Positive Indicators**: Conditions under which this signal is invalid or neutral.

---

## Red Flags Catalog

### 1. Financial Red Flags (F-FIN)

#### F-FIN-001: Net Income vs. CFO Divergence
- **Category**: Financial
- **Description**: positive net income co-occurs with negative cash flow from operations (CFO).
- **Detection Method**: Compare balance sheets and cash flow statements in SEC filings (based on H-FIN-003).
- **Severity Level**: Critical.
- **Recommended Action**: Cap the financial health rating at "Critical Risk". Downgrade opportunity qualification to "Disqualify" unless a strong capital bridge is documented.
- **False Positive Indicators**: Early-stage companies capitalizing R&D costs under specific regional accounting rules.

#### F-FIN-002: Days Sales Outstanding (DSO) Increase
- **Category**: Financial
- **Description**: DSO rises >15% year-over-year (based on H-FIN-001).
- **Detection Method**: Compute DSO from quarterly 10-Q report filings.
- **Severity Level**: High.
- **Recommended Action**: Check accounts receivable line items for customer concentrations.
- **False Positive Indicators**: Seasonal retail fluctuations in Q4.

#### F-FIN-003: Declining Deferred Revenue Growth
- **Category**: Financial
- **Description**: Deferred revenue grows slower than recognized revenue (based on H-FIN-002).
- **Detection Method**: SEC balance sheet analytics.
- **Severity Level**: Medium.
- **Recommended Action**: Flag as a leading indicator of revenue deceleration.
- **False Positive Indicators**: Transition from annual upfront contracts to monthly pay-as-you-go billing.

---

### 2. Operational & Leadership (F-OPS)

#### F-OPS-001: Short-Tenured Executive Departure
- **Category**: Operational
- **Description**: VP or C-level officer departs within 12 months of joining.
- **Detection Method**: LinkedIn profiles, executive change press releases, SEC 8-K filings.
- **Severity Level**: High.
- **Recommended Action**: Pause active sales campaigns in that business unit. Re-verify the Economic Buyer and decision criteria.
- **False Positive Indicators**: Executive leaving due to acquisition or health reasons.

#### F-OPS-002: Engineering Headcount Contraction
- **Category**: Operational
- **Description**: Over 15% reduction in engineering/product headcount over a 6-month period.
- **Detection Method**: LinkedIn Department Insights, layoff trackers, local employment registry logs.
- **Severity Level**: High.
- **Recommended Action**: Flag potential system stability risks and project delays. Focus pitches on cost reduction and automation rather than feature expansion.
- **False Positive Indicators**: Outsourcing engineering to a dedicated subsidiary under a different legal entity name.

#### F-OPS-003: Mass Layoff Announcements
- **Category**: Operational
- **Description**: Public announcement of workforce reductions exceeding 10% of total headcount.
- **Detection Method**: News feeds, WARN Act notices (Tier 1), press releases.
- **Severity Level**: Critical.
- **Recommended Action**: Demote opportunity recommendation to "Disqualify" or "Monitor" immediately. Check for active vendor consolidation initiatives.
- **False Positive Indicators**: Strategic restructuring where non-core business divisions are divested while core units continue to expand.

---

### 3. Technical & Infrastructure (F-TECH)

#### F-TECH-001: Legacy Software Dependencies
- **Category**: Technical
- **Description**: Target continues to run deprecated software versions (e.g., Python 2.7, Windows Server 2008) in customer-facing infrastructure.
- **Detection Method**: Technographic profiling databases, active job board searches seeking support for obsolete technologies.
- **Severity Level**: High.
- **Recommended Action**: Pitch migration support, modernization services, or legacy isolation security software.
- **False Positive Indicators**: Legacy technology used exclusively in isolated internal test environments.

#### F-TECH-002: Chronic API Performance Degradation
- **Category**: Technical
- **Description**: Target's public API status page shows recurring outages or response times exceeding 1000ms.
- **Detection Method**: Monitoring status page feeds, developer forums.
- **Severity Level**: Medium.
- **Recommended Action**: Pitch performance monitoring, SRE automation, or cache acceleration systems.
- **False Positive Indicators**: Planned maintenance windows clearly communicated in advance.

---

### 4. Compliance & Legal (F-COMP)

#### F-COMP-001: Regulatory Investigation or Fine Disclosure Lag
- **Category**: Compliance
- **Description**: Delayed disclosure of material regulatory or financial penalties (based on H-REG-001).
- **Detection Method**: SEC filings compared with news media.
- **Severity Level**: High.
- **Recommended Action**: Audit the legal liabilities section of the latest 10-Q.
- **False Positive Indicators**: Fines that fall below the material disclosure threshold ($50K for smaller reporting companies).