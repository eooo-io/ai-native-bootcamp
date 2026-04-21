# Session 3 — AI-Native Team Workflows: "Responsibility, Not Automation"

**Duration:** 90 minutes
**Format:** 25 min discussion/decisions + 25 min demo + 40 min team exercise

## Prerequisites

- Sessions 1 and 2 complete
- Each participant used a CLI agent + at least one MCP server on real work
- Facilitator pre-filled [`../templates/conventions-decision-sheet.md`](../templates/conventions-decision-sheet.md) as a starting point the team will modify

---

## Goal

Establish team conventions for AI-assisted development. Decide — *together* — who is accountable for what, what boundaries the team will not cross, and what this looks like in day-to-day code review.

By the end: a shared, version-controlled document that governs how your team uses AI. Not forever — revisit every quarter — but as of today.

---

## Part A — The Accountability Framework (25 min, discussion + decisions)

### Core principle to anchor the discussion

> AI is a force multiplier for experienced engineers, not a replacement. The best AI workflows make responsibility **sharper**, not softer.

Restate this. The rest of Part A is making that principle real in the form of team conventions.

### Use the decision sheet

Open [`../templates/conventions-decision-sheet.md`](../templates/conventions-decision-sheet.md). The facilitator has pre-filled it with starting positions. Work through it section by section. Agreement does not require consensus — "rough consensus" + "one person owns the call" is enough. But everyone needs to know what was decided.

The decision sheet covers three categories:

### 1. Workflow conventions

- **Project-context file maintenance** — who updates it, what goes in, what doesn't. Is it reviewed in PRs like documentation?
- **Commit messages** — how do you attribute AI involvement? Do you use a `Co-Authored-By` footer, mention in body, or not mention? (All three are defensible; the team needs *one* position.)
- **Role separation in practice** — when is it OK to collapse roles? Default suggestion: OK for <20-line changes in well-tested code you understand. Not OK for new features, security-sensitive code, database changes, or anything touching multiple files.
- **When to use which tool** — does the team follow a shared orchestration matrix, or is everyone free to choose?

### 2. Quality conventions

- **Review of AI-generated code** — what extra review does it get? AI has specific failure modes that human code usually doesn't: over-engineering, unnecessary error handling, hallucinated APIs, convention drift, premature abstraction. Reviewers should watch for these specifically.
- **Testing** — AI-generated code must have tests. The iteration loop (execute → test → fix → re-test) is what makes AI output trustworthy. Decide: does "AI wrote the tests too" count, or does test code need separate human sign-off?
- **The human decides, the AI implements** — architectural decisions, data model choices, security-sensitive logic. Write down what the team considers "architectural" — this is where teams most often trust AI too much.

### 3. Security conventions

- **What never leaves the local environment** — list explicitly. Typical items: production `.env` values, API keys, customer data, access tokens, database credentials. Non-negotiable.
- **What is safe to share** — code structure, architectural questions, redacted error messages, documentation.
- **Privacy-regulation implications** — if you handle customer data under GDPR, CCPA, HIPAA, or similar: customer data must not leave the local environment. MCP servers that query the database are fine (local). Pasting query results into a chat AI is not. Write this down.
- **Third-party tool evaluation** — what approval process does a new AI tool go through before it gets company data access? Who signs off?

### Write it down

The facilitator updates the team's actual conventions file (kept in-repo, reviewed like any other doc) with the decisions as they're made. No going back to "I thought we agreed X." It's written.

---

## Part B — Advanced Orchestration Patterns (25 min, live demo)

Show the patterns that compound the five-role model for experienced users. Everyone's already seen basic agentic work — now show what a mature workflow looks like.

### Parallel agents — thinking and executing at once

Kick off a research agent in the background while you work on something else.

Example:
- "Research how [library] handles [edge case]" runs in background
- You build the migration in the foreground
- Research returns, you decide whether to adjust based on findings

This only works if your CLI tool supports background / sub-agents. Demo it if yours does.

### Memory systems — continuity across sessions

