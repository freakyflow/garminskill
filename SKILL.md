---
name: garmin-connect
description: Syncs daily health and fitness data from Garmin Connect into markdown files. Provides sleep, activity, heart rate, stress, body battery, HRV, SpO2, and weight data.
homepage: https://github.com/freakyflow/garminskill
metadata: {"clawdbot":{"emoji":"💪","requires":{"bins":["uv"],"env":["GARMIN_EMAIL","GARMIN_PASSWORD"]},"install":[{"id":"uv","kind":"brew","formula":"uv","bins":["uv"],"label":"Install uv via Homebrew"},{"id":"uv-sh","kind":"download","url":"https://astral.sh/uv/install.sh","bins":["uv"],"label":"Install uv via shell script","os":["linux"]}]}}
---

# Garmin Connect

This skill syncs your daily health data from Garmin Connect into readable markdown files.

## Syncing Data

Sync today's data:

```bash
uv run {baseDir}/scripts/sync_garmin.py
```

Sync a specific date:

```bash
uv run {baseDir}/scripts/sync_garmin.py --date 2026-02-07
```

Sync the last N days:

```bash
uv run {baseDir}/scripts/sync_garmin.py --days 7
```

## Reading Health Data

Health files are stored at `{baseDir}/health/YYYY-MM-DD.md` — one file per day.

To answer health or fitness questions, read the relevant date's file from the `{baseDir}/health/` directory. If the file doesn't exist for the requested date, run the sync command for that date first.

## Dependencies

This skill uses [uv](https://docs.astral.sh/uv/) to run the sync script. `uv` is a fast Python package manager by Astral that reads inline script metadata (PEP 723) and automatically installs dependencies (`garminconnect`, `cloudscraper`) in an isolated environment — no manual `pip install` needed.

## Credentials

Garmin Connect does not offer a public OAuth API. The `garminconnect` library authenticates with your email and password once, then caches OAuth tokens locally in `~/.garminconnect/` for approximately one year. Your credentials are only used for the initial login and are never stored or transmitted beyond Garmin's SSO endpoint.

## Cron Setup

Schedule the sync script to run every morning using OpenClaw's `cron` tool so your health data stays up to date automatically.
