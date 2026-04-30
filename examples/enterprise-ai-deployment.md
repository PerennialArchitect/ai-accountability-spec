# AI Accountability Specification — Automated Loan Decision System

> **ILLUSTRATIVE EXAMPLE — FICTIONAL**
> This example is fictional and illustrative. Organizations, people, and systems are invented for the purpose of demonstrating the specification standard. No real organization's data, credit models, or proprietary information is contained herein.

---

**System Name:** ClearPath Automated Credit Decision System (ACDS)
**System Version:** v1.4.2
**Date:** 2026-02-01
**Specification Version:** v0.1.0
**Specification Owner:** Marcus Webb
**Specification Owner Contact:** m.webb@clearpath-example.com (direct)

---

## System Summary

ClearPath ACDS is an AI-powered credit decision system for small business loan applications. The system makes automated approval or decline decisions for applications below defined thresholds. Applications above those thresholds, and all applications flagged by the system's fairness monitoring layer, route to human underwriters.

This specification covers two decision boundaries: automated approval (BOUNDARY-001) and automated decline (BOUNDARY-002). The distinction matters because the consequence chains differ substantially — an erroneous approval creates credit risk; an erroneous decline creates regulatory and reputational risk under fair lending law.

---

## Decision Boundaries

---

### Boundary BOUNDARY-001

**Name:** Automated Small Business Loan Approval

---

#### Decision Boundary

**Trigger Condition:**
All of the following conditions are true simultaneously:
- Loan application amount is $25,000 or less
- ACDS composite credit score (CCS) is 720 or above (range: 300–850)
- Applicant business has been in operation for at least 24 months with verifiable registration
- Debt-to-income ratio (DTI) is 0.38 or below
- No open derogatory marks on primary applicant credit file in preceding 24 months
- Fairness monitoring layer has not flagged the application for human review
- No manual hold flag on the application

**Autonomous Action:**
ACDS autonomously generates an approval decision with the following outputs — without human underwriter review:
- Approval confirmation to the applicant (email + in-app notification)
- Loan terms: approved amount, interest rate (risk-tiered), repayment schedule
- Disclosure package (Truth in Lending Act disclosure, ECOA notice) generated and delivered
- Application moved to funding queue for disbursement processing (next business day)

**Authority Limit:**
ACDS may not:
- Approve loan amounts exceeding $25,000 under any circumstances — this limit is hard-coded and cannot be overridden by system configuration
- Modify credit terms after initial generation without human underwriter sign-off
- Approve applications where applicant identity verification has not been completed
- Initiate disbursement without a separate human authorization step in the funding queue (disbursement authorization is outside this decision boundary)
- Suppress or modify the fairness monitoring layer outputs

**Exclusion Set:**
- Applications from applicants currently in bankruptcy proceedings or with open bankruptcy filings — route to senior underwriter, do not auto-approve
- Applications from businesses in restricted industries (cannabis, firearms manufacturing, payday lending) — route to compliance review, do not auto-approve
- Applications where identity verification confidence score is below 0.92 — route to human identity verification team
- Applications where the fairness monitoring layer flags a potential disparate impact concern — route to senior underwriter regardless of CCS
- Applications from applicants who have previously received an adverse action through ACDS in the preceding 12 months — route to human underwriter to review prior decision before proceeding

---

#### Named Owner

**Full Name:** Danielle Ofosu
**Role:** Chief Credit Officer, ClearPath Financial (sole accountability owner for all automated credit decisions)
**Contact:** d.ofosu@clearpath-example.com | +1-555-012-3456 (direct, monitored 24/7 by EA for urgent flags)
**Appointed by:** Robert Tran, Chief Executive Officer, ClearPath Financial
**Date Appointed:** 2025-09-01
**Acceptance Confirmed:** [x] Yes — Danielle Ofosu signed accountability acceptance letter 2025-09-01, on file with CEO office and Legal.

---

#### Consequence Chain

