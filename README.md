# AI Accountability Specification Standard

**An engineering artifact — not an ethics checklist — for naming human ownership of AI decision boundaries before deployment.**

[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)
[![Version](https://img.shields.io/badge/version-v0.1.0-blue)](https://github.com/TechySingh/ai-accountability-spec/releases/tag/v0.1.0)
[![Built with AI Accountability Spec](https://img.shields.io/badge/Built%20with-AI%20Accountability%20Spec-0a0a0a)](https://github.com/TechySingh/ai-accountability-spec)

---

Every AI system deployed today has logs. Almost none have a document that names a person, defines a decision boundary, and maps a consequence chain — written before the system acted.

This is that document.

---

## The Gap That Kills

Boeing's MCAS system had rules, logs, certification, and test data. What it did not have was a named person accountable for the decision boundary that allowed the system to override pilot input without confirmation. When that boundary produced the wrong outcome on two flights, 346 people died. The accountability gap was not discovered after deployment. It was designed in before the system flew.

Auditable is not the same as accountable. Every AI system has logs. Almost none have a pre-deployment document that names who owns each decision boundary and what happens — in sequence — when it goes wrong.

This standard closes that gap.

---

## The 2AM Test

Your AI system just did something unexpected. It's 2AM. Someone calls.

Can you answer these in under 10 seconds?

- What was the system **authorized** to do?
- **Who** owns the outcome?
- What happens **next**?

If any answer is "I'd have to check," you don't have an accountability specification. You have logs.

---

## The Three Required Fields

Every AI system decision boundary requires exactly three fields to be complete before deployment review proceeds:

### 1. Decision Boundary
What the system can do without human confirmation. Defined precisely: the trigger condition, the scope of autonomous action, and the hard limit of that authority.

**Empty looks like:** "The system makes recommendations."
**Complete looks like:** "The system autonomously re-routes delivery vehicles when delay exceeds 15 minutes, without dispatcher confirmation, for routes under 50km with no hazardous cargo."

### 2. Named Owner
The person — not the team, not the department, not the vendor — who is accountable if that boundary produces the wrong outcome. Named owner includes: their name, their role, who appointed them, and the date of appointment.

**Empty looks like:** "The ML team owns model behavior."
**Complete looks like:** "Priya Anand, VP Operations, appointed by CTO on 2025-03-15, accountable for all autonomous re-routing decisions."

**Why appointment matters:** A name without an appointing authority is decoration. The appointment creates the accountability chain.

### 3. Consequence Chain
What happens, in sequence, when this boundary is crossed incorrectly. Written before deployment. Not inferred from logs after the fact.

**Empty looks like:** "Incidents are escalated per standard protocol."
**Complete looks like:** "1. System halt triggered. 2. Named owner notified within 5 minutes. 3. Affected routes returned to manual control. 4. Incident ticket opened with boundary ID. 5. Resolution authority (COO) reviews within 24 hours. 6. Boundary suspended pending review."

---

## Deployment Clearance Gate

All three fields must be:
1. Complete (no empty fields, no placeholder text)
2. Reviewed by a named authority (not the same person as the named owner)
3. Signed off with a date

If any field is incomplete, the system is **not cleared for deployment review**. Not pending review — not cleared.

---

## International Context

This specification operationalizes principles already encoded in law and policy:

- **EU AI Act (2024):** Assigns liability to deployers of high-risk AI systems. This spec creates the pre-deployment record that demonstrates accountability was assigned — not assumed.
- **NATO AI Principles:** Require human oversight of AI in defense contexts. The Named Owner field and Consequence Chain together constitute the engineering implementation of "meaningful human control."
- **NIST AI RMF:** Recommends accountability structures for AI governance. This spec is the artifact that makes those structures concrete and auditable.

The standard is not a compliance tool. It is what compliance tools should point to.

---

## How to Use This Standard

### For a new AI system:
1. For each decision the system makes without human confirmation, fill in [`templates/accountability-spec.md`](templates/accountability-spec.md)
2. Have the Named Owner confirm in writing before deployment review begins
3. Store the completed spec alongside your deployment documentation
4. Review and update on every model update, scope change, or boundary modification

### For an existing system:
1. Audit your current AI systems for decision boundaries that are undocumented
2. Retrofit the spec for each identified boundary
3. Treat any unfillable Named Owner field as a critical finding — it means no human currently owns that decision

### For teams and organizations:
- Require spec completion as a gate in your deployment pipeline
- Version the spec alongside your model versions
- Use the JSON schema ([`templates/accountability-spec.json`](templates/accountability-spec.json)) to integrate with CI/CD tooling

---

## Repository Structure

```
ai-accountability-spec/
├── README.md                          — This document
├── SPECIFICATION.md                   — Full standard document
├── templates/
│   ├── accountability-spec.md         — Markdown template (fill and commit)
│   └── accountability-spec.json       — JSON schema for programmatic use
├── examples/
│   ├── README.md                      — Index of examples
│   ├── isr-ai-recommendation-system.md — Defense AI example (fictional/illustrative)
│   ├── enterprise-ai-deployment.md   — Enterprise loan decision AI example
│   └── manufacturing-ops-ai.md       — Manufacturing production scheduling AI example
├── CONTRIBUTING.md                    — How to contribute examples and proposals
└── LICENSE                           — CC BY 4.0
```

---

## Badge

Add this to your project README to signal that your AI system has a completed accountability specification:

```markdown
[![Built with AI Accountability Spec](https://img.shields.io/badge/Built%20with-AI%20Accountability%20Spec-0a0a0a)](https://github.com/TechySingh/ai-accountability-spec)
```

[![Built with AI Accountability Spec](https://img.shields.io/badge/Built%20with-AI%20Accountability%20Spec-0a0a0a)](https://github.com/TechySingh/ai-accountability-spec)

---

## Contributing

Examples from real deployments (anonymized) and feedback on the standard are welcome. See [CONTRIBUTING.md](CONTRIBUTING.md).

---

## Stay Connected

**Human × AI Weekly** — a newsletter on building AI systems that are accountable, not just capable.
[Subscribe](#) *(link coming)*

---

## Author

**Paramdeep Singh** — The Perennial Architect
Building the infrastructure for a perennial hybrid society where humans, AI agents, and robots are equal productive members.

> "Accountability is a name, not a policy."
> — Paramdeep Singh, The Perennial Architect

---

*Copyright © Paramdeep Singh. Licensed under [CC BY 4.0](LICENSE).*