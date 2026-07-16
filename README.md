# .github — shared workflows

## changelog.yml (reusable)

Posts a pt-BR changelog to the team Discord channel after every production
deploy. Commits in the pushed range are summarized by the Ollama instance on
the VPS (qwen2.5-coder:7b, R$0); if the model is unreachable the raw commit
list is posted instead. The job is `continue-on-error` — it can never fail a
deploy.

Wire it into a service by appending to `.github/workflows/deploy.yml`:

```yaml
  changelog:
    name: Changelog
    needs: deploy
    uses: escalasaas/.github/.github/workflows/changelog.yml@main
    with:
      service: <repo-name>
    secrets: inherit
```

Required repo secrets: `VPS_SSH_KEY`, `VPS_HOST` (already set by the deploy
job) and `DISCORD_CHANGELOG_WEBHOOK_URL`.
