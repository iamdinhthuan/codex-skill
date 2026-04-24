# Global Bootstrap

This repo is the source of the shared Codex operating model. The local machine
also has a global install under `~/.codex` so new projects can inherit the team
workflow, skill stack, Stitch frontend design rules, and task memory behavior.

## What Loads Automatically

Codex reads `~/.codex/AGENTS.md` as global guidance. New projects do not need to
copy this repo's `AGENTS.md` to receive the shared behavior.

Global skills are installed at:

- `~/.codex/skills/everything-claude-code/`
- `~/.codex/skills/stitch-skills/`

These skills should be loaded only when their trigger matches the current task.

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
