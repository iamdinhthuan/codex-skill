# Codex Skill

Multi-agent operating instructions for Codex.

This repo defines a compact software team workflow in `AGENTS.md`: PM,
Architect, Backend, Frontend, QA, and Review. The goal is for Codex to accept a
natural-language requirement, run the role handoffs automatically, implement
real code when requested, verify the result, and report only the useful output.

## Files

- `AGENTS.md` - main Codex team operating prompt
- `docs/CLIENT_SETUP.md` - how another machine installs and uses this repo as
  its global Codex operating model
- `docs/GLOBAL_BOOTSTRAP.md` - how global Codex instructions, skills, and
  project work logs are bootstrapped into new projects
- `docs/SKILL_REVIEW.md` - review notes for moving from vendored skills to the
  Superpowers plugin
- `docs/WORKLOG.md` - durable task memory for completed, active, pending,
  blocked, and skipped work
- `global/AGENTS.md` - global Codex instructions copied to `~/.codex/AGENTS.md`
- `global/agents/` - named specialist subagent config copied to
  `~/.codex/agents/`
- `templates/` - project bootstrap templates copied into `~/.codex/templates`
- `bin/codex-bootstrap-project` - helper script for creating project-local
  `AGENTS.md` and `docs/WORKLOG.md`

## Behavior

When a requirement is clear, Codex should proceed through the team workflow
without asking the user to manually trigger each role. When a requirement is
ambiguous enough to affect product behavior, architecture, data, security, or
UX, the PM role asks up to 5 focused questions; otherwise the team continues
with explicit assumptions.

The repo no longer owns a local skill set. Workflow skills come from the
installed Superpowers plugin; this repo only keeps global prompts, agent
configuration, templates, and verification scripts.

## Quick Install On Another Machine

Use this when setting up a new machine with the same Codex team workflow,
specialist subagents, Superpowers plugin workflow, and project bootstrap
templates.

### 1. Clone And Install Runtime

```bash
git clone https://github.com/iamdinhthuan/codex-skill.git ~/codex_skill
cd ~/codex_skill
bin/sync-codex-runtime ~/.codex
```

This installs:

- `~/.codex/AGENTS.md`
- `~/.codex/agents/*.toml`
- `~/.codex/templates/`
- `~/.codex/bin/codex-bootstrap-project`
- `~/.codex/bin/sync-codex-runtime`
- `~/.codex/bin/check-agent-workflow`

It also removes old repo-managed skill bundles from `~/.codex/skills/`. Skills
come from the installed Superpowers plugin.

### 2. Configure Codex

Install the Superpowers plugin from the Codex plugin marketplace, then add or
verify these blocks in `~/.codex/config.toml`:

```toml
[features]
apps = true
multi_agent = true

[agents]
max_threads = 12
max_depth = 1
job_max_runtime_seconds = 1800

[agents.pm]
description = "PM specialist for product scope, assumptions, user stories, acceptance criteria, edge cases, and requirement clarification."
config_file = "agents/pm.toml"
nickname_candidates = ["PM", "Product", "Requirements"]

[agents.architect]
description = "Architect specialist for system design, module boundaries, data flow, API contracts, data model decisions, and technical tradeoffs."
config_file = "agents/architect.toml"
nickname_candidates = ["Architect", "Architecture", "Design"]

[agents.backend]
description = "Backend specialist for API routes, validation, service logic, persistence, authorization, migrations, backend tests, and security-sensitive server work."
config_file = "agents/backend.toml"
nickname_candidates = ["Backend", "API", "Server"]

[agents.frontend]
description = "Frontend specialist for UI, components, state, forms, routing, API integration, responsive behavior, accessibility, and browser QA."
config_file = "agents/frontend.toml"
nickname_candidates = ["Frontend", "UI", "Client"]

[agents.qa]
description = "QA specialist for focused tests, E2E flows, browser checks, regression coverage, verification commands, and residual-risk reporting."
config_file = "agents/qa.toml"
nickname_candidates = ["QA", "Tests", "Verifier"]

[agents.review]
description = "Review specialist for requirements fit, architecture consistency, security, maintainability, test coverage, and concrete code-review findings."
config_file = "agents/review.toml"
nickname_candidates = ["Review", "Guard", "Auditor"]

[agents.stitch-frontend]
description = "Frontend design specialist for UI direction, design-system prompts, visual references, .stitch/DESIGN.md, and frontend conversion."
config_file = "agents/stitch-frontend.toml"
nickname_candidates = ["Stitch", "Design", "UX"]
```

Optional MCP servers:

```toml
[mcp_servers.playwright]
command = "npx"
args = ["-y", "@playwright/mcp@latest"]

[mcp_servers.stitch]
url = "https://stitch.googleapis.com/mcp"
env_http_headers = { "X-Goog-Api-Key" = "STITCH_API_KEY" }
```

If using Stitch, set the API key in the shell that launches Codex:

```bash
export STITCH_API_KEY="your-key"
```

### 3. Verify Install

```bash
test -f ~/.codex/AGENTS.md
test -f ~/.codex/agents/pm.toml
test -f ~/.codex/agents/architect.toml
test -f ~/.codex/agents/backend.toml
test -f ~/.codex/agents/frontend.toml
test -f ~/.codex/agents/qa.toml
test -f ~/.codex/agents/review.toml
test -f ~/.codex/agents/stitch-frontend.toml
test -x ~/.codex/bin/sync-codex-runtime
test -x ~/.codex/bin/check-agent-workflow
~/.codex/bin/check-agent-workflow
test ! -d ~/.codex/skills/codex-team-skills
test ! -d ~/.codex/skills/frontend-skills
find ~/.codex/plugins/cache/openai-curated/superpowers -path '*/skills/*/SKILL.md' | wc -l
~/.codex/bin/codex-bootstrap-project /tmp/codex-bootstrap-test
test -f /tmp/codex-bootstrap-test/docs/WORKLOG.md
test -f /tmp/codex-bootstrap-test/AGENTS.md
```

Expected skill counts:

- Superpowers plugin skills: 14

### 4. Use In A Project

Open Codex inside any repo. The global `~/.codex/AGENTS.md` should load
automatically. For meaningful implementation, planning, review, or debugging,
Codex should:

1. Check or create `docs/WORKLOG.md`.
2. Use PM, Architect, Backend, Frontend, QA, Review, and Stitch roles as needed.
3. Spawn real subagents when work has independent role/file slices.
4. Run read-only discovery/review agents first.
5. Validate the approach against repo reality before any writing agent edits.
6. Assign writing agents exclusive file ownership.
7. Coordinate through the parent thread with structured handoffs and record durable state in
   `docs/WORKLOG.md`.
8. Send writing-agent handoffs through QA before final Review.
9. Route QA failures back to the owning writer for up to 3 focused repair
   cycles before Review or Blocked.

Manual project bootstrap:

```bash
cd /path/to/project
~/.codex/bin/codex-bootstrap-project
```

After changing this repo on a client machine, refresh the installed runtime:

```bash
cd ~/codex_skill
git pull --ff-only
bin/sync-codex-runtime ~/.codex
~/.codex/bin/check-agent-workflow
```

## Skill Source

This repo no longer vendors skill files. Workflow skills come from the installed
Superpowers plugin:

https://github.com/obra/superpowers
