# Orchestration Decision Matrix

Which tool do you reach for, for which role? This matrix is a starting point, not a verdict.

---

## The matrix (generic)

| Role | Good fit | Why | Common antipattern |
|---|---|---|---|
| **Thinking / Planning** | Chat-based AI (Claude Desktop, ChatGPT), research tools (Perplexity) | Sandboxed, conversational, no side effects. Good for reasoning without risk of accidental changes. | Using an execution-capable tool ("just think out loud with it") — it will try to act, and sometimes will. |
| **Executing** | CLI-based agentic tools (Claude Code, Codex CLI, Aider) | Full filesystem context, shell execution, git integration, test runner access. Understands project via its context file. | Copy-pasting chat-AI output manually into files. You're re-performing a job the tool was designed to automate. |
| **Editing** | In-editor AI (Cursor, Windsurf, Zed AI) *or* CLI-agent | Inline awareness of surrounding code; diffs you can accept/reject. CLI-agent when changes span multiple files. | Editing in a chat tool's code block and pasting back — loses indentation, misses context. |
| **Reviewing** | CLI-agent with a review skill; chat-AI for architectural review; second human | Perspective, not execution authority. Different tool than the one that wrote the code. | Same tool that wrote the code also reviewing it. That's rationalization, not review. |
| **Iterating** | CLI-based agentic tool | Can run tests, read errors, adjust, re-run. The autonomous loop is the whole point. | Manual copy-paste of stack traces into a chat tool. You're paying for context-switching you don't need to pay. |

---

## How to read this

**The fit column names a *category* of tool, not a specific product.** "CLI-based agentic tool" today means Claude Code / Codex CLI / Aider. Three years from now it may mean something else. The principle — a tool with filesystem access, shell execution, and iteration autonomy — will still apply.

**The antipattern column is the one to remember.** Most bad AI workflows fail not because they picked the "wrong" tool for a role, but because one tool is doing multiple roles without anyone noticing.

---

## Example mappings

The same task can map different ways depending on scope:

### "Should I put this logic in a service class or keep it in the controller?"

- **Role:** Thinking (mostly), some Reviewing
- **Tool:** Chat AI, or conversation with a teammate, or a notebook
- **Not:** CLI agent — it will suggest one and start implementing

### "Create the database migration and model for this feature"

- **Role:** Executing
- **Tool:** CLI agent
- **Pre-req:** Role 1 (thinking) already done — you know the schema you want
- **Review:** You read the diff. Does it match what you decided?

### "Refactor this function to be testable"

- **Role:** Editing
- **Tool:** In-editor AI if it's a surgical change in one file. CLI agent if it spans multiple.
- **Watch for:** the AI "improving" more than you asked for. Reject scope creep in the diff.

### "Review this PR for security and code quality"

- **Role:** Reviewing
- **Tool:** CLI agent with a `/review` skill, or chat AI with the diff pasted in (smaller PRs), or a human
- **Not:** the same agent that wrote the PR

### "My Pest test is failing — figure out why and fix it"

- **Role:** Iterating
- **Tool:** CLI agent with shell access
- **Watch for:** the agent changing the test to match broken behavior, instead of fixing the behavior. Ask "did you change production code or test code?" explicitly.

### "Research how library X handles Y edge case"

- **Role:** Thinking
- **Tool:** Perplexity, Claude Desktop, or the library's own docs
- **Watch for:** hallucinated APIs. Verify the cited function/flag actually exists.

---

## The shape of a healthy workflow

```
Thinking    → chat AI or human notebook
   ↓
Executing   → CLI agent (given clear intent from above)
   ↓
Editing     → same CLI agent, or IDE AI for surgical fixes
   ↓
Reviewing   → you + a different tool (CLI /review skill, or chat AI with diff, or another human)
   ↓
Iterating   → CLI agent (has test runner, can fix & re-run autonomously)
   ↓
back to Reviewing → you approve, then merge
```

The handoffs matter as much as the tools. Every arrow is a point where you should be able to answer: "Am I happy to pass this to the next step, or does it need another pass here?"

---

## Anti-prescription

Nothing about this matrix is sacred. A team using only CLI agents (no chat AI, no IDE AI) can work well. A team using only chat AI and manual paste can work — slowly. The *point* is that you've thought about which tool does which role, rather than defaulting to whichever one your team lead opened first.

If you build your own orchestration matrix and it looks different from this one, that's fine. What matters is that the matrix exists and everyone on the team follows it.

## Adapting to your team

Make a copy of this file in your own repo. For each role, write down:

1. The tool your team will use by default
2. When to deviate and use a different one
3. One antipattern you've personally seen in this role

Keep it under one page. Revisit every six months as tools change.
