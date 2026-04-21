# Capability Boundaries

Understanding what each tool can and cannot *reach* prevents mismatched expectations. Tools look interchangeable on their marketing pages and behave wildly differently in practice.

---

## The capabilities that actually matter

| Capability | What it means | Why it matters |
|---|---|---|
| **Filesystem access** | Can the tool read and write arbitrary files on your machine? | Determines whether it can understand your codebase as a whole or only what you paste in. |
| **Shell execution** | Can the tool run commands (tests, builds, git, linters) and read the output? | Determines whether iteration is autonomous or manual. |
| **Project context loading** | Does the tool read a project-level config / instruction file (e.g., `CLAUDE.md`) at session start? | Determines whether conventions are enforced automatically or you re-type them each session. |
| **MCP / plugin support** | Can you add new capabilities at runtime via a standard protocol? | Determines whether the tool's ceiling is fixed or extensible. |
| **Git integration** | Does the tool understand commits, branches, diffs? | Determines whether reviewing and iterating can happen without context-switching. |
| **Multi-file awareness** | Can the tool modify multiple files consistently in a single operation? | Determines fitness for refactors and new-feature creation. |
| **Agentic loop** | Can the tool run → observe output → correct → re-run, without you mediating each step? | Separates iteration-capable tools from single-turn tools. |

---

## Tool categories (as of 2026)

This is a snapshot. Tool names will change. The **categories** are the durable part.

### Category 1: Chat-based AI (no filesystem / no shell)

Examples: ChatGPT web/desktop, Claude.ai web, Claude Desktop app.

- Filesystem: None (or via MCP in some desktop apps)
- Shell: None (by design)
- Project context: None unless you paste it
- MCP: Varies — Claude Desktop supports it; ChatGPT has Projects but no arbitrary MCP

**Best for:** Thinking. Research. Architectural discussion. Writing prose. Answering general questions.

**Not for:** Executing work in a codebase. Every minute you spend copy-pasting between a chat tool and your editor is cognitive overhead an agentic tool would eliminate.

### Category 2: In-editor AI (project-scoped filesystem, limited shell)

Examples: Cursor, Windsurf, Zed AI, GitHub Copilot Chat.

- Filesystem: Yes, scoped to open project
- Shell: Limited (terminal in-editor, sometimes agent-mode)
- Project context: Project-specific rules files, increasing support for MCP
- Multi-file: Yes, but typically single-editor-instance scope

**Best for:** Editing, inline suggestions, single-session refactors, acceptance-based workflows where you approve each change.

**Not for:** Long-horizon autonomous tasks. The review-accept loop on every suggestion is human-in-every-step by design.

### Category 3: CLI-based agentic (full filesystem + shell)

Examples: Claude Code CLI, Codex CLI, Aider, various open-source agentic frameworks.

- Filesystem: Full
- Shell: Full
- Project context: Yes, via convention (`CLAUDE.md`, `AGENTS.md`, `.cursorrules` equivalents, project-specific)
- MCP: Yes (first-class or via config)
- Git: Full
- Agentic loop: Yes — the defining property

**Best for:** Executing, Iterating, multi-file work, autonomous task completion with human review at checkpoints.

**Not for:** Pure thinking. It will try to act. If you want to reason without acting, use a chat tool.

### Category 4: IDE-integrated (the middle ground)

Examples: JetBrains AI Assistant, Visual Studio IntelliCode, IDE plugins.

Capability varies widely. Evaluate each against the table above. Many are closer to Category 2 (in-editor) with some Category 3 features.

---

## Why "can do X" doesn't mean "good at X"

A tool can *technically* do a role and still be the wrong choice for it.

**Example: using Claude Desktop (chat) for executing.**

You can paste code in, ask for changes, and paste the output back. It works. It also:
- Misses surrounding file context
- Strips formatting
- Doesn't see your test suite
- Can't run Pint/prettier/etc.
- Requires you to mediate every iteration

It technically did the role. Poorly. A CLI agent would have done it faster and more reliably.

**Example: using a CLI agent for thinking.**

You open Claude Code and ask "should we use pattern X or Y?" It will answer. It may also decide to start implementing one of them. You wanted a conversation; you got a commit.

Both examples illustrate the same principle: **capability is not suitability**. Choose tools that *naturally* fit the role, not tools that *can be coerced* into the role.

---

## Evaluating a new tool

When a new tool enters the market — and they do, constantly — evaluate it against:

1. **Which of the five roles is it best at?** (be specific)
2. **What does its capability table look like** from the list above?
3. **What does it require you to mediate manually?** (every manual step is a failure mode)
4. **How does it fail?** (read the fail mode, not the happy path)

If a tool is a good fit for one or two roles, it earns a place in your orchestration. If it's "pretty good at everything," it's probably the same category as what you already have. Having two of the same thing is not a benefit.

---

## The MCP factor

Model Context Protocol (MCP) servers are how tools escape their default capability boundaries.

A chat AI that can use MCP to query your database, read your issue tracker, or call your internal API has effectively grown a new role. Not always for the better — an execution-capable chat AI is an interesting hazard.

**Useful MCP servers (by role):**

- **Thinking**: documentation search servers (per-stack), issue tracker read access
- **Executing**: database query servers, build/test runners
- **Reviewing**: VCS host integration (PR comments, diff analysis), browser automation (visual verification)
- **Iterating**: same as executing — MCP mostly extends reach, not loop-closure

**Watch for:** capability creep. Adding an MCP server to a chat tool that gives it filesystem write access means it's no longer "just for thinking" — it's now a Category 3 tool pretending to be Category 1. Treat it accordingly.

---

## One-page summary

```
For thinking:      pick a tool that CAN'T act.
For executing:     pick a tool that HAS shell + filesystem.
For editing:       pick a tool that SHOWS you the diff.
For reviewing:     pick a tool that DIDN'T write the code.
For iterating:     pick a tool that LOOPS autonomously.

If your orchestration uses the same tool for two adjacent roles,
make sure you understand the verification you're trading away.
```
