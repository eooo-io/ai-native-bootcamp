# Session 2 — Agentic Workflows: "Controlled Iteration with Leverage"

**Duration:** 90 minutes
**Format:** 15 min review + 20 min demo + 25 min hands-on setup + 30 min exercise

## Prerequisites

- Session 1 complete
- Each participant used their CLI agent on ≥2 real tasks in the past week
- Each participant has a task in mind for Part D (self-calibration exercise)

---

## Goal

Move from single-turn interactions to multi-step autonomous workflows. Introduce MCP servers, agent routing, and the deliberate iteration loop.

---

## Part A — Review & Discussion (15 min)

Ground the session in real experience before introducing new concepts.

### Go around the room

Each person answers briefly:

- Where did role separation help in the past week?
- Where did it feel like overhead?
- What did the CLI agent do that surprised you — positively or negatively?
- One thing you'd do differently now.

### Listen for

- **"I ended up collapsing roles because it was faster for a small task"** — fine, that's realistic. Discuss when collapse is OK vs. not in Session 3.
- **"The tool did something I didn't intend"** — great material. What went wrong? Did the team member review the diff before the damage was done?
- **"I didn't really need the agent for this, a chat AI would have been faster"** — also great. The tool-for-role principle working as designed.

---

## Part B — The Iteration Loop (20 min, live demo)

The most powerful capability of agentic tools is not code generation — it is **autonomous iteration.**

### Demo: build a small feature end-to-end

Pick something that plausibly fits in 15 minutes of AI execution. Examples:

- "Create a new endpoint that returns X with tests"
- "Add a field to this model, handle the migration, update the serializer, add a test"
- "Implement rate-limiting middleware for the /public route"

Run it live. Narrate what's happening:

1. The agent plans.
2. The agent executes.
3. The agent runs the tests.
4. A test fails.
5. The agent reads the failure, adjusts, re-runs.
6. You review the final diff.

**Point out the iteration moment (step 5).** That is the loop that manual copy-paste workflows cannot replicate. This is the whole reason "agentic" is a category.

### Key concepts to name

- **Agent routing / specialized agents** — if your CLI tool supports sub-agents (domain-specific like database, frontend, testing), demo routing a task to one. Show how it changes the quality of the output.
- **Plan mode** — for complex tasks, have the tool plan before executing. Explicit role separation: role 1 and role 2 happen in distinct steps instead of blurred into one.
- **Trust but verify** — reviewing is yours. Read the diff. Read the test output. Do not blindly accept. The agent iterates; you approve.

### Demo what *not* to do

Optionally: show a scenario where the agent gets stuck in a fix-break-fix-break loop. Either it keeps breaking the same test, or it "fixes" the test by changing the assertion instead of the production code. Show how to intervene:

- Ask explicitly: "Did you change the production code or the test code?"
- Revert with git and restart with clearer constraints
- Sometimes: step away from the agent and use a chat AI to think about what's wrong

This is a feature, not a bug, of the curriculum — participants need to see agentic tools fail and see how you recover.

---

## Part C — MCP Servers: Extending the Capability Boundary (25 min, demo + setup)

Return to [`../framework/capability-boundaries.md`](../framework/capability-boundaries.md). MCP servers are what extend a tool's reach beyond its default boundaries.

### Demo at least one MCP server

Choose based on what your team actually needs. Common starting points:

- **VCS host MCP** (GitHub, GitLab) — lets the agent read MRs/PRs, check pipelines, post comments, create branches. Brings reviewing into the agentic loop.
- **Documentation / docs-search MCP** — searches version-specific docs for your stack. Reduces hallucinated API calls.
- **Database MCP** — queries and schema inspection. Lets the agent confirm assumptions about data before writing migrations.

### Live demo a workflow that spans multiple roles

Example with a VCS host MCP:

> "Review MR #X, check why the pipeline failed, and suggest a fix."

The agent:
1. Fetches the MR diff (executing)
2. Fetches the pipeline logs (executing)
3. Analyzes what's failing (thinking + reviewing)
4. Proposes a fix (executing)
5. Optionally commits and pushes (executing + iterating)

