# AI Accountability Specification

> **Instructions:** Complete all fields before submitting for deployment review.
> Fields marked [REQUIRED] must not be empty. Fields left blank block deployment clearance.
> Reference [SPECIFICATION.md](../SPECIFICATION.md) for field definitions and completion criteria.

---

**System Name:** [REQUIRED — The name of the AI system or component]
**System Version:** [REQUIRED — Model version or deployment version]
**Date:** [REQUIRED — YYYY-MM-DD]
**Specification Version:** [REQUIRED — Version of the AI Accountability Spec Standard used, e.g., v0.1.0]
**Specification Owner:** [REQUIRED — Full name of the person responsible for this document's accuracy]
**Specification Owner Contact:** [REQUIRED — Direct contact]

---

## Decision Boundaries

*Add one section per decision boundary. Duplicate this section for each boundary. Every autonomous decision the system makes must be represented here.*

---

### Boundary [ID — e.g., BOUNDARY-001]

**Name:** [Short descriptive name for this boundary]

---

#### Decision Boundary

**Trigger Condition:**
[REQUIRED — The observable state or input that causes the system to act autonomously. Must be specific enough to be detectable in logs.]

**Autonomous Action:**
[REQUIRED — What the system does when triggered, without human confirmation. Be precise: not "the system responds" but what it actually does.]

**Authority Limit:**
[REQUIRED — The hard upper bound of what the system may do under this boundary. Actions beyond this limit require human confirmation regardless of trigger state.]

**Exclusion Set:**
[REQUIRED — Conditions under which the system must defer to a human even if the trigger condition is met.]

---

#### Named Owner

**Full Name:** [REQUIRED]
**Role:** [REQUIRED — Their current organizational role as it relates to this system]
**Contact:** [REQUIRED — Direct contact, not a team inbox]
**Appointed by:** [REQUIRED — Full name and role of the appointing authority]
**Date Appointed:** [REQUIRED — YYYY-MM-DD]
**Acceptance Confirmed:** [ ] Yes, Named Owner has been notified and has confirmed acceptance in writing

---

#### Consequence Chain

*Complete in sequence. Each step must be specific: name people, name systems, name timelines.*

**Immediate System Response:**
[REQUIRED — What the system does automatically when a boundary violation is detected. Must be a defined, testable behavior.]

**Named Owner Notification:**
[REQUIRED — How and when the Named Owner is notified. Include method and time bound. Example: "Via PagerDuty alert and SMS within 5 minutes of detection."]

**Notification Recipients:**
| Party | Trigger Condition | Method | Time Bound |
|-------|------------------|--------|------------|
| [Name or role] | [What triggers their notification] | [Email/SMS/call] | [e.g., within 30 min] |
| | | | |

**Escalation Path:**
| Step | If Named Owner does not respond within... | Escalate to... | Via... |
|------|------------------------------------------|----------------|--------|
| 1 | [time] | [Name, Role] | [method] |
| 2 | [time] | [Name, Role] | [method] |

**Resolution Authority:**
**Name:** [REQUIRED — The specific person, not a role, who can declare the incident resolved and authorize boundary modifications]
**Role:** [REQUIRED]

**Review Trigger:**
[REQUIRED — Under what conditions does a boundary violation trigger a formal review of the decision boundary itself, beyond remediation of the immediate incident.]

---

#### Consequence Chain Test Record

**Tested:** [ ] Yes / [ ] No
**Test Method:** [Tabletop exercise / Staging simulation / Structured walkthrough]
**Test Date:** [YYYY-MM-DD]
**Test Participants:** [Names of Named Owner and Resolution Authority who participated]
**Test Outcome:** [Pass / Fail / Gaps identified — describe]

---

#### Deployment Clearance

**Checklist:**
- [ ] Decision boundary complete (trigger condition, autonomous action, authority limit, exclusion set all specified)
- [ ] Named owner confirmed and has accepted designation in writing
- [ ] Consequence chain reviewed and complete
- [ ] Consequence chain tested

**Cleared by:** [REQUIRED — Full name of Clearance Authority — must not be the Named Owner, Specification Owner, or primary system developer]
**Clearance Authority Role:** [REQUIRED]
**Date Cleared:** [REQUIRED — YYYY-MM-DD]

**Clearance Status:** [ ] CLEARED / [ ] NOT CLEARED

*If NOT CLEARED: List the specific fields that are incomplete and the owner responsible for completing each.*

---

*(Add additional BOUNDARY-00N sections as needed)*

---

## Specification History

| Version | Date | Author | Change |
|---------|------|--------|--------|
| 1.0 | [YYYY-MM-DD] | [Name] | Initial specification |
| | | | |

---

## Re-Clearance Triggers

Check all that apply to this system. If any box is checked after initial clearance, re-clearance is required:

- [ ] Decision boundary scope has changed
- [ ] Named Owner has changed for any boundary
- [ ] Underlying model updated in a way that may affect boundary behavior
- [ ] Boundary violation occurred that was not addressed by the existing consequence chain
- [ ] 12 months have elapsed since last clearance

---

*This specification was completed using the [AI Accountability Specification Standard](https://github.com/TechySingh/ai-accountability-spec) v0.1.0.*
*Copyright of completed specification: [Your Organization]. Standard copyright: Paramdeep Singh, licensed CC BY 4.0.*
