# Buying Signals Specification

## Version: 2.0.0

## Purpose

This document catalogs the validated buying signals used by the Company Intelligence Expert to estimate buying readiness, pilot timing, and account prioritization.

---

## Signal Schema

Every buying signal must follow this structure:
- **Signal ID**: S-[CATEGORY]-[NNN]
- **Category**: Domain classification.
- **Description**: What the signal represents.
- **Detection Method**: Query parameters and source locations used to find it.
- **Confidence Rating**: Base confidence multiplier (0.0 to 1.0) before decay.
- **Decay Rate**: Volatility classification (High/Medium/Low) for freshness check.
- **Urgency Level**: Tactical priority (Immediate, High, Medium, Low).
- **Suggested Next Action**: Direct recommendation for sales or pilot teams.

---

## Buying Signals Catalog

### 1. Organizational & Leadership (S-ORG)

#### S-ORG-001: New C-Level Executive (CIO, CISO, CTO, CRO)
- **Category**: Organizational & Leadership
- **Description**: An external executive hire in a core business unit.
- **Detection Method**: Press releases (Tier 1/4), SEC 8-K filings (Tier 1), LinkedIn profile updates (Tier 5).
- **Confidence Rating**: $0.85$ (based on H-HIRE-001).
- **Decay Rate**: High (90-day buying window).
- **Urgency Level**: High.
- **Suggested Next Action**: Initiate outreach on Day 90, referencing their strategic transition objectives and offering an introductory executive brief on operational gaps.

#### S-ORG-002: New Director/VP of Infrastructure or Engineering
- **Category**: Organizational & Leadership
- **Description**: External hire in a key mid-management role responsible for technical execution.
- **Detection Method**: Job board hire data, LinkedIn title change updates.
- **Confidence Rating**: $0.75$.
- **Decay Rate**: High (90-day window).
- **Urgency Level**: Medium.
- **Suggested Next Action**: Pitch technical proof-of-concept (PoC) templates to accelerate their onboarding assessment phase.

#### S-ORG-003: M&A Deal Announcement (Acquirer Role)
- **Category**: Organizational & Leadership
- **Description**: The target company acquires another entity, requiring system and tech stack integration.
- **Detection Method**: SEC 8-K filings, PR announcements, corporate development press releases.
- **Confidence Rating**: $0.80$.
- **Decay Rate**: Medium (180-day window).
- **Urgency Level**: High.
- **Suggested Next Action**: Pitch software integration, identity access management (IAM) consolidation, or data migration solutions.

---

### 2. Financial & Budget (S-FIN)

#### S-FIN-001: Late-Stage Venture Funding (Series C+)
- **Category**: Financial & Budget
- **Description**: Target private company closes a funding round exceeding $30M, indicating cash availability for scaling.
- **Detection Method**: Venture capital portfolio updates, press releases, PitchBook/Crunchbase databases.
- **Confidence Rating**: $0.70$.
- **Decay Rate**: High (90-day window).
- **Urgency Level**: High.
- **Suggested Next Action**: Pitch scalability platforms, compliance automation, or enterprise security frameworks.

#### S-FIN-002: Segment R&D Budget Expansion
- **Category**: Financial & Budget
- **Description**: SEC 10-K report shows a >20% increase in R&D capital allocation for the specific business unit.
- **Detection Method**: SEC 10-K/10-Q Item 2 (MD&A) and Notes to Financial Statements.
- **Confidence Rating**: $0.90$.
- **Decay Rate**: Low (360-day window).
- **Urgency Level**: Medium.
- **Suggested Next Action**: Map engineering hiring templates to the expanded segment to locate target hiring teams.

---

### 3. Technology & Infrastructure (S-TECH)

#### S-TECH-001: Tech Stack Migration Job Postings
- **Category**: Technology & Infrastructure
- **Description**: Multiple active job listings seeking expertise in a competitor's replacement technology (e.g., hiring Snowflake engineers while running Oracle).
- **Detection Method**: Corporate job boards, Indeed/LinkedIn job posting data.
- **Confidence Rating**: $0.90$ (based on H-HIRE-002).
- **Decay Rate**: Medium (180-day window).
- **Urgency Level**: High.
- **Suggested Next Action**: Deliver migration case studies, automated conversion tool pitches, or architectural design reviews.

#### S-TECH-002: Trust Center or Security Badge Updates
- **Category**: Technology & Infrastructure
- **Description**: Addition of SOC 2, HIPAA, or ISO 27001 landing pages.
- **Detection Method**: Automated subdomain crawlers targeting `security.company.com` or `trust.company.com`.
- **Confidence Rating**: $0.70$ (based on H-BUY-001).
- **Decay Rate**: Medium (180-day window).
- **Urgency Level**: Medium.
- **Suggested Next Action**: Pitch continuous compliance monitoring, vendor risk management, or automated penetration testing.

#### S-TECH-003: Public Status Page Outage Spikes
- **Category**: Technology & Infrastructure
- **Description**: Spikes in latency or outages on the public-facing status portal.
- **Detection Method**: Status page feed monitoring (e.g., Atlassian Statuspage RSS).
- **Confidence Rating**: $0.80$ (based on H-HIRE-003).
- **Decay Rate**: High (30-day window).
- **Urgency Level**: Immediate.
- **Suggested Next Action**: Reach out to the VP of SRE/Infrastructure with uptime optimization frameworks or automated failover solutions.

---

### 4. Regulatory & Compliance (S-REG)

#### S-REG-001: Industry-Wide Compliance Deadlines (e.g., DORA, NIS2)
- **Category**: Regulatory & Compliance
- **Description**: Regulatory enforcement dates approaching for the target's operating sector.
- **Detection Method**: Regulatory body announcements cross-referenced with target's primary sector.
- **Confidence Rating**: $0.85$.
- **Decay Rate**: Low (tied to regulatory calendar).
- **Urgency Level**: High.
- **Suggested Next Action**: Pitch targeted compliance audits and pre-packaged regulatory mapping modules.

#### S-REG-002: Consent Decree / Regulatory Remediation Orders
- **Category**: Regulatory & Compliance
- **Description**: Company is legally ordered by a regulator (FTC, SEC, CFPB) to audit or remediate operations.
- **Detection Method**: Regulatory enforcement databases, legal news portals.
- **Confidence Rating**: $0.95$.
- **Decay Rate**: Medium (180-day window).
- **Urgency Level**: Immediate.
- **Suggested Next Action**: Pitch specialized audit, compliance mapping, or security logging tools.