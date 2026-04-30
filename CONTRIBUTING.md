# Contributing to the AI Accountability Specification Standard

Thank you for contributing. This standard exists to solve a real problem in how AI systems are deployed — and it improves by learning from real deployments.

There are three ways to contribute:

1. **Submit an example** — show the standard applied to a real or realistic deployment
2. **Propose a change to the standard** — suggest improvements to the specification itself
3. **Report a gap** — identify a scenario the current standard does not handle well

---

## 1. Submitting Examples

Examples are the most valuable contribution. A good example shows someone who has never seen this standard how to fill it in for their domain.

### What makes a strong example

- **A non-trivial decision boundary.** "The system sends a notification" is not a meaningful boundary. "The system autonomously closes a customer account when inactivity exceeds 180 days and no dispute is on file" is.
- **A consequence chain that was actually tested.** Show what the test found — including gaps. An example that only shows a clean pass is less useful than one that shows a gap found and resolved.
- **A real Named Owner structure.** Show the appointment chain. Show what "acceptance confirmed" actually looks like in practice.
- **Domain-specific exclusion sets.** The exclusion set is where most real-world complexity lives. Show the edge cases your domain required you to think through.

### What disqualifies an example

- Generic text that could apply to any system
- Empty or placeholder Named Owner fields
- A consequence chain with no notification timelines or named resolution authority
- No consequence chain test record

### How to submit an example

1. Fork this repository
2. Create a file in `examples/` named for your use case: `examples/your-system-name.md`
3. Use the template in `templates/accountability-spec.md` as your base
4. Anonymize as needed — we do not require real organization or person names, but the structure must be real
5. Add your example to the index in `examples/README.md`
6. Open a Pull Request with the title: `[EXAMPLE] Your system name — domain`

Your PR will be reviewed for completeness of the three fields. The review is not a judgment of whether your AI system is "ethical" — it is a check that the example is useful to someone trying to learn the standard.

---

## 2. Proposing Changes to the Standard

The standard is at v0.1.0 — Community Review. The community review period is open for proposals.

### What qualifies as a proposed change

- A gap in the current definitions that causes a real completion problem
- A field that is systematically gamed or misunderstood in practice
- A domain (healthcare, infrastructure, autonomous vehicles) where the current language does not map cleanly
- A structural change to the Deployment Clearance Gate

### What does not qualify

- Style preferences
- Requests to add new fields "just in case"
- Changes that reduce the rigor of the three required fields (proposals to make fields optional will not be accepted)
- Ethics frameworks, AI principles, or governance philosophy additions — this standard is an engineering artifact, not an ethics document

### How to propose a change

1. Open a GitHub Issue with the label `spec-change`
2. Use this structure:
   - **Current text:** Paste the section or field as it currently reads
   - **Problem:** Describe the real-world scenario where this text fails or causes confusion
   - **Proposed change:** Write the new text
   - **Impact on existing specs:** Would completed specifications under the current version need to be updated?

Changes that are accepted will be incorporated in the next minor version (0.2.0). Changes that invalidate prior work go into the major version queue.

---

## 3. Reporting Gaps

A gap is a scenario the current standard does not address — not a bug in the text, but a hole in the coverage.

Examples of gaps worth reporting:
- Multi-system deployments where decision boundaries span multiple AI components and Named Owner assignment is genuinely ambiguous
- Systems where the "consequence chain" must remain confidential (defense, law enforcement) — how does the standard apply?
- Systems operated by third-party vendors where the deploying organization does not control the Named Owner appointment

Open a GitHub Issue with the label `gap-report`. Describe the scenario concretely. We will discuss how the standard should address it, or whether it requires a domain-specific extension.

---

## Versioning and Merge Process

- All changes to `SPECIFICATION.md` require two reviewers who have completed at least one accountability specification
- 30-day community review period before any change to the standard is merged
- Examples do not require a review period but do require one reviewer
- Pull requests that modify the standard without going through the Issue process first will not be merged

---

## Code of Conduct

This is a technical community, not a forum for AI ethics debates. Contributions stay grounded in: what is the engineering artifact, who fills it in, and how do we know it is complete. Off-topic threads will be closed.

---

*Copyright © Paramdeep Singh. Licensed under [CC BY 4.0](LICENSE).*
