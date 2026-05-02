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
- `docs/SKILL_REVIEW.md` - review notes for each vendored skill and its
  Codex-specific adaptations
- `docs/WORKLOG.md` - durable task memory for completed, active, pending,
  blocked, and skipped work
- `skills/codex-team-skills/` - Codex-adapted curated local copies of
  essential skills from `affaan-m/everything-claude-code`
- `skills/stitch-skills/` - local copies of Google Stitch skills for
  AI-assisted frontend design and Stitch-to-React workflows
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

The local skill set is intentionally small. Framework-specific, infrastructure,
benchmarking, and skill-audit skills are excluded until repo evidence shows
they are needed.

## Quick Install On Another Machine

Use this when setting up a new machine with the same Codex team workflow,
specialist subagents, skills, and project bootstrap templates.

### 1. Clone And Install Runtime

```bash
git clone https://github.com/iamdinhthuan/codex-skill.git ~/codex_skill
cd ~/codex_skill
bin/sync-codex-runtime ~/.codex
```

This installs:

- `~/.codex/AGENTS.md`
- `~/.codex/agents/*.toml`
- `~/.codex/skills/codex-team-skills/`
- `~/.codex/skills/stitch-skills/`
- `~/.codex/templates/`
- `~/.codex/bin/codex-bootstrap-project`
- `~/.codex/bin/sync-codex-runtime`
- `~/.codex/bin/check-agent-workflow`

### 2. Configure Codex

Add or verify these blocks in `~/.codex/config.toml`:

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
description = "Stitch frontend design specialist for AI-assisted UI design, Stitch MCP workflows, prompt enhancement, .stitch/DESIGN.md, and React conversion."
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
find ~/.codex/skills/codex-team-skills -maxdepth 2 -name SKILL.md | wc -l
find ~/.codex/skills/stitch-skills -maxdepth 2 -name SKILL.md | wc -l
~/.codex/bin/codex-bootstrap-project /tmp/codex-bootstrap-test
test -f /tmp/codex-bootstrap-test/docs/WORKLOG.md
test -f /tmp/codex-bootstrap-test/AGENTS.md
```

Expected skill counts:

- `codex-team-skills`: 15
- `stitch-skills`: 4

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

## Source Attribution

The curated skill files under `skills/codex-team-skills/` are adapted from:

https://github.com/affaan-m/everything-claude-code

Pinned source commit:

```text
4e66b2882da9afb9747468b08a253ca2f09c85f3
```

The copied upstream license is available at
`skills/codex-team-skills/LICENSE`.

The Stitch skill files under `skills/stitch-skills/` come from:

https://github.com/google-labs-code/stitch-skills

Pinned source commit:

```text
6c0cbdb909b7d256c8b9b3854c8c8f87aab2c140
```

The copied upstream license is available at `skills/stitch-skills/LICENSE`.