**Immediate System Response:**
Upon detection of a BOUNDARY-001 violation (approval issued outside defined parameters):
1. Application flagged REVIEW-HOLD in the loan management system — funding queue processing paused for that application
2. ACDS generates a violation incident record with: application ID, violation type, decision timestamp, model inputs at time of decision, CCS components
3. Automated alert raised in the ClearPath operations dashboard

**Named Owner Notification:**
Via direct email to d.ofosu@clearpath-example.com and SMS to registered mobile within 15 minutes of violation detection. If no acknowledgment (read receipt or reply) within 30 minutes during business hours (6am–8pm local), escalation path activates.

**Notification Recipients:**

| Party | Trigger Condition | Method | Time Bound |
|-------|------------------|--------|------------|
| Danielle Ofosu (Named Owner) | Any BOUNDARY-001 violation | Email + SMS | 15 minutes |
| Marcus Webb (Specification Owner) | Any BOUNDARY-001 violation | Email | 20 minutes |
| ClearPath Legal (Helen Marsh, General Counsel) | Any violation involving a potential fair lending concern | Email + direct call | 30 minutes |
| ClearPath Compliance Officer | Any violation | Email | 30 minutes |
| Affected applicant | If their application is placed on REVIEW-HOLD after an issued approval | Email — standard hold notice | Within 2 hours of hold placement |
| CFPB (regulatory notification) | If violation constitutes a material adverse event under ClearPath's regulatory agreements | Formal written notice | As required by agreement (typically 72 hours) |

**Escalation Path:**

| Step | If Named Owner Does Not Respond Within... | Escalate To | Via |
|------|------------------------------------------|-------------|-----|
| 1 | 30 minutes (business hours) / 2 hours (off-hours) | Robert Tran, CEO | SMS + Email |
| 2 | 60 minutes (business hours) / 4 hours (off-hours) from initial alert | Helen Marsh, General Counsel | Direct call |
| 3 | 2 hours from initial alert (any time) | ClearPath Board Audit Committee Chair | Email + direct call via board contact list |

**Resolution Authority:**
**Name:** Danielle Ofosu (Named Owner) for incidents below $25,000 aggregate exposure; Robert Tran (CEO) for incidents involving potential regulatory reporting or aggregate exposure above $25,000.
**Role:** Chief Credit Officer / Chief Executive Officer

Resolution actions available to the Resolution Authority:
- Confirm the approval stands (if violation was a detection false positive, with documented rationale)
- Rescind the approval (customer notified, adverse action notice issued, regulatory requirements followed)
- Modify the application terms and obtain applicant consent
- Authorize a manual underwriting override with senior underwriter sign-off

**Review Trigger:**
A formal review of BOUNDARY-001 itself (not just the incident) is triggered when:
- Any erroneous approval exceeds $15,000 (more than half the authority limit)
- Three or more violations of the same violation type occur within a 30-day period
- Fairness monitoring layer flags a statistically significant disparate impact pattern across any 90-day window
- Any regulatory inquiry, complaint, or enforcement action references a BOUNDARY-001 decision

Formal review is conducted by Danielle Ofosu (Named Owner), Helen Marsh (General Counsel), and the ACDS model team. Results reported to the CEO within 10 business days.

---

#### Consequence Chain Test Record

**Tested:** [x] Yes
**Test Method:** Staging simulation
**Test Date:** 2025-12-05
**Test Participants:** Danielle Ofosu (Named Owner), Marcus Webb (Specification Owner), ACDS engineering lead (Priya Nair), Helen Marsh (General Counsel observing)
**Test Outcome:** Pass with gaps identified

**Outcome Notes:**
Staging test revealed that the 30-minute escalation trigger for off-hours (when Named Owner was simulated as unreachable) was too slow for potential fair lending violations. Off-hours escalation time to General Counsel reduced from 4 hours to 2 hours, and Legal added to the initial notification path for any violation involving the fairness monitoring layer flag (previously Legal was only notified at step 3). Consequence chain updated accordingly. Re-test not required — change was additive (faster notification, more recipients), not a reduction in coverage.

