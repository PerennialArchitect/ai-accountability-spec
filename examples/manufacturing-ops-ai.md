# Accountability Specification — Manufacturing Production Scheduling AI

**System:** Automated Production Scheduling AI (APSA)
**Facility:** Precision components manufacturing plant
**Deployment context:** APSA monitors inline quality sensors across five production lines and manages work order flow: holding, queuing, and releasing work orders based on real-time quality data. The system operates across three shifts. Shift supervisors receive alerts but the system acts autonomously within defined boundaries.
**Spec version:** 1.0
**Spec date:** 2025-11-04
**Reviewing authority:** Marcus Holt, VP Manufacturing Operations
**Review date:** 2025-11-10

---

## Background

The plant runs on continuous flow. Expediting pressure from the floor and from sales is constant — a held work order costs approximately $4,200/hour in downstream idle time. Before APSA, supervisors routinely released work orders under pressure despite marginal sensor readings, reasoning that "it'll probably be fine." Three quality escapes in eighteen months — two caught at final inspection, one reaching a customer — prompted the decision to automate the hold decision and lock the release decision behind a named human.

The accountability spec was written before APSA went live. It exists so that at 2AM on a Saturday, when a shift supervisor is getting calls from the floor about a held order, they know exactly what they can and cannot do, and who they need to call.

---

## Decision Boundary 1 — Autonomous Quality Hold

### Decision Boundary

APSA may place a quality hold on any active work order when **all** of the following conditions are met:

1. At least two of three inline sensors for the affected operation read outside the control limit (not merely outside the warning limit) for three consecutive sampling intervals.
2. The affected work order is not already in held status.
3. The hold action does not affect a work order flagged as a customer-critical shipment with confirmed dock date within 48 hours. (Customer-critical work orders are flagged in the ERP by the Scheduling Manager at order release; APSA cannot override that flag autonomously.)

**Scope of autonomous action:** APSA places the hold in the ERP, suspends the work order from the queue, sends an immediate alert to the shift supervisor and quality technician on duty, and logs the sensor readings and timestamp that triggered the hold.

**Hard limit:** APSA may not place a hold based on a single sensor reading, may not hold customer-critical work orders (as defined above), and may not take any action on a work order beyond placing the hold and sending notifications. APSA does not diagnose root cause. APSA does not recommend corrective action. APSA does not contact customers.

**What this boundary is not:** APSA is not a substitute for the quality technician's investigation. The hold buys time. The human does the work.

### Named Owner

**Elena Vasquez**, Quality Systems Manager
Appointed by: Marcus Holt, VP Manufacturing Operations, on 2025-11-04
Accountable for: all autonomous hold decisions made by APSA, the accuracy and currency of sensor control limits loaded into APSA, and the process by which those limits are reviewed and updated.

Elena is not accountable for what caused the quality deviation. She is accountable for whether the system's boundary was correctly defined and whether it fired correctly.

### Consequence Chain — Hold Fires Incorrectly (False Positive)

A false positive occurs when APSA places a hold and subsequent investigation confirms the process was in control.

1. Shift supervisor documents the false positive in the APSA incident log within 30 minutes of determination.
2. Quality technician on duty preserves sensor data and calibration records for the relevant interval.
3. Elena Vasquez is notified within 2 hours regardless of time of day.
4. Elena reviews sensor data and control limit settings within 4 hours.
5. If a misconfigured control limit is identified, Elena corrects it and documents the correction before APSA resumes autonomous hold decisions on that line.
6. If the sensor is suspect, the line remains in manual hold-authority mode until calibration is confirmed.
7. Elena files a boundary review record within 5 business days. Three false positives within 30 days triggers a mandatory review of APSA's hold logic with Marcus Holt.

### Consequence Chain — Hold Fails to Fire (False Negative / Miss)

A miss occurs when a quality escape is traced to a work order that APSA should have held but did not.

