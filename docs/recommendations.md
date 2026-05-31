# Recommendations

## Keep Auto Sync Conservative

Scheduled sync should not auto-commit. Use it to fetch, pull clean repos, and push already committed work.

Use interactive commit mode manually:

```bash
workspace-sync --commit
```

## Add A Project Manifest Later

A manifest can make behavior explicit:

```yaml
projects:
  - name: aenix.io
    path: ~/projects/aenix.io
    type: website
    sync: origin
  - name: cozystack-website
    path: ~/claude/cozystack-website
    type: upstream-mirror
    sync: manual
```

This is better than guessing policy from paths forever.

## Separate Source Sync From Backup

GitHub sync is not a full backup. Use `restic` or `borg` for:

- large artifacts
- generated outputs
- documents
- files not committed yet
- private local data

## Use Secret Scanning Before Organization Push

Run at least:

```bash
workspace-sync --dry-run
~/projects/_codex/scripts/audit-secrets ~/projects/<project>
```

Prefer `gitleaks` or `trufflehog` if installed.

## Keep Instructions Small

Use:

- global files for personal working style
- project files for build/test/project rules
- rules/skills for long topic-specific guidance

Large always-loaded instruction files reduce context quality.

