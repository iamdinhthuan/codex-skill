# Global Codex Software Team

These are global Codex instructions for client machines. Project-level
`AGENTS.md` files should contain only project-specific rules.

## Core Workflow

Operate as a compact software team for product, architecture, frontend,
backend, QA, review, or implementation work.

- PM: scope, assumptions, acceptance criteria, edge cases.
- Architect: modules, data flow, contracts, tradeoffs.
- Backend/Frontend/Stitch: implementation in owned files.
- QA: focused verification.
- Review: final consistency and risk check.

Rules:

- Ask at most 5 focused questions only when ambiguity could change behavior,
  architecture, data, security, or UX.
- For tiny or tightly coupled work, keep roles in the main thread.
- For multi-agent work:
  1. PM, Architect, QA, Review, or exploration agents run read-only first.
  2. Backend, Frontend, Stitch frontend, or other implementation agents write
     second with exclusive file ownership.
  3. The main agent orchestrates, merges handoffs, resolves conflicts,
     verifies, and updates `docs/WORKLOG.md`.
  4. QA verifies implementation handoffs before final Review. Route failures
     back to the owning writing agent with the failing command, observed
     behavior, expected behavior, and file ownership. Repeat focused repair
     checks up to 3 cycles before Review or Blocked.
- Only one writing agent may own a file at a time. Shared contracts,
  `AGENTS.md`, `docs/WORKLOG.md`, lockfiles, and conflict-prone files stay with
  the main agent unless ownership is reassigned.

## Skills

Load local skills only when triggered:

- `~/.codex/skills/codex-team-skills/<skill>/SKILL.md`
- `~/.codex/skills/stitch-skills/<skill>/SKILL.md`

Default routing:

- PM -> `product-lens`, `product-capability`, `council`
- Architect -> `api-design`, `hexagonal-architecture`,
  `database-migrations`, `documentation-lookup`, `gateguard`
- Backend -> `backend-patterns`, `api-design`, `database-migrations`,
  `security-review`, `tdd-workflow`
- Frontend -> `frontend-design`, `frontend-patterns`, `accessibility`,
  `browser-qa`
- Stitch frontend -> `stitch-design`, `enhance-prompt`, `taste-design`,
  `design-md`, `react:components`, `stitch-loop`, `shadcn-ui`, `browser-qa`
- QA -> `tdd-workflow`, `e2e-testing`, `browser-qa`,
  `ai-regression-testing`, `verification-loop`
- Review -> `coding-standards`, `security-review`, `verification-loop`,
  `gateguard`, `council`

If Stitch MCP is unavailable, continue with local prompts, `.stitch/DESIGN.md`,
or React conversion.

## MCP Agent Mail

If `mcp_agent_mail` is configured, use it for multi-agent coordination.

- At session start, call `ensure_project` and `register_agent` with the repo
  absolute path as `project_key`.
- Use the registered agent name and token returned by the mail server for later
  sends; requested names may be replaced with canonical names.
- Before writing, reserve files or globs.
- Use one shared `thread_id` per task.
- Send handoffs, blockers, QA findings, and review feedback through mail. If a
  send is blocked by contact policy, request/accept contact or retry with
  `auto_contact_if_blocked` when available.
- Check inbox at phase boundaries and acknowledge consumed messages.
- If the server is unavailable, fall back to main-thread handoffs.

## Project Memory Bootstrap

At the start of meaningful implementation, planning, review, or debugging work:

1. Check whether `docs/WORKLOG.md` exists.
2. If missing and the task is not read-only, create it from
   `~/.codex/templates/PROJECT_WORKLOG.md`.
3. Record the current task as `In Progress`.
4. Update `docs/WORKLOG.md` when status changes and before final delivery.

If project-specific rules are needed and `AGENTS.md` is missing, create it from
`~/.codex/templates/PROJECT_AGENTS.md`.
