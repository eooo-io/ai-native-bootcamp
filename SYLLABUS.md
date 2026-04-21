# Syllabus

**AI-Native Development Bootcamp** · Three 90-minute sessions · One week apart

---

## Learning Outcomes

By the end of this bootcamp, every participant should be able to:

1. **Name the five roles** (Thinking, Executing, Editing, Reviewing, Iterating) and describe what goes wrong when one tool performs all five.
2. **Choose the appropriate tool for a given role** on a real task — and explain *why* that tool, not just "because it's AI."
3. **Run an agentic workflow end to end** using a CLI-based AI tool with MCP (or equivalent) server integrations on their own codebase.
4. **Review AI-generated code for its specific failure modes** — over-engineering, convention violations, hallucinated APIs, missing tests.
5. **Apply the team's agreed security and quality conventions** when AI is in the loop, including what never gets pasted into an external tool.
6. **Decide when AI is the wrong choice** for a given task.

---

## Pre-requisites

- Working familiarity with the team's codebase (you can build, run, and test it locally).
- Some exposure to AI coding tools (ChatGPT, Copilot, or Cursor). Heavy daily usage is not required.
- Ability to install CLI tools on your machine (admin rights on work laptop, package manager available).
- A real, self-contained task you're currently procrastinating on. You'll use it in Session 2.

No prior experience with agentic AI tools or MCP servers is assumed. Part of Session 1 is making sure everyone has them installed.

---

## Format & Time Commitment

| Block | Time | Who |
|---|---|---|
| Participant pre-work | ~60 min | Each participant, on their own time, ≥1 week before Session 1 |
| Session 1: Foundations | 90 min | Whole team |
| Between-session practice (S1 → S2) | ~2 hours | Each participant, on real work |
| Session 2: Agentic Workflows | 90 min | Whole team |
| Between-session practice (S2 → S3) | ~2 hours | Each participant, on real work |
| Session 3: Team Conventions | 90 min | Whole team |
| 2-week follow-up | 30 min | Whole team |
| **Total commitment per person** | ~9 hours | |

---

## Session 1 — Foundations: "Stop Collapsing the Roles" (90 min)

**Goal:** shift mindset from "ask AI a question" to "assign AI a role." Everyone leaves with the installed toolchain from pre-work verified and a role-separated practice task under their belt.

- **Part A (25 min)** — The collapsing-roles antipattern. Introduce the five-role model. Walk through the same task done three ways (ChatGPT way, IDE-inline way, CLI-agent way) to illustrate what verification you gain or lose at each step.
- **Part B (20 min)** — Tour of your CLI-agentic tool against your codebase. Install check, operations vs. suggestions, project-context file walkthrough.
- **Part C (45 min)** — Hands-on role-aware task. Each person picks from a list of 5–6 prepared backlog tasks. Consciously separate think → execute → review → iterate. Group debrief.
- **Takeaway:** ≥2 real tasks completed between sessions using the role-separated workflow.

See: [`sessions/01-foundations.md`](sessions/01-foundations.md)

---

## Session 2 — Agentic Workflows: "Controlled Iteration with Leverage" (90 min)

**Goal:** move from single-turn interactions to multi-step autonomous loops. Introduce MCP servers, agent routing, and deliberate iteration.

- **Part A (15 min)** — Review & discussion of the past week's real usage. Where did role separation help? Where was it overhead?
- **Part B (20 min)** — The iteration loop, live demo. Build a small feature end-to-end: AI plans, executes, tests, sees the failure, fixes, tests again. Point out the moments a non-agentic workflow would have required manual intervention.
- **Part C (25 min)** — MCP servers as capability extensions. Set up at least one (e.g., your VCS host, a documentation server). Demonstrate how it brings the review role into the agentic loop.
- **Part D (30 min)** — Self-calibration exercise. Each person picks their own task (no prescribed task, no shared stopwatch) and runs it two ways — default workflow vs. orchestrated. Reflect individually, then share one observation each.
- **Takeaway:** each person uses their agentic tool + MCP on a task that normally takes them 30+ minutes.

See: [`sessions/02-agentic-workflows.md`](sessions/02-agentic-workflows.md)

---

## Session 3 — AI-Native Team Workflows: "Responsibility, Not Automation" (90 min)

