# Adapting to Your Stack

The curriculum is stack-agnostic by design. The framework (five roles, orchestration matrix, capability boundaries) applies whether you ship in Rails, Go, Python, TypeScript, or anything else. What changes per stack:

- **What goes in the project-context file** (`CLAUDE.md` / `AGENTS.md` / `.cursorrules`)
- **Which CLI-agent tool fits best** (most work fine; some have stack-specific strengths)
- **Which MCP servers are worth setting up** (almost always includes your VCS host + a docs-search server; maybe a framework-specific server)
- **What Session 1 Part B tour looks like** (the "operations vs. suggestions" demo)
- **What gotchas are likely**

This guide shows worked examples for four common stacks. If yours isn't here, follow the pattern and fill in your own specifics. The pattern is: answer the five questions below for your stack.

---

## The five questions to answer for your stack

Before adapting the bootcamp, have concrete answers to:

1. **What's the project-context file your tool uses?** (`CLAUDE.md`, `.cursorrules`, `AGENTS.md`, etc.) What sections does it need?
2. **What's your dev loop?** Install, run tests, lint, format — one command each. The agent will be running these in real time.
3. **What can go wrong autonomously?** The agent will be running commands. Which ones are destructive? Which ones require a specific environment?
4. **Which MCP servers give the best leverage?** VCS host almost always. Docs-search often. Database sometimes. What else helps?
5. **What stack-specific failure modes should reviewers watch for?** Every stack has its own "AI-generated code smells."

Answers below per stack.

---

## Node / TypeScript

### Project-context file

Priorities for `CLAUDE.md`:

- Node version (use a `.nvmrc` or `engines` field reference)
- Package manager — npm / pnpm / yarn / bun. If monorepo, mention the workspace tool (pnpm workspaces, Nx, Turborepo).
- TypeScript config — strict mode on/off, path aliases, baseUrl.
- Framework — Next.js / Nest / Express / Fastify / SvelteKit / Hono.
- Directory structure, especially for monorepos — `apps/`, `packages/`, `tools/`.
- Test runner — Vitest / Jest / Mocha. Conventions for test file location (`__tests__/`, colocated `.test.ts`).
- Lint/format — ESLint config, Prettier config, whether they run on commit.
- Import style — relative, absolute via path aliases, or both.

### Dev loop example

```bash
pnpm install
pnpm dev            # local dev server
pnpm test           # run tests
pnpm test <filter>  # run filtered tests
pnpm lint           # lint check
pnpm format         # apply Prettier
pnpm typecheck      # TypeScript strict check
```

### Destructive-command watchlist

- `pnpm install` + upgrades can rewrite lockfile unexpectedly. The agent should not run `update` without asking.
- `npx` running arbitrary packages — supply chain risk. Don't let the agent `npx` packages it "discovered."
- `rm -rf node_modules` is fine in most flows but flag anyway.

### MCP servers worth setting up

- Your VCS host (GitHub / GitLab)
- A **TypeScript docs** server (several community MCP servers exist for searching TS / npm package docs)
- **Database MCP** if using Prisma — helps the agent verify schemas before generating queries

### Session 1 Part B demo ideas

- Add a new endpoint / route handler with a typed response
- Rename a function across 10+ call sites, watch the agent handle import updates
- Run `pnpm test --filter <foo>` and watch the agent read failures

### Stack-specific failure modes

- **Hallucinated types** — especially against third-party libraries with weak `@types`. Agent invents a method or field. Always verify against node_modules.
- **ESM vs CJS confusion** — agent mixes `import`/`require`, breaks the module system. Enforce "module": "ESNext" consistency in `CLAUDE.md`.
- **Version drift** — agent writes for a Next.js version you're not on (14 vs 13 routing is a common miss). Pin version in the context file.
- **TypeScript strictness gaps** — agent uses `any` liberally when your config forbids it. Call it out.
- **Overly generic abstractions** — agent loves creating "generic" utility types / HOFs that nobody needs.

---

## Python / Django

### Project-context file

Priorities:

- Python version (from `.python-version` or `pyproject.toml`)
- Package manager — poetry / pip-tools / uv / pipenv. Virtualenv location.
- Django version and style — function-based vs class-based views, DRF usage, ninja, etc.
- Settings module structure — single `settings.py` vs `settings/base.py` + `settings/dev.py`.
- Database — which one, which ORM features are used heavily (migrations, signals, raw SQL tolerance).
- Test runner — pytest / Django TestCase / both. Fixture approach (pytest fixtures vs factory_boy).
- Lint/format — ruff, black, mypy strictness.
- Type hints — used or not. If yes, how strict?

