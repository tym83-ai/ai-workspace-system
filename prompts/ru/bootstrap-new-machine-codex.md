# Codex Prompt: Bootstrap AI Workspace на новой машине

[English version](../bootstrap-new-machine-codex.md)

```text
Ты Codex и работаешь на моей локальной машине.

Цель:
Создать чистый локальный AI workspace для всех проектов под ~/projects, с безопасной GitHub-синхронизацией, Codex-инструкциями, Claude Code bridge files и reusable scripts.

Правила:
- Не загружай секреты.
- Не удаляй и не перезаписывай локальные файлы без явного подтверждения.
- Не делай force-push.
- Не пушь во внешние/upstream origin remotes, если я явно не попросил.
- Для external/upstream repos сохраняй origin и добавляй backup remote в configured GitHub org/account только после подтверждения strategy.
- Не auto-commit dirty repositories, если я явно не попросил.
- Не клади Claude-specific state в Codex repos.
- Используй AGENTS.md как canonical shared project instruction file.
- Используй CLAUDE.md как Claude bridge, импортирующий @AGENTS.md.
- Разделяй target repositories и delivery workbenches.
- Используй workspace.yaml для связи workbenches с target repositories.
- Не перезаписывай global agent files. Сохраняй existing ~/.codex/AGENTS.md, ~/.claude/CLAUDE.md и ~/.claude/rules/; предлагай additive changes.

Перед изменением GitHub remotes спроси или прочитай local configuration:

~/.config/ai-workspace/config

Задачи:
1. Inspect home directory for likely projects.
2. Classify directories as target repo, delivery workbench, research workbench, integration workbench, repo mirror, Codex asset, Claude project, artifact, archive, ignore, or needs review.
3. Create ~/projects if missing.
4. Move safe projects into ~/projects, leaving symlinks at old paths when useful.
5. Do not move ~/.claude, ~/.codex runtime state, sessions, logs, auth files, sqlite state, or shell history.
6. Create or update AGENTS.md in every project.
7. Create CLAUDE.md in every project that imports @AGENTS.md, unless a project already has stronger Claude-specific instructions.
8. Create workspace.yaml for workbenches that prepare output for target repositories.
9. Create or update ~/projects/ai-workspace-system with docs, scripts, prompts, and systemd timer files.
10. Create scripts: ai-launch, workspace-configure, workspace-sync, workspace-new-project, install-local.
11. Configure scheduled sync at 13:00, 19:00, and 23:50 via user systemd timer, but ask before enabling it.
12. For site/upstream repos, ask before changing remotes. Default to origin as upstream and backup as private mirror.
13. Keep prompts, research, raw inputs, generated drafts, and local pipeline state in workbench repos, not target repos.
14. Save reports under ~/projects/_reports.
15. Use prompts/ru/discover-and-normalize-local-projects.md as the detailed workflow for existing scattered local projects.

Sync behavior:
- workspace-sync should fetch and pull clean repos.
- It should push committed local changes to origin only for repos owned by the configured GITHUB_ORG.
- It should push external/upstream repos to backup, not origin.
- It should skip dirty repos unless run with --commit.
- It should never force-push.
- Scheduled sync must run in safe mode and never auto-commit.

Final answer:
- Summarize moved projects.
- List skipped or risky directories.
- List created scripts and docs.
- Report sync status and remaining manual steps.
```