This is one task that naturally spans roles 2, 3, 4, 5 — with MCP collapsing the mechanics but *not* the accountability. You still review the final diff before merge.

### Help everyone connect one MCP server

Use the remaining time to get each participant connected to at least one MCP server.

**Mention the common gotchas up front** (from [`../reference/common-gotchas.md`](../reference/common-gotchas.md)):

- MCP servers inherit the CLI's environment at spawn time. If you rotate a secret (e.g., a PAT), restart the CLI or the server keeps the old value.
- Project-level MCP config files (`.mcp.json`, `.cursor/mcp.json`, etc.) may be gitignored but still tracked from earlier commits. Check `git ls-files` before hardcoding secrets.
- A 401 from an MCP tool typically means "auth drift," not "broken server."

---

## Part D — Self-Calibration Exercise: Pick Your Own Comparison (30 min)

See [`../exercises/session-2-self-calibration.md`](../exercises/session-2-self-calibration.md).

The goal is **not** to crown a winning tool. It's to let each person feel the difference between their default workflow and an orchestrated one, on something they actually care about. A prescribed task with a shared stopwatch turns into a bakeoff, and bakeoffs produce evangelism, not calibration.

### Setup (this is pre-work, should be done already)

Each participant brought a task from their current or recent work — self-contained, roughly 15 minutes manually.

### Run the task twice (20 min)

1. **Default workflow** — whatever they normally reach for (chat AI + IDE, in-editor AI, etc.)
2. **Orchestrated workflow** — chat AI for thinking, CLI agent for executing + iterating, human review of the diff

No shared stopwatch. Timing your own runs for your own notes is fine — it's not what we're comparing.

### Reflect individually (5 min, written)

- Where did each approach feel easier? Where did each feel clunky?
- Which produced output that matched your *intent*, not just "worked"?
- Did either catch something the other missed — a convention, an edge case, a missing test?
- For this *type* of task, which would you reach for next time?

### Group debrief (5 min)

One observation each — something that surprised them about **their** task.

**Not** "tool X beat tool Y." The variance between people's observations is the point. If everyone had the same experience, the tool-picking decision would be easy, and it isn't.

---

## Takeaway Assignment (before Session 3)

1. **Use the CLI agent with at least one MCP server for real work this week.**
2. **Try a review-style skill** — `/review-mr`, a code-review agent, or equivalent — on a real PR.
3. **Try asking the CLI agent to help with a task that normally takes you 30+ minutes.** Note how long it actually took, including your review time.
4. **Come to Session 3 with one convention you think the team should adopt** (or abandon).

---

## Facilitator Notes

### Watch for

- **Part B demo going sideways.** Have a backup: a pre-recorded version, or a second known-working task. Don't spend 20 min trying to unstick a live demo.
- **MCP setup taking too long in Part C.** If more than a third of the room is stuck, freeze there and have the unstuck pair help the stuck ones. Don't push through hoping everyone catches up.
- **Part D turning into a leaderboard anyway.** If someone starts comparing times, redirect: "Was your *task* faster or was the *tool* faster? Would the time difference matter on a bigger task?"
- **The person who says "I already do all this."** They might. Or they might not. Ask them to share one specific workflow. Often the specific workflow reveals collapsed roles.

### Flex points

- If Part B's demo succeeds cleanly and fast, use the saved time to do the "what not to do" demo (the stuck-loop scenario). It's valuable.
- If Part C runs long, cut Part D's "run twice" to "run once with the orchestrated workflow, compare from memory to your default." Less rigorous but still teaches the point.
- Part A can compress to 10 min if nobody has much to share. Do not skip it — even a quiet debrief establishes that real experience counts here.

### What to record

Start keeping notes on what worked and what didn't for Session 3's accountability framework:

- Pain points the team hit — these become rules ("always run Pint after AI edits")
- Patterns worth sharing — these become templates
- Security incidents, even minor ("I nearly pasted the .env into ChatGPT but caught myself") — these become boundaries