### Dev loop example

```bash
poetry install
poetry run python manage.py migrate
poetry run python manage.py runserver
poetry run pytest
poetry run pytest <filter>
poetry run ruff check .
poetry run ruff format .
poetry run mypy .
```

### Destructive-command watchlist

- `python manage.py migrate` against a real DB — agent might try to run this without warning. Should always be confirmed.
- `python manage.py flush` wipes data. Restrict.
- `python manage.py makemigrations` — fine, but review the generated migration. Agent will happily name migrations badly.

### MCP servers worth setting up

- Your VCS host
- **Python docs** server (community MCP server for searching Python / PyPI docs)
- **Database MCP** — PostgreSQL MCP is widely used. Lets the agent read schema before writing migrations.
- Maybe: a Django-admin helper MCP if your team has one

### Session 1 Part B demo ideas

- Add a new Django model with migration + serializer + test
- Run `pytest -k <testname>` and watch the agent iterate
- Extract logic from a bloated view into a service function (common Django refactor)

### Stack-specific failure modes

- **Migration naming** — agent generates `0042_auto_20260421_1530.py` instead of descriptive names. Enforce explicit `name` in `makemigrations` calls.
- **N+1 queries** — agent writes `for obj in qs: obj.related.all()` and doesn't `prefetch_related`. Every review should check query count.
- **Signal side effects** — agent adds a `post_save` signal to solve a localized problem. Signals create distant action-at-a-distance. Prefer explicit calls.
- **`settings` module confusion** — agent modifies `base.py` when the change should be env-specific. Document which module gets edited for what.
- **Raw SQL over ORM** — agent falls back to `cursor.execute` instead of using the ORM. Usually wrong for correctness and testability.
- **Blanket `except Exception`** — extremely common in AI-generated Python. Ban in your conventions.

---

## Go

### Project-context file

Priorities:

- Go version (from `go.mod`)
- Module path (e.g., `github.com/eooo-io/your-project`)
- Directory layout — `cmd/`, `internal/`, `pkg/`, `test/`. The `internal/` convention matters; agents sometimes place code incorrectly.
- Framework if any — net/http stdlib / chi / gin / echo / fiber.
- Database — database/sql, sqlx, ent, gorm, sqlc. Migration tool (goose, migrate, atlas).
- Error-handling style — sentinel errors, wrap with `fmt.Errorf("%w", ...)`, errors.Is/As expectations.
- Logging style — stdlib `log/slog`, zap, zerolog.
- Test style — table-driven tests, `testify` usage (or avoidance), integration test location.
- Interface philosophy — accepted size of interfaces, "interfaces defined at the consumer" Go idiom.

### Dev loop example

```bash
go build ./...
go run ./cmd/server
go test ./...
go test -run TestFoo ./pkg/foo
go vet ./...
gofumpt -l -w .
golangci-lint run
```

### Destructive-command watchlist

- `go mod tidy` + `go get` can change dependencies in ways you didn't expect. Review go.mod/go.sum diffs carefully.
- Database migration commands (`goose up`, etc.) — same rule as Django.

### MCP servers worth setting up

- Your VCS host
- **Go docs** MCP server — helps with `go doc` queries for stdlib and dependencies
- Database MCP if applicable

### Session 1 Part B demo ideas

- Add a new handler to an HTTP router with a table-driven test
- Refactor a function to return `(T, error)` instead of panicking on failure
- Watch the agent handle Go's strict unused-import / unused-variable errors

### Stack-specific failure modes

- **Overly large interfaces** — agent defines a 12-method interface when two would do. Go idiom is "accept small interfaces, return structs."
- **`interface{}` / `any` abuse** — when a concrete type would be cleaner. Agent reaches for `any` under ambiguity.
- **Ignored errors** — `_ = json.Marshal(...)` patterns. Sometimes intentional, usually not. Reviewers must eyeball every `_`.
- **Goroutine leaks** — agent spawns goroutines without a `context.Context` or a way to signal shutdown. Enforce context plumbing in `CLAUDE.md`.
- **Over-wrapping errors** — `fmt.Errorf("failed to do X: failed to do Y: failed to do Z: %w", ...)` chains that don't help diagnosis.
- **Mocking everything** — Go's testing idiom often uses real dependencies for integration tests. Agents raised on other languages over-mock.

