# Prompt: Нормализовать существующий проект для Codex и Claude

[English version](../normalize-existing-project.md)

Нормализуй текущий repository для использования с Codex, Claude Code и GitHub.

Задачи:

1. Inspect project structure.
2. Identify setup, test, lint, build, and run commands.
3. Classify the project as `target-repo`, `delivery-workbench`, `research-workbench`, or `integration-workbench`.
4. If it prepares work for another repo, add or update `workspace.yaml` and keep prompts/research/drafts out of the target repo.
5. Add or update `README.md` if missing or clearly incomplete.
6. Add or update `AGENTS.md` as the canonical shared AI instructions file.
7. Add or update `CLAUDE.md` so Claude Code imports `@AGENTS.md` and adds only Claude-specific notes.
8. Add `.gitignore` entries for secrets, caches, dependencies, local outputs, and agent state.
9. Add `.env.example` if environment variables are used.
10. Do not remove project-specific existing instructions. Merge carefully.
11. Do not commit secrets or session logs.
12. Run the narrowest useful verification command.

Финальный ответ:

- files changed
- project commands discovered
- project type and target/workbench boundary
- sync risks
- remaining manual follow-up
