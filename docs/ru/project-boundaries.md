# Границы проектов

[English version](../project-boundaries.md)

Некоторые проекты готовят работу для другого repository. Эти роли нужно разделять.

## Target Repo

Target repo владеет материалом, который можно review/build/deploy/submit upstream:

- product code
- website source
- public docs
- reviewed content
- PR branches

Он не должен содержать private prompts, session logs, raw AI drafts, pipeline runtime или local agent state.

## Delivery Workbench

Delivery workbench владеет процессом:

- prompts and instructions
- research and briefs
- source notes and raw inputs
- generated drafts before review
- local pipeline scripts/state
- Claude/Codex agents, skills, workflow assets

Workbench может ссылаться на один или несколько target repos через `workspace.yaml`.

## Default Policy

- Держите `origin` target repo направленным на real upstream.
- Добавляйте `backup` в configured GitHub org/account для automatic private backup, если это безопасно.
- Scheduled sync может пушить target repo state в `backup`.
- Pushes/PRs в upstream `origin` требуют явного подтверждения пользователя.
- Если непонятно, где должен жить файл, сначала держите его в workbench.

## Manifest

Форма:

```yaml
name: example-delivery-workbench
type: delivery-workbench
owner: <github-org-or-user>

targets:
  - name: example-site
    path: ~/projects/example-site
    origin: https://github.com/example/site.git
    backup: https://github.com/<github-org-or-user>/mirror-example-site.git
    publish_policy: explicit
    role: target website repository

boundaries:
  workbench_owns:
    - prompts
    - research
    - drafts
  target_owns:
    - reviewed website content
    - public assets
```
