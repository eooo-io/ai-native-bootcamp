# Contributing

Thanks for considering a contribution. This project is a living curriculum — the framework is stable but the tools, gotchas, and field experience will keep evolving.

## Good contributions

- **Field reports**: you ran this with your team — what did you change? What landed? What bombed? Open an issue or PR adding a case study.
- **New gotchas**: hit something a teammate wasted hours diagnosing? Add it to [`reference/common-gotchas.md`](reference/common-gotchas.md).
- **Clarifications**: places where the framework wording is ambiguous or easy to misread.
- **Translations**: if you're running this in a non-English team, translations are welcome. Keep each language in its own top-level directory.
- **Exercise variants**: alternative practice tasks for different team shapes (frontend-heavy, backend-heavy, platform, etc.).
- **Tool mappings**: if your team's stack is different enough that the orchestration matrix needs a variant, add it under a new file in `framework/`.

## Probably not a good fit

- **Prompt libraries** — this repo is about *workflow*, not prompts. There are better projects for that.
- **Model benchmarks / rankings** — out of date the moment they land. Not the point.
- **Tool advocacy** — adding a tool is fine if it fits a role. Removing or downgrading a tool to promote another is not.
- **"AI is bad" / "AI is magic" essays** — this curriculum takes a specific position (AI is leverage for experienced engineers, not a replacement). Disagreements that would require rewriting the thesis belong in a fork, not a PR.

## Style guidelines

- **Be direct.** Say what goes wrong and what to do about it. Skip the "let's explore..." preamble.
- **Name the antipattern.** "Don't do X" lands harder than "prefer Y."
- **Use concrete examples.** Generic advice ages faster than specific, dated examples do.
- **Prefer short.** If a section grows past ~200 lines, it's probably two sections.
- **Keep Markdown simple.** Tables for comparisons, code blocks for anything shell-runnable, prose otherwise. No HTML unless absolutely necessary.

## Submitting changes

1. Fork, branch, edit.
2. Run through the change yourself — if you added a step to pre-work, walk through it with a fresh machine or VM.
3. Open a PR. Describe:
   - What changed
   - Why (real problem that prompted it)
   - Any field experience behind the change
4. Be patient with reviews. This is a side project. Substantive PRs get faster responses than typo fixes.

## Code of conduct

Be kind. Assume good faith. If you have a technical disagreement, argue the technical point, not the person. Bad-faith engagement, gatekeeping, or hostility will get you blocked without ceremony.
