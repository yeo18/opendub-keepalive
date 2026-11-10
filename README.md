# OpenDub keepalive

Public, secret-free workflow that pings the OpenDub API every five minutes to reduce
Render Free cold starts.

- API health: https://opendub-api.onrender.com/api/health
- Frontend proxy: https://opendub.vercel.app/api/health

The application code lives in the private repository `yeo18/opendub`. This repository
contains no application code, credentials, or user data. A monthly heartbeat commit keeps
GitHub from disabling scheduled workflows after 60 days of repository inactivity.
