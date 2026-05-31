# Launcher, обнаружение проектов и глобальные инструкции

[English version](../launcher-discovery-and-global-instructions.md)

Этот документ описывает first-run workflow, который делает workspace полезным на реальной машине.

## Launcher Workflow

Используйте launcher вместо прямого запуска агентов:

```bash
ai-launch codex
ai-launch claude
```

Launcher всегда показывает:

1. `New project`
2. `No project`
3. discovered projects from `PROJECT_ROOTS`

`New project` вызывает `workspace-new-project <name>`.
Команда создает проект в `PROJECTS_HOME`, пишет baseline `README.md`, `AGENTS.md`, `CLAUDE.md`, `workspace.yaml`, `.gitignore`, `.env.example`, затем выполняет:

```bash
git init -b main
git add README.md AGENTS.md CLAUDE.md workspace.yaml .gitignore .env.example
git commit -m "Initial project scaffold"
```

Если `GITHUB_ORG` задан и `gh` авторизован, команда может также создать и запушить private GitHub repository.

`No project` запускает выбранного агента из `$HOME`. Используйте это для разовых вопросов и задач без project context.

## Обнаружение существующих локальных проектов

Используйте [Найти и нормализовать локальные проекты](../../prompts/ru/discover-and-normalize-local-projects.md), если на машине уже есть разбросанные проекты.

Prompt является частью workspace system. Он просит агента:

- инвентаризировать вероятные project directories;
- классифицировать каждый candidate;
- создать configured project root, если его нет;
- предложить move plan перед перемещением файлов;
- добавить baseline docs and instructions;
- инициализировать Git только для real projects;
- сохранить existing remotes and branches;
- избегать secrets, caches, sessions, logs, auth state и runtime state.

## Global Agent Files Policy

Глобальные файлы - это machine-level preferences, а не project source.

Типичные файлы:

- `~/.codex/AGENTS.md`
- `~/.claude/CLAUDE.md`
- `~/.claude/rules/*.md`

Агенты не должны слепо перезаписывать их.

Policy:

- Если global file отсутствует, агент может предложить его создать.
- Если global file существует, агент должен прочитать его, сохранить содержимое и предложить небольшой additive patch.
- Если automated editing явно одобрен, append/update делается в clearly marked section, а не полной заменой файла.
- Никогда не коммитьте global runtime state, sessions, auth files, logs, sqlite state или shell history.
- Reusable public instructions держите в project docs; personal machine preferences - в local config или global agent files.
