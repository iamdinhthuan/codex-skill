# Work Log

Durable project memory for future Codex conversations. Keep this file current
when meaningful task status changes, but keep it short enough to read at session
start.

## Status Legend

- `Done` - implemented and verified, or intentionally completed with notes.
- `In Progress` - currently being worked on in this branch/session.
- `Pending` - known next work that has not started.
- `Blocked` - cannot continue without a missing dependency, credential, answer,
  or external action.
- `Skipped` - intentionally not done; include the reason.

## Current State

- Active branch: `codex/add-stitch-skills`
- Open PR: https://github.com/iamdinhthuan/codex-skill/pull/2
- Base branch: `main`
- Runtime source of truth: repo prompts/agent TOML/templates plus installed
  Superpowers plugin skills.
- Superpowers plugin path:
  `~/.codex/plugins/cache/openai-curated/superpowers/`
- Expected Superpowers skill count: `14`
- Recommended coding plugins: `superpowers`, `github`, `build-ios-apps`,
  `codex-security`, `browser-use` or `chrome`, and `computer-use`.
- Global MCP servers: none. Prefer plugin-provided tools for iOS and browser
  workflows.
- Removed repo-managed skill bundles: `codex-team-skills`, `frontend-skills`,
  and `stitch-skills`.
- Unrelated worktree changes: root `SKILL.md` is deleted, and `.gitignore` has
  a local `docs/WORKLOG.md` ignore entry. They are not part of this refactor
  unless the user explicitly includes them.

## Recent Tasks

| Status | Task | Notes | Verification |
| --- | --- | --- | --- |
| Done | Use only Superpowers plugin skills | Removed repo-managed skill bundles, stopped syncing skills into `~/.codex/skills/`, routed all skill guidance through the installed Superpowers plugin, and updated verification to require the plugin's 14 skills. Synced runtime to `~/.codex`. Commit `83df4d4`. | `bin/sync-codex-runtime ~/.codex`; `bin/check-agent-workflow ~/.codex`; old runtime skill bundles absent; Superpowers plugin exposes 14 `SKILL.md` files; source/runtime `diff -q`; `bash -n`; agent TOML parse; bootstrap smoke test; `git diff --check`. |
| Done | Set recommended coding plugin baseline | Documented the recommended plugin set: `superpowers` for workflow, `github` for repo/PR/CI, `build-ios-apps` for iOS/SwiftUI, `codex-security` for appsec scans/review, `browser-use` or `chrome` for browser QA, and `computer-use` for desktop/manual control. Enabled `build-ios-apps` in local `~/.codex/config.toml`; `codex-security` was already enabled by the user. | `bin/check-agent-workflow ~/.codex`; plugin config parsed by `tomllib`; `git diff --check`. |
| Done | Remove unused global MCP servers | Removed global MCP blocks for Playwright, OpenAI Developer Docs, XcodeBuildMCP, and disabled Stitch from `~/.codex/config.toml`; documented that the baseline relies on plugin-provided tools instead. | `tomllib` parsed `~/.codex/config.toml` with no `mcp_servers`; `bin/check-agent-workflow ~/.codex`; stale MCP config search; `git diff --check`. |
| Done | Replace frontend skills with Taste Skill | Temporary step that replaced local frontend/Stitch skills with `Leonxlnx/taste-skill`. This has been superseded by the Superpowers-only decision. Commit `63b0489`. | Previously verified repo/runtime counts and frontmatter names; no longer the active skill model. |
| Done | Add lightweight workflow safeguards | Added validation-before-writing, structured handoffs, parent-thread tending, project template propagation, and `bin/check-agent-workflow`. Commit `f1a6618`. | Runtime sync, workflow check, temp bootstrap checks, TOML checks, and `git diff --check`. |
| Done | Simplify Codex team workflow | Removed external coordination/mail assumptions, kept local parent-thread/subagent coordination, pruned old skill stack, and aligned subagent config with OpenAI docs. Commit `91f2367`. | Runtime sync, stale-reference search, source/runtime diffs, TOML parse, skill count checks at the time, and `git diff --check`. |

## Active Decisions

- Use global `~/.codex/AGENTS.md` for behavior that should apply to every
  project. New projects should inherit this automatically.
- Use project-level `AGENTS.md` only for project-specific rules, commands,
  architecture constraints, domain context, or deployment notes.
- Bootstrap `docs/WORKLOG.md` automatically for meaningful work in new projects
  so task state survives across conversations.
- Do not vendor skill sources in this repo. Workflow skills come from the
  installed Superpowers plugin.
- Keep the coding plugin baseline focused: `superpowers`, `github`,
  `build-ios-apps`, `codex-security`, browser QA via `browser-use` or
  `chrome`, and `computer-use`. Keep `spreadsheets` and `presentations` off
  unless needed for those file types.
- Do not keep global MCP servers in the baseline unless a plugin or workflow
  cannot provide the needed tool.
- Runtime sync must delete old repo-managed skill directories:
  `~/.codex/skills/codex-team-skills`,
  `~/.codex/skills/frontend-skills`, and
  `~/.codex/skills/stitch-skills`.
- Keep shared prompts compact. Put durable workflow, routing, worklog, and
  coordination rules in `AGENTS.md` and `global/AGENTS.md`; move setup and
  operational detail to docs.
- Use Codex subagents plus parent-thread handoffs for role coordination. Keep
  durable task state in `docs/WORKLOG.md`.
- Keep unrelated worktree changes out of commits unless explicitly included.

## Verification Baseline

After changing prompts, agent configs, runtime sync, templates, or skill
routing, run:

```bash
bin/sync-codex-runtime ~/.codex
bin/check-agent-workflow ~/.codex
bash -n bin/check-agent-workflow bin/sync-codex-runtime bin/codex-bootstrap-project
git diff --check
```

For a stronger local smoke test:

```bash
tmp_project="$(mktemp -d /tmp/codex-bootstrap-XXXXXX)"
~/.codex/bin/codex-bootstrap-project "$tmp_project"
test -f "$tmp_project/AGENTS.md"
test -f "$tmp_project/docs/WORKLOG.md"
```

## How To Update This Log

When starting a task:

1. Add or update a row in Recent Tasks with `In Progress`.
2. Include branch, PR, affected files, or runtime path when useful.
3. State intended verification before implementation if the task is risky.

When finishing a task:

1. Change status to `Done`, `Skipped`, or `Blocked`.
2. Add commit/PR or command evidence that proves the outcome.
3. Move durable policy into Active Decisions only if it should guide future
   sessions.
4. Note unrelated worktree changes if they are still present.
