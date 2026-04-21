# Session 3 — Part C Handout: Team Orchestration Exercise

**Time:** 40 minutes
**Group size:** ideally 3 people; 2 works; 4+ should split into sub-teams

## What this is

A mini-project that forces deliberate role separation **across people**, not just across tools. You'll coordinate like a real team, using agentic workflows, with a shared clock.

The goal is to feel what healthy orchestration looks like when more than one human is involved.

---

## The task

*The facilitator has pre-prepared one feature that can plausibly be split 3 ways.*

**[FILL IN: facilitator's chosen feature description]**

Example splits (pick one based on your feature):

### Split A — Schema + Backend + Tests
- **Person 1**: Database migration and model layer
- **Person 2**: Backend logic / service / endpoint
- **Person 3**: Test suite (unit + integration)

### Split B — Backend + Frontend + Docs
- **Person 1**: API endpoint and business logic
- **Person 2**: UI component that consumes it
- **Person 3**: User-facing docs + release notes

### Split C — Feature + Telemetry + Rollout
- **Person 1**: Feature implementation
- **Person 2**: Metrics / logging / observability
- **Person 3**: Feature flag wiring + rollout plan

---

## The four phases

### Phase 1 — Collective thinking (10 min)

**All together.** Use a chat AI, whiteboard, or just talk. Figure out:

- What are the acceptance criteria?
- What's the interface between the three pieces? (API shape, function signatures, event names)
- What could go wrong at the seams?
- What does "done" look like for each piece?

**Write it down.** Shared doc, whiteboard photo, pinned chat message — something everyone can refer to during Phase 2.

The temptation here is to draw boxes and move on. Resist. The interfaces matter. If you mis-align on the shape of something between Phase 2 pieces, you'll find out too late.

### Phase 2 — Parallel execution (15 min)

Each person takes their piece. Use your CLI agent. If your tool supports specialized sub-agents (e.g., database-architect, frontend-specialist, test-engineer), route your piece to the appropriate one.

Rules:

- **Nobody reads anyone else's code yet.** You're each trusted to deliver your piece against the interface agreed in Phase 1.
- **You are responsible for the tests of your own piece.** Even if you're "the tests person" for another piece, you also need sanity tests for yours.
- **When you're done, don't touch it further.** Go help someone, or wait.

### Phase 3 — Cross-review (10 min)

Everyone reviews someone **else's** piece. Nobody reviews their own.

Rotation:
- Person A reviews Person B's diff
- Person B reviews Person C's diff
- Person C reviews Person A's diff

Use the review checklist your team agreed in Part A of this session. Specifically watch for:

- **Over-engineering** — extra abstractions, premature flexibility, unused parameters
- **Convention violations** — naming, file layout, error handling patterns
- **Hallucinated APIs** — the tool invoked a function or field that doesn't exist
- **Missing tests** — the happy path is covered, the edge cases aren't
- **Scope creep** — the diff does more than the plan asked for

Post comments/questions back. Original author addresses them.

### Phase 4 — Debrief (5 min)

Everyone answers:

- **Did the thinking phase actually prevent problems** in execution? Or did you draw boxes and then ignore them?
- **Did the cross-review catch anything** that self-review wouldn't have?
- **Where did role separation help?** Where did coordinating slow you down vs. just doing it yourself?
- **What would you do differently** if you ran this exercise again?

---

## What success looks like

- The three pieces integrate without major rework.
- Each person's review of someone else's code found something worth addressing — even small things.
- The team could describe, for a future real feature, when to split like this and when not to.
- Nobody says "I would've finished faster alone" as a complaint — if they say it as an observation, that's valid and worth discussing.

---

## What likely happens (and why it's fine)

**Interfaces drift in Phase 2.** Person B and Person C realize they interpreted the Phase 1 plan differently. This happens in real teams every week. It's good practice to notice it happening and recover in Phase 3 rather than ignore it until production.

**One person finishes way early.** They should help someone else or start pre-reviewing their own piece extra carefully. Not sit and wait.

**One person gets stuck.** Phase 2 is parallel but not solo — ask for help from someone who's done. Or pair briefly. The rule is "nobody touches someone else's code" not "nobody talks."

**The AI does something unexpected in one person's piece.** Good — this is a normal occurrence. Phase 3 is where you catch it. If Phase 3 misses it too, you're about to learn something about your review checklist.

---

## Commitment

After this exercise, write down — in the team's shared conventions doc — one commitment for how you'll approach collaborative work with AI going forward. Not a vague "use AI more deliberately." Something specific:

- "Every multi-person feature gets a 10-min Phase-1 style alignment before execution."
- "Every AI-generated piece gets reviewed by someone who didn't touch it."
- "I will catch myself before skipping the write-it-down step."

You'll revisit these at the 2-week follow-up.