---

## Ruby on Rails

### Project-context file

Priorities:

- Ruby version + Rails version (from `Gemfile` / `.ruby-version`)
- Bundler conventions — any custom groups, `--without` flags in deploy
- Database — which DB, strong_migrations in use or not
- Background jobs — Sidekiq / Good Job / Solid Queue / Resque
- Test framework — RSpec / Minitest. Factory_bot vs fixtures. System tests with Capybara.
- Linting — RuboCop (Omakase config or custom). Standard Ruby.
- Frontend pipeline — importmap / jsbundling-rails / Propshaft / Sprockets. Hotwire / React / Vue?
- Active Record conventions — service objects usage, interactor patterns, callback policy

### Dev loop example

```bash
bundle install
bin/rails db:migrate
bin/dev
bundle exec rspec
bundle exec rspec spec/models/user_spec.rb
bundle exec rubocop
bundle exec rubocop -a    # autofix
```

### Destructive-command watchlist

- `db:migrate` and especially `db:rollback` — agent might try to roll back a production-touched migration. Restrict.
- `db:seed` against real data.
- Gem upgrades via `bundle update` — can cascade unpredictably.

### MCP servers worth setting up

- Your VCS host
- **Rails / Ruby docs** MCP server (search ruby-doc / api.rubyonrails.org)
- Database MCP

### Session 1 Part B demo ideas

- Add a new model with migration, factory, and controller with RSpec coverage
- Extract a fat-model method into a service object
- Run `rspec --only-failures` and watch the iteration loop

### Stack-specific failure modes

- **Fat models** — agent pile of business logic into `User` / `Order` because "that's where the data is." Enforce service-object patterns or explicit extraction rules.
- **Strong_migrations violations** — agent adds a NOT NULL column to a 10M-row table. Will work in dev, blow up in prod. Strong_migrations gem catches this; pin its use in `CLAUDE.md`.
- **N+1 queries** — `@users.each { |u| puts u.posts.count }`. Same as Django, different idiom. `includes/preload/eager_load` awareness required.
- **Callback chains** — agent adds `before_save` / `after_commit` hooks that trigger other hooks. Rails callbacks are a known mess; prefer explicit service calls.
- **Global state via `Thread.current`** — agent reaches for it when passing a parameter would do. Almost always the wrong call.
- **Over-using metaprogramming** — `define_method` / `class_eval` when a plain method would work. Rails code especially tempts agents into this.

---

## Generalizing to your stack

If your stack isn't here — Elixir, Rust, Kotlin, Swift, PHP, C#, Clojure, etc. — the pattern is the same. Build your own worked example:

1. **Decide your `CLAUDE.md` priorities.** What does a fresh AI session need to know to not embarrass itself?
2. **Document the dev loop.** One command per verb. Agent will run them in real time.
3. **List destructive commands.** Anything that touches shared state, migrations, dependencies, or infrastructure.
4. **Pick your MCP servers.** VCS host is almost always worth it. Docs search often is. Database sometimes. Ignore the rest until you hit friction.
5. **List stack-specific failure modes.** These are the code smells your reviewers will be chasing for the next year. Writing them down once prevents writing them down fifty times in PR comments.

Contribute your worked example back via PR. The more stacks this guide covers, the more teams can skip the "figure it out from scratch" phase.

---

## Cross-cutting advice (all stacks)

- **Version-pin everything in the context file.** "TypeScript 5.4", "Rails 7.1", "Go 1.22." The agent will happily write for the wrong version if you leave it ambiguous.
- **Name your conventions explicitly.** "We use service objects over fat models" > implied convention by example. The agent reads the context file literally; it doesn't infer.
- **Document exceptions to common patterns.** If your Rails app doesn't use `ActiveRecord::Base.transaction` the usual way, say so. Agents default to idiomatic; if you're non-idiomatic by choice, say why and where.
- **Keep the context file short enough to read.** If it's 2,000 lines, nobody maintains it. Prefer linking out to detailed docs for anything the agent doesn't need at session-start.
- **Revisit quarterly.** Stacks drift, conventions evolve. A year-old context file is a lie the agent will happily repeat.
