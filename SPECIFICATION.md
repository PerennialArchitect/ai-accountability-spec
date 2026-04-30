# AI Accountability Specification Standard

**Version:** v0.1.0 — Community Review
**Status:** Draft — Open for Community Review
**Author:** Paramdeep Singh, The Perennial Architect
**Date:** 2026-04-29

---

## 1. Purpose and Scope

### 1.1 Purpose

This standard defines a minimum engineering artifact that must be completed before any AI system decision boundary is accepted into a deployment review. The artifact creates a pre-deployment accountability record: who owns each autonomous decision, what that ownership entails, and what happens when that decision goes wrong.

The standard does not govern model architecture, training methodology, or performance benchmarks. It governs the human accountability structure that must exist alongside any AI system that acts autonomously in a consequential domain.

### 1.2 The Accountability Gap

An AI system is auditable when its behavior can be reconstructed after the fact from logs, traces, or telemetry. An AI system is accountable when a named person is assigned — before deployment — to answer for each decision boundary that the system exercises autonomously.

Auditability is a technical property. Accountability is a human property. Almost all deployed AI systems achieve the first. Almost none formally achieve the second before deployment.

This standard closes that gap at design time, not post-incident time.

### 1.3 Scope of Application

This standard applies to any AI system that:

1. Makes decisions that affect external parties (customers, citizens, patients, operators) without requiring human confirmation for each individual decision
2. Operates within a domain where incorrect decisions carry material consequences (financial, physical, reputational, legal)
3. Is intended for deployment beyond controlled testing environments

It is especially applicable to — but not limited to — systems regulated under the EU AI Act (high-risk AI systems), systems requiring human oversight under NATO AI Principles, and any system subject to fiduciary, medical, or safety certification requirements.

### 1.4 What This Standard Does Not Do

This standard does not:
- Replace safety engineering or formal verification requirements
- Satisfy regulatory compliance in full (though it supports it)
- Assess model quality, bias, or fairness
- Define appropriate decision boundaries (that is a domain-specific judgment)
- Eliminate risk (it makes risk ownership explicit)
- Replace safety engineering artifacts (FMEA, FTA, HARA, hazard log) or formal verification requirements

> **Note:** The Named Owner of a decision boundary should have access to the hazard analysis for that boundary as part of accepting their designation. Accountability without understanding of failure modes is incomplete.

---

## 2. Definitions

**Decision Boundary**
A defined scope within which an AI system may act autonomously — without requiring human confirmation for each individual action — to produce an outcome that affects external parties. A decision boundary is bounded by: a trigger condition (what initiates autonomous action), an authority limit (the maximum scope of that action), and an exclusion set (conditions under which the system must defer to a human regardless of the trigger).

**Named Owner**
A specific, identifiable human being — not a role, team, department, or vendor — who is formally designated as accountable for the outcomes produced by a specific decision boundary. The Named Owner must be reachable, must have sufficient authority to intervene in the system's operation, and must have explicitly accepted the accountability assignment. Named ownership transfers when the person leaves the organization or the role; it does not automatically transfer and cannot be left vacant.

**Consequence Chain**
A pre-written, ordered sequence of events that is specified — before deployment — to describe what happens when a decision boundary is crossed incorrectly. The consequence chain is not a retrospective incident report; it is a prospective commitment about how the system, the named owner, and the organization will respond to boundary violations. It must include: immediate system response, notification recipients, escalation path, resolution authority, and review trigger.

**Deployment Clearance**
A binary gate — not a score, not a risk rating — that determines whether an AI system with defined decision boundaries may proceed to deployment review. Clearance requires all three fields (Decision Boundary, Named Owner, Consequence Chain) to be complete and reviewed by a named authority distinct from the Named Owner. A system that does not achieve clearance is not cleared. There is no partial clearance.

**Specification Owner**
The person responsible for the accuracy and currency of this accountability specification document. The Specification Owner is not necessarily the Named Owner of any individual decision boundary. They are accountable for the document: keeping it updated when boundaries change, ensuring Named Owners are current, and triggering re-review when system scope changes.

**Boundary Violation**
Any instance in which an AI system acts outside its defined decision boundary — either by exceeding its authority limit, acting on a trigger condition outside its defined scope, or acting in a circumstance listed in its exclusion set.

---

## 3. The Three Required Fields

### 3.1 Decision Boundary

#### 3.1.1 Definition

The Decision Boundary field defines precisely what the AI system can do without human confirmation. It must be specific enough that a person unfamiliar with the system could determine, given a described scenario, whether the system is authorized to act autonomously in that scenario.

#### 3.1.2 Why It Matters

Vague decision boundaries are the primary source of accountability gaps. When a system's autonomous authority is not precisely bounded, violations cannot be detected, ownership cannot be assigned, and consequences cannot be anticipated. Precision is a prerequisite for accountability.

