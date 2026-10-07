# claude-warmup

A GitHub Actions job that starts my Claude 5-hour usage window every morning, even when my laptop is off.

## Schedule

Runs daily at **06:30 Sri Lanka time** (cron `0 1 * * *`, 01:00 UTC). It installs Claude Code, logs in with my subscription token, and runs `claude -p "hi" --model haiku`.

GitHub can start scheduled runs a few minutes late (sometimes 10–30 minutes) when its servers are busy.

## Run it manually

- **GitHub mobile app:** open this repo, go to **Actions**, open **Claude session warmup**, tap **Run workflow**, then pick `main`.
- **Terminal:** `gh workflow run claude-warmup.yml --repo Sheran101/claude-warmup`

## Refresh the token

If a run fails with an auth error, or the Discord message says to refresh:

```sh
claude setup-token
gh secret set CLAUDE_CODE_OAUTH_TOKEN --repo Sheran101/claude-warmup
```

Paste the new token when `gh` prompts for it.

## Secrets

| Secret | Required | Purpose |
|---|---|---|
| `CLAUDE_CODE_OAUTH_TOKEN` | yes | Token from `claude setup-token` |
| `DISCORD_WEBHOOK_URL` | no | Posts a success or failure message |
