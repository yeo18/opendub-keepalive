# OpenDub keepalive

Public, secret-free automation that keeps the OpenDub API awake between Render Free
sleep cycles.

- API health: https://opendub-api.onrender.com/api/health
- Frontend proxy: https://opendub.vercel.app/api/health

## How it works

- `keepalive.yml` pings the API every five minutes for about 4.5 hours, then
  re-dispatches itself through the GitHub API. The next run starts as soon as the
  current one ends, so the API never reaches Render's 15-minute sleep threshold.
- `fallback.yml` runs every hour and starts a keepalive run only if none is
  already active. It covers a canceled or failed long run.
- `heartbeat.yml` commits a timestamp once a month so GitHub does not disable
  scheduled workflows after 60 days of repository inactivity.

The workflow needs `actions: write` to dispatch the next run. It uses the ephemeral
`GITHUB_TOKEN`; no personal token or secret is stored. The application code lives in
the private repository `yeo18/opendub`. This repository contains no application code,
credentials, or user data.

Manual test with a short loop:

```bash
gh workflow run keepalive.yml --repo yeo18/opendub-keepalive \
  -f iterations=2 -f interval_seconds=5
```
