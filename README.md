# reusable-workflows

Reusable GitHub Actions workflows (`on: workflow_call`) shared across the
agglayer repositories. They live in `.github/workflows/`.

## Pinning callers

Callers should reference these workflows by an immutable ref, not `@main`:

```yaml
jobs:
  audit:
    # Prefer a full commit SHA; keep the version or date in a trailing comment
    uses: agglayer/reusable-workflows/.github/workflows/audit.yml@<full-40-char-sha> # <tag or date>
```

`@main` is a moving target: every merge here is picked up by every caller on
its next run, including workflows that receive secrets (`BUF_TOKEN`,
`TEAM_READ_GH_TOKEN`, `SLACK_WEBHOOK_URL`) or run caller-supplied commands.

- **Full commit SHA** is the only ref that cannot be moved. Prefer it.
- **A release tag** (`@vX.Y.Z`) is acceptable if the repository protects tags
  against being re-pointed.
- Bump the pin on purpose (Dependabot's `github-actions` ecosystem or Renovate
  can open the PRs) instead of tracking `@main`.

> **Status:** this repository does not publish release tags yet, so callers
> should pin to a commit SHA until one is cut.
