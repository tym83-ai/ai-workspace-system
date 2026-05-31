# Prompt: Normalize Existing Project For Codex And Claude

Normalize the current repository for use with Codex, Claude Code, and GitHub.

Tasks:

1. Inspect project structure.
2. Identify setup, test, lint, build, and run commands.
3. Add or update `README.md` if missing or clearly incomplete.
4. Add or update `AGENTS.md` as the canonical shared AI instructions file.
5. Add or update `CLAUDE.md` so Claude Code imports `@AGENTS.md` and adds only Claude-specific notes.
6. Add `.gitignore` entries for secrets, caches, dependencies, local outputs, and agent state.
7. Add `.env.example` if environment variables are used.
8. Do not remove project-specific existing instructions. Merge carefully.
9. Do not commit secrets or session logs.
10. Run the narrowest useful verification command.

Final answer:

- files changed
- project commands discovered
- sync risks
- remaining manual follow-up

