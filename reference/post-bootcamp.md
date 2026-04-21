# Post-Bootcamp

What happens after Session 3, where the real work of adoption starts.

---

## The 2-Week Follow-Up

Schedule this **in Session 3**, before people leave. 30 minutes, calendar it now. If you wait to schedule it later, it won't happen.

### Agenda

1. **Commitment check (10 min)** — go around, everyone revisits the workflow change they committed to in Session 3. Did they do it? What happened?
2. **What's working (10 min)** — concrete examples from real work.
3. **What's not (10 min)** — be blunt. If a convention is being ignored, why?

### Expected outcomes

- **Some commitments held, some didn't.** Normal. Investigate the ones that didn't — was the commitment too vague, too ambitious, or poorly targeted?
- **At least one convention needs revision.** Real usage always finds edges the decision sheet missed.
- **New gotchas.** Add them to `../reference/common-gotchas.md`. This is the team's institutional knowledge accumulating.

### What to change if it's not working

If, two weeks out, the team has not adopted the practices:

- **Framework confusion.** Someone can't articulate the five roles in their own words. Re-read [`../framework/five-role-model.md`](../framework/five-role-model.md) together.
- **Tool access issues.** Someone couldn't actually get the tool installed properly. Dedicate pairing time.
- **"Feels like more work."** Yes, role separation has overhead. For the smallest tasks, let them collapse roles — but insist on separation for anything non-trivial. Make the trigger for separation a hard rule (e.g., "new feature = separated roles").
- **Conventions being ignored.** Either they're wrong or they're not visible. Wrong → revise. Not visible → embed them in tooling (pre-commit hooks, CI checks, review templates).

---

## Recorded Reference Material

Within the first month after Session 3, record 3–5 short (5–10 min) screencasts of **your team's actual workflows**, not idealized demos. Good candidates:

1. "Creating a new feature end-to-end" (execute + iterate)
2. "Reviewing and fixing a failing CI pipeline" (review + iterate)
3. "Writing tests for an existing untested feature" (execute + iterate)
4. "Using docs search + database queries via MCP" (thinking + executing)
5. "Debugging a UI issue with AI assistance" (reviewing + iterating)

These become the team's async reference library — **faster than re-reading documentation**. They also become onboarding material for new hires.

Keep them short. A 20-minute screencast won't get watched. A 7-minute one will.

---

## Living Documentation

Three artifacts should be living, not static:

### 1. The project-context file

`CLAUDE.md` (or equivalent) is not a one-time write. Update it when:

- A recurring AI mistake happens → add a rule to prevent it
- A convention changes → update the relevant section
- A new convention is adopted → document it
- A dependency upgrades → reflect the new version

Review it in PRs like any other documentation. If nobody touches it for 3 months, either nothing has changed (unlikely) or it's being ignored (likely).

### 2. Team conventions doc

Output from Session 3. Review quarterly. Amend whenever:

- A rule turns out to be wrong in practice
- A new category of tool is considered
- A security incident reveals a gap

Add changelog entries when anything changes. Future team members (and future you) need to know when and why a rule shifted.

### 3. Gotchas file

Accumulate institutional knowledge. Every time a team member spends >30 min diagnosing something that turns out to be a known issue, the issue goes here. This is the most valuable artifact long-term — it's the list of mistakes you only make once.

---

## Onboarding New Hires

Once your team is established in AI-native workflows, new hires have a specific onboarding path:

1. **Pre-work** — same pre-work document that existed for the original bootcamp. Install, clone, skim context file.
2. **Watch the screencasts** (if you recorded them).
3. **Read the team conventions** doc and the gotchas file.
4. **Pair for two weeks.** New hire pairs with an existing engineer on real work. Observe the role separation in practice.
5. **Solo, reviewed work for one month.** New hire works solo, but their PRs get extra review from an existing team member specifically on AI-workflow adherence.
6. **Steady state.** By week 6, they're contributing like everyone else.

You do not re-run the full bootcamp for new hires. Pair + read is enough once the team-level workflow is established.

---

## When to re-run the bootcamp

You re-run when:

- **Half the team has turned over.** New majority, new workflow.
- **A fundamentally new tool category emerges.** If autonomous agents get 10x better, the orchestration matrix might change. Redo it.
- **Security incident involving AI tooling.** Even if the cause was a convention violation, the team needs to reset expectations.
- **Workflow decay.** If the team's code review is no longer catching AI failure modes, the muscle has atrophied. Short refresher (a single 90-min session) may be enough.

You do **not** re-run annually by default. This isn't compliance training. Re-run when there's a reason.

---

## Sharing back

If you adapted this bootcamp for your team, consider:

1. **Blog post or talk** about what you changed and why.
2. **PR to this repo** with notable adaptations — "field report" issues or PRs are welcome.
3. **Translation** if your team runs in a non-English language.

The framework is more robust the more teams stress-test it. You had insights running this that we didn't. Share them.

---

## What success looks like at 3 months

- **Nobody on the team is pasting customer data, `.env` values, or secrets into cloud AI tools.** Full stop.
- **The project-context file is up to date** and being referenced in PRs and code reviews.
- **AI-generated PRs are reviewed for AI-specific failure modes** — not just "does it compile."
- **At least one custom skill / slash command exists** for a recurring team workflow.
- **When someone says "I'll get AI to do it,"** the team's shared reflex is "to do which *part* of it?"
- **Junior engineers are *not* using AI to ship code they don't understand.** If they are, the senior engineers intervene.

If these are happening: the bootcamp worked. You can probably stop paying this much attention to it.

If these are not happening: something in the workflow-to-reality translation broke. Diagnose before re-running.
