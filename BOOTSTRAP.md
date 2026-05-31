# Bootstrap AI Workspace

Use this file when setting up a new machine with Codex or Claude Code.

This repository is generic. Before running bootstrap work, provide the agent with:

- this repository URL
- optional GitHub organization or user account for owned repos and private mirrors
- desired local project root, usually `~/projects`

## Start Prompt

```text
Bootstrap my AI workspace on this machine.

Repository URL for the workspace system:
<ai-workspace-system-repo-url>

GitHub organization or user for owned repos and private mirrors:
<github-org-or-user, or leave empty if disabled>

Local project root:
~/projects

First clone the workspace system into:
~/projects/ai-workspace-system

Then read:
~/projects/ai-workspace-system/docs/new-machine-setup.md
~/projects/ai-workspace-system/prompts/bootstrap-new-machine-codex.md
~/projects/ai-workspace-system/prompts/bootstrap-new-machine-claude.md

Follow the instructions.

Rules:
- Do not upload secrets.
- Do not delete local files.
- Do not force-push.
- Do not auto-commit dirty repositories.
- Do not enable scheduled sync until you show me what will be enabled.
- Keep Codex and Claude runtime state out of Git.
- Use AGENTS.md as the canonical shared project instruction file.
- Use CLAUDE.md as the Claude Code bridge that imports @AGENTS.md.
- Ask before changing remotes for upstream or mirror repositories.

At the end, show:
- what was cloned
- what was installed
- what repositories are connected to GitHub
- what remains local only
- what needs manual review
```

## Minimal Manual Setup

```bash
mkdir -p ~/projects
git clone <ai-workspace-system-repo-url> ~/projects/ai-workspace-system
~/projects/ai-workspace-system/bin/install-local
workspace-configure
```

Then review:

```bash
sed -n '1,120p' ~/.config/ai-workspace/config
```
