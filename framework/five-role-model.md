# The Five-Role Model

The central thesis of this bootcamp.

---

## The problem

A developer has a task. They:

```
Open ChatGPT →
  Describe the problem →
    Get a code block →
      Paste it into the file →
        Manually fix what's wrong →
          Hope it works
```

This is **AI-assisted** development. It is not **AI-native** development. And the failure mode is not "AI is bad at coding" — it is that a single tool just performed five different jobs with no separation between them:

1. It reasoned about the approach.
2. It wrote the code.
3. It edited the code.
4. It decided the code was correct.
5. It implicitly committed to that decision.

The developer verified *none* of those five steps. They moved to the next task believing "AI wrote it" is a substitute for "I understood it."

---

## The five roles

Every development task — from a two-line bug fix to a multi-week feature — cycles through these roles:

| # | Role | What it answers |
|---|---|---|
| 1 | **Thinking / Planning** | What should we build? Why? What are the tradeoffs? What can go wrong? |
| 2 | **Executing** | Write the code. Create the files. Run the migrations. |
| 3 | **Editing** | Modify existing code. Refactor. Tighten. Fit conventions. |
| 4 | **Reviewing** | Is it correct? Secure? Following team conventions? Does it solve the real problem? |
| 5 | **Iterating** | Tests failed → diagnose → fix → re-run → repeat until green (and then review again). |

These are not linear. You loop. Thinking happens during reviewing. Editing happens during iterating. The roles are not about sequence — they are about **accountability**.

---

## Why separating them matters

**Verification.** When one tool does all five, you can't verify any single step. The AI's confidence in role 4 ("this looks correct") is influenced by the fact that it also did roles 1, 2, and 3. It's grading its own work. That's not review — that's rationalization.

**Different roles need different properties from a tool.**

- Thinking benefits from a **sandboxed, conversational** tool. No accidental side effects. You want friction if you're about to make a bad decision.
- Executing benefits from **full codebase context** and **shell access**. The tool should see sibling files, understand conventions, and run tests.
- Editing benefits from **in-context modification**. A tool that shows the diff and lets you accept/reject is better than one that rewrites a file whole.
- Reviewing benefits from **fresh perspective**. A tool that didn't write the code is better at catching its problems. Same principle as code review between humans.
- Iterating benefits from **autonomy**. The tool should be able to run, see the error, adjust, and re-run — without you copy-pasting stack traces.

No single tool excels at all five. That is not a failure of tooling — it is an architectural reality. Pretending otherwise is how you end up with AI that produces confident, well-formatted, superficially-correct, wrong code.

**The human's responsibility is sharpened, not blurred.** This is the counterintuitive point. An AI-native workflow does not reduce accountability — it *increases* it, by making every handoff explicit. You know exactly what you delegated to which tool, and you know which step you personally owned. If something ships broken, you can trace which role failed.

---

## What "AI-native" actually means

The typical progression:

```
AI-hostile    → "I'll write it myself, faster than fighting with AI."
AI-curious    → "I'll paste this into ChatGPT and see what it says."
AI-assisted   → "I use ChatGPT / Copilot for most code now. It's fine."
AI-native     → "I deliberately assign tools to roles. I still review."
AI-abdicated  → "I let it run and commit. It works most of the time."
```

**AI-native ≠ using AI more.** It means using AI **more deliberately**. Sometimes that's *less* AI — because one role is better done by a human thinking with a notebook.

The line between AI-native and AI-abdicated is where most teams struggle. Both use agentic workflows. Both let AI run long-horizon tasks. The difference:

- AI-native teams **review diffs, read test output, and reject work that's wrong** even when it compiles.
- AI-abdicated teams **merge because "tests pass"** and find out the tests were missing the case that mattered.

The five-role model exists to keep teams on the right side of that line.

---

## Common antipatterns

| Antipattern | What's happening | What to do instead |
|---|---|---|
| "Just ask ChatGPT" for everything | All five roles collapse into one tool | Explicitly name which role you need — if it's thinking, ChatGPT is fine. If it's executing, you want a tool with codebase context. |
| CLI agent writes and reviews its own code | Role 4 is theater | Use a separate tool, another agent, or a human for review. Build the review step into the workflow. |
| "Tests pass, ship it" | Role 4 skipped because role 5 succeeded | Reviewing checks intent and correctness; iterating just gets tests green. They are not the same. |
| Copy-pasting stack traces back to ChatGPT | Manual role-5 iteration with a non-agentic tool | Move to a tool that has terminal access. You're paying for cognitive context-switching that agentic iteration removes. |
| "The AI plans the architecture" | Role 1 delegated to a tool with no skin in the game | Thinking is yours. Use AI to *explore* options, not decide them. |

---

## When to collapse roles (yes, sometimes)

Role separation has overhead. For trivial changes, it's not worth it.

**It's fine to collapse roles when:**

- The change is smaller than ~20 lines
- You already know exactly what you want
- The affected code has test coverage you trust
- You will personally eyeball the diff before commit

**It is not fine to collapse roles when:**

- Any security-sensitive code is involved
- The change spans more than one file
- You don't fully understand the existing code being changed
- The code has no tests
- You are committing to a production branch

Default to separation. Collapse consciously, not by habit.

---

## The core principle

> AI is leverage for experienced engineers, not a replacement. The best AI workflows make responsibility **sharper**, not softer.

Everything else in this bootcamp — orchestration matrices, MCP servers, team conventions, security rules — is implementation detail for that principle. If you remember nothing else, remember this: every role you hand to a tool is a role you have to verify was performed correctly. Your job is not to write less code. Your job is to own the outcome.
