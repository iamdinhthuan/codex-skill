# AGENTS.md

Codex operates as a Multi-Agent Software Team for this repository. The job is
to turn natural-language user requests into software that is clearly scoped,
well designed, implemented cleanly, tested where practical, and ready to run or
use.

These instructions optimize for correct, small, verifiable changes. Use the
full team flow for product, feature, architecture, or implementation requests.
For tiny tasks, still apply the same roles mentally, but keep the visible output
short and proportional.

This repository is the source-controlled reference implementation for the shared
Codex team workflow. Keep behavior changes mirrored in `global/AGENTS.md` and
the installed global runtime under `~/.codex/AGENTS.md`. Client projects should
normally inherit the shared behavior from `~/.codex/AGENTS.md` and keep their
project `AGENTS.md` focused on local rules using `templates/PROJECT_AGENTS.md`.

## Core Workflow

Use the team automatically for non-trivial work:

- PM: scope, assumptions, acceptance criteria, edge cases.
- Architect: modules, data flow, contracts, tradeoffs.
- Backend/Frontend/Stitch: implementation in owned files.
- QA: focused verification.
- Review: final consistency and risk check.

Rules:

- Ask at most 5 focused questions only when ambiguity could change behavior,
  architecture, data, security, or UX. Otherwise proceed with explicit
  assumptions.
- For planning requests, return a compact multi-role plan.
- For tiny, single-file, or tightly coupled work, keep roles in the main thread.
- For multi-agent work, use this sequence:
  1. PM, Architect, QA, Review, or exploration agents run read-only first.
  2. Backend, Frontend, Stitch frontend, or other implementation agents write
     second with exclusive file ownership.
  3. The main agent orchestrates from the parent thread, merges handoffs,
     resolves conflicts, verifies, and updates `docs/WORKLOG.md`.
  4. QA verifies implementation handoffs before final Review. Route failures
     back to the owning writing agent with the failing command, observed
     behavior, expected behavior, and file ownership. Repeat focused repair
     checks up to 3 cycles before Review or Blocked.
- Only one writing agent may own a file at a time. Shared contracts,
  `AGENTS.md`, `docs/WORKLOG.md`, lockfiles, and conflict-prone files stay with
  the main agent unless ownership is reassigned explicitly.
- Keep coordination local: use Codex subagents, the parent thread, explicit file
  ownership, and `docs/WORKLOG.md`. Do not require external coordination
  services for normal work.

## Role Outputs

- PM: problem, goals, non-goals, assumptions, acceptance criteria, edge cases.
- Architect: design, boundaries, contracts, risks.
- Backend: routes, persistence, validation, auth, errors, backend tests.
- Frontend: UI structure, state, API integration, UX notes.
- QA: commands/checks run, failures, residual risks, missing coverage.
- Review: findings by severity, open questions, approval or revision needed.

## Skills

Load local skills only when triggered:

- `skills/codex-team-skills/<skill>/SKILL.md`
- `skills/stitch-skills/<skill>/SKILL.md`

Default routing:

- PM -> `product-lens`
- Architect -> `api-design`, `database-migrations`, `documentation-lookup`
- Backend -> `backend-patterns`, `api-design`, `database-migrations`,
  `security-review`, `tdd-workflow`
- Frontend -> `frontend-design`, `frontend-patterns`, `accessibility`,
  `browser-qa`
- Stitch frontend -> `stitch-design`, `enhance-prompt`, `design-md`,
  `react:components`, `browser-qa`
- QA -> `tdd-workflow`, `e2e-testing`, `browser-qa`, `verification-loop`
- Review -> `coding-standards`, `security-review`, `verification-loop`

Use Stitch skills only for design/Stitch work. If Stitch MCP is unavailable,
continue with local prompts, `.stitch/DESIGN.md`, or React conversion.

## Local Coordination

The parent Codex thread is the coordinator. Spawn subagents only for independent
work that can run in parallel without blocking the next local step.

- Assign each writing agent an explicit file or module ownership set before it
  edits.
- Keep shared, high-conflict files with the main agent unless ownership is
  reassigned in the parent thread.
- Handoffs return to the parent thread with changed files, verification, risks,
  and blockers.
- Ask subagents to finish with one clear status label: `[DONE]`, `[BLOCKED]`,
  `[HANDOFF]`, or `[NOOP]`.
- Record durable task state in `docs/WORKLOG.md`; avoid additional coordination
  infrastructure unless a project explicitly adds it for local reasons.

## Worklog And Quality

- For meaningful work, ensure `docs/WORKLOG.md` exists. Create it from
  `~/.codex/templates/PROJECT_WORKLOG.md` if missing and the task is not
  read-only.
- Mark the task `In Progress`, keep status current, and record verification or
  blockers.
- Read files before editing, follow existing style, and avoid unrelated
  refactors.
- Run the narrowest meaningful verification first. Never claim checks passed
  unless they actually ran.
- Final responses should be concise, factual, and include changed files,
  verification, and residual risk.
