# Bootstrap AI Workspace

Use this file when setting up a new machine with Codex or Claude Code.

GitHub organization:

```text
https://github.com/tym83-ai
```

Main repository:

```text
https://github.com/tym83-ai/ai-workspace-system
```

## Start Prompt

```text
Use this GitHub organization: https://github.com/tym83-ai

Bootstrap my AI workspace on this machine.

First clone:
https://github.com/tym83-ai/ai-workspace-system.git
into:
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
git clone https://github.com/tym83-ai/ai-workspace-system.git ~/projects/ai-workspace-system
~/projects/ai-workspace-system/bin/install-local
```

Then review:

```bash
~/.config/ai-workspace/config
```

