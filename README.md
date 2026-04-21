# AI-Native Development Bootcamp

A **three-session, 90-minutes-each** bootcamp that helps engineering teams move from *AI-assisted* development (one tool doing everything) to *AI-native* development (deliberately assigning the right tool to the right role).

> If your AI plans the system, executes changes, edits files, reviews results, and declares success on its own, you have not built a workflow. You have abdicated responsibility.

This curriculum is free, open, and designed to be forked and adapted to your team. It is stack-agnostic — it works whether your team writes Rails, Go, Django, Next.js, or anything else.

---

## What makes this different

Most "AI for developers" training focuses on tools. This one focuses on **roles**.

```
1. Thinking / Planning     — architectural reasoning, design decisions
2. Executing               — writing code, running commands, making changes
3. Editing                 — modifying, refactoring, adjusting existing work
4. Reviewing               — verifying correctness, quality, security
5. Iterating               — cycling back through 1–4 until accepted
```

Most developers today collapse all five into one tool (usually ChatGPT or Cursor). That feels productive and is actually a liability: when a single tool thinks, acts, and grades itself, you lose every checkpoint that separates "works" from "correct." This bootcamp is the antidote.

See [`framework/five-role-model.md`](framework/five-role-model.md) for the full argument.

---

## Who this is for

- **Engineering teams of 3–10** who are already using AI but sense they're using it badly.
- **Tech leads** who want to set workflow conventions their team can actually follow.
- **Engineering managers** shopping for structure before "let's use AI more" becomes a mandate.

## Who this is *not* for

- **Solo developers** — most of the team-coordination content doesn't apply to you. You can still skim the framework and gotchas.
- **"How do I write prompts?" training** — this is about workflow architecture, not prompt craft.
- **Teams without any AI exposure** — you'll get more from two weeks of experimenting first, then running this bootcamp.

---

## How to use this repository

### Option A: Run it as-is

1. Read [`SYLLABUS.md`](SYLLABUS.md) top to bottom.
2. Work through the **Preparation Checklist** in the syllabus — it has three phases (content prep, pre-work rollout, dry run).
3. Send [`exercises/participant-pre-work.md`](exercises/participant-pre-work.md) to participants a week before Session 1.
4. Run Sessions 1–3 from [`sessions/`](sessions/), one week apart.
5. Follow up with [`reference/post-bootcamp.md`](reference/post-bootcamp.md).

### Option B: Fork and adapt

1. Fork this repo.
2. Replace placeholders (`[your stack]`, `[your codebase]`, `[your VCS host]`) with your specifics.
3. Customize [`templates/`](templates/) for your team's conventions.
4. Keep the five-role framework — that's the durable part. Everything else is scaffolding you should reshape.

The curriculum is intentionally short on specifics so you can bring yours.

---

## Repository structure

```
ai-native-bootcamp/
├── README.md                              (this file)
├── SYLLABUS.md                            high-level syllabus + learning outcomes
├── CONTRIBUTING.md                        how to contribute back
├── LICENSE                                MIT
│
├── sessions/                              detailed facilitator plans (90 min each)
│   ├── 01-foundations.md
│   ├── 02-agentic-workflows.md
│   └── 03-team-conventions.md
│
├── framework/                             the durable concepts
│   ├── five-role-model.md                 why role separation matters
│   ├── orchestration-matrix.md            which tool for which role
│   └── capability-boundaries.md           what each tool can/cannot do
│
├── exercises/                             handouts for participants
│   ├── participant-pre-work.md            send a week before Session 1
│   ├── session-1-role-aware-task.md
│   ├── session-2-self-calibration.md
│   └── session-3-team-orchestration.md
│
├── templates/                             starting points for team artifacts
│   ├── CLAUDE.md.template                 project-context file for AI tools
│   ├── team-conventions.template.md       what you decide in Session 3
│   └── conventions-decision-sheet.md      the decision checklist for Session 3
│
└── reference/                             supporting material
    ├── common-gotchas.md                  real friction you will hit
    ├── cost-reality.md                    honest view of what this costs
    ├── security-conventions.md            what never goes in an AI tool
    └── post-bootcamp.md                   follow-up + living documentation
```

---

## The durable part vs. the scaffolding part

| Durable (don't change) | Scaffolding (change freely) |
|---|---|
| The five-role framework | Specific tool recommendations |
| "Separate thinking from executing" | Session timings |
| "Trust but verify — reviewing is always yours" | Example tasks |
| "The human decides, the AI implements" | Which MCP servers are in scope |
| Security boundary around customer data / secrets | Specific stack references |

Tools change quarterly. The framework should still make sense a year from now even if every tool in the orchestration matrix has been replaced.

---

## Contributing

PRs welcome — see [`CONTRIBUTING.md`](CONTRIBUTING.md). Good candidates:

- Field reports: "We ran this with X people on Y team, here's what worked / didn't"
- New gotchas that teams keep hitting
- Translations
- Clarifications to the framework

Not a good fit: prompt libraries, model benchmarks, tool rankings. This is a workflow curriculum, not a tool catalog.

---

## License

MIT. Fork, remix, teach it at your company, turn it into a blog post, adapt it for your language. Attribution appreciated but not required.
