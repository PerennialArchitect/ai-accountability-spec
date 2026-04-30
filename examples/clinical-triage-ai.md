# AI Accountability Specification — Clinical Triage AI

> **ILLUSTRATIVE EXAMPLE — FICTIONAL**
> This example is fictional and illustrative. Organizations, people, and systems are invented for the purpose of demonstrating the specification standard. No real clinical, patient, or proprietary information is contained herein.

---

**System Name:** TriageIQ — Emergency Department Priority System
**System Version:** v3.1.0
**Date:** 2026-03-10
**Specification Version:** v0.1.0
**Specification Owner:** Dr. Sarah Okonkwo
**Specification Owner Contact:** s.okonkwo@example-hospital.org (direct, 24/7 pager)

---

## System Summary

TriageIQ analyzes incoming patient vitals, chief complaint, and arrival data to assign an ESI (Emergency Severity Index) triage level — 1 through 5 — within 90 seconds of patient registration. Level 1 (immediate) and Level 2 (emergent) assignments are flagged for immediate charge nurse review. Levels 3–5 are assigned autonomously and displayed on the queue board without nurse confirmation unless the system's confidence score falls below threshold.

This specification covers two decision boundaries: autonomous triage assignment (BOUNDARY-001) and confidence-based escalation override (BOUNDARY-002).

---

## Decision Boundaries

---

### Boundary BOUNDARY-001

**Name:** Autonomous ESI Level Assignment (Levels 3–5)

---

#### Decision Boundary

**Trigger Condition:**
Patient registration complete with vitals, chief complaint, and arrival mode entered. System confidence score ≥ 0.82.

**Autonomous Action:**
System assigns ESI Level 3, 4, or 5 and posts to the queue board. Assignment is visible to clinical staff within 30 seconds of trigger. No charge nurse confirmation required before posting.

**Authority Limit:**
The system may not assign ESI Level 1 or 2 autonomously under any circumstances. Assignments for patients flagged as pediatric (under 12), pregnant, or with active sepsis indicator require charge nurse confirmation regardless of confidence score.

---

#### Named Owner

**Name:** Dr. Sarah Okonkwo
**Role:** Chief Medical Information Officer
**Appointed by:** CMO, Dr. Adebayo Fashola
**Date Appointed:** 2025-11-01
**Contact:** s.okonkwo@example-hospital.org · Pager 4471

**Why this owner:** Dr. Okonkwo oversees all clinical AI deployment decisions and has authority to suspend system operation. She has direct accountability to the CMO for all autonomous clinical decisions made by this system.

---

#### Consequence Chain

If an autonomous ESI assignment (Level 3–5) is later escalated by clinical staff to Level 1 or 2:

1. Immediate: Clinical team escalates manually — system does not block or delay escalation
2. Within 15 minutes: Charge nurse logs discrepancy in the TriageIQ feedback module
3. Within 2 hours: Incident ticket auto-generated and routed to Dr. Okonkwo
4. Within 24 hours: Dr. Okonkwo reviews the case and determines whether the discrepancy represents a system error or a clinical judgment difference
5. If system error: BOUNDARY-001 suspended pending root cause analysis; CMO notified
6. Monthly: All discrepancy logs reviewed at Clinical AI Governance committee
7. Resolution authority: Dr. Okonkwo for single incidents; CMO for systemic patterns

---

#### Deployment Clearance

- [x] Decision boundary complete and reviewed by clinical informatics
- [x] Named owner confirmed and notified in writing (2025-11-01)
- [x] Consequence chain reviewed with ED charge nurse team (2025-11-15)
- [x] Exclusion criteria (pediatric, pregnant, sepsis) validated against EHR integration
- **Cleared by:** Dr. Adebayo Fashola, CMO
- **Date:** 2025-11-20

---

### Boundary BOUNDARY-002

**Name:** Confidence-Based Escalation Override

---

#### Decision Boundary

**Trigger Condition:**
System confidence score < 0.82 for any triage assignment, regardless of ESI level.

**Autonomous Action:**
System suppresses queue posting and flags patient as "Requires Nurse Review" with reason code "LOW CONFIDENCE." Alert displays to charge nurse on duty with a 3-minute acknowledgement timer.

**Authority Limit:**
System may not suppress or delay the alert once triggered. If charge nurse does not acknowledge within 3 minutes, a secondary alert fires to the attending physician on duty.

---

#### Named Owner

**Name:** James Reinholt
**Role:** Director of Emergency Nursing
**Appointed by:** VP Clinical Operations, Sandra Chiu
**Date Appointed:** 2025-11-01
**Contact:** j.reinholt@example-hospital.org · Pager 3302

**Why this owner:** Mr. Reinholt owns all nursing workflow decisions in the ED. The escalation override affects nursing queue management directly. Dr. Okonkwo owns BOUNDARY-001 (clinical AI decisions); Mr. Reinholt owns BOUNDARY-002 (nursing workflow impact).

---

#### Consequence Chain

If the 3-minute acknowledgement window expires without nurse confirmation:

1. Immediate: Secondary alert fires to attending physician on duty
2. Within 5 minutes: If attending does not acknowledge, triage coordinator is paged
3. Patient remains in "Requires Nurse Review" status — system does not auto-assign ESI
4. Within 30 minutes: Mr. Reinholt notified of any unacknowledged escalation
5. Root cause review within 48 hours: Was the delay a staffing issue, a workflow issue, or a system alert failure?
6. Resolution authority: Mr. Reinholt for workflow issues; Dr. Okonkwo for system issues

---

#### Deployment Clearance

- [x] Decision boundary complete
- [x] Named owner (Reinholt) confirmed and notified
- [x] Consequence chain reviewed with charge nurse team and attending staff
- [x] 3-minute timer validated against ED workflow study (average triage event: 4.2 min)
- **Cleared by:** Sandra Chiu, VP Clinical Operations
- **Date:** 2025-11-20

---

## Specification History

| Version | Date | Author | Change |
|---|---|---|---|
| v1.0 | 2025-11-20 | Dr. S. Okonkwo | Initial deployment clearance |
