# Global Bootstrap

This repo is the source of the shared Codex operating model. The local machine
also has a global install under `~/.codex` so new projects can inherit the team
workflow, skill stack, Stitch frontend design rules, and task memory behavior.

## What Loads Automatically

Codex reads `~/.codex/AGENTS.md` as global guidance. New projects do not need to
copy this repo's project `AGENTS.md` to receive the shared behavior. Client
machines should install `global/AGENTS.md` from this repo as
`~/.codex/AGENTS.md`.

Use the repo sync helper to install or refresh the runtime files:

```bash
bin/sync-codex-runtime ~/.codex
```

Global skills are installed at:

- `~/.codex/skills/codex-team-skills/`
- `~/.codex/skills/stitch-skills/`

These skills should be loaded only when their trigger matches the current task.

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
- `stitch-frontend` for Stitch design, prompt enhancement, `.stitch/DESIGN.md`,
  and Stitch-to-React work.

The global `AGENTS.md` decides when to spawn real specialists versus keeping
the roles inside the main thread. Use real subagents for explicit multi-agent
requests or independent PM/Architect/BE/FE/QA/Review/Stitch work slices. For
non-trivial implementation, run PM/Architect/QA/Review or exploration agents
read-only first, then assign Backend/Frontend/Stitch or other implementation
agents as writers with exclusive file ownership. The main agent should
orchestrate, merge, resolve conflicts, verify, and update `docs/WORKLOG.md`,
only coding shared, conflict-prone, or explicitly unassigned files. Keep tiny
or tightly coupled tasks in the main thread.

After Backend, Frontend, Stitch frontend, or other writing agents finish,
route their handoffs through QA before final Review. QA should verify the PM
acceptance criteria and changed behavior. If QA finds a bug, the main agent
routes the failing command, observed behavior, expected behavior, and affected
file ownership back to the owning implementation agent, then asks QA to re-run
the focused checks. Repeat up to 3 repair cycles before passing Review or
marking the task Blocked.

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
```
