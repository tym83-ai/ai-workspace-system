# Codex Prompt: Bootstrap AI Workspace On A New Machine

[Русская версия](ru/bootstrap-new-machine-codex.md)

You are Codex working on my local machine.

Goal:
Create a clean local AI workspace for all projects under `~/projects`, with safe GitHub synchronization, Codex instructions, Claude Code bridge files, and reusable scripts.

Rules:

- Do not upload secrets.
- Do not delete or overwrite local files without explicit approval.
- Do not force-push.
- Do not push to external/upstream `origin` remotes unless I explicitly ask.
- For external/upstream repos, preserve `origin` and add a `backup` remote in the configured GitHub org/account only after confirming the strategy.
- Do not auto-commit dirty repositories unless I explicitly ask.
- Keep Claude-specific state out of Codex repos.
- Use `AGENTS.md` as the canonical shared project instruction file.
- Use `CLAUDE.md` as a Claude bridge that imports `@AGENTS.md`.
- Separate target repositories from delivery workbenches.
- Use `workspace.yaml` to connect workbenches to target repositories.
- Do not overwrite global agent files. Preserve existing `~/.codex/AGENTS.md`, `~/.claude/CLAUDE.md`, and `~/.claude/rules/`; propose additive changes instead.

Before changing GitHub remotes, ask for or read local configuration:

```text
~/.config/ai-workspace/config
```

Tasks:

1. Inspect the home directory for likely projects.
2. Classify directories as target repo, delivery workbench, research workbench, integration workbench, repo mirror, Codex asset, Claude project, artifact, archive, ignore, or needs review.
3. Create `~/projects` if missing.
4. Move safe projects into `~/projects`, leaving symlinks at old paths when useful.
5. Do not move `~/.claude`, `~/.codex` runtime state, sessions, logs, auth files, sqlite state, or shell history.
6. Create or update `AGENTS.md` in every project.
7. Create `CLAUDE.md` in every project that imports `@AGENTS.md`, unless a project already has stronger Claude-specific instructions.
8. Create `workspace.yaml` for workbenches that prepare output for target repositories.
9. Create or update a project `~/projects/ai-workspace-system` with docs, scripts, prompts, and systemd timer files.
10. Create scripts:
   - `ai-launch`
   - `workspace-configure`
   - `workspace-sync`
   - `workspace-new-project`
   - `workspace-shell-setup`
   - `install-local`
11. Run `workspace-shell-setup` so `LOCAL_BIN_DIR` is present in shell startup files and `ai-launch` works in new shells.
12. After `GITHUB_ORG` is configured and GitHub CLI is authenticated, run `workspace-sync --clone-only` to hydrate missing repositories from GitHub into `PROJECTS_HOME`.
13. Configure scheduled sync at 13:00, 19:00, and 23:50 via user systemd timer, but ask before enabling it.
14. For site/upstream repos, ask before changing remotes. Default to `origin` as upstream and `backup` as private mirror.
15. Keep prompts, research, raw inputs, generated drafts, and local pipeline state in workbench repos, not target repos.
16. Save reports under `~/projects/_reports`.
17. Use `prompts/discover-and-normalize-local-projects.md` as the detailed workflow for existing scattered local projects.

Sync behavior:

- `workspace-sync` should list repositories in `GITHUB_ORG` with `gh repo list` and clone missing repos into `PROJECTS_HOME`.
- `workspace-sync` should fetch and pull clean repos.
- It should push committed local changes to `origin` only for repos owned by the configured `GITHUB_ORG`.
- It should push external/upstream repos to `backup`, not `origin`.
- It should skip dirty repos unless run with `--commit`.
- It should never force-push.
- Scheduled sync must run in safe mode and never auto-commit.

Final answer:

- Summarize moved projects.
- List skipped or risky directories.
- List created scripts and docs.
- Report sync status and remaining manual steps.