1. Quality escape investigation must include an APSA log review as a mandatory step.
2. Elena Vasquez is named in the quality escape report as the accountable party for the boundary miss.
3. APSA is suspended from autonomous hold authority on the affected line pending investigation.
4. Marcus Holt reviews with Elena within 24 hours.
5. If the miss resulted from a sensor gap (sensor offline, data dropout), the sensor maintenance record is reviewed and the maintenance owner is identified.
6. If the miss resulted from an incorrectly set control limit, Elena documents the correction and a root cause for how the limit became incorrect.
7. APSA is not restored to autonomous hold authority on the affected line until Marcus Holt signs off.

---

## Decision Boundary 2 — Hold Release Requires Named Supervisor Sign-Off

### Decision Boundary

APSA **may not** release a held work order autonomously under any circumstances.

A held work order may only be released when **all** of the following are true:

1. The quality technician on duty has investigated the hold, documented findings in the quality log, and recorded a disposition: **reject**, **rework**, or **release with deviation**.
2. For a release-with-deviation disposition: the shift supervisor on duty has reviewed the technician's findings and explicitly approved the release in the ERP using their named credentials.
3. For a rework disposition: the rework is completed, re-inspected, and the technician has updated the quality log.
4. For a reject disposition: the work order is closed; no release action is taken.

**Hard limit:** No automated release path exists. APSA cannot be configured, patched, or overridden in the field to release a held work order. The release action in the ERP requires a named credential from a supervisor-level or above role. Shift leads do not have release authority; shift supervisors do.

**Expediting pressure does not modify this boundary.** Sales urgency, customer calls, and dock date pressure are not inputs to the release decision. If a customer-critical work order is held and the situation escalates, the escalation path is to Marcus Holt — not around the quality hold.

### Named Owner

**Daniel Osei**, Manufacturing Operations Manager
Appointed by: Marcus Holt, VP Manufacturing Operations, on 2025-11-04
Accountable for: all release decisions on held work orders, the integrity of the release authorization process, and ensuring shift supervisors understand and follow the release authority chain.

Daniel owns the release side. Elena owns the hold side. They are separate because the person who defines whether to hold and the person who owns the operational consequence of holding must not be the same — that is where expediting pressure collapses accountability.

### Consequence Chain — Unauthorized Release (Release Without Required Sign-Off)

An unauthorized release occurs when a held work order moves to active status without the required quality log entry and named supervisor authorization.

1. APSA detects the unauthorized state change within one sampling interval and generates a critical alert to Daniel Osei, Elena Vasquez, and Marcus Holt simultaneously.
2. The work order is immediately returned to held status by APSA (this is the only autonomous write action APSA can take on a held order).
3. The work order does not move until steps in Boundary 2 are fully completed.
4. Daniel Osei identifies the credential used to authorize the release within 1 hour.
5. If the release was made by a shift supervisor under expediting pressure without quality sign-off, the incident is documented and reviewed with the supervisor and their manager within 24 hours.
6. If the release was made using a shared or borrowed credential, it is treated as a security incident — IT and HR are notified.
7. Marcus Holt reviews within 48 hours. Repeat violations result in removal of release authority from the individual.

### Consequence Chain — Released Work Order Produces a Quality Escape

A quality escape occurs when a work order released from hold reaches a downstream stage (final inspection or customer) with a defect traceable to the condition that triggered the hold.

1. Daniel Osei is named in the quality escape report as the accountable party for the release decision.
2. The full release record — technician findings, supervisor authorization, timestamps — is pulled and reviewed.
3. If the release was properly authorized and the escape reflects a gap in the hold logic, the review shifts to Elena Vasquez and Boundary 1.
4. If the release was improperly authorized, consequence chain for unauthorized release applies (above) plus customer notification protocol is activated by Marcus Holt.
5. APSA's hold logic for the affected parameter is reviewed and tightened before the line resumes full production.
6. Lessons learned are documented in the APSA boundary review log and reviewed at the next quarterly operations review.

---

## Deployment Clearance Sign-Off

| Field | Value |
|---|---|
| Spec author | Elena Vasquez, Quality Systems Manager |
| Reviewing authority | Marcus Holt, VP Manufacturing Operations |
| Review date | 2025-11-10 |
| Status | Cleared for deployment |
| Next mandatory review | 2026-05-10 or upon any model update, sensor configuration change, or quality escape involving APSA |

---

*Copyright © Paramdeep Singh. Licensed under [CC BY 4.0](../LICENSE).*