#### 3.1.3 What "Empty" Looks Like

- "The system makes recommendations based on user data."
- "The AI assists operators with decision-making."
- "Automated processing for standard cases."
- Any description that does not specify what the system can do without asking a human.

#### 3.1.4 What "Complete" Looks Like

A complete Decision Boundary specifies:

1. **Trigger Condition:** The observable state or input that causes the system to act autonomously. Must be specific enough to be detectable in logs.

   Example: "Customer account balance falls below $50 AND no human transaction has occurred in the preceding 30 days."

2. **Autonomous Action:** What the system does when triggered, without human confirmation.

   Example: "System sends a single SMS notification to the primary account phone number and downgrades the account tier to Basic."

3. **Authority Limit:** The hard upper bound of what the system may do. Actions beyond this limit require human confirmation regardless of trigger state.

   Example: "The system may not close accounts, issue refunds, or contact third parties. These require named operator confirmation."

4. **Exclusion Set:** Conditions under which the system must defer to a human even if the trigger condition is met.

   Example: "Accounts flagged for fraud review, accounts held by minors, accounts under regulatory hold."

#### 3.1.5 Validation Criteria

A Decision Boundary is valid if: (a) a test case can be constructed that clearly falls inside the boundary, and (b) a test case can be constructed that clearly falls outside the boundary, and (c) these can be verified by someone who did not write the boundary.

---

### 3.2 Named Owner

#### 3.2.1 Definition

The Named Owner field assigns a specific human being as the accountable party for outcomes produced by this decision boundary. Accountability means: if the boundary is crossed incorrectly and harm results, this person is the first answerable party — to leadership, regulators, affected parties, and the public.

#### 3.2.2 Why It Matters

Collective accountability is no accountability. When a team, a department, or a vendor "owns" an AI decision boundary, no individual is answerable when that boundary produces harm. The consequence — whatever it is — must be able to follow a person. This field makes that possible.

The appointment requirement (who designated this person) is not bureaucratic formality. It establishes the authority chain: if the Named Owner cannot fulfill their accountability, who steps in, and who decides that.

#### 3.2.3 What "Empty" Looks Like

- "The ML Platform team."
- "Product and Engineering."
- "Vendor-managed."
- No field populated.
- A name with no appointment record.

#### 3.2.4 What "Complete" Looks Like

A complete Named Owner entry specifies:

1. **Full Name:** The person's legal name as it appears in organizational records.
2. **Role:** Their current organizational role — not a generic title, but the specific role as it relates to this system.
3. **Contact:** A direct, current contact method (not a team inbox).
4. **Appointing Authority:** The name and role of the person who formally designated them as Named Owner.
5. **Date Appointed:** The date the designation was made.
6. **Acceptance:** Confirmation that the Named Owner has been notified and has accepted the designation. Unsigned designations are incomplete.

#### 3.2.5 Named Owner Vacancy

If a Named Owner leaves the organization or the role, the Decision Boundary is immediately in a state of accountability vacancy. An AI system operating on a vacant decision boundary is not cleared. The Specification Owner is responsible for detecting vacancy and triggering reassignment within a defined period (recommended: 5 business days).

A decision boundary with a Named Owner vacant for more than 5 business days is automatically in NOT CLEARED status. Any system operating on a boundary in NOT CLEARED status due to vacancy must be flagged to the Clearance Authority within 24 hours of the vacancy deadline passing.

---

### 3.3 Consequence Chain

#### 3.3.1 Definition

The Consequence Chain field defines what happens — in writing, in sequence, before deployment — when this decision boundary is crossed incorrectly. It is a prospective commitment, not a retrospective reconstruction.

#### 3.3.2 Why It Matters

Most organizations plan for what a system will do when it works correctly. The Consequence Chain requires planning for what happens when it does not. This is the engineering discipline of failure design — it forces the question of whether the organization is actually prepared to operate the system.

A consequence chain that cannot be written (because no one knows who would be notified, or who has resolution authority, or what remediation looks like) is a signal that the system is not ready for deployment, independent of whether the model performs well.

#### 3.3.3 What "Empty" Looks Like

- "Incidents will be escalated per standard protocol."
- "The on-call team handles issues."
- "Follow the incident management runbook."
- Any consequence chain that does not name specific people, specific actions, and specific timelines.

#### 3.3.4 What "Complete" Looks Like

A complete Consequence Chain specifies, in order:

1. **Immediate System Response:** What the system does automatically when a boundary violation is detected. This should be a defined and testable behavior, not a hoped-for one.

2. **Named Owner Notification:** How and when the Named Owner is notified. Must include a time bound (e.g., "within 5 minutes via PagerDuty and SMS").

