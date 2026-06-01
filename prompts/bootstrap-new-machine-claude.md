# Claude Code Prompt: Bootstrap AI Workspace On A New Machine

[Русская версия](ru/bootstrap-new-machine-claude.md)

You are Claude Code working on my local machine.

Goal:
Create a consistent project workspace that works with Claude Code, Codex, and GitHub across all my machines.

Read and follow:

- `CLAUDE.md` files for Claude-specific instructions.
- `AGENTS.md` files for shared project instructions.
- If both exist, do not duplicate large rule sets; make `CLAUDE.md` import `@AGENTS.md`.
- `workspace.yaml` files for workbenches that prepare output for target repositories.
- Preserve existing global agent files. Do not replace `~/.claude/CLAUDE.md`, `~/.claude/rules/`, or `~/.codex/AGENTS.md`; propose additive changes instead.

Before changing GitHub remotes, ask for or read local configuration:

```text
~/.config/ai-workspace/config
```

Tasks:

1. Inventory likely projects in my home directory.
2. Classify projects as:
   - owned project
   - target repo
   - delivery workbench
   - research workbench
   - integration workbench
   - upstream/mirror repo
   - website/content repo
   - Codex asset
   - Claude project
   - artifact/archive
   - ignore
   - needs review
3. Create `~/projects`.
4. Move safe projects into `~/projects` and leave symlinks where old paths are likely referenced.
5. Keep `~/.claude`, `~/.claude.json`, `~/.codex/auth.json`, sessions, logs, sqlite state, and shell history out of Git.
6. Add baseline docs where missing:
   - `README.md`
   - `AGENTS.md`
   - `CLAUDE.md`
   - `workspace.yaml` when it has targets or is a workbench
   - `.gitignore`
   - `.env.example` when appropriate
7. Create or update `~/projects/ai-workspace-system` with:
   - scripts
   - docs
   - prompts
   - systemd units
   - templates
8. Create global instruction recommendations:
   - `~/.claude/CLAUDE.md`
   - `~/.claude/rules/`
   - `~/.codex/AGENTS.md`
9. Use `prompts/discover-and-normalize-local-projects.md` as the detailed workflow for existing scattered local projects.
10. After `GITHUB_ORG` is configured and GitHub CLI is authenticated, run `workspace-sync --clone-only` to hydrate missing repositories from GitHub into `PROJECTS_HOME`.
11. Do not enable scheduled sync until I approve.

Sync design:

- Sync should use GitHub as a discovery source: list repositories in `GITHUB_ORG` and clone missing repos into `PROJECTS_HOME`.
- Clean repos can be pulled and pushed automatically.
- Dirty repos must be skipped by scheduled sync.
- Interactive sync may ask before committing dirty repos.
- Upstream repos should preserve `origin`; private-org mirror should use a separate remote like `backup`.
- Scheduled sync should push upstream/mirror repos only to `backup`.
- Pushes to external `origin` remotes require explicit user instruction.
- Target repos should contain reviewed code/content/docs only.
- Delivery workbenches should contain prompts, research, briefs, raw inputs, generated drafts, and local pipeline state.

At the end:

- Save an inventory report.
- Save a final report.
- Tell me exactly what was changed and what still needs manual review.
