# AI Workspace System Agent Notes

This repository owns the local project launcher, sync workflow, bootstrap prompts, and shared AI workspace conventions.

## Goals

- Keep all local projects discoverable from `~/projects`.
- Support both Codex and Claude Code without duplicating instructions.
- Keep automatic sync safe by default.
- Keep reusable prompts, scripts, and docs versioned in a private GitHub organization.

## Safety Rules

- Do not put credentials, auth files, sessions, logs, sqlite state, or raw conversation history into Git.
- Do not make scheduled sync auto-commit by default.
- Do not force-push.
- Do not change existing project remotes silently.
- For mirror/upstream repos, preserve upstream remotes and add organization remotes explicitly.

## Instruction Conventions

- `AGENTS.md` is the canonical shared project instruction file.
- `CLAUDE.md` should import `@AGENTS.md` and add Claude-specific notes only when needed.
- Global Codex guidance lives in `~/.codex/AGENTS.md`.
- Global Claude guidance lives in `~/.claude/CLAUDE.md` and optional `~/.claude/rules/`.

## Verification

- Run `bash -n bin/*` after editing shell scripts.
- Run `workspace-sync --dry-run` after changing sync logic.
- Keep docs and templates aligned with script behavior.

