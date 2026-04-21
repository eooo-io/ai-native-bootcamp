# Session 1 — Part C Handout: Role-Aware Task

**Time:** 45 minutes

## Your job

Pick one task from the list below. Complete it using your CLI agent, but **consciously separate the five roles** as you go.

Don't rush. The point is to *notice* what role you're in at each moment, not to finish quickly.

---

## Task options

*The facilitator has pre-filled this section with 5–6 small backlog tasks suitable for a ~30-minute execute-and-iterate window.*

Criteria a task needs to meet:

- Small: 5–50 lines of diff, 1–3 files
- Clear acceptance criteria
- Has existing tests, or is testable
- Does not touch security-sensitive code or production data
- Does not require decisions about architecture

**[FILL IN WITH 5–6 PREPARED TASKS FROM YOUR BACKLOG]**

Example tasks (replace with real ones):

1. Add a minimum-length validation rule to the comment field on the feedback form.
2. Write a test for `UserService::sendWelcomeEmail` — the method has been tested manually but has no unit test.
3. Refactor `OrderCalculator::calculate` to extract the tax logic into a separate method. Do not change behavior. Tests should still pass.
4. Add a "copied to clipboard" tooltip to the copy button on the share panel.
5. Replace the hardcoded feature flag `ENABLE_X_EXPORT` in three places with a single `config()` call.

---

## The workflow

### Step 1: Think first (3 min)

**Without touching the CLI agent yet**, use a chat AI (or a notebook, or a whiteboard) to reason about:

- What's the approach?
- What files are affected?
- What could go wrong?
- What does "done" look like — how will you verify?

**Write down** a 3-4 line plan. This is role 1.

Yes, even for small tasks. Especially for small tasks — this is where the habit forms.

### Step 2: Execute (20 min)

Open your CLI agent in the repo. Give it the task and your plan. Let it work.

Watch what it does. Don't micromanage — this is role 2 and you are *not* currently in it. Your job here is to observe, not to steer.

If the agent goes clearly off-track, interrupt. Otherwise, let it finish.

### Step 3: Review (5 min)

**Read the diff.** Not skim — read.

Ask yourself:
- Does it match the plan from step 1?
- Did the agent add extras you didn't ask for (unnecessary error handling, new helper functions, scope creep)?
- Did it follow the project-context file's conventions?
- Are there obvious bugs you can spot without running tests?

If something's wrong, go to step 4. If not, you may still have caught something worth noting.

### Step 4: Iterate (10 min)

If tests failed, or review found issues, hand it back to the agent with **specific feedback**. Not "fix it" — something like:

- "The test failed because X. The fix should be in file Y, not Z."
- "You added a try/catch that swallows errors. Remove it — we want errors to surface."
- "The function name doesn't match our convention (camelCase, not snake_case)."

Let it fix and re-run. Review again.

### Step 5: Individual reflection (2 min)

Jot one sentence per role:

- **Thinking:** What did I plan? Did the plan hold up?
- **Executing:** What did the AI do well? What did it do badly?
- **Editing:** Did I need to tweak anything after, or was the diff clean?
- **Reviewing:** What did I catch? What might I have missed?
- **Iterating:** Did the agent's self-correction work, or did I have to steer?

---

## Group debrief (5 min)

When everyone's done, go around the room. Each person answers:

- Did separating the roles change the outcome vs. how you normally work?
- Was the "think first" step useful or annoying?
- What surprised you?

**Honest answers welcome.** If "think first" felt like overhead, say so. If the agent was clunkier than ChatGPT, say so. This is calibration — the team is trying to figure out what actually works, not cheerlead for a new tool.

---

## What success looks like for this exercise

- You notice the difference between "the tests passed" (iteration) and "this is correct" (reviewing).
- You caught at least one thing in the diff that the agent did differently than you planned.
- You can describe, in one sentence, when you'd reach for this workflow vs. your usual one.

Not required: finishing the task. Some tasks won't close in 30 minutes, and that's fine.

---

## What to bring to Session 2

Use the CLI agent on at least 2 more real tasks this week. For each one, note:

- Which role(s) surprised you
- One thing the agent did well, one thing it did badly
- Whether you'd use this workflow again for that task type
