# Prompt: Найти и нормализовать локальные проекты

[English version](../discover-and-normalize-local-projects.md)

Используйте этот prompt с Codex или Claude Code, если на машине уже есть разбросанные локальные проекты и их нужно привести к AI Workspace System.

```text
Ты работаешь на моей локальной машине.

Цель:
Найти вероятные локальные проекты, классифицировать их, перенести безопасные проекты в ~/projects, добавить базовую AI/project documentation и инициализировать Git repositories там, где это уместно.

Сначала прочитай локальный конфиг, если он есть:
~/.config/ai-workspace/config

Используй PROJECTS_HOME как целевой каталог. Если он не настроен, используй ~/projects.

Правила:
- Не загружай секреты.
- Не удаляй файлы.
- Не перемещай ничего, пока не покажешь move plan, если я явно не одобрил этот prompt как executable.
- Не трогай runtime state, auth, sessions, logs, caches, shell history, package caches, browser profiles, sqlite state или agent conversation history.
- Не перемещай ~/.claude, ~/.codex, ~/.ssh, ~/.gnupg, ~/.config, ~/.local/state или hidden service directories в Git.
- Не делай force-push.
- Не пушь во внешние/upstream origin remotes без моего явного запроса.
- Для external/upstream repos сохраняй origin и используй backup только после подтверждения strategy.
- Держи target repos отдельно от delivery workbenches.
- Не перезаписывай global agent files. Если ~/.codex/AGENTS.md, ~/.claude/CLAUDE.md или ~/.claude/rules уже существуют, сохрани их и предложи только additive changes.

Задачи:
1. Инвентаризируй вероятные проекты в home directory и configured PROJECT_ROOTS.
2. Классифицируй каждый candidate как:
   - owned project
   - target repo
   - delivery workbench
   - research workbench
   - integration workbench
   - upstream/mirror repo
   - website/content repo
   - agent assets
   - artifact/archive
   - ignore
   - needs review
3. Сохрани inventory report в ~/projects/_reports.
4. Создай ~/projects, если его нет.
5. Для safe project directories вне ~/projects предложи move plan в ~/projects.
6. После approval перенеси approved projects и оставь symlinks на старых путях, где это полезно.
7. Для project directories без Git инициализируй Git только если это real project, а не artifact/cache/archive.
8. Перед первым commit добавь или обнови:
   - README.md
   - AGENTS.md
   - CLAUDE.md importing @AGENTS.md
   - .gitignore
   - .env.example if env vars are used
   - workspace.yaml when the project is a workbench or has target repos
9. Private prompts, raw research, generated drafts, pipeline runtime и local agent state держи в workbench repos, не в target repos.
10. Для existing Git repos сохраняй remotes and branches. Не переписывай history.
11. Для dirty repos покажи status и спроси перед commit.
12. Запусти самый узкий полезный verification command для измененных проектов.
13. Для global agent files создавай missing files только после approval. Для existing files покажи recommended additions вместо замены файла.

Финальный ответ:
- projects moved
- projects initialized as Git repos
- docs/instructions added
- projects skipped
- projects needing manual review
- sync strategy for owned repos and upstream/mirror repos
- exact commands for optional next steps
```