3. **Notification Recipients:** A list of additional parties notified (legal, compliance, affected users, regulators) and the trigger condition for each notification. Not everyone is notified for every violation.

4. **Escalation Path:** If the Named Owner does not respond within a defined period, who is notified next, in sequence, with time bounds for each step.

5. **Resolution Authority:** The specific person (not a role) who has authority to declare the incident resolved, lift a system suspension, or authorize a boundary modification.

6. **Review Trigger:** Under what conditions does a boundary violation trigger a formal review of the decision boundary itself — not just remediation of the immediate incident.

#### 3.3.5 Testability Requirement

The Consequence Chain must be tested before deployment clearance. A consequence chain that has never been exercised cannot be claimed as complete. Testing may be done via tabletop exercise, staging environment simulation, or structured walkthrough with the Named Owner and resolution authority present.

Minimum acceptable test: a structured walkthrough in which the Named Owner, Clearance Authority, and at least one system operator confirm they can execute each step in the chain without reference to documentation they have not previously read. The walkthrough must be documented with participant names and date.

---

## 4. Deployment Clearance Gate

### 4.1 The Gate

Deployment Clearance is a binary determination: **CLEARED** or **NOT CLEARED**.

A system achieves CLEARED status when, for every decision boundary defined in its accountability specification:

1. The Decision Boundary field is complete per Section 3.1.4
2. The Named Owner field is complete per Section 3.2.4, including confirmed acceptance
3. The Consequence Chain field is complete per Section 3.3.4 and has been tested per Section 3.3.5
4. All three fields have been reviewed by a named Clearance Authority who is not the Named Owner, the Specification Owner, or the system's primary developer
5. The Clearance Authority has signed the specification with their name and the date of clearance

### 4.2 Clearance Authority

The Clearance Authority is the person who reviews and signs the completed accountability specification. Requirements:

- Must not be the Named Owner for any boundary in the document
- Must not be the Specification Owner
- Must have sufficient organizational authority to block deployment if the specification is incomplete
- Must have read and understood this standard before conducting a review

The Clearance Authority's signature is not a rubber stamp. It is a commitment that they have verified the fields are complete, the Named Owner has accepted their designation, and the Consequence Chain is testable.

### 4.3 Partial Clearance Does Not Exist

If one decision boundary in a system's accountability specification is incomplete, the entire system is NOT CLEARED. A system may not be deployed with a partial specification and a commitment to complete it later.

Exception: If a system is deployed in a strictly limited scope (a defined pilot with a bounded user population and enhanced monitoring), a partial specification may be accepted for that limited scope only, documented explicitly, with a defined deadline for completion. This exception must be authorized in writing by the Clearance Authority and the Named Owner of the incomplete boundary.

### 4.4 Specification Currency

An accountability specification becomes invalid — and requires re-clearance — when any of the following occur:

- The system's decision boundaries change (expanded or contracted scope, new trigger conditions, new authority limits)
- The Named Owner changes for any boundary
- The system's underlying model is updated in a way that may affect boundary behavior
- A boundary violation occurs that was not addressed by the existing Consequence Chain
- 12 months have elapsed since last clearance (annual review minimum)

---

## 5. Versioning

### 5.1 Standard Versioning

This specification follows semantic versioning:

- **Major version** (1.0.0): Changes that are incompatible with previously completed specifications
- **Minor version** (0.1.0): New sections, new guidance, new definitions that do not invalidate prior work
- **Patch version** (0.1.1): Clarifications, corrections, editorial changes

Current version: **v0.1.0 — Community Review**

Completed specifications should record the version of this standard they were completed against. When the standard is updated, previously completed specifications remain valid unless the change log for the new version specifies otherwise.

### 5.2 Change Process

Changes to this standard are proposed via GitHub Issues and require:
1. A written rationale explaining why the current standard is insufficient
2. A proposed change with specific text
3. At least two reviewers from organizations who have completed at least one accountability specification
4. A 30-day community review period before merge

Bootstrap provision (v0.1.x only): For changes to v0.1.x versions, reviewers may be practitioners who have evaluated this standard against a real system — whether or not they have completed a formal accountability specification under this standard. This bootstrap provision expires at v1.0.0.

---

## 6. Acknowledgements

This standard was created by Paramdeep Singh (The Perennial Architect) as part of the broader work toward a perennial hybrid society — an organizational and technical framework for humans, AI agents, and autonomous systems to be productive, accountable members of shared endeavors.

The accountability framework is rooted in four principles:
1. Accountability is a name, not a role
2. You cannot own what you cannot understand
3. Override authority requires override economics
4. The consequence must follow the decision

These principles are the foundation of the three required fields.

---

*Copyright © Paramdeep Singh. Licensed under [CC BY 4.0](LICENSE).*
