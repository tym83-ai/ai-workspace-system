# AI Workspace System Agent Notes

This repository owns the local project launcher, sync workflow, bootstrap prompts, and shared AI workspace conventions.

## Goals

- Keep local projects discoverable from configured project roots.
- Support both Codex and Claude Code without duplicating instructions.
- Keep automatic sync safe by default.
- Keep reusable prompts, scripts, and docs versioned without machine-specific values.

## Safety Rules

- Do not put credentials, auth files, sessions, logs, sqlite state, or raw conversation history into Git.
- Do not make scheduled sync auto-commit by default.
- Do not force-push.
- Do not change existing project remotes silently.
- For mirror/upstream repos, preserve upstream remotes and add organization remotes explicitly.
- Do not hardcode a maintainer's home path, GitHub organization, project names, or private helper scripts in this repository.

## Configuration Rules

- Local machine config lives in `~/.config/ai-workspace/config`.
- `config/config.example` must stay generic and safe to publish.
- Use `workspace-configure` to collect machine-specific values interactively.
- Keep generated logs under `$XDG_STATE_HOME` or `~/.local/state`, not inside this repository.

## Instruction Conventions

- `AGENTS.md` is the canonical shared project instruction file.
- `CLAUDE.md` should import `@AGENTS.md` and add Claude-specific notes only when needed.
- Global Codex guidance lives in `~/.codex/AGENTS.md`.
- Global Claude guidance lives in `~/.claude/CLAUDE.md` and optional `~/.claude/rules/`.

## Verification

- Run `bash -n bin/*` after editing shell scripts.
- Run `workspace-sync --dry-run` after changing sync logic.
- Search for personal values before publishing.
- Keep docs and templates aligned with script behavior.
