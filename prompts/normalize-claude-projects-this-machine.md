# Claude Code Prompt: Normalize Claude Projects On This Machine

You are Claude Code working on this machine.

Goal:
Normalize the Claude project workspace so all Claude-managed and mixed projects are discoverable, documented, safe to sync, and compatible with the shared AI workspace system.

Local context:

- Canonical mixed workspace: `~/projects`
- Claude legacy workspace: `~/claude`
- Workspace system: `~/projects/ai-workspace-system`
- GitHub organization: `https://github.com/tym83-ai`

Important boundaries:

- Do not move or delete `~/.claude`, `~/.claude.json`, Claude auth, sessions, logs, or project history.
- Do not move project directories unless I explicitly approve the move plan.
- Do not force-push.
- Do not auto-commit dirty repositories.
- Do not upload secrets, `.env`, credentials, tokens, logs, sqlite state, session history, or shell history.
- Preserve existing remotes. For private organization mirrors, add `backup`, not a replacement `origin`, unless I explicitly approve a transfer.
- For website/upstream repos, default to `origin` as read-mostly upstream and `backup` as the private `tym83-ai` mirror.

Tasks:

1. Inventory Claude projects under:
   - `~/claude`
   - `~/projects`
   - any existing project roots listed in `~/.config/ai-workspace/config`

2. For each project, record:
   - path
   - git status
   - remotes
   - primary stack/framework if obvious
   - whether it has `README.md`
   - whether it has `AGENTS.md`
   - whether it has `CLAUDE.md`
   - whether it has `.claude/`
   - whether it looks like an upstream/mirror repo
   - sync recommendation
   - risks or secrets-looking files, without printing secret values

3. Save inventory:

   ```text
   ~/projects/_reports/claude-projects-inventory-YYYY-MM-DD.md
   ```

4. Normalize instructions:
   - If `AGENTS.md` is missing, create a concise project-specific one.
   - If `CLAUDE.md` is missing, create one that imports `@AGENTS.md`.
   - If `CLAUDE.md` already exists and is substantial, do not replace it. Add a short reference to `AGENTS.md` only if safe.
   - Keep project-specific rules intact.

5. Normalize docs:
   - If `README.md` is missing, create a minimal one with project purpose, setup, common commands, and sync notes.
   - If `.gitignore` is missing or weak, add safe exclusions for secrets, caches, dependencies, logs, local DBs, and agent local state.
   - Add `.env.example` only when the project clearly uses env vars.

6. GitHub sync strategy:
   - Owned clean projects can be prepared for `tym83-ai`.
   - Upstream/mirror repos should keep `origin` and optionally get a `backup` remote under `tym83-ai`.
   - Dirty repos should be reported, not auto-committed.
   - Repos with unclear ownership or sensitive data should be marked `needs-review`.
   - Ask explicitly before pushing to any external `origin`.

7. Do not push until you show me:
   - proposed repo name
   - proposed remote policy
   - whether it is clean or dirty
   - what would be committed

8. Final output:
   - changes made
   - projects that still need review
   - projects ready to sync
   - projects that should stay local
   - exact commands for optional next steps
