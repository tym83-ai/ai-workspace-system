# Claude Code Prompt: Bootstrap AI Workspace на новой машине

[English version](../bootstrap-new-machine-claude.md)

```text
Ты Claude Code и работаешь на моей локальной машине.

Цель:
Создать consistent project workspace, который работает с Claude Code, Codex и GitHub на всех моих машинах.

Читай и соблюдай:
- CLAUDE.md files для Claude-specific instructions.
- AGENTS.md files для shared project instructions.
- Если оба файла есть, не дублируй large rule sets; пусть CLAUDE.md imports @AGENTS.md.
- workspace.yaml files для workbenches, которые готовят output для target repositories.
- Сохраняй existing global agent files. Не заменяй ~/.claude/CLAUDE.md, ~/.claude/rules/ или ~/.codex/AGENTS.md; предлагай additive changes.

Перед изменением GitHub remotes спроси или прочитай local configuration:

~/.config/ai-workspace/config

Задачи:
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
3. Create ~/projects.
4. Move safe projects into ~/projects and leave symlinks where old paths are likely referenced.
5. Keep ~/.claude, ~/.claude.json, ~/.codex/auth.json, sessions, logs, sqlite state, and shell history out of Git.
6. Add baseline docs where missing: README.md, AGENTS.md, CLAUDE.md, workspace.yaml when needed, .gitignore, .env.example when appropriate.
7. Create or update ~/projects/ai-workspace-system with scripts, docs, prompts, systemd units, templates.
8. Create global instruction recommendations for ~/.claude/CLAUDE.md, ~/.claude/rules/, ~/.codex/AGENTS.md.
9. Use prompts/ru/discover-and-normalize-local-projects.md as the detailed workflow for existing scattered local projects.
10. Do not enable scheduled sync until I approve.

Sync design:
- Clean repos can be pulled and pushed automatically.
- Dirty repos must be skipped by scheduled sync.
- Interactive sync may ask before committing dirty repos.
- Upstream repos should preserve origin; private-org mirror should use backup.
- Scheduled sync should push upstream/mirror repos only to backup.
- Pushes to external origin remotes require explicit user instruction.
- Target repos should contain reviewed code/content/docs only.
- Delivery workbenches should contain prompts, research, briefs, raw inputs, generated drafts, and local pipeline state.

At the end:
- Save an inventory report.
- Save a final report.
- Tell me exactly what was changed and what still needs manual review.
```
