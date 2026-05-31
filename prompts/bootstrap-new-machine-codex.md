# Codex Prompt: Bootstrap AI Workspace On A New Machine

You are Codex working on my local machine.

Goal:
Create a clean local AI workspace for all projects under `~/projects`, with safe GitHub synchronization, Codex instructions, Claude Code bridge files, and reusable scripts.

Rules:

- Do not upload secrets.
- Do not delete or overwrite local files without explicit approval.
- Do not force-push.
- Do not auto-commit dirty repositories unless I explicitly ask.
- Keep Claude-specific state out of Codex repos.
- Use `AGENTS.md` as the canonical shared project instruction file.
- Use `CLAUDE.md` as a Claude bridge that imports `@AGENTS.md`.

GitHub organization:

```text
<ORG_URL>
```

Tasks:

1. Inspect the home directory for likely projects.
2. Classify directories as real project, repo mirror, Codex asset, Claude project, artifact, archive, ignore, or needs review.
3. Create `~/projects` if missing.
4. Move safe projects into `~/projects`, leaving symlinks at old paths when useful.
5. Do not move `~/.claude`, `~/.codex` runtime state, sessions, logs, auth files, sqlite state, or shell history.
6. Create or update `AGENTS.md` in every project.
7. Create `CLAUDE.md` in every project that imports `@AGENTS.md`, unless a project already has stronger Claude-specific instructions.
8. Create a project `~/projects/ai-workspace-system` with docs, scripts, prompts, and systemd timer files.
9. Create scripts:
   - `ai-launch`
   - `workspace-sync`
   - `workspace-new-project`
   - `install-local`
10. Configure scheduled sync at 13:00, 19:00, and 23:50 via user systemd timer, but ask before enabling it.
11. Save reports under `~/projects/_reports`.

Sync behavior:

- `workspace-sync` should fetch and pull clean repos.
- It should push committed local changes.
- It should skip dirty repos unless run with `--commit`.
- It should never force-push.
- Scheduled sync must run in safe mode and never auto-commit.

Final answer:

- Summarize moved projects.
- List skipped or risky directories.
- List created scripts and docs.
- Report sync status and remaining manual steps.

