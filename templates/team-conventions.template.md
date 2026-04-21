# [Team Name] — AI-Native Development Conventions

**Last reviewed:** *[YYYY-MM-DD]*
**Owner:** *[name]*
**Review cadence:** *[every quarter / as-needed]*

*These conventions are the output of Session 3. They govern how this team uses AI in development. They are not forever — they are as-of-today.*

---

## 1. Workflow Conventions

### Project-context file maintenance

- The project-context file lives at: *[e.g., `CLAUDE.md` in repo root]*
- Owner for updates: *[person or role]*
- Updated when: *[new convention introduced, recurring AI mistake, dependency upgrade]*
- Reviewed in PRs: *[yes / no — if yes, who must review]*

### Commit message conventions for AI-involved code

*[Pick one of the following team positions and stick to it.]*

- [ ] **No attribution** — commits look identical to human-authored commits. AI usage is a team tool, not a meaningful authorship signal.
- [ ] **Body-level mention** — commit subject is normal; body mentions AI if it did substantial work (e.g., "Generated initial implementation via [tool]; reviewed and adjusted").
- [ ] **`Co-Authored-By` footer** — used when AI generated the bulk of the code.

**Our position:** *[fill in which option was chosen and any nuances]*

### Role collapse — when it's OK

The team has agreed to explicit collapse rules:

**OK to collapse all five roles into one tool when:**
- Change is <20 lines diff
- Affected code has tests we trust
- Single file
- You understand the existing code
- You will personally eyeball the diff before commit

**NOT OK to collapse roles for:**
- New features
- Database schema changes
- Security-sensitive code (auth, permissions, payments, PII handling)
- Code touching multiple files
- Code with no test coverage
- Production hotfixes (explicitly: these need review, not fewer checkpoints)

*[Adjust these lists based on your team's actual risk tolerance.]*

### Default orchestration

*[Fill in which tool the team uses for which role. See `../framework/orchestration-matrix.md` for the model.]*

| Role | Tool (default) | Alternatives |
|---|---|---|
| Thinking / Planning | *[...]* | *[...]* |
| Executing | *[...]* | *[...]* |
| Editing | *[...]* | *[...]* |
| Reviewing | *[...]* | *[...]* |
| Iterating | *[...]* | *[...]* |

Deviating from default is fine — but note it in the PR if it's a judgment call.

---

## 2. Quality Conventions

### Review requirements for AI-generated code

PRs that are substantially AI-generated require:

- [ ] A human has read the full diff. Not skimmed. Read.
- [ ] Tests exist and have been run (CI green is not enough — the human has *seen* them run locally at least once).
- [ ] The diff has been checked for these AI-specific failure modes:
  - Over-engineering / unnecessary abstractions
  - Silent error handling (`except: pass`, `catch { }`, `if err != nil { return nil }`)
  - Hallucinated APIs — methods or flags that don't exist
  - Convention drift from the project-context file
  - Scope creep — changes beyond what was asked
  - Missing edge case tests (happy path covered, failure cases aren't)

### Test expectations

*[Our team's position:]*

- AI-generated code must have tests: *[yes / no]*
- AI-generated tests count toward coverage: *[yes / no / with human review]*
- Tests must be run locally before push: *[yes / no]*
- CI must be green before merge: *[yes / no / allow override with approval]*

### The human decides, the AI implements

**Architectural and design decisions stay with humans.** The AI may explore options; it does not pick.

Our team definition of "architectural":

- *[e.g., data model changes]*
- *[e.g., choice of external service / vendor]*
- *[e.g., introducing a new framework dependency]*
- *[e.g., security-related design (auth, permissions, encryption)]*
- *[e.g., breaking API changes]*

---

## 3. Security Conventions

### What never leaves the local environment

Full stop. No exceptions:

- Production `.env` values and the contents of any secrets manager
- API keys, tokens, database credentials, OAuth client secrets
- Customer data of any kind (names, emails, addresses, financial data, health data, identifiers)
- Internal-only documents not already published externally
- Employee information

"Leaves the local environment" means: sent to any cloud-based AI tool including ChatGPT, Claude, Copilot chat, etc., **and** stored in a chat history or memory.

### What is safe to discuss

- Code structure and architecture
- Publicly-documented API usage
- Error messages with sensitive details redacted (replace IDs with `[redacted]`)
- Fake fixture data
- Open-source dependencies

### Privacy regulations

This team handles data subject to: *[e.g., GDPR, CCPA, HIPAA, PCI DSS — list all that apply]*

Implications:
- Customer data does not leave local environment, period.
- MCP servers that query the database locally: fine.
- Pasting query results into a chat AI: not fine, even if "just for debugging."
- Data subject access requests (GDPR): AI conversation logs may be considered processing — keep them local or deletable.

### New tool approval

Before a new AI tool gets access to any company code or data:

- Approver: *[who must sign off]*
- Data processing agreement required if: *[conditions]*
- Disallowed if: *[conditions — e.g., trains on customer submissions by default]*

---

## 4. Living Document

### Review cadence

- Every quarter: the team revisits the whole document in a 30-min meeting.
- Anytime a recurring AI-related issue comes up: add a convention or update an existing one.
- When a new category of tool is considered: update section 1 (Default orchestration).

### How changes happen

- Propose in a PR against this file.
- Discuss in a team meeting if non-trivial.
- Merge means: effective immediately for all team members.

### Historical notes

*[Keep a changelog. When a rule changed, note when and why.]*

- *YYYY-MM-DD:* Initial conventions adopted at Session 3 of AI-Native Bootcamp.
- *YYYY-MM-DD:* ...
