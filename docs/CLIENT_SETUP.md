# Client Setup

Use this guide to make another machine read this repo and install the same
Codex team workflow, Superpowers plugin workflow, and project work-log
bootstrap.

## 1. Clone The Repo

```bash
git clone https://github.com/iamdinhthuan/codex-skill.git ~/codex_skill
cd ~/codex_skill
```

If the Stitch integration PR has not been merged yet, use the active branch:

```bash
git fetch origin codex/add-stitch-skills
git switch codex/add-stitch-skills
```

## 2. Install Into Codex Home

The install copies shared instructions, named specialist agents, templates, and
helper scripts into `~/.codex`. It removes old repo-managed skill bundles; skill
workflow comes from the installed Superpowers plugin.

```bash
bin/sync-codex-runtime ~/.codex
```

## 3. Configure Codex

Install the Superpowers plugin from the Codex plugin marketplace, then add or
verify these entries in `~/.codex/config.toml`.

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

[plugins."superpowers@openai-curated"]
enabled = true

[plugins."github@openai-curated"]
enabled = true

[plugins."build-ios-apps@openai-curated"]
enabled = true

[plugins."browser-use@openai-bundled"]
enabled = true

[plugins."chrome@openai-bundled"]
enabled = true

[plugins."computer-use@openai-bundled"]
enabled = true

[plugins."spreadsheets@openai-primary-runtime"]
enabled = false

[plugins."presentations@openai-primary-runtime"]
enabled = false

[mcp_servers.playwright]
command = "npx"
args = ["-y", "@playwright/mcp@latest"]

[mcp_servers.stitch]
url = "https://stitch.googleapis.com/mcp"
env_http_headers = { "X-Goog-Api-Key" = "STITCH_API_KEY" }
```

Recommended coding plugin set:

- Core workflow: `superpowers`
- Repo, PR, and CI: `github`
- iOS and SwiftUI: `build-ios-apps`
- Frontend/browser QA: `browser-use` or `chrome`
- Desktop/manual app control: `computer-use`

Set the API key in the shell environment used to launch Codex:

```bash
export STITCH_API_KEY="your-key"
```

Do not commit secrets into this repo or into project files.

## 4. Verify Install

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

Expected counts:

- Superpowers plugin skills: 14

## 5. Daily Usage

For a new project, open Codex in that project normally. Codex should read
`~/.codex/AGENTS.md` automatically.

When meaningful work starts, Codex should:

1. Check for `docs/WORKLOG.md`.
2. Create it from `~/.codex/templates/PROJECT_WORKLOG.md` if missing.
3. Update the current task to `In Progress`.
4. Decide whether logical team roles are enough, or whether real specialist
   subagents should be spawned for PM, Architect, Backend, Frontend, QA,
   Review, or Stitch design work.
5. For non-trivial implementation, run review/discovery agents read-only first,
   validate the approach against repo reality, then assign writing
   implementation agents by exclusive file ownership.
6. Keep the main agent focused on orchestration, structured handoff merge,
   conflict
   resolution, final verification, and work-log updates unless a file is shared,
   conflict-prone, or explicitly unassigned.
7. After Backend, Frontend, Stitch frontend, or other writing agents finish,
   send their handoffs through QA verification before final Review. If QA finds
   a bug, route the failing command, observed behavior, expected behavior, and
   file ownership back to the owning implementation agent, then have QA re-run
   the focused checks. Repeat up to 3 repair cycles before passing Review or
   marking the task Blocked.
8. Keep coordination in the parent Codex thread and keep
   `docs/WORKLOG.md` current when task status changes.

Manual bootstrap is available:

```bash
cd /path/to/project
~/.codex/bin/codex-bootstrap-project
```

Use project-level `AGENTS.md` only for project-specific rules: commands,
architecture constraints, style rules, domain context, and deployment notes.

## 6. Updating A Client Machine

```bash
cd ~/codex_skill
git pull --ff-only
bin/sync-codex-runtime ~/.codex
~/.codex/bin/check-agent-workflow
```

After updating, rerun the verification commands above.

## 7. Troubleshooting

- If Codex does not follow the team workflow, verify `~/.codex/AGENTS.md`
  exists and restart the Codex session.
- If a skill is missing, verify the Superpowers plugin is installed and exposes
  14 `SKILL.md` files.
- If Stitch generation fails, verify `STITCH_API_KEY` is set and the Stitch MCP
  server is configured.
- If a project should not create local docs, tell Codex the task is read-only.
