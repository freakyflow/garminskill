# Garmin Connect — OpenClaw Skill

An [OpenClaw](https://openclaw.ai) skill that syncs your daily health data from Garmin Connect into markdown files. OpenClaw can then reference your health and fitness data in conversation.

## What it syncs

- **Sleep** — duration, stages (deep/light/REM/awake), sleep score
- **Body** — steps, calories, distance, floors
- **Heart** — resting HR, max HR, HRV
- **Body Battery & SpO2**
- **Stress** — average level
- **Training Readiness** — score and level
- **Respiration** — waking and sleeping breathing rate
- **Fitness Age**
- **Intensity Minutes** — weekly moderate/vigorous totals
- **Weight** — if recorded
- **Activities** — name, duration, distance, calories, HR, elevation, pace, cadence, power, training effect, VO2 max

## Example output

```markdown
# Health — January 26, 2026

## Sleep: 8h 39m (Good)
Deep: 1h 50m | Light: 4h 30m | REM: 2h 19m | Awake: 0h 54m
Sleep Score: 85

## Body: 9,720 steps | 2,317 cal
Distance: 8.0 km | Floors: 42
Resting HR: 37 bpm | Max HR: 111 bpm
HRV: 68 ms
SpO2: 94.0%

## Training Readiness: 100 (Prime) — Ready To Go

## Respiration: Waking: 12 brpm | Sleeping: 13 brpm | Range: 5–20

## Fitness Age: 33 (6 years younger)

## Intensity Minutes: 385 weekly
Moderate: 69 | Vigorous: 158 | Goal: 150

## Activities
- **5K Run** — 28:15, 5.0 km, 320 cal
  Avg HR 155 / Max 172 | Elevation: +45m | Pace: 5:39/km | Cadence: 168 spm | Training Effect: 3.2 aerobic | VO2 Max: 50
```

Sections are only included when data is available.

## Setup

### Requirements

- Python 3.10+
- [uv](https://docs.astral.sh/uv/) (no pip install needed — dependencies are inline)
- A Garmin Connect account

### Environment variables

```bash
export GARMIN_EMAIL="you@example.com"
export GARMIN_PASSWORD="yourpassword"
```

### Run it

```bash
# Sync today
uv run scripts/sync_garmin.py

# Sync a specific date
uv run scripts/sync_garmin.py --date 2025-01-26

# Sync the last 7 days
uv run scripts/sync_garmin.py --days 7
```

Markdown files are written to `health/YYYY-MM-DD.md`.

### Install as an OpenClaw skill

```bash
ln -s /path/to/garminskill ~/.openclaw/skills/garmin-connect
```

### Cron

Schedule the sync to run every morning so your data stays up to date automatically. OpenClaw's `cron` tool can handle this, or use a system crontab:

```bash
0 7 * * * GARMIN_EMAIL="..." GARMIN_PASSWORD="..." uv run /path/to/garminskill/scripts/sync_garmin.py
```

## Auth notes

The script uses [garminconnect](https://github.com/cyberjunky/python-garminconnect) with [cloudscraper](https://github.com/VeNoMouS/cloudscraper) to bypass Cloudflare protection on Garmin's SSO. OAuth tokens are cached in `~/.garminconnect/` and are valid for ~1 year, so credentials are only needed on first login.
