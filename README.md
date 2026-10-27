# HiBob attendance skill

A personal Codex skill that fills missing attendance for the current month through today in HiBob using Chrome. It saves daily entries and leaves final monthly submission to you.

## Requirements

- Codex with Chrome UI automation tools available. This repository contains instructions, not a browser extension or automation server.
- Access to your own HiBob account. If you are logged out, Codex asks you to log in through Chrome before continuing.

## Install

Clone this repository, then copy the skill into your user-level Codex skills directory:

```sh
git clone https://github.com/rodrigo-berlochi/hibob-attendance.git
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills/hibob-attendance"
cp hibob-attendance/SKILL.md "${CODEX_HOME:-$HOME/.codex}/skills/hibob-attendance/SKILL.md"
```

If you already have a customized version, review it before replacing the file. Start a new Codex session after installation.

## Use

Ask Codex:

> Fill the time attendance in my HiBob account for the current ongoing month until today.

Or invoke `$hibob-attendance` explicitly.

The default working-day schedule is:

| Entry | Start | End |
| --- | --- | --- |
| Work — Teleworking | 09:00 | 14:00 |
| Unpaid break | 14:00 | 15:00 |
| Work — Teleworking | 15:00 | 18:00 |

This includes today's full schedule, even before 18:00. Review the schedule and project in [SKILL.md](SKILL.md) before use, and customize your installed copy to match your actual attendance.

The skill skips non-working days, holidays, and full-day leave; preserves complete entries; and stops without changes when everything is already filled. Conflicting or ambiguous entries require clarification. It never clicks the monthly **Submit** button or uses the live clock-in timer.

No credentials or attendance records are included. Browser controls can change; the skill verifies each saved day and reports unfinished work if it encounters a blocker.