---

#### Deployment Clearance

**Checklist:**
- [x] Decision boundary complete
- [x] Named owner confirmed and has accepted designation in writing
- [x] Consequence chain reviewed and complete
- [x] Consequence chain tested

**Cleared by:** Victoria Andersen, ClearPath Board Audit Committee Chair
**Clearance Authority Role:** Board Audit Committee Chair — independent of executive team, not the Named Owner, Specification Owner, or system developer
**Date Cleared:** 2026-01-20

**Clearance Status:** [x] CLEARED

---

### Boundary BOUNDARY-002

**Name:** Automated Small Business Loan Decline

---

#### Decision Boundary

**Trigger Condition:**
All of the following conditions are true simultaneously:
- Loan application amount is $25,000 or less
- ACDS composite credit score (CCS) is below 620
- Fairness monitoring layer has not flagged the application for human review
- No manual hold flag on the application
- Applicant has not explicitly requested human review (an option provided during application)

**Autonomous Action:**
ACDS autonomously generates a decline decision with the following outputs — without human underwriter review:
- Adverse action notice delivered to applicant (email + postal mail within 3 business days) per ECOA requirements — includes specific reasons for decline (top 4 CCS factors)
- Application closed in loan management system
- Applicant informed of right to request a free copy of their credit report and to dispute inaccurate information

**Authority Limit:**
ACDS may not:
- Issue a decline without a complete adverse action notice meeting ECOA requirements — this is a hard block in the system
- Issue a decline based solely on age, race, sex, marital status, national origin, religion, or receipt of public assistance — these are filtered inputs
- Prevent the applicant from reapplying
- Decline applications that have been flagged by the fairness monitoring layer

**Exclusion Set:**
- Applications flagged by the fairness monitoring layer — route to senior underwriter
- Applications where the CCS is between 620 and 659 (borderline band) — route to human underwriter; automated decline applies only to clear declines below 620
- Applications where the applicant has explicitly requested human review
- Applications from protected classes where machine-only decline decisions create regulatory exposure per current legal guidance — route to human underwriter
- Any application where identity verification is incomplete

---

#### Named Owner

**Full Name:** Danielle Ofosu
**Role:** Chief Credit Officer, ClearPath Financial
**Contact:** d.ofosu@clearpath-example.com | +1-555-012-3456
**Appointed by:** Robert Tran, Chief Executive Officer
**Date Appointed:** 2025-09-01
**Acceptance Confirmed:** [x] Yes — same acceptance letter as BOUNDARY-001, both boundaries explicitly named.

---

#### Consequence Chain

**Immediate System Response:**
Upon detection of a BOUNDARY-002 violation (decline issued outside defined parameters, or decline issued without complete adverse action notice):
1. Application flagged COMPLIANCE-HOLD in the loan management system
2. If adverse action notice was not issued: automated hold on all pending BOUNDARY-002 decline decisions system-wide until Named Owner clears the hold
3. Violation incident record generated with: application ID, violation type, decision timestamp, model inputs

**Named Owner Notification:**
Via email + SMS within 15 minutes. BOUNDARY-002 violations are treated as potential ECOA violations until confirmed otherwise — Legal is notified simultaneously, not in sequence.

**Notification Recipients:**

| Party | Trigger Condition | Method | Time Bound |
|-------|------------------|--------|------------|
| Danielle Ofosu (Named Owner) | Any BOUNDARY-002 violation | Email + SMS | 15 minutes |
| Helen Marsh (General Counsel) | Any BOUNDARY-002 violation | Email + direct call | 15 minutes (simultaneous with Named Owner) |
| Marcus Webb (Specification Owner) | Any BOUNDARY-002 violation | Email | 20 minutes |
| Affected applicant | If their decline is placed on hold — do not send a final decision until hold is resolved | Hold notification only | Within 1 business day |

