# Recommendations

[Русская версия](ru/recommendations.md)

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
  - name: example-site
    path: ~/projects/example-site
    type: website
    sync: backup
  - name: vendor-docs
    path: ~/projects/vendor-docs
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

## Ask Before Changing Upstream Strategy

For repositories whose `origin` is not owned by the configured `GITHUB_ORG`, agents should ask what strategy to use before changing remotes or pushing to upstream.

Default:

- keep `origin` as canonical upstream
- add `backup` under the configured GitHub org/account
- scheduled sync pushes to `backup`
- upstream PRs/commits happen only after explicit instruction
- exclude deep nested clones such as pipeline workspaces unless explicitly promoted to a managed project

## Use Secret Scanning Before Organization Push

Configure an optional scanner:

```text
SECRET_SCAN_CMD="gitleaks detect --source"
```

Then use:

```bash
workspace-sync --commit
```

The sync script appends the repository path to `SECRET_SCAN_CMD`.

## Keep Instructions Small

Use:

- global files for personal working style
- project files for build/test/project rules
- rules/skills for long topic-specific guidance

Large always-loaded instruction files reduce context quality.
