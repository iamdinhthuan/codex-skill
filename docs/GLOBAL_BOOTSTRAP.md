# Global Bootstrap

This repo is the source of the shared Codex operating model. The local machine
also has a global install under `~/.codex` so new projects can inherit the team
workflow, Superpowers plugin workflow, and task memory behavior.

## What Loads Automatically

Codex reads `~/.codex/AGENTS.md` as global guidance. New projects do not need to
copy this repo's project `AGENTS.md` to receive the shared behavior. Client
machines should install `global/AGENTS.md` from this repo as
`~/.codex/AGENTS.md`.

Use the repo sync helper to install or refresh the runtime files:

```bash
bin/sync-codex-runtime ~/.codex
```

Workflow skills come from the installed Superpowers plugin. This repo no longer
installs skill bundles into:

- `~/.codex/skills/codex-team-skills/`
- `~/.codex/skills/frontend-skills/`

Those old directories are removed during runtime sync.

Recommended coding plugins:

- `superpowers@openai-curated` for the core workflow.
- `github@openai-curated` for repo, PR, CI, and publish workflows.
- `build-ios-apps@openai-curated` for iOS and SwiftUI work.
- `codex-security@openai-curated` for security scans, threat modeling,
  finding validation, attack-path analysis, and security finding fixes.
- `browser-use@openai-bundled` or `chrome@openai-bundled` for frontend/browser
  QA.
- `computer-use@openai-bundled` for desktop or simulator workflows that need
  manual app control.

Keep productivity plugins such as `spreadsheets` and `presentations` disabled
unless the task is specifically about those file types.

No global MCP servers are required for the baseline. Prefer plugin-provided
tools: `build-ios-apps` supplies XcodeBuildMCP-backed workflows, and the
browser plugins cover frontend/browser QA.

## Specialist Subagents

Codex supports named agent roles through `~/.codex/config.toml`. This repo
stores the source-controlled role config in:

```text
global/agents/
```

Install those files to:

```text
~/.codex/agents/
```

Then configure named roles in `~/.codex/config.toml` with `agents.<name>`
entries that point at those files. The intended specialist roles are:

- `pm` for product scope, assumptions, user stories, acceptance criteria, and
  edge cases.
- `architect` for architecture, module boundaries, data flow, API contracts,
  data model decisions, and technical tradeoffs.
- `backend` for API, validation, persistence, auth, migrations, and backend
  tests.
- `frontend` for UI, components, client state, integration, accessibility, and
  browser QA.
- `qa` for focused test strategy, E2E checks, regression coverage, and final
  verification.
- `review` for architecture, security, maintainability, and missing-test
  review.
- `stitch-frontend` for frontend design prompts, visual references,
  `.stitch/DESIGN.md`, and frontend conversion work.

The global `AGENTS.md` keeps the workflow compact: review/discovery agents run
read-only first, the main agent validates the approach against repo reality,
implementation agents write second with exclusive file ownership, and the main
agent orchestrates, merges structured handoffs, verifies, and updates the work
log. Coordination stays local to Codex subagents and the parent thread.
QA routes failures back to the owning writer for up to 3 focused repair cycles
before Review or Blocked.

## Project Memory

For meaningful implementation, planning, review, or debugging work, global
instructions now require Codex to check for `docs/WORKLOG.md` in the target
repo. If it is missing and the user did not ask for read-only work, Codex should
create it from:

```text
~/.codex/templates/PROJECT_WORKLOG.md
```

The work log is the project-local memory for future conversations:

- task status
- active branch or PR
- decisions
- verification
- blockers
- unrelated worktree notes

## Project AGENTS.md

Do not copy this repo's full `AGENTS.md` into every project by default. Use the
global file for shared behavior. Create a project-level `AGENTS.md` only when
the project needs local rules, commands, architecture context, or domain
constraints.

If needed, bootstrap it from:

```text
~/.codex/templates/PROJECT_AGENTS.md
```

## Manual Bootstrap

Run this when you want to create both `AGENTS.md` and `docs/WORKLOG.md` in a
project explicitly:

```bash
~/.codex/bin/codex-bootstrap-project /path/to/project
```

If no path is provided, the command bootstraps the current directory.

When this repo changes shared runtime files, re-sync them with:

```bash
bin/sync-codex-runtime ~/.codex
~/.codex/bin/check-agent-workflow
```
