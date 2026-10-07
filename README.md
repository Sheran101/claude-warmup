# claude-warmup

A GitHub Actions job that starts my Claude 5-hour usage window every morning, even when my laptop is off.

## Schedule

Runs three times a day (Sri Lanka time) so back-to-back 5-hour windows cover a 09:00–17:30 workday:

| Trigger | Cron (UTC) | Window | Covers |
|---|---|---|---|
| 05:30 | `0 0 * * *` | 05:30–10:30 | 09:00 until the 10:30 tea break |
| 10:30 | `0 5 * * *` | 10:30–15:30 | after the tea break, plus lunch (13:00–14:00) |
| 15:30 | `0 10 * * *` | 15:30–20:30 | until leaving at 17:30 |

Each run installs Claude Code, logs in with my subscription token, and runs `claude -p "hi" --model haiku`. If I'm already using Claude when a trigger fires, that run just uses the window I already have open.

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
