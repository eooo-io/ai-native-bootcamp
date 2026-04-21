# Session 1 — Foundations: "Stop Collapsing the Roles"

**Duration:** 90 minutes
**Format:** 25 min slides/discussion + 20 min live demo + 45 min hands-on

## Prerequisites

Participants have completed [`../exercises/participant-pre-work.md`](../exercises/participant-pre-work.md) at least 24 hours before this session. That means:

- CLI-agent tool installed and authenticated
- Repo cloned locally
- Project-context file (e.g., `CLAUDE.md`) skimmed
- One real task identified to bring to Part C

If someone's setup is broken, flag it in team chat ≥24h out. No live-session install support.

## Goal

Shift mindset from "ask AI a question" to "assign AI a role." Everyone leaves with the installed toolchain verified and a role-separated practice task under their belt.

---

## Part A — The Collapsing Roles Problem (25 min, slides + discussion)

### Open with the antipattern

```
Developer has a task →
  Opens ChatGPT →
    Describes the problem →
      Gets a code block →
        Pastes it into the file →
          Manually fixes what's wrong →
            Hopes it works

This is AI-assisted. It is not AI-native.
```

### Introduce the five roles

```
1. Thinking / Planning    — What should we build? Why? What are the tradeoffs?
2. Executing              — Write the code, create the files, run the migrations
3. Editing                — Modify existing code, refactor, adjust
4. Reviewing              — Is this correct? Secure? Following conventions?
5. Iterating              — Tests failed → fix → re-run → repeat until green
```

**The key insight:** when one tool does all five, you lose the ability to verify any single step. You have not leveraged responsibility — you have delegated it and then forgotten you delegated it.

See [`../framework/five-role-model.md`](../framework/five-role-model.md) for the full argument — send it as a follow-up.

### Show the same task done three ways

Pick a small, illustrative task. Examples (adapt to your stack):

- Add a validation rule to an existing form
- Rename a method across 3 call sites
- Add a nullable column to a model

Walk through:

1. **Chat-AI way:** paste code, get block, paste back, manually adapt. Note every manual step.
2. **IDE-inline way:** inline suggestion, accept/reject in the open file. Note what the tool *didn't* see (sibling files, conventions, tests).
3. **CLI-agent way:** tool reads the project-context file, looks at sibling patterns, writes the change, runs the linter, runs the test. You review the diff.

Time each. Compare the *quality* of the output — not just whether it compiled. Discuss what was verified vs. assumed at each step.

### Discussion questions

- Which of the five roles did each tool perform? Which did it *not* perform?
- In the chat-AI workflow, who performed role 4 (reviewing)? Did they have the context to do it well?
- What would make you *more* confident the result was correct?

---

## Part B — Tour of CLI Agent Against Your Codebase (20 min, live demo)

Install is pre-work. This is orientation.

### Install check (3 min)

Everyone runs a smoke test:

- `your-cli-tool --version`
- `your-cli-tool ask "what is this project?"` (or equivalent)

This confirms auth works and the project-context file is being loaded. If anyone's install is broken, pair them with a participant whose is working for Part C.

### Operations vs. suggestions (12 min)

Demonstrate what makes a CLI agent different from chat AI and inline IDE AI:

- **Read a file** the agent has not been told about explicitly — watch it find the right one.
- **Ask about a specific function's callers** — show how it greps, reads, and summarizes, not just one-shot answers.
- **Ask to run a command** — a test, a lint, whatever your toolchain uses. Watch the agent interpret output.
- **Do a small multi-file edit** — e.g., rename a function and update all call sites. Contrast with an IDE AI that stays in one file.
- **Show a scoped-skill invocation** — e.g., a `/test` or `/review` command if your tool supports them.

### Project-context file walk (5 min)

Open your team's context file (`CLAUDE.md`, `.cursorrules`, `AGENTS.md` — whatever your tool uses). Walk the sections:

- Stack/framework versions
- Directory structure
- Naming conventions
- Testing conventions
- Agent routing rules (if you've configured them)

**The meta-point:** this file is the mechanism that makes a CLI agent "know" your codebase. Maintaining it is now part of your team's workflow, like documentation.

### Framing

Reinforce: this tool is an **execution + iteration tool**. Thinking (chat AI, notebook, hallway conversation) and reviewing (the human) stay yours.

---

## Part C — First Exercise: Role-Aware Task (45 min, hands-on)

See [`../exercises/session-1-role-aware-task.md`](../exercises/session-1-role-aware-task.md) for the handout.

Each participant picks one of the 5–6 prepared backlog tasks (you prepared these in the Preparation Checklist — don't skip that step, or this becomes chaos).

### Structure

1. **Think first (3 min)** — use a chat AI or a notebook. What's the approach? Write it down before touching code. This is the one step everyone wants to skip; insist on it.
2. **Execute (20 min)** — CLI agent implements. Watch the diffs roll in.
3. **Review (5 min)** — read the diff yourself. Does it match what you planned in step 1? Did it add extras you didn't ask for?
4. **Iterate (10 min)** — if tests fail or something's off, hand it back to the agent with specific feedback. Let it fix and re-run.
5. **Individual reflection (2 min)** — jot down one sentence per role: what went well? what didn't?

### Group debrief (5 min)

Go around the room. Each person answers:

- Did separating the roles change the outcome vs. how you normally work?
- Was the "think first" step useful or annoying?
- What surprised you?

**Honest answers welcome.** This is calibration, not evangelism. If someone says "separating felt like overhead," that's a legitimate data point. Record it. Revisit in Session 3.

---

## Takeaway Assignment (before Session 2)

1. **Use the CLI agent for at least 2 real tasks this week.** Not toy tasks — actual work.
2. **For each task, consciously note which role you're in at each moment.** Write it down.
3. **Note one thing that surprised you, good or bad.** Bring it to Session 2 Part A.

---

## Facilitator Notes

### Watch for

- **The "let me just try" urge during Part A.** Someone will want to open their CLI agent mid-discussion and start executing. Redirect — Part A is thinking, hands off keyboards.
- **Install failures in Part B despite pre-work.** Have a backup: pair-programming setup, or pre-provisioned machine.
- **Over-scoping in Part C.** If a task is too big, it won't finish in 30 min of execute+iterate time. Pick tasks that are small on purpose.
- **Dismissive "this is just a better autocomplete" framing.** The five-role model is the point. Push back gently — what role is the person performing when they accept an autocomplete suggestion? Who's reviewing?

### Flex points

- If Part A feels stale, compress to 20 min and give Part C more time.
- If half the room has setup problems, pair aggressively in Part C — you'll cover fewer people, but everyone gets a real experience.
- If a heated discussion breaks out in the debrief, let it run. Cut Part C short next session rather than cut the debrief short here.

### Red flags in the debrief

- "I didn't really plan, I just let the tool go." → They skipped role 1. Address in Session 3 conventions.
- "I didn't review the diff, I just accepted." → They skipped role 4. Red flag.
- "The tests passed so it's fine." → Role 4 and role 5 are being conflated. Use this as a setup for Session 2's iteration-loop demo.
