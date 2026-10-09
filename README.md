# claude-warmup

A GitHub Actions job that starts my Claude 5-hour usage window every morning, even when my laptop is off.

## Schedule

[cron-job.org](https://cron-job.org) starts the workflow at exact times (Asia/Colombo time zone), so back-to-back 5-hour windows cover a 09:00–17:30 workday:

| Trigger | Window | Covers |
|---|---|---|
| 05:30 | 05:30–10:30 | 09:00 until the 10:30 tea break |
| 10:35 | 10:35–15:35 | after the tea break, plus lunch (13:00–14:00) |
| 15:40 | 15:40–20:40 | until leaving at 17:30 |

The later triggers come a few minutes after each window ends, so they never fire into a window that is still open. Each run installs Claude Code, logs in with my subscription token, and runs `claude -p "hi" --model haiku`.

GitHub's built-in `schedule:` trigger is not used. It ran 20 minutes to 17 hours late, and sometimes skipped runs entirely.

### cron-job.org job settings

- **URL:** `https://api.github.com/repos/Sheran101/claude-warmup/actions/workflows/claude-warmup.yml/dispatches`
- **Method:** `POST`, **Body:** `{"ref":"main"}`
- **Headers:** `Accept: application/vnd.github+json`, `Authorization: Bearer <fine-grained PAT>`, `X-GitHub-Api-Version: 2022-11-28`, `Content-Type: application/json`
- **PAT:** fine-grained, this repo only, *Actions: Read and write*. **Expires 2027-10-07.** Renew it and paste the new one into all 3 jobs.

A successful call returns **204 No Content**.

## Run it manually

- **GitHub mobile app:** open this repo, go to **Actions**, open **Claude session warmup**, tap **Run workflow**, then pick `main`.
- **Terminal:** `gh workflow run claude-warmup.yml --repo Sheran101/claude-warmup`

## Refresh the token

If a run fails at **Start Claude session**, or the Discord message says to refresh:

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
