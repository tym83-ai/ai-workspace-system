# New Machine Setup

[Русская версия](ru/new-machine-setup.md)

## Manual Bootstrap

1. Install `git`, `gh`, Codex, and Claude Code.
2. Authenticate GitHub:

   ```bash
   gh auth login
   ```

3. Clone the workspace system:

   ```bash
   mkdir -p ~/projects
   git clone <ai-workspace-system-repo-url> ~/projects/ai-workspace-system
   ```

4. Install scripts:

   ```bash
   ~/projects/ai-workspace-system/bin/install-local
   ```

   The installer symlinks helper commands into `LOCAL_BIN_DIR` and runs `workspace-shell-setup`
   so `ai-launch` and `workspace-*` are available in new shells.

5. Configure this machine:

   ```bash
   workspace-configure
   sed -n '1,120p' ~/.config/ai-workspace/config
   ```

6. If commands are not found in a new shell, repair PATH integration:

   ```bash
   ~/.local/bin/workspace-shell-setup
   command -v ai-launch
   ```

7. Hydrate local projects from GitHub:

   ```bash
   workspace-sync --clone-only
   ```

   This lists repositories in `GITHUB_ORG` with `gh repo list` and clones missing ones into `PROJECTS_HOME`.
   Existing directories and existing remotes are not overwritten.

8. Enable scheduled sync only after reviewing config and dry-run output:

   ```bash
   workspace-sync --dry-run
   systemctl --user daemon-reload
   systemctl --user enable --now ai-workspace-sync.timer
   ```

## First Agent Run

Use:

```bash
ai-launch codex
ai-launch claude
```

Optional aliases:

```bash
alias codex='ai-launch codex'
alias claude='ai-launch claude'
```

Do not add aliases until you are comfortable that the launcher works.

The launcher shows `New project`, `No project`, then discovered projects.
`New project` creates a Git repository with baseline docs and an initial commit.
`No project` starts from `$HOME` for one-off tasks.

## Normalize Existing Local Projects

If the machine already has scattered projects, give the agent:

```text
~/projects/ai-workspace-system/prompts/discover-and-normalize-local-projects.md
```

That prompt inventories local directories, proposes a move plan into the configured project root, adds baseline docs/instructions, and initializes Git repositories only for real projects.

## Global Agent Files

Global Codex and Claude files should be preserved.
If they are missing, agents may propose creating them.
If they already exist, agents should read them and apply only additive changes after approval.

Do not move or commit global runtime state, sessions, auth files, logs, sqlite state, shell history, or local machine secrets.