**Goal:** establish team conventions. Decide *together* who is accountable for what, and what boundaries you will and will not cross.

- **Part A (25 min)** — The accountability framework. Agree on workflow, quality, and security conventions. Use the decision sheet in [`templates/conventions-decision-sheet.md`](templates/conventions-decision-sheet.md).
- **Part B (25 min)** — Advanced orchestration patterns live demo. Parallel agents, memory across sessions, custom slash commands / skills, the full 5-role loop in practice.
- **Part C (40 min)** — Team exercise: three participants, three roles (e.g., schema + component + tests), one shared feature. Deliberate role separation across people and tools. Cross-review. Debrief.
- **Takeaway:** each person commits, in writing, to *one* workflow change they will make permanently. Team's Session 3 conventions live in a shared, version-controlled document from this day forward.

See: [`sessions/03-team-conventions.md`](sessions/03-team-conventions.md)

---

## Preparation Checklist (for the facilitator)

### Content & Materials (anytime before pre-work goes out)

- [ ] Prepare 5–6 small practice tasks from your backlog suitable for Session 1 Part C — see `exercises/session-1-role-aware-task.md` for criteria
- [ ] Prepare one self-contained feature for Session 3's team exercise — one that can plausibly be split 3 ways
- [ ] Set up a shared document where the team logs their between-session experiences (Notion, a wiki page, whatever sticks)
- [ ] Prepare a walkthrough of your project-context file (e.g., `CLAUDE.md`) explaining *why* each section exists
- [ ] Fill in [`templates/conventions-decision-sheet.md`](templates/conventions-decision-sheet.md) as a starting point the team modifies in Session 3
- [ ] Print or share [`framework/orchestration-matrix.md`](framework/orchestration-matrix.md) for reference during all sessions

### Pre-Work Rollout (1 week before Session 1)

- [ ] Send [`exercises/participant-pre-work.md`](exercises/participant-pre-work.md) to every participant
- [ ] Have a backup plan if someone's setup breaks: pair them with a participant whose setup works for Part C
- [ ] Verify each person's agentic tool install is working ≥48 hours before Session 1

### Dry Run (3–4 days before Session 1)

- [ ] Run Session 1 Part B + Part C with one team member as a pilot
- [ ] Note what broke, what dragged, what landed — especially where your assumed knowledge became a blocker for them
- [ ] Revise timings, examples, and framing based on what actually happened
- [ ] Add any surprising blockers to [`reference/common-gotchas.md`](reference/common-gotchas.md) so they become live-session talking points instead of live-session surprises

---

## Tools Assumed (Swap as Needed)

This curriculum references specific tools as examples. They are illustrative, not prescriptive. What matters is the **role** each tool fills, not the tool itself.

| Role | Example tools (as of 2026) |
|---|---|
| Thinking / Planning | Claude Desktop, ChatGPT Plus, Perplexity for research |
| Executing | Claude Code CLI, Codex CLI, Aider |
| Editing | Cursor, Claude Code CLI, Windsurf, Zed AI |
| Reviewing | CLI-agent with `/review-mr`-style skill, Claude Desktop for architectural review, browser-automation tools |
| Iterating | Whichever CLI agent has access to your test runner and source |

The bootcamp works if your team's tools differ. Replace references in the session materials with your stack's equivalents before running — or keep them as-is and tell participants to map as they go.

See [`framework/capability-boundaries.md`](framework/capability-boundaries.md) for how to evaluate a new tool against the five roles.

---

## What Success Looks Like

Two weeks after Session 3, a team that absorbed the bootcamp shows:

- Nobody is pasting production data, `.env` contents, or customer information into external AI tools. Full stop.
- The project-context file (`CLAUDE.md` or equivalent) is being maintained like documentation — reviewed in PRs, updated when conventions change.
- Pull request descriptions reference which roles AI filled and which stayed human.
- At least one team member has built or adapted a custom agentic skill / slash command for a recurring workflow.
- When someone says "I'll get AI to do it," the team's shared reflex is "to do *which part* of it?"

If none of that shows up four weeks out, the bootcamp didn't land. Read [`reference/post-bootcamp.md`](reference/post-bootcamp.md) for how to diagnose and rerun.
