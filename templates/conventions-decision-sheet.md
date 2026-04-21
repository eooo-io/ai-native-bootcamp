# Session 3 — Conventions Decision Sheet

*For the facilitator to pre-fill with starting positions before Session 3. The team then modifies during Part A.*

The goal is **decisions, not discussion**. Each item needs a team position, even if the position is "we haven't figured this out yet, owner X will come back with a recommendation in two weeks."

---

## Workflow Conventions

### Q1. Who owns the project-context file?

- Starting position: *[name]*
- Updated when: *[conditions]*
- Reviewed in PRs: *[yes/no]*

**Team decision:** _______________

### Q2. Commit message attribution for AI-involved code

Options:
- (a) No attribution — AI is a tool like an IDE
- (b) Mention in commit body when AI did substantial work
- (c) `Co-Authored-By` footer
- (d) Custom convention

Starting position: *[choice + reasoning]*

**Team decision:** _______________

### Q3. When is collapsing all five roles into one tool acceptable?

Starting position: *[e.g., "for <20-line changes in tested code you understand"]*

**Team decision:** _______________

### Q4. Default tool per role

| Role | Default tool | Alternatives |
|---|---|---|
| Thinking | | |
| Executing | | |
| Editing | | |
| Reviewing | | |
| Iterating | | |

**Team decision:** fill in the table above

---

## Quality Conventions

### Q5. What extra review does AI-generated code need?

Starting position: *[e.g., "a human reads the full diff; specifically checks for over-engineering, hallucinated APIs, missing edge case tests"]*

**Team decision:** _______________

### Q6. Does AI-generated code require tests?

- Starting position: *[yes/no]*
- Does AI-written test count as test coverage? *[yes/no/with human review]*

**Team decision:** _______________

### Q7. What counts as "architectural" and therefore requires human decision?

Starting position (tick all that apply):
- [ ] Data model changes
- [ ] Choice of external services or vendors
- [ ] Introducing new framework dependencies
- [ ] Security design (auth, permissions, encryption, PII handling)
- [ ] Breaking API changes
- [ ] Performance-critical code paths
- [ ] Anything touching the deployment or infrastructure layer

**Team decision:** _______________ *(additions/removals)*

---

## Security Conventions

### Q8. What never leaves the local environment?

Non-negotiable starting list:
- [ ] Production `.env` values and secrets
- [ ] API keys and tokens
- [ ] Customer data (PII, financial, health, etc.)
- [ ] Database credentials
- [ ] OAuth secrets

Additional items specific to our team:
- [ ] _______________
- [ ] _______________

**Team decision:** agreed to list above? yes / modifications:

### Q9. Privacy-regulation coverage

Starting position: this team handles data subject to *[e.g., GDPR, CCPA, HIPAA]*.

Implications the team agrees to:
- Customer data stays local — AI tools cannot process it.
- Conversation histories in cloud AI tools may count as processing — *[choice: keep non-customer conversations only / disable history / other]*

**Team decision:** _______________

### Q10. New tool approval process

Before a new AI tool gets access to any company code or data:

- Approver: _______________
- Required review: *[e.g., security review, data processing agreement, model-training policy check]*
- Auto-disallowed if: _______________

---

## Ownership

- **Document owner:** _______________ *(who maintains this file going forward)*
- **Review cadence:** _______________ *(e.g., quarterly)*
- **Next scheduled review:** _______________ *(date)*

---

## Commitments (per-person)

Each team member writes one concrete workflow change they will adopt:

- *[Name]*: _______________
- *[Name]*: _______________
- *[Name]*: _______________

To be revisited at the 2-week follow-up.
