# Curated Codex Team Skills

Source: https://github.com/affaan-m/everything-claude-code

Pinned source commit: `4e66b2882da9afb9747468b08a253ca2f09c85f3`

This directory contains a Codex-adapted curated subset of the upstream skills
referenced by this repo's `AGENTS.md`. It is meant for reading and selective
use, not for loading every skill into every session.

## Read First

- Team operating prompt: `../../AGENTS.md`
- License copied from source repo: `LICENSE`
- Each skill lives at `<skill-name>/SKILL.md`

## Essential Skills

Team operation:
- `search-first`
- `documentation-lookup`
- `verification-loop`

PM Agent:
- `product-lens`

Architect Agent:
- `api-design`
- `database-migrations`

Backend Agent:
- `backend-patterns`
- `api-design`
- `database-migrations`
- `security-review`

Frontend Agent:
- `frontend-design`
- `frontend-patterns`
- `accessibility`
- `browser-qa`

QA Agent:
- `tdd-workflow`
- `e2e-testing`
- `browser-qa`
- `verification-loop`

Review Agent:
- `security-review`
- `coding-standards`
- `verification-loop`

## Excluded By Design

Heavy orchestration, rare architecture, safety-duplicate, framework-specific,
release/infrastructure, benchmarking, and skill-audit/meta skills are
intentionally not copied here. Add them later only when repo evidence proves
they are needed.
