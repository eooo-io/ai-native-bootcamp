# Common Gotchas & Troubleshooting

Real friction teams hit. Flag these early so nobody spends hours diagnosing a known issue.

This list is not exhaustive. Add to it as your team runs into new ones.

---

## MCP Server Authentication / Token Drift

**The problem:** MCP servers inherit environment variables from the process that spawned them. When your CLI tool launches, any MCP server it starts gets a **snapshot** of your environment at that moment.

**Symptom:** A tool call returns 401 "token expired" / "re-authorization required" — but running `echo $YOUR_TOKEN` in a fresh shell shows a valid value. Something has rotated the token (e.g., you edited `~/.zshrc`), but the running MCP server is still holding the old value.

**Fix:** Exit your CLI tool completely. Open a new terminal tab (so `~/.zshrc` is sourced fresh). Restart the CLI tool. The MCP server will respawn with the current environment.

**Don't:** try to "refresh" the MCP server in-place. Most tools don't support hot-reload of MCP env vars.

**Harder problem:** If you hardcoded the token into the MCP config file to avoid env drift, see the next gotcha before you push that change anywhere.

---

## `.mcp.json` / `.cursor/mcp.json` Gitignore Trap

**The problem:** Your team may have an MCP config file listed in `.gitignore` — but *already committed* to the repo from before the ignore rule was added. `.gitignore` only affects *untracked* files. Files already in the index stay tracked forever until explicitly removed.

**Symptom:** You hardcode a secret into `.mcp.json` thinking it's gitignored. Some weeks later, `git add -A` or `git commit -a` picks it up, and you've now leaked a token into your repo.

**Check:** `git ls-files <path-to-config>` — if it returns the path, the file is tracked. `.gitignore` is not protecting you.

**Fix:**

```bash
git rm --cached <path-to-config>
git commit -m "chore: stop tracking MCP config"
git push
```

After this, `.gitignore` takes effect. Local copies stay untouched on every developer's machine. Fresh clones will not get the file.

**This trap is almost universal** — most teams have at least one gitignored-but-still-tracked config file. Run the check on every config file that could hold a secret.

---

## Session Context Drift

**The problem:** Long-running CLI agent sessions accumulate stale context. A file that existed at session start but was since deleted, a convention that changed mid-session, an assumption the agent made early and is still carrying.

**Symptom:** The agent's output starts feeling wrong in subtle ways. It references patterns that don't match current code, or it argues against a decision you both already agreed to revisit.

**Fix:** Exit and restart the CLI tool. Yes, really. Long sessions produce worse output than two shorter ones with a restart between them.

**Related:** Project-context files (`CLAUDE.md`, etc.) are loaded at session start. If you update the file mid-session, the running session won't see the change until restart.

---

## Browser-Automation Tool Flakiness

**The problem:** Browser-automation MCP servers (e.g., Chrome extensions controlled via MCP) can fail silently if the extension isn't loaded, the wrong tab is focused, or a modal dialog is blocking the page.

**Symptom:** The tool call returns a generic "failed" with no specific reason. Retrying does the same thing.

**Check:**
- Is the extension loaded in the browser?
- Is the expected tab the active one?
- Is there a JavaScript `alert()` or `confirm()` dialog blocking interaction? (Some APIs will block on these invisibly.)

**Fix:** Refresh the extension, switch to the right tab, dismiss any modals, and retry. If that doesn't work, it's time to drop down to screenshots or a different verification method.

**Prevent:** Don't let your workflow trigger `alert`/`confirm`/`prompt` dialogs — they block all subsequent automation.

---

## "Tests Pass, Therefore It's Correct" Fallacy

**The problem:** AI-generated code that passes existing tests can still be wrong. Tests might not cover the case that matters. Worse, the AI might have *changed* the tests to match its broken implementation.

**Symptom:** A pipeline is green but something feels off. Or, in retrospect: "we merged it and then the customer found a bug that broke in an obvious edge case."

**Preventive workflow:**
1. Write the test *first* for the case you care about
2. Watch it fail
3. Then have the AI fix the production code
4. Confirm the test passes
5. Review both the production and test diffs

This reverses the usual AI trust direction and catches the "AI fixed the test instead of the code" failure mode.

**Detect retroactively:** if a diff includes both test changes and production changes, scrutinize the test changes specifically. Did a passing test get adjusted to pass with new behavior? That's a red flag unless the behavior change was intentional.

---

## The "Helpful" Refactor

**The problem:** You ask the AI to do X. It does X *and* refactors nearby code "for clarity" or "to match conventions." You now have scope creep in your diff.

**Symptom:** A 10-line bug fix turns into a 200-line diff because the AI "improved" adjacent code.

**Prevent:** Be explicit in your prompt. "Do only X. Do not modify any other code." For agentic tools, use plan mode and reject the plan if it includes scope creep.

**Detect:** When you review the diff, count the logical changes. If there are more than you asked for, push back — don't accept "but it's an improvement."

**Real risk:** A "helpful" refactor that breaks something unrelated to your bug fix. Now you have two bugs to diagnose and no clear separation of concerns.

---

## Convention Drift in Long-Running Agents

**The problem:** Over a long session, the agent's output style slowly drifts. File structure gets inconsistent. Naming becomes less aligned with the project-context file. The first five files it wrote followed conventions; the 15th doesn't.

**Prevent:**
- Keep sessions shorter.
- Re-reference the project-context file explicitly for each new file: "Follow the conventions in `CLAUDE.md` — specifically the naming rules in section X."
- Run the linter and formatter after every file. Let the tooling catch drift, not you.

---

## Secret Exposure in Memory / Conversation History

**The problem:** A CLI agent's memory system or a chat tool's conversation history may be storing sensitive data you passed through in an earlier turn. Later tasks might reference it, or the storage itself might be a risk.

**Prevent:**
- Never paste secrets into any AI tool — even "just to show it what the config looks like." Redact.
- Audit memory systems regularly — some CLI tools store per-project memory files in predictable paths.
- When rotating secrets, check whether any AI tool's memory might still be storing the old value.

---

## Cost Surprises

**The problem:** Agentic tools with autonomous iteration can consume a *lot* of tokens on a single task — especially if they get stuck in a fix-break-fix loop.

**Symptom:** Monthly bill is 3x what you expected. Or: you hit a rate limit mid-task because a single iteration cycle used more quota than a day's worth of normal work.

**Prevent:**
- Watch usage for the first month so you know your team's actual consumption profile.
- Set tool-level cost alerts if the platform supports them.
- If a task is in a loop (three failed iterations in a row), intervene — don't let it keep spinning.

---

## The Silent Success

**The problem:** Everything looked fine. Tests passed. Review was quick. Deployed. Three weeks later, someone notices an obvious problem that was in the diff the whole time.

**Why it happens:** AI-generated code is often well-formatted, consistently indented, and syntactically clean. It *looks* reviewed even when it wasn't. The brain skips past surface-level validation and never actually engages.

**Prevent:**
- Review deliberately. Out loud, to yourself, like you would a new hire's PR.
- Pair-review AI PRs occasionally, even if solo review is the team norm.
- Treat a 500-line AI diff as a 500-line diff, not "just AI output."

---

## When you find a new gotcha

1. Document it here.
2. Update the team's project-context file so the AI avoids it next time.
3. If it's a tool-wide issue, open an issue or contribute a fix upstream.

The goal is to never have the same person on the team get bitten by the same issue twice.
