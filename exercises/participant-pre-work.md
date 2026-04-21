# Pre-Work — Before Session 1

**Send this to every participant at least 1 week before Session 1.**

---

You've got a spot in the AI-Native Development Bootcamp. It runs as three 90-minute sessions, one week apart. Total commitment including pre-work and between-session practice is about 9 hours over 4 weeks.

To make Session 1 productive (not a live tech support call), we need you to arrive with your tools installed and working. Please complete the steps below at least **24 hours** before Session 1 — ideally a couple of days out, so there's slack for debugging.

---

## What you need to do

### 1. Install the CLI-agent tool the team will use

*Tool:* **[FILL IN: e.g., Claude Code CLI, Codex CLI, Aider]*

Installation instructions: [FILL IN link]

Verify it works:
- Run `[tool] --version` in a terminal and confirm it prints a version.
- Run `[tool]` in an interactive session and ask it a simple question like "hello" or "what files are here." It should respond and, where relevant, read from your current working directory.

### 2. Authenticate

The tool needs credentials (an API key, an OAuth login, or similar). Follow the login flow the first time you run the tool.

Verify:
- The tool can answer a simple question without an auth error.
- If the tool uses per-workspace limits (token limits, rate limits), you've confirmed you have enough quota to run a handful of tasks in a session.

### 3. Clone the team's main codebase locally

If you don't already have our main repo cloned:

```
git clone [YOUR_REPO_URL]
cd [YOUR_REPO_NAME]
```

If you already have it, make sure it's up-to-date:

```
git pull
```

**Always run the CLI agent from inside the repo directory.** That is how it picks up the team's project-context file automatically.

### 4. Skim the project-context file

Open `[FILL IN: CLAUDE.md, .cursorrules, AGENTS.md, etc.]` in the repo root. You don't need to memorize it — just see what's there. It's the instructions the AI reads at the start of every session.

If the file has stack info, conventions, agent routing rules, or security notes, that's what will keep the AI aligned with how our team actually works. Noticing what's in it now will help you appreciate *why* it exists during the session.

### 5. Bring a real task

Think about something you're currently doing in ChatGPT / Cursor / wherever you usually work. Pick **one small, real task** you want to try with the CLI agent during Session 1. Examples:

- A minor bug you've been putting off
- A validation rule you know you need to add
- A test you've been meaning to write
- A refactor that's been bothering you

**Not a toy problem.** Real work is the whole point. Write down the task in one sentence so you can start quickly when we get to the Part C hands-on.

---

## If something breaks

Flag it in the team chat at least **24 hours before Session 1**. We can:

- Pair you with a teammate who's working so you still get hands-on time
- Help debug before the session
- Have you join as an observer for Parts A and B, then pair for Part C

What we **cannot** do is spend live session time walking through installation for one person while everyone else waits. That's a waste of everyone's time.

---

## Rough time estimate

- Install + auth: 10–20 min
- Repo clone + verify: 5 min
- CLAUDE.md skim: 10 min
- Picking a task: 10 min
- Buffer for when something goes slightly wrong: 15 min

Plan on ~60 minutes total. If it's taking you twice that, you've hit a real blocker — flag it in chat.

---

## What to expect in Session 1

- **Part A (25 min)** — why one tool doing everything is a bad idea, and the five roles framework.
- **Part B (20 min)** — tour of the CLI agent against our codebase. We'll verify your install is working here.
- **Part C (45 min)** — you'll use your CLI agent on the real task you brought.

If that's all you have time to absorb today, that's fine. The pre-work is the only prep you need.

Questions? Drop them in the team chat. See you at Session 1.