**Escalation Path:**

| Step | If Named Owner Does Not Respond Within... | Escalate To | Via |
|------|------------------------------------------|-------------|-----|
| 1 | 30 minutes (any time) | Robert Tran, CEO | SMS + Email |
| 2 | 90 minutes from initial alert | Victoria Andersen, Board Audit Committee Chair | Direct call |

**Resolution Authority:**
**Name:** Helen Marsh, General Counsel (for all BOUNDARY-002 violations — fair lending legal exposure makes this a legal determination, not a credit determination)
**Role:** General Counsel, ClearPath Financial

Resolution actions:
- Confirm the decline stands with adverse action notice verified complete (if violation was detection false positive)
- Rescind the decline and route to human underwriter for fresh review
- Initiate adverse action notice correction if notice was incomplete
- Determine whether regulatory self-disclosure is required

**Review Trigger:**
Formal review of BOUNDARY-002 triggered by:
- Any decline later found to have violated ECOA requirements
- Any pattern of declines where the fairness monitoring layer subsequently identifies disparate impact
- Any regulatory inquiry referencing a BOUNDARY-002 decision
- More than two violations of the same type within 30 days

---

#### Consequence Chain Test Record

**Tested:** [x] Yes
**Test Method:** Tabletop exercise
**Test Date:** 2025-12-08
**Test Participants:** Danielle Ofosu (Named Owner), Helen Marsh (General Counsel / Resolution Authority), Marcus Webb (Specification Owner)
**Test Outcome:** Pass

**Outcome Notes:**
Tabletop confirmed that simultaneous notification of Named Owner and Legal was operationally workable. Helen Marsh confirmed resolution authority scope in writing during the exercise — documented in the tabletop record maintained by Marcus Webb.

---

#### Deployment Clearance

**Checklist:**
- [x] Decision boundary complete
- [x] Named owner confirmed and has accepted designation in writing
- [x] Consequence chain reviewed and complete
- [x] Consequence chain tested

**Cleared by:** Victoria Andersen, ClearPath Board Audit Committee Chair
**Clearance Authority Role:** Board Audit Committee Chair
**Date Cleared:** 2026-01-20

**Clearance Status:** [x] CLEARED

---

## Specification History

| Version | Date | Author | Change |
|---------|------|--------|--------|
| 1.0 | 2025-08-15 | Marcus Webb | Initial draft |
| 1.1 | 2025-09-05 | Marcus Webb | Named Owner confirmed and added |
| 1.2 | 2025-12-10 | Marcus Webb | Consequence chains updated after tabletop findings |
| 1.3 | 2026-01-05 | Marcus Webb | BOUNDARY-002 exclusion set expanded per legal review |
| 2.0 | 2026-01-20 | Marcus Webb | Clearance recorded |

---

## Notes on This Example

**Why two boundaries for one system:** Approval and decline have materially different consequence chains. An erroneous approval creates credit risk borne by ClearPath. An erroneous decline creates regulatory risk under fair lending law and is borne by the applicant — who may have been denied credit on discriminatory grounds. The consequences are not symmetric, so the named owners and resolution authorities reflect this asymmetry.

**EU AI Act alignment:** Under the EU AI Act, automated credit assessment systems are classified as high-risk AI. The Named Owner field in this spec creates the concrete accountability record the Act anticipates when it assigns liability to deployers. This spec is the engineering artifact that makes that assignment real.

**The applicant's view:** This spec ensures that if an applicant challenges a decline or an approval — legally, regulatorily, or publicly — there is a named person accountable, a defined response chain, and a documented review trigger. This is not only good governance; it is defensible product design.

---

*This example was created using the [AI Accountability Specification Standard](https://github.com/TechySingh/ai-accountability-spec) v0.1.0.*
*Copyright © Paramdeep Singh. Licensed under [CC BY 4.0](../LICENSE).*