Tools that persist context across sessions (like Claude Code's memory system) prevent re-learning team conventions every conversation.

Show your memory file. Walk through what's worth storing (feedback, preferences, project quirks) vs. what isn't (ephemeral task state, anything derivable from the code). See [`../reference/common-gotchas.md`](../reference/common-gotchas.md) — memory hygiene is a real problem.

### Custom skills / slash commands — codified workflows

If your CLI tool supports custom commands, demo one your team has built:

- A `/commit` skill that enforces your commit-message style
- A `/test` skill that runs your test suite scoped correctly
- A `/review` skill that runs a structured review against the current diff

Key framing: **skills are how team conventions become tool-level defaults.** Instead of telling every new hire "remember to run Pint first," you embed it in a skill everyone invokes.

### Speech-to-text — quick aside

Voice input for describing tasks. Particularly useful for thinking-out-loud before executing. Five-minute demo is fine; skip if time is short.

### The full orchestration, in practice

Show a compact end-to-end:

```
1. THINK    → chat AI: "How should we model this new feature?"
             → discuss, decide, write down the approach

2. EXECUTE  → CLI agent: "Implement the approach we discussed"
             → creates migration, model, component, tests

3. EDIT     → in-editor AI: surgical fix to a specific line
             → or CLI agent for multi-file refactor

4. REVIEW   → CLI agent /review skill on your own branch before pushing
             → visual verification (browser automation) for UI changes
             → teammate reviews the MR

5. ITERATE  → CLI agent: "Tests are failing, fix and re-run"
             → loop until green, back to REVIEW, then merge
```

This is the target state. Not everyone will get there in six weeks. That's fine.

---

## Part C — Team Orchestration Exercise (40 min, hands-on)

See [`../exercises/session-3-team-orchestration.md`](../exercises/session-3-team-orchestration.md).

A realistic mini-project that forces deliberate role separation across people and tools.

### The task

The facilitator prepared one self-contained feature that can plausibly be split 3 ways. Generic examples:

- **Frontend + backend + tests split**: a new endpoint with a UI consumer and a test suite
- **Schema + logic + migration split**: a new field with business logic and a data migration
- **Feature + docs + rollout split**: a new feature with user-facing docs and a deployment plan

### Phase 1 — Collective thinking (10 min)

Everyone uses a chat AI or a whiteboard to reason about the approach. Share back. Agree on the design *before* anyone touches code.

### Phase 2 — Parallel execution (15 min)

Each person takes one piece using a CLI agent. If your tool supports role-specific agents (e.g., database-architect, frontend-specialist, test-engineer), route each person to the appropriate one.

### Phase 3 — Cross-review (10 min)

Nobody reviews their own code. Swap:

- Person A reviews person B's diff
- Person B reviews person C's diff
- Person C reviews person A's diff

Use the review checklist from Part A conventions. Watch for: over-engineering, convention violations, hallucinated APIs.

### Phase 4 — Debrief (5 min)

- Did the thinking phase actually prevent problems in execution, or did you just draw boxes and then do something else?
- Did the cross-review catch anything?
- Where did role separation help? Where did coordinating slow you down?

---

## Commitments

Before the session ends, each participant commits — **in writing, in the shared doc** — to *one* workflow change they will make permanently.

Examples:
- "I will always write the think-first note before invoking the CLI agent on new features."
- "I will stop pasting stack traces into ChatGPT and use the CLI agent instead."
- "I will set up a `/review` skill for our team this week."
- "I will update `CLAUDE.md` any time I notice a convention the AI missed."

Revisit these in the 2-week follow-up (see [`../reference/post-bootcamp.md`](../reference/post-bootcamp.md)).

---

## Facilitator Notes

### Watch for

- **Part A taking forever.** It can. Budget carefully. If you run long, cut demo in Part B before cutting Part C.
- **Silence on controversial questions.** Some teams avoid conflict by agreeing too quickly. If everyone nods at "we'll never paste production data into ChatGPT," ask: "Has anyone come close? What stopped you?" Real answers reveal what the convention actually needs to address.
- **Someone's pet tool.** The conventions document is not the place to debate which CLI agent is best. If that fight breaks out, table it. The team can have multiple tools fitting the same role.
- **The "we already do this informally" response.** Sometimes true. But informal conventions don't help a new hire, and they don't survive when the culture shifts. Write them down anyway.

### Don't skip

- **The written commitments at the end.** These are the entire point of Session 3. If someone can't articulate one change, they didn't engage with the bootcamp.
- **The shared conventions document.** If it's not in-repo and version-controlled by the end of Session 3, you didn't finish. Make time.

### After the session

Within 48 hours:

1. Post the conventions document to the team chat. Tag everyone who was in the session.
2. Schedule the 2-week follow-up on calendars.
3. Add a reminder to revisit the decision sheet every quarter.
