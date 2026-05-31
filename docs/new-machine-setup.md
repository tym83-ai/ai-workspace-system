# New Machine Setup

## Manual Bootstrap

1. Install `git`, `gh`, Codex, and Claude Code.
2. Authenticate GitHub:

   ```bash
   gh auth login
   ```

3. Clone the workspace system:

   ```bash
   mkdir -p ~/projects
   git clone git@github.com:<org>/ai-workspace-system.git ~/projects/ai-workspace-system
   ```

4. Install scripts:

   ```bash
   ~/projects/ai-workspace-system/bin/install-local
   ```

5. Review config:

   ```bash
   sed -n '1,120p' ~/.config/ai-workspace/config
   ```

6. Enable scheduled sync:

   ```bash
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

