# Session 2 — Part D Handout: Self-Calibration Exercise

**Time:** 30 minutes

## What this is

You'll run **the same task** two different ways. Once with your default AI workflow, once with the orchestrated workflow you've been practicing.

Then you'll reflect on your own. We are **not** crowning a winning tool. We are not timing the group. We are figuring out where each approach works for *you*, on *your* kind of work.

---

## Why it's set up this way

If we gave everyone the same task and a shared stopwatch, this would turn into a bakeoff. Bakeoffs produce evangelism — "the winner is obviously X, therefore everyone should use X" — and that's not useful. Different tasks favor different tools. The goal is for you to get honest signal about your own workflow, not to cheerlead.

---

## What you need

**Bring a task from your real recent or current work.** Ideally:

- Self-contained (doesn't require a design meeting)
- ~15 minutes to do manually
- Something you have a clear mental model of already — so "the AI did it well" vs. "the AI did it badly" is a real signal, not "I don't know if this is right"

If you didn't bring one, pick from the Session 1 task list or from your last week's closed tickets. Just don't reuse the *exact* task you did in Session 1.

---

## The two runs

### Run 1: Your default workflow (~10 min)

Whatever you normally reach for. Examples:

- Chat AI (ChatGPT, Claude Desktop) + manual paste into your IDE
- In-editor AI (Cursor, Copilot, Windsurf) with inline suggestions
- Stack Overflow + your own code
- Pair programming with a colleague (AI-free)

Do the task however you normally would. Note anything you paid conscious attention to.

### Run 2: The orchestrated workflow (~10 min)

1. Chat AI for **thinking**: 1-2 min planning the approach.
2. CLI agent (with the MCP servers you set up in Part C) for **executing + iterating**.
3. Read the diff yourself for **reviewing**.

Don't worry if your MCP servers aren't fully useful for this specific task. The workflow is what matters.

---

## Reflect on your own (5 min, written)

**Do not share these answers yet.** Write them down for yourself first.

1. Where did each approach feel easier? Where did each feel clunky?
2. Which produced output that matched your *intent*, not just "worked"?
3. Did either catch something the other missed — a convention, an edge case, a missing test, a naming issue?
4. For this *type* of task, which would you reach for next time?

**Optional:** how long did each take you end-to-end, including your review time? This is for your notes. We're not comparing.

---

## Group debrief (5 min)

One observation each — something that surprised you about **your** task.

Ground rules for the debrief:

- **Not** "tool X beat tool Y" — the variance between people is the point.
- **Not** "the orchestrated workflow is obviously better" — maybe for your task, maybe not.
- **Yes** "I noticed that when X happened, the orchestrated workflow did Y."
- **Yes** "I think for this *type* of task, I'd use A. For *this other type*, I'd use B."

---

## What success looks like

- You can describe, concretely, when you'd pick one workflow over the other.
- You noticed at least one thing each workflow does differently — not just "faster" or "slower."
- You're less confident about blanket tool rankings and more confident about task-shaped decisions.

If you leave this exercise convinced one tool is always better than another, you probably picked a task that played to that tool's strengths. That's informative too — notice *why* your task favored that tool.

---

## What to bring to Session 3

1. One convention you think the team should adopt (or abandon) based on the last two weeks.
2. One situation where AI did something you didn't want, and how you caught it.
3. Any "I nearly pasted [sensitive thing] into [external tool]" near-misses. No shame — these become team-level rules.
